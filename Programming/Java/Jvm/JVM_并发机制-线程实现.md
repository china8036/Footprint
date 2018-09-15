- [JVM 并发机制 - 线程实现](#jvm-并发机制---线程实现)
  - [Refer Links](#refer-links)

# JVM 并发机制 - 线程实现

Java 线程在 JDK1.2 之前，是基于“绿色线程”的用户线程实现的，而在 JDK1.2 中，线程模型替换为基于操作系统原生线程模型来实现，也就是说操作系统支持怎么样的线程模型，很大程度就决定了 Java 虚拟机的线程与内核线程是怎样映射的。因此，**java.lang.Thread 类的所有关键方法都是声明为 Native 的，意味着这些方法都使用平台相关的手段来实现**。

不同的主流操作系统都提供了不同的线程实现，但事实上，线程模型只是对线程的并发规模和操作成本产生影响，对于 Java 程序的编码和运行过程来说，这些差异都是透明的。Java 在语言层面提供了在不同硬件和操作系统平台下对线程操作的统一处理：每个已执行 start() 方法且还未结束的 java.lang.Thread 类的实例都代表了一个线程。

对于 Java 的线程模型，只能针对具体 JVM 实现来看，因为**在 JVM 规范里是没有规定的**，具体实现用 1:1（内核线程）、N:1（用户态线程）、M:N（混合）模型的任何一种都不违反规范。Java 并不暴露出不同线程模型的区别，上层应用是感知不到差异的，只是性能特性会不太一样。

Java SE 最常用的 JVM 是 Oracle/Sun 研发的 HotSpot VM，对于 HotSpot VM：
- **在除 Solaris 系统外的所有系统中，HotSpot VM 都是使用 1:1 线程模型来实现线程的**，也就是说一个 Java 创建的线程就映射到一个轻量级进程之中。
- **在 Solaris 系统中，HotSpot VM 支持 M:N 和 1:1 模型，当前默认是用 1:1 模型。当然它也对应提供了专有虚拟机参数：`-XX:+UserLWPSynchronization`和`-XX:+UserBoundThreads`来明确指定 JVM 具体使用的线程模型类型**。

## Refer Links

《深入理解 Java 虚拟机》 周志明 著

[深入聊聊 java 线程模型实现？](https://www.zhihu.com/question/263955521)

[JVM 中的线程模型是用户级的么？](https://www.zhihu.com/question/23096638)