- [计算机网络 - 网络层：Internet 协议 - 概述](#计算机网络---网络层internet-协议---概述)
  - [1. 互联网设计原则](#1-互联网设计原则)
  - [2. Internet 结构](#2-internet-结构)
  - [3. Internet 协议](#3-internet-协议)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 网络层：Internet 协议 - 概述

## 1. 互联网设计原则

根据 RFC 1958，互联网的设计应遵循以下原则：
- 保证它能够工作
- 尽可能使它简单
- 处处明确的选择
- 尽可能做到模块化
- 期望具备异构性
- 避免使用固定不变的选择和参数
- 寻找一个好的设计，不见得是最完美的
- 发送严格，接收有一定的容忍度
- 考虑伸缩性
- 考虑性能和代价

## 2. Internet 结构

在网络层上，可将整个 Internet 看作一种相互关联的网络或自治域（自治系统）的集合。

![image](http://img.cdn.firejq.com/jpg/2018/6/11/212ab6ba7079f90e4fc14509fb241c70.jpg)

- Internet 没有真正的结构，但存在几个主要骨干网，都是由高带宽线路和快速路由器组成。这些骨干网中最大的一个称为一级网络，没个骨干网都与它连接，进而到达其它骨干网。
- 连接到骨干网上的是 Internet 服务提供商 (ISP)，它为家庭和企业、数据中心和服务器托管设施，以及区域（中级）网络提供 Internet 接入服务。
- 连接到区域网络的是更多是 ISP、许多大学和公司的局域网和其它边缘网络。

## 3. Internet 协议

将整个 Internet 整合在一起的正是网络层协议，即 Internet Protocol。

![image](http://img.cdn.firejq.com/jpg/2018/6/11/998cd85fa2d23e888b107af706a6bd4b.jpg)

>  IP is to provide a Best-Efforts (i.e., not guaranteed) way to transport datagrams (packet) from source to destination.

Internet Protocol 在设计之初就把网络互联作为目标，其任务是**提供一种尽力传送 (Best-Efforts) 的方法（不提供任何保证），将数据报从源端传送到目的端**，使用者无须考虑这些机器是否在同一个网络，也不必关心它们之间是否还有其它网络。

Internet Protocol 的通信过程：
1. 传输层获取数据流，并将数据流拆分成数据段，以便作为 IP 数据包发送。
    - 理论上，每个数据包最多可容纳 64KB，但实际上数据包通常不超过 1500 Bytes（正好可被放到一个以太网帧中）。
1. IP 路由器转发每个数据包穿过 Internet，沿着一条路径把数据包从一个路由器转发到下一个路由器，直到数据包到达目的地。
1. 当所有的数据段都到达接收方机器后，接收方机器的网络层将其重新组装还原成最初的数据报。
1. 接收方网络层将该数据报传递给传输层。

在 Internet 上存在着很多冗余连接，着意味着两个主机之间存在着许多可能的路径，而决定使用哪些路径进行数据包的传输正是 Internet Protocol 的主要任务。

## 4. Refer Links