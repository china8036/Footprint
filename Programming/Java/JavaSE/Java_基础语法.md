- [Java 基础语法](#java-%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95)
  - [Java 概述](#java-%E6%A6%82%E8%BF%B0)
    - [发展历史](#%E5%8F%91%E5%B1%95%E5%8E%86%E5%8F%B2)
    - [语言特性](#%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7)
  - [JDK 概述与安装](#jdk-%E6%A6%82%E8%BF%B0%E4%B8%8E%E5%AE%89%E8%A3%85)
    - [JDK 概述](#jdk-%E6%A6%82%E8%BF%B0)
    - [安装 JDK](#%E5%AE%89%E8%A3%85-jdk)
      - [Windows](#windows)
    - [获取源码包](#%E8%8E%B7%E5%8F%96%E6%BA%90%E7%A0%81%E5%8C%85)
  - [注释](#%E6%B3%A8%E9%87%8A)
  - [变量 & 常量](#%E5%8F%98%E9%87%8F-%E5%B8%B8%E9%87%8F)
  - [运算符](#%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [流程控制](#%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6)
  - [输入输出](#%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)
  - [字符串](#%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [数组](#%E6%95%B0%E7%BB%84)

# Java 基础语法

## Java 概述

Java 是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级 Web 应用开发和移动应用开发。

Java 技术栈主要可以分成：Java 语言、Java 运行环境、类库。

### 发展历史

- 1995 年 5 月 23 日，Java 语言诞生
- 1996 年 1 月，第一个 JDK-JDK1.0 诞生
- 1997 年 2 月 18 日，JDK1.1 发布
- 1998 年 12 月 8 日，JAVA2 企业平台 J2EE 发布
- 1999 年 6 月，SUN 公司发布 Java 的三个版本：标准版（J2SE）、企业版（J2EE）和微型版（J2ME）
- 2000 年 5 月 8 日，JDK1.3 发布
- 2000 年 5 月 29 日，JDK1.4 发布
- 2004 年 9 月 30 日，J2SE1.5 发布，成为 Java 语言发展史上的又一里程碑。为了表示该版本的重要性，J2SE1.5 更名为 Java SE 5.0
- 2005 年 6 月，JavaOne 大会召开，SUN 公司公开 Java SE 6。此时，Java 的各种版本已经更名，以取消其中的数字“2”：J2EE 更名为 Java EE，J2SE 更名为 Java SE，J2ME 更名为 Java ME
- 2011 年 7 月 28 日，Oracle 公司发布 Java SE 7
- 2014 年 3 月 18 日，Oracle 公司发表 Java SE 8
- 2017 年 9 月 21 日，Oracle 公司发表 Java SE 9

### 语言特性

Java 之所以被开发，是要达到以下五个目的：
- 应当使用面向对象程序设计方法学
- 应当允许同一程序在不同的计算机平台执行
- 应当包括内建的对计算机网络的支持
- 应当被设计成安全地执行远端代码
- 应当易于使用，并借鉴以前那些面向对象语言（如 C++）的长处。

因此，Java 具有以下[特性](http://www.oracle.com/technetwork/java/langenv-140151.html)：
- 简单性

  Java 的设计借鉴了 C++，但为使系统更容易理解，Java 剔除了 C++ 中许多很少使用、难以处理、易混淆的语法。比如，Java 中没有头文件，指针、结构、联合、操作符重载、虚基类等。

- 面向对象

  Java 是完全面向对象的语言，在 Java 中一切皆对象。但与 C++ 不同的地方在于 Java 中不支持多重继承，取而代之的是更简洁的接口概念。

- 分布式

  Java 可通过例程库处理 TCP/IP 协议。

- 健壮性

  Java 编译器能够检测许多在其它语言中仅在运行时才能够检测出来的问题。

  Java 采用的指针模型可以消除重写内存和损坏数据的可能性，这也是 Java 和 C++ 最大的不同。

- 安全性

  Java 在安全方面投入了很大的精力，使 Java 可以在一定程度上构建防病毒、防篡改的系统。

- 体系结构中立

  Java 编译器将 Java 源代码编译生成一种体系结构中立、与平台无关的字节码文件。这种编译后的字节码文件可以在安装了 Java 运行时系统的任何平台上运行。

- 可移植性

  在 Java 中，数据类型具有固定的大小，这消除了代码移植时令人头痛的主要问题（例如：Java 中 int 永远为 32 位整数，二进制数据以固定格式存储和传输，字符串统一采用 Unicode 进行存储）。

  对于 Java 类库，除了与用户界面有关的部分外，所有其它的 Java 库都能很好的支持平台独立性。

- 解释型

  Java 解释器可以在任何移植了解释器的机器上执行 Java 字节码。

  早期 Java 程序的执行依赖于 Java 解释器，但现在 Java 虚拟机都用了即时编译器，其运行速度与 c++ 相差无几，有些情况下甚至更快。

- 高性能

  字节码可以在运行时动态的翻译成对于 CPU 的机器码。

  Java 的即时编译器在传统编译器上做了很多优化，例如，即时编译器可以监控那些经常执行的代码并优化这些代码以提高速度。

- 多线程

  Java 是第一个支持并发程序设计的主流语言。

- 动态性

  Java 相比 C++ 更具动态性，在 Java 库中可以自由添加新方法和实例变量，而对客户端没有任何影响。

## JDK 概述与安装

### JDK 概述

<!-- TODO: 待补充：JDK&JRE 的关系 -->

版本说明：Java SE 8u31 表示 Java SE 8 的第 31 次更新，内部版本号是 1.8.0_31。

### 安装 JDK

#### Windows

在 oracle 官网下载 JDK 的安装执行文件后，双击运行，选择不带空格和中文的目录，安装即可。

安装完成后，需配置环境变量：
- JAVA_HOME = D:\JAVA\jdk1.6.0
- JRE_HOME = D:\JAVA\jdk1.6.0\jre
- path = ********;%JAVA_HOME%\bin

测试：
```
$ java --version
```
java 9.0.1
Java(TM) SE Runtime Environment (build 9.0.1+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)
```
$ javac --version
```
javac 9.0.1
```

#### Ubuntu 16.04

-	APT 安装

  https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04 

  ```bash
  sudo add-apt-repository ppa:webupd8team/java
  sudo apt-get update
  sudo apt-get install oracle-java8-installer
  sudo update-alternatives --config java
  vim /etc/environment
  ```
  
  add:
  - JAVA_HOME="/usr/java/jdk1.8.0_121"
  - JRE_HOME=”/usr/java/jdk1.8.0_121/jre”
  
  ```bash
  source /etc/environment
  echo $JAVA_HOME
  ```

- 压缩包安装

  到 oracle 官网下载 tar.gz 压缩文件，解压到 /usr/java/；

  将 /usr/java/jdk1.8.0_121/bin 添加到环境变量：
  ```bash
  vim /etc/environment
  ```
  add:
  - JAVA_HOME="/usr/java/jdk1.8.0_121"
  - JRE_HOME=”/usr/java/jdk1.8.0_121/jre”
  ```bash
  source /etc/environment
  echo $JAVA_HOME
  ```
  测试：`java -version`；

### 获取源码包

库源文件在 JDK 中以一个压缩文件 src.zip 的形式发布，若想获取编译器、虚拟机、本地方法、私有辅助类的源代码，可在[这里](http://jdk.java.net/9/) 获取。

## 注释

## 变量 & 常量

## 运算符

## 流程控制

## 输入输出

## 字符串

## 数组
