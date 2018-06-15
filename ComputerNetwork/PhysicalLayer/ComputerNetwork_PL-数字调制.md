- [计算机网络 - 物理层：数字调制](#计算机网络---物理层数字调制)
  - [1. 基带传输 ()](#1-基带传输-)
    - [1.1. 线路编码](#11-线路编码)
    - [1.2. 带宽效率](#12-带宽效率)
  - [2. 通带传输 (Passband Transmission)](#2-通带传输-passband-transmission)
  - [3. Refer Links](#3-refer-links)

# 计算机网络 - 物理层：数字调制

比特与代表他们的信号之间的转换过程称为数字调制。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/8eae766465aa78e05d27743f1c1be8cc.jpg)

## 1. 基带传输 (Baseband Transmission)

基带传输是有线介质普遍使用的一种调制方法。直接将数据比特转化为信号，信号的传输占有传输介质上从零到最大值的全部频率，而最大频率取决于信令频率。

### 1.1. 线路编码

稍微复杂的编码方案能将比特转换成更加满足工程考虑的信号。

不归零逆转： 1 为有跳变。

曼彻斯特编码：每个比特时间跳变一次。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/435ae6834d7a291d2b5c2467eeb8376a.jpg)

### 1.2. 带宽效率

符号率 / 波特率：每秒钟信号变化的次数。

比特率 / 位传输率 / 数据传输速率：符号率与每个符号的比特数的乘积，即 C = B x log2(n)，C 是比特率，B 是波特率，n 是调制电平数或线路的状态数，为 2 的整数倍。

## 2. 通带传输 (Passband Transmission)

通带传输是无线、光纤信道最常用的调制方法。通过调节信号的振幅、相位或频率来传输比特，信号占据了以载波信号频率为中心的一段频带；

- 星座图：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/10/da18cbaea56f8b57e4c8fb2bc10c79ba.jpg)

- 格雷码

## 3. Refer Links
