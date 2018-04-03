
# Executor 框架 & 线程池

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

