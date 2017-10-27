- [Siege Note](#siege-note)
  - [介绍](#%E4%BB%8B%E7%BB%8D)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [使用](#%E4%BD%BF%E7%94%A8)

# Siege Note

## 介绍

Siege是一款开源的压力测试工具，设计用于评估WEB应用在压力下的承受能力。可以根据配置对一个WEB站点进行多用户的并发访问，记录每个用户所有请求过程的相应时间，并在一定数量的并发访问下重复进行。siege可以从您选择的预置列表中请求随机的URL。所以siege可用于仿真用户请求负载，而ab则不能。但不要使用siege来执行最高性能基准调校测试，这方面ab就准确很多。


## 安装

在https://github.com/JoeDog/siege/releases 下载最新版源码包后编译安装即可：
```shell
wget https://github.com/JoeDog/siege/releases
tar -zxvf siege-latest.tar.gz
cd siege-2.72/
./configure
make
make install
```

## 使用