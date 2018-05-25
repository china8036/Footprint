- [Java 多线程 - 线程控制：基础接口和类](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6%EF%BC%9A%E5%9F%BA%E7%A1%80%E6%8E%A5%E5%8F%A3%E5%92%8C%E7%B1%BB)
	- [1. Runnable 接口](#1-runnable-%E6%8E%A5%E5%8F%A3)
	- [2. Thread 类](#2-thread-%E7%B1%BB)
		- [2.1. 常用 API](#21-%E5%B8%B8%E7%94%A8-api)
		- [2.2. 实现分析](#22-%E5%AE%9E%E7%8E%B0%E5%88%86%E6%9E%90)
			- [2.2.1. 内部属性](#221-%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)
			- [2.2.2. 关键方法](#222-%E5%85%B3%E9%94%AE%E6%96%B9%E6%B3%95)
	- [3. Callable 接口](#3-callable-%E6%8E%A5%E5%8F%A3)
	- [4. Future 接口](#4-future-%E6%8E%A5%E5%8F%A3)
	- [5. FutureTask 包装器](#5-futuretask-%E5%8C%85%E8%A3%85%E5%99%A8)
		- [5.1. 类定义](#51-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [5.2. 内部属性](#52-%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)
		- [5.3. 构造方法](#53-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
	- [6. Refer Links](#6-refer-links)

# Java 多线程 - 线程控制：基础接口和类

## 1. Runnable 接口

[Runnable](https://docs.oracle.com/javase/9/docs/api/java/lang/Runnable.html) 接口位于 java.lang 包下，用于封装一个没有参数和返回值的异步执行的任务。

Runnable 接口的定义十分简单，只包含了一个 `void	run​()` 方法。
```java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

使用该接口存放线程任务代码的方式有两种：
- 实现 Runnable 接口，将线程任务代码在 run 方法中实现。
- 从 JDK8 开始，Runnable 接口实现为一个函数式接口，因此可以用 lambda 表达式建立实例存放线程任务代码：
  ```java
  Runnbale r = () -> {
    // task code
  };
  ```
  
## 2. Thread 类

Java 使用 [Thread](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html) 类代表线程，所有的线程对象都必须是 Thread 类或其子类的实例。

### 2.1. 常用 API

- `Thread​(Runnable target)`：创建一个新的线程，用于调用给定 target 的 run 方法。
- `static Thread	currentThread​()`: 	Returns a reference to the currently executing thread object.
- `String	getName​()`: Returns this thread's name.
- `long	getId​()`: Returns the identifier of this Thread.
- `void	setName​(String name)`: Changes the name of this thread to be equal to the argument name.
- `void	start​()`：启动线程。该方法会立即返回，新线程将并发执行。
- `void	run​()`：调用关联 Runnable 对象的 run 方法。

NOTE：
- 不要调用 Thread 类或 Runnable 接口中的 run 方法，直接调用该方法只会执行同一个线程中的任务，而不会启动新线程。只有调用 start 方法，才会创建一个执行 run 方法的新线程。
- 当 Java 程序开始允许后，程序至少会创建一个主线程，主线程的执行体不是由 run() 方法确定的，而是由 main() 方法确定的。
- 默认情况下，主线程的 name 为 main，其它线程的 name 依次为 thread-0、thread-1...thread-n 等。
- [Q](https://www.zhihu.com/question/53459084): 对于多核 CPU java 中 Thread.currentThread() 指的是哪个核上的线程？

	Thread.currentThread() 返回的是代表当前 Java 线程的 java.lang.Thread 对象的引用。而所谓“当前线程”只跟调用 Thread.currentThread() 的方法在哪个线程上执行有关系。JVM 线程与内核态线程并不是直接映射的关系，因此这跟单核多核没有任何关系。

### 2.2. 实现分析

Thread 类是 Runnable 接口的实现类。

#### 2.2.1. 内部属性

```java
private volatile char  name[];// 线程名称的字节数组
private int    priority;// 线程优先级
private boolean single_step;  // 线程是否单步
private boolean daemon = false;  // 是否是守护线程
private boolean stillborn = false; //JVM state
private Runnable target;  // 从构造方法传过来的 Runnable
private ThreadGroup group; // 线程组
private ClassLoader contextClassLoader;  // 类加载器
private static int threadInitNumber;  // 线程编号
private volatile int threadStatus = 0; // 初始状态
public final static int MIN_PRIORITY = 1; // 最低优先级
public final static int NORM_PRIORITY = 5; // 默认优先级
public final static int MAX_PRIORITY = 10; // 最高优先级
```

#### 2.2.2. 关键方法

- start
	
	start() 用来启动一个线程，当调用 start 方法后，系统才会开启一个新的线程来执行用户定义的子任务，在这个过程中，会为相应的线程分配需要的资源。
	```java
	public synchronized void start() {
			/**
				* This method is not invoked for the main method thread or "system"
				* group threads created/set up by the VM. Any new functionality added
				* to this method in the future may have to also be added to the VM.
				*
				* A zero status value corresponds to state "NEW".
				*/
			if (threadStatus != 0)
					throw new IllegalThreadStateException();

			/* Notify the group that this thread is about to be started
				* so that it can be added to the group's list of threads
				* and the group's unstarted count can be decremented. */
			group.add(this);

			boolean started = false;
			try {
					start0();
					started = true;
			} finally {
					try {
							if (!started) {
									group.threadStartFailed(this);
							}
					} catch (Throwable ignore) {
							/* do nothing. If start0 threw a Throwable then
								it will be passed up the call stack */
					}
			}
	}

	private native void start0();
	```

- run
	
	run() 方法是不需要用户来调用的，当通过 start 方法启动一个线程之后，当线程获得了 CPU 执行时间，便进入 run 方法体去执行具体的任务。注意，继承 Thread 类必须重写 run 方法，在 run 方法中定义具体要执行的任务。
	```java
	@Override
	public void run() {
			if (target != null) {
					target.run();
			}
	}
	```

- sleep

	sleep() 线程睡眠，交出 CPU，让 CPU 去执行其他的任务。sleep 方法不会释放锁，也就是说如果当前线程持有对某个对象的锁，则即使调用 sleep 方法，其他线程也无法访问这个对象。sleep 方法相当于让线程进入阻塞状态。
	```java
 	public static native void sleep(long millis) throws InterruptedException;	
	
	public static void sleep(long millis, int nanos)
    throws InterruptedException {
        if (millis < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                                "nanosecond timeout value out of range");
        }

        if (nanos >= 500000 || (nanos != 0 && millis == 0)) {
            millis++;
        }

        sleep(millis);
	}
	```

- yield

	调用 yield 方法会让当前线程交出 CPU 权限，让 CPU 去执行其他的线程。它跟 sleep 方法类似，同样不会释放锁。但是 yield 不能控制具体的交出 CPU 的时间，另外，yield 方法只能让拥有相同优先级的线程有获取 CPU 执行时间的机会。

	注意，调用 yield 方法并不会让线程进入阻塞状态，而是让线程重回就绪状态，它只需要等待重新获取 CPU 执行时间，这一点是和 sleep 方法不一样的。
	```java
	public static native void yield();
	```

- join

	当调用 thread.join() 方法后，main 线程会进入等待，然后等待 thread 执行完之后再继续执行。
	```java
	// join 方法有三个重载版本
	public final synchronized void join(long millis)
	throws InterruptedException {
			long base = System.currentTimeMillis();
			long now = 0;

			if (millis < 0) {
					throw new IllegalArgumentException("timeout value is negative");
			}

			if (millis == 0) {
					while (isAlive()) {
							// 实际上调用 join 方法是调用了 Object 的 wait 方法
							// wait 方法会让线程进入阻塞状态，并且会释放线程占有的锁，并交出 CPU 执行权限
							wait(0);
					}
			} else {
					while (isAlive()) {
							long delay = millis - now;
							if (delay <= 0) {
									break;
							}
							wait(delay);
							now = System.currentTimeMillis() - base;
					}
			}
	}

	public final synchronized void join(long millis, int nanos)
    throws InterruptedException {

			if (millis < 0) {
					throw new IllegalArgumentException("timeout value is negative");
			}

			if (nanos < 0 || nanos > 999999) {
					throw new IllegalArgumentException(
															"nanosecond timeout value out of range");
			}

			if (nanos >= 500000 || (nanos != 0 && millis == 0)) {
					millis++;
			}

			join(millis);
	}

	public final void join() throws InterruptedException {
			join(0);
	}
	```

- interrupt

　interrupt，中断。单独调用 interrupt 方法可以使得处于阻塞状态的线程抛出一个异常，也就说，它可以用来中断一个正处于阻塞状态的线程。
	```java
	public void interrupt() {
			if (this != Thread.currentThread())
					checkAccess();

			synchronized (blockerLock) {
					Interruptible b = blocker;
					if (b != null) {
							interrupt0();           // Just to set the interrupt flag
							b.interrupt(this);
							return;
					}
			}
			interrupt0();
	}

	private native void interrupt0();
	```

- stop

　stop 方法已经是一个废弃的方法，它是一个不安全的方法。因为调用 stop 方法会直接终止 run 方法的调用，并且会抛出一个 ThreadDeath 错误，如果线程持有某个对象锁的话，会完全释放锁，导致对象状态不一致。所以 stop 方法基本是不会被用到的。
	```java
	@Deprecated
	public final void stop() {
			SecurityManager security = System.getSecurityManager();
			if (security != null) {
					checkAccess();
					if (this != Thread.currentThread()) {
							security.checkPermission(SecurityConstants.STOP_THREAD_PERMISSION);
					}
			}
			// A zero status value corresponds to "NEW", it can't change to
			// not-NEW because we hold the lock.
			if (threadStatus != 0) {
					resume(); // Wake up thread if it was suspended; no-op otherwise
			}

			// The VM can handle all thread states
			stop0(new ThreadDeath());
	}

	private native void stop0(Object o);
	```

## 3. Callable 接口

[Callable](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Callable.html) 接口位于 java.util.concurrent 包下，用于封装一个**有返回值**的**异步执行**的任务。

Callable 接口的定义与 Runnable 接口类似，只包含一个泛型方法：
```java
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```
类似 Runnable 接口，使用 Callable 接口存放线程任务代码的方式有两种：
- 实现 Callable 接口，将线程任务代码在 call 方法中实现。
- 从 JDK8 开始，Callable 接口实现为一个函数式接口，因此可以用 lambda 表达式建立实例存放线程任务代码：
  ```java
  Callable<Integer> compute = () -> {
    //...
  };
  FutureTask<Integer> task = new FutureTask<Integer>(compute);
  Thread t = new Thread(task);
  t.start();
  //...
  Integer result = task.get();// 阻塞直到计算完成
  ```

Callable 接口是一个参数化的类型，类似参数即为返回值的类型。`Callable<Integer>` 表示最终返回一个 Integer 对象。

Callable 里面的 call 方法是可以抛出异常的，可以捕获异常进行处理；而 Runnable 里面的 run 方法是不可以抛出异常的，异常要在 run 方法内部必须得到处理，不能向外界抛出。

## 4. Future 接口

[Future](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Future.html) 接口位于 java.util.concurrent 包下，用于保存异步执行的结果。

查看源码可知，Future 接口中定义了 5 个方法：
```java
public interface Future<V> {
    // 用于取消任务，如果取消任务成功则返回 true，如果取消任务失败则返回 false
    // 参数 mayInterruptIfRunning 表示是否允许取消正在执行却没有执行完毕的任务
    boolean cancel(boolean mayInterruptIfRunning);
    // 判断任务是否被取消成功，如果在任务正常完成前被取消成功，则返回 true
    boolean isCancelled();
    // 判断任务是否已经完成，若任务完成，则返回 true
    boolean isDone();
    // 获取执行结果，这个方法会产生阻塞，直到任务执行完毕才返回
    V get() throws InterruptedException, ExecutionException;
    // 获取执行结果，如果在指定时间内，还没获取到结果，就直接返回 null
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
```

可见 Future 接口提供了三种功能：
- 判断任务是否完成 / 取消。
- 中断任务。
- 获取任务执行结果。

## 5. FutureTask 包装器

[FutureTask](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/FutureTask.html) 包装器位于 java.util.concurrent 包下，它同时实现了 Future 接口和 Runnable 接口，可将 Callable 转换为 Future 和 Runnable。

FutureTask 也是 Future 接口的唯一实现类。

### 5.1. 类定义

FutureTask 包装器的类定义
```java
public class FutureTask<V> implements RunnableFuture<V>
```
RunnableFuture 接口的定义：
```java
public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}
```
可以看出 RunnableFuture 继承了 Runnable 接口和 Future 接口，而 FutureTask 实现了 RunnableFuture 接口。因此，FutureTask 既可以作为 Runnable 被线程执行，又可以作为 Future 得到 Callable 的返回值。

### 5.2. 内部属性

```java
// 线程状态
private volatile int state;
private static final int NEW          = 0;
private static final int COMPLETING   = 1;
private static final int NORMAL       = 2;
private static final int EXCEPTIONAL  = 3;
private static final int CANCELLED    = 4;
private static final int INTERRUPTING = 5;
private static final int INTERRUPTED  = 6;

/** The underlying callable; nulled out after running */
private Callable<V> callable;
/** The result to return or exception to throw from get() */
private Object outcome; // non-volatile, protected by state reads/writes
/** The thread running the callable; CASed during run() */
private volatile Thread runner;
/** Treiber stack of waiting threads */
private volatile WaitNode waiters;
```

### 5.3. 构造方法

FutureTask 提供了 2 个构造器：
```java
public FutureTask(Callable<V> callable) {
    if (callable == null)
        throw new NullPointerException();
    this.callable = callable;
    this.state = NEW;       // ensure visibility of callable
}
public FutureTask(Runnable runnable, V result) {
    this.callable = Executors.callable(runnable, result);
    this.state = NEW;       // ensure visibility of callable
}
```

## 6. Refer Links

[Java 并发编程：Callable、Future 和 FutureTask](https://www.cnblogs.com/dolphin0520/p/3949310.html)

[Callable 与 Runnable 的区别及其在 JDK 源码中的应用](https://blog.csdn.net/hzw19920329/article/details/52383287)

[Thread 类源码剖析](https://www.cnblogs.com/dennyzhangdd/p/7280032.html)
