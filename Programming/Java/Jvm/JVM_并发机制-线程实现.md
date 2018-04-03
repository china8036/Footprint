- [JVM 并发机制 - 线程实现](#jvm-%E5%B9%B6%E5%8F%91%E6%9C%BA%E5%88%B6---%E7%BA%BF%E7%A8%8B%E5%AE%9E%E7%8E%B0)
  - [Refer Links](#refer-links)

# JVM 并发机制 - 线程实现

Java 线程在 JDK1.2 之前，是基于“绿色线程”的用户线程实现的，而在 JDK1.2 中，线程模型替换为基于操作系统原生线程模型来实现，也就是说操作系统支持怎么样的线程模型，很大程度就决定了 Java 虚拟机的线程与内核线程是怎样映射的。因此，java.lang.Thread 类的所有关键方法都是声明为 Native 的，意味着这些方法都使用平台相关的手段来实现。

不同的主流操作系统都提供了不同的线程实现，但事实上，线程模型只是对线程的并发规模和操作成本产生影响，对于 Java 程序的编码和运行过程来说，这些差异都是透明的。Java 在语言层面提供了在不同硬件和操作系统平台下对线程操作的统一处理：每个已执行 start() 方法且还未结束的 java.lang.Thread 类的实例都代表了一个线程。

对于 Sun JDK：
- 由于 Windows 和 Linux 系统提供了一对一的线程模型，因此**在 Windows 和 Linux 系统中 JDK 都是使用一对一线程模型来实现线程的**，也就是说一个 Java 创建的线程就映射到一个轻量级进程之中。
- 而由于 Solaris 系统可以同时支持一对一线程模型和多对多线程模型，因此**在 Solaris 系统中 JDK 也对应提供了专有虚拟机参数：`-XX:+UserLWPSynchronization`和`-XX:+UserBoundThreads`来明确指定 JVM 使用的线程模型类型**。

## Refer Links

《深入理解 Java 虚拟机》 周志明 著

[深入聊聊 java 线程模型实现？](https://www.zhihu.com/question/263955521)