- [Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：AQS 框架](#java-多线程---线程安全---阻塞同步方案---api-层面aqs-框架)
    - [1. 基本概念](#1-基本概念)
    - [2. 提供方法](#2-提供方法)
    - [3. 工作原理](#3-工作原理)
        - [3.1. 类定义](#31-类定义)
        - [3.2. 关键属性](#32-关键属性)
        - [3.3. 内部类](#33-内部类)
        - [3.4. 模板方法](#34-模板方法)
        - [3.5. 其它方法](#35-其它方法)
    - [4. Refer Links](#4-refer-links)

# Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：AQS 框架

## 1. 基本概念

队列同步器 AQS([AbstractQueuedSynchronizer](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/AbstractQueuedSynchronizer.html)) 是 Java 构建锁或其他同步组件的基础框架，一般以静态内部类的形式实现在某个同步组件类中，并通过代理的方式向外提供同步服务。

AQS 实现同步器时涉及大量细节问题，例如获取同步状态、FIFO 同步队列，基于 AQS 来构建同步器可以带来很多好处：

- **它不仅能够极大地减少实现工作，而且也不必处理在多个位置上发生的竞争问题。**
- 在基于 AQS 构建的同步器中，只能在一个时刻发生阻塞，从而降低上下文切换的开销，提高了吞吐量。
- 同时在设计 AQS 时充分考虑了可伸缩行，因此 J.U.C 中所有基于 AQS 构建的同步器均可以获得这个优势。

## 2. 提供方法

AQS 主要提供了如下一些方法实现：
- `getState()`: 返回同步状态的当前值；
- `setState(int newState)`: 设置当前同步状态；
- `compareAndSetState(int expect, int update)`: 使用 CAS 设置当前状态，该方法能够保证状态设置的原子性；
- `tryAcquire(int arg)`: 独占式获取同步状态，获取同步状态成功后，其他线程需要等待该线程释放同步状态才能获取同步状态；
- `tryRelease(int arg)`: 独占式释放同步状态；
- `tryAcquireShared(int arg)`: 共享式获取同步状态，返回值大于等于 0 则表示获取成功，否则获取失败；
- `tryReleaseShared(int arg)`: 共享式释放同步状态；
- `isHeldExclusively()`: 当前同步器是否在独占式模式下被线程占用，一般该方法表示是否被当前线程所独占；
- `acquire(int arg)`: 独占式获取同步状态，如果当前线程获取同步状态成功，则由该方法返回，否则，将会进入同步队列等待，该方法将会调用可重写的 tryAcquire(int arg) 方法；
- `acquireInterruptibly(int arg)：与 acquire(int arg)`: 相同，但是该方法响应中断，当前线程为获取到同步状态而进入到同步队列中，如果当前线程被中断，则该方法会抛出 InterruptedException 异常并返回；
- `tryAcquireNanos(int arg,long nanos)`: 超时获取同步状态，如果当前线程在 nanos 时间内没有获取到同步状态，那么将会返回 false，已经获取则返回 true；
- `acquireShared(int arg)`: 共享式获取同步状态，如果当前线程未获取到同步状态，将会进入同步队列等待，与独占式的主要区别是在同一时刻可以有多个线程获取到同步状态；
- `acquireSharedInterruptibly(int arg)`: 共享式获取同步状态，响应中断；
- `tryAcquireSharedNanos(int arg, long nanosTimeout)`: 共享式获取同步状态，增加超时限制；
- `release(int arg)`: 独占式释放同步状态，该方法会在释放同步状态之后，将同步队列中第一个节点包含的线程唤醒；
- `releaseShared(int arg)`: 共享式释放同步状态；

## 3. 工作原理

**AQS 作为基础组件，对于锁的实现存在两种不同的模式，即共享模式（如 Semaphore) 和独占模式（如 ReetrantLock)，无论是共享模式还是独占模式的实现类，其内部都是基于 AQS 实现的**，也都维持着一个虚拟的同步队列，当请求锁的线程超过现有模式的限制时，会将线程包装成 Node 结点并将线程当前必要的信息存储到 node 结点中，然后加入同步队列等会获取锁，而这系列操作都有 AQS 协助我们完成，这也是称 AQS 为基础组件的原因。

### 3.1. 类定义

```java
public abstract class AbstractQueuedSynchronizer
    extends AbstractOwnableSynchronizer
    implements java.io.Serializable
```

### 3.2. 关键属性

```java
/**
  * Head of the wait queue, lazily initialized.  Except for
  * initialization, it is modified only via method setHead.  Note:
  * If head exists, its waitStatus is guaranteed not to be
  * CANCELLED.
  */
private transient volatile Node head;

/**
  * Tail of the wait queue, lazily initialized.  Modified only via
  * method enq to add new wait node.
  */
private transient volatile Node tail;

/**
  * The synchronization state.
  */
private volatile int state;
```

AQS 内部通过一个 int 类型的成员变量 state 来控制同步状态：
- 当 state=0 时，则说明没有任何线程占有共享资源的锁。
- 当 state=1 时，则说明有线程目前正在使用共享变量，其他线程必须加入同步队列进行等待，AQS 内部通过内部类 Node 构成 FIFO 的同步队列来完成线程获取锁的排队工作，同时利用内部类 ConditionObject 构建等待队列。
  - 当 Condition 调用 wait() 方法后，线程将会加入等待队列中。
  - 当 Condition 调用 signal() 方法后，线程将从等待队列转移动同步队列中进行锁竞争。
  
  注意**这里涉及到两种队列，一种是**同步队列**，由于请求锁而等待的线程将加入同步队列，而另一种则是**等待队列**（可有多个)，通过 Condition 调用 await() 方法释放锁后，将加入等待队列**。

head 指向同步队列的头部，注意 head 为空结点，不存储信息，而 tail 则是同步队列的队尾。

**同步队列采用的是双向链表的结构这样可方便队列进行结点增删操作**。执行当前线程调用 lock 方法进行加锁后：
- 如果此时 state 的值为 0，则将 state 设置为 1，表示获取成功。
- 如果 state 已为 1，也就是当前锁已被其他线程持有，那么当前执行线程将被封装为 Node 结点加入同步队列等待。

### 3.3. 内部类

**AQS 的内部类 Node 是对每一个访问同步代码的线程的封装，其包含了需要同步的线程本身以及线程的状态，如是否被阻塞，是否等待唤醒，是否已经被取消等**。每个 Node 结点内部关联其前继结点 prev 和后继结点 next，这样可以方便线程释放锁后快速唤醒下一个在等待的线程。

```java
static final class Node {
    // 共享模式
    static final Node SHARED = new Node();
    // 独占模式
    static final Node EXCLUSIVE = null;

    // 标识线程已处于结束状态
    static final int CANCELLED =  1;
    // 等待被唤醒状态
    static final int SIGNAL    = -1;
    // 条件状态，
    static final int CONDITION = -2;
    // 在共享模式中使用表示获得的同步状态会被传播
    static final int PROPAGATE = -3;

    // 等待状态，存在 CANCELLED、SIGNAL、CONDITION、PROPAGATE 4 种
    volatile int waitStatus;

    // 同步队列中前驱结点
    volatile Node prev;

    // 同步队列中后继结点
    volatile Node next;

    // 请求锁的线程
    volatile Thread thread;

    // 等待队列中的后继结点，这个与 Condition 有关，稍后会分析
    Node nextWaiter;

    // 判断是否为共享模式
    final boolean isShared() {
        return nextWaiter == SHARED;
    }

    // 获取前驱结点
    final Node predecessor() throws NullPointerException {
        Node p = prev;
        if (p == null)
            throw new NullPointerException();
        else
            return p;
    }

    //.....
}
```
其中：
- **SHARED 和 EXCLUSIVE 常量分别代表共享模式和独占模式**
  - 所谓共享模式是一个锁允许多条线程同时操作，如**信号量 Semaphore 采用的就是基于 AQS 的共享模式实现的**。
  - 独占模式是同一个时间段只能有一个线程对共享资源进行操作，多余的请求线程需要排队等待，如 **ReentranLock 采用的就是基于 AQS 的独占模式实现的**。
- waitStatus 变量则表示当前被封装成 Node 结点的等待状态，共有 4 种取值 CANCELLED、SIGNAL、CONDITION、PROPAGATE。
  - CANCELLED：值为 1，在同步队列中等待的线程等待超时或被中断，需要从同步队列中取消该 Node 的结点，其结点的 waitStatus 为 CANCELLED，即结束状态，进入该状态后的结点将不会再变化。

  - SIGNAL：值为 -1，被标识为该等待唤醒状态的后继结点，当其前继结点的线程释放了同步锁或被取消，将会通知该后继结点的线程执行。说白了，就是处于唤醒状态，只要前继结点释放锁，就会通知标识为 SIGNAL 状态的后继结点的线程执行。

  - CONDITION：值为 -2，与 Condition 相关，该标识的结点处于等待队列中，结点的线程等待在 Condition 上，当其他线程调用了 Condition 的 signal() 方法后，CONDITION 状态的结点将从等待队列转移到同步队列中，等待获取同步锁。

  - PROPAGATE：值为 -3，与共享模式相关，在共享模式中，该状态标识结点的线程处于可运行状态。

  - 0 状态：值为 0，代表初始化状态。
- pre 和 next，分别指向当前 Node 结点的前驱结点和后继结点。
- thread 变量存储的请求锁的线程。
- nextWaiter 与 Condition 相关，代表等待队列中的后继结点。

### 3.4. 模板方法

```java
public abstract class AbstractOwnableSynchronizer
    implements java.io.Serializable {
    protected AbstractOwnableSynchronizer() { }

    private transient Thread exclusiveOwnerThread;

    protected final void setExclusiveOwnerThread(Thread thread) {
        exclusiveOwnerThread = thread;
    }

    protected final Thread getExclusiveOwnerThread() {
        return exclusiveOwnerThread;
    }
}
```

AQS 虽是一个扩展自 AbstractOwnableSynchronizer 的抽象类，但其源码中并没一个抽象的方法，这是因为 AQS 只是作为一个基础组件，并不希望直接作为直接操作类对外输出，而**更倾向于作为基础组件，为真正的实现类提供基础设施，如构建同步队列，控制同步状态等**。

事实上，**从设计模式角度来看，AQS 是采用的模板模式的方式构建的，其内部除了提供并发操作核心方法以及同步队列操作外，还提供了一些模板方法让子类自己实现，如加锁操作以及解锁操作**。

为什么这么做？这是因为 AQS 作为基础组件，封装的是核心并发操作，但是实现上分为两种模式，即共享模式与独占模式，而这两种模式的加锁与解锁实现方式是不一样的，但 AQS 只关注内部公共方法实现并不关心外部不同模式的实现，所以提供了模板方法给子类使用。也就是说实现独占锁，如 ReentrantLock 需要自己实现 tryAcquire() 方法和 tryRelease() 方法，而实现共享模式的 Semaphore，则需要实现 tryAcquireShared() 方法和 tryReleaseShared() 方法。

这样做的好处是显而易见的，无论是共享模式还是独占模式，其基础的实现都是同一套组件 (AQS)，只不过是加锁解锁的逻辑不同罢了，更重要的是如果我们需要自定义锁的话，也变得非常简单，只需要选择不同的模式实现不同的加锁和解锁的模板方法即可。

AQS 提供给独占模式和共享模式的模板方法（由子类实现）如下：
```java
// 独占模式下获取锁的方法
protected boolean tryAcquire(int arg) {
    throw new UnsupportedOperationException();
}

// 独占模式下解锁的方法
protected boolean tryRelease(int arg) {
    throw new UnsupportedOperationException();
}

// 共享模式下获取锁的方法
protected int tryAcquireShared(int arg) {
    throw new UnsupportedOperationException();
}

// 共享模式下解锁的方法
protected boolean tryReleaseShared(int arg) {
    throw new UnsupportedOperationException();
}
// 判断是否为持有独占锁
protected boolean isHeldExclusively() {
    throw new UnsupportedOperationException();
}
```

### 3.5. 其它方法

```java
protected final int getState() {
    return state;
}

protected final void setState(int newState) {
    state = newState;
}

// 封装对 state 进行的 CAS 操作
protected final boolean compareAndSetState(int expect, int update) {
    // See below for intrinsics setup to support this
    return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}

public final void acquire(int arg) {
    // 再次尝试获取同步状态
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}

private Node addWaiter(Node mode) {
    // 将请求同步状态失败的线程封装成结点
    Node node = new Node(Thread.currentThread(), mode);

    Node pred = tail;
    // 如果是第一个结点加入肯定为空，跳过。
    // 如果非第一个结点则直接执行 CAS 入队操作，尝试在尾部快速添加
    if (pred != null) {
        node.prev = pred;
        // 使用 CAS 执行尾部结点替换，尝试在尾部快速添加
        if (compareAndSetTail(pred, node)) {
            pred.next = node;
            return node;
        }
    }
    // 如果第一次加入或者 CAS 操作没有成功执行 enq 入队操作
    enq(node);
    return node;
}

private Node enq(final Node node) {
    // 死循环
    for (;;) {
        Node t = tail;
        // 如果队列为 null，即没有头结点
        if (t == null) { // Must initialize
            // 创建并使用 CAS 设置头结点
            if (compareAndSetHead(new Node()))
                tail = head;
        } else {// 队尾添加新结点
            node.prev = t;
            if (compareAndSetTail(t, node)) {
                t.next = node;
                return t;
            }
        }
    }
}

// 自旋操作
final boolean acquireQueued(final Node node, int arg) {
    boolean failed = true;
    try {
        boolean interrupted = false;
        // 自旋，死循环
        for (;;) {
            // 获取前驱结点
            final Node p = node.predecessor();
            当且仅当 p 为头结点才尝试获取同步状态
            if (p == head && tryAcquire(arg)) {
                // 将 node 设置为头结点
                setHead(node);
                // 清空原来头结点的引用便于 GC
                p.next = null; // help GC
                failed = false;
                return interrupted;
            }
            // 如果前驱结点不是 head，判断是否挂起线程
            if (shouldParkAfterFailedAcquire(p, node) &&
                parkAndCheckInterrupt())
                interrupted = true;
        }
    } finally {
        if (failed)
            // 最终都没能获取同步状态，结束该线程的请求
            cancelAcquire(node);
    }
}
```

## 4. Refer Links

[JAVA REENTRANTLOCK、SEMAPHORE 的实现与 AQS 框架](http://niceaz.com/java-reentrantlock%E3%80%81semaphore%E7%9A%84%E5%AE%9E%E7%8E%B0%E4%B8%8Eaqs%E6%A1%86%E6%9E%B6/)

[深入剖析基于并发 AQS 的（独占锁) 重入锁 (ReetrantLock) 及其 Condition 实现原理](https://blog.csdn.net/javazejian/article/details/75043422)

[剖析基于并发 AQS 的共享锁的实现（基于信号量 Semaphore)](https://blog.csdn.net/javazejian/article/details/76167357)

[【死磕 Java 并发】-----J.U.C 之 AQS：AQS 简介](https://blog.csdn.net/chenssy/article/details/60479594)

[Java 多线程（七）之同步器基础：AQS 框架深入分析](https://blog.csdn.net/vernonzheng/article/details/8275624)