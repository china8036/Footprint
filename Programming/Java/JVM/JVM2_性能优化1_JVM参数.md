- [JVM 参数](#jvm-参数)
  - [1. 标准参数](#1-标准参数)
  - [2. X 参数](#2-x-参数)
  - [3. XX 参数](#3-xx-参数)
    - [3.1. 参数类型](#31-参数类型)
    - [3.2. 常用参数](#32-常用参数)
  - [4. Refer Links](#4-refer-links)

# JVM 参数

## 1. 标准参数

标准参数在 JVM 的不同版本中基本保持不变，相对比较稳定。

e.g.
- `-help`
- `-server`
- `-client`
- `-version` / `-showversion`
- `-cp` / `-classpath`

## 2. X 参数

X 参数，属于非标准化参数 (extra options)，在 JVM 的不同版本中可能会发生变化。

通过 `java -X` 可查看当前 JVM 支持的 extra options:
```
λ  java -X
    -Xbatch           disable background compilation
    -Xbootclasspath/a:<directories and zip/jar files separated by ;>
                      append to end of bootstrap class path
    -Xcheck:jni       perform additional checks for JNI functions
    -Xcomp            forces compilation of methods on first invocation
    -Xdebug           provided for backward compatibility
    -Xdiag            show additional diagnostic messages
    -Xfuture          enable strictest checks, anticipating future default
    -Xint             interpreted mode execution only
    -Xinternalversion
                      displays more detailed JVM version information than the
                      -version option
    -Xloggc:<file>    log GC status to a file with time stamps
    -Xmixed           mixed mode execution (default)
    -Xmn<size>        sets the initial and maximum size (in bytes) of the heap
                      for the young generation (nursery)
    -Xms<size>        set initial Java heap size
    -Xmx<size>        set maximum Java heap size
    -Xnoclassgc       disable class garbage collection
    -Xprof            output cpu profiling data (deprecated)
    -Xrs              reduce use of OS signals by Java/VM (see documentation)
    -Xshare:auto      use shared class data if possible (default)
    -Xshare:off       do not attempt to use shared class data
    -Xshare:on        require using shared class data, otherwise fail.
    -XshowSettings    show all settings and continue
    -XshowSettings:all
                      show all settings and continue
    -XshowSettings:locale
                      show all locale related settings and continue
    -XshowSettings:properties
                      show all property settings and continue
    -XshowSettings:vm show all vm related settings and continue
    -Xss<size>        set java thread stack size
    -Xverify          sets the mode of the bytecode verifier
```

其中：
- `-Xint`: 解释模式 (interpreted mode)，完全解释执行。
- `-Xcomp`: 编译模式 (compiled mode)，第一次使用就编译成本地代码。
- `-Xmixed`: 混合模式 (mixed mode)（默认），JVM 自己决定是否编译成本地代码。

e.g.
```shell
λ  java -version
    java version "9.0.1"
    Java(TM) SE Runtime Environment (build 9.0.1+11)
    Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)

λ  java -Xint -version
    java version "9.0.1"
    Java(TM) SE Runtime Environment (build 9.0.1+11)
    Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, interpreted mode)

λ  java -Xcomp -version
    java version "9.0.1"
    Java(TM) SE Runtime Environment (build 9.0.1+11)
    Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, compiled mode)

λ  java -Xmixed -version
    java version "9.0.1"
    Java(TM) SE Runtime Environment (build 9.0.1+11)
    Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)
```

## 3. XX 参数

XX 参数，同样属于非标准化参数，在 JVM 的不同版本中可能会发生变化，主要用于 JVM 调优和 Debug。

### 3.1. 参数类型

- Boolean 类型
  - `-XX:+<option>`: 启用该 option，其中 `+` 号表示 true，开启的意思。例如：`-XX:+PrintGCDetails` 启动打印 GC 信息的选项。
  - `-XX:-<option>`: 禁用该 option，其中 `-` 号表示 false，关闭的意思。例如：`-XX:-PrintGCDetails` 关闭启动打印 GC 信息的选项。

- 非 Boolean 类型（Key-Value 类型）
  - `-XX:<option>=<number>`: 设定该 option 的值为数字类型（可跟单位），例如 32k, 1024m, 2g。例如：`-XX:MaxPermSize=64m`。
  - `-XX:<option>=<string>`: 设定该 option 的值为字符串，例如：`-XX:HeapDumpPath="C:\Users\Daxin\Desktop\jvmgcin"`。

### 3.2. 常用参数

- Print Options
  - `-XX:+PrintFlagsInitial`: 查看 JVM 运行时参数的默认值。
  - `-XX:+PrintFlagsFinal`: 查看 JVM 运行时参数的当前值。显示时，`=` 表示当前值是默认值，`:=` 表示当前值是被修改后的值
  - `-XX:+PrintCommandLineFlags`: 查看 JVM 运行时的命令行参数。
  - `-XX:+PrintGCDetails`: 查看 GC 详细信息。

- JVM Stack Options
  - `-XX:ThreadStackSize=<size>` / `-Xss<size>`: 设置 JVM 栈最大值。例如，`java -Xss128k`。

- Java Heap Options
  - `-XX:InitialHeapSize=<size>` / `-Xms<size>`: 设置堆的初始值。
  - `-XX:MaxHeapSize=<size>` / `-Xmx<size>`: 设置堆的最大值。
  - `-XX:NewSize=<size>`: 设置堆中年轻代大小的初始值。
  - `-XX:MaxNewSize=<size>`: 设置堆中年轻代大小的最大值。
  - `-XX:NewRatio=n`: 设置年轻代和年老代的比值。如：`-XX:NewRatio=3`，表示年轻代与年老代比值为 1:3，年轻代占整个年轻代年老代和的 1/4。
  - `-XX:SurvivorRatio=n`: 设置年轻代中 Eden 区与两个 Survivor 区的比值。注意 Survivor 区有两个，如：`-XX:SurvivorRatio=3` 表示 `Eden:Survivor=3:2`，即一个 Survivor 区占整个年轻代的 1/5。

- Method Area / No-Heap Area Options
  - 适用于 JDK <= 1.7
    - `-XX:PermSize=<size>`: 设置非堆区中永久代大小的初始值。
    - `-XX:MaxPermSize=<size>`: 设置非堆区中永久代大小的最大值。
  - 适用于 JDK >= 1.8
    - `-XX:MetaspaceSize=<size>`: 设置非堆区中元数据区大小的初始值。
    - `-XX:MaxMetaspaceSize=<size>`: 设置非堆区中元数据区大小的最大值。
    - `-XX:+UseCompressedClassPointers`: 启用压缩类指针，启用后会在 Metaspace 中产生 CCS 区域（不启用则 CCS 区域大小为 0）。
    - `-XX:CompressedClassSpaceSize=<size>`: 设置 Metaspace 中 CCS 区域大小的初始值。
    - `-XX:InitialCodeCacheSize=<size>`: 设置 Metaspace 中 Code Cache 区域大小的初始值。
    - `-XX:ReservedCodeCacheSize=<size>`: 设置 Metaspace 中 Code Cache 区域大小的最大值。

- Direct Memory Options
  - `-XX:MaxDirectMemorySize=<size>` 限制直接内存的大小。例如，`java -Xmx20M -XX:MaxDirectMemorySize=10M`。

- GC Options
  - 与串行回收器相关的参数
    - `-XX:+UseSerialGC`: 在新生代和老年代使用串行回收器。
    - `-XX:+SuivivorRatio`: 设置 eden 区大小和 survivor 区大小的比例。
    - `-XX:+PretenureSizeThreshold`: 设置大对象直接进入老年代的阈值。当对象的大小超过这个值时，将直接在老年代分配。
    - `-XX:MaxTenuringThreshold=<size>`: 设置对象进入老年代的年龄的最大值。每一次 Minor GC 后，对象年龄就加 1。任何大于这个年龄的对象，一定会进入老年代。
  - 与并行 GC 相关的参数
    - `-XX:+UseParNewGC`: 在新生代使用并行收集器。
    - `-XX:+UseParallelOldGC`: 老年代使用并行回收收集器。
    - `-XX:ParallelGCThreads=n`: 设置用于垃圾回收的线程数。通常情况下可以和 CPU 数量相等。但在 CPU 数量比较多的情况下，设置相对较小的数值也是合理的。
    - `-XX:MaxGCPauseMills=n`: 设置最大垃圾收集停顿时间。它的值是一个大于 0 的整数。收集器在工作时，会调整 Java 堆大小或者其他一些参数，尽可能地把停顿时间控制在 MaxGCPauseMills 以内。
    - `-XX:GCTimeRatio=n`: 设置吞吐量大小，它的值是一个 0-100 之间的整数。假设 GCTimeRatio 的值为 n，那么系统将花费不超过 `1/(1+n)` 的时间用于垃圾收集。
    - `-XX:+UseAdaptiveSizePolicy`: 打开自适应 GC 策略。在这种模式下，新生代的大小，eden 和 survivor 的比例、晋升老年代的对象年龄等参数会被自动调整，以达到在堆大小、吞吐量和停顿时间之间的平衡点。
  - 与 CMS 回收器相关的参数
    - `-XX:+UseConcMarkSweepGC`: 新生代使用并行收集器，老年代使用 CMS+ 串行收集器。
    - `-XX:+ParallelCMSThreads`: 设定 CMS 的线程数量。
    - `-XX:+CMSInitiatingOccupancyFraction`: 设置 CMS 收集器在老年代空间被使用多少后触发，默认为 68%。
    - `-XX:+UseFullGCsBeforeCompaction`: 设定进行多少次 CMS 垃圾回收后，进行一次内存压缩。
    - `-XX:+CMSClassUnloadingEnabled`: 允许对类元数据进行回收。
    - `-XX:+CMSParallelRemarkEndable`: 启用并行重标记。
    - `-XX:CMSInitatingPermOccupancyFraction=n`: 当永久区占用率达到这一百分比后，启动 CMS 回收 （前提是 -XX:+CMSClassUnloadingEnabled 激活了)。
    - `-XX:+UseCMSInitatingOccupancyOnly`: 表示只在到达阈值的时候，才进行 CMS 回收。
    - `-XX:+CMSIncrementalMode`: 使用增量模式，比较适合单 CPU。
  - 与 G1 回收器相关的参数
    - `-XX:+UseG1GC`: 使用 G1 回收器。
    - `-XX:+UnlockExperimentalVMOptions`: 允许使用实验性参数。
    - `-XX:+MaxGCPauseMills`: 设置最大垃圾收集停顿时间。
    - `-XX:+GCPauseIntervalMills`: 设置停顿间隔时间。
  - 其他参数
    - `-XX:+DisableExplicitGC`: 禁用显示 GC。

- Other Options
  - `-XX:+UserLWPSynchronization` 和 `-XX:+UserBoundThreads` 指定使用的线程模型。
  - `-XX:-UseBasedLocking` 禁用新型锁的优化（轻量级锁和偏向锁）。

## 4. Refer Links

[JVM 参数设置规则以及参数含义](https://blog.csdn.net/Dax1n/article/details/77163540)

[java jvm 参数 -Xms -Xmx -Xmn -Xss 调优总结](https://blog.csdn.net/xiajian2010/article/details/17376157)

TODO:

[JVM 实用参数系列](http://ifeve.com/useful-jvm-flags/)