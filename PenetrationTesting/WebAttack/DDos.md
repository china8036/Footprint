  - [DDos](#ddos)
  - [Kill Packet Dos](#kill-packet-dos)
    - [Teardrop](#teardrop)
      - [小片段攻击](#%E5%B0%8F%E7%89%87%E6%AE%B5%E6%94%BB%E5%87%BB)
      - [重叠分片攻击](#%E9%87%8D%E5%8F%A0%E5%88%86%E7%89%87%E6%94%BB%E5%87%BB)
  - [Ping of Death](#ping-of-death)
    - [Land Attack](#land-attack)
    - [Oscillate Attack](#oscillate-attack)
  - [Flood Type Dos](#flood-type-dos)
    - [Directed Flood Type Dos](#directed-flood-type-dos)
      - [Ping Dos](#ping-dos)
      - [TCP SYN Dos](#tcp-syn-dos)
      - [TCP Handshake Dos](#tcp-handshake-dos)
      - [HTTP Flood](#http-flood)
    - [Reflected Flood Type Dos](#reflected-flood-type-dos)
      - [Smurf Attack](#smurf-attack)
      - [Fraggle Attack](#fraggle-attack)
  - [其它应用](#%E5%85%B6%E5%AE%83%E5%BA%94%E7%94%A8)
      - [DHCP Dos](#dhcp-dos)
      - [ARP 毒化 Dos](#arp-%E6%AF%92%E5%8C%96-dos)

## DDos 

拒绝服务攻击 Denial of Service (Dos) 是一种行之有效且难以防范的攻击手段，主要通过消耗网络带宽或系统资源（如处理器、磁盘、内存等）导致网络或系统瘫痪而停止提供正常的网络服务器或使服务质量显著降低；有的 Dos 通过修改系统配置使系统无法正常工作（如更改路由表）。

分布式拒绝服务攻击 Distributed Denial of Service (	DDos) 是 Dos 最主要的一种形式。

按照攻击机制分类，Dos 可分为 Kill Packet Dos 与 Flood Type Dos；

## Kill Packet Dos 

Kill Packet Dos 也称为 Vulnerability/Protocol Attack，主要利用协议本身或软件实现中的漏洞，向目标发送畸形的数据包，使目标系统在处理数据包时出现异常，甚至崩溃。

此类攻击对攻击者的计算机能力和网络带宽没有什么要求，也就是说一台普通的 PC 机足以攻破一台大型机。

### Teardrop Attack

Teardrop 即碎片 / 泪滴攻击，是利用 Windows 3.1、95、NT 和 低版本的 Linux 中处理 IP 分片时的漏洞，向目标主机发送**分片偏移地址异常**的 **UDP** 数据包分片，使得目标主机在重组分片时出现异常而崩溃或重启。

碎片攻击的变种有很多，如小片段攻击、重叠分片攻击，但这两类攻击都是以穿透防火墙为目的，而不是以 Dos 为目的。

#### 小片段 Attack

通过很小的片段使防火墙需要检测的信息进入到下一个片段中，达到穿透防火墙的目的。

#### 重叠分片 Attack

利用防火墙在冲力重叠分片时的策略与目标主机处理重叠分片方法间可能存在的差异（当收到重叠分片时，有些系统以先到的数据包为主，有些系统以后到的数据包覆盖先到的数据包），当防火墙重组数据时与目标主机不同，就可达到穿透防火墙的目的。

## Ping of Death 

利用 ICMP 协议的漏洞，向目标主机发送超长的 ICMP 包，导致目标主机死机、重启、崩溃。

根据 RFC791 规定：

> IP Packet 最大长度不可超过 64kB 即 65535B，因此减去 IP 头部后，IP Packet 的数据部分长度不可超过 65515B；而 ICMP Packet 被封装在 IP Packet 中进行传送，而 ICMP 头的长度为 8B，因此，一个 ICMP Packet 中的数据部分最大长度不能超过 65507B。

若攻击者发送的 ICMP 包中的数据长度超过了 65507B，封装后的 ping 封包将超过 65535B，这对于 IP 通讯协定而言不是合法的用法，此时 ping 封包会分成多个片段，目标电脑必须不断重组封包，期间可能因发缓冲区溢位，而导致系统异常甚至崩溃；实际上，部分系统一旦接受到 payload 长度超过 4000B 的 ICMP packet 就会出现异常了。

**在 1997-1998 年后几乎所有的现代系统都已经修正了这问题**。

由于大部分 OS 对 ping 命令都做了限制，不允许发送长度超过 65507B 的 packet，若要实现该攻击，需要使用 scapy 之类的工具编写程序。

### Land Attack 
利用主机处理 TCP 连接请求时的安全漏洞：构造一个源地址与目的地址都设置为目标主机 IP 地址的 TCP SYN 包，当目标主机收到该 SYN 包后，会向自己回复 SYN/ACK，进而又给自己回复 ACK，而创建了一个空连接，该连接直到超时才会被关闭；

Land Attack 的前提是目标主机的 port 必须是 listened 状态，否则目标主机会直接回复 RST 关闭连接。

### Oscillate Attack 
Oscillate Attack 即振荡 / 乒乓攻击，当两个都会产生输出的端口建立连接后，第一个端口的输出称为第二个端口的输入，导致第二个端口产生输出，同时第二个端口的输出又成为第一个端口的输入，最终导致两个端口间产生大量的数据包，导致 Dos。

## Flood Type Dos 

Flood Type Dos 是最主要的 Dos 形式，通常情况下 Dos 即指的是 Flood Type Dos；   

Flood Type Dos 主要通过向目标发送给大量的网络数据包，导致目标系统或网络的资源耗尽而瘫痪。

Flood Type Dos 成功的原因
- TCP/IP 协议存在可利用的漏洞
- 网络提供 best-effort 的服务，无法区分数据库流量是否是攻击流量
- TCP/IP 没有认证机制，容易进行 IP 欺骗
- 互联网中的路由器不具备数据追踪功能，无法验证一个数据包是否来自己其声称的地方
- 网络带宽与系统资源有限，这是根本原因

### Directed Flood Type Dos 

#### Ping Dos

Ping Dos 利用控制大量主机向目标主机发送大量的 ICMP Echo Request，使目标主机忙于处理这些消息而达到 Dos 的效果。
现代操作系统的防火墙基本上都对 ping 进行了规避，因此成功率较低。

#### TCP SYN Dos 

TCP SYN Dos / SYN Flood，即 SYN 泛洪攻击，是当前最流行的 Dos 方式之一；   

TCP SYN Dos 向目标主机发送大量 SYN 报文，但对服务器响应的 SYN/ACK 不予应答，即只完成了 TCP 两次握手，此时服务器一般会将该连接加入半连接表中，并进行重试（即再次发送 SYN/ACK）并等待一段时间（SYN Timeout，一般为 30s~2min），超时后才会 drop 这个半连接。服务器为了维护一个极大的半连接表，将会耗费很多资源，且同时还要不断对列表中的 IP 进行 SYN/ACK 重试，最终将导致系统的拒绝服务。

一般系统中，半连接数量上限为 1024，超过后服务器将不再接受新的请求。

**实际攻击时，攻击者一般需要伪造源 IP 地址**，否则在未修改本地 TCP 协议栈的情况下，系统会自动对收到的 SYN/ACK 进行响应，无论是回复 ACK 建立 TCP 全连接，或者回复 RST 关闭 TCP 连接，都会释放掉服务器上对应的半连接；同时，目标主机得以较早的释放半连接，大大降低了攻击效果。

- 使用 scapy 实现 SYN Flood   
来源：乾颐堂 python 教学

![image](http://img.cdn.firejq.com/jpg/2017/9/22/37aa5640afbc7b602a4da0ac0493d519.jpg)

#### TCP Handshake Dos

TCP Handshake Dos 也称为空连接攻击，此类攻击会完成完整的 TCP 三次握手，建立 TCP 连接，通过建立大量的 TCP 连接而耗尽目标主机的连接资源，达到 Dos 的效果。

若目标主机有并发连接数量的限制（如路由器对 Telnet/SSH 有登录并发连接数限制），可使用 TCP Handshake Dos，强行与目标主机建立足够多的 TCP 并发连接，占用合法用户的资源以达到 Dos 的效果。

TCP Handshake Dos 与 SYN Flood 的区别在于 SYN Flood 需要不断地发送 SYN 请求，一旦停下来目标主机即可恢复；TCP Handshake Dos 不需要不停地发送连接请求，只需要连接数量达到一定程度即可。

- 使用 scapy 实现 TCP Handshake Dos
来源：乾颐堂 python 教学   
	- 建立 TCP 三次握手：
	发送 SYN：
	SN 值为本地系统根据发送时间点使用特定算法得到的值，代表从本地到目标主机的传输序列号
	![image](http://img.cdn.firejq.com/jpg/2017/9/22/08c64483ecb49299d45b427ee8fbb590.jpg)
	收到 SYN/ACK：
	SN 值为目标主机根据其发送时间点使用特定算法得到的值，代表从目标主机到本地的传输序列号；ACK Number 即第一步的 SN 值加一（ACK 标志位占 1Byte）
	![image](http://img.cdn.firejq.com/jpg/2017/9/22/8c296f9c7a3c278e87764284de85c887.jpg)
	发送 ACK：
	SN 值为第二步中的 ACK Number，ACK Number 为第二步中的 SN 值加一
	![image](http://img.cdn.firejq.com/jpg/2017/9/22/dbd96500e4d56b57a2e8c8f3745a7e83.jpg)
	- 注意：实际使用 scapy 建立 TCP 三次握手连接时，当本地 Linux 系统收到 SYN/ACK 后，由于第一步的 SYN 是由用户脚本（scapy）发出，导致系统不认得该 SYN/ACK，因此系统会自动回应 RST 关闭连接，导致三次握手失败；
	因此，在运行脚本前，需要在 Linux 防火墙中，drop 掉从本地发出的 RST 标志的 TCP 包以及 ICMP 响应包：
	![image](http://img.cdn.firejq.com/jpg/2017/9/22/be09de34505eecd69b4d592a5d6b56d2.jpg)
	- 实现：   
	![image](http://img.cdn.firejq.com/jpg/2017/9/22/2f8cbc3c8447ff48f30da13892c37f20.jpg)

#### HTTP Flood 

HTTP Flood 即利用 HTTP 对网页进行的语义上合法的请求，不停地从目标主机获取数据，占用连接带宽。一般表现为不断从目标网站上下载大文件，从而使一次请求占用系统更多的资源。

HTTP Flood 的缺点是，由于要建立真实的 TCP 的连接进行数据传输，必须使用真实的 IP 地址，但该地址通常是被攻击者控制的傀儡机 IP。

### Reflected Flood Type Dos 

反射型 Dos 在实施时不直接向目的主机发送数据包，而是通过中间主机（反射器）间接向目标主机发送大量数据包，以达到 Dos 的目的。

此类攻击一般会将目标主机 IP 设置为数据包的源地址（SYN/ICMP/UDP 包），向高速高带宽的服务器发送该数据包，服务器收到该包后就会向目标主机回送响应，从而达到利用高性能高带宽服务器对目标主机 Dos 的效果；攻击者还可利用多线程，同时向多个高性能服务器发包，让多个服务器同时进行攻击。

#### Smurf Attack 

Smurf Attack 是一种特殊的 Ping Dos；

Smurf Attack 向某个支持三层广播的网络中发送一个 ICMP 应答请求，且请求地址为该网络的三层广播地址，ICMP 包的源 IP 设置为目标主机的 IP，最终导致该网络所有主机都会向目标主机做出应答，达到 Dos 的效果。

为进行 Smurf Attack，攻击者可利用网络扫描器来查找对三层广播数据包不过滤的路由器，以进而利用该路由器的 LAN 向目的主机进行 Dos。

#### Fraggle Attack 

Fraggle Attack 与 Smurf Attack 类似，但其使用的是 UDP 应答消息而不是 ICMP 报文。

## 其它应用 

#### DHCP Dos 

DHCP Dos 首先对 DHCP 服务器进行 Dos 攻击，请求完所有可用的 IP，然后部署流氓 DHCP Server，发放地址、网关、DNS 等信息，引导 LAN 中其它主机的流量经过本地，从而从流量中获取客户敏感信息。

#### ARP 毒化 Dos

使用随机产生的 MAC 地址伪装目的主机的 MAC，造成整个 LAN 无法连接外网，达到 Dos 的效果。