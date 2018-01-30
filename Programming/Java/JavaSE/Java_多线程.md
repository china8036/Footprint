- [Java 多线程](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B)
  - [基本 API](#%E5%9F%BA%E6%9C%AC-api)
    - [线程创建和执行](#%E7%BA%BF%E7%A8%8B%E5%88%9B%E5%BB%BA%E5%92%8C%E6%89%A7%E8%A1%8C)
    - [中断线程](#%E4%B8%AD%E6%96%AD%E7%BA%BF%E7%A8%8B)
    - [线程状态](#%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81)
    - [线程优先级](#%E7%BA%BF%E7%A8%8B%E4%BC%98%E5%85%88%E7%BA%A7)
    - [守护线程](#%E5%AE%88%E6%8A%A4%E7%BA%BF%E7%A8%8B)
    - [未捕获异常处理器](#%E6%9C%AA%E6%8D%95%E8%8E%B7%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E5%99%A8)
  - [线程同步](#%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A5)
    - [Lock & Condition](#lock-condition)
      - [锁机制](#%E9%94%81%E6%9C%BA%E5%88%B6)
      - [条件对象](#%E6%9D%A1%E4%BB%B6%E5%AF%B9%E8%B1%A1)
    - [synchronized & volatile](#synchronized-volatile)
      - [synchronized](#synchronized)
      - [volatile](#volatile)
    - [原子操作](#%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C)
    - [死锁](#%E6%AD%BB%E9%94%81)
    - [线程局部变量](#%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F)
    - [阻塞队列](#%E9%98%BB%E5%A1%9E%E9%98%9F%E5%88%97)
  - [线程安全的集合](#%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E7%9A%84%E9%9B%86%E5%90%88)
  - [Executor 框架 & 线程池](#executor-%E6%A1%86%E6%9E%B6-%E7%BA%BF%E7%A8%8B%E6%B1%A0)
  - [Fork-Join 框架](#fork-join-%E6%A1%86%E6%9E%B6)
  - [同步器](#%E5%90%8C%E6%AD%A5%E5%99%A8)

# Java 多线程

## 基本 API

### 线程创建和执行

- 存放线程任务：
  - [Runnable](https://docs.oracle.com/javase/9/docs/api/java/lang/Runnable.html) 接口
    
    Runnable 接口的定义十分简单，只包含了一个 `void	run​()` 方法，用于封装一个没有参数和返回值的异步执行的任务。
    
    使用该接口存放线程任务代码的方式有两种：
    - 实现 Runnable 接口，将线程任务代码在 run 方法中实现。
    - Runnable 接口是一个函数式接口，因此可以用 lambda 表达式建立实例存放线程任务代码：
      ```java
      Runnbale r = () -> {
        // task code
      };
      ```

  - [Callable](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Callable.html) 接口

    Callable 接口与 Runnable 接口类似，只有一个方法 call，但是用于封装一个有返回值的异步执行的任务。

    Callable 接口是一个参数化的类型，类似参数即为返回值的类型。`Callable<Integer>` 表示最终返回一个 Integer 对象。

    [Future](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Future.html) 接口用于保存异步执行的结果

    [FutureTask](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/FutureTask.html) 包装器同时实现了 Future 和 Runnable 的接口，可将 Callable 转换为 Future 和 Runnable。
    ```java
    Callable<Integer> compute = () -> {//...};
    FutureTask<Integer> task = new FutureTask<Integer>(compute);
    Thread t = new Thread(task);
    t.start();
    //...
    Integer result = task.get();// 阻塞直到计算完成
    ```

- 创建线程：[Thread](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html) 类

  每一个 Thread 对象实例代表一个创建的线程。
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
  常用 API
  - `Thread​(Runnable target)`：创建一个新的线程，用于调用给定 target 的 run 方法
  
  - `void	start​()`：启动线程。该方法会立即返回，新线程将并发执行。
  
  - `void	run​()`：调用关联 Runnable 对象的 run 方法。

    NOTE：不要调用 Thread 类或 Runnable 接口中的 run 方法，直接调用该方法只会执行同一个线程中的任务，而不会启动新线程。只有调用 start 方法，才会创建一个执行 run 方法的新线程。

### 中断线程

线程的退出有以下两种情况：
- 线程中的 run 方法执行完方法体中的最后一条语句后，经由执行 return 语句自然返回。
- 出现了在方法中没有捕获的异常，意外退出。
- 调用 Thread.stop 方法终止线程，但该方法在新版 JDK 中已被弃用。

因此，没有可以强制线程终止的方法。但推荐使用 [interrupt](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html#interrupt--) 方法请求终止线程。

interrupt 方法会将该线程的中断状态被置为 true，而每个线程应不断地检查该状态标志，以判断线程是否被请求中断，然后定义处理请求的响应方式：
```java
while (!Thread.currentThread().isInterrupted() && more work) {
  // do work;
}
```
当在一个被阻塞的线程上调用 interrupt 方法时，会引发 InterruptedException 中断。当在一个中断状态为 true 的线程上调用 sleep 方法时，该线程不仅不会休眠，还会清除该中断状态并抛出 InterruptedException。

相关 API
- `static boolean	interrupted​()`：静态方法，检测当前线程是否被中断，该方法会清除该线程中的中断状态。
- `boolean	isInterrupted​()`：实例方法，检验是否有线程被中断，该方法不会改变中断状态。

P.S.

- stop 被弃用的原因？

  当线程要终止另外一个线程时，无法确定什么时候调用 stop 是安全的，很容易造成对象数据被破坏。因此，弃用了 stop。转而使用 interrupt 方法，在希望停止线程的时候发起请求，被请求的线程保证安全的前提下停止。

- suspend 被弃用的原因？

  当用 suspend 挂起持有一个锁的线程时，该锁在恢复之前是不可用的，若调用 suspend 的线程同时申请获得该锁，将造成死锁。因此，弃用了 suspend。

### 线程状态

在 Java 中，线程有以下 6 种可能的状态：
- New：新创建。当使用 new 创建一个线程后，还未调用 start 之前该线程的状态为 New。
- Runnable：可执行。一旦调用了 start 方法，线程状态即 Runnable，可能正在运行也可能不在运行，取决于操作系统给线程的执行时间长短。一个正在运行的线程依旧属于 Runnable 状态。
- Blocked：被阻塞。当一个线程试图获取一个内部的对象锁，而该锁被其它线程持有，则该线程进入阻塞状态，直到所有其它线程释放该锁。
- Waiting：等待。当一个线程等待另一个线程通知线程调度器一个条件时，线程本身进入等待状态。被阻塞状态与等待状态是很不同的。
- Timed waiting：计时等待。调用带计时参数的方法，将让线程进入计时等待的状态，直到超时或接收到适当的通知。
- Terminated：被终止。

相关 API
- `void	join​()`：等待终止指定的线程。
- `void	join​(long millis)`：等待指定的线程死亡或经过指定毫秒数后超时。

- `Thread.State	getState​()`：获取指定的线程的当前状态。

### 线程优先级

在 Java 的多线程中，每一个线程都有一个优先级。默认情况下，一个线程继承它的父线程的优先级。

通过 `void	setPriority​(int newPriority)` 方法可以手动设置线程的优先级（可将优先级设置为`MIN_PRIORITY`与`MAX_PRIORITY`之间的任意值，Thread 中`MAX_PRIORITY`为 10，`MIN_PRIORITY`为 1）。

NOTE：线程优先级是高度依赖于操作系统的。当虚拟机依赖于宿主机平台的线程实现机制时，Java 线程优先级会被映射到宿主机平台的优先级上，可能更多也可能更少。因此，不要将应用程序构建为功能的正确性依赖于优先级。

相关 API
- `void	setPriority​(int newPriority)`：设置线程的优先级。
- `static void	yield​()`：静态方法，导致当前线程处于让步状态。若有其它可运行线程具有至少与该线程同样的优先级，则这些线程会在接下来被调度。

### 守护线程

守护线程的唯一用途是为其它线程提供服务，例如用于计时的线程。因此，当只剩下守护线程时，虚拟机会自动退出。

由于守护线程可能在任意时刻被强制退出，因此在守护线程中永远不应访问固有资源，如文件、数据库。

通过 `t.setDaemon​(true)` 可将普通线程转换为守护线程。

相关 API
- `void	setDaemon​(boolean on)`：将普通线程转换为守护线程，该方法必须在线程启动之前调用。

### 未捕获异常处理器

线程的 run 方法无法抛出任何异常，但线程发生异常后，会将异常传递到一个用于未捕获异常的处理器。该处理器为一个实现了 Thread.UncaughtExceptionHandler​接口的类（这个接口只有一个方法`void uncaughtExcept(Thread t, Throwable e)`）。

可通过静态方法 `Thread.setDefaultUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)` 为所有线程安装一个默认的处理器，也可以通过方法 `setUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)` 为任何线程安装一个处理器。若不安装处理器，默认处理器为空。

相关 API
- `static void	setDefaultUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)`
- `void	setUncaughtExceptionHandler​(Thread.UncaughtExceptionHandler eh)`

## 线程同步

线程同步指的是为避免线程之间的竞争条件而采取的同步存取措施。

### Lock & Condition

#### 锁机制

- Lock

  通过锁机制 [Lock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/Lock.html) ，可确保在任何时刻只有一个线程进入临界区，如果锁同时被另一个线程拥有则发生阻塞，从而保证同步的安全。

  基本 API
  - `void	lock​()`
  - `void	unlock​()`
  - `Condition	newCondition​()`
  - `boolean	tryLock​()`：尝试获得锁。若成功获取锁，返回 true；否则，立即返回 false 而不会阻塞。
  - `boolean	tryLock​(long time, TimeUnit unit)`：尝试获得锁，阻塞时间不会超过给定的值。如果线程在阻塞期间被中断，会抛出 InterruptException。
  - `void	lockInterruptibly​()`：相当于一个超时时间设置为无限的 tryLock 方法。

- ReentrantLock

  可重入锁 [ReentrantLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/ReentrantLock.html) 是 Lock 接口的实现类：
  ```java
  Lock mylock = new ReentrantLock();
  // ...
  mylock.lock();
  try {
    // 临界区
  } finally {
    mylock.unlock();
  }
  ```

  锁是可重入的。线程可以重复地获得已持有的锁，锁保持一个持有计数来跟踪对 lock 方法的嵌套调用。线程在每一次调用 lock 后都要调用 unlock 来释放锁。基于这一特性，被一个锁保护的代码可以调用另一个使用相同的锁的方法。

  相关 API
  - `ReentrantLock​()`：构建一个可重入锁
  - `ReentrantLock​(boolean fair)`：构建一个带有公平策略的可重入锁。公平锁偏向于调度等待时间最长的线程，但公平锁会大大降低性能。实际上，即便使用公平锁也无法保证线程调度器是公平的。

- ReentrantReadWriteLock

  读 / 写锁 [ReentrantReadWriteLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/ReentrantReadWriteLock.html) 也是 Lock 接口的实现类，若在多线程应用中很多线程从一个数据结构读取数据而很少线程修改其中数据，可使用该类对读者、写者采用不同的访问控制策略（读者线程可共享访问，写者线程互斥访问）。

  ```java
  ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
  rlock = rwl.readLock();
  wlock = rwl.writeLock();

  // 对所有读方法加读锁
  public int getxxx() {
    rlock.lock();
    try {}
    finally {
      rlock.unlock();
    }
  }

  // 对所有写方法加写锁
  public void setxxx() {
    wlock.lock();
    try {}
    finally {
      wlock.unlock();
    }
  }
  ```

  相关 API
  - `ReentrantReadWriteLock.ReadLock	readLock​()`：得到一个可以被多个读操作共用的读锁，但会排斥所有写操作。
  - `ReentrantReadWriteLock.WriteLock	writeLock​()`：得到一个写锁，排斥所有其它读操作和写操作。

#### 条件对象

条件对象 [Condition](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/Condition.html) 用于管理那些已经获得一个锁，但却需要等待某个条件满足才能进行有效工作的线程。

通过 `lock.newCondition()` 方法可以获取与锁相关联的条件对象，习惯上为条件对象命名为可以反映它所表达的条件的名字。一个锁对象上可以有一个或多个相关联的条件对象。

当在锁保护的代码块中需要等待条件满足时，调用 `condition.await()`，该方法释放当前的锁并阻塞当前线程，进入该条件对象的等待集。当锁可用时，该线程不能马上解除阻塞，而是要等到其它线程调用同一条件的 `condition.signalAll()`，该方法激活所有因为该条件对象而等待的所有线程。同时，它们将试图重新进入该对象，一旦锁成为可用的，其中的某个线程将从 await 调用返回，获得该锁并从被阻塞的地方继续执行。

因此，当一个线程调用 await 时，它没有办法重新激活自身，直至有其它线程调用 signalAll 重新激活等待的线程。一般情况下，在对象的状态有利于等待线程的方向改变时就应该调用 signalAll 方法。

```java
mylock.lock();
try {
  while (some condition) {
    mycondition.await();
  }
  // ... 
  mycondition.signalAll();
} finally {
  mylock.unlock();
}
```

相关 API
- `void	await​()`：将当前线程放到条件等待集中。
- `void	signal​()`：从该条件的等待集中随机选择一个线程，解除其阻塞状态。
- `void	signalAll​()`：解除该条件的等待集中的所有线程的阻塞状态。

### synchronized & volatile

#### synchronized

为简化锁控机制，减小出错概率，Java 提供了一种嵌入到 Java 语言内部的机制，每一个对象都有一个内部锁，若一个方法使用 synchronized 关键字声明，则对象锁将保护整个方法。

```java
public synchronized void method() {
  // ...
}
```
等同于
```java
public void method() {
  this.lock.lock();
  try {

  } finally {
    this.unlock();
  }
}
```

对象内部锁只有一个相关的条件对象。通过 Object 类提供的 final 方法 wait 添加一个线程到等待集中，notifyAll、notify 方法解除等待线程的阻塞状态（这三个方法只能在一个同步方法或同步块中调用，如果当前线程不是对象锁的持有者，方法会抛出 IllegalMonitorStateException）。

#### volatile

实际开发中，同步方法中很多时候只读写了一两个实例，浪费了大量开销。因此，Java 提供了 volatile 关键字，为实例域的同步访问提供了另一种免锁机制。若一个域被声明为 volatile，则编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

```java
private boolean done;
public synchronized boolean isDone() {}
public synchronized void setDone() {}
```
等同于
```java
private volatile boolean done;
public boolean isDone() {}
public void setDone() {}
```

NOTE：volatile 变量不能提供原子性，无法保证读取、翻转、写入不被中断。

### 原子操作

java.util.concurrent.atomatic 包中提供了很多用高效的机器级指令实现的原子性操作，保证了操作不会被中断。

### 死锁

Java 语言中没有任何可以避免死锁或打破死锁的机制，因此开发者必须仔细设计并发的应用程序，以确保不会出现死锁。

### 线程局部变量

当多个线程同时访问共享变量时，需要通过锁机制控制出错的风险。有时可能需要避免共享变量，Java 提供了 [ThreadLocal](https://docs.oracle.com/javase/9/docs/api/java/lang/ThreadLocal.html) 辅助类，为每个线程提供各自的变量实例，即线程局部变量。

```java
ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd"));

String date = dateFormat.get().format(new Date());
```

相关 API
- `T	get​()`：获取当前线程的变量局部实例。首次调用 get 时，会调用 initialize 来得到初始值。
- `protected T	initialValue​()`：应覆盖该方法来提供一个初始值。默认返回 null。
- `void	set​(T value)`：为当前的线程的局部变量设置一个新值。
- `void	remove​()`：删除对应这个线程的值。
- `static <S> ThreadLocal<S>	withInitial​(Supplier<? extends S> supplier)`：创建一个线程局部变量，初始值通过调用给定的 supplier 生成。

### 阻塞队列

Lock/Condition、synchronized/volatile 形成了 Java 并发程序设计的底层构建基础，但在实际的业务开发中，应远离底层结构，转而使用由并发处理的专业人士实现的较高层次的结构。

对于许多线程问题，可通过使用一个或多个队列以优雅安全的方式将其形式化。生产者线程向队列中插入元素，而消费者线程想队列中取出元素，同步问题由队列类负责实现，当添加元素时队列已满、取出元素时队列为空，阻塞队列导致线程阻塞。

Java 中的 java.util.concurrent 包提供了一系列阻塞队列的实现：
- `LinkedBlockingQueue`：无限阻塞队列
- `LinkedBlockingDeque`：无限双端阻塞队列
- `ArrayBlockingQueue`: 有限阻塞队列
- `PriorityBlockingQueue`: 无限优先级阻塞队列
- `DelayQueue`
- `LinkedTransferQueue`：允许生产者线程等待，直到消费者线程准备就绪可以接收一个元素。

## 线程安全的集合

java.util.concurrent 包中提供了映射、有序集、队列的线程安全的高效实现，它们使用复杂的算法，通过允许并发访问数据结构的不同部分来使竞争最小化：
- `ConcurrentHashMap`
- `ConcurrentSkipListMap`
- `ConcurrentSkipListSet`
- `ConcurrentLinkedQueue`

## Executor 框架 & 线程池

http://blog.csdn.net/ns_code/article/details/17465497

应使用线程池的情况：
- 若程序中创建了大量生命周期很短的线程，会造成很多用于线程创建和销毁的开销浪费。这种情况下使用线程池以减少线程重复创建和销毁的无用开销。
- 当创建线程的数量过大时，会造成效率的大大降低甚至 JVM 崩溃。这种情况下使用线程池可减少并发线程的数目，使用固定数目的线程池，限制并发总数。

Java 中可通过 [Executors](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Executors.html) 类的一系列静态工程方法来构建不同机制的线程池：
- `static ExecutorService	newCachedThreadPool​()`
- `static ExecutorService	newFixedThreadPool​(int nThreads)`
- `static ExecutorService	newSingleThreadExecutor​()`
- `static ScheduledExecutorService	newScheduledThreadPool​(int corePoolSize)`
- `static ScheduledExecutorService	newSingleThreadScheduledExecutor​()`

## Fork-Join 框架

http://www.infoq.com/cn/articles/fork-join-introduction

## 同步器

- 信号量
- 倒计时门栓
- 障栅：[CyclicBarrier](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/CyclicBarrier.html) 类
- 交换器
- 同步队列