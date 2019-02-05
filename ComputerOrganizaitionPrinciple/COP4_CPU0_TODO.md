- [CPU](#cpu)
    - [1.1. CPU 基本参数](#11-cpu-基本参数)
    - [1.2. Linux 查看 CPU 参数](#12-linux-查看-cpu-参数)
    - [1.3. Windows 查看 CPU 参数](#13-windows-查看-cpu-参数)
  - [2. CPU 架构](#2-cpu-架构)
    - [2.1. SMP(Symmetric Multi-Processors) / UMA(Uniform Memory Access)](#21-smpsymmetric-multi-processors--umauniform-memory-access)
    - [2.2. NUMA(Non-Uniform Memory Access)](#22-numanon-uniform-memory-access)
    - [2.3. MPP(Massive Parallel Processing)](#23-mppmassive-parallel-processing)
  - [3. Refer Links](#3-refer-links)

# CPU

[CPU 性能天梯图](http://www.mydrivers.com/zhuanti/tianti/cpu/)

[CPU 性能对比](https://www.cpu-monkey.com/en/)

### 1.1. CPU 基本参数

- 型号

  以 Intel CPU 为例，CPU 分为高中低端，最低端的是 G 系列，其次是低端 i3 系列，然后是中端 i5 系列，高端 i7 系列和至尊 i9 系列。这个命名就和汽车一样，比如宝马 3 系，宝马 5 系，宝马 7 系。但要注意的是，Intel 的命名可不止就是 i3、i5、i7，如常见的 i7 8700K、i5 8400、i7 7700HQ、i3 8100，**i3、i5、i7 只是代表 CPU 中高低端产品线而不是代表具体产品型号**。

  ![image](http://img.cdn.firejq.com/jpg/2018/10/29/bc537d5ebd0d0ee0ec7b21507a122b0b.jpg)

  其中，i7 代表了它属于高端系列产品，7700，第一个数字 7 代表了他是第 7 代产品，后面的 700 基本就代表了他处于什么样的性能等级，后缀 K 代表着他的功率水平和特殊功能。

  Intel 更新 CPU 的时候是 i3 6100、i3 7100、i3 8100，也就是不断更新“代”，而不是不断更新 i3、i5、i7、i9、i11。什么意思？就是 intel 每次发布新的 CPU 就会 i3、i5、i7，全部发布一套，下一次升级就再发布一套新的 i3、i5、i7。

  ![image](http://img.cdn.firejq.com/jpg/2018/10/29/702497e333f9758cb7f1c6dfe7031612.jpg)

  CPU 代数（架构系列）是保证 CPU 性能最主要的因素，如果不是相隔代数过多来比较其他参数完全没有意义。例如 zen+ 架构下的 2600，和推土机架构下的 fx8300，论核心 fx8300 是八核，2600 只是六核，论主频 8300 单核心最大 4.2ghz，2600 只有 3.9ghz, 但是由于 zen+ 相比推土机架构领先太多太多，2600 甚至性能超 fx8300 两倍多。

  Intel CPU 主要系列：
  - Generation 3 / Sandy Bridge
  - Generation 4 / Ivy Bridge
  - Generation 5 / Haswell
  - Generation 6 / Broadwell / Skylake
  - Generation 7 / Kaby Lake
  - Generation 8 / Coffee Lake

  其中除了初代，从二代开始架构提升并不明显，每代平均 5% 左右，也就是说其他参数一样，3 代酷睿和 2 代酷睿差距也就 5%，4 代比 3 代强 5%。但是积小成多，每代挤牙膏，也过了 6 代了，8 代和 2 代的差距也是非常大的了，一般来说越新的架构越强。

- CPU ID

  CPU ID 是 CPU 生产厂家为识别不同类型的 CPU，而为 CPU 制订的不同的单一的代码，通常以十六进制表示。如 “0F24”（Inter 处理器）、“681H”（AMD 处理器），根据这些数字代码即可判断 CPU 属于哪种类型。

  不同厂家的 CPU，其 CPU ID 定义也是不同的：
  - Inter

    Intel CPU 的 CPU ID 一般包含四个十六进制数字，如“0F24”，从左至右分别表示 Type（类型）、Family（系列）、Mode（型号）和 Stepping（步进编号）。从 CPUID 为“068X”的处理器开始，Inter 另外加了 Brand ID（品种标识）用来辅助应用程序识别 CPU 的类型，因此根据“068X”CPUID 还不能正确判别 Pentium 和 Celerom 处理器，必须配合 Brand ID 来进行细分。
    - **CPU Type**（类型）: 类型标识，用来区别 Intel CPU 的适合应用场景（数字“1”标识所测试的微处理器是用于由用户安装的；数字“0”标识所测试的微处理器是用于由专业 PC 系统集成商、服务公司或制作商安装的）。我们通常使用的 Intel CPU 类型标识都是“0”，如“0F24”。**Intel 酷睿处理器 i3、i5、i7 指的就是 CPU Type**。
    - **CPU Family**（系列）: 产品系列标识，用来确定处理器是属于哪一代的产品。**Intel 酷睿处理器 Sandy Bridge、Ivy Bridge、Haswell 等指的就是 CPU Family**。
    - **CPU Mode**（型号）: 型号与系列通常是相互配合使用的，用以确定 CPU 是属于处理器系列中的哪一种特定类型。
    - **CPU Stepping**（步进编号）: 更新版本标识。
    - **CPU Brand_id**（品种标识）:
    - **CPU Vendor_id**（制造商标识）:

  - AMD

    AMD CPU 的 CPUID 一般包含三位十六进制数字，如“681”，从左至右分别表示为 Family（系列）、Mode（型号）和 Stepping（步进编号）。

- 主频

  在电子技术中，脉冲信号是一个按一定电压幅度，一定时间间隔连续发出的模拟信号。脉冲信号之间的时间间隔称为周期；而将在单位时间（如 1 秒）内所产生的脉冲个数称为频率。频率是描述周期性循环信号（包括脉冲信号）在单位时间内所出现的脉冲数量多少的计量名称；频率的标准计量单位是 Hz（赫）。电脑中的系统时钟就是一个典型的频率相当精确和稳定的脉冲信号发生器。

  CPU 的主频，即 CPU 内核工作的时钟频率（CPU Clock Speed）。通常所说的某某 CPU 是多少兆赫的，而这个多少兆赫就是“CPU 的主频”。**CPU 的主频表示在 CPU 内数字脉冲信号震荡的速度，与 CPU 实际的运算能力并没有直接关系**。由于主频并不直接代表运算速度，所以在一定情况下，很可能会出现主频较高的 CPU 实际运算速度较低的现象。主频的比较是建立在其他条件基本相同的情况下来讨论的，比如你手机或许 2.5GHz 的频率，而我笔记本 2.0GHz，并不代表你手机的 CPU 性能比我笔记本的还好。因为核心数、缓存、架构（最主要是它）等等参数完全不一样。所以同是 i5 4460 和 i5 4590 的时候，3.3GHz 相对于 3.2GHz 才有优势，但 0.1 的频率实际感受有多大？个人意见：几乎没有。

  提高 CPU 工作主频主要受到生产工艺的限制。由于 CPU 是在半导体硅片上制造的，在硅片上的元件之间需要导线进行联接，由于在高频状态下要求导线越细越短越好，这样才能减小导线分布电容等杂散干扰以保证 CPU 运算正确。因此制造工艺的限制，是 CPU 主频发展的最大障碍之一。

  `CPU 的主频 = 外频 x 倍频`
  - 外频

    外频是 CPU 乃至整个计算机系统的基准频率，因为外频是 CPU 与主板之间同步运行的速度，而且绝大部分电脑系统中外频也是内存与主板之间的同步运行的速度。在这种方式下，可以理解为 CPU 的外频直接与内存相连通，实现两者间的同步运行状态。

  - 倍频

    倍频即主频与外频之比的倍数。早期的 CPU 并没有“倍频”这个概念，那时主频和系统总线的速度是一样的。随着技术的发展，CPU 速度越来越快，内存、硬盘等配件逐渐跟不上 CPU 的速度了，而倍频的出现解决了这个问题，它可使内存等部件仍然工作在相对较低的系统总线频率下，而 CPU 的主频可以通过倍频来无限提升（理论上）。

    CPU 厂商基本上都会把倍频锁死，因此要超频只有从外频下手，通过倍频与外频的搭配来对主板的跳线或在 BIOS 中设置软超频，从而达到计算机总体性能的部分提升。

  - 自动降频

    CPU 温度超过一定限度后，CPU 会出于自我保护而降频。

  - 超频

    超频就是通过人为的方式将 CPU、显卡等硬件的工作频率提高（实际就是提高电压），让它们在高于其额定的频率状态下稳定工作。以 Intel P4C2.4GHz 的 CPU 为例，它的额定工作频率是 2.4GHz，如果将工作频率提高到 2.6GHz，系统仍然可以稳定运行，那这次超频就成功了。

- 核心数

  一般来说，相同架构下，四核心四线程的 CPU 性能大约是双核双线程的 1.8 倍，当然这只是理论上，不过一般四核心主频更高缓存更高，性能表现为 2 倍。这里我们说一下超线程技术，四核四线程和四核八线程有什么区别，超线程是一种技术而非物理元器件，它和核心不同，这种技术使得一个核心模拟两个虚拟核心运作，提高单个运行效率，一般我们认为超线程的提升是 25% 到 40%，这和架构也有关系，也就是说在其他一样的情况下四核八线程比四核四线程强 30%。但是四核八线程只是四核心，相比六核六线程它就更差一点了。

- 制程工艺

  常见的 45nm，28nm 都是说制程的。CPU 就是几亿个负责开与关的晶体管，而在面积相同的一个 CPU 上，当然是塞进去的晶体管越多其性能越好。怎样放的更多呢？那就是降低晶体管的大小。同样数量不同大小的晶体管，更小的制程意味着更低的功耗和发热。

  处理器发展经过了几十年，130nm、65nm、45nm、32nm、22nm、14nm、12nm 等等，工艺越先进，等大小的硅晶片上能集成的晶体管数量越多，CPU 性能就越好，功耗发热处理也更好。

- 热设计功耗 (TDP)

- 接口

  常见的接口有 1155，1150，FM2，这些只是接口的名字，就像 USB、VGA、HDMI 一样，Intel 的接口通常数字代表了其针角的数量。而数字前面一般还有 LGA，BGA，Socket 等字样，这代表的是其封装方式，这和接口一样，并没有孰优孰劣，只是平台不同。

- 缓存

  一般来说 CPU 的缓存分为 L1、L2、L3，缓存是因为内存的速度太慢，拖累处理器，处理器把即将要用的数据从内存调用到高速缓存中，先看 L1，CPU 有 80% 概率从 L1 缓存中获取数据，如果不行那就 L2，最后实在不行那就 L3，缓存的速度非常块一般来说相当于 CPU 的频率，锐龙现在单核心比不过酷睿也和缓存延迟大有关系。

- 指令集

  CPU 所支持的指令集，其实就是一个任务表，比如任务表里有 ABCDE 这 5 个选项，指令集发送 A，那么 A 所对应的 1、2、3、4、5 步操作就由 CPU 来完成。

  指令集越新效率越高，越多性能越强，与软件契合度越高。如 G4600 主频核心缓存都和 i3 6100 接近，但是它缺少部分指令集，性能就稍微弱一点。

- 字长

  i.e. 16 位 CPU / 字长为 16 位：
  - 运算器一次最多可以处理 16 位的数据。字长越大，计算精度越高。
  - 寄存器的最大宽度为 16 位。
  - 寄存器和运算器之间的传输通路为 16 位。

- 其它
  - CPU 的性能还和主板支持的技术有关例如 amd 的 Precision Boost 2，AMD 的 400 系主板的 bios 会采用更激进的电压和温控策略对 cpu 在多核使用情况下进行睿频，甚至在某些情况下全核都能达到单核心最大的频率，目前也只有 X470 支持。

eg:

i3-8100 参数说明：
- CPU 系列	  酷睿 i3 8 代系列
- 制作工艺	14 纳米
- 核心代号	Coffee Lake
- 插槽类型	LGA 1151
- CPU 主频	  3.6GHz
- 核心数量	四核心
- 线程数量	四线程
- 三级缓存	6MB
- 热设计功耗 (TDP)	65W
- 内存类型	DDR4 2400MHz
- 集成显卡	Intel UHD Graphics 630
- 超线程技术	不支持

### 1.2. Linux 查看 CPU 参数

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

### 1.3. Windows 查看 CPU 参数

在 CMD 命令中输入 `wmic`，然后在出现的新窗口中分别输入：
- `cpu get Name`: 查看物理 CPU 数
- `cpu get NumberOfCores`: 查看 CPU 核心数
- `cpu get NumberOfLogicalProcessors`: 查看线程数

![image](http://img.cdn.firejq.com/jpg/2018/8/21/6fa069a6de19417f5a6cb09cee1a8a80.jpg)

## 2. CPU 架构

从系统架构来看，目前商用服务器的 CPU 架构大体可以分为三类：
- 对称多处理器结构 (SMP：Symmetric Multi-Processor)
- 非一致存储访问结构 (NUMA：Non-Uniform Memory Access)
- 海量并行处理结构 (MPP：Massive Parallel Processing)

### 2.1. SMP(Symmetric Multi-Processors) / UMA(Uniform Memory Access)

**SMP（对称多处理器结构）是指服务器中多个 CPU 对称工作，无主次或从属关系**。各 CPU 共享相同的物理内存，每个 CPU 访问内存中的任何地址所需时间是相同的，因此 SMP 也被称为一致存储器访问结构 (UMA：Uniform Memory Access)。

对 SMP 服务器进行扩展的方式包括增加内存、使用更快的 CPU、增加 CPU、扩充 I/O（槽口数与总线数) 以及添加更多的外部设备（通常是磁盘存储)。

**SMP 服务器的主要特征是共享，系统中所有资源 (CPU、内存、I/O 等) 都是共享的**。也正是由于这种特征，导致了 SMP 服务器的主要问题，那就是**它的扩展能力非常有限**。对于 SMP 服务器而言，每一个共享的环节都可能造成 SMP 服务器扩展时的瓶颈，而最受限制的则是内存。由于每个 CPU 必须通过相同的内存总线访问相同的内存资源，因此随着 CPU 数量的增加，内存访问冲突将迅速增加，最终会造成 CPU 资源的浪费，使 CPU 性能的有效性大大降低。实验证明，**SMP 服务器 CPU 利用率最好的情况是 2 至 4 个 CPU**。

### 2.2. NUMA(Non-Uniform Memory Access)

由于 SMP 在扩展能力上的限制，人们开始探究如何进行有效地扩展从而构建大型系统的技术，NUMA 就是这种努力下的结果之一。**利用 NUMA 技术，可以把几十个 CPU（甚至上百个 CPU) 组合在一个服务器内**。

![image](http://img.cdn.firejq.com/jpg/2018/9/18/383a9aaa37e28ff202d5fd7b5d5f9ce2.jpg)

**NUMA 服务器的基本特征是具有多个 CPU 模块，每个 CPU 模块由多个 CPU（如 4 个）组成，并且具有独立的本地内存、I/O 槽口等。由于其节点之间可以通过互联模块（如称为 Crossbar Switch）进行连接和信息交互，因此每个 CPU 可以访问整个系统的内存（这是 NUMA 系统与 MPP 系统的重要差别）**。显然，访问本地内存的速度将远远高于访问远地内存（系统内其它节点的内存）的速度，这也是非一致存储访问 NUMA 的由来。由于这个特点，为了更好地发挥系统性能，开发应用程序时需要尽量减少不同 CPU 模块之间的信息交互。利用 NUMA 技术，可以较好地解决原来 SMP 系统的扩展问题，在一个物理服务器内可以支持上百个 CPU。比较典型的 NUMA 服务器的例子包括 HP 的 Superdome、SUN15K、IBMp690 等。

**但 NUMA 技术同样有一定缺陷，由于访问远地内存的延时远远超过本地内存，因此当 CPU 数量增加时，系统性能无法线性增加**。如 HP 公司发布 Superdome 服务器时，曾公布了它与 HP 其它 UNIX 服务器的相对性能值，结果发现，64 路 CPU 的 Superdome (NUMA 结构) 的相对性能值是 20，而 8 路 N4000（共享的 SMP 结构) 的相对性能值是 6.3。从这个结果可以看到，8 倍数量的 CPU 换来的只是 3 倍性能的提升。

### 2.3. MPP(Massive Parallel Processing)

![image](http://img.cdn.firejq.com/jpg/2018/9/18/eb96144523d1e2dbe021fd9fe3127264.jpg)

**和 NUMA 不同，MPP 提供了另外一种进行系统扩展的方式，它由多个 SMP 服务器通过一定的节点互联网络进行连接，协同工作，完成相同的任务，从用户的角度来看是一个服务器系统。其基本特征是由多个 SMP 服务器（每个 SMP 服务器称节点) 通过节点互联网络连接而成，每个节点只访问自己的本地资源（内存、存储等)，是一种完全无共享 (Share Nothing) 结构，因而扩展能力最好，理论上其扩展无限制，目前的技术可实现 512 个节点互联，数千个 CPU**。目前业界对节点互联网络暂无标准，如 NCR 的 Bynet，IBM 的 SPSwitch，它们都采用了不同的内部实现机制。但节点互联网仅供 MPP 服务器内部使用，对用户而言是透明的。

在 MPP 系统中，每个 SMP 节点也可以运行自己的操作系统、数据库等。但和 NUMA 不同的是，它不存在异地内存访问的问题。换言之，每个节点内的 CPU 不能访问另一个节点的内存。节点之间的信息交互是通过节点互联网络实现的，这个过程一般称为数据重分配 (Data Redistribution)。

但是 MPP 服务器需要一种复杂的机制来调度和平衡各个节点的负载和并行处理过程。目前一些基于 MPP 技术的服务器往往通过系统级软件（如数据库) 来屏蔽这种复杂性。举例来说，NCR 的 Teradata 就是基于 MPP 技术的一个关系数据库软件，基于此数据库来开发应用时，不管后台服务器由多少个节点组成，开发人员所面对的都是同一个数据库系统，而不需要考虑如何调度其中某几个节点的负载。

## 3. Refer Links

[线程数与多核 CPU 的关系](https://mp.weixin.qq.com/s?__biz=MzIzODQyNjE1NA==&mid=2247484150&idx=1&sn=9a67dc870e59b2baf4b1dc8adb2e7722&chksm=e938c3b3de4f4aa561cd4c5068c304852c63ea802792188362dfd9bfe29118c4f4d1ea030683&mpshare=1&scene=1&srcid=0818XnRIhSKLDRPKYaXqpMvy#rd)

[物理 CPU，物理核，逻辑 CPU，虚拟 CPU（vCPU）区别](https://www.jianshu.com/p/6903604cd1d4)

[服务器三大体系 SMP、NUMA、MPP 介绍](http://server.51cto.com/sCollege-198840.htm)

[i7 真的就比 i5 好吗，我们一定要 i7 吗](https://zhuanlan.zhihu.com/p/37798889)

[如何看懂电脑参数——CPU 篇](https://zhuanlan.zhihu.com/p/20495438)

[百度百科：CPU 主频](https://baike.baidu.com/item/%E4%B8%BB%E9%A2%91/103191?fromtitle=CPU%E4%B8%BB%E9%A2%91&fromid=3519849)

[CPU Family, Model, Stepping 以及 CPU ID, DisplayFamily_DisplayModel](http://blog.sina.com.cn/s/blog_605f5b4f010180st.html)