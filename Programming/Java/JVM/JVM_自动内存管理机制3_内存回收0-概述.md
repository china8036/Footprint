- [JVM 内存回收：概述](#jvm-内存回收概述)
  - [1. 基本概念](#1-基本概念)
  - [2. 主要问题](#2-主要问题)
  - [3. GC 日志](#3-gc-日志)
    - [3.1. 日志设置](#31-日志设置)
    - [3.2. 日志说明](#32-日志说明)
  - [4. GC 相关参数](#4-gc-相关参数)
  - [5. Refer Links](#5-refer-links)

# JVM 内存回收：概述

## 1. 基本概念

1960 年诞生于 MIT 的 Lisp 是第一门真正使用内存动态分配和垃圾收集技术的语言。

经过半个多世纪的发展，GC 技术已经相当成熟，进行了自动化的时代。但当需要排查各种内存溢出、内存泄漏问题时，当垃圾收集称为系统达到更高并发量的瓶颈时，我们就需要对这些自动化的技术进行必要的监控和调节。

Java 内存运行时区域中，程序计数器、JVM 栈和本地方法栈随着线程的生命周期创建和销毁，因此这 3 个区域的内存分配和回收都具有确定性，在这几个区域内无需过多考虑 GC 的问题，因为方法结束或线程结束时，内存自然就随着被回收了。而**Java 堆和方法区则不一样，一个接口中的多个实现类需要的内存可能不一样，一个方法中的多个分支需要的内存也可能不一样，我们只有在程序运行期间才能知道会创建哪些对象，因此这部分内存的分配和回收都是动态的，也是垃圾收集机制所关注的内存区域，常说的内存分配和回收指的都是这部分内存**。

## 2. 主要问题

GC 需要解决 3 个主要的问题：
- 哪些内存需要回收？
- 什么时候回收？
- 如何回收？

## 3. GC 日志

### 3.1. 日志设置

Eclipse 设置 GC 日志输出：
1. 在 eclipse 根目录下的 eclipse.ini 配置文件中添加以下参数：
    ```
    -verbose:gc （开启打印垃圾回收日志）
    -Xloggc:eclipse_gc.log （设置垃圾回收日志打印的文件，文件名称可以自定义）
    -XX:+PrintGCTimeStamps （打印垃圾回收时间信息时的时间格式）
    -XX:+PrintGCDetails （打印垃圾回收详情）
    ```
　　添加完以上参数后当启动 Eclipse 后就能在 Eclipse 根目录看到一个 eclipse_gc.log 的 gc 日志文件。

1. 设置 eclipse 初始堆、非堆内存大小以及年轻代 `-Xms50m –Xmx200m -XX:PermSize=30m -XX:MaxPermSize=60m`.

1. 添加 JVM 监控参数 `-Djava.rmi.server.hostname=127.0.0.1 -Dcom.sun.management.jmxremote.port=6688 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false`.

### 3.2. 日志说明

**每一种收集器的日志格式都不一样，但虚拟机设计者为了方便用户阅读，将各个收集器的日志都维持了一定的共性**。

例：
```
Java HotSpot(TM) Client VM (25.25-b02) for windows-x86 JRE (1.8.0_25-b18), built on Oct  7 2014 14:31:05 by "java_re" with MS VC++ 10.0 (VS2010)
Memory: 4k page, physical 8340720k(3545144k free), swap 8787248k(2501780k free)
CommandLine flags: -XX:InitialHeapSize=268435456 -XX:MaxHeapSize=943718400 -XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:-UseLargePagesIndividualAllocation
4.458: [GC (Allocation Failure) 4.458: [DefNew: 69952K->8704K(78656K), 0.0681410 secs] 69952K->22643K(253440K), 0.0690407 secs] [Times: user=0.06 sys=0.02, real=0.08 secs]
5.741: [Full GC (Metadata GC Threshold) 5.741: [Tenured: 13939K->33208K(174784K), 0.1036054 secs] 48618K->33208K(253440K), [Metaspace: 11442K->11442K(12672K)], 0.1041130 secs] [Times: user=0.11 sys=0.00, real=0.11 secs]
12.427: [GC (Allocation Failure) 12.427: [DefNew: 70016K->8704K(78720K), 0.0821340 secs] 103224K->55838K(253504K), 0.0822684 secs] [Times: user=0.08 sys=0.00, real=0.09 secs]
12.787: [Full GC (Metadata GC Threshold) 12.787: [Tenured: 47134K->56307K(174784K), 0.1662610 secs] 59785K->56307K(253504K), [Metaspace: 19215K->19215K(20864K)], 0.1663899 secs] [Times: user=0.16 sys=0.00, real=0.16 secs]
17.018: [GC (Allocation Failure) 17.018: [DefNew: 70016K->8704K(78720K), 0.0475891 secs] 126323K->68567K(253504K), 0.0477196 secs] [Times: user=0.03 sys=0.02, real=0.06 secs]
21.047: [GC (Allocation Failure) 21.047: [DefNew: 78720K->8704K(78720K), 0.0752255 secs] 138583K->83739K(253504K), 0.0753766 secs] [Times: user=0.08 sys=0.00, real=0.06 secs]
21.320: [Full GC (Metadata GC Threshold) 21.320: [Tenured: 75035K->61015K(174784K), 0.2800589 secs] 87423K->61015K(253504K), [Metaspace: 31969K->31969K(34176K)], 0.2802057 secs] [Times: user=0.28 sys=0.00, real=0.29 secs]
```

GC 日志说明：
- GC 打印时间：
  ```
  [ 垃圾回收类型回收时间：[ 收集器名称：年轻代回收前占用大小 -> 年轻代回收后占用大小（年轻代当前容量）, 年轻代局部 GC 时 JVM 暂停处理的时间 ] 堆空间 GC 前占用的空间 -> 堆空间 GC 后占用的空间（堆空间当前容量），GC 过程中 JVM 暂停处理的时间 ]
  ```

- 垃圾回收类型：
  - GC: 一般为堆空间某个区发生了垃圾回收。
  - Full GC: 基本都是整个堆空间及持久代发生了垃圾回收，通常优化的目标之一是尽量减少 GC 和 Full GC 的频率。

- 收集器名称：

  一般都为收集器的简称或别名，通过收集器名称基本都能判断出哪个区域发生了 GC:
  - DefNew: 年轻代（新生代）发生了 GC （若为 DefNew 可知当前 JVM 年轻代使用的串行收集器）。
  - ParNew: 年轻代（新生代）发生了 GC （若为 ParNew 可知当前 JVM 年轻代使用了并行收集器）。
  - Tenured: 老年代发生了 GC。
  - Perm: 永久代发生了 GC。

## 4. GC 相关参数

- 与串行回收器相关的参数
  ```
  -XX:+UseSerialGC: 在新生代和老年代使用串行回收器。
  -XX:+SuivivorRatio: 设置 eden 区大小和 survivor 区大小的比例。
  -XX:+PretenureSizeThreshold: 设置大对象直接进入老年代的阈值。当对象的大小超过这个值时，将直接在老年代分配。
  -XX:MaxTenuringThreshold: 设置对象进入老年代的年龄的最大值。每一次 Minor GC 后，对象年龄就加 1。任何大于这个年龄的对象，一定会进入老年代。
  ```　
- 与并行 GC 相关的参数
  ```
  -XX:+UseParNewGC: 在新生代使用并行收集器。
  -XX:+UseParallelOldGC: 老年代使用并行回收收集器。
  -XX:ParallelGCThreads：设置用于垃圾回收的线程数。通常情况下可以和 CPU 数量相等。但在 CPU 数量比较多的情况下，设置相对较小的数值也是合理的。
  -XX:MaxGCPauseMills：设置最大垃圾收集停顿时间。它的值是一个大于 0 的整数。收集器在工作时，会调整 Java 堆大小或者其他一些参数，尽可能地把停顿时间控制在 MaxGCPauseMills 以内。
  -XX:GCTimeRatio: 设置吞吐量大小，它的值是一个 0-100 之间的整数。假设 GCTimeRatio 的值为 n，那么系统将花费不超过 1/(1+n) 的时间用于垃圾收集。
  -XX:+UseAdaptiveSizePolicy: 打开自适应 GC 策略。在这种模式下，新生代的大小，eden 和 survivor 的比例、晋升老年代的对象年龄等参数会被自动调整，以达到在堆大小、吞吐量和停顿时间之间的平衡点。
  ```
- 与 CMS 回收器相关的参数
  ```
  -XX:+UseConcMarkSweepGC: 新生代使用并行收集器，老年代使用 CMS+ 串行收集器。
  -XX:+ParallelCMSThreads: 设定 CMS 的线程数量。
  -XX:+CMSInitiatingOccupancyFraction: 设置 CMS 收集器在老年代空间被使用多少后触发，默认为 68%。
  -XX:+UseFullGCsBeforeCompaction: 设定进行多少次 CMS 垃圾回收后，进行一次内存压缩。
  -XX:+CMSClassUnloadingEnabled: 允许对类元数据进行回收。
  -XX:+CMSParallelRemarkEndable: 启用并行重标记。
  -XX:CMSInitatingPermOccupancyFraction: 当永久区占用率达到这一百分比后，启动 CMS 回收 （前提是 -XX:+CMSClassUnloadingEnabled 激活了)。
  -XX:UseCMSInitatingOccupancyOnly: 表示只在到达阈值的时候，才进行 CMS 回收。
  -XX:+CMSIncrementalMode: 使用增量模式，比较适合单 CPU。
  ```
- 与 G1 回收器相关的参数
  ```
  -XX:+UseG1GC：使用 G1 回收器。
  -XX:+UnlockExperimentalVMOptions: 允许使用实验性参数。
  -XX:+MaxGCPauseMills: 设置最大垃圾收集停顿时间。
  -XX:+GCPauseIntervalMills: 设置停顿间隔时间。
  ```
- 其他参数
  ```
  -XX:+DisableExplicitGC: 禁用显示 GC。
  ```

## 5. Refer Links