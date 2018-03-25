- [Java I/O 编码](#java-io-%E7%BC%96%E7%A0%81)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [1.1. 为什么要编码](#11-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%BC%96%E7%A0%81)
    - [1.2. 如何编码](#12-%E5%A6%82%E4%BD%95%E7%BC%96%E7%A0%81)
  - [2. Java 中的编码](#2-java-%E4%B8%AD%E7%9A%84%E7%BC%96%E7%A0%81)
  - [3. Refer Links](#3-refer-links)

# Java I/O 编码

## 1. 基本概念

### 1.1. 为什么要编码

在计算机的世界，所有的信息都是 0/1 组合的二进制序列，计算机是无法直接识别和存储字符的。因此，字符只有按照一定规则编码，最终表示为 0/1 二进制序列的形式，才能被计算机处理。

由于人类的语言有太多，表示这些语言的符号太多，无法用计算机中一个基本的存储单元 byte 来表示（1byte 能表示的字符范围是 0~255 个），因而必须要对语言符号进行拆分或一些翻译工作，以存储在多个 byte 中，让计算机能“理解”。这个拆分 / 翻译的过程，就是**编码**。

### 1.2. 如何编码

常见的编码格式有 ASCII、ISO-8859-1、GB2312、GBK、UTF-8、UTF-16 等，它们都可以被看作为字典，定义了转化 / 翻译的规则，按照这个规则就可以让计算机正确的表示我们人类语言的字符。

目前的编码格式有很多，例如 GB2312、GBK、UTF-8、UTF-16 这几种格式都可以表示一个汉字，那我们到底选择哪种编码格式来存储汉字呢？这就要考虑到其它因素了，是存储空间重要还是编码的效率重要？根据这些因素来正确选择编码格式。

常见编码格式：
- ASCII
  
  在计算机刚出现的时候只能传输英文字符，这里的传输包括是显示和存储，前面提到要进行编码存储，既然要编码就需要一张表来表示 A 是什么，B 是什么，就好比摩斯密码中的密码本一样。那时的“码表”也就是编码方式叫做 ASCII。

  ASCII 码总共有 128 个，用一个字节的低 7 位表示，0~31 是控制字符如换行回车删除等；32~126 是打印字符，可以通过键盘输入并且能够显示出来。

- ANSI
  
  计算机继续在发展，需要发展到其他国家和地区，此时就需要对汉字、日文、韩文等进行编码，但原有的 ASCII 肯定不能满足，它的设计是包括了英文和符号，此时就出现了 ANSI 编码（也叫做 ASCII 扩展）。
  
  ANSI 编码实际上是一种本地化的规范编码，例如在中文操作系统中 ANSI 代表的就是 GB2312 编码（当然也有它的扩展叫做 GBK 编码），在日文操作系统中 ANSI 代表的就是 JIS 等。

  ANSI 编码采用 2 个字节来表示一个字符（范围在 0x80-0xFF），两个字节也就是 16 个二进制位，理论上可以表示 216 个字符，当然这需要减去 0x00-0x79 这个范围，这就能表示很多很多的字符了。GB2312 编码也就才表示了 6000 多个常用汉字。不过这种编码方式还是带来了新的问题，这只是做了本地化，也就是说在 GB2312 的编码环境下，无法对日文进行编码。所以还需要做国际化。

- UNICODE

  随着计算机的继续发展，国际化越来越重要这当然也就包括编码方式的改变，为避免 ANSI 不兼容的状况，又制定了新的编码规则 UNICODE。在 Java 中使用的就是 UNICODE 编码，符合 Java 跨平台的特性。
  
  Unicode 是一种编码规范，是为解决全球字符通用编码而设计的，而 UTF-8、UTF-16 等是这种规范的一种具体实现。

  - UTF-16
    
    UTF-16 具体定义了 Unicode 字符在计算机中存取方法，即以 2 个字节表示 1 个字符（因此在 Java 中 char 字符的数据类型占用 2 个字节）。这是一种定长的表示方法，也就是说不论什么字符都用两个字节表示，两个字节是 16 个 bit，所以叫 UTF-16。这个在字符串操作时就大大简化了操作，这也是 **Java 以 UTF-16 作为内存的字符存储格式**的一个很重要的原因。

    UNICODE 解决了不同语言在不同平台不兼容的情况，但也有一个小小的弊端，也就是稍微比前面两种要占空间，以 UNICODE 字符集在内存中存储的字符串我们称之为为“宽字节字符串”，实际上之后对于字符编码的工作就集中在了如何缩短字节空间上。UNICODE 编码之所以略占空间，是因为它使用 2 个字节来表示 1 个字符。就算是英文也是使用 2 个字节。而 ACSII 和 ANSI 可以使用 1 个字节表示英文。

  - UTF-8

    由于 UTF-16 给任意字符都是采用的 2 个字节表示 1 个字符，会造成空间浪费，在现在的网络带宽还非常有限的今天，无疑会增大网络传输的流量。因此，在 UNICODE 编码基础上，又出现了可变长编码的 UTF-8 编码，这种编码方式会灵活地进行字符的空间分配，不同字符所占用的内存空间不相同，在保证兼容性的同时，也保证了空间的最合理使用。

    UTF-8 有以下编码规则：
    - 如果一个字节，最高位（第 8 位）为 0，表示这是一个 ASCII 字符（00 - 7F）。可见，所有 ASCII 编码已经是 UTF-8 了。
    - 如果一个字节，以 11 开头，连续的 1 的个数暗示这个字符的字节数，例如：110xxxxx 代表它是双字节 UTF-8 字符的首字节。
    - 如果一个字节，以 10 开始，表示它不是首字节，需要向前查找才能得到当前字符的首字节。

## 2. Java 中的编码

在 Java 中，编码实际上就是从 byte 到 char 的过程，而译码就是从 char 到 byte 的过程。

- Java 中需要进行 byte 和 char 转换的主要场景就是在 I/O 的时候，因此在 java.io 包下包含了一系列字节流和字符流的操作实现类：

  InputStream 抽象类和 OutputStream 抽象类是 Java 中字节输入输出实现的父类，Reader 抽象类和 Writer 抽象类是 Java 中字符输入输出实现的父类，而 InputStreamReader 类和 OutputStreamWriter 类就是关联字节到字符的桥梁，分别负责在 I/O 过程中处理读取字节到字符的转换和输出字符到字节的转换。

- Java 中的 Charset 类提供的 encode() 与 decode() 方法分别对应 char[] 到 byte[] 的编码和 byte[] 到 char[] 的解码：
  ```java
  Charset charset = Charset.forName("UTF-8"); 
  ByteBuffer byteBuffer = charset.encode(string); 
  CharBuffer charBuffer = charset.decode(byteBuffer);
  ```

- Java 中用 String 表示字符串，所以 String 类也提供了转换到字节的 getBytes() 方法，也支持将字节转换为字符串的构造函数：
  ```java
  String s = "这是一段中文字符串"; 
  byte[] b = s.getBytes("UTF-8"); 
  String n = new String(b, "UTF-8");
  ```

  例：将“I am 君山”这个字符串以 ISO-8859-1、GB2312、GBK、UTF-16、UTF-8 编码格式进行编码。
  ```java
  public static void encode() { 
      String name = "I am 君山"; 
      toHex(name.toCharArray()); 
      try { 
          byte[] iso8859 = name.getBytes("ISO-8859-1"); 
          toHex(iso8859); 
          byte[] gb2312 = name.getBytes("GB2312"); 
          toHex(gb2312); 
          byte[] gbk = name.getBytes("GBK"); 
          toHex(gbk); 
          byte[] utf16 = name.getBytes("UTF-16"); 
          toHex(utf16); 
          byte[] utf8 = name.getBytes("UTF-8"); 
          toHex(utf8); 
      } catch (UnsupportedEncodingException e) { 
          e.printStackTrace(); 
      } 
  }
  ```

- Java 中还有一个 ByteBuffer 类，它提供一种 char 和 byte 之间的软转换，它们之间转换不需要编码与解码，只是把一个 16bit 的 char 格式，拆分成为 2 个 8bit 的 byte 表示，它们的实际值并没有被修改，仅仅是数据的类型做了转换：
  ```java
  ByteBuffer heapByteBuffer = ByteBuffer.allocate(1024); 
  ByteBuffer byteBuffer = heapByteBuffer.putChar(c);
  ```

## 3. Refer Links

[Java IO（1）基础知识——字节与字符](https://www.cnblogs.com/yulinfeng/p/7896470.html)

[深入分析 Java 中的中文编码问题](https://www.ibm.com/developerworks/cn/java/j-lo-chinesecoding/)