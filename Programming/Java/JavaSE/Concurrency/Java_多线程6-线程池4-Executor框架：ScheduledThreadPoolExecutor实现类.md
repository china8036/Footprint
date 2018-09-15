- [Java 多线程 - 线程池 - Executor 框架：线程实现类 ScheduledThreadPoolExecutor](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E6%B1%A0---executor-%E6%A1%86%E6%9E%B6%EF%BC%9A%E7%BA%BF%E7%A8%8B%E5%AE%9E%E7%8E%B0%E7%B1%BB-scheduledthreadpoolexecutor)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 基本 API](#2-%E5%9F%BA%E6%9C%AC-api)
	- [3. 源码分析](#3-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
		- [3.1. 类定义](#31-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.2. 内部属性](#32-%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)
		- [3.3. 内部类](#33-%E5%86%85%E9%83%A8%E7%B1%BB)
			- [3.3.1. ScheduledFutureTask](#331-scheduledfuturetask)
			- [3.3.2. DelayedWorkQueue](#332-delayedworkqueue)
		- [3.4. 构造方法](#34-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
		- [3.5. 任务调度实现](#35-%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E5%AE%9E%E7%8E%B0)
			- [3.5.1. schedule()](#351-schedule)
			- [3.5.2. scheduleAtFixedRate()](#352-scheduleatfixedrate)
			- [3.5.3. scheduleWithFixedDelay()](#353-schedulewithfixeddelay)
		- [3.6. 其它方法](#36-%E5%85%B6%E5%AE%83%E6%96%B9%E6%B3%95)
	- [4. Refer Links](#4-refer-links)

# Java 多线程 - 线程池 - Executor 框架：线程实现类 ScheduledThreadPoolExecutor

## 1. 基本概念

JDK1.5 之前，实现周期性的任务需要依赖于 Timer 和 TimerTask 或者其它第三方工具来完成，但 Timer 有不少的缺陷：
- Timer 是单线程模式，因此如果在执行任务期间某个 TimerTask 耗时较久，那么就会影响其它任务的调度。
- Timer 的任务调度是基于绝对时间的，对系统时间敏感。
- Timer 不会捕获执行 TimerTask 时所抛出的异常，且由于 Timer 是单线程，所以一旦出现异常，则线程就会终止，其他任务也得不到执行。

JDK1.5 开始，JDK 提供了 [ScheduledThreadPoolExecutor](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ScheduledThreadPoolExecutor.html) 类来支持周期性任务的调度，**ScheduledThreadPoolExecutor 是多线程模式、基于相对时间，可以完全替代 Timer 进行周期任务调度**。

## 2. 基本 API

- `ScheduledFuture<?> schedule(Runnable command, long delay, TimeUnit unit)`: Creates and executes a one-shot action that becomes enabled after the given delay.
- `<V> ScheduledFuture<V> schedule(Callable<V> callable, long delay, TimeUnit unit)`: Creates and executes a ScheduledFuture that becomes enabled after the given delay.
-  `ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)`: Creates and executes a periodic action that becomes enabled first after the given initial delay, and subsequently with the given period; that is, executions will commence after initialDelay, then initialDelay + period, then initialDelay + 2 * period, and so on.
-  `ScheduledFuture<?> scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)`: Creates and executes a periodic action that becomes enabled first after the given initial delay, and subsequently with the given delay between the termination of one execution and the commencement of the next.
-  `void shutdown()`: Initiates an orderly shutdown in which previously submitted tasks are executed, but no new tasks will be accepted.
-  `List<Runnable> shutdownNow()`: Attempts to stop all actively executing tasks, halts the processing of waiting tasks, and returns a list of the tasks that were awaiting execution.

e.g.
```java
public class ScheduledThreadPoolTest {
    public static void main(String[] args) throws InterruptedException {
        // 创建大小为 5 的线程池
        ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
        for (int i = 0; i < 3; i++) {
            Task worker = new Task("task-" + i);
            // 只执行一次
          	// scheduledThreadPool.schedule(worker, 5, TimeUnit.SECONDS);
            // 周期性执行，每 5 秒执行一次
            scheduledThreadPool.scheduleAtFixedRate(worker, 0, 5, TimeUnit.SECONDS);
        }
        Thread.sleep(10000);
        System.out.println("Shutting down executor...");
        // 关闭线程池
        scheduledThreadPool.shutdown();
        boolean isDone;
        // 等待线程池终止
        do {
            isDone = scheduledThreadPool.awaitTermination(1, TimeUnit.DAYS);
            System.out.println("awaitTermination...");
        } while(!isDone);
        System.out.println("Finished all threads");
    }
}
class Task implements Runnable {
    private String name;
    public Task(String name) {
        this.name = name;
    }
    @Override
    public void run() {
        System.out.println("name = " + name + ", startTime = " + new Date());
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("name = " + name + ", endTime = " + new Date());
    }
}
```

## 3. 源码分析

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/11/af8c0bc7704b0495bd0cd4ead26a73dc.jpg)

### 3.1. 类定义

**ScheduledThreadPoolExecutor 继承自 ThreadPoolExecutor，实现了 ScheduledExecutorService 接口，该接口定义了 schedule 等任务调度的方法**。
```java
public class ScheduledThreadPoolExecutor
        extends ThreadPoolExecutor
        implements ScheduledExecutorService {
}
```

### 3.2. 内部属性

```java
/**
	* False if should cancel/suppress periodic tasks on shutdown.
	*/
private volatile boolean continueExistingPeriodicTasksAfterShutdown;

/**
	* False if should cancel non-periodic tasks on shutdown.
	*/
private volatile boolean executeExistingDelayedTasksAfterShutdown = true;

/**
	* True if ScheduledFutureTask.cancel should remove from queue
	*/
private volatile boolean removeOnCancel = false;

/**
	* Sequence number to break scheduling ties, and in turn to
	* guarantee FIFO order among tied entries.
	*/
private static final AtomicLong sequencer = new AtomicLong();
```

### 3.3. 内部类

ScheduledThreadPoolExecutor 有两个重要的内部类：DelayedWorkQueue 和 ScheduledFutureTask。

#### 3.3.1. ScheduledFutureTask

ScheduledThreadPoolExecutor 将任务封装成 ScheduledFutureTask 对象，ScheduledFutureTask 基于相对时间，不受系统时间的改变所影响：
```java
private class ScheduledFutureTask<V>
				extends FutureTask<V> implements RunnableScheduledFuture<V> {

		/** Sequence number to break ties FIFO */
		private final long sequenceNumber;

		/** The time the task is enabled to execute in nanoTime units */
		private long time;

		private final long period;

		/** The actual task to be re-enqueued by reExecutePeriodic */
		RunnableScheduledFuture<V> outerTask = this;

		/**
			* Index into delay queue, to support faster cancellation.
			*/
		int heapIndex;

		ScheduledFutureTask(Runnable r, V result, long ns) {
				super(r, result);
				this.time = ns;
				this.period = 0;
				this.sequenceNumber = sequencer.getAndIncrement();
		}
		ScheduledFutureTask(Runnable r, V result, long ns, long period) {
				super(r, result);
				this.time = ns;
				this.period = period;
				this.sequenceNumber = sequencer.getAndIncrement();
		}
		ScheduledFutureTask(Callable<V> callable, long ns) {
				super(callable);
				this.time = ns;
				this.period = 0;
				this.sequenceNumber = sequencer.getAndIncrement();
		}
		// getDelay 方法用于返回距离下次任务执行时间的时间间隔
		public long getDelay(TimeUnit unit) {
				return unit.convert(time - now(), NANOSECONDS);
		}
		// compareTo 方法用于比较任务之间的优先级关系，如果距离下次执行的时间间隔较短，则优先级高
		public int compareTo(Delayed other) {
				if (other == this) // compare zero if same object
						return 0;
				if (other instanceof ScheduledFutureTask) {
						ScheduledFutureTask<?> x = (ScheduledFutureTask<?>)other;
						long diff = time - x.time;
						if (diff < 0)
								return -1;
						else if (diff > 0)
								return 1;
						else if (sequenceNumber < x.sequenceNumber)
								return -1;
						else
								return 1;
				}
				long diff = getDelay(NANOSECONDS) - other.getDelay(NANOSECONDS);
				return (diff < 0) ? -1 : (diff > 0) ? 1 : 0;
		}
		public boolean isPeriodic() {
				return period != 0;
		}

		// setNextRunTime 方法会在 run 方法中执行完任务后调用
		private void setNextRunTime() {
				long p = period;
				// 固定频率，上次执行时间加上周期时间
				if (p > 0)
						time += p;
				// 相对固定延迟执行，使用当前系统时间加上周期时间
				else
						time = triggerTime(-p);
		}

		public boolean cancel(boolean mayInterruptIfRunning) {
				boolean cancelled = super.cancel(mayInterruptIfRunning);
				if (cancelled && removeOnCancel && heapIndex >= 0)
						remove(this);
				return cancelled;
		}

		// 重载 FutureTask 的 run() 方法，以实现延迟执行
		public void run() {
				// 是否是周期性任务
				boolean periodic = isPeriodic();
				// 当前线程池运行状态下如果不可以执行任务，取消该任务
				if (!canRunInCurrentRunState(periodic))
						cancel(false);
				// 如果不是周期性任务，调用 FutureTask 中的 run 方法执行
				else if (!periodic)
						ScheduledFutureTask.super.run();
				// 如果是周期性任务，调用 FutureTask 中的 runAndReset 方法执行，runAndReset 方法不会设置执行结果，所以可以重复执行任务
				else if (ScheduledFutureTask.super.runAndReset()) {
						// 计算下次执行该任务的时间
						setNextRunTime();
						// 重复执行任务
						reExecutePeriodic(outerTask);
				}
		}

}
```

```java
// 该方法和 delayedExecute 方法类似，不同的是：
// 1. 由于调用 reExecutePeriodic 方法时已经执行过一次周期性任务了，所以不会 reject 当前任务
// 2. 传入的任务一定是周期性任务
void reExecutePeriodic(RunnableScheduledFuture<?> task) {
		if (canRunInCurrentRunState(true)) {
				super.getQueue().add(task);
				if (!canRunInCurrentRunState(true) && remove(task))
						task.cancel(false);
				else
						ensurePrestart();
		}
}
```

#### 3.3.2. DelayedWorkQueue

由于定时任务执行时需要取出最近要执行的任务，即每次出队时一定要是当前队列中执行时间最靠前的任务，因此需要使用优先级队列。

ScheduledThreadPoolExecutor 定义了一个 DelayedWorkQueue，它是一个**基于最小堆的优先级队列**，会通过每个任务按照距离下次执行时间间隔的大小来排序，可以保证每次出队的任务都是当前队列中执行时间最靠前的。

- 队列执行插入和删除操作时的最坏时间复杂度是 O(logN)。

- Leader-Follower Pattern

	比如说现在队列中的第一个任务 1 分钟后执行，那么用户提交新的任务时会不断的加入 woker 线程，如果新提交的任务都排在队列后面，也就是说新的 woker 现在都会取出这第一个任务进行执行延迟时间的等待，当该任务到触发时间时，会唤醒很多 woker 线程，这显然是没有必要的。

	所以，为了不让多个线程频繁的做无用的定时等待，队列使用了 [Leader-Follower 模式](http://blog.csdn.net/goldlevi/article/details/7705180)，用于**减少不必要的定时等待**。
	
	- 在该模型中，所有线程都处于以下 3 种状态之一：leader、follower 和 proccesser。其中，leader 线程在任何时刻永远只有一个，而所有 follower 都在等待成为 leader。
	- 线程池启动时，会自动产生一个 Leader 线程负责等待网络 IO 事件，当有一个事件产生时，Leader 线程首先通知一个 Follower 线程将其提拔为新的 Leader，然后旧 Leader 便成为 Follower 并对该 IO 事件进行处理。处理完毕后加入 Follower 线程等待队列，等待下次被选中成为 Leader。
	- 该模型可以增强 CPU 高速缓存的相似性，消除动态内存分配和线程间的数据交换。

	如果 leader 不为空，则说明队列中第一个节点已经在等待出队，这时其它的线程会一直阻塞，减少了无用的阻塞（注意，在 finally 中调用了 signal() 来唤醒一个线程，而不是 signalAll()）。

- 执行流程
	
	1. 向 ScheduledThreadPoolExecutor 中提交任务的时候，任务被包装成 ScheduledFutureTask 对象加入延迟队列并启动一个 woker 线程。
	1. 用户提交的任务加入延迟队列时，会按照执行时间进行排列，也就是说队列头的任务是需要最早执行的。而 woker 线程会从延迟队列中获取任务，如果已经到了任务的执行时间，则开始执行，否则阻塞等待剩余延迟时间后再尝试获取任务。
	1. 任务执行完成以后，如果该任务是一个需要周期性反复执行的任务，则计算好下次执行的时间后会重新加入到延迟队列中。

```java
static class DelayedWorkQueue extends AbstractQueue<Runnable>
				implements BlockingQueue<Runnable> {
		// 队列初始容量
		private static final int INITIAL_CAPACITY = 16;
		// 根据初始容量创建 RunnableScheduledFuture 类型的数组
		private RunnableScheduledFuture<?>[] queue =
				new RunnableScheduledFuture<?>[INITIAL_CAPACITY];
		private final ReentrantLock lock = new ReentrantLock();
		private int size = 0;

		// leader 线程
		private Thread leader = null;
		// 当较新的任务在队列的头部可用时，或者新线程可能需要成为 leader，则通过该条件发出信号
		private final Condition available = lock.newCondition();
		
		// 入队的操作如 add 和 put 方法都调用了 offer 方法
		public boolean offer(Runnable x) {
				if (x == null)
						throw new NullPointerException();
				RunnableScheduledFuture<?> e = (RunnableScheduledFuture<?>)x;
				final ReentrantLock lock = this.lock;
				lock.lock();
				try {
						int i = size;
						// queue 是一个 RunnableScheduledFuture 类型的数组，如果容量不够需要扩容
						if (i >= queue.length)
								grow();
						size = i + 1;
						// i == 0 说明堆中还没有数据
						if (i == 0) {
								queue[0] = e;
								setIndex(e, 0);
						} else {
						// i != 0 时，需要对堆进行重新排序
								siftUp(i, e);
						}
						// 如果传入的任务已经是队列的第一个节点了，这时 available 需要发出信号
						if (queue[0] == e) {
								// leader 设置为 null 为了使在 take 方法中的线程在通过 available.signal(); 后会执行 available.awaitNanos(delay);
								leader = null;
								available.signal();
						}
				} finally {
						lock.unlock();
				}
				return true;
		}

		// 循环根据 key 节点与它的父节点来判断，如果 key 节点的执行时间小于父节点，则将两个节点交换，使执行时间靠前的节点排列在队列的前面
		private void siftUp(int k, RunnableScheduledFuture<?> key) {
				while (k > 0) {
						// 找到父节点的索引
						int parent = (k - 1) >>> 1;
						// 获取父节点
						RunnableScheduledFuture<?> e = queue[parent];
						// 如果 key 节点的执行时间大于父节点的执行时间，不需要再排序了
						if (key.compareTo(e) >= 0)
								break;
						// 如果 key.compareTo(e) < 0，说明 key 节点的执行时间小于父节点的执行时间，需要把父节点移到后面
						queue[k] = e;
						// 设置索引为 k
						setIndex(e, k);
						k = parent;
				}
				// key 设置为排序后的位置中
				queue[k] = key;
				setIndex(key, k);
		}
		// 使堆从 k 开始向下调整
		private void siftDown(int k, RunnableScheduledFuture<?> key) {
				// 根据二叉树的特性，数组长度除以 2，表示取有子节点的索引
				int half = size >>> 1;
				// 判断索引为 k 的节点是否有子节点
				while (k < half) {
						// 左子节点的索引
						int child = (k << 1) + 1;
						RunnableScheduledFuture<?> c = queue[child];
						// 右子节点的索引
						int right = child + 1;
						// 如果有右子节点并且左子节点的时间间隔大于右子节点，取时间间隔最小的节点
						if (right < size && c.compareTo(queue[right]) > 0)
								c = queue[child = right];
						// 如果 key 的时间间隔小于等于 c 的时间间隔，跳出循环
						if (key.compareTo(c) <= 0)
								break;
						// 设置要移除索引的节点为其子节点
						queue[k] = c;
						setIndex(c, k);
						k = child;
				}
				// 将 key 放入索引为 k 的位置
				queue[k] = key;
				setIndex(key, k);
		}

		public RunnableScheduledFuture<?> take() throws InterruptedException {
				final ReentrantLock lock = this.lock;
				lock.lockInterruptibly();
				try {
						for (;;) {
								RunnableScheduledFuture<?> first = queue[0];
								if (first == null)
										available.await();
								else {
										// 计算当前时间到执行时间的时间间隔
										long delay = first.getDelay(NANOSECONDS);
										if (delay <= 0)
												return finishPoll(first);
										first = null; // don't retain ref while waiting
										// leader 不为空，阻塞线程
										if (leader != null)
												available.await();
										else {
												// leader 为空，则把 leader 设置为当前线程，
												Thread thisThread = Thread.currentThread();
												leader = thisThread;
												try {
														// 阻塞到执行时间
														available.awaitNanos(delay);
												} finally {
														// 设置 leader = null，让其他线程执行 available.awaitNanos(delay);
														if (leader == thisThread)
																leader = null;
												}
										}
								}
						}
				} finally {
						// 如果 leader 不为空，则说明 leader 的线程正在执行 available.awaitNanos(delay);
						// 如果 queue[0] == null，说明队列为空
						if (leader == null && queue[0] != null)
								available.signal();
						lock.unlock();
				}
		}

		public RunnableScheduledFuture<?> poll(long timeout, TimeUnit unit)
				throws InterruptedException {
				long nanos = unit.toNanos(timeout);
				final ReentrantLock lock = this.lock;
				lock.lockInterruptibly();
				try {
						for (;;) {
								RunnableScheduledFuture<?> first = queue[0];
								if (first == null) {
										if (nanos <= 0)
												return null;
										else
												nanos = available.awaitNanos(nanos);
								} else {
										long delay = first.getDelay(NANOSECONDS);
										// 如果 delay <= 0，说明已经到了任务执行的时间，返回。
										if (delay <= 0)
												return finishPoll(first);
										// 如果 nanos <= 0，说明已经超时，返回 null
										if (nanos <= 0)
												return null;
										first = null; // don't retain ref while waiting
										// nanos < delay 说明需要等待的时间小于任务要执行的延迟时间
										// leader != null 说明有其它线程正在对任务进行阻塞
										// 这时阻塞当前线程 nanos 纳秒
										if (nanos < delay || leader != null)
												nanos = available.awaitNanos(nanos);
										else {
												Thread thisThread = Thread.currentThread();
												leader = thisThread;
												try {
														// 这里的 timeLeft 表示 delay 减去实际的等待时间
														long timeLeft = available.awaitNanos(delay);
														// 计算剩余的等待时间
														nanos -= delay - timeLeft;
												} finally {
														if (leader == thisThread)
																leader = null;
												}
										}
								}
						}
				} finally {
						if (leader == null && queue[0] != null)
								available.signal();
						lock.unlock();
				}
		}

		
		// ...

}
```

### 3.4. 构造方法

ScheduledThreadPoolExecutor 继承自 ThreadPoolExecutor，所以 ScheduledThreadPoolExecutor 的构造方法都是直接调用其父类 ThreadPoolExecutor 的构造方法：
```java
public ScheduledThreadPoolExecutor(int corePoolSize) {
		super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
					new DelayedWorkQueue());
}

public ScheduledThreadPoolExecutor(int corePoolSize,
																		ThreadFactory threadFactory) {
		super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
					new DelayedWorkQueue(), threadFactory);
}

public ScheduledThreadPoolExecutor(int corePoolSize,
																		RejectedExecutionHandler handler) {
		super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
					new DelayedWorkQueue(), handler);
}

public ScheduledThreadPoolExecutor(int corePoolSize,
																		ThreadFactory threadFactory,
																		RejectedExecutionHandler handler) {
		super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
					new DelayedWorkQueue(), threadFactory, handler);
}
```

### 3.5. 任务调度实现

#### 3.5.1. schedule()

schedule() 方法用来进行延时任务调度，该方法有两个重载版本，但区别仅在于传入的第一个参数类型不同，方法体的执行逻辑完全相同：
```java
public ScheduledFuture<?> schedule(Runnable command,
																		long delay,
																		TimeUnit unit) {
		if (command == null || unit == null)
				throw new NullPointerException();
		RunnableScheduledFuture<?> t = decorateTask(command,
				new ScheduledFutureTask<Void>(command, null,
																			triggerTime(delay, unit)));
		delayedExecute(t);
		// 返回一个 ScheduledFuture 对象
		return t;
}

public <V> ScheduledFuture<V> schedule(Callable<V> callable,
																				long delay,
																				TimeUnit unit) {
		if (callable == null || unit == null)
				throw new NullPointerException();
		RunnableScheduledFuture<V> t = decorateTask(callable,
				new ScheduledFutureTask<V>(callable,
																		triggerTime(delay, unit)));
		delayedExecute(t);
		// 返回一个 ScheduledFuture 对象
		return t;
}

// 用于获取下一次执行的具体时间
private long triggerTime(long delay, TimeUnit unit) {
		return triggerTime(unit.toNanos((delay < 0) ? 0 : delay));
}
long triggerTime(long delay) {
		// 判断是否要防止 Long 类型溢出
		// 如果 delay 的值小于 Long 类型最大值的一半，则直接返回 delay，否则需要进行防止溢出处理
    return now() +
        ((delay < (Long.MAX_VALUE >> 1)) ? delay : overflowFree(delay));
}

// decorateTask 默认什么功能都没有做，只直接返回给定任务，子类可以重写该方法，用于管理内部任务
protected <V> RunnableScheduledFuture<V> decorateTask(
		Runnable runnable, RunnableScheduledFuture<V> task) {
		return task;
}

protected <V> RunnableScheduledFuture<V> decorateTask(
		Callable<V> callable, RunnableScheduledFuture<V> task) {
		return task;
}

// 延时执行任务
private void delayedExecute(RunnableScheduledFuture<?> task) {
		// 如果线程池已经关闭，使用拒绝策略拒绝任务
		if (isShutdown())
				reject(task);
		else {
				// 添加到阻塞队列中
				super.getQueue().add(task);
				if (isShutdown() &&
						!canRunInCurrentRunState(task.isPeriodic()) &&
						remove(task))
						task.cancel(false);
				else
						// 确保线程池中至少有一个线程启动，即使 corePoolSize 为 0，该方法在 ThreadPoolExecutor 中实现
            // ensurePrestart 方法在 ThreadPoolExecutor 中定义
						ensurePrestart();
    }
}
```

#### 3.5.2. scheduleAtFixedRate()

scheduleAtFixedRate() 方法设置了执行周期，下一次执行时间是**上一次任务的开始执行时间加上 period**，它是**采用已固定的频率来执行任务**：
```java
public ScheduledFuture<?> scheduleAtFixedRate(Runnable command,
																							long initialDelay,
																							long period,
																							TimeUnit unit) {
		if (command == null || unit == null)
				throw new NullPointerException();
		if (period <= 0)
				throw new IllegalArgumentException();
		ScheduledFutureTask<Void> sft =
				new ScheduledFutureTask<Void>(command,
																			null,
																			triggerTime(initialDelay, unit),
																			unit.toNanos(period));
		RunnableScheduledFuture<Void> t = decorateTask(command, sft);
		sft.outerTask = t;
		delayedExecute(t);
		return t;
}
```

#### 3.5.3. scheduleWithFixedDelay()

scheduleWithFixedDelay() 方法也设置了执行周期，但与 scheduleAtFixedRate 方法不同的是，下一次执行时间是**上一次任务执行完毕后的系统时间加上 delay**，因而具体执行时间不是固定的，但周期是固定的，是**采用相对固定的延迟来执行任务**：
```java
public ScheduledFuture<?> scheduleWithFixedDelay(Runnable command,
																									long initialDelay,
																									long delay,
																									TimeUnit unit) {
		if (command == null || unit == null)
				throw new NullPointerException();
		if (delay <= 0)
				throw new IllegalArgumentException();
		// 把周期设置为负数来表示以相对固定的延迟执行
		ScheduledFutureTask<Void> sft =
				new ScheduledFutureTask<Void>(command,
																			null,
																			triggerTime(initialDelay, unit),
																			unit.toNanos(-delay));
		RunnableScheduledFuture<Void> t = decorateTask(command, sft);
		sft.outerTask = t;
		delayedExecute(t);
		return t;
}
```

### 3.6. 其它方法

```java
// 限制队列中所有节点的延迟时间在 Long.MAX_VALUE 之内，防止在 compareTo 方法中溢出
private long overflowFree(long delay) {
		Delayed head = (Delayed) super.getQueue().peek();
		if (head != null) {
				long headDelay = head.getDelay(NANOSECONDS);
				if (headDelay < 0 && (delay - headDelay < 0))
						delay = Long.MAX_VALUE + headDelay;
		}
		return delay;
}
```
```java
// onShutdown 方法是 ThreadPoolExecutor 中的钩子方法，但在 ThreadPoolExecutor 中什么都没有做，该方法在执行 shutdown 方法时被调用
@Override void onShutdown() {
    BlockingQueue<Runnable> q = super.getQueue();
    // 获取在线程池已 shutdown 的情况下是否继续执行现有延迟任务
    boolean keepDelayed =
        getExecuteExistingDelayedTasksAfterShutdownPolicy();
    // 获取在线程池已 shutdown 的情况下是否继续执行现有定期任务
    boolean keepPeriodic =
        getContinueExistingPeriodicTasksAfterShutdownPolicy();
    // 如果在线程池已 shutdown 的情况下不继续执行延迟任务和定期任务，则依次取消任务，否则则根据取消状态来判断
    if (!keepDelayed && !keepPeriodic) {
        for (Object e : q.toArray())
            if (e instanceof RunnableScheduledFuture<?>)
                ((RunnableScheduledFuture<?>) e).cancel(false);
        q.clear();
    }
    else {
        // Traverse snapshot to avoid iterator exceptions
        for (Object e : q.toArray()) {
            if (e instanceof RunnableScheduledFuture) {
                RunnableScheduledFuture<?> t =
                    (RunnableScheduledFuture<?>)e;
                // 如果有在 shutdown 后不继续的延迟任务或周期任务，则从队列中删除并取消任务
                if ((t.isPeriodic() ? !keepPeriodic : !keepDelayed) ||
                    t.isCancelled()) { // also remove if already cancelled
                    if (q.remove(t))
                        t.cancel(false);
                }
            }
        }
    }
    tryTerminate();
}
```

## 4. Refer Links

[深入理解 Java 线程池：ScheduledThreadPoolExecutor](http://www.ideabuffer.cn/2017/04/14/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Java%E7%BA%BF%E7%A8%8B%E6%B1%A0%EF%BC%9AScheduledThreadPoolExecutor/)

[Java 调度线程池 ScheduledThreadPoolExecutor 源码分析](https://www.jianshu.com/p/18f4c95aca24)