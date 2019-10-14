- [Ray: Distributed Training - 架构设计 (Architecture)](#ray-distributed-training---架构设计-architecture)
  - [1. Application Layer](#1-application-layer)
    - [1.1. Driver](#11-driver)
    - [1.2. Worker](#12-worker)
    - [1.3. Actor](#13-actor)
  - [2. System Layer](#2-system-layer)
    - [2.1. Global Control Store (GCS)](#21-global-control-store-gcs)
    - [2.2. Bottom-Up Distributed Scheduler](#22-bottom-up-distributed-scheduler)
    - [2.3. In-Memory Distributed Object Store](#23-in-memory-distributed-object-store)
  - [3. Refer Links](#3-refer-links)

# Ray: Distributed Training - 架构设计 (Architecture)

> Ray is an active open source project developed at the University of California, Berkeley. **Ray fully integrates with the Python environment** and is easy to install by simply running `pip install ray`. The implementation comprises ≈ **40K lines of code (LoC), 72% in C++ for the system layer, 28% in Python for the application layer**.
>
> The GCS uses one Redis [50] key-value store per shard, with entirely single-key operations. GCS tables are sharded by object and task IDs to scale, and every shard is chainreplicated [61] for fault tolerance.
>
> We implement both the local and global schedulers as event-driven, singlethreaded processes. Internally, local schedulers maintain cached state for local object metadata, tasks waiting for inputs, and tasks ready for dispatch to a worker.
>
> To transfer large objects between different object stores, we stripe the object across multiple TCP connections.

Ray 的关键思想有两个：
- 将所有的系统控制状态存储在全局中心（global control store），这样系统的其它部分就可以是无状态的（应该是说需要的时候从中心“查”就可以了），节点可以轻易地水平扩展和重启；为了实现更好的性能，global control store 允许分片（sharding）和复制 (replication)。

  Ray 没有采用中心任务调度的方案，而是采用了类似层级（hierarchy）调度的方案，除了一个全局的中心调度服务节点（实际上这个中心调度节点也是可以水平拓展的），任务的调度也可以在具体的执行任务的工作节点上，由本地调度服务来管理和执行。

- 设计了一个自底向上的分布式 scheduler , 分为 local 和 global , local 负责调度任务执行或转发任务，每个节点有一个 local scheduler，由 driver 提交任务，worker 来执行。Object Store 存储了各种值，例如 被 remote 修饰的远程函数，提交的数据， 任务状态等等。

  与传统的层级调度方案，至上而下分配调度任务的方式不同的是，Ray 采用了至下而上的调度策略。也就是说，任务调度的发起，并不是先提交给全局的中心调度器统筹规划以后再分发给次级调度器的。而是由任务执行节点直接提交给本地的调度器，本地的调度器如果能满足该任务的调度需求就直接完成调度请求，在无法满足的情况下，才会提交给全局调度器，由全局调度器协调转发给有能力满足需求的另外一个节点上的本地调度器去调度执行。

  这么做的好处，一方面减少了跨节点的 RPC 开销，另一方面也能规避中心节点的瓶颈问题。当然缺点也不是没有，由于缺乏全局的任务视图，无法进行全局规划，因此任务的拓扑逻辑结构也就未必是最优的了。

## 1. Application Layer

### 1.1. Driver

> A process executing the user program.

### 1.2. Worker

> A stateless process that executes tasks (remote functions) invoked by a driver or another worker. Workers are started automatically and assigned tasks by the system layer. When a remote function is declared, the function is automatically published to all workers. A worker executes tasks serially, with no local state maintained across tasks.

### 1.3. Actor

> A stateful process that executes, when invoked, only the methods it exposes. Unlike a worker, an actor is explicitly instantiated by a worker or a driver. Like workers, actors execute methods serially, except that each method depends on the state resulting from the previous method execution

## 2. System Layer

The system layer consists of three major components:
- a global control store
- a distributed scheduler
- a distributed object store
All components are horizontally scalable and fault-tolerant.

### 2.1. Global Control Store (GCS)

### 2.2. Bottom-Up Distributed Scheduler

### 2.3. In-Memory Distributed Object Store

To minimize task latency, we implement an in-memory distributed storage system to store the inputs and outputs of every task, or stateless computation.

**On each node, we implement the object store via shared memory**. This allows zero-copy data sharing between tasks running on the same node. **As a data format, we use Apache Arrow**.

If a task’s inputs are not local, the inputs are replicated to the local object store before execution. Also, a task writes its outputs to the local object store. Replication eliminates the potential bottleneck due to hot data objects and minimizes task execution time as a task only reads/writes data from/to the local memory. This increases throughput for computation-bound workloads, a proﬁle shared by many AI applications. For low latency, we **keep objects entirely in memory** and **evict them as needed to disk using an LRU policy**.

As with existing cluster computing frameworks, such as Spark [64], and Dryad [28], the object store is limited to immutable data. This obviates the need for complex consistency protocols (as objects are not updated), and simpliﬁes support for fault tolerance. In the case of node failure, Ray recovers any needed objects through lineage re-execution. The lineage stored in the GCS tracks both stateless tasks and stateful actors during initial execution; we use the former to reconstruct objects in the store.

For simplicity, our object store does not support distributed objects, i.e., each object ﬁts on a single node. Distributed objects like large matrices or trees can be implemented at the application level as collections of futures.

https://arrow.apache.org/docs/python/plasma.html

https://arrow.apache.org/docs/python/plasma.html#using-arrow-and-pandas-with-plasma



## 3. Refer Links

[Ray Github](https://github.com/ray-project/ray)

[Ray Docs](https://ray.readthedocs.io/en/latest/index.html)

[Ray: A Distributed Framework for Emerging AI Applications](https://arxiv.org/pdf/1712.05889.pdf)

[Real-Time Machine Learning: The Missing Pieces](https://arxiv.org/pdf/1703.03924.pdf)

[Ray - 面向增强学习场景的分布式计算框架](https://blog.csdn.net/colorant/article/details/80417412)

[知乎：如何看 UCBerkeley RISELab 即将问世的 Ray，replacement of Spark？](https://www.zhihu.com/question/265485941)

[Intel Courses: Distributed AI with the Ray Framework](https://software.intel.com/en-us/ai/courses/distributed-AI-ray)

[Ray：面向 AI 应用的分布式执行框架](https://www.infoq.cn/article/ray-ai-framework)

[取代 Python 多进程！伯克利开源分布式框架 Ray](https://www.infoq.cn/article/6_7CfthGiXg0aytptoai)
