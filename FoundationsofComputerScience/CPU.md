- [CPU](#cpu)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [1.1. 物理 CPU](#11-%E7%89%A9%E7%90%86-cpu)
    - [1.2. cpu 物理核 (cpu cores)](#12-cpu-%E7%89%A9%E7%90%86%E6%A0%B8-cpu-cores)
    - [1.3. cpu 逻辑核 (processor)](#13-cpu-%E9%80%BB%E8%BE%91%E6%A0%B8-processor)
    - [1.4. cpu 虚拟核 (vCPU)](#14-cpu-%E8%99%9A%E6%8B%9F%E6%A0%B8-vcpu)
  - [2. 查看 CPU 参数](#2-%E6%9F%A5%E7%9C%8B-cpu-%E5%8F%82%E6%95%B0)
    - [2.1. Linux](#21-linux)
    - [2.2. Windows](#22-windows)
  - [3. Refer Links](#3-refer-links)

# CPU

## 1. 基本概念

### 1.1. 物理 CPU

物理 CPU 是相对于虚拟 CPU 而言的概念，指实际存在的处理器，即实际主板插槽上的 CPU。  

在 /proc/cpuinfo 中，有多少个不同的 physical id 就有多少个物理 CPU：
```bash
$ cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l  
2 
```

### 1.2. cpu 物理核 (cpu cores)

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

### 1.3. cpu 逻辑核 (processor)

cpu 逻辑核主要用在 Intel 超线程的环境下，将原本的一个物理核心虚拟成多个核心，从而实现单物理核心并行的调度多个线程。当有多个计算任务时，可以让其中一个计算任务使用 ALU 的时候，另一个则去使用 FPU，从而充分利用物理核中的各个部件，使得同一个物理核中也可以并行处理多个计算任务。

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

### 1.4. cpu 虚拟核 (vCPU)

虚拟 cpu 是在做虚拟化时利用虚拟化技术虚拟出来的 CPU。讨论 vCPU 离不开 VM，因此 vCPU 的讨论都是在虚拟化时候，划分 cpu 才会讨论的问题。通常一个物理 CPU 按照 1:4 ~ 1：10 的比例划分，假如我们有 4 个 8 物理核心的 CPU 按照 1:5 的比例划分，可以得到 4 x 8 x 5 = 160 vCPU。

## 2. 查看 CPU 参数

### 2.1. Linux

Linux 中查看 CPU 参数一般通过 /proc/cpuinfo 来查看。在 /proc/cpuinfo 中，具有相同 physical id 的 CPU 是同一个物理 CPU 封装的物理核，具有相同 core id 的 CPU 是同一个物理核的多个超线程（逻辑核）：
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

在 cmd 命令中输入 `wmic`，然后在出现的新窗口中分别输入：
- `cpu get Name`: 查看物理 CPU 数
- `cpu get NumberOfCores`: 查看 CPU 核心数
- `cpu get NumberOfLogicalProcessors`: 查看线程数

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/21/6fa069a6de19417f5a6cb09cee1a8a80.jpg)

## 3. Refer Links

[线程数与多核 CPU 的关系](https://mp.weixin.qq.com/s?__biz=MzIzODQyNjE1NA==&mid=2247484150&idx=1&sn=9a67dc870e59b2baf4b1dc8adb2e7722&chksm=e938c3b3de4f4aa561cd4c5068c304852c63ea802792188362dfd9bfe29118c4f4d1ea030683&mpshare=1&scene=1&srcid=0818XnRIhSKLDRPKYaXqpMvy#rd)

[物理 CPU，物理核，逻辑 CPU，虚拟 CPU（vCPU）区别](https://www.jianshu.com/p/6903604cd1d4)