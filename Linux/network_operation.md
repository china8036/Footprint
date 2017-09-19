# 网络操作 #

## netstat ##
### 参考 ###
https://en.wikipedia.org/wiki/Netstat

### 概述 ###
netstat 命令用于监控 TCP/IP 网络，它可以显示路由表、实际的网络连接、每一个网络接口设备的状态信息、与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。

### 使用 ###

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/18/5bd840f18b94ef2ba34e61fad4648a37.jpg)


#### 参数说明 ####
- -a 显示所有active socket connections，以及正在监听的 TCP/UDP ports；
- -b 显示在创建每个连接或侦听端口时涉及的可执行程序；
- -c 每隔1秒就刷新重新显示一遍，直到用户中断它；
- -i 显示所有网络接口的信息；
- -l 仅列出正在监听的端口；
- -n 以网络IP地址和端口代替域名和服务名，显示出网络连接情形；
- -r 显示核心路由表，格式同“route -e”；
- -t 显示TCP协议的连接情；
- -u 显示UDP协议的连接情况；
- -v 显示正在进行的工作；
- -p 显示建立相关连接的程序名和PID；
- -e 显示以太网统计，此选项可以与 -s 选项结合使用；
- -f 显示外部地址的完全限定域名(FQDN)；
- -o 显示与每个连接相关的所属进程 ID；
- -s 显示每个协议的统计；
- -x 显示 NetworkDirect 连接、侦听器和共享端点；
- -y 显示所有连接的 TCP 连接模板，无法与其他选项结合使用；
- Interval 重新显示选定的统计，各个显示间暂停的间隔秒数。按 CTRL+C 停止重新显示统计。如果省略，则 netstat 将打印当前的配置信息一次；


#### 结果分析 ####

TCP 连接状态
- LISTEN：侦听来自远方的TCP端口的连接请求；
- SYN-SENT：在发送连接请求后等待匹配的连接请求；
- SYN-RECEIVED：在收到和发送一个连接请求后等待对方对连接请求的确认；
- ESTABLISHED：代表一个打开的连接；
- FIN-WAIT-1：等待远程TCP连接中断请求，或先前的连接中断请求的确认；
- FIN-WAIT-2：从远程TCP等待连接中断请求；
- CLOSE-WAIT：等待从本地用户发来的连接中断请求；
- CLOSING：等待远程TCP对连接中断的确认；
- LAST-ACK：等待原来的发向远程TCP的连接中断请求的确认；
- TIME-WAIT：等待足够的时间以确保远程TCP接收到连接中断请求的确认；
- CLOSED：没有任何连接状态；


#### 常用示例 ####

- 显示出所有 TCP/UDP 连接（不包括 UNIX Domain sockets）：
```shell
netstat -pantu
```
使用 grep 进行过滤并使用 sort 进行排序
```shell
netstat -pantu | grep :80 | sort
```


-  显示当前的 SYN-RECEIVED 连接（正常情况下该值一般小于5，当有Dos攻击或者邮件炸弹的时候，这个值相当的高）
```shell
netstat -n -p | grep SYN_REC | sort -u
```


- 列出所有连接过的IP地址
```shell
netstat -n -p | grep SYN_REC | awk '{print $5}' | awk -F: '{print $1}'`
```


## ifconfig ##



## route ##



## ping ##




## curl ##



## whois ##






## traceroute ##



## host ##


## nslookup ##





## dig ##





## telnet ##


## wget ##



## tcpdump ##




## ssh ##
































































































