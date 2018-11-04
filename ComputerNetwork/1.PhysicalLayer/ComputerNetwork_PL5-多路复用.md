- [计算机网络 - 物理层：多路复用](#计算机网络---物理层多路复用)
  - [1. 频分多路复用 FDM(Frequency Division Multiplexing)](#1-频分多路复用-fdmfrequency-division-multiplexing)
  - [2. 时分多路复用 TDM(Time Division Multiplexing)](#2-时分多路复用-tdmtime-division-multiplexing)
    - [2.1. 同步 TDM](#21-同步-tdm)
    - [2.2. 异步 TDM](#22-异步-tdm)
  - [3. 码分多路复用 CDMA(Code Division Multiple Access)](#3-码分多路复用-cdmacode-division-multiple-access)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 物理层：多路复用

复用技术是让多用户共享同一根信道。

![image](http://img.cdn.firejq.com/jpg/2018/6/10/a7e713d1168cec467b437a3b06bdbfb4.jpg)

## 1. 频分多路复用 FDM(Frequency Division Multiplexing)

![image](http://img.cdn.firejq.com/jpg/2018/6/10/416bd2abd2b25e9825ed39bd248f9b3a.jpg)

- 波分多路复用 WDM(Wavelength Division Multiplexing)
  
  本质跟 FDM 一样，在光纤上复用信号。

## 2. 时分多路复用 TDM(Time Division Multiplexing)

### 2.1. 同步 TDM

### 2.2. 异步 TDM

## 3. 码分多路复用 CDMA(Code Division Multiple Access)

![image](http://img.cdn.firejq.com/jpg/2018/6/10/506c8365bd466d932ae96df2633ce705.jpg)

每个用户拥有一个唯一码片序列，码片是正交的，能够同时传输。CDMA 允许每个用户利用整个频段发送信号，而且没有时间限制。

CDMA 的关键在于：能够提取出期望的信号，同时拒绝所有其它的信号并把这些信号当作噪声。

- 原理
  - 每个比特 (bit) 时间被分成 m 个短的时间片，称为时间片 (Chip)。通常每个比特有 64 个或 128 个时间片（Chip）。
  - 每个站点被指定一个唯一的 m 位的代码，称为时间片序列（Chip sequence）。
    
    时间片序列的正交特性：设某站的时间序列是 S，内含 S1…Si，另一个站的时间序列是 T，含 T1…Ti
    
    S 和 T 有如下这些性质：

    ![image](http://img.cdn.firejq.com/jpg/2018/6/10/8b690d0bae6235d52297c772565eb78b.jpg)

  - 过程
    - 发送“1”时，站点发送 chip sequence 的原码。
    - 发送“0”时，站点发送 chip sequence 的反码。

    例：
    A 的时间片序列为 00011011
    ```
    A 发送“1”时为 00011011
    A 发送“0”时为 11100100
    ```
    为解释时容易理解，使用双极概念，用“+1”代替“1”，用“-1”代替“0”
    ```
    A 发送“1”时为 -1-1-1+1+1-1+1+1
    A 发送“0”时为 +1+1+1-1-1+1-1-1
    ```

    例题：

    ![image](http://img.cdn.firejq.com/jpg/2018/6/10/17f2ef5377822f92cdea2f551614d1df.jpg)

- 应用：除了蜂窝网络，广泛用于卫星通信、有线电视网络、3G/4G 网通信。

## 4. Refer Links
