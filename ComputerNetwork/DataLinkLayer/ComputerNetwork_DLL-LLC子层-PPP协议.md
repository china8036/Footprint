- [计算机网络 - 数据链路层 - LLC 子层：PPP 协议](#计算机网络---数据链路层---llc-子层ppp-协议)
  - [1. 基本概念](#1-基本概念)
  - [2. PPP 协议功能](#2-ppp-协议功能)
  - [3. PPP 会话建立过程](#3-ppp-会话建立过程)
  - [4. 帧格式](#4-帧格式)
  - [5. PPP 组成](#5-ppp-组成)
  - [6. PPP 认证](#6-ppp-认证)
    - [6.1. PAP](#61-pap)
    - [6.2. CHAP](#62-chap)
  - [7. Refer Links](#7-refer-links)

# 计算机网络 - 数据链路层 - LLC 子层：PPP 协议

## 1. 基本概念

大多数广域网的基础设施是以点到点的方式建设的，这类基础设施的通信主要属于以下 2 种常见情形：
- 通过广域网中的 SONET 光纤链路发送数据包，如：这些链路被广泛用于连接一个 ISP 网络中不同位置的路由器。
- 运行在 Internet 边缘的电话网络本地贿赂上的 ADSL 链路。

针对以上使用场景，都需要使用点到点的数据链路层协议，即 PPP 协议 (Point-to-Point Protocol)，PPP 协议由 RFC 1661 定义，并在 RFC 1662 种得到进一步的阐述。

SONET 和 ADSL 链路都使用了 PPP 协议，但在使用方式上有所不同。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/5fbd47a7ed3599d28604a9d39265a138.jpg)

## 2. PPP 协议功能

- 处理错误监测
- 支持多种协议（IP、 IPX、 DECnet 等）
- 连接时允许协商 IP 地址
- 允许身份认证

## 3. PPP 会话建立过程

1. 链路建立阶段
1. 可选的认证阶段 Authentication
1. 网络层协议阶段

## 4. 帧格式

HDLC 是一个早期被广泛使用的家庭协议实例，PPP 帧格式的选择酷似 HDLC 帧格式；但 PPP 和 HDLC 的主要区别在于，PPP 是面向字节 / 字符而不是面向比特的，且 PPP 使用字节填充技术，所有帧的长度是字节的整数倍。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/95d0a612cb7c1894f0797e7a1d0811c9.jpg)

## 5. PPP 组成

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/493af0b3b20cdeb3c179f9ec2fa84b2c.jpg)

LCP （Link Control Protocol）提供了建立、配置、维护和终止点对点链接的方法。

## 6. PPP 认证

PPP 两种认证协议： PAP and CHAP。

### 6.1. PAP

Passwords 以明文的形式传送。

远端节点控制重试频率和次数。

### 6.2. CHAP

使用 “secret” ，只有认证者和远端节点知道。

## 7. Refer Links
