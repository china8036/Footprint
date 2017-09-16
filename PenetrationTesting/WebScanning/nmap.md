# nmap #

Nmap (Network Mapper) 是一款用于网络发现和安全审计的网络安全工具。


## 基本用法 ##
```
nmap [Scan Type(s)] [Options] {target specification}
```

## 核心功能 ##
- Host discovery
用于发现目标主机是否处于活动状态。
Nmap 提供了多种检测机制，可以更有效地辨识主机。例如可用来列举目标网络中哪些主机已经开启，类似于Ping命令的功能。
- Port scanning
用于扫描主机上的端口状态。
	- 端口状态
		- open：正在监听
		- closed：没有程序监听
		- filtered：防火墙阻止了该端口被访问，无法得知是 open / closed
		- unfiltered：nmap 无法确定
- Version detection
用于识别端口上运行的应用程序与程序版本。
Nmap目前可以识别数千种应用的签名（Signatures）,检测数百种应用协议。而对于不识别的应用，Nmap默认会将应用的指纹(Fingerprint)打印出来，如果用户确知该应用程序，那么用户可以将信息提交到社区，为社区做贡献。

- OS detection
用于识别目标主机的操作系统类型、版本编号及设备类型。
Nmap 目前提供1500个操作系统或设备的指纹数据库，可以识别通用PC系统、路由器、交换机等设备类型。

- NSE脚本引擎
NSE是Nmap最强大最灵活的特性之一，可以用于增强主机发现、端口扫描、版本侦测和操作系统侦测等功能，还可以用来扩展高级的功能如web扫描、漏洞发现和漏洞利用等。Nmap使用Lua语言来作为NSE脚本语言，目前的Nmap脚本库已经支持350多个脚本。

- Firewall/IDS 规避和哄骗
Nmap提供多种机制来规避防火墙、IDS的的屏蔽和检查，便于秘密地探查目标主机的状况。
基本的规避方式包括：分片、IP诱骗、IP伪装、MAC地址伪装。

## 扫描类型 ##


- **-sS**: TCP SYN scan，nmap 默认使用该类型进行扫描；


- **-sT**: TCP connect scan，会调用系统 connect()，完成完整的 TCP 三次握手，当 SYN scan 不可用时，nmap 默认使用该类型进行扫描；


- **-sN**; **-sF**; **-sX**；
	- **-sN**：TCP NULL scan；
	- **-sF**：TCP FIN scan；
	- **-sX**：TCP XMAS scan；


- **-sA**：TCP ACK scan


- **-sW**：TCP Window scan
	- 窗口扫描与 ACK 扫描完全相同，但它能利用某些系统的实现细节来区分端口的 open / closed；
	This scan relies on an implementation detail of a minority of systems out on the Internet, so you can't always trust it. Systems that don't support it will usually return all ports closed. Of course, it is possible that the machine really has no open ports. If most scanned ports are closed but a few common port numbers (such as 22, 25, 53) are filtered, the system is most likely susceptible. Occasionally, systems will even show the exact opposite behavior. If your scan shows 1,000 open ports and three closed or filtered ports, then those three may very well be the truly open ones.


- **-sM**：TCP Maimon scan
	- The Maimon scan is named after its discoverer, Uriel Maimon. He described the technique in Phrack Magazine issue #49 (November 1996). Nmap, which included this technique, was released two issues later. This technique is exactly the same as NULL, FIN, and Xmas scans, except that the probe is FIN/ACK. According to RFC 793 (TCP), a RST packet should be generated in response to such a probe whether the port is open or closed. However, Uriel noticed that many BSD-derived systems simply drop the packet if the port is open.


- **-sU**：UDP scan
	- UDP 扫描向每个目标端口发送 UDP 数据包，一般情况下包内不携带任何 payload（可使用`--data`，`--data-string` 或 `--data-length` 选项指定 payload），但对于一些常见的端口（如 53 和 161)，会携带特定的有效载荷来提高响应速率；
	- UDP 扫描可以与 TCP 扫描（如 SYN 扫描等）同时使用；
	- 针对 UDP 扫描速度慢的问题，可使用`--host-timeout`跳过响应过慢的主机扫描；


- **--scanflags**：Custom TCP scan
	- nmap 允许用户自定义扫描类型，使用 `--scanflags` 指定探测包的 flags，然后使用其它扫描类型（如`-sS`，`-sA`等）指定结果判断标准（若不指定其它扫描类型，则默认使用 SYN 扫描的检测标准作为结果判断标准）；
	- `--scanflags` 允许使用数字或flags名称指定要发送的flags，如 `--scanflags 9` 相当于 `--scanflags PSHFIN`；


- **-sI `<zombie host>[:<probeport>]`**：idle scan
	- 僵尸网络扫描/匿名扫描，使用其他网络的主机发检测包进行扫描，意味着没有数据包从您的真实IP地址发送到目标；


- **-sO**：IP protocol scan，用于确定目标机器支持哪些 IP 协议（TCP，ICMP，IGMP等）；




- **-b `<FTP relay host>`**：FTP bounce scan
	- 利用 FTP 代理连接，即允许用户连接到一个FTP服务器，然后要求将文件发送到第三方服务器；
	- 参数 `<username>：<password> @ <target server>：<target port>`；




