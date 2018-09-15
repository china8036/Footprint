- [Java 多线程 - 线程池 - Executor 框架：基础 API](#java-多线程---线程池---executor-框架基础-api)
	- [1. Executor 接口](#1-executor-接口)
	- [2. ExecutorService 接口](#2-executorservice-接口)
		- [2.1. 基本概念](#21-基本概念)
		- [2.2. 基本 API](#22-基本-api)
		- [2.3. 源码分析](#23-源码分析)
	- [3. ThreadPoolExecutor 实现类](#3-threadpoolexecutor-实现类)
	- [4. ScheduledExecutorService 接口](#4-scheduledexecutorservice-接口)
		- [4.1. 基本概念](#41-基本概念)
		- [4.2. 基本 API](#42-基本-api)
		- [4.3. 源码分析](#43-源码分析)
	- [5. ScheduledThreadPoolExecutor 实现类](#5-scheduledthreadpoolexecutor-实现类)
	- [6. Executors 工厂类](#6-executors-工厂类)
		- [6.1. 基本概念](#61-基本概念)
		- [6.2. 基本 API](#62-基本-api)
			- [6.2.1. 构建线程池](#621-构建线程池)
			- [6.2.2. 其它方法](#622-其它方法)
		- [6.3. 源码分析](#63-源码分析)
			- [6.3.1. 类定义](#631-类定义)
			- [6.3.2. 内部类](#632-内部类)
			- [6.3.3. 线程池方法实现](#633-线程池方法实现)
				- [6.3.3.1. 基于 ThreadPoolExecutor 实现](#6331-基于-threadpoolexecutor-实现)
				- [6.3.3.2. 基于 ScheduledThreadPoolExecutor 实现](#6332-基于-scheduledthreadpoolexecutor-实现)
				- [6.3.3.3. 基于 ForkJoinPool 实现](#6333-基于-forkjoinpool-实现)
			- [6.3.4. 其它方法实现](#634-其它方法实现)
		- [6.4. 使用实例](#64-使用实例)
	- [7. Refer Links](#7-refer-links)

# Java 多线程 - 线程池 - Executor 框架：基础 API

在 Java 5 之前，开发者必须手动实现自己的线程池；**从 Java 5 开始，JDK 在 J.U.C 包中引入了 Executor 框架以内建支持线程池**。Executor 框架中包含了很多新的启动、调度和管理线程的 API，通过该框架来控制线程的启动、执行和关闭，可以简化并发编程的操作。

**Executor 框架包括：线程池，Executor 接口，Executors 工厂类，ExecutorService 接口，CompletionService 接口，Future 接口，Callable 接口，FutureTask 实现类等**。

## 1. Executor 接口

每一个 [Executor](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Executor.html) 接口的实例都是一个线程池，该接口中只定义了一个方法 `execute(Runnable command)`，该方法用来执行一个任务，即一个实现了 Runnable 接口的类。

```java
public interface Executor {
    // 一旦 Runnable 任务传递到 execute（）方法，该方法便会自动在一个线程上执行 
    void execute(Runnable command);
}
```

## 2. ExecutorService 接口

### 2.1. 基本概念

[ExecutorService](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ExecutorService.html) 接口扩展自 Executor 接口，它提供了更丰富的实现多线程的方法，因此我们一般用该接口来实现和管理线程池。

**ExecutorService 线程池的生命周期包括三种状态：运行、关闭、终止**。
- 创建后便进入运行状态。
- 当调用了 shutdown（）方法时，便进入关闭状态，此时意味着 ExecutorService 不再接受新的任务，但它还在执行已经提交了的任务。如果不调用 shutdown（）方法，ExecutorService 会一直处在运行状态，不断接收新的任务，执行新的任务，服务器端一般不需要关闭它，保持一直运行即可。
- 当所有已经提交了的任务执行完后，便到达终止状态。

**ExecutorService 对象表示一个“尽快执行”的线程池，只要线程池中有空闲的线程，就会立即执行线程任务**。

### 2.2. 基本 API

- `submit()`
	- `Future<?> submit(Runnable task)`: Submits a Runnable task for execution and returns a Future representing that task.

	- `<T> Future<T> submit(Runnable task, T result)`: Submits a Runnable task for execution and returns a Future representing that task.

	- `<T> Future<T> submit(Callable<T> task)`: Submits a value-returning task for execution and returns a Future representing the pending results of the task.

	**submit() 和 execute() 方法的区别**：
	- 接收的参数不一样。
	- submit() 有返回值，而 execute() 没有。
	- submit() 可以进行 Exception 处理，如果 task 里会抛出 exception，而你又希望外面的调用者能够感知这些 exception 并做出及时的处理，那么就需要用到 submit，通过对 Future.get() 进行抛出异常的捕获，然后对其进行处理。

- `void shutdown()`: 该方法将启动线程的关闭序列，线程池不再接收新的任务，但会将所有已提交的任务完成。

- `List<Runnable> shutdownNow()`: 该方法试图停止所有正在执行的活动任务，暂停所有正在等待的任务，并返回等待执行的任务列表。

- `boolean isShutdown()`: Returns true if this executor has been shut down.

- `boolean isTerminated()`: Returns true if all tasks have completed following shut down.

- `boolean awaitTermination(long timeout, TimeUnit unit) throws InterruptedException`: Blocks until all tasks have completed execution after a shutdown request, or the timeout occurs, or the current thread is interrupted, whichever happens first.

e.g.
```java
public class CallableDemo{   
    public static void main(String[] args){   
        ExecutorService executorService = Executors.newCachedThreadPool();   
        List<Future<String>> resultList = new ArrayList<Future<String>>();   

        // 创建 10 个任务并执行   
        for (int i = 0; i < 10; i++) {   
            // 使用 ExecutorService 执行 Callable 类型的任务，并将结果保存在 future 变量中   
            Future<String> future = executorService.submit(new TaskWithResult(i));   
            // 将任务执行结果存储到 List 中   
            resultList.add(future);   
        }   

        // 遍历任务的结果   
        for (Future<String> fs : resultList) {   
            try{   
                while(!fs.isDone);//Future 返回如果没有完成，则一直循环等待，直到 Future 返回完成 
                System.out.println(fs.get());     // 打印各个线程（任务）执行的结果   
            }catch(InterruptedException e){   
                e.printStackTrace();   
            }catch(ExecutionException e){   
                e.printStackTrace();   
            }finally{   
                // 启动一次顺序关闭，执行以前提交的任务，但不接受新任务  
                executorService.shutdown();   
            }   
        }   
    }   
}   

class TaskWithResult implements Callable<String> {   
    private int id;   

    public TaskWithResult(int id){   
        this.id = id;   
    }   

    public String call() throws Exception {  
        System.out.println("call() 方法被自动调用！！！" + Thread.currentThread().getName());   
        // 该返回结果将被 Future 的 get 方法得到  
        return "call() 方法被自动调用，任务返回的结果是：" + id + "    " + Thread.currentThread().getName();   
    }   
}  
```

### 2.3. 源码分析

```java
public interface ExecutorService extends Executor {

    void shutdown();

    List<Runnable> shutdownNow();

    boolean isShutdown();

    boolean isTerminated();

    boolean awaitTermination(long timeout, TimeUnit unit)
            throws InterruptedException;

    <T> Future<T> submit(Callable<T> task);

    <T> Future<T> submit(Runnable task, T result);

    Future<?> submit(Runnable task);

    <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks)
            throws InterruptedException;

    <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks,
                                      long timeout, TimeUnit unit)
            throws InterruptedException;
    <T> T invokeAny(Collection<? extends Callable<T>> tasks)
            throws InterruptedException, ExecutionException;

    <T> T invokeAny(Collection<? extends Callable<T>> tasks,
                        long timeout, TimeUnit unit)
            throws InterruptedException, ExecutionException, TimeoutException;
}
```

## 3. ThreadPoolExecutor 实现类

```java
public class ThreadPoolExecutor extends AbstractExecutorService {}
```

```java
public abstract class AbstractExecutorService implements ExecutorService {}
```

## 4. ScheduledExecutorService 接口

### 4.1. 基本概念

[ScheduledExecutorService](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ScheduledExecutorService.html) 接口扩展自 ExecutorService 接口，其对象表示一个“延迟执行”或“周期执行”的线程池。

### 4.2. 基本 API

- `ScheduledFuture<V> schedule(Callable<V> callable, long delay, TimeUnit unit)`: 指定 callable 任务将在 delay 延迟后执行。

- `ScheduledFuture<?> schedule(Runnable command, long delay, TimeUnit unit)`: 指定 runnable 任务将在 delay 延迟后执行。

- `ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)`: 指定 command 任务将在 delay 延迟后执行，并以设定频率周期 period 重复执行。

- `ScheduledFuture<?> scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)`: 创建并执行一个在给定初始延迟后首次启用的定期操作，随后在每一次执行终止和下一次执行开始之间都存在给定的延迟。如果任务在第一次执行时遇到异常，就会取消后续执行；否则，只能通过程序来显式终止或取消该任务。

### 4.3. 源码分析

```java
public interface ScheduledExecutorService extends ExecutorService {
    public ScheduledFuture<?> schedule(Runnable command,
                                       long delay, TimeUnit unit);
    public <V> ScheduledFuture<V> schedule(Callable<V> callable,
                                           long delay, TimeUnit unit);
    public ScheduledFuture<?> scheduleAtFixedRate(Runnable command,
                                                  long initialDelay,
                                                  long period,
                                                  TimeUnit unit);
    public ScheduledFuture<?> scheduleWithFixedDelay(Runnable command,
                                                     long initialDelay,
                                                     long delay,
                                                     TimeUnit unit);
}
```

其中，ScheduledFuture 接口：
```java
public interface ScheduledFuture<V> extends Delayed, Future<V> {
}
```
- Delayed 接口
	```java
	public interface Delayed extends Comparable<Delayed> {
			long getDelay(TimeUnit unit);
	}
	```

- Future 接口
	```java
	public interface Future<V> {
			boolean cancel(boolean mayInterruptIfRunning);
			boolean isCancelled();
			boolean isDone();
			V get() throws InterruptedException, ExecutionException;
			V get(long timeout, TimeUnit unit)
					throws InterruptedException, ExecutionException, TimeoutException;
	}
	```

## 5. ScheduledThreadPoolExecutor 实现类

```java
public class ScheduledThreadPoolExecutor
        extends ThreadPoolExecutor
        implements ScheduledExecutorService {}
```

## 6. Executors 工厂类

### 6.1. 基本概念

Executor 框架提供了一个 [Executors](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Executors.html) 工厂类。

### 6.2. 基本 API

#### 6.2.1. 构建线程池

Executors 工厂类中包含了一系列静态工程方法来构建不同机制的线程池：
- `newCachedThreadPool()`
	- `ExecutorService newCachedThreadPool()`: 创建一个具有缓存功能的线程池，如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60 秒处于等待任务到来）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池的最大值是 Integer 的最大值 (2^31-1)。
	- `ExecutorService newCachedThreadPool(ThreadFactory threadFactory)`

	e.g.
	```java
	ExecutorService executorService = Executors.newCachedThreadPool();
	for (int i = 0; i < 100; i++) {
			executorService.execute(() -> {
				Log.e(TAG, Thread.currentThread().getName());
			});
	}
	```

- `newFixedThreadPool​()`
	- `ExecutorService newFixedThreadPool(int nThreads)`: 创建一个可重用的、具有固定线程数的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，若再提交新任务，任务将会进入等待队列中等待。若某个线程因为执行异常而结束，那么线程池会补充一个新线程。
	- `ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory)`

	e.g.
	```java
	ExecutorService executorService = Executors.newFixedThreadPool(5);
	for (int i = 0; i < 20; i++) {
			executorService.execute(() -> {
				Log.e(TAG, Thread.currentThread().getName());
			});
	}
	```

- `newSingleThreadExecutor​()`
	- `ExecutorService newSingleThreadExecutor()`: 创建一个只有单线程的线程池，该方法相当于调用 newFixedThreadPool() 时传入参数为 1。单线程线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么将会有一个新的线程来替代它。此线程池可保证所有任务的执行顺序按照任务的提交顺序执行。
	- `ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory)`

- `newScheduledThreadPool​()`
	- `ScheduledExecutorService newScheduledThreadPool(int corePoolSize)`: 创建一个具有指定线程数的线程池，它可以在指定延迟后执行线程任务。参数 corePoolSize 表示线程池中所保存的线程数，即使线程是空闲的也被保存在线程池内。多数情况下可用来替代 Timer 类。
	- `ScheduledExecutorService newScheduledThreadPool(int corePoolSize, ThreadFactory threadFactory)`

	e.g.
	```java
	// 从提交任务开始计时，5 秒后执行
	ScheduledExecutorService executorService = Executors.newScheduledThreadPool(10);
	for (int i = 0; i < 20; i++) {
			executorService.schedule(() -> {
				Log.e(TAG, Thread.currentThread().getName());
			}, 5000, TimeUnit.MILLISECONDS);
	}
	```

- `newSingleThreadScheduledExecutor()`
	- `ScheduledExecutorService newSingleThreadScheduledExecutor()`: 创建只有一个线程的线程池，它可以在指定延迟后执行线程任务。
	- `ScheduledExecutorService newSingleThreadScheduledExecutor(ThreadFactory threadFactory)`

- `newWorkStealingPool()`
	- `ExecutorService newWorkStealingPool(int parallelism)`: 创建一个持有足够线程的线程池来支持给定的并行级别。该方法是 JDK8 中新增的静态方法，该方法利用 JDK8 提供的 ForkJoin 框架，可充分利用多核 CPU 的并行能力；除此之外，还会通过多个队列来减少竞争。
	- `ExecutorService newWorkStealingPool()`: 该方法是前一个方法的简化版本。若当前机器有 4 个 CPU，则目标并行级别将会被设置为 4，即相当于调用前一个方法并传入参数为 4。

#### 6.2.2. 其它方法

- `ThreadFactory defaultThreadFactory()`: Returns a default thread factory used to create new threads.

### 6.3. 源码分析

#### 6.3.1. 类定义

```java
public class Executors {}
```

#### 6.3.2. 内部类

```java
/**
	* A callable that runs given task and returns given result
	*/
static final class RunnableAdapter<T> implements Callable<T> {
		final Runnable task;
		final T result;
		// ...
}

/**
	* A callable that runs under established access control settings
	*/
static final class PrivilegedCallable<T> implements Callable<T> {
		private final Callable<T> task;
		private final AccessControlContext acc;
		// ...
}

/**
	* A callable that runs under established access control settings and
	* current ClassLoader
	*/
static final class PrivilegedCallableUsingCurrentClassLoader<T> implements Callable<T> {
		private final Callable<T> task;
		private final AccessControlContext acc;
		private final ClassLoader ccl;
		// ...
}

/**
	* The default thread factory
	*/
static class DefaultThreadFactory implements ThreadFactory {
		private static final AtomicInteger poolNumber = new AtomicInteger(1);
		private final ThreadGroup group;
		private final AtomicInteger threadNumber = new AtomicInteger(1);
		private final String namePrefix;
		DefaultThreadFactory() {
				SecurityManager s = System.getSecurityManager();
				group = (s != null) ? s.getThreadGroup() :
															Thread.currentThread().getThreadGroup();
				namePrefix = "pool-" +
											poolNumber.getAndIncrement() +
											"-thread-";
		}

		public Thread newThread(Runnable r) {
				Thread t = new Thread(group, r,
															namePrefix + threadNumber.getAndIncrement(),
															0);
				if (t.isDaemon())
						t.setDaemon(false);
				if (t.getPriority() != Thread.NORM_PRIORITY)
						t.setPriority(Thread.NORM_PRIORITY);
				return t;
		}
}

/**
	* Thread factory capturing access control context and class loader
	*/
static class PrivilegedThreadFactory extends DefaultThreadFactory {
		private final AccessControlContext acc;
		private final ClassLoader ccl;
		// ...
}

/**
	* A wrapper class that exposes only the ExecutorService methods
	* of an ExecutorService implementation.
	*/
static class DelegatedExecutorService extends AbstractExecutorService {
		private final ExecutorService e;
		DelegatedExecutorService(ExecutorService executor) { e = executor; }
		// ...
}

static class FinalizableDelegatedExecutorService
		extends DelegatedExecutorService {
		FinalizableDelegatedExecutorService(ExecutorService executor) {
				super(executor);
		}
		protected void finalize() {
				super.shutdown();
		}
}

/**
	* A wrapper class that exposes only the ScheduledExecutorService
	* methods of a ScheduledExecutorService implementation.
	*/
static class DelegatedScheduledExecutorService
				extends DelegatedExecutorService
				implements ScheduledExecutorService {
		private final ScheduledExecutorService e;
		DelegatedScheduledExecutorService(ScheduledExecutorService executor) {
				super(executor);
				e = executor;
		}
		// ...
}
```

#### 6.3.3. 线程池方法实现

##### 6.3.3.1. 基于 ThreadPoolExecutor 实现

- `newCachedThreadPool​()`

  CachedThreadPool 是可缓存线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60 秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说 JVM）能够创建的最大线程大小。其中，SynchronousQueue 是一个是缓冲区为 1 的阻塞队列。

  ```java
  // 初始化一个可以缓存线程的线程池，其中线程池的线程数可达到 Integer.MAX_VALUE，即 2147483647
  // keepAliveTime 设置为 60，因此当线程的空闲时间超过 keepAliveTime，会自动释放线程资源，当提交新任务时，如果没有空闲线程，则创建新线程执行任务，会导致一定的系统开销
  // 使用 SynchronousQueue 作为阻塞队列，每个插入操作必须等待另一个线程的对应移除操作
  public static ExecutorService newCachedThreadPool() {
      return new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
  }
  public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory) {
      return new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>(), threadFactory);
  }
  ```
  Thinking in Java 4th:
  > 一般来说，CachedTheadPool 在程序执行过程中通常会创建与所需数量相同的线程，然后在它回收旧线程时停止创建新线程，因此它是合理的 Executor 的首选，只有当这种方式会引发问题时（比如需要大量长时间面向连接的线程时），才需要考虑用 FixedThreadPool。

- `newFixedThreadPool​()`

	FixedThreadPool 是固定数量的线程池，只有核心线程，每提交一个任务就是一个线程，直到达到线程池的最大数量，然后后面进入等待队列，直到前面的任务完成才继续执行。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
	
	```java
	// 初始化一个指定线程数的线程池，其中 corePoolSize == maximumPoolSize；
	// keepAliveTime 设置为 0，因此所有线程都不会失活，即当线程池没有可执行任务时，也不会释放线程，直到调用 shutDown() 方法
	// 使用 LinkedBlockingQuene 作为阻塞队列
	public static ExecutorService newFixedThreadPool(int nThreads) {
			return new ThreadPoolExecutor(nThreads, nThreads,
																		0L, TimeUnit.MILLISECONDS,
																		new LinkedBlockingQueue<Runnable>());
	}
	public static ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory) {
			return new ThreadPoolExecutor(nThreads, nThreads,
																		0L, TimeUnit.MILLISECONDS,
																		new LinkedBlockingQueue<Runnable>(),
																		threadFactory);
	}
	```

- `newSingleThreadExecutor​()`
	
	SingleThreadExecutor 是单个线程的线程池，即线程池中每次只有一个线程在运行，单线程串行执行任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
	
	```java
	// 初始化的线程池中只有一个线程，如果该线程异常结束，会重新创建一个新的线程继续执行任务，唯一的线程可以保证所提交任务的顺序执行
	// 使用 LinkedBlockingQueue 作为阻塞队列
	public static ExecutorService newSingleThreadExecutor() {
			return new FinalizableDelegatedExecutorService
					(new ThreadPoolExecutor(1, 1,
																	0L, TimeUnit.MILLISECONDS,
																	new LinkedBlockingQueue<Runnable>()));
	}
	public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
			return new FinalizableDelegatedExecutorService
					(new ThreadPoolExecutor(1, 1,
																	0L, TimeUnit.MILLISECONDS,
																	new LinkedBlockingQueue<Runnable>(),
																	threadFactory));
	}
	```
##### 6.3.3.2. 基于 ScheduledThreadPoolExecutor 实现

- `newScheduledThreadPool​()`

	ScheduledThreadPool 是核心线程池固定、大小无限制的线程池，支持定时和周期性的执行线程。如果有线程闲置，非核心线程池会在 DEFAULT_KEEPALIVEMILLIS 时间内回收。

	```java
	public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
			return new ScheduledThreadPoolExecutor(corePoolSize);
	}
	public static ScheduledExecutorService newScheduledThreadPool(
					int corePoolSize, ThreadFactory threadFactory) {
			return new ScheduledThreadPoolExecutor(corePoolSize, threadFactory);
	}
	```

- `newSingleThreadScheduledExecutor​()`

	```java
	public static ScheduledExecutorService newSingleThreadScheduledExecutor() {
			return new DelegatedScheduledExecutorService
					(new ScheduledThreadPoolExecutor(1));
	}
	public static ScheduledExecutorService newSingleThreadScheduledExecutor(ThreadFactory threadFactory) {
			return new DelegatedScheduledExecutorService
					(new ScheduledThreadPoolExecutor(1, threadFactory));
	}
	```

##### 6.3.3.3. 基于 ForkJoinPool 实现

- `newWorkStealingPool()`

	```java
	public static ExecutorService newWorkStealingPool(int parallelism) {
			return new ForkJoinPool
					(parallelism,
						ForkJoinPool.defaultForkJoinWorkerThreadFactory,
						null, true);
	}
	// Runtime.getRuntime().availableProcessors() 用于获取当前 CPU 核数
	public static ExecutorService newWorkStealingPool() {
			return new ForkJoinPool
					(Runtime.getRuntime().availableProcessors(),
						ForkJoinPool.defaultForkJoinWorkerThreadFactory,
						null, true);
	}
	```

#### 6.3.4. 其它方法实现

```java
public static ThreadFactory defaultThreadFactory() {
		return new DefaultThreadFactory();
}
```

### 6.4. 使用实例

使用线程池来执行线程任务的一般流程如下：
1. 调用 Executors 工厂类的静态方法创建一个线程池，即 ExecutorService 对象。
1. 创建 Runnable 接口或 Callable 接口的实现类，作为线程执行任务。
1. 调用 ExecutorService 对象的 submit() 方法来提交线程执行任务并执行。
1. 当不再需要添加任务时，调用 ExecutorService 对象的 shutdown() 方法来关闭线程池。

## 7. Refer Links

[深入理解 Java 线程池：ThreadPoolExecutor](https://www.jianshu.com/p/d2729853c4da)

[【从 0 到 1 学习 Java 线程池】Java 线程池的简介以及使用](http://blog.luoyuanhang.com/2017/02/26/thread-pool-in-java-1/)