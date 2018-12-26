- [JVM 内存回收：GC 日志](#jvm-内存回收gc-日志)
  - [1. 日志打印](#1-日志打印)
  - [2. 日志格式](#2-日志格式)
    - [2.1. 关注点](#21-关注点)
    - [2.2. 实例说明](#22-实例说明)
  - [3. 日志分析](#3-日志分析)
    - [3.1. gceasy.io](#31-gceasyio)
    - [3.2. GCViewer](#32-gcviewer)
  - [4. Refer Links](#4-refer-links)

# JVM 内存回收：GC 日志

## 1. 日志打印

GC 日志打印的相关参数：
- `-XX:+PrintGCDetails`
- `-XX:+PrintGCTimeStamps`
- `-XX:+PrintGCDateStamps`
- `-Xloggc:$CATALINA_HOME/logs/gc.log`
- `-XX:+PrintHeapAtGC`
- `-XX:+PrintTenuringDistribution`

i.e. Eclipse 设置 GC 日志输出：
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

## 2. 日志格式

**每一种收集器的日志格式都不一样，但虚拟机设计者为了方便用户阅读，将各个收集器的日志都维持了一定的共性**。

### 2.1. 关注点

在分析 GC 日志时，一般按以下优先级顺序关注日志数据：
- 单次 Full GC 运行时间
- 单次 Minor GC 运行时间
- Full GC 运行间隔
- Minor GC 运行间隔
- 整个 Full GC 的时间
- 整个 Minor GC 的运行时间
- 整个 GC 的运行时间
- Full GC 的执行次数
- Minor GC 的执行次数

### 2.2. 实例说明

TODO:

例 1：
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

例 2：
```
2016-07-05T10:43:18.093+0800: 25.395: [GC [PSYoungGen: 274931K->10738K(274944K)] 371093K->147186K(450048K), 0.0668480 secs] [Times: user=0.17 sys=0.08, real=0.07 secs]
2016-07-05T10:43:18.160+0800: 25.462: [Full GC [PSYoungGen: 10738K->0K(274944K)] [ParOldGen: 136447K->140379K(302592K)] 147186K->140379K(577536K) [PSPermGen: 85411K->85376K(171008K)], 0.6763541 secs] [Times: user=1.75 sys=0.02, real=0.68 secs]
```
分析：
- Young GC 日志：

  ![image](http://img.cdn.firejq.com/jpg/2018/11/25/b40a91e31e0593068d72ee3746f87f06.jpg)

- Full GC 日志：

  ![image](http://img.cdn.firejq.com/jpg/2018/11/25/4be60336168616622dc1bef9d6bd6b6c.jpg)

通过上面日志分析得出，PSYoungGen、ParOldGen、PSPermGen 属于 Parallel 收集器。其中 PSYoungGen 表示 gc 回收前后年轻代的内存变化；ParOldGen 表示 gc 回收前后老年代的内存变化；PSPermGen 表示 gc 回收前后永久区的内存变化。young gc 主要是针对年轻代进行内存回收比较频繁，耗时短；full gc 会对整个堆内存进行回城，耗时长，因此一般尽量减少 full gc 的次数。

## 3. 日志分析

### 3.1. gceasy.io

[GC 日志在线分析工具](http://gceasy.io/)

### 3.2. GCViewer

[GCViewer](https://github.com/chewiebug/GCViewer)

## 4. Refer Links

[CMS 日志格式](https://blogs.oracle.com/poonam/understanding-cms-gc-logs)

[G1 日志格式](https://blogs.oracle.com/poonam/understanding-g1-gc-logs)