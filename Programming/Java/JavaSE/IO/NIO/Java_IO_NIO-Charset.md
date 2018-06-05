- [Java IO NIO: Charset](#java-io-nio--charset)
  - [1. Charset](#1-charset)
    - [1.1. 初始化](#11)
    - [1.2. 主要操作](#12)
  - [2. CharsetDecoder](#2-charsetdecoder)
  - [3. CharsetEncoder](#3-charsetencoder)
  - [4. StandardCharsets](#4-standardcharsets)
  - [5. Refer Links](#5-refer-links)

# Java IO NIO: Charset

Java 在 java.nio.charset 包下提供了用于字节序列与字符序列转换处理的相关类，其中包含了解码器 CharsetDecoder(byte -> char)、编码器 Charsetencoder(char -> byte) 以及各种字符集的支持 Charset/StandardCharsets。

## 1. Charset

### 1.1. 初始化

[Charset](https://docs.oracle.com/javase/9/docs/api/java/nio/charset/Charset.html) 类没有提供公有构造方法，但提供了一个静态方法 availableCharsets() 来获取当前 JDK 支持的所有字符集对象：
- `static SortedMap<String,Charset>	availableCharsets​()`: Constructs a sorted map from canonical charset names to charset objects.

  P.S. 可以通过 `System.getProperties(file.encoding)` 来获取本地系统的文件编码格式。

若已知字符集别名，则可以通过静态方法 forName() 来构造一个字符集对象：
- `static Charset	forName​(String charsetName)`: Returns a charset object for the named charset.
  ```java
  Charset csCn = Charset.forName​("GBK");
  ```

### 1.2. 主要操作

获取了 Charset 对象后，可通过该对象的 API 获取该字符集的编码器和解码器，通过它们的 decode() 和 encode() 方法进行编解码操作：
- `CharsetDecoder	newDecoder​()`: Constructs a new decoder for this charset.
- `CharsetEncoder	newEncoder​()`: Constructs a new encoder for this charset.

实际上，如果仅需要进行简单的 Unicode 字符集的编解码操作，可直接调用 Charset 对象的 encode() 和 decode() 方法，而无须创建 CharsetDecoder 和 CharsetEncoder 对象：
- `ByteBuffer	encode​(String str)`: Convenience method that encodes a string into bytes in this charset.
- `ByteBuffer	encode​(CharBuffer cb)`: Convenience method that encodes Unicode characters into bytes in this charset.
- `CharBuffer	decode​(ByteBuffer bb)`: Convenience method that decodes bytes in this charset into Unicode characters.

## 2. CharsetDecoder

[CharsetDecoder](https://docs.oracle.com/javase/9/docs/api/java/nio/charset/CharsetDecoder.html) 提供了 decode() 方法，用于将 ByteBuffer 转化为 CharBuffer。
- `CharBuffer	decode​(ByteBuffer in)`: Convenience method that decodes the remaining content of a single input byte buffer into a newly-allocated character buffer.

## 3. CharsetEncoder

[CharsetEncoder](https://docs.oracle.com/javase/9/docs/api/java/nio/charset/CharsetEncoder.html) 提供了 encode() 方法，用于将 CharBuffer 转化为 ByteBuffer。
- `ByteBuffer	decode​(CharBuffer in)`: Convenience method that encodes the remaining content of a single input character buffer into a newly-allocated byte buffer.

## 4. StandardCharsets

[StandardCharsets](https://docs.oracle.com/javase/9/docs/api/java/nio/charset/StandardCharsets.html) 类是 Java 7 新增的类，该类的类变量包含了最常用的字符集对应的 Charset 对象：IOS_8859_1、UTF_8、UTF_16 等。

## 5. Refer Links
