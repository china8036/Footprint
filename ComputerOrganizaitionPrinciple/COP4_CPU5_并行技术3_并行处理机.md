- [计算机组成原理：CPU - 并行技术：并行处理机](#计算机组成原理cpu---并行技术并行处理机)
  - [1. 超标量处理机 / 超长指令字处理机](#1-超标量处理机--超长指令字处理机)
  - [2. 多线程 / 超线程处理机](#2-多线程--超线程处理机)
    - [2.1. 多线程技术](#21-多线程技术)
    - [2.2. 超线程技术](#22-超线程技术)
  - [3. 多处理机](#3-多处理机)
    - [3.1. 并行向量处理机 (PVP: Parallel Vector Processor)](#31-并行向量处理机-pvp-parallel-vector-processor)
    - [3.2. 对称多处理机结构 (SMP：Symmetric Multi-Processor) / UMA(Uniform Memory Access)](#32-对称多处理机结构-smpsymmetric-multi-processor--umauniform-memory-access)
    - [3.3. 非一致存储访问处理机 (NUMA：Non-Uniform Memory Access)](#33-非一致存储访问处理机-numanon-uniform-memory-access)
    - [3.4. 海量并行处理机 (MPP：Massive Parallel Processing)](#34-海量并行处理机-mppmassive-parallel-processing)
    - [3.5. 分布共享存储器多处理机 (DSM: Distributed Shared Memory)](#35-分布共享存储器多处理机-dsm-distributed-shared-memory)
  - [4. 多核处理机 / 片上多处理机](#4-多核处理机--片上多处理机)
    - [4.1. 基本概念](#41-基本概念)
    - [4.2. 同构多核处理机 (homoghenous multi-core)](#42-同构多核处理机-homoghenous-multi-core)
    - [4.3. 异构多核处理机 (heterogeneous multi-core)](#43-异构多核处理机-heterogeneous-multi-core)
  - [5. 相关概念辨析](#5-相关概念辨析)
    - [5.1. 物理 CPU](#51-物理-cpu)
    - [5.2. CPU 物理核 (cpu cores)](#52-cpu-物理核-cpu-cores)
    - [5.3. CPU 逻辑核 (processor)](#53-cpu-逻辑核-processor)
    - [5.4. CPU 虚拟核 (vCPU)](#54-cpu-虚拟核-vcpu)
  - [6. 并行处理机参数](#6-并行处理机参数)
    - [6.1. Linux](#61-linux)
    - [6.2. Windows](#62-windows)
  - [7. Refer Links](#7-refer-links)

# 计算机组成原理：CPU - 并行技术：并行处理机

1966 年，M.J.Flynn 从计算机体系结构的并行性出发，按照指令流和数据流的不同组织方式，把计算机系统结构分为如下 4 种类型：
- 单指令流单数据流 (SISD): 其代表机型是单处理机。
- 单指令流多数据流 (SIMD): 其代表机型是向量处理机。
- 多指令流单数据流 (MISD): 这种结构从来没有实现过。
- 多指令流多数据流 (MIMD): 其代表机型是多处理机和机群系统。前者为紧耦合系统，后者为松耦合系统。

计算机体系结构可以采用不同方式的并行机制，因此有了以下几种组织结构的并行处理机。

## 1. 超标量处理机 / 超长指令字处理机

在计算机系统的最底层，流水线技术将时间并行性引入处理机，而多发射处理机则把空间并行性引入处理机。

超标量 (superscalar) 设计采用多发射技术，在处理机内部设置多条并行执行的指令流水线，通过在每个时钟周期内向执行单元发射多条指令实现指令级并行。超长指令字技术 VLIW (very long instruction word) 则由编译器在编译时找出指令间潜在的并行性，进行适当的调度安排，把多个能够并行执行的操作组合在一起，控制处理机中的多个相互独立的功能部件，相当于同时执行多条指令，从而提高处理机的并行性。

## 2. 多线程 / 超线程处理机

### 2.1. 多线程技术

多线程技术允许 CPU 同时运行多个硬件线程，如果某个线程被迫暂停，其它线程仍能运行，这样能保证硬件资源被充分利用，是提高处理机并行度的有效手段。

### 2.2. 超线程技术

2002年，Intel公司推出采用超线程技术的Pentium 4 CPU。超线程技术是同时多线程技术在Intel CPU中的具体实现，**在经过特殊设计的CPU中，原有的单个物理核经过简单扩展后被模拟成为2个逻辑内核，并能够同时执行2个相互独立的程序，从而充分利用了CPU的执行资源**。

超线程技术是通过隐藏潜在访问存储器延迟的方法来提高处理机性能的，其主要目的是充分利用空闲的处理机资源，本质上仍然是多个线程共享一个处理机核。因此，采用超线程技术是否能获得性能的提升取决于应用程序以及硬件平台。

## 3. 多处理机

在单个处理机性能一定的情况下，进一步提高计算机系统处理能力的简单方法就是让多个处理机协同工作，共同完成任务。**多处理机系统由多个独立的处理机组成，每个处理机能够独立执行自己的程序**。

从系统架构来看，现有的多处理机系统主要有以下类型：

### 3.1. 并行向量处理机 (PVP: Parallel Vector Processor)

### 3.2. 对称多处理机结构 (SMP：Symmetric Multi-Processor) / UMA(Uniform Memory Access)
  **SMP（对称多处理器结构）是指服务器中多个 CPU 对称工作，无主次或从属关系**。各 CPU 共享相同的物理内存，每个 CPU 访问内存中的任何地址所需时间是相同的，因此 SMP 也被称为一致存储器访问结构 (UMA：Uniform Memory Access)。
  对 SMP 服务器进行扩展的方式包括增加内存、使用更快的 CPU、增加 CPU、扩充 I/O（槽口数与总线数) 以及添加更多的外部设备（通常是磁盘存储)。
  **SMP 服务器的主要特征是共享，系统中所有资源 (CPU、内存、I/O 等) 都是共享的**。也正是由于这种特征，导致了 SMP 服务器的主要问题，那就是**它的扩展能力非常有限**。对于 SMP 服务器而言，每一个共享的环节都可能造成 SMP 服务器扩展时的瓶颈，而最受限制的则是内存。由于每个 CPU 必须通过相同的内存总线访问相同的内存资源，因此随着 CPU 数量的增加，内存访问冲突将迅速增加，最终会造成 CPU 资源的浪费，使 CPU 性能的有效性大大降低。实验证明，**SMP 服务器 CPU 利用率最好的情况是 2 至 4 个 CPU**。

### 3.3. 非一致存储访问处理机 (NUMA：Non-Uniform Memory Access)

由于 SMP 在扩展能力上的限制，人们开始探究如何进行有效地扩展从而构建大型系统的技术，NUMA 就是这种努力下的结果之一。**利用 NUMA 技术，可以把几十个 CPU（甚至上百个 CPU) 组合在一个服务器内**。

![image](http://img.cdn.firejq.com/jpg/2018/9/18/383a9aaa37e28ff202d5fd7b5d5f9ce2.jpg)

**NUMA 服务器的基本特征是具有多个 CPU 模块，每个 CPU 模块由多个 CPU（如 4 个）组成，并且具有独立的本地内存、I/O 槽口等。由于其节点之间可以通过互联模块（如称为 Crossbar Switch）进行连接和信息交互，因此每个 CPU 可以访问整个系统的内存（这是 NUMA 系统与 MPP 系统的重要差别）**。显然，访问本地内存的速度将远远高于访问远地内存（系统内其它节点的内存）的速度，这也是非一致存储访问 NUMA 的由来。由于这个特点，为了更好地发挥系统性能，开发应用程序时需要尽量减少不同 CPU 模块之间的信息交互。利用 NUMA 技术，可以较好地解决原来 SMP 系统的扩展问题，在一个物理服务器内可以支持上百个 CPU。比较典型的 NUMA 服务器的例子包括 HP 的 Superdome、SUN15K、IBMp690 等。

**但 NUMA 技术同样有一定缺陷，由于访问远地内存的延时远远超过本地内存，因此当 CPU 数量增加时，系统性能无法线性增加**。如 HP 公司发布 Superdome 服务器时，曾公布了它与 HP 其它 UNIX 服务器的相对性能值，结果发现，64 路 CPU 的 Superdome (NUMA 结构) 的相对性能值是 20，而 8 路 N4000（共享的 SMP 结构) 的相对性能值是 6.3。从这个结果可以看到，8 倍数量的 CPU 换来的只是 3 倍性能的提升。

### 3.4. 海量并行处理机 (MPP：Massive Parallel Processing)

![image](http://img.cdn.firejq.com/jpg/2018/9/18/eb96144523d1e2dbe021fd9fe3127264.jpg)

**和 NUMA 不同，MPP 提供了另外一种进行系统扩展的方式，它由多个 SMP 服务器通过一定的节点互联网络进行连接，协同工作，完成相同的任务，从用户的角度来看是一个服务器系统。其基本特征是由多个 SMP 服务器（每个 SMP 服务器称节点) 通过节点互联网络连接而成，每个节点只访问自己的本地资源（内存、存储等)，是一种完全无共享 (Share Nothing) 结构，因而扩展能力最好，理论上其扩展无限制，目前的技术可实现 512 个节点互联，数千个 CPU**。目前业界对节点互联网络暂无标准，如 NCR 的 Bynet，IBM 的 SPSwitch，它们都采用了不同的内部实现机制。但节点互联网仅供 MPP 服务器内部使用，对用户而言是透明的。

在 MPP 系统中，每个 SMP 节点也可以运行自己的操作系统、数据库等。但和 NUMA 不同的是，它不存在异地内存访问的问题。换言之，每个节点内的 CPU 不能访问另一个节点的内存。节点之间的信息交互是通过节点互联网络实现的，这个过程一般称为数据重分配 (Data Redistribution)。

但是 MPP 服务器需要一种复杂的机制来调度和平衡各个节点的负载和并行处理过程。目前一些基于 MPP 技术的服务器往往通过系统级软件（如数据库) 来屏蔽这种复杂性。举例来说，NCR 的 Teradata 就是基于 MPP 技术的一个关系数据库软件，基于此数据库来开发应用时，不管后台服务器由多少个节点组成，开发人员所面对的都是同一个数据库系统，而不需要考虑如何调度其中某几个节点的负载。

### 3.5. 分布共享存储器多处理机 (DSM: Distributed Shared Memory)

## 4. 多核处理机 / 片上多处理机

### 4.1. 基本概念

由于单核技术面临继续发展的瓶颈，另一方面由于大规模集成电路技术的发展使单芯片容量增长到足够大，能够把原来大规模并行处理机结构中的多处理机和多计算机节点集成到同一个芯片中，让各个处理机核实现片内并行运行，多核技术开始兴起。

多核技术 (multicore) 通过开发程序内的线程级或进程级并行性提高性能，多核处理机是指在一颗处理机芯片内集成两个或两个以上完整且并行工作的计算引擎（核），也称为片上多处理机 (CMP. chip multi-processor)。核 / 核心 (core) 是指包含指令部件、算术 / 逻辑部件、寄存器堆和一级或两级 cache 的处理单元，这些核通过某种方式互联后，能够相互交换数据，对外呈现为一个统一的多核处理机。从这个角度上看，**多核处理机是一种特殊的多处理机架构**。

多核处理机的优势：
- 高并行性。
- 高通信效率。多个核之间通过高速的通信接口进行连接。
- 高资源利用率。
- 低功耗。不同于传统的多处理机架构中多个处理机需要分布于不同的主板上，多核处理机中各个核心可以构建在同一块主板上，因此大量减少了热量消耗。
- 低设计复杂度。
- 较低的成本。

这些优势最终推动了多核的发展并使多核逐渐取代单核处理机成为主流的技术。

### 4.2. 同构多核处理机 (homoghenous multi-core)

同构多核处理机 (homoghenous multi-core) 内的所有计算内核结构相同，地位对等。

同构多核处理机大多由通用的处理机核心构成，每个处理机核心可以独立执行任务，其结构与通用单核处理机结构相近。同构多核处理机的各个核心之间可以通过共享存储器互连，也可以通过cache或局部存储器互连。在Intel公司的通用桌面计算机上的多核处理机通常采用同构多核结构。

### 4.3. 异构多核处理机 (heterogeneous multi-core)

异构多核处理机 (heterogeneous multi-core) 内的各个计算内核结构不同，地位不对等。

异构多核处理机根据不同应用程序的需求配置不同的处理机核心，一般多采用“主处理核+协处理核”的主从架构。

异构多核处理机的优势在于可以同时发挥不同类型处理机各自的长处来满足不同种类的应用的性能和功耗需求。研究表明，异构组织方式比同构的多核处理机执行任务更有效率，实现了资源的最佳化配置，而降低了系统的整体功耗。

## 5. 相关概念辨析

### 5.1. 物理 CPU

物理 CPU 是相对于虚拟 CPU 而言的概念，指实际存在的处理器，即实际主板插槽上的 CPU。

在 /proc/cpuinfo 中，有多少个不同的 physical id 就有多少个物理 CPU：
```bash
$ cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l
2
```

### 5.2. CPU 物理核 (cpu cores)

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

### 5.3. CPU 逻辑核 (processor)

**CPU 逻辑核主要用在 Intel 超线程的环境下**，将原本的一个物理核心虚拟成多个核心，从而实现单物理核心并行的调度多个线程。当有多个计算任务时，可以让其中一个计算任务使用 ALU 的时候，另一个则去使用 FPU，从而充分利用物理核中的各个部件，使得同一个物理核中也可以并行处理多个计算任务。

因此在开启超线程时，`CPU 线程数 = 总逻辑 CPU 数 = 物理 CPU 个数 × 每颗物理 CPU 的核数 × 超线程数`。

eg: Intel i5 4200H CPU

![image](http://img.cdn.firejq.com/jpg/2018/8/21/497195ab2d2c12bf24c24b26c06bfb64.jpg)

即表示有 2 个物理核心，使用 HT 超线程数为 2，使线程数为 2 x 2 = 4。

而 AMD CPU 没有超线程技术，物理核心数就等于 CPU 线程数，也因此对于 AMD 的 CPU 来说，只有核心数的概念，没有线程数的概念。

在 /proc/cpuinfo 中，processor 指的就是 CPU 逻辑核数:
```bash
$ cat /proc/cpuinfo | grep "processor" | wc -l

  24 # 2 个物理 CPU，每个 CPU 有 6 个 core，同时支持超线程，因此共 24 个逻辑 CPU
```
使用 top 查看负载时，按 1，看到的 CPU0~CPUn 也是 processor number。在 Windows 系统下看到的 CPU 数量实际上也是逻辑 CPU 数量。

VM 虚拟机中的 CPU 选择的核心数实际上指的是 CPU 逻辑核。

在设置线程数时，对于计算密集型的任务，一般建议将线程数设置为物理核数而不是逻辑核数。因为计算密集任务一般对各个计算部件（FPU\ALU）的使用并不是均匀的，一般 ALU 的使用占大头，FPU 的使用只占小部分，所以超线程技术并不能带来很大的并行度提升。而这一点点提升，也被线程切换带来的消耗所抵消了。最终导致“超线程”技术，并没有像理论中的那样加大并行度，从而提高吞吐量。

### 5.4. CPU 虚拟核 (vCPU)

虚拟 cpu 是在做虚拟化时利用虚拟化技术虚拟出来的 CPU。讨论 vCPU 离不开 VM，因此 vCPU 的讨论都是在虚拟化时候，划分 cpu 才会讨论的问题。通常一个物理 CPU 按照 1:4 ~ 1：10 的比例划分，假如我们有 4 个 8 物理核心的 CPU 按照 1:5 的比例划分，可以得到 4 x 8 x 5 = 160 vCPU。

## 6. 并行处理机参数

### 6.1. Linux

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

### 6.2. Windows

在 CMD 命令中输入 `wmic`，然后在出现的新窗口中分别输入：
- `cpu get Name`: 查看物理 CPU 数
- `cpu get NumberOfCores`: 查看 CPU 核心数
- `cpu get NumberOfLogicalProcessors`: 查看线程数

![image](http://img.cdn.firejq.com/jpg/2018/8/21/6fa069a6de19417f5a6cb09cee1a8a80.jpg)


## 7. Refer Links

[线程数与多核 CPU 的关系](https://mp.weixin.qq.com/s?__biz=MzIzODQyNjE1NA==&mid=2247484150&idx=1&sn=9a67dc870e59b2baf4b1dc8adb2e7722&chksm=e938c3b3de4f4aa561cd4c5068c304852c63ea802792188362dfd9bfe29118c4f4d1ea030683&mpshare=1&scene=1&srcid=0818XnRIhSKLDRPKYaXqpMvy#rd)

[物理 CPU，物理核，逻辑 CPU，虚拟 CPU（vCPU）区别](https://www.jianshu.com/p/6903604cd1d4)

[服务器三大体系 SMP、NUMA、MPP 介绍](http://server.51cto.com/sCollege-198840.htm)
