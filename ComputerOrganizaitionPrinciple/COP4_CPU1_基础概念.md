- [计算机组成原理：CPU - 基础概念](#计算机组成原理cpu---基础概念)
  - [1. 基本概念](#1-基本概念)
    - [1.1. 主要功能](#11-主要功能)
    - [1.2. 发展历史](#12-发展历史)
  - [2. 基本组成](#2-基本组成)
    - [2.1. 主要寄存器](#21-主要寄存器)
    - [2.2. 操作控制器](#22-操作控制器)
    - [2.3. 时序产生器](#23-时序产生器)
  - [3. 运作模式](#3-运作模式)
  - [4. Refer Links](#4-refer-links)

# 计算机组成原理：CPU - 基础概念

## 1. 基本概念

### 1.1. 主要功能

### 1.2. 发展历史

## 2. 基本组成

### 2.1. 主要寄存器

<!-- ----- -->

寄存器个数，现在 Intel 的最新一代 CPU 里大概有上百个寄存器，扣除重复使用的相同空间的寄存器，寄存器的大小大概是 2KB 多，具体的寄存器如下图：

![image](http://img.cdn.firejq.com/jpg/2019/1/11/7c72e6d942e557b1551e15742f29a921.jpg)

(Registers available in the x86-64 instruction set)

<!-- ----- -->

[汇编语言里所有寄存器， 就是现代 CPU 内部中所有的寄存器吗？](https://www.zhihu.com/question/24229120)

- 指令集中包含的寄存器和 CPU 内部的寄存器有点儿不太一样。你说的那些东西是指令集里的寄存器，因为考虑程序兼容性的问题，所以这些东西一般不变，变的话也是只增不减。
  我猜测你的疑问是寄存器这么几十年没增加，性能会好么？
  对于这个问题，其实 CPU 硬件里的寄存器不只这些，尤其是 CPU 支持多级流水以及乱序发射之后，有很多内部的寄存器被流水线电路使用，但是这些寄存器不是软件可见的，所以你不用关心。
  另外你说的也不准确，从 X86 到 X64 转换的时候，不光寄存器的位数增加了，寄存器的个数也增加了，后面加了 8 个通用寄存器。
  而且还有指令集的扩展比如 SSE, SSE2 等也会引入一些新的寄存器。

- 远远不止这些啊，比如 ring 0 的代码才能访问的各种 control registers 和 debug registers，比如 SSE 对应的各种 XMM，AVX 对应的 YMM 寄存器，AVX-512 加的 ZMM 寄存器，比如 64 位模式下的 r8~r15，等等。这些都是可以在汇编里访问的，更别说各种不让你访问的。指令集 ISA 决定编译器可以看到的寄存器。

<!-- ----- -->

[阮一峰：为什么寄存器比内存快？](http://www.ruanyifeng.com/blog/2013/10/register.html)

- 距离不同

  内存离 CPU 比较远，所以要耗费更长的时间读取。距离对于桌面电脑影响很大，对于手机影响就要小得多。手机 CPU 的时钟频率比较慢（iPhone 5s 为 1.3GHz），而且手机的内存紧挨着 CPU。

- 硬件设计不同

  内存的设计相对简单，每个位就是一个电容和一个晶体管，而寄存器的设计则完全不同，多出好几个电子元件。并且通电以后，寄存器的晶体管一直有电，而内存的晶体管只有用到的才有电，没用到的就没电，这样有利于省电。这些设计上的因素，决定了寄存器比内存读取速度更快。

- 工作方式不同

  寄存器的工作方式很简单，只有两步：
  1. 找到相关的位
  1. 读取这些位

  内存的工作方式就要复杂得多：
  1. 找到数据的指针。（指针可能存放在寄存器内，所以这一步就已经包括寄存器的全部工作了。）
  1. 将指针送往内存管理单元（MMU），由 MMU 将虚拟的内存地址翻译成实际的物理地址。
  1. 将物理地址送往内存控制器（memory controller），由内存控制器找出该地址在哪一根内存插槽（bank）上。
  1. 确定数据在哪一个内存块（chunk）上，从该块读取数据。
  1. 数据先送回内存控制器，再送回 CPU，然后开始使用。

  内存的工作流程比寄存器多出许多步。每一步都会产生延迟，累积起来就使得内存比寄存器慢得多。

为了缓解寄存器与内存之间的巨大速度差异，硬件设计师做出了许多努力，包括在 CPU 内部设置缓存、优化 CPU 工作方式，尽量一次性从内存读取指令所要用到的全部数据等等。

<!-- ----- -->

[为什么 X86 的寄存器数量没有随着性能的提升而增加？](https://www.zhihu.com/question/65682145/answer/233730966)

- 从编译的角度看，寄存器压力（局部的数量不够用导致不得不访存），在寄存器多到一定程度（大概 16 到 32），就不那么大了。
- 从乱序执行硬件的角度看，现代处理器一般都应用了**寄存器重命名**技术，很多还特别优化了 reg2reg mov 操作，所以对于寄存器不够用导致的额外搬移，在这些处理器上并不会有非常大的性能损失（或者说软件在这方面的优化空间有限）。
- 另外 x86 处理器的访存单元会特别优化，像 st fwd ld 都做的比较激进，所以你用一些内存读写也不影响性能。

<!-- ----- -->

### 2.2. 操作控制器

### 2.3. 时序产生器

## 3. 运作模式

https://zh.wikipedia.org/wiki/X86#%E9%81%8B%E4%BD%9C%E6%A8%A1%E5%BC%8F

https://zh.wikipedia.org/wiki/%E7%9C%9F%E5%AF%A6%E6%A8%A1%E5%BC%8F

https://blog.csdn.net/trochiluses/article/details/8954527

https://blog.csdn.net/yang_yulei/article/details/22613327

https://www.kancloud.cn/digest/linuxnotes/161268

https://www.cnblogs.com/bethunebtj/articles/4839781.html

https://www.cnblogs.com/chengxuyuancc/archive/2013/05/12/3073738.html

- 实模式

  DOS 工作在实模式下

- 保护模式

  Windows/Linux 工作在保护模式下

- 虚拟 x86 模式

<!-- ------------------ -->

这里的运作模式只针对 x86 架构的 CPU

TODO: 看**基本组成**的内容是不是也只针对 x86，考虑独立出去

## 4. Refer Links

<!-- -------------- -->

Intel CPU 产品

![image](http://img.cdn.firejq.com/jpg/2019/1/6/5317555c1806c5055ec361952a146033.jpg)

<!-- -------------- -->

- Intel 8086

  [Wikipedia: Intel 8086](https://en.wikipedia.org/wiki/Intel_8086)

  > Intel 8086 (iAPX 86) is a 16-bit microprocessor chip designed by Intel between early 1976 and June 8, 1978, when it was released. The 8086 gave rise to the x86 architecture, which eventually became Intel's most successful line of processors.
  >
  >  On June 5, 2018, Intel released a limited edition CPU celebrating the anniversary of the Intel 8086, called the Intel Core i7-8086K.

  - 晶体管：大约 20,000 个
  - 制造工艺（制程）：3.2 μm
  - 主频：5 MHz
  - 指令集：x86-16 （迄今 Intel 8086 的指令集仍然是 PC 机与服务器的基本指令集）
  - 字长：16 bits
  - 寄存器宽度：16 bits
  - Data Bus 宽度：16 bits
  - Control Bus 宽度：16 bits
  - Inner Address Bus 宽度：16 bits
  - External Address Bus 宽度：20 bits（物理定址空间为 2^20 bit = 1 MB）

  ![image](http://img.cdn.firejq.com/jpg/2019/1/6/7535badeb63e5a14d0f7b61ca312c1a4.jpg)

  8086 有 8 个 16 bits 的寄存器，包括栈寄存器 SP 与 BP，但不包括指令寄存器 IP、控制寄存器 FLAGS 以及四个段寄存器。AX, BX, CX, DX, 这四个寄存器可以按照字节访问；但 BP, SI, DI, SP, 这四个地址寄存器只能按照 16 位宽访问。

  - 外部总线作为数据与地址的复用，降低了 CPU 的性能。8086 的后继处理器的内存访问性能大大增强了。80186 与 80286 有专门的地址计算硬件，节约了很多时间周期。80286 的外部地址总线与数据总线是分开各自专用的。
  - 8086/8088 可以连接上专用的数学协处理器以增加浮点计算性能。标准的数学协处理是 Intel 8087，执行 80 位浮点数运算。

  外设：
  - Intel 8237：直接存储器存取（DMA）控制器
  - Intel 8251：USART
  - Intel 8253：可编程间隔定时器
  - Intel 8255：可编程序外围接口
  - Intel 8259：可编程中断控制器
  - Intel 8279：键盘 / 显示控制器
  - Intel 8282/8283：8 位锁存器
  - Intel 8284：时钟发生器
  - Intel 8286/8287：双向 8 位驱动
  - Intel 8288：总线控制器
  - Intel 8289：总线仲裁器

<!-- ----- -->

TODO:

[为什么 ARM 和 MIPS 那么多寄存器，x86 那么少？](https://www.zhihu.com/question/24551779)

[关于现代 CPU，程序员应当更新的知识](https://linux.cn/article-6201-1.html)
