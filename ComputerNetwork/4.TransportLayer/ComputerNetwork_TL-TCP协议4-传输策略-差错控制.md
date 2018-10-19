- [计算机网络 - 传输层：Transport control protocol 传输策略：差错控制](#计算机网络---传输层transport-control-protocol-传输策略差错控制)
  - [1. 重传机制 (Retransmit)](#1-重传机制-retransmit)
    - [1.1. 计时器管理](#11-计时器管理)
      - [1.1.1. RFC 经典算法](#111-rfc-经典算法)
      - [1.1.2. Karn / Partridge 算法](#112-karn--partridge-算法)
      - [1.1.3. Jacobson / Karels 算法](#113-jacobson--karels-算法)
    - [1.2. 具体机制](#12-具体机制)
      - [1.2.1. 超时重传机制 (Timeout Retransmit)](#121-超时重传机制-timeout-retransmit)
      - [1.2.2. 快速重传机制 (Fast Retransmit)](#122-快速重传机制-fast-retransmit)
      - [1.2.3. 选择确认重传 (Selective Acknowledgment Retransmit)](#123-选择确认重传-selective-acknowledgment-retransmit)
      - [1.2.4. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)](#124-重复选择确认重传-duplicate-selective-acknowledgment-retransmit)
  - [2. 校验机制](#2-校验机制)
  - [3. KeepAlive 机制](#3-keepalive-机制)
    - [3.1. TCP 断连问题](#31-tcp-断连问题)
    - [3.2. 基本原理](#32-基本原理)
    - [3.3. KeepAlive 缺陷](#33-keepalive-缺陷)
    - [3.4. TCP KeepALive 与应用层心跳](#34-tcp-keepalive-与应用层心跳)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 传输层：Transport control protocol 传输策略：差错控制

## 1. 重传机制 (Retransmit)

### 1.1. 计时器管理

TCP 使用多个计时器来辅助其它工作的完成，其中**重传计时器 (RTO, Retranmission Timeout)** 用于协助实现 TCP 的重传机制。当 TCP 实体发出一个段后，它同时启动一个重传计时器，若在该计时器超时前该段被确认，则计时器停止；若该计时器超时后仍没有收到确认，则会启动针对该段的重传机制，同时重新启动该计时器。

那么存在一个关键问题是：**Timeout 时间应该设置为多长**？
- 设长了，重发就慢，没有效率，性能差。
- 设短了，会导致可能实际上没有丢包就重发，且会增加网络拥塞，导致更多的超时，更多的超时导致更多的重发。
- 不同的网络环境传输质量差异很大，因此不能将 Timeout 设置为固定的值。

因此，**RTO (Retransmission TimeOut) 的设置需要一个动态算法，它可以根据网络性能的连续测量情况，不断地调整超时间隔**。为此，TCP 引入了 RTT (Round Trip Time)，即一个数据包从发出去到回来的时间，在发送端发包时记下 t0，然后接收端再在这个 ack 回来时记下 t1，于是 RTT = t1 – t0。

#### 1.1.1. RFC 经典算法

在 [RFC 793](http://tools.ietf.org/html/rfc793) 中定义了设置 RTO 的经典算法：

1. 首先，先采样 RTT，记下最近几次的 RTT 值。

1. 然后做平滑计算求出 SRTT (Smoothed RTT)。公式为：（Exponential weighted moving average，指数加权移动平均）
    ```
    SRTT = (α * SRTT) + ((1- α) * RTT) (0.8 <= α <= 0.9)
    ```
1. 计算 RTO。公式为：
    ```
    RTO = min[UBOUND,  max[LBOUND, (β * SRTT)]] (1.3 <= β <= 2.0)
    ```
    其中：
    - UBOUND 是最大的 timeout 时间，上限值
    - LBOUND 是最小的 timeout 时间，下限值

这个算法在重传的时候会出现一个严重的问题：你是用第一次发数据的时间和 ACk 回来的时间做 RTT 样本值，还是用重传的时间和 ACK 回来的时间做 RTT 样本值？
- 情况（a）是 ack 没回来，所以重传。如果你计算第一次发送和 ACK 的时间，那么，明显算大了。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/77afb57b33741ab30554e1626e270066.jpg)

- 情况（b）是 ack 回来慢了，但是导致了重传，但刚重传不一会儿，之前 ACK 就回来了。如果你是算重传的时间和 ACK 回来的时间的差，就会算短了。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/5f9fda5c3546abed428558f5a8ae2f2a.jpg)

#### 1.1.2. Karn / Partridge 算法

针对 RFC 793 经典算法存在的问题，1987 年出现了 Karn / Partridge Algorithm，**这个算法的最大特点是忽略重传，不把重传的 RTT 做采样**。

**只要一发生重传，就对现有的 RTO 值翻倍（Exponential backoff）**。但很明显，这种死规矩对于一个需要估计比较准确的 RTT 也不靠谱。

#### 1.1.3. Jacobson / Karels 算法

使用 Exponential weighted moving average 计算 RTO 存在一个问题：如果 RTT 有一个大的波动的话，很难被发现，因为被平滑掉了。

针对这个问题，**1988 年又出现了 Jacobson / Karels Algorithm，这个算法引入了最新的 RTT 的采样和平滑过的 SRTT 的差距做因子来计算**。

Jacobson / Karels Algorithm 公式如下：（其中的 DevRTT 是 Deviation RTT 的意思）
```
计算 SRTT:
SRTT = SRTT + α (RTT – SRTT)
计算平滑 RTT 和真实的差距（加权移动平均）:
DevRTT = (1-β)*DevRTT + β*(|RTT-SRTT|)
神一样的公式：
RTO = µ * SRTT + ∂ *DevRTT
```
在 Linux 下，α = 0.125，β = 0.25， μ = 1，∂ = 4 。

**Jacobson / Karels 算法也是如今 TCP 协议中真正采用的 RTO 计算算法 (Linux 的源代码在：tcp_rtt_estimator)**。

### 1.2. 具体机制

接收端给发送端的 ACK 确认只会确认最后一个连续的包，例如，发送端发了 1,2,3,4,5 一共五份数据，接收端收到了 1，2，于是回 ack 3，然后收到了 4（注意此时 3 没收到），此时的 TCP 会怎么办？

**接收端确认的基本原则：由于 SeqNum 和 ACK 是以字节数为单位，所以 ACK 的时候，不能跳着确认，只能确认最大的连续收到的包，不然，发送端就以为之前的都收到了**。

重传机制主要需要解决以下 2 个问题：

- 什么时候重传？
- 重传哪些数据？

为解决这些问题，主要有以下几种重传机制。

#### 1.2.1. 超时重传机制 (Timeout Retransmit)

超时重传机制指的是接收端不回 ACK，一直等待 3 的到来。而发送方发出一个报文分组时，会启动一个计时器，若计时器计数完毕，ACK 还未到达，发送方就会重传 3。接收方收到 3 后，才会 ACK 4，意味着 3 收到了。

但是，**这种方式存在比较严重的问题：由于接收端一直等待 3 的到来而不回复 ACK，所以会导致 4 和 5 即便已经收到了，而发送方也完全不知道，发送方可能会悲观地认为 4 和 5 也丢了，会导致 4 和 5 的重传，浪费网络资源**。

对于发送方，有两种选择：
- 仅重传 timeout 的包，即第 3 份数据。这种方式节省了带宽，但是处理速度较慢。

- 重传 timeout 后所有的数据，即第 3，4，5 这三份数据。这种方式处理速度较快，但是浪费带宽，降低网络利用率。

**总体来说都不好，因为都在等 timeout，而 timeout 可能会很长**。

#### 1.2.2. 快速重传机制 (Fast Retransmit)

针对超时重传机制的需要持续等待 timeout 的问题，TCP 引入了 Fast Retransmit，**不以时间驱动，而以数据驱动重传**。在快速重传机制中，**如果包没有连续到达，就只 ACK 最后那个可能被丢了的包，如果发送方连续收到 3 次相同的 ACK，就马上重传**，避免了 timeout 的等待时间。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c9bcc65d5532ed212e7a832960b4dca2.jpg)

  发送方发出了 1，2，3，4，5 份数据，第一份先到送了，于是接收方 ACK 2，结果 2 因为某些原因没收到，3 到达了，于是还是 ACK 2，后面的 4 和 5 都到了，但是还是 ACK 2，因为 2 还是没有收到。于是发送端连续收到了三个 ACKnum = 2 的 ACK 包，知道了 2 已丢失，就马上重传 2。然后，接收端收到了 2，此时因为 3，4，5 都收到了，于是 直接 ACK 6。

**Fast Retransmit 解决了 timeout 的问题，但它和 Timeout Retransmit 一样，面临是重传之前的一个分段还是重传所有的问题**。对于上面的示例来说，是重传 #2 呢还是重传 #2，#3，#4，#5 呢？也许发送端发了 20 份数据，是#6，#10，#20 传来的呢。这样，发送端很有可能要重传从 2 到 20 的这堆数据（这就是某些 TCP 的实际的实现）。可见，这是一把双刃剑。

#### 1.2.3. 选择确认重传 (Selective Acknowledgment Retransmit)

**Selective Acknowledgment (SACK) 在 [RFC 2018](http://tools.ietf.org/html/rfc2018) 中定义，是针对 Fast Retransmit 的改进**。这种重传方式需要在 TCP Header 中设置 SACK 的字段，用于存放接收端已收到的分段序列号。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/38841d17cf1c5afced8395078b90b1a4.jpg)

由于接收端永远不能把 SACK 的包标记为 ACK，在发送端就可以根据回传的 SACK 来知道哪些数据到了，哪些没有到。于是就优化了 Fast Retransmit 的算法。当然，这个协议需要两边都支持。在 Linux 下，可以通过 tcp_sack 参数打开这个功能（Linux 2.4 后默认打开）。

**但这种方式存在安全问题：SACK 会消费发送方的资源，如果一个攻击者给数据发送方发一堆 SACK 的选项，这会导致发送方开始要重传甚至遍历已经发出的数据，这会消耗很多发送端的资源**。

#### 1.2.4. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)

Duplicate SACK 又称 D-SACK，在 [RFC 2883](http://www.ietf.org/rfc/rfc2883.txt) 中定义，其主要使用了 SACK 来告诉发送方有哪些数据被重复接收了。

D-SACK 使用了 SACK 的第一个段来做标志：
- 如果 SACK 的第一个段的范围被 ACK 所覆盖，那么就是 D-SACK。
- 如果 SACK 的第一个段的范围被 SACK 的第二个段覆盖，那么就是 D-SACK。

D-SACK 优势：
- 可以让发送方知道，是发出去的包丢了，还是回来的 ACK 包丢了。
- 是不是自己的 timeout 太小了，导致重传。
- 网络上出现了先发的包后到的情况（又称 reordering）。
- 网络上是不是把我的数据包给复制了。

## 2. 校验机制

TODO: 

## 3. KeepAlive 机制

### 3.1. TCP 断连问题

> 当客户端与服务器建立起正常的 TCP 连接后，如果客户主机掉线（网线断开、电源掉电、或系统崩溃等），服务端进程将永远不会知道（通过常用的 select/epoll 都监测不到断开或错误事件）。

因此，如果在 Server-Client 已建立了长连接的期间，断开客户端与服务器的物理连接，可能存在以下情况：

- 如果断开的时间短暂，在服务端 SO_KEEPALIVE 设定的探测时间间隔内，并且两端在此期间没有任何针对此长连接的网络操作，也就是说断开期间不足以让服务端“发现”客户端的连接出错了。因此，当连上网线后此 TCP 连接可以自动恢复，继续进行正常的网络操作。

- 如果断开的时间很长，超出了 SO_KEEPALIVE 设定的探测时间间隔，或者两端期间在此有了任何针对此长连接的网络操作，也就是说断开期间服务端“发现”了客户端的连接出错了，于是服务端会抛弃这个连接。因此，当连上网线时就会出现 ETIMEDOUT 或者 ECONNRESET 的错误，必须重新建立一个新的长连接进行网络操作。

### 3.2. 基本原理

由于 TCP 断连问题，在长时间无数据交互的时间段内，交互双方都有可能出现掉电、死机、异常重启等各种意外，当这些意外发生之后，这些 TCP 连接并未来得及正常释放，在软件层面上，连接的另一方并不知道对端的情况，它会一直维护这个连接，长时间的积累会导致非常多的半打开连接，造成端系统资源的消耗和浪费。

**为解决这个问题，在传输层 TCP 提供了 [KeepAlive 机制](http://www.tldp.org/HOWTO/html_single/TCP-Keepalive-HOWTO/)，主流的操作系统内核都支持了这个机制的实现，其基本原理是，隔一段时间给连接对端发送一个探测包，如果收到对方回应的 ACK，则认为连接还是存活的，在超过一定重试次数之后还是没有收到对方的回应，则丢弃该 TCP 连接**。

但 **TCP 的 KeepAlive 机制默认是不打开的，可通过修改内核参数 SO_KEEPALIVE 为 1 来开启**，并通过以下内核参数进行配置：
- `/proc/sys/net/ipv4/tcp_keepalive_time` 表示连接闲置多久开始发 keepalive 的 ACK 包。
- `/proc/sys/net/ipv4/tcp_keepalive_probes` 表示重试多少次未收到 ACK 响应才丢弃 TCP 连接。
- `/proc/sys/net/ipv4/tcp_keepalive_intvl` 表示两个 ACK 包之间间隔多长。

HTTP 应用层协议中的 KeepAlive 字段就是基于 TCP 的这个机制实现的，且几乎所有的浏览器都默认会使用 TCP 的这个机制，重用一个 TCP 连接来处理多个 HTTP 请求，然后让客户端去断链接（浏览器可能会非常贪婪，他们不到万不得已不会主动断连接）。

### 3.3. KeepAlive 缺陷

TCP KeepAlive 机制主要存在以下 2 个问题：

- TCP KeepAlive 监测的方式是发送一个 probe 包，会给网络带来额外的流量。

- TCP KeepAlive 检查不到机器断电、网线拔出、防火墙这些断线情况，只能在内核层级监测连接的存活与否，而连接的存活不一定代表服务的可用。本质上来说，KeepAlive 是用来检测长时间不活跃的连接的，所以，不适合用来及时检测连接的状态。
  
  例如，当一个服务器 CPU 进程服务器占用达到 100%，已经卡死不能响应请求了，但 TCP KeepAlive 依然会认为连接是存活的。因此 TCP KeepAlive 对于应用层程序的价值相对较小，**需要做连接保活的应用层程序（例如 QQ 等长连接需求的程序），一般需要在应用层实现自己的心跳检测和保活功能，并在获知了断线之后，可以按照服务器逻辑进行相应操作（如断线后的数据清理、重新连接等）。**。

  <!-- [既然 TCP 中有心跳检测（KeepAlive 机制）了，为什么还要在应用层做心跳检测？](https://www.cnblogs.com/1wen/p/5808276.html ) -->

  <!-- 回答：https://blog.coderzh.com/2015/03/05/WhyHeartBeatNeeded/  -->

  <!-- http://blog.csdn.net/hengyunabc/article/details/44310193 -->

  <!-- [服务端主动发送心跳包，还是客户端发送比较好？](https://www.zhihu.com/question/35896874) -->

### 3.4. TCP KeepALive 与应用层心跳

**心跳包机制一般两种：TCP 自带的 keep-alive 和应用层的心跳包探活。**那到底使用 TCP 的 keep-alive 还是应用层自己实现的心跳机制呢？

- TCP KeepALive 机制占用带宽少，不用开发人员实现。但在有代理的网络下，不一定能探活到服务器，除非反向代理也有相应的心跳机制。
- 应用层心跳能携带更多状态，但需要开发人员自己实现。

## 4. Refer Links

[TCP 协议：笔试面试知识点整理](https://hit-alibaba.github.io/interview/basic/network/TCP.html)

[TCP 的那些事儿（上）](https://coolshell.cn/articles/11564.html)

[TCP 的那些事儿（下）](https://coolshell.cn/articles/11609.html)

[网络基本功（九）：细说 TCP 重传](https://wizardforcel.gitbooks.io/network-basic/content/8.html)

[网络基本功（十）：细说 TCP 确认机制](https://wizardforcel.gitbooks.io/network-basic/content/9.html)

[阮一峰：TCP 协议简介](http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)

[TCP 连接断连问题剖析](https://www.ibm.com/developerworks/cn/aix/library/0808_zhengyong_tcp/index.html)

[网络编程释疑之：TCP 连接拔掉网线后会发生什么](http://blog.51cto.com/yaocoder/1589919)

[TCP Keepalive HOWTO](http://www.tldp.org/HOWTO/html_single/TCP-Keepalive-HOWTO/#configuringkernel)

[聊聊 TCP 中的 KeepAlive 机制](http://www.importnew.com/27624.html)