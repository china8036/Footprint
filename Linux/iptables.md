- [iptables](#iptables)
  - [查看并清空现有规则](#%E6%9F%A5%E7%9C%8B%E5%B9%B6%E6%B8%85%E7%A9%BA%E7%8E%B0%E6%9C%89%E8%A7%84%E5%88%99)
  - [添加规则](#%E6%B7%BB%E5%8A%A0%E8%A7%84%E5%88%99)
  - [设定默认规则阻断流量](#%E8%AE%BE%E5%AE%9A%E9%BB%98%E8%AE%A4%E8%A7%84%E5%88%99%E9%98%BB%E6%96%AD%E6%B5%81%E9%87%8F)
  - [防黑客安全设置](#%E9%98%B2%E9%BB%91%E5%AE%A2%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE)
  - [FORWARD转发功能](#forward%E8%BD%AC%E5%8F%91%E5%8A%9F%E8%83%BD)
  - [关于iptables存储的命令：（若不存储到文件中重启之后就会丢失）](#%E5%85%B3%E4%BA%8Eiptables%E5%AD%98%E5%82%A8%E7%9A%84%E5%91%BD%E4%BB%A4%EF%BC%9A%EF%BC%88%E8%8B%A5%E4%B8%8D%E5%AD%98%E5%82%A8%E5%88%B0%E6%96%87%E4%BB%B6%E4%B8%AD%E9%87%8D%E5%90%AF%E4%B9%8B%E5%90%8E%E5%B0%B1%E4%BC%9A%E4%B8%A2%E5%A4%B1%EF%BC%89)
  - [Refer Links](#refer-links)

# iptables

以下命令都以 Ubuntu 16.04 为平台；

对于防火墙的设置，有两种策略：
- 全部通讯口都允许使用，只是阻止一些我们知道的不安全的或者容易被利用的口；
- 先屏蔽所有的通讯口，而只是允许我们需要使用的通讯端口。
这里使用的是第二种原则，如果需要开启其他端口，先参考计算机通讯口说明，然后自己添加。

## 查看并清空现有规则

- 查看本机关于IPTABLES的设置情况:
  ```shell
  iptables -L –nv
  ```
  如果你在安装linux时没有选择启动防火墙,什么规则都没有.是这样的   
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/9/7ec808e0873c52504ea634c75bf750b2.jpg)

- 删除已添加的iptables规则：
  将所有iptables规则按照1.2.3....进行排序，执行： 
  ```shell
  iptables -L -nv --line-numbers 
  ```
  如果要删除INPUT里序号为5的规则，执行：
  ```shell
  iptables -D INPUT 5
  ```

- 删除原来 iptables 里面已经有的所有规则
  ```shell
  iptables -F
  iptables -X
  ```

## 添加规则

添加规则时，应特别注意各条规则的先后顺序，先被DROP掉的规则，后续再处理也无效.
（1）允许建立会话来接受流量
```shell
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```
（2）为特定的端口开放流量
SSH
对其他主要允许的端口的 OUTPUT设置，在指定端口上允许入站流量：（eth1未必正确，视具体情况而定）
（阻断所有流量您也可以启动系统，但是您可能正在通过SSH工作，所有在您阻断其他流量前有必要允许SSH流量。）
```shell
iptables -A INPUT -p tcp -i eth0 --dport ssh -j ACCEPT
```
HTTP
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 80 -j ACCEPT
```
允许ping
```shell
iptables -A INPUT -p icmp -j ACCEPT
```
DNS
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 53 -j ACCEPT
iptables -A OUTPUT -o eth1 -p UDP --sport 1024:65535 --dport 53 -j ACCEPT
```
HTTPS
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 443 -j ACCEPT
```
Email 接受 和发送
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 110 -j ACCEPT
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 25 -j ACCEPT
```
FTP 数据和控制
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 20 -j ACCEPT
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 21 -j ACCEPT
```
DHCP
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 68 -j ACCEPT
iptables -A OUTPUT -o eth1 -p UDP --sport 1024:65535 --dport 68 -j ACCEPT
```
POP3S Email安全接收
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 995 -j ACCEPT
```
时间同步服务器 NTP
```shell
iptables -A OUTPUT -o eth1 -p TCP --sport 1024:65535 --dport 123 -j ACCEPT
```
拒绝 eth1 其他剩下的
```shell
iptables -A OUTPUT -o eth1 --match state --state NEW,INVALID -j LOG
```

--limit 对由此规则引发的记录事件的频率进行限制, --log-prefix 在每条记录前加上一个前缀，以便查找
```shell
iptables-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7
```
（3）本地lo流量
方法一：
设置：本地进程 lo 的 INPUT 和 OUTPUT 链接 ； eth1的 INPUT链
```shell
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i eth1 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i eth1 -m state --state NEW,INVALID -j LOG
iptables -A OUTPUT -o lo -j ACCEPT
```
方法二：
到目前为止我们设置过程中唯一的问题是回环端口(loopbakc)也被阻断了。我们本可以通过指定 -i eth0 来仅仅丢弃eth0上的数据包，但我们也可以为回环端口(loopback)添加一条规则。如果我们追加这条规则，这将太晚了----因为所有的流量已经被丢弃。我们必须插入这条规则到第4行（从0开始，此处第4行即为最后一行，即匹配不了其他规则时才进入此规则）。
```shell
iptables -I INPUT 4 -i lo -j ACCEPT
```


## 设定默认规则阻断流量
方法一：   
一旦一条规则对一个包进行了匹配，其他规则不再对这个包有效。因为我们的规则首先允许SSH和WEB流量，所以只要我们阻断所有流量的规则紧跟其后，我们依然能接受我们感兴趣的流量。我们要做的仅仅是把阻断所有流量的规则放在最后，所以我们需要再次用到它。
```shell
iptables -A INPUT -j DROP
```

方法二：   
设定预设(默认)规则（policy），抛弃所有不符合三种链规则的数据包
```shell
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD DROP
```


## 防黑客安全设置
对于OUTPUT规则，因为预设的是ACCEPT，所以要添加DROP规则，减少不安全的端口链接。例如：
```shell
iptables -A OUTPUT -p tcp --sport 31337 -j DROP
iptables -A OUTPUT -p tcp --dport 31337 -j DROP
```
具体要DROP掉哪些端口，可以查询相关的资料，可以把一些黑客常用的扫描端口全部DROP掉，多多少少提高一点服务器的安全性。
我们还可以把规则限制到只允许某个IP：
```shell
iptables -A INPUT -s 192.168.0.18 -p tcp --dport 22 -j ACCEPT
```
如果要允许一个IP段，可以使用下面的写法：
```shell
iptables -A INPUT -s 192.168.0.1/255 -p tcp --dport 22 -j ACCEPT
```


## FORWARD转发功能
开启转发功能：
```shell
iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth1 -o eh0 -j ACCEPT
```
丢弃损坏的TCP包：
```shell
iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth1 -o eh0 -j ACCEPT
```
处理IP碎片数量，防止攻击，允许100个/s ：
```shell
iptables -A FORWARD -f -m limit --limit 100/s --limit-burst 100 -j ACCEPT
```
设置ICMP包过滤，允许1个包/s，限制触发条件是10个包：
```shell
iptables -A FORWARD -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT
```



## 关于iptables存储的命令：（若不存储到文件中重启之后就会丢失）

代码:
```shell
iptables-save > /etc/iptables.up.rule ＃ 存在你想存的地方
```
代码:
```shell
iptables-restore < /etc/iptables.up.rules ＃调用的命令
```

因为iptables 在每次机器重新启动以后，需要再次输入或者调用，为了方便操作，
```shell
sudo vim /etc/network/interfaces
```
在
```shell
auto ath0
iface ath0 inet dhcp
```
后面加上:
```shell
pre-up iptables-restore < /etc/iptables.up.rules ＃启动自动调用已存储的iptables
post-down iptables-save > /etc/iptables.up.rules #关机时，把当前iptables 储存
```


**注：不要随便/etc/init.d/networking restart! 后果及解决方法待补充**


## Refer Links
http://wiki.ubuntu.org.cn/UbuntuHelp:IptablesHowTo/zh

https://www.logcg.com/archives/541.html

http://m.lao8.org/a1353

http://www.centoscn.com/CentOS/config/2014/0428/2884.html

https://www.deamwork.com/archives/ubuntu-iptables-conn.orz6
