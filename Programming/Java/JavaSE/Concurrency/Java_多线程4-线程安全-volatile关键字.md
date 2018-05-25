
- [Java 多线程 - 线程安全 - volatile 关键字](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---volatile-%E5%85%B3%E9%94%AE%E5%AD%97)
  - [1. 功能特性](#1-%E5%8A%9F%E8%83%BD%E7%89%B9%E6%80%A7)
    - [1.1. 保证内存可见性](#11-%E4%BF%9D%E8%AF%81%E5%86%85%E5%AD%98%E5%8F%AF%E8%A7%81%E6%80%A7)
    - [1.2. 禁止指令重排序](#12-%E7%A6%81%E6%AD%A2%E6%8C%87%E4%BB%A4%E9%87%8D%E6%8E%92%E5%BA%8F)
      - [1.2.1. 使用方法](#121-%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)
      - [1.2.2. 实现原理](#122-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
  - [2. 适用场景](#2-%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF)
  - [3. Refer Links](#3-refer-links)

# Java 多线程 - 线程安全 - volatile 关键字

实际开发中，很多需要并发访问的方法或代码块中只读写了一两个实例，使用 synchronized 关键字或 Lock 锁都会导致浪费大量的开销。因此，JVM 提供了 volatile 关键字，为实例域的同步访问提供了另一种免锁机制，它可以说是 JVM 提供的最轻量级的同步机制。

若一个域被声明为 volatile，则编译器和虚拟机就知道该域是可能会被另一个线程并发更新的。

## 1. 功能特性

当一个变量定义成 volatile 之后，它将具备两种特性。

### 1.1. 保证内存可见性

第一是保证此变量对所有线程的可见性。
  
这里的“可见性”是指当一条线程修改了这个变量的值，新值对于其它线程是可以立即得知的，变量值在线程间传递均需要通过主内存来完成，如：线程 A 修改一个普通变量的值，然后向主内存进行回写，另外一条线程 B 在线程 A 回写完成了之后再从主内存进行读取操作，新变量的值才会对线程 B 可见。

由于 volatile 关键字保证了变量的内存可见性，因此 volatile 变量在各个线程的工作内存中不存在一致性问题。但由于 Java 里的运算并非原子操作，且 volatile 关键字无法提供原子性保证，因此导致 volatile 变量的运算在并发下一样有可能是不安全的。

因此，在不符合以下条件规则的运算场景中，仍然需要通过加锁（synchronized 或 java.util.concurrent 中的原子类）来保证原子性，从而保证线程安全：
- 运算结果不依赖变量的当前值，或者能确保只有单一的线程改变变量的值。
- 变量不需要与其它的状态变量共同参与不变约束。

### 1.2. 禁止指令重排序

第二是禁止指令重排序优化。

#### 1.2.1. 使用方法

普通的变量仅仅会保证在该方法的执行过程中所有依赖赋值结果的地方能获取到正确的结果，而不能保证变量的赋值操作的顺序与程序代码中的执行顺序一致。因为在一个线程的方法执行过程中无法感知到这一点，这也就是 Java 内存模型中描述的所谓的”线程内表现为串行的语义“(Within-Thread As-If-SerialSematics)。

例：单例的双重检测
```java
public class DoubleCheckLock {
    private static DoubleCheckLock instance;
    private DoubleCheckLock(){}
    public static DoubleCheckLock getInstance(){
        // 第一次检测
        if (instance==null){
            // 同步
            synchronized (DoubleCheckLock.class){
                if (instance == null){
                    // 多线程环境下可能会出现问题的地方
                    instance = new DoubleCheckLock();
                }
            }
        }
        return instance;
    }
}
```
这段代码在单线程环境下并没有什么问题，但如果在多线程环境下就可以出现线程安全问题。原因在于某一个线程执行到第一次检测，读取到的 instance 不为 null 时，instance 的引用对象可能没有完成初始化，因为`instance = new DoubleCheckLock();`可以分为以下 3 步完成（伪代码)：
```java
memory = allocate(); //1. 分配对象内存空间
instance(memory);    //2. 初始化对象
instance = memory;   //3. 设置 instance 指向刚分配的内存地址，此时 instance！=null
```
步骤 1 和步骤 2 间可能会重排序：
```java
memory = allocate(); //1. 分配对象内存空间
instance = memory;   //3. 设置 instance 指向刚分配的内存地址，此时 instance！=null，但是对象还没有初始化完成！
instance(memory);    //2. 初始化对象
```
由于步骤 2 和步骤 3 不存在数据依赖关系，而且无论重排前还是重排后程序的执行结果在单线程中并没有改变，因此这种重排优化是允许的。但是指令重排只会保证串行语义的执行的一致性（单线程)，但并不会关心多线程间的语义一致性。所以当一条线程访问 instance 不为 null 时，由于 instance 实例未必已初始化完成，也就造成了线程安全问题。

那么该如何解决呢？我们使用 volatile 禁止 instance 变量被执行指令重排优化即可。
```java
// 禁止指令重排优化
private volatile static DoubleCheckLock instance;
```

#### 1.2.2. 实现原理

若查看使用 volatile 修饰的变量编译后的指令代码，可以发现，对 volatile 修饰的变量赋值后会增加一个 `lock addl $0x0, (%esp)` 操作，这个操作称为内存屏障 (Memory Barrier)，又称内存栅栏。

内存屏障是一个 CPU 指令，它会告诉编译器和 CPU，在执行重排序优化时不能把 Memory Barrier 后边的指令重排序到 Memory Barrier 之前的位置。

## 2. 适用场景

volatile 只提供了内存可见性，而没有提供原子性，操作互斥提供了操作整体的原子性，同一个变量多个线程间的可见性与多个线程中操作互斥是两件事情，所以说**如果用这个关键字来实现高并发的线程安全，是不可靠的**。

什么场景下应该使用 volatile 关键字？

当我们知道了 volatile 的作用，我们也就知道了它应该用在哪些地方，很显然，**最好是那种只有一个线程修改变量，但有多个线程读取变量的地方。也就是对内存可见性要求高，而对原子性要求低的场景**。在这种场景下，使用 volatile 既可以实现线程安全，又能将用于并发安全的资源消耗降到最低。

例：状态标记量
```java
public class Test {
    volatile boolean flag = false;
    // 线程 1
    while(!flag){
        doSomething();
    }
      // 线程 2
    public void setFlag() {
        flag = true;
    }
}
```

## 3. Refer Links

[全面理解 Java 内存模型 (JMM) 及 volatile 关键字](https://blog.csdn.net/javazejian/article/details/72772461)

[你真的了解 volatile 关键字吗](https://www.jianshu.com/p/7798161d7472)