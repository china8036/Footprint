- [CPU](#cpu)
  - [1. 基本概念](#1-基本概念)
    - [1.1. 物理 CPU](#11-物理-cpu)
    - [1.2. CPU 物理核 (cpu cores)](#12-cpu-物理核-cpu-cores)
    - [1.3. CPU 逻辑核 (processor)](#13-cpu-逻辑核-processor)
    - [1.4. CPU 虚拟核 (vCPU)](#14-cpu-虚拟核-vcpu)
  - [2. 查看 CPU 参数](#2-查看-cpu-参数)
    - [2.1. Linux](#21-linux)
    - [2.2. Windows](#22-windows)
  - [3. CPU 架构](#3-cpu-架构)
    - [3.1. SMP(Symmetric Multi-Processors) / UMA(Uniform Memory Access)](#31-smpsymmetric-multi-processors--umauniform-memory-access)
    - [3.3. NUMA(Non-Uniform Memory Access)](#33-numanon-uniform-memory-access)
    - [3.4. MPP(Massive Parallel Processing)](#34-mppmassive-parallel-processing)
  - [4. Refer Links](#4-refer-links)

# CPU

## 1. 基本概念

### 1.1. 物理 CPU

物理 CPU 是相对于虚拟 CPU 而言的概念，指实际存在的处理器，即实际主板插槽上的 CPU。  

在 /proc/cpuinfo 中，有多少个不同的 physical id 就有多少个物理 CPU：
```bash
$ cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l  
2 
```

### 1.2. CPU 物理核 (cpu cores)

cpu 物理核指的是物理 CPU 上封装的处理数据的芯片组的数量，在每一个 CPU 上，都可能有多个核（core），每一个核中都有独立的一套 ALU、FPU、Cache 等组件。如 i5 760，是双核心四线程的 CPU，而 i5 2250 是四核心四线程的 CPU。

一般来说，`逻辑 CPU 个数 = 物理 CPU 个数 × 每颗核数`，如果不相等的话，则表示服务器的 CPU 支持超线程技术 (Hyper-Threading)。

每个 CPU 核心在同一时间只能执行一个线程，因此在单核 CPU 下的多线程其实都只是并发，不是并行。

P.S.  为什么非得把 CPU 核心封装到一块芯片中呢？不能通过简单得增加 CPU 个数来达到同样效果吗？

答：多核 CPU，资源共享（共享缓存），沟通方便（CPU 内数据传输速度远大于总线速度）。

在 /proc/cpuinfo 中，cpu cores 记录了对应的物理 CPU（以该条目中的 physical id 标识）有多少个物理核：
```bash
$ cat /proc/cpuinfo |grep "cores"|uniq  
6   
```

### 1.3. CPU 逻辑核 (processor)

CPU 逻辑核主要用在 Intel 超线程的环境下，将原本的一个物理核心虚拟成多个核心，从而实现单物理核心并行的调度多个线程。当有多个计算任务时，可以让其中一个计算任务使用 ALU 的时候，另一个则去使用 FPU，从而充分利用物理核中的各个部件，使得同一个物理核中也可以并行处理多个计算任务。

因此在开启超线程时，`CPU 线程数 = 总逻辑 CPU 数 = 物理 CPU 个数 × 每颗物理 CPU 的核数 × 超线程数`。

eg: Intel i5 4200H CPU

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/21/497195ab2d2c12bf24c24b26c06bfb64.jpg)

即表示有 2 个物理核心，使用 HT 超线程数为 2，使线程数为 2 x 2 = 4。

而 AMD CPU 没有超线程技术，物理核心数就等于 CPU 线程数，也因此对于 AMD 的 CPU 来说，只有核心数的概念，没有线程数的概念。

在 /proc/cpuinfo 中，processor 指的就是 CPU 逻辑核数。
```bash
$ cat /proc/cpuinfo |grep "processor"|wc -l  
24 # 2 个物理 CPU，每个 CPU 有 6 个 core，同时支持超线程，因此共 24 个逻辑 CPU
```
使用 top 查看负载时，按 1，看到的 CPU0~CPUn 也是 processor number。在 Windows 系统下看到的 CPU 数量实际上也是逻辑 CPU 数量。

VM 虚拟机中的 CPU 选择的核心数实际上指的是 CPU 逻辑核。

在设置线程数时，对于计算密集型的任务，一般建议将线程数设置为物理核数而不是逻辑核数。因为计算密集任务一般对各个计算部件（FPU\ALU）的使用并不是均匀的，一般 ALU 的使用占大头，FPU 的使用只占小部分，所以超线程技术并不能带来很大的并行度提升。而这一点点提升，也被线程切换带来的消耗所抵消了。最终导致“超线程”技术，并没有像理论中的那样加大并行度，从而提高吞吐量。

### 1.4. CPU 虚拟核 (vCPU)

虚拟 cpu 是在做虚拟化时利用虚拟化技术虚拟出来的 CPU。讨论 vCPU 离不开 VM，因此 vCPU 的讨论都是在虚拟化时候，划分 cpu 才会讨论的问题。通常一个物理 CPU 按照 1:4 ~ 1：10 的比例划分，假如我们有 4 个 8 物理核心的 CPU 按照 1:5 的比例划分，可以得到 4 x 8 x 5 = 160 vCPU。

## 2. 查看 CPU 参数

### 2.1. Linux

Linux 中查看 CPU 参数一般通过 `/proc/cpuinfo` 来查看。在 `/proc/cpuinfo` 中，具有相同 physical id 的 CPU 是同一个物理 CPU 封装的物理核，具有相同 core id 的 CPU 是同一个物理核的多个超线程（逻辑核）：
```shell
$ cat /proc/cpuinfo

processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 158
model name      : Intel(R) Core(TM) i5-7500 CPU @ 3.40GHz
stepping        : 9
microcode       : 0xffffffff
cpu MHz         : 3408.000
cache size      : 256 KB
physical id     : 0 
siblings        : 4 
core id         : 0 
cpu cores       : 4  
apicid          : 0
initial apicid  : 0   
fpu             : yes
fpu_exception   : yes
cpuid level     : 6
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss 
ht tm pbe syscall nx pdpe1gb rdtscp lm pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4_1 
sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave osxsave avx f16c rdrand 
bogomips        : 6816.00 
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management: 

.....
```

### 2.2. Windows

在 CMD 命令中输入 `wmic`，然后在出现的新窗口中分别输入：
- `cpu get Name`: 查看物理 CPU 数
- `cpu get NumberOfCores`: 查看 CPU 核心数
- `cpu get NumberOfLogicalProcessors`: 查看线程数

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/21/6fa069a6de19417f5a6cb09cee1a8a80.jpg)

## 3. CPU 架构

从系统架构来看，目前商用服务器的 CPU 架构大体可以分为三类：
- 对称多处理器结构 (SMP：Symmetric Multi-Processor)
- 非一致存储访问结构 (NUMA：Non-Uniform Memory Access)
- 海量并行处理结构 (MPP：Massive Parallel Processing)

### 3.1. SMP(Symmetric Multi-Processors) / UMA(Uniform Memory Access)

**SMP（对称多处理器结构）是指服务器中多个 CPU 对称工作，无主次或从属关系**。各 CPU 共享相同的物理内存，每个 CPU 访问内存中的任何地址所需时间是相同的，因此 SMP 也被称为一致存储器访问结构 (UMA：Uniform Memory Access)。

对 SMP 服务器进行扩展的方式包括增加内存、使用更快的 CPU、增加 CPU、扩充 I/O（槽口数与总线数) 以及添加更多的外部设备（通常是磁盘存储)。

**SMP 服务器的主要特征是共享，系统中所有资源 (CPU、内存、I/O 等) 都是共享的**。也正是由于这种特征，导致了 SMP 服务器的主要问题，那就是**它的扩展能力非常有限**。对于 SMP 服务器而言，每一个共享的环节都可能造成 SMP 服务器扩展时的瓶颈，而最受限制的则是内存。由于每个 CPU 必须通过相同的内存总线访问相同的内存资源，因此随着 CPU 数量的增加，内存访问冲突将迅速增加，最终会造成 CPU 资源的浪费，使 CPU 性能的有效性大大降低。实验证明，**SMP 服务器 CPU 利用率最好的情况是 2 至 4 个 CPU**。           

### 3.3. NUMA(Non-Uniform Memory Access)

由于 SMP 在扩展能力上的限制，人们开始探究如何进行有效地扩展从而构建大型系统的技术，NUMA 就是这种努力下的结果之一。**利用 NUMA 技术，可以把几十个 CPU（甚至上百个 CPU) 组合在一个服务器内**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/18/383a9aaa37e28ff202d5fd7b5d5f9ce2.jpg)

**NUMA 服务器的基本特征是具有多个 CPU 模块，每个 CPU 模块由多个 CPU（如 4 个）组成，并且具有独立的本地内存、I/O 槽口等。由于其节点之间可以通过互联模块（如称为 Crossbar Switch）进行连接和信息交互，因此每个 CPU 可以访问整个系统的内存（这是 NUMA 系统与 MPP 系统的重要差别）**。显然，访问本地内存的速度将远远高于访问远地内存（系统内其它节点的内存）的速度，这也是非一致存储访问 NUMA 的由来。由于这个特点，为了更好地发挥系统性能，开发应用程序时需要尽量减少不同 CPU 模块之间的信息交互。利用 NUMA 技术，可以较好地解决原来 SMP 系统的扩展问题，在一个物理服务器内可以支持上百个 CPU。比较典型的 NUMA 服务器的例子包括 HP 的 Superdome、SUN15K、IBMp690 等。

**但 NUMA 技术同样有一定缺陷，由于访问远地内存的延时远远超过本地内存，因此当 CPU 数量增加时，系统性能无法线性增加**。如 HP 公司发布 Superdome 服务器时，曾公布了它与 HP 其它 UNIX 服务器的相对性能值，结果发现，64 路 CPU 的 Superdome (NUMA 结构) 的相对性能值是 20，而 8 路 N4000（共享的 SMP 结构) 的相对性能值是 6.3。从这个结果可以看到，8 倍数量的 CPU 换来的只是 3 倍性能的提升。

### 3.4. MPP(Massive Parallel Processing)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/18/eb96144523d1e2dbe021fd9fe3127264.jpg)

**和 NUMA 不同，MPP 提供了另外一种进行系统扩展的方式，它由多个 SMP 服务器通过一定的节点互联网络进行连接，协同工作，完成相同的任务，从用户的角度来看是一个服务器系统。其基本特征是由多个 SMP 服务器（每个 SMP 服务器称节点) 通过节点互联网络连接而成，每个节点只访问自己的本地资源（内存、存储等)，是一种完全无共享 (Share Nothing) 结构，因而扩展能力最好，理论上其扩展无限制，目前的技术可实现 512 个节点互联，数千个 CPU**。目前业界对节点互联网络暂无标准，如 NCR 的 Bynet，IBM 的 SPSwitch，它们都采用了不同的内部实现机制。但节点互联网仅供 MPP 服务器内部使用，对用户而言是透明的。

在 MPP 系统中，每个 SMP 节点也可以运行自己的操作系统、数据库等。但和 NUMA 不同的是，它不存在异地内存访问的问题。换言之，每个节点内的 CPU 不能访问另一个节点的内存。节点之间的信息交互是通过节点互联网络实现的，这个过程一般称为数据重分配 (Data Redistribution)。

但是 MPP 服务器需要一种复杂的机制来调度和平衡各个节点的负载和并行处理过程。目前一些基于 MPP 技术的服务器往往通过系统级软件（如数据库) 来屏蔽这种复杂性。举例来说，NCR 的 Teradata 就是基于 MPP 技术的一个关系数据库软件，基于此数据库来开发应用时，不管后台服务器由多少个节点组成，开发人员所面对的都是同一个数据库系统，而不需要考虑如何调度其中某几个节点的负载。

## 4. Refer Links

[线程数与多核 CPU 的关系](https://mp.weixin.qq.com/s?__biz=MzIzODQyNjE1NA==&mid=2247484150&idx=1&sn=9a67dc870e59b2baf4b1dc8adb2e7722&chksm=e938c3b3de4f4aa561cd4c5068c304852c63ea802792188362dfd9bfe29118c4f4d1ea030683&mpshare=1&scene=1&srcid=0818XnRIhSKLDRPKYaXqpMvy#rd)

[物理 CPU，物理核，逻辑 CPU，虚拟 CPU（vCPU）区别](https://www.jianshu.com/p/6903604cd1d4)

[服务器三大体系 SMP、NUMA、MPP 介绍](http://server.51cto.com/sCollege-198840.htm)