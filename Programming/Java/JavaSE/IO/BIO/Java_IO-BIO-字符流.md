- [Java IO BIO 字符流](#java-io-bio-%E5%AD%97%E7%AC%A6%E6%B5%81)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 体系结构](#2-%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84)
		- [2.1. 字符输入流](#21-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%85%A5%E6%B5%81)
			- [2.1.1. 字符输入节点流](#211-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%85%A5%E8%8A%82%E7%82%B9%E6%B5%81)
			- [2.1.2. 字符输入处理流](#212-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%85%A5%E5%A4%84%E7%90%86%E6%B5%81)
		- [2.2. 字符输出流](#22-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%87%BA%E6%B5%81)
			- [2.2.1. 字符输出节点流](#221-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%87%BA%E8%8A%82%E7%82%B9%E6%B5%81)
			- [2.2.2. 字符输出处理流](#222-%E5%AD%97%E7%AC%A6%E8%BE%93%E5%87%BA%E5%A4%84%E7%90%86%E6%B5%81)
	- [3. Reader 抽象类](#3-reader-%E6%8A%BD%E8%B1%A1%E7%B1%BB)
		- [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
	- [4. Writer 抽象类](#4-writer-%E6%8A%BD%E8%B1%A1%E7%B1%BB)
		- [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
	- [5. Refer Links](#5-refer-links)

# Java IO BIO 字符流

## 1. 基本概念

Java IO 字符流是指在输入输出的过程中，操作以 2 个字节即 1 个字符作为数据单元的 IO 流。对于字节流的输入顶层类是 Reader、其输出对应的顶层类是 Writer。

**字符流的操作与字节流几乎完全一样，不同之处仅在于操作的数据单元不同**。读取文本文件时，字节流读取出来是一个字节，想要输出显示这个字节则需要将这个字节转换为字符；而字符流读取出来的文件则直接就是字符，不需要再重新转化。

## 2. 体系结构

### 2.1. 字符输入流

![image](http://img.cdn.firejq.com/jpg/2018/3/22/b3ab2a51abc4f8a13e90289ae1951f2a.jpg)

Reader: JavaIO 中的顶级的字符读取的抽象类，定义了最基础的读取方法。实现了 Readable 和 Closeable 接口。

#### 2.1.1. 字符输入节点流

- InputStreamReader: 继承自 Reader，用于将字节输入流转换成字符输入流。是字节流通向字符流的桥梁。如果不指定字符集编码，该解码过程将使用平台默认的字符编码。

  - FileReader: 继承自 InputStreamReader，用来读取字符文件的便捷类。

- CharArrayReader: 继承自 Reader 的字符数组输入流类。

- PipedReader: 继承自 Reader 的字符管道输入流类。作用是可以通过管道进行线程间的通讯。必须和 PipedWriter 配合使用。

- StringReader: 继承自 Reader，用于字符串读取的字符流。

#### 2.1.2. 字符输入处理流

- BufferedReader: 继承自 Reader 的带缓冲功能的字符流类，默认缓冲区大小是 8K，从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。创建 BufferReader 时，我们会通过它的构造函数指定某个 Reader 为参数。BufferReader 会将该 Reader 中的数据分批读取，每次读取一部分到缓冲中；操作完缓冲中的这部分数据之后，再从 Reader 中读取下一部分的数据。

  - LineNumberReader: 继承自 BufferedReader，可以获取当前的行号或设置当前行号。

- FilterReader: 继承自 Reader 的字符过滤输入流抽象类。

  - PushbackReader: 继承自 FilterReader 的字符回退输入流类。

### 2.2. 字符输出流

![image](http://img.cdn.firejq.com/jpg/2018/3/22/74bd27aa7cc105f9bc5a53cc16017f45.jpg)

Writer: JavaIO 中的顶级的字符写入的抽象类，定义了最基础的写入方法。实现了 Appendable 、 Closeable 和 Flushable 接口。

#### 2.2.1. 字符输出节点流

- OutputStreamWriter: 继承自 Reader，用于将字节输出流转换成字符输出流。是字节流通向字符流的桥梁。如果不指定字符集编码，该解码过程将使用平台默认的字符编码。

  - FileWriter: 继承自 OutputStreamWriter，用来向文件中写入字符的便捷类。

- CharArrayWriter: 继承自 Writer 的字符数组输出流类。

- PipedWriter: 继承自 Writer 的字符管道输出流类。作用是可以通过管道进行线程间的通讯。必须和 PipedReader 配合使用。

- StringWriter: 继承自 Writer，用于字符串写入的字符流。

#### 2.2.2. 字符输出处理流

- BufferedWriter: 继承自 Writer 的带缓冲功能的字符流类，默认缓冲区大小是 8K，从字符输出流中写入字符到文本中，缓冲各个字符，从而实现字符、数组和行的高效写入。

- FilterWriter: 继承自 Writer 的字符过滤输出流抽象类。与 FilterOutputStream 功能一样、只是简单重写了父类的方法、目的是为所有装饰类提供标准和基本的方法、要求子类必须实现核心方法、和拥有自己的特色。这里 FilterWriter 没有子类、可能其意义只是提供一个接口、留着以后的扩展，本身是一个抽象类。

- PrintWriter: 继承自 Writer 的打印写入类，提供了 PrintStream 的所有打印方法，其方法也从不抛出 IOException。与 PrintStream 的区别：作为处理流使用时，PrintStream 只能封装 OutputStream 类型的字节流，而 PrintWriter 既可以封装 OutputStream 类型的字节流，还能够封装 Writer 类型的字符输出流并增强其功能。

## 3. Reader 抽象类

### 3.1. 基本概念

[Reader](https://docs.oracle.com/javase/9/docs/api/java/io/Reader.html) 抽象类是所有输入字符流的基类。

### 3.2. 类定义

```java
public abstract class Reader
extends Object
implements Readable, Closeable
```

### 3.3. 常用 API

输入字符流 Reader 的基本操作与输入字节流 InputStream 的基本操作几乎相同，区别在于操作的数据单元不同。

- `int read()`: 从流中读取数据，读取一个字符，**返回值为所读得的字符所对应的 int 值**。
- `int read(char cbuf[])`: 从流中读取数据，最多读取 cbuf.length 个字符的数据，放置到字符数组 cbuf 中，**返回值为实际独取的字符的数量**。
- `abstract int	read​(char[] cbuf, int off, int len)`: 从流中读取数据，最多读取 len 个字符的数据，放置到以下标 off 开始字符数组 cbuf 中，**返回值为实际读取的字符的数量**。
- `int read(java.nio.CharBuffer target)`: 试图读取字符入指定的字符缓冲区。

- `long skip(long n)`: 跳过 n 个字符。
- `boolean ready()`: 通知此流是否已准备好被读取。
- `boolean markSupported()`: 告诉此流是否支持 mark() 操作。
- `void mark(int readAheadLimit)`: 标记流中的当前位置。
- `void reset()`: 重置流。
- `void close()`: 关闭该流并释放与之关联的所有系统资源。

	NOTE：与 JDBC 编程一样，程序中打开的文件 IO 资源不属于内存里的资源，因此 GC 无法将其回收，必须在程序中显式关闭文件 IO 资源。在 JDK7 中，所有 IO 资源类都实现了 AutoCloseable 接口，因此可通过 try 语句自动关闭 IO 资源，无须再进行显式的关闭。

	eg:
	```java
	try(
		// 使用自动关闭 IO 资源的 try 语句，可以保证输入流一定会被关闭
		FileReader fileReader = new FileReader("xxx.java");
	) {
		char [] cbuf = new char[32];
		int hasRead = 0;
		while ((hasRead = fileReader.read(cbuf)) > 0) {
			System.out.println(new String(cbuf, 0, hasRead));
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	```

## 4. Writer 抽象类

### 4.1. 基本概念

[Writer](https://docs.oracle.com/javase/9/docs/api/java/io/Writer.html) 是所有输出字符流的抽象基类。

### 4.2. 类定义

```java
public abstract class Writer
extends Object
implements Appendable, Closeable, Flushable
```

### 4.3. 常用 API

输出字符流 Writer 的基本操作与输出字节流 OutputStream 的基本操作几乎相同，区别在于操作的数据单元不同。

- `void write(int c)`: 写入单个字符（参数 c 是 ASCII 表中的码值，不是普通数字）。
- `void write(char cbuf[])`: 写入字符数组。
- `abstract void write(char cbuf[], int off, int len)`: 写入字符数组的一部分。
- `void write(String str)`: 写入一个字符串。
- `void write(String str, int off, int len)`: 写入一个字符串的一部分。

- `Writer append(CharSequence csq)`: 将指定的字符序列追加写到 writer 中。
- `Writer append(CharSequence csq, int start, int end)`: 将指定的字符序列的子序列追加写入此 writer。
- `Writer append(char c)`: 将指定字符追加到这个 writer。

- `abstract void flush()`: 刷新流。
- `abstract void close()`: 关闭流，但要先刷新它。

## 5. Refer Links

[Java IO 知识整理](http://blinkfox.com/java-io-zhi-shi-zheng-li/)

[JAVA IO 分析一：File 类、字节流、字符流、字节字符转换流](https://www.cnblogs.com/pony1223/p/8030200.html)
