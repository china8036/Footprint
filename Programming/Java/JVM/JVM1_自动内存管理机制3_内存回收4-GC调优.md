- [JVM 内存回收：GC Tuning](#jvm-内存回收gc-tuning)
  - [1. 基本概念](#1-基本概念)
    - [1.1. 必要性](#11-必要性)
    - [1.2. 关键指标](#12-关键指标)
    - [1.3. 基本目的](#13-基本目的)
    - [1.4. 基本步骤](#14-基本步骤)
  - [2. 指导原则](#2-指导原则)
    - [2.1. Parallel GC Tuning](#21-parallel-gc-tuning)
    - [2.2. Garbage First Tuning](#22-garbage-first-tuning)
  - [3. 垃圾收集器选择](#3-垃圾收集器选择)
  - [4. 内存大小设置](#4-内存大小设置)
  - [5. Refer Links](#5-refer-links)

# JVM 内存回收：GC Tuning

## 1. 基本概念

### 1.1. 必要性

> GC tuning is the last task to be done.

准确地说，GC 优化对 Java 基础服务来说是必要的吗？答案是否定的，事实上 GC 优化对 Java 基础服务来说在有些场合是可以省去的。如果 GC 执行时间满足下列所有条件，就没有必要进行 GC 优化了：
- Minor GC 执行非常迅速（50ms 以内）
- Minor GC 没有频繁执行（大约 10s 执行一次）
- Full GC 执行非常迅速（1s 以内）
- Full GC 没有频繁执行（大约 10min 执行一次）
括号中的数字并不是绝对的，它们也随着服务的状态而变化。有些服务可能要求一次 Full GC 在 0.9s 以内，而有些则会放得更宽一些。因此，对于不同的服务，需要按照不同的标准考虑是否需要执行 GC 优化。

### 1.2. 关键指标

评价垃圾收集器性能的 2 个关键指标：
- 吞吐量
- 停顿时间

### 1.3. 基本目的

GC 优化的两个基本目的：
- 将进入老年代的对象数量降到最低

  除了可以在 JDK 7 及更高版本中使用的 G1 收集器以外，其他分代 GC 都是由 Oracle JVM 提供的。关于分代 GC，就是对象在 Eden 区被创建，随后被转移到 Survivor 区，在此之后剩余的对象会被转入老年代。也有一些对象由于占用内存过大，在 Eden 区被创建后会直接被传入老年代。老年代 GC 相对来说会比新生代 GC 更耗时，因此，减少进入老年代的对象数量可以显著降低 Full GC 的频率。你可能会以为减少进入老年代的对象数量意味着把它们留在新生代，事实正好相反，新生代内存的大小是可以调节的。

- 减少 Full GC 的执行时间

  Full GC 的执行时间比 Minor GC 要长很多，因此，如果在 Full GC 上花费过多的时间（超过 1s），将可能出现超时错误。

  如果通过减小老年代内存来减少 Full GC 时间，可能会引起 OutOfMemoryError 或者导致 Full GC 的频率升高；如果通过增加老年代内存来降低 Full GC 的频率，Full GC 的时间可能因此增加。因此，你需要把老年代的大小设置成一个“合适”的值。

### 1.4. 基本步骤

1. 打印 GC 日志
1. 根据日志得到关键性能指标
1. 分析 GC 原因，调优 JVM 参数

## 2. 指导原则

### 2.1. Parallel GC Tuning

- 除非确定，否则不要设置最大 Heap Size。
- 优先设置吞吐量目标。
- 如果吞吐量目标达不到，调大最大内存，不能让操作系统使用 swap 内存；如果仍然达不到，则应降低目标。

### 2.2. Garbage First Tuning

- 避免使用 `-Xmn`、`-XX:NewRatio` 等显式设置 Young 区大小，会覆盖暂停时间目标。
- 对 GC 停顿时间不要太严苛，其吞吐量目标的 90% 的应用程序时间和 10% 的垃圾回收时间，太严苛会直接影响到吞吐量。

## 3. 垃圾收集器选择

Unless your application has rather strict pause time requirements, **first run your application and allow the VM to select a collector. If necessary, adjust the heap size to improve performance**. If the performance still does not meet your goals, then use the following guidelines as a starting point for selecting a collector:

- If the application has a small data set (up to approximately 100 MB), then select the serial collector with the option `-XX:+UseSerialGC`.

- If the application will be run on a single processor and there are no pause time requirements, then let the VM select the collector, or select the serial collector with the option `-XX:+UseSerialGC`.

- If (a) peak application performance is the first priority and (b) there are no pause time requirements or pauses of 1 second or longer are acceptable, then let the VM select the collector, or select the parallel collector with `-XX:+UseParallelGC`.

- If response time is more important than overall throughput and garbage collection pauses must be kept shorter than approximately 1 second, then select the concurrent collector with `-XX:+UseConcMarkSweepGC` or `-XX:+UseG1GC`.

These guidelines provide only a starting point for selecting a collector because performance is dependent on the size of the heap, the amount of live data maintained by the application, and the number and speed of available processors. Pause times are particularly sensitive to these factors, so the threshold of 1 second mentioned previously is only approximate: the parallel collector will experience pause times longer than 1 second on many data size and hardware combinations; conversely, the concurrent collector may not be able to keep pauses shorter than 1 second on some combinations.

If the recommended collector does not achieve the desired performance, first attempt to adjust the heap and generation sizes to meet the desired goals. If performance is still inadequate, then try a different collector: use the concurrent collector to reduce pause times and use the parallel collector to increase overall throughput on multiprocessor hardware.

## 4. 内存大小设置

如何设置内存的大小，没有一个标准答案。
- 大内存空间：
  - 减少了 GC 的次数
  - 提高了 GC 的运行时间
- 小内存空间：
  - 增多了 GC 的次数
  - 降低了 GC 的运行时间

如果服务器资源充足并且 Full GC 能在 1s 内完成，把内存设为 10GB 也是可以的，但是大部分服务器并不处在这种状态中，当内存设为 10GB 时，Full GC 会耗时 10-30s, 具体的时间自然与对象的大小有关。

那么，如何设置内存大小呢？**通常推荐设为 500MB**，这不是说你要通过 `-Xms500m` 和 `-Xmx500m` 参数来设置 WAS 内存。根据 GC 优化之前的状态，如果 Full GC 后还剩余 300MB 的空间，那么把内存设为 1GB 是一个不错的选择（300MB（默认程序占用）+ 500MB（老年代最小空间）+200MB（空闲内存））。这意味着你需要为老年代设置至少 500MB 空间，因此如果你有三个运行服务器，可以把它们的内存分别设置为 1GB，1.5GB，2GB，然后检查结果。实际的运行时间还依赖于服务器的性能和对象大小，因此，最好的方法是创建尽可能多的测量数据并监控它们，从而比较性能测试的结果，获取性能最好的参数设置。

除此之外，在设置内存空间大小时，还需要设置一个参数：NewRatio。NewRatio 的值是新生代和老年代空间大小的比例。如果 `-XX:NewRatio=1`，则新生代空间：老年代空间 =1:1，如果堆内存为 1GB，则`新生代：老年代 = 500MB:500MB`。如果 NewRatio 等于 2，则新生代：老年代 =1:2，因此，NewRatio 的值设置得越大，则老年代空间越大，新生代空间越小。根据经验，当 NewRatio 设为 2 或 3 时，整个 GC 的状态表现得比 `1:1` 更好。

## 5. Refer Links

<!-- TODO: 若要做 GC Tuning，首先至少应将 Document 通读一两遍，其中已包含了最大多数情况的最佳实践 -->

[官方 Document: JDK 8 GC 调优指南](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/toc.html)

[官方 Document: JDK 10 GC 调优指南](https://docs.oracle.com/javase/10/gctuning/introduction-garbage-collection-tuning.htm#JSGCT-GUID-326EB4CF-8C8C-4267-8355-21AB04F0D304)

[官方 Document: 如何选择垃圾收集器](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)