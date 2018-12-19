- [Java 多线程 - 线程安全 - 阻塞同步方案 - 语法层面：synchronized 关键字](#java-多线程---线程安全---阻塞同步方案---语法层面synchronized-关键字)
	- [1. 使用方法](#1-使用方法)
		- [1.1. 同步实例方法](#11-同步实例方法)
		- [1.2. 同步静态方法](#12-同步静态方法)
		- [1.3. 同步代码块](#13-同步代码块)
	- [2. 实现原理](#2-实现原理)
		- [2.1. 对象锁 monitor](#21-对象锁-monitor)
		- [2.2. 显式同步](#22-显式同步)
		- [2.3. 隐式同步](#23-隐式同步)
	- [3. Refer Links](#3-refer-links)

# Java 多线程 - 线程安全 - 阻塞同步方案 - 语法层面：synchronized 关键字

在 Java 中最基本的互斥同步手段是 synchronized 关键字，它可以保证在同一个时刻，只有一个线程可以执行某个方法或者某个代码块。由于 synchronized 在 Java 语言层面以关键字形式提供，因此也称为**内置锁**。

## 1. 使用方法

synchronized 关键字最主要有以下 3 种使用方式。

NOTE: synchronized 关键字不可修饰构造方法、成员变量等。

### 1.1. 同步实例方法

使用 synchronized 关键字修饰实例方法，创建同步实例方法。锁是当前的实例对象，进入实例方法前要获得当前实例的锁。

```java
public class AccountingSync implements Runnable{
    // 共享资源（临界资源)
    static int i=0;
    // synchronized 修饰实例方法
    public synchronized void increase(){
        i++; // 不具备原子性，若不采取同步措施，会导致线程不安全
    }

    @Override
    public void run() {
        for(int j=0;j<1000000;j++){
            increase();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        AccountingSync instance=new AccountingSync();
        // 对同一对象实例进行多线程访问
        Thread t1=new Thread(instance);
        Thread t2=new Thread(instance);
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        System.out.println(i); // 输出结果：2000000
    }
}
```
上述代码对同一共享资源创建了一个对象实例进行多线程的访问，**由于 synchronized 修饰的是实例方法，进入实例方法前要获得当前实例的锁**，而一个对象只有一个锁，因此可以在多线程访问的环境下保障线程安全。

```java
public class AccountingSyncBad implements Runnable{
    static int i=0;
    public synchronized void increase(){
        i++;
    }
    @Override
    public void run() {
        for(int j=0;j<1000000;j++){
            increase();
        }
    }
    public static void main(String[] args) throws InterruptedException {
        // 对不同对象实例进行多线程访问
        Thread t1=new Thread(new AccountingSyncBad());
        Thread t2=new Thread(new AccountingSyncBad());
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        System.out.println(i); // 输出结果：1452317
    }
}
```
上述代码同时创建了两个新实例 AccountingSyncBad，并启动两个不同的线程对静态的共享变量 i 进行操作，由于存在多个实例对象，也就存在多个对象锁，因此无法保证共享资源的线程安全，输出了非期望结果值。

### 1.2. 同步静态方法

使用 synchronized 关键字修饰静态方法，可创建同步静态方法。**锁是当前类的 Class 对象，进入静态方法前要获得当前类 Class 对象的锁**。由于静态成员不专属于任何一个实例对象，因此通过 Class 对象锁可以控制静态成员的并发操作。

NOTE: 若两个线程同时调用同步实例方法和同步静态方法，是可以同时执行的。因为同步实例方法需要获得的是实例对象锁，而同步静态方法需要获得的是类的 Class 对象锁。

```java
public class AccountingSyncClass implements Runnable{
    static int i=0;

    public static synchronized void increase(){
        i++;
    }

    @Override
    public void run() {
        for(int j=0;j<1000000;j++) {
            increase();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread(new AccountingSyncClass());
        Thread t2=new Thread(new AccountingSyncClass());
        t1.start();t2.start();
        t1.join();t2.join();
        System.out.println(i);
    }
}
```

### 1.3. 同步代码块

除了使用关键字修饰实例方法和静态方法外，还可以使用 synchronized 关键字修饰代码块，创建同步代码块。**在 synchronized 括号中指定要加锁的对象，进入同步代码块前要获得给定对象的锁**。

在某些情况下，我们编写的方法体可能比较大，同时存在一些比较耗时的操作，而需要同步的代码又只有一小部分，如果直接对整个方法进行同步操作，可能会得不偿失，此时我们可以使用同步代码块的方式对需要同步的代码进行包裹，这样就无需对整个方法进行同步操作了。

```java
public class AccountingSync implements Runnable{
    static AccountingSync instance=new AccountingSync();
    static int i=0;
    @Override
    public void run() {
        // 省略其他耗时操作。...
        // 使用同步代码块对变量 i 进行同步操作，锁对象为 instance
        synchronized(instance) {
            for (int j=0;j<1000000;j++)
                  i++;
        }
    }
    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread(instance);
        Thread t2=new Thread(instance);
        t1.start();t2.start();
        t1.join();t2.join();
        System.out.println(i);
    }
}
```
还可以使用 this 对象（代表当前实例) 或者当前类的 class 对象作为锁：
```java
// this, 当前实例对象锁
synchronized(this) {
    for(int j=0;j<1000000;j++)
        i++;
}

// Class 对象锁
synchronized(AccountingSync.class) {
    for(int j=0;j<1000000;j++)
        i++;
}
```

## 2. 实现原理

JVM 中的同步 (Synchronization) 基于进入和退出监视器 (Monitor) 对象实现，无论是显式同步（有明确的 monitorenter 和 monitorexit 指令，即同步代码块) 还是隐式同步（同步方法）都是如此。

### 2.1. 对象锁 monitor

**在 HotSpot 中，每个 Java 对象都存在着一个 monitor 对象与之关联**。当对象头中 Mark Word 的锁标志位为 `10` 时，表明该对象持有一个重量级锁，此时对象头中就存储着指向 monitor 对象起始地址的指针。

monitor 是 ObjectMonitor 结构的实例对象，查看 JVM 源码可知 ObjectMonitor 的主要数据结构如下：
```cpp
// ObjectMonitor.hpp
ObjectMonitor() {
    _header       = NULL;
    _count        = 0;      // 记录个数
    _waiters      = 0,
    _recursions   = 0;
    _object       = NULL;
    _owner        = NULL;   // 指向持有 ObjectMonitor 对象的线程
    _WaitSet      = NULL;   // 处于 wait 状态的线程，会被加入到_WaitSet 队列，保存 ObjectWaiter 对象
    _WaitSetLock  = 0;
    _Responsible  = NULL;
    _succ         = NULL;
    _cxq          = NULL;
    FreeNext      = NULL;
    _EntryList    = NULL;  // 处于 block 状态（等待锁）的线程，会被加入到_EntryList 队列，保存 ObjectWaiter 对象
    _SpinFreq     = 0;
    _SpinClock    = 0;
    OwnerIsThread = 0;
}
```
当多个线程同时访问一段同步代码时，会依次进入 monitor 的 _EntryList 队列，
- 若某个线程获取到对象的 monitor，会把 monitor 中的 _owner 变量设置为当前线程，同时将 monitor 中的计数器 _count 加 1 （由此可知 **synchronized 锁是可重入锁**）。
- 若线程调用 wait() 方法，将释放当前持有的 monitor， _owner 变量恢复为 null， _count 自减 1，同时该线程进入 _WaitSet 队列中等待被唤醒。
- 若当前线程执行完毕也将释放 monitor（锁) 并复位变量的值，以便其他线程进入获取 monitor（锁）。

![image](http://img.cdn.firejq.com/jpg/2018/3/31/a7cd1289359cd279095af6a87c35a497.jpg)

**synchronized 就是通过这种方式来获取锁的，这也是为什么 Java 中任意对象可以作为锁的原因，以及为什么 notify/notifyAll/wait 等方法存在于顶级对象 Object 中的原因。**

### 2.2. 显式同步

**显式同步指的是使用 synchronized 关键字修饰代码块的同步，由于在代码中明确确定了锁对象，因此称为显式同步。它是由 monitorenter 和 monitorexit 指令来实现的。**

synchronized 关键字经过编译之后，会在同步块的前后分别形成 monitorenter 和 monitorexit 这两个字节码指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。这两个指令都需要一个 reference 类型的参数来指明要锁定和解锁的对象。

- 在执行 monitorenter 指令时，首先会尝试获取 objectref（即对象锁） 所对应的 monitor 的持有权。
  - 如果这个对象没被锁定，或者当前线程已经拥有了那个对象的锁，那么线程可以成功取得 monitor 或重入这个 monitor，并把锁的计数器加 1。
  - 如果其它线程已经拥有 objectref 的 monitor 的所有权，那当前线程将被阻塞，直到正在执行线程执行完毕。
- 在执行 monitorexit 指令时，会将锁计数器减 1。当计数器为 0 时，执行线程将释放 monitor（锁），从而使其它线程将有机会持有 monitor。

例：
```java
public class SyncCodeBlock {
   public int i;
   public void syncTask(){
       synchronized (this){
           i++;
       }
   }
}
```
通过 `javac .\SyncCodeBlock.java` 将以上代码编译为字节码文件后，使用 `javap -v SyncCodeBlock` 进行反编译可得到以下字节码：
```java
public class SyncCodeBlock {
  public int i;

  public SyncCodeBlock();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public void syncTask();
    Code:
       0: aload_0
       1: dup
       2: astore_1
       3: monitorenter                      // 注意此处，进入同步方法
       4: aload_0
       5: dup
       6: getfield      #2                  // Field i:I
       9: iconst_1
      10: iadd
      11: putfield      #2                  // Field i:I
      14: aload_1
      15: monitorexit                       // 注意此处，退出同步方法
      16: goto          24
      19: astore_2
      20: aload_1
      21: monitorexit                       // 注意此处，退出同步方法，异常结束时才会执行
      22: aload_2
      23: athrow
      24: return
    Exception table:
       from    to  target type
           4    16    19   any
          19    22    19   any
}
```

NOTE:

编译器会确保无论方法通过何种方式完成，方法中调用过的每条 monitorenter 指令都有执行其对应 monitorexit 指令，而无论这个方法是正常结束还是异常结束。

为了保证在方法异常完成时 monitorenter 和 monitorexit 指令依然可以正确配对执行，编译器会自动产生一个异常处理器，这个异常处理器声明可处理所有的异常，它的目的就是用来执行 monitorexit 指令。**从以上字节码中可以看到多了一个 monitorexit 指令，事实上它就是异常结束时被执行的释放 monitor 的指令**。

### 2.3. 隐式同步

**隐式同步指的是使用 synchronized 关键字修饰方法（实例方法和静态方法）的同步。它是由方法调用指令，读取运行时常量池中方法的 ACC_SYNCHRONIZED 标志来隐式实现的。**

JVM 可以从方法常量池中的方法表结构 (method_info Structure) 中的 ACC_SYNCHRONIZED 访问标志区分一个方法是否同步方法。**当方法调用时，调用指令将会检查方法的 ACC_SYNCHRONIZED 访问标志是否被设置**，如果设置了，执行线程将先持有 monitor， 然后再执行方法，最后在方法完成（无论是正常完成还是非正常完成) 时释放 monitor。在方法执行期间，执行线程持有了 monitor，其他任何线程都无法再获得同一个 monitor。如果一个同步方法执行期间抛出了异常，并且在方法内部无法处理此异常，那这个同步方法所持有的 monitor 将在异常抛到同步方法之外时自动释放。

例：
```java
public class SyncMethod {
   public int i;
   public synchronized void syncTask(){
           i++;
   }
}
```
通过 `javac .\SyncMethod.java` 将以上代码编译为字节码文件后，使用 `javap -v SyncMethod` 进行反编译可得到以下字节码：
```java
public class SyncMethod {
  public int i;
    descriptor: I
    flags: (0x0001) ACC_PUBLIC

  public SyncMethod();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0

  public synchronized void syncTask();
    descriptor: ()V
    flags: (0x0021) ACC_PUBLIC, ACC_SYNCHRONIZED // 方法标识 ACC_PUBLIC 代表 public 修饰，ACC_SYNCHRONIZED 指明该方法为同步方法
    Code:
      stack=3, locals=1, args_size=1
         0: aload_0
         1: dup
         2: getfield      #2                  // Field i:I
         5: iconst_1
         6: iadd
         7: putfield      #2                  // Field i:I
        10: return
      LineNumberTable:
        line 4: 0
        line 5: 10
}
```

## 3. Refer Links

[深入理解 Java 并发之 synchronized 实现原理](https://blog.csdn.net/javazejian/article/details/72828483)

[Java 8 并发篇 - 冷静分析 Synchronized（下）](https://zhuanlan.zhihu.com/p/34606653)