- **-sY**：SCTP INIT scan
	- SCTP是TCP和UDP协议的一个相对较新的替代方案，结合了TCP和UDP的大多数特性，并添加了诸如多归属和多流传输等新功能，主要用于SS7 / SIGTRAN相关服务；
	- SCTP INIT 扫描相当于 TCP SYN 扫描在 SCIP 协议上的实现版本，具有隐蔽性和速度快的特点；


- **-sZ**： SCTP COOKIE ECHO scan
	- SCTP COOKIE ECHO scan is a more advanced SCTP scan. It takes advantage of the fact that SCTP implementations should silently drop packets containing COOKIE ECHO chunks on open ports, but send an ABORT if the port is closed. The advantage of this scan type is that it is not as obvious a port scan than an INIT scan. Also, there may be non-stateful firewall rulesets blocking INIT chunks, but not COOKIE ECHO chunks. Don't be fooled into thinking that this will make a port scan invisible; a good IDS will be able to detect SCTP COOKIE ECHO scans too. The downside is that SCTP COOKIE ECHO scans cannot differentiate between open and filtered ports, leaving you with the state open|filtered in both cases.



## 扫描选项 ##
- **-P** `<port>`：指定探测的端口，默认情况下，Nmap会扫描 1660 个常用的端口，可以覆盖大多数基本应用情况
	- `-P 80, 8080, 22, 443, 445`
	- `-P 1-65535`
	- `-P 80, 125, 3380-3389`

- **-o**
	- **-oN**：输出为文本文件；
	eg: `nmap -oN scan.txt 192.168.1.66`
	- **-oX**：输出为 XML 文件；
	- **-oG**：输出为 GREPable；
	- **-oS**：输出为脚本文件；

- **-v**：显示扫描进程，一般都会使用该选项；


- **-T**
	- **-T0**：串行扫描，每两次扫描间隔 5min；
	- **-T1**：串行扫描，每两次扫描间隔 15s；
	- **-T2**：串行扫描，每两次扫描间隔 400ms；
	- **-T3**：并行扫描，每两次扫描间隔 0s，默认速度；
	- **-T4**：并行扫描，每两次扫描间隔 xx，速度较快；
	- **-T5**：并行扫描，每两次扫描间隔 xx，速度极快；


- **-Pn**：目标主机已确认是存活的，要求 nmap 不发送 ping request，适用于某些 firewall 禁 ping 来欺骗扫描的情况；


- **-O**：开启 OS detection，发送 TCP and UDP 包，检测目标响应的 TCP/IP 协议栈指纹，与结果数据库 nmap-OS-DB 进行比对，从而推测操作系统信息；
	- **--osscan-guess**：开启模糊猜测，适用于无法准确匹配结果指纹的情况；
	eg: `nmap -O --osscan-guess 192.168.1.150`
	- **--osscan-limit**: Limit OS detection to promising targets；


- **-A**：全面扫描，Enable OS detection, version detection, script scanning, and traceroute；

- **-6**： Enable IPv6 scanning；

- **-sn**： Ping Scan - disable port scan，在 WAN 中使用 ping 扫描，在 LAN 中自动转换为使用 ARP 扫描；
eg：扫描局域网内存活主机
```
nmap -sn 192.168.1.1-254
```


## 扫描目标 ##
- 单一主机
根据 ip 扫描目标主机
`nmap 192.168.1.150`
根据域名扫描目标主机
`nmap www.baidu.com`


- 多个主机
扫描多个主机
`nmap <ip1> <ip2> <ip3> ...`
`nmap 192.168.1.1-100`
扫描整个子网网段
`nmap 192.168.1.1/24`
扫描整个子网网段，但排除个别 ip
`nmap 192.168.1.1/24 -exclude 192.168.1.50`
`nmap 192.168.1.1/24 -exclude file exc.txt`
扫描百度服务器整个 C 段主机
`nmap -sP www.baidu.com/24`
扫描 ip 列表文件中的 ip
`nmap -iL target.txt`
随机选取 10 个主机进行扫描
`nmap -iR 10`



## 脚本引擎 ##
Nmap Scripting Engine (NSE) 是 Nmap 最强大最灵活的特性之一，可以用于增强主机发现、端口扫描、版本侦测和操作系统侦测等功能，还可以用来扩展高级的功能如 web 扫描、漏洞发现和漏洞利用等；  
Nmap使用 Lua 语言来作为 NSE 脚本语言，目前的 Nmap 脚本库已经支持 350 多个脚本。


用法：`--scrpit=脚本名称`

例：
- 扫描 SQL 注入
```
nmap -p 80 www.baidu.com  --script=sql.injection.nse
```
- 使用通配符
```
nmap --script="http-*" www.baidu.com
```
- 使用默认脚本
`nmap -sC` 或 `nmap --script=default`


- 使用 SMB 系列脚本
```
nmap --script="smb*"
```

## 常用用法 ##
- 全面进攻性扫描（包括各种主机发现、端口扫描、版本扫描、OS扫描及默认脚本扫描）:
```
nmap -A -v targetip
```
- Ping 扫描
```
nmap -sn -v targetip
```
- 快速端口扫描
```
nmap -F -v targetip
```
- 版本扫描
```
nmap -sV -v targetip 
```
- 操作系统扫描
```
nmap -O -v targetip
```


## 参考 ##
https://nmap.org/book/man-port-scanning-techniques.html
https://en.wikipedia.org/wiki/Nmap
















