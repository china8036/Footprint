- [Siege Note](#siege-note)
  - [1. 介绍](#1-介绍)
  - [2. 安装](#2-安装)
  - [3. 使用](#3-使用)
  - [4. Refer Links](#4-refer-links)

# Siege Note

## 1. 介绍

Siege 是一款开源的压力测试工具，设计用于评估 WEB 应用在压力下的承受能力。可以根据配置对一个 WEB 站点进行多用户的并发访问，记录每个用户所有请求过程的相应时间，并在一定数量的并发访问下重复进行。siege 可以从您选择的预置列表中请求随机的 URL。所以 siege 可用于仿真用户请求负载，而 ab 则不能。但不要使用 siege 来执行最高性能基准调校测试，这方面 ab 就准确很多。

## 2. 安装

在 https://github.com/JoeDog/siege/releases 下载最新版源码包后编译安装即可：
```shell
wget https://github.com/JoeDog/siege/releases
tar -zxvf siege-latest.tar.gz
cd siege-2.72/
./configure
make
make install
```

## 3. 使用

参数说明：
- -C, 或–config 在屏幕上打印显示出当前的配置，配置是包括在他的配置文件 $HOME/.siegerc 中，可以编辑里面的参数，这样每次 siege 都会按照它运行。
- -v 运行时能看到详细的运行信息
- **-c n, 或–concurrent=n 模拟有 n 个用户在同时访问，即设置并发线程数，n 不要设得太大，因为越大，siege 消耗本地机器的资源越多**
- -i,–internet 随机访问 urls.txt 中的 url 列表项，以此模拟真实的访问情况（随机性), 当 urls.txt 存在是有效
- -d n,–delay=n hit 每个 url 之间的延迟，在 0-n 之间
- **-r n,–reps=n 重复运行测试 n 次，不能与 -t 同时存在**
- **-t n,–time=n 持续运行 siege ‘n’秒（如 10S), 分钟 (10M), 小时 (10H)**
- -l 运行结束，将统计数据保存到日志文件中 siege .log, 一般位于 /usr/local/var/siege .log 中，也可在。siegerc 中自定义
- -R SIEGERC,–rc=SIEGERC 指定用特定的 siege 配置文件来运行，默认的为 $HOME/.siegerc
- **-f FILE, –file=FILE 指定用特定的 urls 文件运行 siege , 默认为 urls.txt, 位于 siege 安装目录下的 etc/urls.txt**
- **-u URL,–url=URL 测试指定的一个 URL, 对它进行”siege “, 此选项会忽略有关 urls 文件的设定**

urls.txt 文件：是很多行待测试 URL 的列表以换行符断开，格式为：
```
[protocol://]host.domain.com[:port](path/to/file)
```

举例：
```
siege -c10 -r10 -f url.txt
```
说明：-c 是并发量，-r 是每个线程的重复次数。url.txt 就是一个文本文件，每行都是一个 url，它会从里面的 URL 中随机选择访问。
![image](http://img.cdn.firejq.com/jpg/2017/10/28/94ff33a39c1ea8ba4ed02ead98a6f9e7.jpg)

Transactions: 100 hits // 完成 100 次处理

Availability: 100.00 % //100.00 % 成功率

Elapsed time: 3.70 secs // 总共使用时间

Data transferred: 0.09 MB // 共数据传输 0.09 MB

Response time: 0.08 secs // 响应时间，显示网络连接的速度

Transaction rate: 27.03 trans/sec // 平均每秒完成 27.03 次处理

Throughput: 0.02 MB/sec // 平均每秒传送数据

Concurrency: 2.24 // 实际最高并发连接数

Successful transactions: 100 // 成功处理次数

Failed transactions: 0 // 失败处理次数

Longest transaction: 0.10 // 每次传输所花最长时间

Shortest transaction: 0.07 // 每次传输所花最短时间

## 4. Refer Links