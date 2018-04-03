- [Java 多线程 - 线程安全 - 阻塞同步方案 - API层面：同步器](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---%E9%98%BB%E5%A1%9E%E5%90%8C%E6%AD%A5%E6%96%B9%E6%A1%88---api%E5%B1%82%E9%9D%A2%EF%BC%9A%E5%90%8C%E6%AD%A5%E5%99%A8)
	- [1. Refer Linked](#1-refer-linked)

# Java 多线程 - 线程安全 - 阻塞同步方案 - API层面：同步器

- 信号量：[Semaphore](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Semaphore.html)
- 倒计时门栓：[CountDownLatch](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/CountDownLatch.html)
- 障栅：[CyclicBarrier](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/CyclicBarrier.html) 
- 交换器：[Exchanger](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Exchanger.html)
- 阻塞队列: [BlockingQueue](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/BlockingQueue.html)

	Lock/Condition、synchronized/volatile 形成了 Java 并发程序设计的底层构建基础，但**在实际的业务开发中，应远离底层结构，转而使用由并发处理的专业人士实现的较高层次的结构**。

	对于许多线程问题，可通过使用一个或多个队列以优雅安全的方式将其形式化。**生产者线程向队列中插入元素，而消费者线程想队列中取出元素，同步问题由队列类负责实现，当添加元素时队列已满、取出元素时队列为空，阻塞队列导致线程阻塞**。

## 1. Refer Linked