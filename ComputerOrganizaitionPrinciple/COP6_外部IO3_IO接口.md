
- [计算机组成原理：外部 I/O - I/O 接口](#计算机组成原理外部-IO---IO-接口)
  - [1. 并行 I/O 标准接口](#1-并行-IO-标准接口)
  - [2. 串行 I/O 标准接口](#2-串行-IO-标准接口)
  - [3. 主流接口规格与传输协议](#3-主流接口规格与传输协议)
  - [4. Refer Links](#4-Refer-Links)

# 计算机组成原理：外部 I/O - I/O 接口

## 1. 并行 I/O 标准接口

并行 I/O 标准接口 SCSI 是小型计算机系统接口的简称，其设计思想来源于 IBM 大型机系统的 I/O 通道结构，目的是使 CPU 摆脱对各种设备的繁杂控制。它是一个高速智能接口，可以混接各种磁盘、光盘、磁 带机、打印机、扫描仪、条码阅读器以及通信设备。它首先应用于 Macintosh 和 Sun 平台上，后来发展到工作站、网络服务器和 pentium 系统中，并成为 ANSI（美国国家标准局) 标准。

## 2. 串行 I/O 标准接口

串行 I/O 标准接口 IEEE1394 与 SCSI 等并行接口相比，有如下三个显著特点：

- 数据传送的高速性：1394 的数据传输率分为 100Mb/s、200Mb/s、400Mb/s 三档。而 SCSI-2 也只有 40MB/s（相当于 320Mb/s)。这样的高速 特性特别适合于新型高速硬盘及多媒体数据传送。1394 之所以达到高速，一是串行传送比并行传送容易提高数据传送时钟速率；二是采用了 DS-Link 编码 技术，把时钟信号的变化转变为选通信号的变化，即使在高的时钟速率下也不易引起信号失真。

- 数据传送的实时性：实时性可保证图像和声音不会出现时断时续的现象，因此对多媒体数据传送特别重要。1394 之所以做到实时性，原因有二：一是它除了异步传送外，还提供了一 种等步传送方式，数据以一系列的固定长度的包规整间隔地连续发送，端到端既有最大延时限制而又有最小延时限制；二是总线仲裁除优先权仲裁之外，还有均等仲 栽和紧急仲栽方式。

- 体积小易安装，连接方便：1394 使用 6 芯电缆，直径约为 6mm，插座也小。而 SCSI 使用 50 芯或 68 芯电缆，插座体积也大。 在当前个人机要连接的设备越来越多、主机箱的体积越显窄小情况下，电缆细、插座小的 1394 是很有吸引力的，尤其对笔记本电脑一类机器。1394 的电缆不 需要与电缆阻抗匹配的终端，而且电缆上的设备随时可从插座重拔出或插入， 即具有热插入能力。这对用户安装和使用 1394 设备很有利。

## 3. 主流接口规格与传输协议

[2019 年 iPhone 会不会放弃闪电（Lightning）接口并使用 USB-C？](https://www.zhihu.com/question/301893026)

[Lightning 和 USB Type-C 设计上各有什么优劣？](https://www.zhihu.com/question/24845265)

[简单说说雷电接口有哪些用途？](https://www.zhihu.com/question/35562114)

- 传输协议：

  雷电 3（Thunderbolt™3，由 Intel 发布）和 USB 3.1 是数据传输协议。不同传输标准区别主要在于传输速率。就拿 USB3.1 来说，还分为 USB3.1 Gen1 和 USB3.1 Gen2，USB3.1 Gen1 最大传输速率为 5 Gb/s，USB3.1 Gen2 最大传输速率为 10 Gb/s。

- 接口规格：

  USB Type C (USB-C) 是一种接口规格，此外还有 USB Type A (USB-A)、USB Type B (USB-B) 以及 Lightning（苹果独家）等其他接口规格。USB-C 接口在 Macbook 系列设备上被大量使用（MBP2015 及之前版本使用的是 Mini DisplayPort 接口），而 Lightning 接口在 iphone、ipad 上被广泛使用（2019Ipad 已经换成了 USB-C 接口）。

  目前的雷电 3 接口统一都是 Type-C 标准，但并不是所有的 Type-C 接口都支持雷电 3 标准。雷电 3 可以说是 USB Type-C 的最顶级呈现。

  - USB Type 系列：

    ![image](http://img.cdn.firejq.com/jpg/2019/7/12/e9931d4814c5a3967fd7e5dc4a475055.jpg)

  - Mini DisplayPort

    ![image](http://img.cdn.firejq.com/jpg/2019/7/12/bce0c2f0154371875cb860e105d83f6c.jpg)

  - Lightning

    ![image](http://img.cdn.firejq.com/jpg/2019/7/12/5013b8cae66f4343256316f9ba427e21.jpg)

## 4. Refer Links
