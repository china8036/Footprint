- [Java 多线程 - 线程池 - Executor 框架：线程实现类 ThreadPoolExecutor](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E6%B1%A0---executor-%E6%A1%86%E6%9E%B6%EF%BC%9A%E7%BA%BF%E7%A8%8B%E5%AE%9E%E7%8E%B0%E7%B1%BB-threadpoolexecutor)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 基本 API](#2-%E5%9F%BA%E6%9C%AC-api)
		- [2.1. 构造方法](#21-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
		- [2.2. 任务提交](#22-%E4%BB%BB%E5%8A%A1%E6%8F%90%E4%BA%A4)
		- [2.3. 线程池监控](#23-%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9B%91%E6%8E%A7)
		- [2.4. 其它方法](#24-%E5%85%B6%E5%AE%83%E6%96%B9%E6%B3%95)
	- [3. 源码分析](#3-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
		- [3.1. 类定义](#31-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.2. 内部属性](#32-%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)
		- [3.3. 构造方法](#33-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
		- [3.4. 内部类](#34-%E5%86%85%E9%83%A8%E7%B1%BB)
		- [3.5. 任务提交实现](#35-%E4%BB%BB%E5%8A%A1%E6%8F%90%E4%BA%A4%E5%AE%9E%E7%8E%B0)
		- [3.6. 其他方法实现](#36-%E5%85%B6%E4%BB%96%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0)
	- [4. 自定义线程池](#4-%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BA%BF%E7%A8%8B%E6%B1%A0)
	- [5. Refer Links](#5-refer-links)

# Java 多线程 - 线程池 - Executor 框架：线程实现类 ThreadPoolExecutor

## 1. 基本概念

java.uitl.concurrent.[ThreadPoolExecutor](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ThreadPoolExecutor.html) 类是线程池中最核心的一个类。

ThreadPoolExecutor 类是 Executor 线程池的一个具体实现类，一般所有的线程池都是基于这个类实现的。

## 2. 基本 API

### 2.1. 构造方法

- `ThreadPoolExecutor​(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue)`: Creates a new ThreadPoolExecutor with the given initial parameters and default thread factory and rejected execution handler.
- `ThreadPoolExecutor​(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler)`: Creates a new ThreadPoolExecutor with the given initial parameters and default thread factory.
- `ThreadPoolExecutor​(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory)`: Creates a new ThreadPoolExecutor with the given initial parameters and default rejected execution handler.
- `ThreadPoolExecutor​(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)`: Creates a new ThreadPoolExecutor with the given initial parameters.

参数说明：
- corePoolSize
	
	核心线程的数量，核心线程指的是即使空闲也不会马上被销毁的线程。
	
	在创建了线程池后，默认情况下，线程池中并没有任何线程，而是等待有任务到来才创建线程去执行任务，除非调用了 prestartAllCoreThreads() 或者 prestartCoreThread() 方法，这 2 个方法在没有任务到来之前就创建 corePoolSize 个线程或者一个线程。

	当提交一个任务时，线程池就会创建一个新线程去执行任务，直到线程池中的线程数目达到 corePoolSize 后，就会把继续提交的任务放到缓存队列当中。

- maximumPoolSize
	
	线程池中允许的最大线程数，表示在线程池中最多能创建多少个线程。如果当前阻塞队列已满，且还有新任务提交，则会创建新的线程执行任务，前提是当前线程池的线程数必须小于 maximumPoolSize。

- keepAliveTime
	
	表示线程没有任务执行时最多保持多久时间会终止。
	
	默认情况下，只有当线程池中的线程数大于 corePoolSize 时，keepAliveTime 才会起作用，即当线程池中的线程数大于 corePoolSize 时，如果一个线程空闲的时间达到 keepAliveTime 就会被终止，直到线程池中的线程数不超过 corePoolSize。
	
	但是如果调用了 allowCoreThreadTimeOut(boolean) 方法，在线程池中的线程数不大于 corePoolSize 时，keepAliveTime 参数也会起作用，直到线程池中的线程数为 0。

- unit
	
	参数 keepAliveTime 的时间单位，值枚举类型 TimeUnit。

- workQueue
	
	用来保存等待被执行的任务的阻塞队列，当任务提交时，若线程池中的线程数量大于等于 corePoolSize，会把该任务封装成一个 Worker 对象放入等待队列。
	
	这个参数的选择会对线程池的运行过程产生重大影响，在 JDK 中提供了如下阻塞队列：
	- `ArrayBlockingQueue`: （有界队列）基于数组结构的有界阻塞队列，按 FIFO 排序任务。
	- `LinkedBlockingQuene`: （无界队列）基于链表结构的阻塞队列，按 FIFO 排序任务，吞吐量通常要高于 ArrayBlockingQuene。
	- `SynchronousQuene`: （直接切换）一个不存储元素的阻塞队列，每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于 LinkedBlockingQuene。
	- `priorityBlockingQuene`: （优先级队列）具有优先级的无界阻塞队列。

- threadFactory
	
	创建线程的工厂，通过自定义的线程工厂可以给每个新建的线程设置一个具有识别度的线程名。

	默认使用 Executors.defaultThreadFactory() 来创建线程。使用默认的 ThreadFactory 来创建线程时，会使新创建的线程具有相同的 NORM_PRIORITY 优先级并且是非守护线程，同时也设置了线程的名称。

- handler
	
	线程池的饱和策略，当阻塞队列已满且没有空闲的工作线程时，如果继续提交任务，必须采取一种策略处理该任务，有以下四种取值：
	- `ThreadPoolExecutor.CallerRunsPolicy`：由调用线程处理该任务。 	
	- `ThreadPoolExecutor.AbortPolicy`: 丢弃任务并抛出 RejectedExecutionException 异常，默认策略。
	- `ThreadPoolExecutor.DiscardPolicy`：也是丢弃任务，但是不抛出异常。
	- `ThreadPoolExecutor.DiscardOldestPolicy`：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）。
	除此之外，还可以根据应用场景实现 RejectedExecutionHandler 接口，自定义饱和策略（如记录日志或持久化存储不能处理的任务）。

### 2.2. 任务提交

线程池框架提供了两种方式提交任务，根据不同的业务需求选择不同的方式。
- `Executor.execute()`: 通过 Executor.execute() 方法提交的任务，必须实现 Runnable 接口，该方式提交的任务不能获取返回值，因此无法判断任务是否执行成功。
- `ExecutorService.submit()`: 通过 ExecutorService.submit() 方法提交的任务，可以获取任务执行完的返回值。

### 2.3. 线程池监控

通过以下方法，可以对线程池进行监控：
- `long getTaskCount()`: 线程池已经执行的和未执行的任务总数；
- `long getCompletedTaskCount()`: 线程池已完成的任务数量，该值小于等于 taskCount；
- `int getLargestPoolSize()`: 线程池曾经创建过的最大线程数量。通过这个数据可以知道线程池是否满过，也就是达到了 maximumPoolSize；
- `int getPoolSize()`: 线程池当前的线程数量；
- `int getActiveCount()`: 当前线程池中正在执行任务的线程数量。

### 2.4. 其它方法

在 ThreadPoolExecutor 类中提供了几个空方法：
- `protected void beforeExecute(Thread t, Runnable r) { }`
- `protected void afterExecute(Runnable r, Throwable t) { }`
- `protected void terminated() { }`

可以扩展这些方法在执行前或执行后增加一些新的操作，例如统计线程池的执行任务的时间等，可以继承自 ThreadPoolExecutor 来进行扩展。

## 3. 源码分析

### 3.1. 类定义

ThreadPoolExecutor 继承了 AbstractExecutorService 类，间接实现了 Executor 接口：
```java
public class ThreadPoolExecutor extends AbstractExecutorService {
	// ...
}
```

### 3.2. 内部属性

```java
// AtomicInteger 变量 ctl 的低 29 位表示线程池中线程数，高 3 位表示线程池的运行状态
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
private static final int COUNT_BITS = Integer.SIZE - 3;
private static final int CAPACITY   = (1 << COUNT_BITS) - 1;

// runState is stored in the high-order bits
private static final int RUNNING    = -1 << COUNT_BITS; // 高 3 位为 111，该状态的线程池会接收新任务，并处理阻塞队列中的任务
private static final int SHUTDOWN   =  0 << COUNT_BITS; // 高 3 位为 000，该状态的线程池不会接收新任务，但会处理阻塞队列中的任务；在线程池处于 RUNNING 状态时，调用 shutdown() 方法会使线程池进入到该状态（finalize() 方法在执行过程中也会调用 shutdown() 方法进入该状态）
private static final int STOP       =  1 << COUNT_BITS; // 高 3 位为 001，该状态的线程池不会接收新任务，也不会处理阻塞队列中的任务，而且会中断正在运行的任务；在线程池处于 RUNNING 或 SHUTDOWN 状态时，调用 shutdownNow() 方法会使线程池进入到该状态
private static final int TIDYING    =  2 << COUNT_BITS; // 高 3 位为 010，如果所有的任务都已终止了，workerCount （有效线程数) 为 0，线程池进入该状态后会调用 terminated() 方法进入 TERMINATED 状态
private static final int TERMINATED =  3 << COUNT_BITS; // 高 3 位为 011，在 terminated() 方法执行完后进入该状态，默认 terminated() 方法中什么也没有做

// 对 ctl 进行计算的方法
private static int runStateOf(int c)     { return c & ~CAPACITY; } // 获取运行状态
private static int workerCountOf(int c)  { return c & CAPACITY; } // 根据 ctl 的低 29 位，得到线程池的当前线程数
private static int ctlOf(int rs, int wc) { return rs | wc; } // 获取运行状态和活动线程数的值
private static boolean runStateLessThan(int c, int s) {
		return c < s;
}
private static boolean runStateAtLeast(int c, int s) {
		return c >= s;
}
private static boolean isRunning(int c) { // 判断线程池是否处于 RUNNING 状态
		return c < SHUTDOWN;
}

private final BlockingQueue<Runnable> workQueue; // 任务缓存队列，用来保存等待中的任务，等待 worker 线程空闲时执行任务
private final ReentrantLock mainLock = new ReentrantLock(); // 更新 poolSize, corePoolSize,maximumPoolSize, runState, and workers set 时需要持有这个锁
private final HashSet<Worker> workers = new HashSet<Worker>(); // 用来保存工作中的执行线程
private final Condition termination = mainLock.newCondition();

private int largestPoolSize; // 记录线程池中出现过的最大线程数大小
private long completedTaskCount; // 已经执行完的线程数
private volatile ThreadFactory threadFactory; // 线程工厂，用来新建线程
private volatile RejectedExecutionHandler handler; // 任务拒绝策略
private volatile long keepAliveTime; // 超过 corePoolSize 外的线程空闲存活之间
private volatile boolean allowCoreThreadTimeOut; // 是否对 corePoolSize 内的线程设置空闲存活时间
private volatile int corePoolSize; // 核心线程数
private volatile int maximumPoolSize; // 最大线程数（即线程池中的线程数目大于这个参数时，提交的任务会被放进任务缓存队列）

private static final RejectedExecutionHandler defaultHandler =
		new AbortPolicy();

private static final RuntimePermission shutdownPerm =
		new RuntimePermission("modifyThread");
```

### 3.3. 构造方法

在 ThreadPoolExecutor 类中提供了四个构造方法：
```java
public ThreadPoolExecutor(int corePoolSize,
													int maximumPoolSize,
													long keepAliveTime,
													TimeUnit unit,
													BlockingQueue<Runnable> workQueue) {
		this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
					Executors.defaultThreadFactory(), defaultHandler);
}

public ThreadPoolExecutor(int corePoolSize,
													int maximumPoolSize,
													long keepAliveTime,
													TimeUnit unit,
													BlockingQueue<Runnable> workQueue,
													ThreadFactory threadFactory) {
		this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
					threadFactory, defaultHandler);
}

public ThreadPoolExecutor(int corePoolSize,
													int maximumPoolSize,
													long keepAliveTime,
													TimeUnit unit,
													BlockingQueue<Runnable> workQueue,
													RejectedExecutionHandler handler) {
		this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
					Executors.defaultThreadFactory(), handler);
}

// 前面三个构造器都是通过调用第四个构造器来进行初始化工作的
public ThreadPoolExecutor(int corePoolSize,
													int maximumPoolSize,
													long keepAliveTime,
													TimeUnit unit,
													BlockingQueue<Runnable> workQueue,
													ThreadFactory threadFactory,
													RejectedExecutionHandler handler) {
		if (corePoolSize < 0 ||
				maximumPoolSize <= 0 ||
				maximumPoolSize < corePoolSize ||
				keepAliveTime < 0)
				throw new IllegalArgumentException();
		if (workQueue == null || threadFactory == null || handler == null)
				throw new NullPointerException();
		this.corePoolSize = corePoolSize;
		this.maximumPoolSize = maximumPoolSize;
		this.workQueue = workQueue;
		this.keepAliveTime = unit.toNanos(keepAliveTime);
		this.threadFactory = threadFactory;
		this.handler = handler;
}
```

### 3.4. 内部类

```java
public static class CallerRunsPolicy implements RejectedExecutionHandler {
		public CallerRunsPolicy() { }
		public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
				if (!e.isShutdown()) {
						r.run();
				}
		}
}

public static class AbortPolicy implements RejectedExecutionHandler {
		public AbortPolicy() { }
		public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
				throw new RejectedExecutionException("Task " + r.toString() +
																							" rejected from " +
																							e.toString());
		}
}

public static class DiscardPolicy implements RejectedExecutionHandler {
		public DiscardPolicy() { }
		public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
		}
}

public static class DiscardOldestPolicy implements RejectedExecutionHandler {
		public DiscardOldestPolicy() { }
		public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
				if (!e.isShutdown()) {
						e.getQueue().poll();
						e.execute(r);
				}
		}
}

// 线程池的工作线程通过 Woker 类实现
// Worker 类继承了 AQS 类，可以方便的实现工作线程的中止操作；实现了 Runnable 接口，可以将自身作为一个任务在工作线程中执行
private final class Worker
		extends AbstractQueuedSynchronizer
		implements Runnable
{
		private static final long serialVersionUID = 6138294804551838833L;

		/** Thread this worker is running in.  Null if factory fails. */
		final Thread thread;
		/** Initial task to run.  Possibly null. */
		Runnable firstTask;
		/** Per-thread task counter */
		volatile long completedTasks;

		// 线程工厂在创建线程 thread 时，将 Woker 实例本身 this 作为参数传入，当执行 start 方法启动线程 thread 时，本质是执行了 ThreadPoolExecutor 的 runWorker 方法
		Worker(Runnable firstTask) {
				setState(-1); // inhibit interrupts until runWorker
				this.firstTask = firstTask;
				this.thread = getThreadFactory().newThread(this);
		}

		/** Delegates main run loop to outer runWorker  */
		public void run() {
				runWorker(this);
		}
		// ...
}
```

### 3.5. 任务提交实现

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/11/a7999d4504c80e5eb4331c37b23c1f61.jpg)

```java
public void execute(Runnable command) {
		if (command == null)
				throw new NullPointerException();
		/*
			* Proceed in 3 steps:
			*
			* 1. If fewer than corePoolSize threads are running, try to
			* start a new thread with the given command as its first
			* task.  The call to addWorker atomically checks runState and
			* workerCount, and so prevents false alarms that would add
			* threads when it shouldn't, by returning false.
			*
			* 2. If a task can be successfully queued, then we still need
			* to double-check whether we should have added a thread
			* (because existing ones died since last checking) or that
			* the pool shut down since entry into this method. So we
			* recheck state and if necessary roll back the enqueuing if
			* stopped, or start a new thread if there are none.
			*
			* 3. If we cannot queue task, then we try to add a new
			* thread.  If it fails, we know we are shut down or saturated
			* and so reject the task.
			*/
		int c = ctl.get();
		if (workerCountOf(c) < corePoolSize) {
				// 如果线程数小于 corePoolSize，则执行 addWorker 方法创建新的线程执行任务
				if (addWorker(command, true))
						return;
				c = ctl.get();
		}
		if (isRunning(c) && workQueue.offer(command)) {
				// 如果线程池处于 RUNNING 状态，且把提交的任务成功放入阻塞队列中
				// 再次检查线程池的状态
				int recheck = ctl.get();
				if (! isRunning(recheck) && remove(command))
						// 如果线程池没有 RUNNING，且成功从阻塞队列中删除任务，则执行 reject 方法处理任务
						reject(command);
				else if (workerCountOf(recheck) == 0)
						addWorker(null, false);
		}
		else if (!addWorker(command, false))
				// 执行 addWorker 方法创建新的线程执行任务，如果 addWoker 执行失败，则执行 reject 方法处理任务
				reject(command);
}

// 创建新的线程并执行任务
private boolean addWorker(Runnable firstTask, boolean core) {
		retry:
		for (;;) {
				int c = ctl.get();
				int rs = runStateOf(c);

				// 判断线程池的状态，如果线程池的状态值大于或等 SHUTDOWN，则不处理提交的任务，直接返回。
				if (rs >= SHUTDOWN &&
						! (rs == SHUTDOWN &&
								firstTask == null &&
								! workQueue.isEmpty()))
						return false;

				for (;;) {
						int wc = workerCountOf(c);
						// 通过参数 core 判断当前需要创建的线程是否为核心线程，如果 core 为 true，且当前线程数小于 corePoolSize，则跳出循环，开始创建新的线程
						if (wc >= CAPACITY ||
								wc >= (core ? corePoolSize : maximumPoolSize))
								return false;
						if (compareAndIncrementWorkerCount(c))
								break retry;
						c = ctl.get();  // Re-read ctl
						if (runStateOf(c) != rs)
								continue retry;
						// else CAS failed due to workerCount change; retry inner loop
				}
		}

		boolean workerStarted = false;
		boolean workerAdded = false;
		Worker w = null;
		try {
				// 线程池的工作线程通过 Woker 类实现，在 ReentrantLock 锁的保证下，把 Woker 实例插入到 HashSet 后，并启动 Woker 中的线程
				w = new Worker(firstTask);
				final Thread t = w.thread;
				if (t != null) {
						final ReentrantLock mainLock = this.mainLock;
						mainLock.lock();
						try {
								// Recheck while holding lock.
								// Back out on ThreadFactory failure or if
								// shut down before lock acquired.
								int rs = runStateOf(ctl.get());

								if (rs < SHUTDOWN ||
										(rs == SHUTDOWN && firstTask == null)) {
										if (t.isAlive()) // precheck that t is startable
												throw new IllegalThreadStateException();
										workers.add(w);
										int s = workers.size();
										if (s > largestPoolSize)
												largestPoolSize = s;
										workerAdded = true;
								}
						} finally {
								mainLock.unlock();
						}
						if (workerAdded) {
								t.start();
								workerStarted = true;
						}
				}
		} finally {
				if (! workerStarted)
						addWorkerFailed(w);
		}
		return workerStarted;
}

// runWorker 方法是线程池的核心
final void runWorker(Worker w) {
		// 线程启动之后，通过 unlock 方法释放锁，设置 AQS 的 state 为 0，表示运行中断
		Thread wt = Thread.currentThread();
		Runnable task = w.firstTask;
		w.firstTask = null;
		w.unlock(); // allow interrupts
		boolean completedAbruptly = true;
		try {
				// 获取第一个任务 firstTask，执行任务的 run 方法，不过在执行任务之前，会进行加锁操作，任务执行完会释放锁
				// 在执行任务的前后，可以根据业务场景自定义 beforeExecute 和 afterExecute 方法
				// firstTask 执行完成之后，通过 getTask 方法从阻塞队列中获取等待的任务，如果队列中没有任务，getTask 方法会被阻塞并挂起，不会占用 cpu 资源
				while (task != null || (task = getTask()) != null) {
						w.lock();
						if ((runStateAtLeast(ctl.get(), STOP) ||
									(Thread.interrupted() &&
									runStateAtLeast(ctl.get(), STOP))) &&
								!wt.isInterrupted())
								wt.interrupt();
						try {
								beforeExecute(wt, task);
								Throwable thrown = null;
								try {
										task.run();
								} catch (RuntimeException x) {
										thrown = x; throw x;
								} catch (Error x) {
										thrown = x; throw x;
								} catch (Throwable x) {
										thrown = x; throw new Error(x);
								} finally {
										afterExecute(task, thrown);
								}
						} finally {
								task = null;
								w.completedTasks++;
								w.unlock();
						}
				}
				completedAbruptly = false;
		} finally {
				processWorkerExit(w, completedAbruptly);
		}
}
```

当有新任务在 execute() 方法提交时，会执行以下判断：
- 如果运行的线程少于 corePoolSize，则创建新线程来处理任务，即使线程池中的其他线程是空闲的。
- 如果线程池中的线程数量大于等于 corePoolSize 且小于 maximumPoolSize，则将任务放在 workQueue 缓冲队列中等待执行。
- 如果线程池中的线程数量大于等于 corePoolSize 且小于 maximumPoolSize，并且 workQueue 缓冲队列已满，则会创建新的线程去处理任务。
- 如果创建新线程后，运行的线程数量大于等于 maximumPoolSize，且此时 workQueue 已满，则通过 handler 所指定的策略来处理新提交的任务。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/11/1453b4f7a67ea5c40ad64c55d7440e2f.jpg)

因此可见：
- 如果设置的 corePoolSize 和 maximumPoolSize 相同，则创建的线程池的大小是固定的。当线程池中的线程数量达到 corePoolSize 且 workQueue 缓冲队列已满时，会直接通过 handler 所指定的策略来处理新提交的任务。
- 新任务提交时，判断的顺序为 corePoolSize --> workQueue --> maximumPoolSize。

### 3.6. 其他方法实现

## 4. 自定义线程池

自定义线程池，可以用 ThreadPoolExecutor 类创建，它有多个构造方法来创建线程池，用该类很容易实现自定义的线程池。

例：
```java
public class ThreadPoolTest{   
    public static void main(String[] args){   
        // 创建等待队列   
        BlockingQueue<Runnable> bqueue = new ArrayBlockingQueue<Runnable>(20);   
        // 创建线程池，池中保存的线程数为 3，允许的最大线程数为 5  
        ThreadPoolExecutor pool = new ThreadPoolExecutor(3,5,50,TimeUnit.MILLISECONDS,bqueue);   
        // 创建七个任务   
        Runnable t1 = new MyThread();   
        Runnable t2 = new MyThread();   
        Runnable t3 = new MyThread();   
        Runnable t4 = new MyThread();   
        Runnable t5 = new MyThread();   
        Runnable t6 = new MyThread();   
        Runnable t7 = new MyThread();   
        // 每个任务会在一个线程上执行  
        pool.execute(t1);   
        pool.execute(t2);   
        pool.execute(t3);   
        pool.execute(t4);   
        pool.execute(t5);   
        pool.execute(t6);   
        pool.execute(t7);   
        // 关闭线程池   
        pool.shutdown();   
    }   
}   
  
class MyThread implements Runnable{   
    @Override   
    public void run(){   
        System.out.println(Thread.currentThread().getName() + "正在执行。");   
        try{   
            Thread.sleep(100);   
        }catch(InterruptedException e){   
            e.printStackTrace();   
        }   
    }   
}  
```

## 5. Refer Links

[深入理解 Java 线程池：ThreadPoolExecutor](https://www.jianshu.com/p/d2729853c4da)

[Java Executor 并发框架（一）整体介绍](https://www.cnblogs.com/vhua/p/5277694.html)

[Java Executor 并发框架（二）剖析 ThreadPoolExecutor 运行过程](http://www.cnblogs.com/vhua/p/5285819.html)

[Java Executor 并发框架（三）ThreadPoolExecutor 队列缓存策略](http://www.cnblogs.com/vhua/p/5297587.html)

[深入分析 java 线程池的实现原理](https://www.jianshu.com/p/87bff5cc8d8c)

[深入理解 Java 线程池：ThreadPoolExecutor](https://www.jianshu.com/p/d2729853c4da)