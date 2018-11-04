- [计算机网络 - 物理层：数据通信理论](#计算机网络---物理层数据通信理论)
  - [1. 傅里叶分析](#1-傅里叶分析)
  - [2. 奈奎斯特定理（Nyquist’s Theorem）](#2-奈奎斯特定理nyquists-theorem)
  - [3. 香农定理（Shannon’s Theorem)](#3-香农定理shannons-theorem)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 物理层：数据通信理论

## 1. 傅里叶分析

- 基本频率：当一个信号的所有频率成分是一个频率的整数倍时，该频率称为基本频率。

- 信号周期：基本频率信号的周期。

- 傅立叶级数：任何正常周期为 T 的函数 g(t)，都可由（无限个）正弦和余弦函数合成：

  ![image](http://img.cdn.firejq.com/jpg/2018/6/10/71f45f27fc75e33125a2ebed96a4ef45.jpg)

  其中，f=1/T 是基频， 和 称为正弦和余弦函数的 n 次谐波的振幅。

- 任何信号的传输都可理解为以傅立叶级数的形式传递。

- 对任何的已知数据信号 g(t)，可求得：

  ![image](http://img.cdn.firejq.com/jpg/2018/6/10/596c84c0f8dff7a0ba640ee451fd527b.jpg)

- 有限带宽信号

  ![image](http://img.cdn.firejq.com/jpg/2018/6/10/2432318ad1d683c0ad07b7d25a2edc7f.jpg)

## 2. 奈奎斯特定理（Nyquist’s Theorem）

在理想、无噪声信道中，当带宽为 B HZ，信号电平为 V 级，则：

![image](http://img.cdn.firejq.com/jpg/2018/6/10/13f68bbb813eda02f6817eeaebcee119.jpg)

其中，信号的电平级数 V，在二进制中，仅为 0、1 两级。即： 以每秒高于 2B 次的速率对线路采样是无意义的，因为高频分量已被滤掉，无法再恢复；

## 3. 香农定理（Shannon’s Theorem)

在噪声信道中，带宽为 B Hz，信噪比为 S/N，则：

![image](http://img.cdn.firejq.com/jpg/2018/6/10/791cb8fc3dd317795d959c1720c74314.jpg)

很多情况下噪声用分贝 (dB) 表示：
 
![image](http://img.cdn.firejq.com/jpg/2018/6/10/41c297807981c2880b8f596a61465980.jpg)

如：噪声为 30dB（分贝），则信噪比为 S / N = 1000；

## 4. Refer Links
