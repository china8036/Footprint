- [Java 基础语法](#java-%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95)
  - [1. Java 概述](#1-java-%E6%A6%82%E8%BF%B0)
    - [1.1. 发展历史](#11-%E5%8F%91%E5%B1%95%E5%8E%86%E5%8F%B2)
    - [1.2. 语言特性](#12-%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7)
  - [2. JDK 概述与安装](#2-jdk-%E6%A6%82%E8%BF%B0%E4%B8%8E%E5%AE%89%E8%A3%85)
    - [2.1. JDK 概述](#21-jdk-%E6%A6%82%E8%BF%B0)
    - [2.2. 安装 JDK](#22-%E5%AE%89%E8%A3%85-jdk)
      - [2.2.1. Windows](#221-windows)
      - [2.2.2. Ubuntu](#222-ubuntu)
    - [2.3. 获取源码包](#23-%E8%8E%B7%E5%8F%96%E6%BA%90%E7%A0%81%E5%8C%85)
  - [3. Java 程序： Application 与 Applet 的区别](#3-java-%E7%A8%8B%E5%BA%8F%EF%BC%9A-application-%E4%B8%8E-applet-%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [3.1. Application](#31-application)
    - [3.2. Applet](#32-applet)
  - [4. 注释](#4-%E6%B3%A8%E9%87%8A)
    - [4.1. 行注释](#41-%E8%A1%8C%E6%B3%A8%E9%87%8A)
    - [4.2. 块注释](#42-%E5%9D%97%E6%B3%A8%E9%87%8A)
    - [4.3. 文档注释](#43-%E6%96%87%E6%A1%A3%E6%B3%A8%E9%87%8A)
  - [5. 变量 & 常量](#5-%E5%8F%98%E9%87%8F-%E5%B8%B8%E9%87%8F)
    - [5.1. 变量](#51-%E5%8F%98%E9%87%8F)
    - [5.2. 常量](#52-%E5%B8%B8%E9%87%8F)
  - [6. 数据类型](#6-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [6.1. 基本数据类型](#61-%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
      - [6.1.1. 整型](#611-%E6%95%B4%E5%9E%8B)
      - [6.1.2. 浮点型](#612-%E6%B5%AE%E7%82%B9%E5%9E%8B)
      - [6.1.3. char](#613-char)
      - [6.1.4. boolean](#614-boolean)
    - [6.2. 引用类型](#62-%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B)
  - [7. 类型转换](#7-%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
    - [7.1. 自动转换](#71-%E8%87%AA%E5%8A%A8%E8%BD%AC%E6%8D%A2)
    - [7.2. 强制转换](#72-%E5%BC%BA%E5%88%B6%E8%BD%AC%E6%8D%A2)
  - [8. 枚举类型](#8-%E6%9E%9A%E4%B8%BE%E7%B1%BB%E5%9E%8B)
  - [9. 大数值](#9-%E5%A4%A7%E6%95%B0%E5%80%BC)
  - [10. 运算符](#10-%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.1. 算术运算符](#101-%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.2. 赋值运算符](#102-%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.3. 关系运算符](#103-%E5%85%B3%E7%B3%BB%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.4. 逻辑运算符](#104-%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.5. 位运算符](#105-%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10.6. 条件运算符](#106-%E6%9D%A1%E4%BB%B6%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [11. 流程控制](#11-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6)
    - [11.1. 条件控制](#111-%E6%9D%A1%E4%BB%B6%E6%8E%A7%E5%88%B6)
    - [11.2. 循环控制](#112-%E5%BE%AA%E7%8E%AF%E6%8E%A7%E5%88%B6)
      - [11.2.1. while](#1121-while)
      - [11.2.2. for](#1122-for)
    - [11.3. 中断控制](#113-%E4%B8%AD%E6%96%AD%E6%8E%A7%E5%88%B6)
    - [11.4. switch](#114-switch)
  - [12. 输入输出](#12-%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)
    - [12.1. 控制台输入输出](#121-%E6%8E%A7%E5%88%B6%E5%8F%B0%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)
    - [12.2. 文件输入输出](#122-%E6%96%87%E4%BB%B6%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)
  - [13. 字符串](#13-%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [13.1. 子串](#131-%E5%AD%90%E4%B8%B2)
    - [13.2. 拼接](#132-%E6%8B%BC%E6%8E%A5)
    - [13.3. 相等判断](#133-%E7%9B%B8%E7%AD%89%E5%88%A4%E6%96%AD)
    - [13.4. 字符串长度](#134-%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%95%BF%E5%BA%A6)
    - [13.5. 空串和 Null 串](#135-%E7%A9%BA%E4%B8%B2%E5%92%8C-null-%E4%B8%B2)
    - [13.6. 不可变字符串](#136-%E4%B8%8D%E5%8F%AF%E5%8F%98%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [13.7. 字符串构建](#137-%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%9E%84%E5%BB%BA)
  - [14. 数组](#14-%E6%95%B0%E7%BB%84)
    - [14.1. 创建与初始化](#141-%E5%88%9B%E5%BB%BA%E4%B8%8E%E5%88%9D%E5%A7%8B%E5%8C%96)
    - [14.2. 多维数组](#142-%E5%A4%9A%E7%BB%B4%E6%95%B0%E7%BB%84)
    - [14.3. 数组拷贝](#143-%E6%95%B0%E7%BB%84%E6%8B%B7%E8%B4%9D)
    - [14.4. 数组填充](#144-%E6%95%B0%E7%BB%84%E5%A1%AB%E5%85%85)
    - [14.5. 快速排序](#145-%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)
    - [14.6. 二分查找](#146-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)

# Java 基础语法

## 1. Java 概述

Java 是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级 Web 应用开发和移动应用开发。

Java 技术栈主要可以分成：Java 语言、Java 运行环境、类库。

Java 是一种半编译半解释的强类型的语言。

### 1.1. 发展历史

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

### 1.2. 语言特性

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

## 2. JDK 概述与安装

### 2.1. JDK 概述

<!-- TODO: 待补充：JDK&JRE 的关系 -->

版本说明：Java SE 8u31 表示 Java SE 8 的第 31 次更新，内部版本号是 1.8.0_31。

### 2.2. 安装 JDK

#### 2.2.1. Windows

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


#### 2.2.2. Ubuntu

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

### 2.3. 获取源码包

库源文件在 JDK 中以一个压缩文件 src.zip 的形式发布，若想获取编译器、虚拟机、本地方法、私有辅助类的源代码，可在[这里](http://jdk.java.net/9/) 获取。

## 3. Java 程序： Application 与 Applet 的区别

Java 的用户程序分为两类：Java Application 和 Java Applet。这两类程序在组成结构和执行机制上都有一定的差异：

### 3.1. Application

- application 主要是桌面应用程序的开发，application 是不能用 Jsp 加载的 。

- Java Application 是完整的程序，可以独立运行。

- Java Application 程序被编译以后，用普通的 Java 解释器就可以使其边解释边执行。

- 每个 Java Application 程序必定含有一个并且只有一个 main 方法，程序执行时，首先寻找 main 方法，并以此为入口点开始运行。含有 main 方法的那个类，常被称为主类，也就是说，Java Application 程序都含有一个主类。

### 3.2. Applet

- applet 一般用于 B/S 页面上作为插件式的开发。

- Java Applet 程序不能单独运行，它必须嵌入到用 HTML 语言编写的 Web 页面中，通过与 Java 兼容的浏览器来控制执行。

- Java Applet 必须通过网络浏览器或者 Applet 观察器才能执行。 

- Applet 程序则没有含 main 方法的主类，这也正是  Applet 程序不能独立运行的原因。

## 4. 注释

### 4.1. 行注释

行注释使用`//`。

### 4.2. 块注释

块注释使用`/* */`。

### 4.3. 文档注释

<!-- TODO: 补充 -->

文档注释示例：
```java
/**
 * @version 1.01
 * @author firejq
 */
```
文档注释可通过 javadoc 工具自动生成 API 文档。

## 5. 变量 & 常量

### 5.1. 变量

在 Java 中，变量名是一个以字母开头并以字母或数字构成的字符串序列。其中，字母包括了所有合法的 Unicode 字符。

不同于 C++，在 Java 中不区分变量的声明和定义。但在声明 / 定义一个变量后，必须用赋值语句对变量进行显式初始化后才能使用，且变量的声明应该尽可能的靠近变量第一次使用的地方。

### 5.2. 常量

在 Java 中，使用关键字 final 指示常量，习惯上常量名使用全大写。

```java
final double HEIGHT = 1.85;
```

NOTE: `const`是 Java 的保留字，但目前（JDK 9）并未使用该关键字。

## 6. 数据类型

Java 是一种强类型的语言，这意味着必须为每一个变量都声明一种类型。

### 6.1. 基本数据类型

在 Java 中，一共有 8 种基本数据类型，包裹 4 种整型、2 种浮点型、1 种用于标识 Unicode 编码的字符单元的字符类型和 1 种用于表示真值的 boolean 类型。

#### 6.1.1. 整型

整型用于表示没有小数部分的数值，允许为负数。Java 提供了 4 种整型：
- int

  占 4 字节，-2147483648(-2^31) ~ 2147483647(2^31 - 1)（正好超过 30 亿），默认值是 0。

  没有使用后缀的整型数值都为 int 型。

- short

  占 2 字节，-32768(-2^15) ~ 32767(2^15 - 1)，默认值是 0。

  Short 数据类型也可以像 byte 那样节省空间。一个 short 变量是 int 型变量所占空间的二分之一。

- byte

  占 1 字节，-128(-2^7) ~ 127(2^7 - 1)，默认值是 0。

  byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一。

- long

  占 8 字节，-9,223,372,036,854,775,808(-2^63) ~ 9,223,372,036,854,775,807(2^63 -1)，默认值是 0L。
  
  长整型数值有一个后缀"L"，"L"理论上不分大小写，但是若写成"l"容易与数字"1"混淆，不容易分辩，所以最好大写。

  long 类型主要使用在需要比较大整数的系统上。

Java 中整型数值默认为 int 类型。

在 Java 中，整型的范围与运行 Java 代码的机器无关，这解决了代码的移植性问题。

使用非十进制表示数时，需要使用不同的前缀：
- 二进制：0b 或 0B
- 八进制：0
- 十六进制：0x 或 0X

从 Java7 开始，支持使用下划线作为数字字面量的分隔符，以增强可读性。例如：10000000 可写作 10_000_000，让人更容易读。

在 Java 中，没有任何无符号形式的 int、long、short、byte 类型。

#### 6.1.2. 浮点型

浮点型用于表示有小数部分的数值。Java 提供了 2 种浮点型：
- float

  单精度浮点型，占 4 字节，有效位数为 6~7 位，默认值是 0.0f。

  float 型数值有一个后缀 F 或 f，**没有后缀的浮点数默认为 double 类型**。

- double

  双精度浮点型，占 8 字节，有效位数为 15 为，默认值是 0.0d。

  double 型数值可使用后缀 D 或 d，也可不使用。

Java 中的浮点数值默认为 double 类型。

实际上绝大部分应用程序都使用 double 类型，很少情况适合使用 float 类型（如：在储存大型浮点数组时使用 float 以节省空间）。

Java 中的浮点数都遵循 IEEE 754 标准。

可以使用十六进制表示浮点数值。例如，0.125 也就是 2^-3 可以表示为 0x1.0p-3（十六进制的浮点数表示法中，p 表示基数为 2 的指数，且尾数采用十六进制，但指数采用十进制表示）。

Java 中提供了三个特殊的浮点数值：
- 正无穷大：用常量 Double.POSITIVE_INFINITY 表示。一个正整数除以 0 的结果为正无穷大。

- 负无穷大：用常量 Double.NEGATIVE_INFINITY 表示。一个负整数除以 0 的结果为正无穷大。

- 不合法的数：用常量 Double.NaN 表示。0/0 或负数的平方根结果为 NaN。
  
  NOTE: 不可通过 == 检测一个特定值是否为 NaN，因为 NaN 不等于任何数，包括它本身。但可使用 Double.isNaN(x) 方法来判断。

#### 6.1.3. char

- char 类型占 2 字节，用于表示 UTF-16 编码中的一个代码单元。

  **一个 char 类型的值不一定能表示单个 Unicode 字符**。由于 16 位的长度无法表示所有 Unicode 字符（大于 65536 个），因此 UTF-16 编码采用不同长度的编码表示所有的 Unicode 码点：在基本的多语言级别中，每个字符用 16 位表示，称为代码单元；而辅助字符采用连续 2 个代码单元进行编码。通过这样的设计，才能使用 16 位的长度表示所有 Unicode 字符。但这就导致了**有些 Unicode 字符可以用一个 char 值描述，另外一些 Unicode 字符需要 2 个 char 值才能表示**。

  core Java 10th:
  > 我们强烈简易不要在程序中使用 char 类型，除非确实需要处理 UTF-16 代码单元。最好将字符串作为抽象数据类型处理。

- char 类型的字面量要使用单引号括起来。
  
  'A'与"A"表示不同的值。'A'是编码值位 65 所对应的字符常量，而"A"是包含一个字符 A 的字符串。

- char 类型的值也可以表示为十六进制值，其范围为、u0000~\uffff。

- 在 Java 中，Unicode 转义序列会在解析代码之前得到处理，而其它转义序列不会。因此，转义序列、u 可以出现在加引号的常量之外，而其它转义序列不可以。
  
  例如：
  
  `public static void main(String \u005B \u005D args)`是完全合法的。

  `// \u00A0 is a newline`会导致语法错误，因为、u00A0 会被替换为一个换行符。

  `// look inside c:\users`会导致语法错误，因为、u 后没有跟着 4 个十六进制数。

#### 6.1.4. boolean

boolean 类型占 1 字节，有两个取值：false 和 true（默认值为 false），用于进行逻辑判断。

整型值与布尔值之间不能进行相互转换，因此，0 不等价于 false，1 不等价于 true。

### 6.2. 引用类型

在 Java 中，引用类型的变量非常类似于 C/C++ 的指针。引用类型指向一个对象，指向对象的变量是引用变量。这些变量在声明时被指定为一个特定的类型，变量一旦声明后，类型就不能被改变了。

对象、数组都是引用数据类型。

所有引用类型的默认值都是 null。

一个引用变量可以用来引用任何与之兼容的类型。

## 7. 类型转换

### 7.1. 自动转换

下图给出数值类型之间的合法转换规则：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/25/c22c9e291a897e9967753c3caaca04f4.jpg)

（转换原则：往表数范围更大的方向转换）

其中，实心箭头表示无信息丢失的转换；虚箭头表示可能会有精度损失的转换。

**当对两个不同类型的操作数进行运算时，会按照合法转换规则发生类型的自动转换，使得转换后的两个操作数类型相同后再进行运算**。例：若两个操作数中有一个 double，则另一个操作数无论是什么类型都会自动转换为 double。

### 7.2. 强制转换

在 Java 中，允许进行数值类型的强制类型转换，当然这种操作可能会导致丢失数值的精度甚至会导致原值被截断成一个完全不同的值。

```java
double x = 9.997;
int nx = (int)x; // nx = 9

int a = 300;
byte b = (byte)a; // b = 44
```

## 8. 枚举类型

```java
enum Size {
  SMALL,
  MEDIUM,
  LARGE,
  X-LARGE
};

Size s = Size.SMALL;
Size s = Size.MEDIUM;
Size s = Size.LARGE;
Size s = Size.X-LARGE;
Size s = Size.a; // error
```

## 9. 大数值

若基本的数值类型不能满足需求，可使用 BigInteger 和 BigDecimal。

这两个类可以处理包含任意长度的数字序列的数值，实现了任意精度的整数运算和浮点运算。

## 10. 运算符

### 10.1. 算术运算符

- `+`：加法

- `-`：减法

- `*`：乘法

- `/`：除法

  当参与运算的操作数都是整数时，表示整数除法；否则，表示浮点除法。

  NOTE: 整数被 0 除将会产生一个异常，但浮点数被 0 除将会得到无穷大或 NaN。

- `%`：取模

- `++`：自增

- `--`：自减

### 10.2. 赋值运算符

- `=`：简单的赋值运算符，将右操作数的值赋给左侧操作数。

- `+=`：加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数。

- `-=`：减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数。

- `*=`：乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数。

- `/=`：除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数。

- `(%)=`：取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数。

- `<<=`：左移位赋值运算符。

- `>>=`：右移位赋值运算符。

- `&=`：按位与赋值运算符。

- `^=`：按位异或赋值操作符	。

- `|=`：按位或赋值操作符。

### 10.3. 关系运算符

- `==`：检查如果两个操作数的值是否相等，如果相等则条件为真。

- `!=`：检查如果两个操作数的值是否相等，如果值不相等则条件为真。

- `>`：检查左操作数的值是否大于右操作数的值，如果是那么条件为真。

- `<`：检查左操作数的值是否小于右操作数的值，如果是那么条件为真。

- `>=`：检查左操作数的值是否大于或等于右操作数的值，如果是那么条件为真。	

- `<=`：检查左操作数的值是否小于或等于右操作数的值，如果是那么条件为真。	

### 10.4. 逻辑运算符

- `&&`：逻辑与运算符。当且仅当两个操作数都为真，条件才为真。

- `||`：逻辑或操作符。如果任何两个操作数任何一个为真，条件为真。

- `！`：逻辑非运算符。用来反转操作数的逻辑状态。如果条件为 true，则逻辑非运算符将得到 false。

### 10.5. 位运算符

- `&`：按位与。

- `|`：按位或。

- `^`：按位异或。

- `~`：按位非。

- `<<`：按位左移运算符。左操作数按位左移右操作数指定的位数。

- `>>`：按位右移运算符。左操作数按位右移右操作数指定的位数。

- `>>>`：按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。

### 10.6. 条件运算符

```
variable x = (expression) ? value if true : value if false
```

## 11. 流程控制

### 11.1. 条件控制

```java
if（布尔表达式 1){
   // 如果布尔表达式 1 的值为 true 执行代码
}else if（布尔表达式 2){
   // 如果布尔表达式 2 的值为 true 执行代码
}else if（布尔表达式 3){
   // 如果布尔表达式 3 的值为 true 执行代码
}else {
   // 如果以上布尔表达式都不为 true 执行代码
}
```

### 11.2. 循环控制

#### 11.2.1. while

```java
while( 布尔表达式 ) {
  // 循环内容
}
```

```java
do {
       // 代码语句
}while（布尔表达式);
```

#### 11.2.2. for

```java
for（初始化；布尔表达式；更新) {
    // 代码语句
}
```

```java
for（声明语句 : 表达式)
{
   // 代码句子
}
```

### 11.3. 中断控制

- break 主要用在循环语句或者 switch 语句中，用来跳出整个语句块。

- continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。

### 11.4. switch

```java
switch(expression){
    case value :
       // 语句
       break; // 可选
    case value :
       // 语句
       break; // 可选
    // 你可以有任意数量的 case 语句
    default : // 可选
       // 语句
}

```

case 标签可以是：
- char、byte、short、int 的常量表达式
- 枚举常量
- 字符串字面值

## 12. 输入输出

### 12.1. 控制台输入输出

- 输入

  - Scanner 对象

    ```java
    Scanner in = new Scanner(System.in);
    String input = in.next();// 读取输入的第一个单词
    String input = in.nextLine();// 读取输入的一行
    int input = in.nextInt();// 读取输入的整型数值
    //...
    ```

  - Console 对象

    ```java
    Console cons = System.console();
    String input = cons.readLine();// 读取输入的一行
    String input = cons.readPassword();// 读取输入的密码，不会在屏幕上显示
    ```

- 输出

  - System.out.print()

  - System.out.println()

  - System.out.printf("<格式化字符串>", <参数表>)
    
    <!-- TODO: http://www.itzhai.com/java-notes-java-in-the-formatted-output-formatter-class-presentation.html#read-more -->

    格式化字符串：
    - int/long/short/byte : 	
      - %d 按无符号十进制整数输出
      - %u 按有符号十进制整数输出
      - %o 按无符号八进制整数输出(不输出前缀0）
      - %x/%X 按无符号十六进制整数输出(不输出前缀0x）
    - double/float :		
      - %f 按定点浮点数输出
      - %e/%E 按指数浮点数（科学计数法）输出
      - %g/%G	按通常浮点数输出（有效位数，如：%8g表示单精度浮点数保留8位有效数字。双精度用lg）
      - %a/%A 按十六进制浮点数输出
    - String：%s（输出字符串中的字符直至字符串中的空字符（字符串以'\0‘结尾，这个'\0'即空字符））
    - char：%c（可以把输入的数字按照ASCII码相应转换为对应的字符）
    - 指针值：%p（以16进制形式输出指针）
    - boolean：%b/%B
    - 时间：%t+转换符（以t开始，以表中人以字母结束的两个字母格式）（详见下）

    - %%		输出百分号
    - %.3f 	保留三位小数
    - %6d  	占6位（默认右对齐）（以空格补全）
    - %06d 	占6位（默认右对齐）（以0补全）
    - %-6d	占6位（左对齐）（以空格补全）
    - %+d  	输出时显示正负号
    - %,f		输出时用“，”分组（如：("%,f", 9999.99)  输出9,999.990000）
    - %#o  	输出时显示前缀o
    - %#x  	输出时显示前缀0x
    - %#e		输出时一定显示小数点
    - %#g		输出时保留尾部的0
    - %(f		输出时用括号包含负数（如：("%(f", -123.321)  输出(123.321000)）
    - %<d  	输出时格式化前一个转换符描述的变量（如： (“%f还有%<.2f”, 99.45) 输出99.450000还有99.45）
    - %i$d  表示第i个变量（若无指定，下一个输出第i+1个变量，可能会越界）

    - 时间日期格式化输出：

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/26/dffd80ab50fd9caf4b6ff16da74ec16f.jpg)

      ```java
      public static void main(String[] args) {
        System.out.printf("%tc\n", new Date());
        System.out.printf("%tF\n", new Date());
        System.out.printf("%ts\n", new Date());
      }
      ```

    用printf判断闰年：
    ```java
    System.out.printf("%s",a%(a%100?4:400)?"NO":"YES");
    ```

### 12.2. 文件输入输出

- 输入

  ```java
  Scanner in = new Scanner(Paths.get("myfile.txt"), "UTF-8");
  ```
- 输出

  ```java
  PrintWriter out = new PrintWriter("myfile.txt", "UTF-8");
  ```

## 13. 字符串

Java 字符串就是 Unicode 字符序列。例如：字符串"java/u2122"由 5 个 Unicode 字符组成，分别是 j、a、v、a、™。

Java 没有设计内置的字符串类型，而是在 Java 标准库中提供了一个预定义类 [String](https://docs.oracle.com/javase/9/docs/api/java/lang/String.html)。每个用双引号括起来的字符串都是 String 类的一个实例。

### 13.1. 子串

`String	substring​(int beginIndex, int endIndex)`：从一个较大的字符串中提取一个子串。子串从 beginIndex 开始，到 endIndex 为止（不包括 endIndex）。
```java
String sub = "hello".substring(1, 3); // "el"
```

### 13.2. 拼接

使用 + 拼接两个字符串，任何类型的值与字符串进行 "+" 运算时，非字符串的值总是被转换为字符串。

`static String	join​(CharSequence delimiter, CharSequence... elements)`：将多个字符串用分隔符进行连接。

### 13.3. 相等判断

使用`String.equals()`方法检测两个字符串的内容是否完全相同，使用`String.equalsIgnoreCase()`进行不区分大小写的判断。

NOTE：使用 `==` 判断的是两个是否指向同一个字符串，而不是判断内容是否相同。

### 13.4. 字符串长度

Java 字符串由 char 值序列组成。`String.length`方法返回采用 UTF-16 编码表示的给定字符串所需要的代码单元数量。

### 13.5. 空串和 Null 串

空串是长度为 0 的字符串对象，写为 `""`，有自己的长度（0）和内容（空）。

Null 串表示没有任何对象与该字符串变量关联，没有长度也没有内容。若对一个 Null 串调用 length 方法，会抛出异常。

一般使用以下条件检查字符串不为 null 也不为空串：
```java
if (str != null && str.length != 0) {//...}
```

### 13.6. 不可变字符串

Java 中将 String 类对象定义为不可变字符串，即**不能修改字符串中的某个字符**（但可以修改字符串变量 a 引用的字符串字面量）。

NOTE: 区分字符串拼接和字符串修改
```java
String str = "hello";
str = str + "world";// 字符串拼接
str += "world";// 字符串拼接
str[2] // error
str = str.substring(0, 2) + "p" + str.substring(3, 5);// "heplo" 字符串修改（实际上仍旧是字符串的拼接）
```

不可变字符串让编译器可以共享字符串，即在公共存储池中存放各种字符串常量，多个字符串变量可以指向同一个字符串常量（只有字符串常量是共享的，`+` 或 `substring()` 产生的临时字符串值并不是共享的）。虽然这样的设计导致**修改字符串时需要进行子串提取、拼接操作**，但共享带来的高效率远胜于多出的操作。因为实际上只有很少的情况需要修改字符串，而对于需要频繁拼接的字符串，Java 提供了 [StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuilder.html) 类。

P.S. 在 C++ 中字符串是可修改的，即可以修改字符串的单个字符。

### 13.7. 字符串构建

由于 Java 字符串不可修改，而每次拼接字符串时都会产生一个新的 String 临时对象。当需要对一个字符串变量进行频繁的拼接时，耗时与空间的浪费就显得非常严重，因此 Java 提供了 [StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuilder.html) 类，用于字符串的构建。

```java
// 创建一个空的字符串构建起
StringBuilder builder = new StringBuilder();
// 每次需要进行字符串拼接时，调用 append 方法
builder.append("xxx");
// 修改字符串中的字符，将第 i 个代码单圈设置为 (char)c
builder.setCharAt(i, 'c');
// 在 offset 位置插入一个字符串
builder.insert(offset, s);
// 构建完成，返回一个 String 对象
String str = builder.toString();
```

P.S. [StringBuffer](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuffer.html) 类是 StringBuilder 类的前身，两者 API 基本上完全相同，效率稍低，但允许采用多线程的方式执行添加或删除字符的操作。但若所有字符串在单线程中编译（通常情况都是这样），应使用 StringBuilder 进行字符串的构建。

## 14. 数组

数组是一种用于存储同一类型的值集合的数据结构。

Java 中对数组的操作主要通过工具类 [Arrays](https://docs.oracle.com/javase/9/docs/api/java/util/Arrays.html) 完成。

### 14.1. 创建与初始化

数组的声明 / 创建：
```java
int [] a;
int a[];
```

数组的初始化：
```java
a = new int[100]; // 固定长度初始化
a = {2,3,4,56,7,78,57}; // 直接赋值初始化
a= new int[] {2,3,4,56,7,78,57}; // 使用匿名数组进行初始化
```

NOTE：
- 在 Java 中不要求数组长度是常量，`new int[n]`是完全合法的。
- 在 Java 中允许数组长度为 0。
- 初始化数组后，数值数值所有元素默认值为 0，boolean 数组所有元素默认值为 false，对象数组所有元素默认值为 null。
- 数组创建初始化后，便无法再改变长度。若需要在运行过程中扩展数组的大小，应使用数组列表 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html)。
- 数组的声明 / 创建可与初始化同时完成，如：`int [] a = new int[n];`。

### 14.2. 多维数组

```java
int [][] a = new int[X](Y);
int [][] b = new int[X][];// 先创建一维数组
int [][] b = new int[][Y];//error，必须从低维到高维创建
int [][] c = {// 创建不规则数组
  {1,2,3},
  {4,5},
  {6,7,8,9}
};
```
- NOTE: 不同于 C++ 中存在真正的多维数组，在 Java 中实际上没有真正的多维数组，只有一维数组。

  ```java
  //java
  int [][] a = new int[10](6);
  ```
  在 C++ 中相当于创建了一个包含 10 个指针的数组，然后再为每个数组元素填充一个包含 6 个数字的数组：
  ```cpp
  //cpp
  // 不等价
  int a[10](6);
  // 不等价
  int (*a)[6] = new int[10](6); 
  // 等价
  int **a = new int*[10];
  for (i = 0; i < 10; i++) {
    a[i] = new int[6];
  }
  ```

### 14.3. 数组拷贝

在 Java 中，数组的拷贝主要通过`Arrays.copyOf​(U [] original, int newLength)`完成，其中 original 为要复制的原数组，newLength 为要返回的数组副本的长度（若长度小于原数组长度，则只拷贝前部分的元素；若长度大小原数组的长度，则多出的部分赋默认值）。

`Arrays.copyOf`常用于数组扩容：
```java
int [] originalArray = {1,2,3,4,5};
int [] newArray = Arrays.copyOf(originalArray, 2 * originalArray.length);
```

NOTE：使用`System.arraycopy()`方法同样可完成数组的拷贝，两者的不同在于：
- `System.arraycopy()` 调用其它语言编写的底层函数，完成数组拷贝。
- `Arrays.copyOf` 实际上是对 `System.arraycopy()` 的二次封装，可以选择拷贝的起点和长度以及放入新数组中的位置。

### 14.4. 数组填充

通过`Arrays.fill​(type [] a, type val)`方法，可将数组的所有元素设置为 val 值。

### 14.5. 快速排序

通过`Arrays.sort​(type [] a, int fromIndex, int toIndex)`方法，可对数值型数组进行优化的快速排序，其中 type 可为 byte、char、double、float、int、long、short。

### 14.6. 二分查找

通过`Arrays.binarySearch​(type [] a, int fromIndex, int toIndex, type key)`方法，可对数组进行二分查找。若查找成功，返回相应下标值；否则，返回一个负数值 r（`-r-1` 是为了保持原数组 a 有序，key 值应插入的位置）。
