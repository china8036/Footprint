- [网络扫描基础](#网络扫描基础)
	- [1. 基本概念](#1-基本概念)
	- [2. 主机发现](#2-主机发现)
		- [2.1. 基于 ICMP 的主机发现](#21-基于-icmp-的主机发现)
		- [2.2. 基于 IP 的主机发现](#22-基于-ip-的主机发现)
	- [3. 端口扫描](#3-端口扫描)
		- [3.1. TCP 全连接扫描](#31-tcp-全连接扫描)
		- [3.2. TCP SYN 扫描](#32-tcp-syn-扫描)
		- [3.3. TCP NULL / FIN / XMAS 扫描](#33-tcp-null--fin--xmas-扫描)
		- [3.4. TCP ACK 扫描](#34-tcp-ack-扫描)
		- [3.5. FTP 代理扫描](#35-ftp-代理扫描)
		- [3.6. UDP 扫描](#36-udp-扫描)
		- [3.7. 端口扫描的隐匿性策略](#37-端口扫描的隐匿性策略)
	- [4. 操作系统检测](#4-操作系统检测)
		- [4.1. 获取 Banner 信息](#41-获取-banner-信息)
		- [4.2. 利用端口信息](#42-利用端口信息)
		- [4.3. 分析 TCP/IP 协议栈指纹](#43-分析-tcpip-协议栈指纹)
			- [4.3.1. FIN 标志位探测](#431-fin-标志位探测)
			- [4.3.2. TCP ISN 取样](#432-tcp-isn-取样)
			- [4.3.3. TCP 初始化窗口大小](#433-tcp-初始化窗口大小)
			- [4.3.4. ACK 确认号的值](#434-ack-确认号的值)
			- [4.3.5. TCP 选项](#435-tcp-选项)
			- [4.3.6. ICMP 差错报告报文统计](#436-icmp-差错报告报文统计)
			- [4.3.7. ICMP 消息引用](#437-icmp-消息引用)
			- [4.3.8. BOGUS 标记检测](#438-bogus-标记检测)
			- [4.3.9. SYN 洪泛限度](#439-syn-洪泛限度)
	- [5. 漏洞扫描](#5-漏洞扫描)
		- [5.1. 基于主机的漏洞扫描](#51-基于主机的漏洞扫描)
		- [5.2. 基于网络的漏洞扫描](#52-基于网络的漏洞扫描)

# 网络扫描基础 

## 1. 基本概念

网络扫描技术是对特定目标进行各种**试探性通信**，以获取目标信息的行为。

通过各种形式的网络扫描，主要可以达到以下几种目的：

1.  主机发现：判断目标主机的工作状态，即判断目标主机是否联网并处于开机状态；
2.  端口扫描：判断目标主机的端口工作状态，即端口处于监听还是关闭状态；
3.  操作系统检测：判断目标主机的操作系统类型；
4.  漏洞扫描：判断目标主机可能存在的安全漏洞；

## 2. 主机发现

### 2.1. 基于 ICMP 的主机发现

ICMP 是 TCP/IP 体系结构中的一类重要协议，它弥补了 IP 在错误报告机制方面的缺陷，且 ICMP 还具有查询功能，一台主机可通过 ICMP 获取路由器或者另外一台主机的某些信息。

利用 ICMP 进行主机发现的最简单方法是使用 **ping** 命令，这也是使用最频繁的网络命令。ping  命令通过发送 ICMP 回送请求，根据是否收到对方主机的回答来检查与另一台主机的网络连接情况。

ping 方法是进行主机发现的一种简单易行的方法，但这种方法存在一个严重缺陷：由于大部分防火墙会对 ICMP 查询报文进行过滤，导致查询报文无法到达目标主机，因此若发送请求后没有接收到应答，无法直接推断没有与该 IP 对应的存活主机。

### 2.2. 基于 IP 的主机发现

基于 IP 进行主机发现往往可以规避防火墙的影响，此类主机发现技术向目标 IP 地址**发送特殊设计的 IP 数据包（实际上就是有意制造通信错误），诱使与目标 IP 地址对应的存活主机反馈 ICMP 差错报告报文**，暴露自己的存活状态。

相比基于 ICMP 的主机发现方法，**防火墙一般不会对 ICMP 的差错报告报文进行过滤**（否则被保护的主机会丧失 ICMP 差错报告的能力），因此，**基于 IP 的主机发现结果往往非常准确**。

- 一种较常用的方法是**发送首部异常的 IP 数据包**。TCP/IP 体系结构中，数据包中大部分字段的格式和内容都有严格要求。若将一些值随意填入数据包的首部，则接收数据包的主机在对数据包进行检查时往往会报错，即向源主机返回“参数有问题”的 ICMP 差错报告报文。

- 另一种常用的方法是**刻意制造分片时间超时**。IP 包在网络传输时，常常要经过多个不同的网络，若数据包的大小比传输途中任一网络的 MTU 大，那么数据包就需要进行分片，分片产生的多个数据包将独立送达目的主机。目的主机在处理分片时，若在规定时间内，有部分分片没有到达目的主机，将导致目的主机无法完成重组，向源主机发送“分组重组超时”的 ICMP 报文。因此，攻击者可以刻意将一个 IP 数据包分成 3 片，将第 1 片和第 2 片发往目标 IP 地址，保留第 3 片数据包不发送，一旦目标主机返回“分片重组超时”的 ICMP 报文，则可推断该主机在网络中是存活的。

## 3. 端口扫描

当确定了目标主机可达后，就可以使用端口扫描技术，发现目标主机的开放端口，包括网络协议和各种应用监听的端口。

端口扫描技术主要包括以下三类：
- 开放扫描：会产生大量的审计数据，容易被对方发现，但其可靠性高，如 TCP Connect 扫描；

- 隐蔽扫描：能有效的避免对方入侵检测系统和防火墙的检测，但这种扫描使用的数据包在通过网络时容易被丢弃从而产生错误的探测信息，如 FIN 扫描、Xmas 扫描、Null 扫描、FTP proxy 扫描； 

- 半开放扫描：隐蔽性和可靠性介于前两者之间，如 SYN 扫描。

### 3.1. TCP 全连接扫描

完成完整的 TCP 三次握手；
- 优点：真实、结果可靠；
- 结果分析
	- 若目标 port 开启：

			攻击方：发送 SYN；
			
			目标：返回 SYN + ACK；
			
			攻击方：发送 ACK；
			
			攻击方：发送 RST + ACK 结束连接；
	- 若目标 port 关闭：
			
			攻击方：发送 SYN ；
			
			目标：返回 RST + ACK 结束连接；

### 3.2. TCP SYN 扫描

只发送 SYN 数据包，接收 SYN + ACK，完成 TCP 的两次握手；
- 优点
	- 没有完整 TCP 连接，不形成会话，不会在目标主机产生日志记录；
	- 高效，速度快；
	- 准确区分端口状态：open/closed/filtered；
- 结果分析
	- 若目标 port 开启：
			
			攻击方：发送 SYN；
			
			目标：返回 SYN + ACK 或 SYN；
			
			攻击方：发送 RST；
	- 若目标 port 关闭：
			
			攻击方：发送 SYN；
			
			目标：返回 RST；
	- 若目标 port 被过滤：
			
			攻击方：发送 SYN；
			
			目标：没有响应或返回 ICMP unreachable error (type 3)；

### 3.3. TCP NULL / FIN / XMAS 扫描

TCP NULL scan，Does not set any bits (TCP flag header is 0)；

TCP FIN scan，只发送 FIN 包；

TCP XMAS scan，发送 FIN + PSH + URG；

- 比较
	
	TCP NULL, FIN, and Xmas scans 除了探测包中设置的 flags 不同，其它行为都基本相同；

- 结果分析
	- **若目标 port 开启或被过滤，则目标无任何返回**；
		- 当接收到 ICMP unreachable error (type 3, code 0, 1, 2, 3, 9, 10, or 13) 时，说明 port 被过滤；
	- 若目标 port 关闭，则目标返回 RST；
- 优点
	- 防火墙可能会 drop SYN，但不会 drop FIN / PSH / URG，实用性强；
	- 没有完成任何握手，不会在目标主机留下日志，隐蔽性强；
- 缺点
	- 并不是所有的系统遵循 RFC 793。对于大多数 Microsoft Windows、Cisco 设备、BSDI 和 IBM OS / 400，无论端口是否打开，系统都会向探针发送 RST 响应，因此**这种扫描只适合基于 Unix 的系统**；
	- 无法区分打开的端口与某些被过滤的端口；

### 3.4. TCP ACK 扫描

发送 ACK 包；
- 与其它扫描不同，ACK scan 无法用于扫描端口，而是用于映射防火墙规则集，确定某些 TCP 端口是否被 firewall 过滤，速度慢；
- 结果分析
	- 若目标 port 未被过滤：
			
			攻击方：发送 ACK；
			
			目标：返回 RST；
	- 若目标 port 被过滤：
			
			攻击方：发送 ACK；
			
			目标：无响应或返回 ICMP error messages back (type 3, code 0, 1, 2, 3, 9, 10, or 13)；

### 3.5. FTP 代理扫描

### 3.6. UDP 扫描

用于扫描 UDP 端口的状态。

- 优点：有效穿过防火墙策略；
- 缺点
	- 速度特别慢；
	- 许多主机默认限制 ICMP 返回端口不可达消息（例如，Linux 2.4.20 内核将目标不可达消息限制为每秒 1 个），结果可靠性较差；
- 结果分析
	- 若目标 port 开启：
			
			攻击方：发送 UDP 包；
			
			目标：返回 UDP 响应；
	- 若目标 port 关闭：
			
			攻击方：发送 UDP 包；
			
			目标：返回 ICMP port unreachable error (type 3, code 3)；
	- 若目标 port 被过滤：
			
			攻击方：发送 UDP 包；
			
			目标：返回 ICMP unreachable errors (type 3, codes 0, 1, 2, 9, 10, or 13)；
	- 若目标没有响应，则说明 port 为 open | filtered，可使用版本检测 (`-sV`) 进行进一步的区分；

### 3.7. 端口扫描的隐匿性策略

1. 调整扫描的顺序 -- 随机端口；
1. 减缓扫描速度；
1. 对数据包中的一些字段进行随机化赋值；
1. 利用虚假的源地址；
1. 采用分布式的方法进行扫描；
1. 分片扫描，将 TCP 控制报文分成多个短 IP 报文；

## 4. 操作系统检测

### 4.1. 获取 Banner 信息

客户端登陆服务器时，服务器往往会返回含有欢迎信息的 Banner，通过该 Banner 往往会暴露出服务程序信息，进而推断其操作系统类型。

### 4.2. 利用端口信息

端口扫描的结果在操作系统检测阶段也可进行利用。不同的操作系统通常会有一些默认开放的服务，这些服务会使用特定的端口进行网络监听。

### 4.3. 分析 TCP/IP 协议栈指纹

相对而言，对 TCP/IP 协议栈指纹进行分析是最为精确的一种检测操作系统的方法。以 Nmap、Queso 为代表的主流扫描软件采用的就是分析 TCP/IP 协议栈指纹的方法。

#### 4.3.1. FIN 标志位探测

根据 RFC 793 规定，主机上开放的端口对到达的 FIN 标志位有效的 TCP 数据包通常不响应，但 Windows、Cisco 等操作系统会返回一个 RST 数据包。

#### 4.3.2. TCP ISN 取样

ISN 是初始化序号，即 TCP 通信的任何一方在 TCP 连接建立时发出的第一个 TCP 报文的序号字段。

向目标主机多次发送 TCP 连接请求，响应手机目标系统的连接响应并对其中的 ISN 信息进行分析。

#### 4.3.3. TCP 初始化窗口大小

对 TCP 连接建立时 TCP 包首部的窗口字段进行监视，因为不少擦欧总系统的 TCP 初始化窗口的大小为常数。例如：Windows 系统的初始化窗口值为 0x402E，而 AIX 系统的初始化窗口值为 0x3F。

#### 4.3.4. ACK 确认号的值

操作系统收到一些特别设计的 TCP 数据包时，响应数据包中 ACK 确认号的值会表现出一定规律。

#### 4.3.5. TCP 选项

TCP 报文首部结构中的选项域是可选的，并不是所有操作系统都支持。

#### 4.3.6. ICMP 差错报告报文统计

一些操作系统对 ICMP 差错报告报文的发送频率进行了限制。

#### 4.3.7. ICMP 消息引用

在 ICMP 差错报告报文中，通常会引用一部分引起错误的源消息。对于一个端口不可达消息，大部分 OS 会返回 IP 请求的首部以及数据符合的前 8B，但 Solaris 系统返回的数据符合会稍微多一些，而 Linux 系统返回的更多。

#### 4.3.8. BOGUS 标记检测

在 SYN 包头中设置未定义的 TCP 标记，Linux 2.0.35 之前问的系统会在响应中保持该标记。

#### 4.3.9. SYN 洪泛限度

## 5. 漏洞扫描

扫描方法：向目标发送特定报文，根据响应判断是否存在漏洞 

### 5.1. 基于主机的漏洞扫描

- 在目标系统上安装扫描程序  
- 赋予管理员权限，以确保访问操作系统内核、系统配置文件以及系统中得各类应用程序 
- 扫描程序依据规则对系统进行分析以发现漏洞 
- 对发现的漏洞给出描述信息和补丁方法 
如：MBSA 微软基线安全分析器、360 安全卫士、QQ 电脑管家

### 5.2. 基于网络的漏洞扫描

扫描主机和被扫描的目标系统通过网络连接。

1. 利用端口扫描结果
	-	通过端口扫描和操作系统检测，掌握目标系统运行的操作系统以及开放的服务 
	- 与事先建立的漏洞库进行匹配，查看是否存在匹配的漏洞
	- 如 Windows 2003，80->IIS->查找 IIS 漏洞 ->发送探测数据包 ->根据结果判断是否存在漏洞 

2. 模拟简单的攻击活动，对目标系统进行具有攻击性的测试

基于网络的漏洞扫描组成：
1. 扫描控制台 
2. 漏洞库：核心，经常更新 
3. 扫描引擎