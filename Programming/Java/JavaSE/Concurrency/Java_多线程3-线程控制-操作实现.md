- [Java 多线程 - 线程控制：操作实现](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6%EF%BC%9A%E6%93%8D%E4%BD%9C%E5%AE%9E%E7%8E%B0)
	- [1. 创建线程](#1-%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B)
		- [1.1. 继承 Thread 类](#11-%E7%BB%A7%E6%89%BF-thread-%E7%B1%BB)
		- [1.2. 实现 Runnable](#12-%E5%AE%9E%E7%8E%B0-runnable)
		- [1.3. 实现 Callable 接口](#13-%E5%AE%9E%E7%8E%B0-callable-%E6%8E%A5%E5%8F%A3)
			- [1.3.1. 使用 Callable 和 FutureTask 创建多线程](#131-%E4%BD%BF%E7%94%A8-callable-%E5%92%8C-futuretask-%E5%88%9B%E5%BB%BA%E5%A4%9A%E7%BA%BF%E7%A8%8B)
			- [1.3.2. 使用 Callable 和线程池创建多线程](#132-%E4%BD%BF%E7%94%A8-callable-%E5%92%8C%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%88%9B%E5%BB%BA%E5%A4%9A%E7%BA%BF%E7%A8%8B)
		- [1.4. 比较](#14-%E6%AF%94%E8%BE%83)
	- [2. 中断线程](#2-%E4%B8%AD%E6%96%AD%E7%BA%BF%E7%A8%8B)
	- [3. 线程状态](#3-%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81)
	- [4. 线程优先级](#4-%E7%BA%BF%E7%A8%8B%E4%BC%98%E5%85%88%E7%BA%A7)
	- [5. 后台线程](#5-%E5%90%8E%E5%8F%B0%E7%BA%BF%E7%A8%8B)
	- [6. 线程睡眠](#6-%E7%BA%BF%E7%A8%8B%E7%9D%A1%E7%9C%A0)
	- [7. 线程让步](#7-%E7%BA%BF%E7%A8%8B%E8%AE%A9%E6%AD%A5)
	- [8. 未捕获异常处理器](#8-%E6%9C%AA%E6%8D%95%E8%8E%B7%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E5%99%A8)
	- [9. Refer Links](#9-refer-links)

# Java 多线程 - 线程控制：操作实现

## 1. 创建线程

在 Java 中线程的创建有三种方式：
- 继承 Thread。
- 实现 Runnable 接口。
- 利用 Callable 与 Future。

### 1.1. 继承 Thread 类

通过继承 Thread 类来创建并启动多线程的步骤如下：
1. 定义 Thread 类的子类，并重写该类的 run 方法，该 run 方法的方法体就代表了线程要完成的任务，因此把 run() 方法称为执行体。
1. 创建 Thread 子类的实例，即创建了线程对象。
1. 调用线程对象的 start() 方法来启动该线程。

i.e.
```java
public class FirstMethod extends Thread {
    @Override
    public void run() {
        // 当一个类继承 Thread 类时，直接使用 this 即可获取当前线程对象
        System.out.println(this.getName());
    }

    public static void main(String [] args) {
        FirstMethod firstMethod = new FirstMethod();
        firstMethod.start();
    }
}

```

### 1.2. 实现 Runnable

实现 Runnable 接口来创建并启动多线程的步骤如下：
1. 定义 Runnable 接口的实现类，并重写该接口的 run() 方法，该 run() 方法的方法体同样是该线程的线程执行体。
1. 创建 Runnable 实现类的实例，并依此实例作为 Thread 的 target 来创建 Thread 对象，该 Thread 对象才是真正的线程对象。
1. 调用线程对象的 start() 方法来启动该线程。

例：
```java
Runnable r = () -> {
    try {

    } catch (InterruptedException e) {

    } finally {

    }
};
Thread t = new Thread(r);// 创建线程
t.start();// 启动线程
```

### 1.3. 实现 Callable 接口

直接继承 Thread 或实现 Runnable 接口来创建线程的方式，都有一个缺陷：在执行完任务之后无法获取执行结果。如果需要获取执行结果，就必须通过共享变量或者使用线程通信的方式来达到效果，这样使用起来就比较麻烦。从 JDK1.5 开始，可以通过 Callable 在任务执行完毕之后得到任务执行结果。

#### 1.3.1. 使用 Callable 和 FutureTask 创建多线程

实现 Callable 接口来创建并启动多线程的步骤如下：
1. 创建 Callable 接口的实现类，并实现 call() 方法，该 call() 方法将作为线程执行体，并且有返回值。
1. 创建 Callable 实现类的实例，使用 FutureTask 类来包装 Callable 实现类的对象，该 FutureTask 对象封装了该 Callable 对象的 call() 方法的返回值。
1. 使用 FutureTask 对象作为 Thread 对象的 target 创建并启动新线程。
1. 调用 FutureTask 对象的 get() 方法来获得子线程执行结束后的返回值。

```java
public class Test {
    public static void main(String[] args) {
        Callable<Integer> task = () -> {
            System.out.println("子线程在进行计算");
            Thread.sleep(3000);
            int sum = 0;
            for(int i=0;i<100;i++)
                sum += i;
            return sum;
        };
        FutureTask<Integer> futureTask = new FutureTask<>(task);
        Thread thread = new Thread(futureTask);
        thread.start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }

        System.out.println("主线程在执行任务");

        try {
            System.out.println("task 运行结果" + futureTask.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

        System.out.println("所有任务执行完毕");
    }
}
```

#### 1.3.2. 使用 Callable 和线程池创建多线程

可以使用线程池的方式来将当前实现 Callable 接口的任务通过 ThreadPoolExecutor 的 submit 方法添加到线程池，submit 方法会返回一个 FutureTask 对象，而 FutureTask 是实现了 Future 接口的，因此我们可以使用该对象的 get 方法获取到任务的返回值。
```java
public class Test {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newCachedThreadPool();
        Task task = new Task();
        Future<Integer> result = executor.submit(task);
        executor.shutdown();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }

        System.out.println("主线程在执行任务");

        try {
            System.out.println("task 运行结果"+result.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

        System.out.println("所有任务执行完毕");
    }
}
class Task implements Callable<Integer>{
    @Override
    public Integer call() throws Exception {
        System.out.println("子线程在进行计算");
        Thread.sleep(3000);
        int sum = 0;
        for(int i=0;i<100;i++)
            sum += i;
        return sum;
    }
}
```
执行结果：
```
子线程在进行计算
主线程在执行任务
task 运行结果 4950
所有任务执行完毕
```

### 1.4. 比较

优缺点：
- 实现 Runnable 或实现 Callable 接口

	实现 Runnable 和实现 Callable 接口的方式基本相同，不过是后者执行 call() 方法有返回值，前者线程执行体 run() 方法无返回值，因此可以把这两种方式归为一种方式。

	- 优点
		- 接口创建线程可以实现资源共享，比如多个线程可以共享一个 Runnable 资源。从而将 CPU、代码和数据分开，形成清晰的模型，充分体现了面向对象的思想。
		- 接口创建线程可以避免由于 Java 的单继承特性而带来的局限。
	- 缺点
		- 编程稍微复杂，如果需要访问当前线程，必须调用 Thread.currentThread() 方法。

- 继承 Thread 的优缺点：
	- 优点
		- 编程简单，若需要访问当前线程，直接使用 this 即可获取当前线程对象。
	- 缺点
		- 由于 Java 是单继承的，所以无法再继承其它父类。

综合以上比较，**一般更加推荐采用实现 Runnable 或实现 Callable 接口的方式来创建多线程**。

## 2. 中断线程

线程的退出有以下 3 种情况：
- 线程中的 run 方法执行完方法体中的最后一条语句后，经由执行 return 语句自然返回。
- 出现了在方法中没有捕获的异常，意外退出。
- 调用 [Thread.stop](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html#stop--)​() 方法终止线程，但**该方法在新版 JDK 中已被弃用**。

因此，**没有可以强制线程终止的方法**。但推荐使用 [interrupt](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html#interrupt--)() 方法请求终止线程。

interrupt() 方法会将该线程的中断状态被置为 true，而每个线程应不断地检查该状态标志，以判断线程是否被请求中断，然后定义处理请求的响应方式：
```java
while (!Thread.currentThread().isInterrupted() && more work) {
  // do work;
}
```
当在一个被阻塞的线程上调用 interrupt() 方法时，会引发 InterruptedException 中断。当在一个中断状态为 true 的线程上调用 sleep 方法时，该线程不仅不会休眠，还会清除该中断状态并抛出 InterruptedException。

相关 API
- `static boolean	interrupted​()`：静态方法，检测当前线程是否被中断，该方法会清除该线程中的中断状态。
- `boolean	isInterrupted​()`：实例方法，检验是否有线程被中断，该方法不会改变中断状态。

P.S.

- stop 被弃用的原因？

  当线程要终止另外一个线程时，无法确定什么时候调用 stop() 是安全的，很容易造成对象数据被破坏。因此，弃用了 stop()。转而使用 interrupt() 方法，在希望停止线程的时候发起请求，被请求的线程保证安全的前提下停止。

- suspend 被弃用的原因？

  当用 suspend() 挂起持有一个锁的线程时，该锁在恢复之前是不可用的，若调用 suspend() 的线程同时申请获得该锁，将造成死锁。因此，弃用了 suspend()。

## 3. 线程状态

自 JDK5 开始，在 Thread 类中定义了 State 内部枚举类，**JVM 中的线程必须只能是以下 6 种状态的一种，且这些状态属于 JVM 状态，并不能和操作系统的线程状态互相映射**：
```java
public enum State {
    NEW,          // 线程刚创建，还未调用 start 方法
    RUNNABLE,     // 已就绪可运行的状态，已调用 start 方法。处于此状态的线程是正在 JVM 中运行的，但可能在等待操作系统级别的资源，例如 CPU 时间片
    BLOCKED,      // 阻塞等待监视器锁。处于此状态的线程正在阻塞等待监视器锁，以进入一个同步块 / 方法，或者在执行完 wait() 方法后重入同步块 / 方法
    WAITING,      // 等待。执行完 Object.wait 无超时参数操作，或者 Thread.join 无超时参数操作（进入等待指定的线程执行结束），或者 LockSupport.park 操作后，线程进入等待状态
                  // 一般处于等待状态的线程都在等待其它线程执行特殊操作，例如：等待另其它线程操作 Object.notify() 唤醒或者 Object.notifyAll() 唤醒所有
    TIMED_WAITING,// 限时等待。Thread.sleep、Object.wait 带超时时间、Thread.join 带超时时间、LockSupport.parkNanos、LockSupport.parkUntil 这些操作会时线程进入限时等待
    TERMINATED;   // 终止。线程执行完毕后进入此状态
}
```

![image](http://img.cdn.firejq.com/jpg/2018/4/3/fd64c1b3421cff8698cbc4aece2e8c75.jpg)

- NEW

	当使用 new 关键字创建一个线程之后，该线程处于 NEW 状态，而不会立即进入 RUNNABLE 状态。此时仅由 JVM 为其分配内存，并初始化成员变量的值，但此时该线程对象并未表现出任何线程的动态特征，程序也不会执行线程的线程执行体。

- RUNNABLE

	当线程对象调用 start 方法后，线程进入 RUNNABLE 状态。此时 JVM 会为其创建 Java 虚拟机栈和程序计数器，但线程不一定就开始运行。因为此时 CPU 可能正在执行其他的事情，当获取到 CPU 执行时间之后，线程便真正进入运行状态，开始执行线程对象的 run 方法体。

	当线程获取到 CPU 执行时间正在运行时，可通过 yield 方法让线程主动放弃 CPU 资源，但仍处于 RUNNABLE 状态等待 CPU 下一次调度。

- BLOCKED

	当线程在运行时，发生以下情况之一，则线程进入 BLOCKED 状态：
	- 线程调用了 sleep 方法主动放弃所占用的 CPU 资源
	- 线程调用了一个阻塞式 IO 方法，在该方法返回之前，该线程会被阻塞
	- 线程试图获取一个同步监视器，但该监视器正被其它线程持有
	- 线程正在等待某个通知 notify
	- 线程调用了 suspend 方法将该线程挂起
	进入 BLOCKED 状态的线程会在合适的时候重新进入 RUNNABLE 状态，再次等待获取 CPU 资源才能运行。

- WAITING

	当线程在运行时，发生以下情况之一，则线程进入 WAITING 状态：
	- 线程调用了 Object.wait 无超时参数操作
	- 线程调用了 Thread.join 无超时参数操作（进入等待指定的线程执行结束）
	- 线程调用了 LockSupport.park 方法
	进入 WAITING 状态的线程一般都在等待其它线程执行特殊操作，例如：等待另其它线程操作 Object.notify() / Object.notifyAll() 唤醒。并会在合适的时候重新进入 RUNNABLE 状态，再次等待获取 CPU 资源才能运行。

- TIMED_WAITING

	当线程在运行时，发生以下情况之一，则线程进入 TIMED_WAITING 状态：
	- 线程调用了 Thread.sleep 方法
	- 线程调用了 Object.wait 带超时时间操作
	- 线程调用了 Thread.join 带超时时间操作
	- 线程调用了 LockSupport.parkNanos 方法
	- 线程调用了 LockSupport.parkUntil 方法
	进入 TIMED_WAITING 状态的线程会在合适的时候重新进入 RUNNABLE 状态，再次等待获取 CPU 资源才能运行。

- TERMINATED

	当线程发生以下情况之一时，线程进入 TERMINATED 状态，并无法再切换状态：
	- run/call 方法体执行结束
	- 线程抛出一个未捕获的异常
	- 直接调用该线程对象的 stop 方法

相关 API
- `void	join​()`：等待终止指定的线程。
- `void	join​(long millis)`：等待指定的线程死亡或经过指定毫秒数后超时。

- `Thread.State	getState​()`：获取指定的线程的当前状态。

## 4. 线程优先级

在 Java 的多线程中，每一个线程都有一个优先级。**默认情况下，一个线程继承它的父线程的优先级**。

通过 `void setPriority​(int newPriority)` 方法可以手动设置线程的优先级（可将优先级设置为 `MIN_PRIORITY` 与 `MAX_PRIORITY` 之间的任意值，Thread 中`MAX_PRIORITY`为 10，`MIN_PRIORITY`为 1，`NORM_PRIORITY`为 5）。

NOTE：
- **线程优先级的值越高，表示越优先**。
- **线程优先级是高度依赖于操作系统的**。当虚拟机依赖于宿主机平台的线程实现机制时，Java 线程优先级会被映射到宿主机平台的优先级上，可能更多也可能更少。因此，不要将应用程序构建为功能的正确性依赖于优先级，建议只使用 MIN_PRIORITY、NORM_PRIORITY、MAX_PRIORITY。

相关 API
```java
public final int getPriority() {
		return priority;
}
public final void setPriority(int newPriority) {
		ThreadGroup g;
		checkAccess();
		if (newPriority > MAX_PRIORITY || newPriority < MIN_PRIORITY) {
				throw new IllegalArgumentException();
		}
		if((g = getThreadGroup()) != null) {
				if (newPriority > g.getMaxPriority()) {
						newPriority = g.getMaxPriority();
				}
				setPriority0(priority = newPriority);
		}
}
private native void setPriority0(int newPriority);
```

## 5. 后台线程 / 守护线程

后台线程的唯一用途是为其它线程提供服务，例如用于计时的线程，JVM 的 GC 线程就是典型的后台线程。因此，当所有前台线程都死亡，只剩下后台线程时，虚拟机会自动退出。

由于后台线程可能在任意时刻被强制退出，因此在后台线程中永远不应访问固有资源，如文件、数据库。

通过 `t.setDaemon​(true)` 可将普通线程转换为后台线程。主线程默认是前台线程，且前台线程创建的子线程默认也是前台线程，后台线程创建的子线程默认是后台线程。

相关 API
- `void	setDaemon​(boolean on)`：将普通线程转换为后台线程，该方法必须在线程启动之前调用，否则会引发异常。
- `boolean isDaemon()`: 判断指定线程是否为后台线程。

## 6. 线程睡眠

Thread.sleep() 方法可使线程睡眠，交出 CPU，让 CPU 去执行其他的任务，相当于让线程进入阻塞状态。**sleep 方法不会释放锁，也就是说如果当前线程持有对某个对象的锁，则即使调用 sleep 方法，其他线程也无法访问这个对象**。
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

## 7. 线程让步

调用 yield 方法会让当前线程交出 CPU 权限，让 CPU 去执行其他的线程。它跟 sleep 方法类似，同样不会释放锁。但是 yield 不能控制具体的交出 CPU 的时间，另外，yield 方法只能让拥有相同 / 更高优先级的线程有获取 CPU 执行时间的机会。

注意，调用 yield 方法并不会让线程进入阻塞状态，而是让线程重回就绪状态，它只需要等待重新获取 CPU 执行时间，这一点是和 sleep 方法不一样的。
```java
public static native void yield();
```

## 8. 未捕获异常处理器

线程的 run 方法无法抛出任何异常，但线程发生异常后，会将异常传递到一个用于未捕获异常的处理器。该处理器为一个实现了 Thread.UncaughtExceptionHandler​接口的类（这个接口只有一个方法`void uncaughtExcept(Thread t, Throwable e)`）。

可通过静态方法 `Thread.setDefaultUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)` 为所有线程安装一个默认的处理器，也可以通过方法 `setUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)` 为任何线程安装一个处理器。若不安装处理器，默认处理器为空。

相关 API
- `static void setDefaultUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)`
- `void setUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)`

## 9. Refer Links

[02. 并发编程 (2)Thread 类源码分析](https://juejin.im/post/5a2948ce6fb9a0450e7604d1)

[Thread 类源码剖析](https://www.cnblogs.com/dennyzhangdd/p/7280032.html)