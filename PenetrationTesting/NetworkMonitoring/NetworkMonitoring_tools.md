- [NetworkMonitoring Tools](#NetworkMonitoring-Tools)
  - [1. driftnet](#1-driftnet)
    - [1.1. SYNOPSIS](#11-SYNOPSIS)
    - [1.2. OPTIONS](#12-OPTIONS)
    - [1.3. EXAMPLE](#13-EXAMPLE)
  - [2. urlsnarf](#2-urlsnarf)
    - [2.1. DESCRIPTION](#21-DESCRIPTION)
    - [2.2. SYNOPSIS](#22-SYNOPSIS)
    - [2.3. OPTIONS](#23-OPTIONS)
    - [2.4. EXAMPLE](#24-EXAMPLE)
  - [3. arpspoof](#3-arpspoof)
    - [3.1. SYNOPSIS](#31-SYNOPSIS)
    - [3.2. DESCRIPTION](#32-DESCRIPTION)
    - [3.3. OPTIONS](#33-OPTIONS)
    - [3.4. EXAMPLE](#34-EXAMPLE)
      - [3.4.1. 使用 arpspoof 进行 arp 欺骗](#341-使用-arpspoof-进行-arp-欺骗)
  - [4. BetterCAP](#4-BetterCAP)
    - [4.1. Installation](#41-Installation)
    - [4.2. OPTIONS](#42-OPTIONS)
      - [4.2.1. GENERAL](#421-GENERAL)
        - [4.2.1.1. EXAMPLE](#4211-EXAMPLE)
      - [4.2.2. GUI](#422-GUI)
      - [4.2.3. SNIFF](#423-SNIFF)
        - [4.2.3.1. OPTIONS](#4231-OPTIONS)
      - [4.2.4. SPOOFing](#424-SPOOFing)
        - [4.2.4.1. OPTIONS](#4241-OPTIONS)
        - [4.2.4.2. Examples](#4242-Examples)
      - [4.2.5. Logging](#425-Logging)
        - [4.2.5.1. OPTIONS](#4251-OPTIONS)
        - [4.2.5.2. Examples](#4252-Examples)
        - [4.2.5.3. Options](#4253-Options)
    - [4.3. 实例](#43-实例)
      - [4.3.1. 使用 bettercap 进行口令嗅探](#431-使用-bettercap-进行口令嗅探)

# NetworkMonitoring Tools

## 1. driftnet

Capture images from network traffic and display them in an X window.

### 1.1. SYNOPSIS

> driftnet [options] [filter code]

### 1.2. OPTIONS

- -h

  Display this help message.

- -v

  Verbose operation.

- -b

  捕获到新的图片时发出 beep 声。

- -i interface

  Select the interface on which to listen (default: all interfaces).

- -f file

  读取一个指定 pcap 数据包中的图片；

  file can be a named pipe for use with Kismet or similar.

- -p

  不让所监听的接口使用混杂模式。

- -a

  Adjunct mode：将捕获的图片保存到目录中（不会显示在屏幕上）.

- -m number

  指定 Adjunct mode 中保存图片数量的最大数目。

- -d directory

  指定 Adjunct mode 中保存图片的路径。

- -x prefix

  指定 Adjunct mode 中保存图片的前缀名。

- -s

  Attempt to extract streamed audio data from the network, in addition to images. At present this supports MPEG data only.

- -S

  Extract streamed audio but not images.

- -M command

  Use the given command to play MPEG audio data extracted with the -s option; this should process MPEG frames supplied on standard input. Default: `mpg123 -'.

Filter code can be specified after any options in the manner of tcpdump(8).
The filter code will be evaluated as `tcp and (user filter code)'

You can save images to the current directory by clicking on them.

Adjunct mode is designed to be used by other programs which want to use
driftnet to gather images from the network. With the -m option, driftnet will
silently drop images if more than the specified number of images are saved
in its temporary directory. It is assumed that some other process is
collecting and deleting the image files.

### 1.3. EXAMPLE

- 实时监听：
  ```
  driftnet -i wlan0
  ```

- 读取一个指定 pcap 数据包中的图片：
  ```
  driftnet -f /home/linger/backup/ap.pcapng -a -d /root/drifnet/
  ```

## 2. urlsnarf

### 2.1. DESCRIPTION

Sniff HTTP requests in Common Log Format.

outputs all requested URLs sniffed from HTTP traffic in CLF (Common Log Format, used by almost all web servers), suitable for offline post-processing with your favorite web log analysis tool (analog, wwwstat, etc.).

### 2.2. SYNOPSIS

> urlsnarf [-n] [-i interface | -p pcapfile]  [[-v] pattern [expression]]

### 2.3. OPTIONS

- -n

  Do not resolve IP addresses to hostnames.

- -i interface

  Specify the interface to listen on.

- -p pcapfile

  Process packets from the specified PCAP capture file instead of the network.

- -v

  "Versus" mode. Invert the sense of matching, to select non-matching URLs.  Specify the interface to listen on.

- pattern

  Specify regular expression for URL matching.

- expression

  Specify a tcpdump(8) filter expression to select traffic to sniff.

### 2.4. EXAMPLE

```
urlsnarf -i eth1
```

## 3. arpspoof

### 3.1. SYNOPSIS

> arpspoof [-i interface] [-c own|host|both] [-t target] [-r] host

### 3.2. DESCRIPTION

arpspoof  redirects  packets  from a target host (or all hosts) on the LAN intended for another host on the LAN by forging ARP replies.

This is an extremely effective way of sniffing traffic on a switch.

Kernel IP forwarding (or a userland program which accomplishes the same, e.g. fragrouter(8)) must be turned on ahead of time.

### 3.3. OPTIONS

- -i interface

  Specify the interface to use.

- -c own|host|both

  Specify which hardware address t use when restoring the arp configuration; while cleaning up, packets can be send with  the  own address  as  well  as with the address of the host. Sending packets with a fake hw address can disrupt connectivity with certain switch/ap/bridge configurations, however it works more reliably than using the own address, which is the  default way arpspoof cleans up afterwards.

- -t target

  指定 arp 攻击的目标。如果不指定，则目标为该局域网内的所有机器。可以指定多个目标，如：`arpspoof -i etho -t 192.168.32.100 -t 192.168.32.101`

- -r

  Poison both hosts (host and target) to capture traffic in both directions.  (only valid in conjuntion with -t)

- host

  Specify the host you wish to intercept packets for (usually the local gateway).

### 3.4. EXAMPLE

#### 3.4.1. 使用 arpspoof 进行 arp 欺骗

http://www.freebuf.com/articles/system/5157.html

1. 开启 IP 转发

    ARP 欺骗一般目的是把自己伪装成网关，从而欺骗目标机器，使本应发送到真实网关的数据包发送到欺骗者的机器。但如果欺骗者机器不对这些数据包作处理，当被欺骗数据包到达后就会被本机丢弃，导致目标机器无法上网而暴露欺骗行为，因此，需要在本机上开启 IP 转发功能，以将该类数据包再转发给真正的网关处理：
    ```
    echo 1 > /proc/sys/net/ipv4/ip_forward
    ```

2. 进行 ARP 欺骗

    重定向受害者的流量传送给攻击者：
    ```
    arpspoof -i eth0 -t 192.168.1.5 192.168.1.1
    ```
    使网关的数据重定向到攻击者的机器：
    ```
    arpspoof -i eth0 -t 192.168.1.1  192.168.1.2
    ```

    注意：上述两步可使用`-r`参数一步完成：
    ```
    arpspoof -i eth0 -t 192.168.1.5 -r 192.168.1.1
    ```

3. 捕获数据包

    使用 driftnet 查看捕获流量中的图片：
    ```
    driftnet -i eth0
    ```

## 4. BetterCAP

官方文档：https://www.bettercap.org/

教程：http://xiaix.me/bettercap-shi-yong-zhi-nan/

BetterCAP is a powerful, flexible and portable tool created to perform various types of MITM attacks against a network, manipulate HTTP, HTTPS and TCP traffic in realtime, sniff for credentials and much more.

在过去，ettercap 是完成 network sniff 和 MITM(Man In The Middle)attack 的首选工具，但 bettercap 的出现足以替代 ettercap。

### 4.1. Installation

Kali Linux
```
apt update
apt install bettercap
```

![image](http://img.cdn.firejq.com/jpg/2017/10/31/21121f5cb57812a3e430d792f944b8f5.jpg)

### 4.2. OPTIONS

#### 4.2.1. GENERAL

- -I, --interface IFACE

  BetterCAP 会默认使用本机的 default network interface, 但也可通过此选项指定使用的网络接口。

- --use-mac ADDRESS

  Change the interface MAC address to this value before performing the attack.

- --random-mac

  Change the interface MAC address to a random one before performing the attack.

- -G, --gateway ADDRESS

  指定网关地址（通常情况下 BetterCAP 会自动识别).

- -T, --target ADDRESS1,ADDRESS2

  指定中间人目标 (IP 地址或者 MAC 地址)，若不指定，bettercap will spoof every single address on the network.

- --ignore ADDRESS1,ADDRESS2

  Ignore these IP addresses if found while searching for targets.

- --no-discovery

  不搜索主机，只使用当前的 ARP 缓存。该选项默认为 false.

- --no-target-nbns

  Disable target NBNS hostname resolution.

- --packet-throttle NUMBER

  Number of seconds ( can be a decimal number ) to wait between each packet to be sent.

- --check-updates

  Will check if any update is available and then exit.

- -h, --help

  Display the available options.

##### 4.2.1.1. EXAMPLE

- Attack specific targets:
  ```
  sudo bettercap -T 192.168.1.10,192.168.1.11
  ```

- Attack a specific target by its MAC address:
  ```
  sudo bettercap -T 01:23:45:67:89:10
  ```

- Attack a range of IP addresses:
  ```
  sudo bettercap -T 192.168.1.1-30
  ```

- Attack a specific subnet:
  ```
  sudo bettercap -T 192.168.1.1/24
  ```

- Randomize the interface MAC address during the attack:
  ```
  sudo bettercap --random-mac
  ```

#### 4.2.2. GUI

bettercap 有几个不同的 GUI 选项，但通常你只需要使用默认模式（不是下面的命令)：

- -C：使用 curses GUI

- -D：禁用 GUI

#### 4.2.3. SNIFF

##### 4.2.3.1. OPTIONS

- -X, --sniffer：开启嗅探模式，默认关闭。

- -L, --local：从本机进行嗅探（默认是从其他主机)，NOTE: will enable the sniffer.

- --sniffer-source FILE：指定需要使用的文件 ( NOTE: will enable the sniffer ).

- --sniffer-output FILE：指定输出的文件 ( NOTE: will enable the sniffer ).

- --sniffer-filter EXPRESSION：指定使用的 BPF 过滤器 ( NOTE: will enable the sniffer ).

- -P, --parsers PARSERS：指定要解析的数据的列表 (COOKIE, CREDITCARD, DHCP, DICT, FTP, HTTPAUTH, HTTPS, IRC, MAIL, MPD, MYSQL, NNTP, NTLMSS, PGSQL, POST, REDIS, RLOGIN, SNMP, SNPP, URL, WHATSAPP)，多个用逗号分隔，默认包含全部 ( NOTE: will enable the sniffer ).

- --custom-parser EXPRESSION：Use a custom regular expression in order to capture and show sniffed data ( NOTE: will enable the sniffer ).

#### 4.2.4. SPOOFing

BetterCap already includes an ARP spoofer ( working both in full duplex and half duplex mode which is the default ), a DNS spoofer and the first, fully working and completely automatized ICMP DoubleDirect spoofer in the world

##### 4.2.4.1. OPTIONS

- -S, --spoofer NAME

  Spoofer module to use, available: ARP, ICMP, NONE - default: ARP.

   注意：`-S` 选项是默认开启的，使用 `-S` 选项只是为了选择具体的欺骗方式。

- --no-spoofing

  Disable spoofing, alias for --spoofer NONE / -S NONE.

- --kill

  停止转发流量，直接使目标主机的流量无法到达网关。

- --full-duplex

  Enable full-duplex MITM, this will make bettercap attack both the target(s) and the router.

- --half-duplex

  当路由器中存在一些内置的防 MITM 攻击策略时，可使用该选项。

##### 4.2.4.2. Examples

- Use the good old ARP spoofing:
  ```
  sudo bettercap or sudo bettercap -S ARP or sudo bettercap --spoofer ARP
  ```

- Use a full duplex ICMP redirect spoofing attack:
  ```
  sudo bettercap -S ICMP or sudo bettercap --spoofer ICMP
  ```

- Disable spoofing:
  ```
  sudo bettercap -S NONE or sudo bettercap --spoofer NONE or sudo bettercap --no-spoofing
  ```

- No dear 192.168.1.2, you won't connect to anything anymore :D
  ```
  sudo bettercap -T 192.168.1.2 --kill
  ```
#### 4.2.5. Logging

These options determine how bettercap console logger is going to behave.

##### 4.2.5.1. OPTIONS

- --log：选择日志输出位置

- --silent：不在终端中输出日志

- --log-timestamp：在日志中增加时间戳

##### 4.2.5.2. Examples
Save log output to the out.log file:

sudo bettercap --log out.log

Save log output to the out.log file and suppress terminal output:

sudo bettercap --log out.log --silent

Save log output to the out-ts.log file and enable timestamps for each line:

sudo bettercap --log-timestamp --log out-ts.log

##### 4.2.5.3. Options
-O, --log LOG_FILE
Log all messages into a file, if not specified the log messages will be only print into the shell.

--log-timestamp
Enable logging with timestamps for each line, disabled by default.

-D, --debug
Enable debug logging, it is good practice to use this option while reporting a bug in order to have the full debug log of the program.

--silent
Suppress every message which is not an error or a warning, default to false.

### 4.3. 实例

#### 4.3.1. 使用 bettercap 进行口令嗅探

```
bettercap -I eth1 -T 10.0.2.15 --proxy -P POST
```
