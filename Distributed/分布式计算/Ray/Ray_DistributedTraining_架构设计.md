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

Global Control Store (GCS) 是 Ray 系统的一个核心部件，通过一个或多个 Redis 服务器实现。GCS 的主要功能可以分为以下部分：
- 系统控制状态的持久性存储。
- 进程之间通信的消息总线（通过 Redis 的发布 - 订阅功能）。

实例：远程函数定义和调用的实现分析
- 定义
  ```python
  @ray.remote
  def f(x):
      return x + 1
  ```
  1. 当远程函数被定义时，将立即对该函数进行 pickle，并分配一个唯一的 ID，然后将其存储在 Redis 服务器中。
  1. 每个 worker 进程都有一个单独的线程用于侦听添加到集中控制状态的远程函数，当添加一个新的远程函数时，线程获取 pickle 的远程函数并进行 unpickle 后，就可以在 worker 进程中执行该函数。

  NOTE：由于远程函数一旦定义就会立即导出，这意味着远程函数不能关闭在远程函数定义之后定义的变量。

- 调用

  当驱动程序或 worker 调用远程函数时：
  1. 首先，创建一个 task 对象。任务对象包括以下内容。
    - 被调用函数的 ID。
    - 函数参数的 ID 或值。 像整数或短字符串这样的 Python 原语将被 pickle 并作为任务对象的一部分包含在内。 通过对 ray.put 的内部调用，将更大或更复杂的对象放入对象库中，并将结果 ID 包含在任务对象中。 直接作为参数传递的对象 ID 也包含在任务对象中。
    - 任务的 ID。这是从上面的内容中唯一生成的。（对应上边生成内容的 id）。
    - 任务返回值的 id。这些都是由上面的内容惟一生成的。

  1. 然后，任务对象被发送到与驱动程序或 worker 程序相同节点上的 raylet。

  1. raylet 决定是在本地调度任务，还是将任务传递给另一个 raylet。
    - 如果任务的所有对象依赖项都存在于本地对象存储中，并且有足够的 CPU 和 GPU 资源可用来执行任务，那么本地 raylet 将把任务分配给它的一个可用 worker。
    - 如果不满足这些条件，任务将被转发给另一个 raylet。这是通过 raylet 之间的对等连接完成的。任务表可以检查如下。

  1. 一旦一个任务被调度到一个 raylet 中，raylet 就会对任务进行排队等待执行。当有足够的资源可用并且对象依赖项在本地可用时，按先入先出的顺序将任务分配给工作人员。
  1. 将任务分配给 worker 后，worker 将执行该任务并将任务的返回值放入对象存储中。 然后，对象存储将更新对象表，该对象表是集中控制状态的一部分，以反映它包含新创建的对象的事实。 对象表可以如下查看。
  1. 当将任务的返回值放入对象存储中时，首先使用 Apache Arrow 数据布局将它们序列化为一个连续的字节团，这有助于在使用共享内存的进程之间高效地共享数据。

### 2.2. Bottom-Up Distributed Scheduler

### 2.3. In-Memory Distributed Object Store

To minimize task latency, we implement an in-memory distributed storage system to store the inputs and outputs of every task, or stateless computation.

**On each node, we implement the object store via shared memory**. This allows zero-copy data sharing between tasks running on the same node. **As a data format, we use Apache Arrow**.

If a task’s inputs are not local, the inputs are replicated to the local object store before execution. Also, a task writes its outputs to the local object store. Replication eliminates the potential bottleneck due to hot data objects and minimizes task execution time as a task only reads/writes data from/to the local memory. This increases throughput for computation-bound workloads, a proﬁle shared by many AI applications. For low latency, we **keep objects entirely in memory** and **evict them as needed to disk using an LRU policy**.

As with existing cluster computing frameworks, such as Spark [64], and Dryad [28], the object store is limited to immutable data. This obviates the need for complex consistency protocols (as objects are not updated), and simpliﬁes support for fault tolerance. In the case of node failure, Ray recovers any needed objects through lineage re-execution. The lineage stored in the GCS tracks both stateless tasks and stateful actors during initial execution; we use the former to reconstruct objects in the store.

For simplicity, our object store does not support distributed objects, i.e., each object ﬁts on a single node. Distributed objects like large matrices or trees can be implemented at the application level as collections of futures.

https://arrow.apache.org/docs/python/plasma.html

https://arrow.apache.org/docs/python/plasma.html#using-arrow-and-pandas-with-plasma

https://ray-project.github.io/2017/10/15/fast-python-serialization-with-ray-and-arrow.html

Ray 中的对象存储主要可分为以下部分：
- 对象放入存储对象的序列化和从对象调出的反序列化。

  Python 对象可以有任意数量的指针，指针的深度可以任意嵌套。要将对象放入对象存储区或在进程之间发送对象，必须首先将其转换为连续的字节字符串。这个过程称为序列化。将字节串转换回 Python 对象的过程称为反序列化。序列化和反序列化常常是分布式计算的瓶颈。

- 特殊的 numpy 数组的 Apache Arrow 化存储。

  对于数值工作负载，pickle 和反 pickle 可能效率不高。例如，如果多个进程希望访问一个由 numpy 数组组成的 Python 列表，则每个进程必须反序列化（unpickle ）该列表，并创建自己的新数组副本。即使所有进程都是只读的，并且可以很容易地共享内存，这也可能导致高内存开销。在 Ray 中，我们使用 Apache Arrow 数据格式对 numpy 数组进行优化。当我们从对象存储区反序列化一个 numpy 数组列表时，我们仍然创建一个 Python 的 numpy 数组对象列表。但是，不是复制每个 numpy 数组，而是每个 numpy 数组对象持有一个指向共享内存中保存的相关数组的指针。这种序列化形式有一些优点：
  - 反序列化可以非常快。
  - 内存在进程之间共享，因此 worker 进程可以读取相同的数据，而不必复制它。

Ray 把对象放入对象存储中的几种情况：
- 远程函数的返回值。
- 调用 ray.put(x) 中的值 x。
- 远程函数的参数（除了像 int 或 float 这样的简单参数)。




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

TODO:

[Ray 论文解读](http://whatbeg.com/2018/03/15/ray-paper.html)

[Ray -- 内部运行机制、对象存储中对象的存储和容错](https://blog.csdn.net/weixin_43255962/article/details/89684608)

[Ray- Plasma 的对象存储、资源管理（cpu，gpu）和临时文件](https://blog.csdn.net/weixin_43255962/article/details/89712162)

[Ray: A Distributed Execution Framework for AI Applications](https://ray-project.github.io/2017/05/20/announcing-ray.html)


