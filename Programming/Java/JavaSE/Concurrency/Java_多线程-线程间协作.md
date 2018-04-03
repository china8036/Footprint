- [Java 多线程 - 线程间协作](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E9%97%B4%E5%8D%8F%E4%BD%9C)
  - [1. 使用 Object 类的方法](#1-%E4%BD%BF%E7%94%A8-object-%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95)
  - [2. 使用 Condition 接口的方法](#2-%E4%BD%BF%E7%94%A8-condition-%E6%8E%A5%E5%8F%A3%E7%9A%84%E6%96%B9%E6%B3%95)
    - [2.1. 常用 API](#21-%E5%B8%B8%E7%94%A8-api)
      - [2.1.1. 获取 Condition 实例](#211-%E8%8E%B7%E5%8F%96-condition-%E5%AE%9E%E4%BE%8B)
      - [2.1.2. 线程间协作](#212-%E7%BA%BF%E7%A8%8B%E9%97%B4%E5%8D%8F%E4%BD%9C)
    - [2.2. 实现原理](#22-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
  - [3. Refer Links](#3-refer-links)

# Java 多线程 - 线程间协作

当线程在系统内运行时，线程的调度具有一定的透明性，程序通常无法准确控制线程的轮换执行。因此，Java 提供了一些机制，来进行线程间协作，以保证线程的协调运行。

## 1. 使用 Object 类的方法

在 Java 中，可以通过配合调用 Object 对象的 wait() 方法、 notify() 方法或 notifyAll() 方法来实现线程间的通信。这三个方法在 Object 类中定义，不属于 Thread 类的方法。

Object 是所有类的超类，因此，所有的类都拥有 Object 类的方法。但为实现线程间协作，需要由同步监视器对象来调用这 3 个方法，即必须使用 synchronized 关键字修饰方法或代码块，并先获得锁：
- 使用 synchronized 修饰的同步方法，同步监视器是该类的实例对象 this，因此可直接调用这 3 个方法。
- 使用 synchronized 修饰的代码块，同步监视器是 synchronized 括号内指定的对象，必须由该对象调用这 3 个方法。

由于 Object 类中的方法几乎都被声明为 `final native`，因此在子类中不能覆写任何一个方法。

- `wait()`

  该方法释放同步锁，并将当前线程置入休眠状态，直到接到通知或被中断为止。

  `wait()`方法有三种重载形式：
  - `public final native void wait(long timeout) throws InterruptedException`: 指定超时时间的等待（毫秒）。
  - `public final void wait(long timeout, int nanos) throws InterruptedException`: 指定超时时间的等待（毫秒 + 微秒）。
  - `public final void wait() throws InterruptedException`: 无时间参数等待，即一直等待，直到其它线程通知唤醒。

- `notify()`

  该方法用来通知那些可能等待该对象的对象锁的其他线程。如果有多个线程等待，则 JVM 的线程规划器**任意**挑选出其中一个 wait() 状态的线程来发出通知，并使它等待获取该对象的对象锁。
  
  NOTE: notify 后，当前线程不会马上释放该对象锁，wait 所在的线程同样不能马上获取该对象锁，要等到程序退出 synchronized 代码块后，当前线程才会释放锁，wait 所在的线程也才可以获取该对象锁）。

- `notifyAll()`

  该方法唤醒在此同步监视器上 wait 的所有线程。

  NOTE: 原本 wait 的线程被唤醒后还需要等待获取该对象上的锁，因此不会马上执行。

例：使用 wait/notify 机制来实现生产者 - 消费者模型
```java
class Info{ // 定义信息类  
    private String name = "name";// 定义 name 属性，为了与下面 set 的 name 属性区别开  
    private String content = "content" ;// 定义 content 属性，为了与下面 set 的 content 属性区别开  
    private boolean flag = true ;   // 设置标志位，初始时先生产  
    public synchronized void set(String name,String content){  
        while(!flag){  
            try{  
                super.wait() ;  
            }catch(InterruptedException e){  
                e.printStackTrace() ;  
            }  
        }  
        this.setName(name) ;    // 设置名称  
        try{  
            Thread.sleep(300) ;  
        }catch(InterruptedException e){  
            e.printStackTrace() ;  
        }  
        this.setContent(content) ;  // 设置内容  
        flag  = false ; // 改变标志位，表示可以取走  
        super.notify();  
    }  
    public synchronized void get(){  
        while(flag){  
            try{  
                super.wait() ;  
            }catch(InterruptedException e){  
                e.printStackTrace() ;  
            }  
        }  
        try{  
            Thread.sleep(300) ;  
        }catch(InterruptedException e){  
            e.printStackTrace() ;  
        }  
        System.out.println(this.getName() +   
            " --> " + this.getContent()) ;  
        flag  = true ;  // 改变标志位，表示可以生产  
        super.notify();  
    }  
    public void setName(String name){  
        this.name = name ;  
    }  
    public void setContent(String content){  
        this.content = content ;  
    }  
    public String getName(){  
        return this.name ;  
    }  
    public String getContent(){  
        return this.content ;  
    }  
}  
class Producer implements Runnable{ // 通过 Runnable 实现多线程  
    private Info info = null ;      // 保存 Info 引用  
    public Producer(Info info){  
        this.info = info ;  
    }  
    public void run(){  
        boolean flag = true ;   // 定义标记位  
        for(int i=0;i<10;i++){  
            if(flag){  
                this.info.set("姓名 --1","内容 --1") ;    // 设置名称  
                flag = false ;  
            }else{  
                this.info.set("姓名 --2","内容 --2") ;    // 设置名称  
                flag = true ;  
            }  
        }  
    }  
}  
class Consumer implements Runnable{  
    private Info info = null ;  
    public Consumer(Info info){  
        this.info = info ;  
    }  
    public void run(){  
        for(int i=0;i<10;i++){  
            this.info.get() ;  
        }  
    }  
}  
public class ThreadCaseDemo03{  
    public static void main(String args[]){  
        Info info = new Info(); // 实例化 Info 对象  
        Producer pro = new Producer(info) ; // 生产者  
        Consumer con = new Consumer(info) ; // 消费者  
        new Thread(pro).start() ;  
        // 启动了生产者线程后，再启动消费者线程  
        try{  
            Thread.sleep(500) ;  
        }catch(InterruptedException e){  
            e.printStackTrace() ;  
        }  

        new Thread(con).start() ;  
    }  
}  
```

## 2. 使用 Condition 接口的方法

通过 Java 对象的监视器方法（wait()、notify()、notifyAll()）可以简单的实现线程间通信与协作，但由于若不使用 synchronized 关键字进行同步，就不存在隐式的监视器对象，因此这些方法必须配合着 synchronized 关键字使用。

当使用 Lock 对象来进行线程同步时，Java 提供了一个条件对象 [Condition](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/Condition.html) 接口用于线程间的通信协作。

Condition 接口比 synchronized 的等待唤醒机制具有更多的灵活性以及精确性，其特性可简单归纳为以下两点：
- 通过 Condition 能够精细的控制多线程的休眠与唤醒。
- 对于一个锁，我们可以为多个线程间建立不同的 Condition。
在这种情况下，Lock 替代了同步方法或同步代码块，Condition 替代了同步监视器的功能。

### 2.1. 常用 API

#### 2.1.1. 获取 Condition 实例

Java 以 AQS 的内部类的形式为 Condition 接口提供了实现类 AbstractQueuedSynchronizer.ConditionObject，
```java
public class ConditionObject implements Condition, java.io.Serializable {
      /** First node of condition queue. */
      private transient Node firstWaiter;
      /** Last node of condition queue. */
      private transient Node lastWaiter;

      /**
        * Creates a new {@code ConditionObject} instance.
        */
      public ConditionObject() { }

      // 省略其它
}
```
在 Lock 接口的各个实现类中，都包含了扩展自 AQS 的同步器：
```java
abstract static class Sync extends AbstractQueuedSynchronizer {
    private final Sync sync;
    
    final ConditionObject newCondition() {
        return new ConditionObject();
    }
}
```
因此，利用 Lock 对象的 newCondition() 方法即可获得 Condition 接口的实现类：
```java
public Condition newCondition() {
    return sync.newCondition();
}
```
也就是说，Condition 实例被绑定在一个 Lock 对象上，要获得特定 Lock 实例的 Condition 实例对象，只需调用 Lock 对象的 `newCondition()` 方法即可，习惯上为条件对象命名为可以反映它所表达的条件的名字。一个锁对象上可以有一个或多个相关联的条件对象。

#### 2.1.2. 线程间协作

Condition 接口提供了以下几个方法用于线程间协作：
- `await()`

  类似 Object 类的 wait 方法，该方法使得当前线程进入等待状态，直到被唤醒。

  `await()`方法有 2 种重载形式和多种变体形式：
  - `void await() throws InterruptedException;`: 使当前线程进入等待状态直到被通知 (signal) 或中断，当其他线程调用 singal() 或 singalAll() 方法时，该线程将被唤醒。当其他线程调用 interrupt() 方法时可中断当前线程。
  - `boolean await(long time, TimeUnit unit) throws InterruptedException;`: 使当前线程进入等待状态，直到被唤醒或被中断或超时，其中超时时间可指明单位。
  - `void awaitUninterruptibly();`: 使当前线程进入等待状态，直到被唤醒，且不响应中断要求。
  - `long awaitNanos(long nanosTimeout) throws InterruptedException;`: 使当前线程进入等待状态，直到被唤醒或被中断或超时，其中 nanosTimeout 指的等待超时时间，单位纳秒。
  - `boolean awaitUntil(Date deadline) throws InterruptedException;`: 使当前线程进入等待状态，直到被唤醒、中断或到达某个时间限制。如果没到指定时间就被唤醒，返回 true，其他情况返回 false。

- `signal()`

  该方法唤醒一个在此 Lock 对象上等待的线程，若有多个线程在 Lock 对象上等待，则任意选择一个。该线程从等待方法返回前必须获取与 Condition 相关联的锁，功能与 notify() 相同。

- `signalAll()`

  该方法唤醒所有在此 Lock 对象上等待的线程。该线程从等待方法返回前必须获取与 Condition 相关联的锁，功能与 notifyAll() 相同。

例：使用 Condition 来实现生产者 - 消费者模型
```java
public class ResourceByCondition {
    private String name;
    private int count = 1;
    private boolean flag = false;

    // 创建一个锁对象。
    Lock lock = new ReentrantLock();

    // 通过已有的锁获取两组监视器，一组监视生产者，一组监视消费者。
    Condition producer_con = lock.newCondition();
    Condition consumer_con = lock.newCondition();

    /**
     * 生产
     * @param name
     */
    public  void product(String name)
    {
        lock.lock();
        try
        {
            while(flag){
                try{producer_con.await();}catch(InterruptedException e){}
            }
            this.name = name + count;
            count++;
            System.out.println(Thread.currentThread().getName()+"... 生产者 5.0..."+this.name);
            flag = true;
            consumer_con.signal();// 直接唤醒消费线程
        }
        finally
        {
            lock.unlock();
        }
    }

    /**
     * 消费
     */
    public  void consume()
    {
        lock.lock();
        try
        {
            while(!flag){
                try{consumer_con.await();}catch(InterruptedException e){}
            }
            System.out.println(Thread.currentThread().getName()+"... 消费者。5.0......."+this.name);// 消费烤鸭 1
            flag = false;
            producer_con.signal();// 直接唤醒生产线程
        }
        finally
        {
            lock.unlock();
        }
    }
}

public class Mutil_Producer_ConsumerByCondition {

    public static void main(String[] args) {
        ResourceByCondition r = new ResourceByCondition();
        Mutil_Producer pro = new Mutil_Producer(r);
        Mutil_Consumer con = new Mutil_Consumer(r);
        // 生产者线程
        Thread t0 = new Thread(pro);
        Thread t1 = new Thread(pro);
        // 消费者线程
        Thread t2 = new Thread(con);
        Thread t3 = new Thread(con);
        // 启动线程
        t0.start();
        t1.start();
        t2.start();
        t3.start();
    }
}

/**
 * @decrition 生产者线程
 */
class Mutil_Producer implements Runnable {
    private ResourceByCondition r;

    Mutil_Producer(ResourceByCondition r) {
        this.r = r;
    }

    public void run() {
        while (true) {
            r.product("北京烤鸭");
        }
    }
}

/**
 * @decrition 消费者线程
 */
class Mutil_Consumer implements Runnable {
    private ResourceByCondition r;

    Mutil_Consumer(ResourceByCondition r) {
        this.r = r;
    }

    public void run() {
        while (true) {
            r.consume();
        }
    }
}
```

### 2.2. 实现原理
TODO:

Condition 的具体实现类是 AQS 的内部类 ConditionObject。

AQS 中存在两种队列，一种是同步队列，一种是等待队列，而等待队列就相对于 Condition 而言的。注意在使用 Condition 前必须获得锁，同时在 Condition 的等待队列上的结点与前面同步队列的结点是同一个类即 Node，其结点的 waitStatus 的值为 CONDITION。在实现类 ConditionObject 中有两个结点分别是 firstWaiter 和 lastWaiter，firstWaiter 代表等待队列第一个等待结点，lastWaiter 代表等待队列最后一个等待结点，如下：
```java
public class ConditionObject implements Condition, java.io.Serializable {
    // 等待队列第一个等待结点
    private transient Node firstWaiter;
    // 等待队列最后一个等待结点
    private transient Node lastWaiter;
    // 省略其他代码。......
}
```
每个 Condition 都对应着一个等待队列，也就是说如果一个锁上创建了多个 Condition 对象，那么也就存在多个等待队列。等待队列是一个 FIFO 的队列，在队列中每一个节点都包含了一个线程的引用，而该线程就是 Condition 对象上等待的线程。当一个线程调用了 await() 相关的方法，那么该线程将会释放锁，并构建一个 Node 节点封装当前线程的相关信息加入到等待队列中进行等待，直到被唤醒、中断、超时才从队列中移出。Condition 中的等待队列模型如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/3/d3e7be31993b0e96c7cfc5af9f58930c.jpg)

## 3. Refer Links

[深入理解 Java 并发之 synchronized 实现原理](http://blog.csdn.net/javazejian/article/details/72828483)

[深入剖析基于并发 AQS 的（独占锁) 重入锁 (ReetrantLock) 及其 Condition 实现原理](https://blog.csdn.net/javazejian/article/details/75043422#%E7%A5%9E%E5%A5%87%E7%9A%84condition)

[线程间协作：wait、notify、notifyAll](http://wiki.jikexueyuan.com/project/java-concurrency/collaboration-between-threads.html)

[生产者—消费者模型](http://wiki.jikexueyuan.com/project/java-concurrency/pc.html)