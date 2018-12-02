- [JVM 常用参数](#jvm-常用参数)
    - [1. 参数设置规则](#1-参数设置规则)
    - [2. 常用参数设置](#2-常用参数设置)
    - [3. Refer Links](#3-refer-links)

# JVM 常用参数

## 1. 参数设置规则

- `-XX:+<option>` 启用 option，例如：`-XX:+PrintGCDetails` 启动打印 GC 信息的选项，其中 `+` 号表示 true，开启的意思。
- `-XX:-<option>` 不启用 option，例如：`-XX:-PrintGCDetails` 关闭启动打印 GC 信息的选项，其中 `-` 号表示 false，关闭的意思。
- `-XX:<option>=<number>` 设定 option 的值为数字类型，可跟单位，例如 32k, 1024m, 2g。例如：`-XX:MaxPermSize=64m`。
- `-XX:<option>=<string>` 设定 option 的值为字符串，例如： `-XX:HeapDumpPath="C:\Users\Daxin\Desktop\jvmgcin"`。

## 2. 常用参数设置

- `-XX:+UserLWPSynchronization` 和 `-XX:+UserBoundThreads` 指定使用的线程模型。
- `-XX:-UseBasedLocking` 禁用新型锁的优化（轻量级锁和偏向锁）。
- JVM 栈设置
  - `-Xss` 限制 JVM 栈大小
    ```bash
    java -Xss128k
    ```
- 直接内存设置
  - `-XX:MaxDirectMemorySize` 限制直接内存的大小
    ```bash
    java -Xmx20M -XX:MaxDirectMemorySize=10M
    ```
- Java 堆设置
  - `-Xms`: 设置堆的初始大小。<!-- TODO: 默认值？ -->
  - `-Xmx`: 设置堆的大小上限。
  - `-XX:NewSize`: 设置堆中年轻代大小。
  - `-XX:MaxPermSize=n`: 设置堆中持久代大小。
  - `-XX:NewRatio=n`: 设置年轻代和年老代的比值。如：`-XX:NewRatio=3`，表示年轻代与年老代比值为 1:3，年轻代占整个年轻代年老代和的 1/4。
  - `-XX:SurvivorRatio=n`: 设置年轻代中 Eden 区与两个 Survivor 区的比值。注意 Survivor 区有两个，如：`-XX:SurvivorRatio=3` 表示 `Eden:Survivor=3:2`，即一个 Survivor 区占整个年轻代的 1/5。

- 垃圾收集器设置
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

## 3. Refer Links

[JVM 参数设置规则以及参数含义](https://blog.csdn.net/Dax1n/article/details/77163540)

[java jvm 参数 -Xms -Xmx -Xmn -Xss 调优总结](https://blog.csdn.net/xiajian2010/article/details/17376157)

TODO:

[JVM实用参数系列](http://ifeve.com/useful-jvm-flags/)