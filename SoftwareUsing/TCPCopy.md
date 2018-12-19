- [TCPCopy](#tcpcopy)
  - [1. 基本概念](#1-基本概念)
  - [2. 基本原理](#2-基本原理)
  - [3. 基本使用](#3-基本使用)
    - [3.1. online 模式](#31-online-模式)
    - [3.2. offline 模式](#32-offline-模式)
  - [4. Refer Links](#4-refer-links)

# TCPCopy

## 1. 基本概念

> An online request replication tool, also a tcp stream replay tool, fit for real testing, performance testing, stability testing, stress testing, load testing, smoke testing, etc.

tcpcopy 是一种请求复制工具。可以将线上流量拷贝到测试机器，实时的模拟线上环境。在不影响线上用户的情况下，使用线上流量进行测试，以尽早发现 bug。也可以通过放大流量，进行压力测试，评估系统承载能力。

tcpcopy 的优势在于其实时性及真实性，除了少量的丢包，完全拷贝线上流量到测试机器，真实的模拟线上流量的变化规律。

## 2. 基本原理

![image](http://img.cdn.firejq.com/jpg/2018/7/27/0014a92f86032b72e8e5f20b7a6c31df.jpg)

1. 一个访问请求到达线上后端机。
1. socket 包在 IP 层被拷贝了一份传给 tcpcopy 进程。
1. tcpcopy 修改包的目的及源地址，发给测试后端机。
1. 拷贝的包到达测试后端机。
1. 测试后端机处理请求包，并返回结果。
1. 返回结果在 IP 层被截获、丢弃（IP 层截获由 iptables 设置来完成 `iptables -I OUTPUT -p tcp –sport 80 -j QUEUE`），由 intercept 拷贝返回结果的 IP header 返回。
1. IP header 被发送给线上后端机的 tcpcopy 进程。

## 3. 基本使用

tcpcopy 有两种工作模式：
- online: 实时拷贝数据包。对于实时拷贝线上流程进行导入的方式，需要分别在线上服务器和测试服务器安装 tcpcopy。
- offline: 通过使用 tcpdump 等抓包生成的文件进行离线请求重放。对于离线模式，只需要在测试服务器上安装 tcpcopy，编译时指定 `--enable-offline`。

### 3.1. online 模式

安装完成后，我们得到了两个二进制文件：tcpcopy（客户端）和 intercept（服务端）：
- tcpcopy 运行在线上后端机上，负责从线上机复制流量到测试机上。

  tcpcopy 利用 raw socket 或者 pcap 将 IP 层的包复制修改包的目的地址和目的端口，当然也可以修改源地址和源端口。然后将修改之后的包发往测试机器，测试机器上测试程序就会处理复制后的包。

- intercept 运行在测试后端机上，负责在测试机上丢弃测试服务响应的数据包，防止对线上用户造成影响。

  当 tcpcopy 的 client 端将现网包复制过来后，测试机器的测试程序会接收进行处理，如果我们直接将测试程序处理过后的包透过协议栈传回去，就会对用户造成干扰。所以 intercept 就是用来丢包的，不将测试程序处理后的结果返回给现网用户（这也是必须的）。intercept 可以在 IP 层（传统架构）和数据链路层（新架构）丢包。

  当测试机器上面的测试程序将包处理完毕之后，返回包进行入 IP 层，根据上面的规则报文被送往 nf_queue 队列，此时内核会马上通知用户态进程 intercept 收包，intercept 收包之后将必要的信息回包给 tcpcopy，并告诉内核如何处理响应包，即 verdict 。

具体操作：
1. 测试机设置丢弃请求回包：`iptables -I OUTPUT -p tcp --sport 8080 -j NFQUEUE`
1. 测试机启动 intercept 进程：`./intercept -x [online server ip] -b [testing server ip] -d`
1. 线上机启动 tcpcopy 进程：`./tcpcopy -x [online server port]-[testing server ip:testing server port] -s [online server ip] -d`

    tcpcopy 有几种可选的启动方式，如：
    ```bash
    ./tcpcopy -x 80-172.16.***.52:80 -s 172.16.***.53 -d       #全流量复制
    ./tcpcopy -x 80-172.16.***.52:80 -s 172.16.***.53 -r 20 -d  #复制 20% 的流量
    ./tcpcopy -x 80-172.16.***.52:80 -s 172.16.***.53 -n 2 -d    #放大 2 倍流量
    ```
NOTE: 顺序很重要，因为 tcpcopy 复制的包的源地址是不修改的，如果先启动 tcpcopy，会使测试服务的回包也返回给用户，影响用户访问。

### 3.2. offline 模式

1. 在线上服务器抓包：`tcpdump -i eth0 tcp and port 80 -s 0 -w online.pcap`
1. 将抓包生成的文件拷贝到测试服务器
1. 在测试服务器上进行执行如下命令进行重放
    ```bash
    sudo ./intercept
    sudo ./tcpcopy -i /path/online.pcap -x 80-10.16.12.11:80
    ```

## 4. Refer Links

TODO:

[GitHub session-replay-tools/TCPCopy](https://github.com/session-replay-tools/tcpcopy)

[GitHub session-replay-tools/intercept](https://github.com/session-replay-tools/intercept)

[真刀真枪压测：基于 TCPCopy 的仿真压测方案](https://www.cnblogs.com/zhengyun_ustc/p/tcpcopy.html)

[使用 tcpcopy 导入线上流量进行功能和压力测试](http://blog.gaoyuan.xyz/2014/01/08/use-tcpcopy-test-online/)

[大神教你玩转 tcpcopy](https://www.imooc.com/article/29726?block_id=tuijian_wz)