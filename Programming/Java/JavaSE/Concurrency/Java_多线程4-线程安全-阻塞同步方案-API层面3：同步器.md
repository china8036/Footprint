- [Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：同步器](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---%E9%98%BB%E5%A1%9E%E5%90%8C%E6%AD%A5%E6%96%B9%E6%A1%88---api-%E5%B1%82%E9%9D%A2%EF%BC%9A%E5%90%8C%E6%AD%A5%E5%99%A8)
	- [1. 信号量 Semaphore](#1-%E4%BF%A1%E5%8F%B7%E9%87%8F-semaphore)
		- [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [1.2. 基本 API](#12-%E5%9F%BA%E6%9C%AC-api)
		- [1.3. 源码分析](#13-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
			- [1.3.1. 类定义](#131-%E7%B1%BB%E5%AE%9A%E4%B9%89)
			- [1.3.2. 内部类](#132-%E5%86%85%E9%83%A8%E7%B1%BB)
			- [1.3.3. 构造方法](#133-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
			- [1.3.4. 关键方法](#134-%E5%85%B3%E9%94%AE%E6%96%B9%E6%B3%95)
	- [2. 倒计时门栓 CountDownLatch](#2-%E5%80%92%E8%AE%A1%E6%97%B6%E9%97%A8%E6%A0%93-countdownlatch)
		- [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [2.2. 基本 API](#22-%E5%9F%BA%E6%9C%AC-api)
		- [2.3. 源码分析](#23-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
			- [2.3.1. 类定义](#231-%E7%B1%BB%E5%AE%9A%E4%B9%89)
			- [2.3.2. 内部类](#232-%E5%86%85%E9%83%A8%E7%B1%BB)
			- [2.3.3. 构造方法](#233-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
			- [2.3.4. 关键方法](#234-%E5%85%B3%E9%94%AE%E6%96%B9%E6%B3%95)
	- [3. 回环栅栏 CyclicBarrier](#3-%E5%9B%9E%E7%8E%AF%E6%A0%85%E6%A0%8F-cyclicbarrier)
		- [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [3.2. 基本 API](#32-%E5%9F%BA%E6%9C%AC-api)
	- [4. 交换器 Exchanger](#4-%E4%BA%A4%E6%8D%A2%E5%99%A8-exchanger)
	- [5. 阻塞队列 BlockingQueue](#5-%E9%98%BB%E5%A1%9E%E9%98%9F%E5%88%97-blockingqueue)
	- [6. Refer Linked](#6-refer-linked)

# Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：同步器

从 JDK1.5 开始，Java 在 J.U.C 中提供了多个基于 AQS 实现的辅助同步器供开发者使用。

## 1. 信号量 Semaphore

### 1.1. 基本概念

信号量 [Semaphore](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Semaphore.html) 可以控制同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。

### 1.2. 基本 API

Semaphore 提供了 2 个构造器：
- `Semaphore​(int permits)`: Creates a Semaphore with the given number of permits and nonfair fairness setting.
- `Semaphore​(int permits, boolean fair)`: Creates a Semaphore with the given number of permits and the given fairness setting.
	- permits 表示运行同时访问的线程数。
	- fair 表示是否公平，即等待时间越久的越先获取许可。

阻塞方法：
- `void acquire()`: Acquires a permit from this semaphore, blocking until one is available, or the thread is interrupted.
- `void acquire(int permits)`: Acquires the given number of permits from this semaphore, blocking until all are available, or the thread is interrupted.
- `void release()`: Releases a permit, returning it to the semaphore.
- `void release(int permits)`: Releases the given number of permits, returning them to the semaphore.

非阻塞方法：
- `boolean tryAcquire()`: 尝试获取一个许可，若获取成功，则立即返回 true，若获取失败，则立即返回 false。
- `boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException`: 尝试获取一个许可，若在指定的时间内获取成功，则立即返回 true，否则则立即返回 false。
- `boolean tryAcquire(int permits)`: 尝试获取 permits 个许可，若获取成功，则立即返回 true，若获取失败，则立即返回 false。
- `boolean tryAcquire(int permits, long timeout, TimeUnit unit) throws InterruptedException`: 尝试获取 permits 个许可，若在指定的时间内获取成功，则立即返回 true，否则则立即返回 false。

其它方法：
- `int availablePermits()`: 获取当前剩余的信号量数量。
- `int drainPermits()`: 获取并返回立即可用的所有许可。例如总共有 10 个信号量，已经使用了 5 个，再调用 drainPermits 方法后，可以获得一个信号量，剩余 4 个信号量就消失了，总共可用的信号量就变成 6 个了。
- `protected void reducePermits(int reduction)`: 减少信号量个数。

e.g. 假若一个工厂有 5 台机器，但是有 8 个工人，一台机器同时只能被一个工人使用，只有使用完了，其他工人才能继续使用。那么我们就可以通过 Semaphore 来实现：
```java
public class Test {
    public static void main(String[] args) {
        int N = 8;            // 工人数
        Semaphore semaphore = new Semaphore(5); // 机器数目
        for(int i=0;i<N;i++)
            new Worker(i,semaphore).start();
    }
     
    static class Worker extends Thread{
        private int num;
        private Semaphore semaphore;
        public Worker(int num,Semaphore semaphore){
            this.num = num;
            this.semaphore = semaphore;
        }
         
        @Override
        public void run() {
            try {
                semaphore.acquire();
                System.out.println("工人"+this.num+"占用一个机器在生产。..");
                Thread.sleep(2000);
                System.out.println("工人"+this.num+"释放出机器");
                semaphore.release();           
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
运行结果：
```
工人 0 占用一个机器在生产 ...
工人 1 占用一个机器在生产 ...
工人 2 占用一个机器在生产 ...
工人 4 占用一个机器在生产 ...
工人 5 占用一个机器在生产 ...
工人 0 释放出机器
工人 2 释放出机器
工人 3 占用一个机器在生产 ...
工人 7 占用一个机器在生产 ...
工人 4 释放出机器
工人 5 释放出机器
工人 1 释放出机器
工人 6 占用一个机器在生产 ...
工人 3 释放出机器
工人 7 释放出机器
工人 6 释放出机器
```

### 1.3. 源码分析

#### 1.3.1. 类定义

```java
public class Semaphore implements java.io.Serializable {}
```

#### 1.3.2. 内部类

Semaphore 通过使用内部类 Sync 继承 AQS 来实现，使用 AQS 的共享锁。
```java
abstract static class Sync extends AbstractQueuedSynchronizer {
		private static final long serialVersionUID = 1192457210091910933L;

		Sync(int permits) {
				setState(permits);
		}

		final int getPermits() {
				return getState();
		}

		final int nonfairTryAcquireShared(int acquires) {
				for (;;) {
						int available = getState();
						int remaining = available - acquires;
						if (remaining < 0 ||
								compareAndSetState(available, remaining))
								return remaining;
				}
		}

		protected final boolean tryReleaseShared(int releases) {
				for (;;) {
						int current = getState();
						int next = current + releases;
						if (next < current) // overflow
								throw new Error("Maximum permit count exceeded");
						if (compareAndSetState(current, next))
								return true;
				}
		}

		final void reducePermits(int reductions) {
				for (;;) {
						int current = getState();
						int next = current - reductions;
						if (next > current) // underflow
								throw new Error("Permit count underflow");
						if (compareAndSetState(current, next))
								return;
				}
		}

		final int drainPermits() {
				for (;;) {
						int current = getState();
						if (current == 0 || compareAndSetState(current, 0))
								return current;
				}
		}
}

/**
	* NonFair version
	*/
static final class NonfairSync extends Sync {
		private static final long serialVersionUID = -2694183684443567898L;

		NonfairSync(int permits) {
				super(permits);
		}

		protected int tryAcquireShared(int acquires) {
				return nonfairTryAcquireShared(acquires);
		}
}

/**
	* Fair version
	*/
static final class FairSync extends Sync {
		private static final long serialVersionUID = 2014338818796000944L;

		FairSync(int permits) {
				super(permits);
		}

		protected int tryAcquireShared(int acquires) {
				for (;;) {
						if (hasQueuedPredecessors())
								return -1;
						int available = getState();
						int remaining = available - acquires;
						if (remaining < 0 ||
								compareAndSetState(available, remaining))
								return remaining;
				}
		}
}
```

#### 1.3.3. 构造方法

```java
// 构造方法指定信号量的许可数量，默认采用的是非公平锁，也只可以指定为公平锁，permits 赋值给 AQS 中的 state 变量
public Semaphore(int permits) {
		sync = new NonfairSync(permits);
}

public Semaphore(int permits, boolean fair) {
		sync = fair ? new FairSync(permits) : new NonfairSync(permits);
}
```

#### 1.3.4. 关键方法

```java
public void acquire() throws InterruptedException {
		sync.acquireSharedInterruptibly(1);
}
public void acquire(int permits) throws InterruptedException {
		if (permits < 0) throw new IllegalArgumentException();
		sync.acquireSharedInterruptibly(permits);
}

public boolean tryAcquire() {
		return sync.nonfairTryAcquireShared(1) >= 0;
}
public boolean tryAcquire(long timeout, TimeUnit unit)
		throws InterruptedException {
		return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
}
public boolean tryAcquire(int permits) {
		if (permits < 0) throw new IllegalArgumentException();
		return sync.nonfairTryAcquireShared(permits) >= 0;
}

public boolean tryAcquire(int permits, long timeout, TimeUnit unit)
		throws InterruptedException {
		if (permits < 0) throw new IllegalArgumentException();
		return sync.tryAcquireSharedNanos(permits, unit.toNanos(timeout));
}
public void release() {
		sync.releaseShared(1);
}
public void release(int permits) {
		if (permits < 0) throw new IllegalArgumentException();
		sync.releaseShared(permits);
}
```

## 2. 倒计时门栓 CountDownLatch

### 2.1. 基本概念

倒计时门栓 [CountDownLatch](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/CountDownLatch.html) 可以实现类似计数器的功能。比如有一个任务 A，它要等待其他 n 个任务执行完毕之后才能执行，此时就可以利用 CountDownLatch 来实现这种功能了。

### 2.2. 基本 API

- `CountDownLatch(int count)`: Constructs a CountDownLatch initialized with the given count.
- `void countDown()`: Decrements the count of the latch, releasing all waiting threads if the count reaches zero.
- `long getCount()`: Returns the current count.
- `void wait()`: Causes the current thread to wait until the latch has counted down to zero, unless the thread is interrupted.
- `boolean await(long timeout, TimeUnit unit)`: Causes the current thread to wait until the latch has counted down to zero, unless the thread is interrupted, or the specified waiting time elapses.

e.g.
```java
 public static void main(String[] args) {   
     final CountDownLatch latch = new CountDownLatch(2);
     new Thread(() -> {
            try {
                 System.out.println("子线程"+Thread.currentThread().getName()+"正在执行");
                 Thread.sleep(3000);
                 System.out.println("子线程"+Thread.currentThread().getName()+"执行完毕");
                 latch.countDown();
            } catch (InterruptedException e) {
                 e.printStackTrace();
            }
     }).start();

     new Thread(() -> {
            try {
                 System.out.println("子线程"+Thread.currentThread().getName()+"正在执行");
                 Thread.sleep(3000);
                 System.out.println("子线程"+Thread.currentThread().getName()+"执行完毕");
                 latch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
     }).start();

    try {
        System.out.println("等待 2 个子线程执行完毕。..");
        latch.await();
        System.out.println("2 个子线程已经执行完毕");
        System.out.println("继续执行主线程");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
 }
```
运行结果：
```
线程 Thread-0 正在执行
线程 Thread-1 正在执行
等待 2 个子线程执行完毕 ...
线程 Thread-0 执行完毕
线程 Thread-1 执行完毕
2 个子线程已经执行完毕
继续执行主线程
```

### 2.3. 源码分析

#### 2.3.1. 类定义

```java
public class CountDownLatch {}
```

#### 2.3.2. 内部类

CountDownLatch 通过使用内部类 Sync 继承 AQS 来实现，使用 AQS 的共享锁。
```java
private static final class Sync extends AbstractQueuedSynchronizer {
		private static final long serialVersionUID = 4982264981922014374L;

		Sync(int count) {
				setState(count);
		}

		int getCount() {
				return getState();
		}

		protected int tryAcquireShared(int acquires) {
				// 只有当 state 为 0 即 count 为 0 的时候才能够返回
				return (getState() == 0) ? 1 : -1;
		}

		protected boolean tryReleaseShared(int releases) {
				// Decrement count; signal when transition to zero
				for (;;) {
						int c = getState();
						if (c == 0)
								return false;
						int nextc = c-1;
						if (compareAndSetState(c, nextc))
								// 成功 CAS 后，将 count 变为 0 的线程负责唤醒等待线程
								return nextc == 0;
				}
		}
}

private final Sync sync;
```

#### 2.3.3. 构造方法

```java
// 创建 CountDownLatch 对象时，需要传入一个 int 的计数器
public CountDownLatch(int count) {
		if (count < 0) throw new IllegalArgumentException("count < 0");
		this.sync = new Sync(count);
}
```

#### 2.3.4. 关键方法

```java
// await 方法调用 AQS 的获得锁的方法，只有 AQS 的 state 状态为 0 时才能获得锁，如果 state 不为 0，则需要在 AQS 的等待队列中阻塞等待
public void await() throws InterruptedException {
		sync.acquireSharedInterruptibly(1);
}
public boolean await(long timeout, TimeUnit unit)
		throws InterruptedException {
		return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
}
// countDown 方法则调用 AQS 的 releaseShared 方法，释放共享锁，也就是每次将 state 状态每次减一，直到减到 0，则唤醒队列中的所有节点（线程）
public void countDown() {
		sync.releaseShared(1);
}
// 获取计数器当前值
public long getCount() {
		return sync.getCount();
}
```

## 3. 回环栅栏 CyclicBarrier

### 3.1. 基本概念

回环栅栏 [CyclicBarrier](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/CyclicBarrier.html) 可以实现让一组线程互相等待至某个状态，然后这一组线程再同时执行。“回环”是因为当所有等待线程都被释放以后，CyclicBarrier 可以被重用。

### 3.2. 基本 API

CyclicBarrier 提供了 2 个构造器：
- `CyclicBarrier​(int parties)`: Creates a new CyclicBarrier that will trip when the given number of parties (threads) are waiting upon it, and does not perform a predefined action when the barrier is tripped.
- `CyclicBarrier​(int parties, Runnable barrierAction)`: Creates a new CyclicBarrier that will trip when the given number of parties (threads) are waiting upon it, and which will execute the given barrier action when the barrier is tripped, performed by the last thread entering the barrier.

其它方法：
- `int await()`: Waits until all parties have invoked await on this barrier.
- `int await(long timeout, TimeUnit unit)`: Waits until all parties have invoked await on this barrier, or the specified waiting time elapses.
- `int getNumberWaiting()`: Returns the number of parties currently waiting at the barrier.
- `int getParties()`: Returns the number of parties required to trip this barrier.
- `boolean isBroken()`: Queries if this barrier is in a broken state.
- `void reset()`: Resets the barrier to its initial state.

e.g. 假若有若干个线程都要进行写数据操作，并且只有所有线程都完成写数据操作之后，这些线程才能继续做后面的事情，此时就可以利用 CyclicBarrier 了：
```java
public static void main(String[] args) {
    int N = 4;
    CyclicBarrier barrier  = new CyclicBarrier(N);
    for(int i=0;i<N;i++)
        new Writer(barrier).start();
}
static class Writer extends Thread {
    private CyclicBarrier cyclicBarrier;
    public Writer(CyclicBarrier cyclicBarrier) {
        this.cyclicBarrier = cyclicBarrier;
    }

    @Override
    public void run() {
        System.out.println("线程"+Thread.currentThread().getName()+"正在写入数据 ...");
        try {
            Thread.sleep(5000);      // 以睡眠来模拟写入数据操作
            System.out.println("线程"+Thread.currentThread().getName()+"写入数据完毕，等待其他线程写入完毕");
            cyclicBarrier.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }catch(BrokenBarrierException e){
            e.printStackTrace();
        }
        System.out.println("所有线程写入完毕，继续处理其他任务。..");
    }
}
```
运行结果：
```
线程 Thread-0 正在写入数据 ...
线程 Thread-3 正在写入数据 ...
线程 Thread-2 正在写入数据 ...
线程 Thread-1 正在写入数据 ...
线程 Thread-2 写入数据完毕，等待其他线程写入完毕
线程 Thread-0 写入数据完毕，等待其他线程写入完毕
线程 Thread-3 写入数据完毕，等待其他线程写入完毕
线程 Thread-1 写入数据完毕，等待其他线程写入完毕
所有线程写入完毕，继续处理其他任务 ...
```

## 4. 交换器 Exchanger

交换器 [Exchanger](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Exchanger.html)

## 5. 阻塞队列 BlockingQueue

阻塞队列 [BlockingQueue](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/BlockingQueue.html)

Lock/Condition、synchronized/volatile 形成了 Java 并发程序设计的底层构建基础，但**在实际的业务开发中，应远离底层结构，转而使用由并发处理的专业人士实现的较高层次的结构**。

对于许多线程问题，可通过使用一个或多个队列以优雅安全的方式将其形式化。**生产者线程向队列中插入元素，而消费者线程想队列中取出元素，同步问题由队列类负责实现，当添加元素时队列已满、取出元素时队列为空，阻塞队列导致线程阻塞**。

## 6. Refer Linked

[Java 并发编程：CountDownLatch、CyclicBarrier 和 Semaphore](http://www.cnblogs.com/dolphin0520/p/3920397.html)

[Semaphore 源码分析](https://www.jianshu.com/p/d1bfa69f864d)

[CountDownLatch 使用和源码分析](https://liuzhengyang.github.io/2017/05/22/countdownlatch/)