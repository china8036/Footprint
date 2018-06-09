- [网络操作](#%E7%BD%91%E7%BB%9C%E6%93%8D%E4%BD%9C)
  - [netstat](#netstat)
    - [参考](#%E5%8F%82%E8%80%83)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [使用](#%E4%BD%BF%E7%94%A8)
      - [参数说明](#%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)
      - [结果分析](#%E7%BB%93%E6%9E%9C%E5%88%86%E6%9E%90)
      - [常用示例](#%E5%B8%B8%E7%94%A8%E7%A4%BA%E4%BE%8B)
  - [ifconfig](#ifconfig)
  - [route](#route)
  - [ping](#ping)
  - [arping](#arping)
  - [curl](#curl)
  - [whois](#whois)
  - [traceroute](#traceroute)
  - [host](#host)
  - [nslookup](#nslookup)
  - [dig](#dig)
  - [telnet](#telnet)
  - [wget](#wget)
  - [tcpdump](#tcpdump)
  - [ssh](#ssh)
  - [ARP](#arp)

# 网络操作

## netstat

### 参考
https://en.wikipedia.org/wiki/Netstat

### 概述
netstat 命令用于监控 TCP/IP 网络，它可以显示路由表、实际的网络连接、每一个网络接口设备的状态信息、与 IP、TCP、UDP 和 ICMP 协议相关的统计数据，一般用于检验本机各端口的网络连接情况。

### 使用

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/18/5bd840f18b94ef2ba34e61fad4648a37.jpg)

#### 参数说明
- -a 显示所有 active socket connections，以及正在监听的 TCP/UDP ports；
- -b 显示在创建每个连接或侦听端口时涉及的可执行程序；
- -c 每隔 1 秒就刷新重新显示一遍，直到用户中断它；
- -i 显示所有网络接口的信息；
- -l 仅列出正在监听的端口；
- -n 以网络`IP`地址和端口代替域名和服务名，显示出网络连接情形；
- -r 显示核心路由表，格式同“route -e”；
- -t 显示 TCP 协议的连接情；
- -u 显示 UDP 协议的连接情况；
- -v 显示正在进行的工作；
- -p 显示建立相关连接的程序名和 PID；
- -e 显示以太网统计，此选项可以与 -s 选项结合使用；
- -f 显示外部地址的完全限定域名 (FQDN)；
- -o 显示与每个连接相关的所属进程 ID；
- -s 显示每个协议的统计；
- -x 显示 NetworkDirect 连接、侦听器和共享端点；
- -y 显示所有连接的 TCP 连接模板，无法与其他选项结合使用；
- Interval 重新显示选定的统计，各个显示间暂停的间隔秒数。按 CTRL+C 停止重新显示统计。如果省略，则 netstat 将打印当前的配置信息一次；

#### 结果分析

TCP 连接状态
- LISTEN：侦听来自远方的 TCP 端口的连接请求；
- SYN-SENT：在发送连接请求后等待匹配的连接请求；
- SYN-RECEIVED：在收到和发送一个连接请求后等待对方对连接请求的确认；
- ESTABLISHED：代表一个打开的连接；
- FIN-WAIT-1：等待远程 TCP 连接中断请求，或先前的连接中断请求的确认；
- FIN-WAIT-2：从远程 TCP 等待连接中断请求；
- CLOSE-WAIT：等待从本地用户发来的连接中断请求；
- CLOSING：等待远程 TCP 对连接中断的确认；
- LAST-ACK：等待原来的发向远程 TCP 的连接中断请求的确认；
- TIME-WAIT：等待足够的时间以确保远程 TCP 接收到连接中断请求的确认；
- CLOSED：没有任何连接状态；

#### 常用示例 

- 显示出所有 TCP/UDP 连接（不包括 UNIX Domain sockets）：
```shell
netstat -pantu
```
使用 grep 进行过滤并使用 sort 进行排序
```shell
netstat -pantu | grep :80 | sort
```

-  显示当前的 SYN-RECEIVED 连接（正常情况下该值一般小于 5，当有 Dos 攻击或者邮件炸弹的时候，这个值相当的高）
```shell
netstat -n -p | grep SYN_REC | sort -u
```

- 列出所有连接过的 IP 地址
```shell
netstat -n -p | grep SYN_REC | awk '{print $5}' | awk -F: '{print $1}'`
```

## ifconfig

## route

## ping 

## arping

```
root@kali:~# arping

Usage: arping [-fqbDUAV] [-c count] [-w timeout] [-I device] [-s source] destination
  -f : quit on first reply
  -q : be quiet
  -b : keep broadcasting, don't go unicast
  -D : duplicate address detection mode
  -U : Unsolicited ARP mode, update your neighbours
  -A : ARP answer mode, update your neighbours
  -V : print version and exit
  -c count : how many packets to send
  -w timeout : how long to wait for a reply
  -I device : which ethernet device to use
  -s source : source ip address
  destination : ask for what ip address
```

例：

- 向指定主机发送 ARP 请求

  ```
  arping -I eth1 192.168.1.11
  ```

- 向指定主机发送 ARP 请求，当收到第一个包后自动退出
  
  ```
  arping -I eth1 -f 192.168.56.1
  ```

- 向指定主机发送 ARP 请求，指定发送 50 次

  ```
  arping -I eth1 -c 50 192.168.1.11
  ```

注：只能向当个主机发起 ARP 请求，因此无法用于 ARP 扫描。

## curl 

## whois 

## traceroute

## host 

## nslookup 

## dig

## telnet 

## wget 

## tcpdump 

## ssh

## ARP

参考：http://hichuann.blog.51cto.com/1024435/1571629

- Windows 中的 arp 命令：
  ```
  ARP -sinet_addr eth_addr [if_addr]
  ARP -dinet_addr [if_addr]
  ARP -a[inet_addr] [-N if_addr] [-v]

    -a            Displays current ARP entries by interrogating the current
                  protocol data.  If inet_addr is specified, the IP and Physical
                  addresses for only the specified computer are displayed.  If
                  more than one network interface uses ARP, entries for each ARP
                  table are displayed.
    -v            Displays current ARP entries in verbose mode.  All invalid
                  entries and entries on the loop-back interface will be shown.
    inet_addr     Specifies an internet address.
    -N if_addr    Displays the ARP entries for the network interface specified
                  by if_addr.
    -d            Deletes the host specified by inet_addr. inet_addr may be
                  wildcarded with * to delete all hosts.
    -s            Adds the host and associates the Internet address inet_addr
                  with the Physical address eth_addr.  The Physical address is
                  given as 6 hexadecimal bytes separated by hyphens. The entry
                  is permanent.
    eth_addr      Specifies a physical address.
    if_addr       If present, this specifies the Internet address of the
                  interface whose address translation table should be modified.
                  If not present, the first applicable interface will be used.
  ```

- Kali 中的 arp 命令：
  ```
  root@kali:~# arp -h
  Usage:
    arp [-vn]  [<HW>] [-i <if>] [-a] [<hostname>]             <-Display ARP cache
    arp [-v]          [-i <if>] -d  <host> [pub]               <-Delete ARP entry
    arp [-vnD] [<HW>] [-i <if>] -f  [<filename>]            <-Add entry from file
    arp [-v]   [<HW>] [-i <if>] -s  <host> <hwaddr> [temp]            <-Add entry
    arp [-v]   [<HW>] [-i <if>] -Ds <host> <if> [netmask <nm>] pub          <-''-

          -a                       display (all) hosts in alternative (BSD) style
          -e                       display (all) hosts in default (Linux) style
          -s, --set                set a new ARP entry
          -d, --delete             delete a specified entry
          -v, --verbose            be verbose
          -n, --numeric            don't resolve names
          -i, --device             specify network interface (e.g. eth0)
          -D, --use-device         read <hwaddr> from given device
          -A, -p, --protocol       specify protocol family
          -f, --file               read new entries from file or from /etc/ethers

    <HW>=Use '-H <hw>' to specify hardware address type. Default: ether
    List of possible hardware types (which support ARP):
      ash (Ash) ether (Ethernet) ax25 (AMPR AX.25) 
      netrom (AMPR NET/ROM) rose (AMPR ROSE) arcnet (ARCnet) 
      dlci (Frame Relay DLCI) fddi (Fiber Distributed Data Interface) hippi (HIPPI) 
      irda (IrLAP) x25 (generic X.25) eui64 (Generic EUI-64) 
  ```

例：

- Displays the arp table:
  ```
  arp -a
  ```
  注：在 Windows 中查看 ARP 缓存时必须使用`-a`参数，而在 Kali 中查看 ARP 缓存可不使用`-a`，即直接使用`arp`或`arp 192.168.56.2`。

- Adds a static entry:
  ```
  arp -s 157.55.85.212   00-aa-00-62-c6-09 
  ```

- 删除 internet_address 指定主机的 ARP 缓存（`inet_addr` 可以是通配符 `*`，以删除所有主机）
  ```
  arp -d 157.55.85.212
  ```

- 显示指定 ip 地址的 ARP 信息：
  ```
  arp -a 192.168.56.2
  ```

- 显示指定网络接口的 ARP 信息：
  ```
  arp -a -n 192.168.56.1
  ```
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/31/b1cead6082ac1713e8999bc6ab6b8311.jpg)
