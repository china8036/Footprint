- [Java 基础语法](#java-基础语法)
  - [1. Java 概述](#1-java-概述)
    - [1.1. 发展历史](#11-发展历史)
    - [1.2. 语言特性](#12-语言特性)
  - [2. JDK 概述与安装](#2-jdk-概述与安装)
    - [2.1. JDK 概述](#21-jdk-概述)
    - [2.2. 安装 JDK](#22-安装-jdk)
      - [2.2.1. Windows](#221-windows)
      - [2.2.2. Ubuntu](#222-ubuntu)
      - [2.2.3. MacOS](#223-macos)
    - [2.3. JDK Document](#23-jdk-document)
    - [2.4. 获取源码包](#24-获取源码包)
  - [3. Java 程序： Application 与 Applet 的区别](#3-java-程序-application-与-applet-的区别)
    - [3.1. Application](#31-application)
    - [3.2. Applet](#32-applet)
  - [4. 注释](#4-注释)
    - [4.1. 行注释](#41-行注释)
    - [4.2. 块注释](#42-块注释)
    - [4.3. 文档注释](#43-文档注释)
  - [5. 变量 & 常量](#5-变量--常量)
    - [5.1. 变量](#51-变量)
    - [5.2. 常量](#52-常量)
  - [6. 运算符](#6-运算符)
    - [6.1. 算术运算符](#61-算术运算符)
    - [6.2. 赋值运算符](#62-赋值运算符)
    - [6.3. 关系运算符](#63-关系运算符)
    - [6.4. 逻辑运算符](#64-逻辑运算符)
    - [6.5. 位运算符](#65-位运算符)
    - [6.6. 条件运算符](#66-条件运算符)
  - [7. 流程控制](#7-流程控制)
    - [7.1. 条件控制](#71-条件控制)
    - [7.2. 循环控制](#72-循环控制)
      - [7.2.1. while](#721-while)
      - [7.2.2. for](#722-for)
    - [7.3. 中断控制](#73-中断控制)
    - [7.4. switch](#74-switch)
  - [8. 输入输出](#8-输入输出)
    - [8.1. 控制台输入输出](#81-控制台输入输出)
    - [8.2. 文件输入输出](#82-文件输入输出)

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
- 2018 年 3 月 21 日，Oracle 公司发表 Java SE 10
- 2018 年 9 月 25 日，Java SE 11 发布

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

#### 2.2.3. MacOS

[JDK 8 Installation for OS X](https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html)

在 Oracle 官网下载 JDK 并安装，在 MacOS 下会自动安装到 `/Library/Java/JavaVirtualMachines/` 下。

[How can I change Mac OS's default Java VM returned from /usr/libexec/java_home?](https://stackoverflow.com/questions/17885494/how-can-i-change-mac-oss-default-java-vm-returned-from-usr-libexec-java-home)

[java_home and JAVA_HOME on macOS](https://medium.com/notes-for-geeks/java-home-and-java-home-on-macos-f246cab643bd)

[管理多个 Java 版本](http://mclspace.com/2015/03/10/mac_java_home/)

在 `.zshrc` 中添加：
```
#java
#use jdk8
export JAVA_8_HOME=`/usr/libexec/java_home -v '1.8*'`
#use jdk11
export JAVA_11_HOME=`/usr/libexec/java_home -v '11*'`
#default use jdk8
export JAVA_HOME=$JAVA_8_HOME
#change version alias
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk11="export JAVA_HOME=$JAVA_11_HOME"
```

### 2.3. JDK Document

- [JDK 7 Documentation](https://docs.oracle.com/javase/7/docs/index.html)

- [JDK 8 Documentation](https://docs.oracle.com/javase/8/)

- [JDK 9 Documentation](https://docs.oracle.com/javase/9/)

- [JDK 10 Documentation](https://docs.oracle.com/javase/10/)

- [JDK 11 Documentation](https://docs.oracle.com/en/java/javase/11/)

### 2.4. 获取源码包

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

## 6. 运算符

### 6.1. 算术运算符

- `+`：加法

- `-`：减法

- `*`：乘法

- `/`：除法

  当参与运算的操作数都是整数时，表示整数除法；否则，表示浮点除法。

  NOTE: 整数被 0 除将会产生一个异常，但浮点数被 0 除将会得到无穷大或 NaN。

- `%`：取模

- `++`：自增

- `--`：自减

### 6.2. 赋值运算符

- `=`：简单的赋值运算符，将右操作数的值赋给左侧操作数。

- `+=`：加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数。

- `-=`：减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数。

- `*=`：乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数。

- `/=`：除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数。

- `(%)=`：取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数。

- `<<=`：左移位赋值运算符。

- `>>=`：右移位赋值运算符。

- `&=`：按位与赋值运算符。

- `^=`：按位异或赋值操作符。

- `|=`：按位或赋值操作符。

### 6.3. 关系运算符

- `==`：检查如果两个操作数的值是否相等，如果相等则条件为真。

- `!=`：检查如果两个操作数的值是否相等，如果值不相等则条件为真。

- `>`：检查左操作数的值是否大于右操作数的值，如果是那么条件为真。

- `<`：检查左操作数的值是否小于右操作数的值，如果是那么条件为真。

- `>=`：检查左操作数的值是否大于或等于右操作数的值，如果是那么条件为真。

- `<=`：检查左操作数的值是否小于或等于右操作数的值，如果是那么条件为真。

NOTE: **在 Java 中，关系运算符可用于 char 类型变量，但不可用于 String/boolean 类型变量**。

### 6.4. 逻辑运算符

- `&&`：逻辑与运算符。当且仅当两个操作数都为真，条件才为真。

- `||`：逻辑或操作符。如果任何两个操作数任何一个为真，条件为真。

- `！`：逻辑非运算符。用来反转操作数的逻辑状态。如果条件为 true，则逻辑非运算符将得到 false。

### 6.5. 位运算符

- `&`：按位与。

- `|`：按位或。

- `^`：按位异或。

- `~`：按位非。

- `<<`：按位左移运算符。左操作数按位左移右操作数指定的位数。

- `>>`：按位右移运算符。左操作数按位右移右操作数指定的位数。

- `>>>`：按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。

### 6.6. 条件运算符

```
variable x = (expression) ? value if true : value if false
```

## 7. 流程控制

### 7.1. 条件控制

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

### 7.2. 循环控制

#### 7.2.1. while

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

#### 7.2.2. for

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

### 7.3. 中断控制

- break 主要用在循环语句或者 switch 语句中，用来跳出整个语句块。

- continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。

### 7.4. switch

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

## 8. 输入输出

### 8.1. 控制台输入输出

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

  - System.out.printf("《格式化字符串》", 《参数表》)

    <!-- TODO: http://www.itzhai.com/java-notes-java-in-the-formatted-output-formatter-class-presentation.html#read-more -->

    格式化字符串：
    - int/long/short/byte :
      - %d 按无符号十进制整数输出
      - %u 按有符号十进制整数输出
      - %o 按无符号八进制整数输出（不输出前缀 0）
      - %x/%X 按无符号十六进制整数输出（不输出前缀 0x）
    - double/float :
      - %f 按定点浮点数输出
      - %e/%E 按指数浮点数（科学计数法）输出
      - %g/%G	按通常浮点数输出（有效位数，如：%8g 表示单精度浮点数保留 8 位有效数字。双精度用 lg）
      - %a/%A 按十六进制浮点数输出
    - String：%s（输出字符串中的字符直至字符串中的空字符（字符串以'\0‘结尾，这个'\0'即空字符））
    - char：%c（可以把输入的数字按照 ASCII 码相应转换为对应的字符）
    - 指针值：%p（以 16 进制形式输出指针）
    - boolean：%b/%B
    - 时间：%t+ 转换符（以 t 开始，以表中人以字母结束的两个字母格式）（详见下）

    - %%		输出百分号
    - %.3f 	保留三位小数
    - %6d  	占 6 位（默认右对齐）（以空格补全）
    - %06d 	占 6 位（默认右对齐）（以 0 补全）
    - %-6d	占 6 位（左对齐）（以空格补全）
    - %+d  	输出时显示正负号
    - %,f		输出时用“，”分组（如：("%,f", 9999.99)  输出 9,999.990000）
    - %#o  	输出时显示前缀 o
    - %#x  	输出时显示前缀 0x
    - %#e		输出时一定显示小数点
    - %#g		输出时保留尾部的 0
    - %(f		输出时用括号包含负数（如：("%(f", -123.321)  输出 (123.321000)）
    - %<d  	输出时格式化前一个转换符描述的变量（如： (“%f 还有 %<.2f”, 99.45) 输出 99.450000 还有 99.45）
    - %i$d  表示第 i 个变量（若无指定，下一个输出第 i+1 个变量，可能会越界）

    - 时间日期格式化输出：

      ![image](http://img.cdn.firejq.com/jpg/2018/1/26/dffd80ab50fd9caf4b6ff16da74ec16f.jpg)

      ```java
      public static void main(String[] args) {
        System.out.printf("%tc\n", new Date());
        System.out.printf("%tF\n", new Date());
        System.out.printf("%ts\n", new Date());
      }
      ```

    用 printf 判断闰年：
    ```java
    System.out.printf("%s",a%(a%100?4:400)?"NO":"YES");
    ```

### 8.2. 文件输入输出

- 输入

  ```java
  Scanner in = new Scanner(Paths.get("myfile.txt"), "UTF-8");
  ```
- 输出

  ```java
  PrintWriter out = new PrintWriter("myfile.txt", "UTF-8");
  ```