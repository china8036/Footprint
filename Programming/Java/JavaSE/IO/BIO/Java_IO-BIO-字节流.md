- [Java IO BIO 字节流](#java-io-bio-%E5%AD%97%E8%8A%82%E6%B5%81)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 体系结构](#2-%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84)
		- [2.1. 字节输入流](#21-%E5%AD%97%E8%8A%82%E8%BE%93%E5%85%A5%E6%B5%81)
			- [2.1.1. 字节输入节点流](#211-%E5%AD%97%E8%8A%82%E8%BE%93%E5%85%A5%E8%8A%82%E7%82%B9%E6%B5%81)
			- [2.1.2. 字节输入处理流](#212-%E5%AD%97%E8%8A%82%E8%BE%93%E5%85%A5%E5%A4%84%E7%90%86%E6%B5%81)
		- [2.2. 字节输出流](#22-%E5%AD%97%E8%8A%82%E8%BE%93%E5%87%BA%E6%B5%81)
			- [2.2.1. 字节输出节点流](#221-%E5%AD%97%E8%8A%82%E8%BE%93%E5%87%BA%E8%8A%82%E7%82%B9%E6%B5%81)
			- [2.2.2. 字节输出处理流](#222-%E5%AD%97%E8%8A%82%E8%BE%93%E5%87%BA%E5%A4%84%E7%90%86%E6%B5%81)
	- [3. InputStream 抽象类](#3-inputstream-%E6%8A%BD%E8%B1%A1%E7%B1%BB)
		- [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
	- [4. OutputStream 抽象类](#4-outputstream-%E6%8A%BD%E8%B1%A1%E7%B1%BB)
		- [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
	- [5. Refer Links](#5-refer-links)

# Java IO BIO 字节流

## 1. 基本概念

Java IO 字节流是指在输入输出的过程中，操作以 1 个字节作为数据单元的 IO 流。对于字节流的输入顶层类是 InputStram、其输出对应的顶层类是 OutputStream。

## 2. 体系结构

### 2.1. 字节输入流

![image](http://img.cdn.firejq.com/jpg/2018/5/25/18a349bab0b81330ce3e4ad842008e39.jpg)

`InputStream`: Java IO 中的顶级的字节输入流的抽象基类，定义了最基础的输入、读取的相关方法。实现了 Closeable 接口。

#### 2.1.1. 字节输入节点流

- `FileInputStream`: 继承自 InputStream 的文件输入流类，用于从本地文件中读取字节数据。
- `ByteArrayInputStream`: 继承自 InputStream 的字节数组输入流类，它包含一个内部缓冲区，该缓冲区包含从流中读取的字节；通俗点说，它的内部缓冲区就是一个字节数组，而 ByteArrayInputStream 本质就是通过字节数组来实现的。InputStream 通过 read() 向外提供接口，供它们来读取字节数据；而 ByteArrayInputStream 的内部额外的定义了一个计数器，它被用来跟踪 read() 方法要读取的下一个字节。
- `PipedInputStream`: 继承自 InputStream 的管道输入流类，在使用管道通信时，必须与 PipedOutputStream 配合使用。让多线程可以通过管道进行线程间的通讯。

#### 2.1.2. 字节输入处理流

- `ObjectInputStream`:

	继承自 InputStream 的对象输入流类，实现了 ObjectInput 和 ObjectStreamConstants 接口。作用是从输入流中读取 Java 对象和基本数据。只有支持 Serializable 或 Externalizable 接口的对象才能被 ObjectInputStream/ObjectOutputStream 所操作！

- `SequenceInputStream`:

	继承自 InputStream 的输入合并流类。SequenceInputStream 会将与之相连接的流集组合成一个输入流并从第一个输入流开始读取，直到到达文件末尾，接着从第二个输入流读取，依次类推，直到到达包含的最后一个输入流的文件末 尾为止。合并流的作用是将多个源合并合一个源。

- `AudioInputStream`:

	继承自 InputStream 的音频输入流类。音频输入流是具有指定音频格式和长度的输入流。长度用示例帧表示，不用字节表示。提供几种方法，用于从流读取一定数量的字节，或未指定数量的字节。音频输入流跟踪所读取的最后一个字节。可以跳过任意数量的字节以到达稍后的读取位置。音频输入流可支持标记。设置标记时，会记住当前位置，以便可以稍后返回到该位置。

- `FilterInputStream`:

	继承自 InputStream 的过滤输入流类（装饰器超类），用来“封装其它的输入流，并为它们提供额外的功能”。

	- `BufferedInputStream`:

		继承自 FilterInputStream 的带缓冲区功能的输入流类（装饰器子类），默认缓冲区大小是 8K，能够减少访问磁盘的次数，提高文件读取性能。

	- `DataInputStream`:

		继承自 FilterInputStream 的数据输入流类，实现了 DataInput 接口。它允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型。

	- `PushbackInputStream`:

		继承自 FilterInputStream 的回退输入流类。允许试探性的读取数据流，如果不是我们想要的则返还回去。

	- `CheckedInputStream`:

		继承自 FilterInputStream 的校验输入流类。

	- `CipherInputStream`:

		继承自 FilterInputStream 的密钥输入流类。

	- `DigestInputStream`:

		继承自 FilterInputStream 的摘要处理输入流类。

	- `InflaterInputStream`:

		继承自 FilterInputStream 的解压缩处理输入流类。
		- `GZIPInputStream`: 继承自 InflaterInputStream 的 gzip 文件处理输入流类。
		- `ZipInputStream`: 继承自 InflaterInputStream 的解压缩处理输入流类。
		- `JarInputStream`: 继承自 ZipInputStream 的解压缩处理输入流类。

	- `DeflaterInputStream`:

		继承自 FilterInputStream 的压缩数据输入流类。

	- `ProgressMonitorInputStream`:

		继承自 FilterInputStream 的进度监控输入流类。

### 2.2. 字节输出流

![image](http://img.cdn.firejq.com/jpg/2018/3/22/95c53279caebb171256e10f32f0eff4e.jpg)

`OutputStream`: JavaIO 中的顶级的字节输出流的抽象基类，定义了最基础的输出、写入的相关方法。实现了 Closeable 和 Flushable 接口。

#### 2.2.1. 字节输出节点流

- `FileOutputStream`:

	继承自 OutputStream 的文件输出流类，用于向本地文件中写入字节数据。

- `ByteArrayOutputStream`:

	继承自 OutputStream 的字节数组输出流类，ByteArrayOutputStream 中的数据会被写入一个 byte 数组。缓冲区会随着数据的不断写入而自动增长。可使用 toByteArray() 和 toString() 获取数据。

- `PipedOutputStream`:

	继承自 OutputStream 的管道输出流类，在使用管道通信时，必须与 PipedInputStream 配合使用。让多线程可以通过管道进行线程间的通讯。

#### 2.2.2. 字节输出处理流

- `ObjectOutputStream`: 继承自 OutputStream 的对象输出流类，实现了 ObjectOutput 和 ObjectStreamConstants 接口。作用是把 Java 对象和基本数据写入到对象输出流中。只有支持 Serializable 或 Externalizable 接口的对象才能被 ObjectInputStream/ObjectOutputStream 所操作！

- `FilterOutputStream`: 继承自 OutputStream 的过滤输出流类，是用来“封装其它的输出流，并为它们提供额外的功能”。

	- `BufferedOutputStream`: 继承自 FilterOutputStream 的带缓冲区功能的输出流类，默认缓冲区大小是 8K，能够提高文件的写入效率。

	- `DataOutputStream`: 继承自 FilterOutputStream 的数据输出流类，实现了 DataOutput 接口。它允许应用程序以与机器无关方式向底层输入流中写入基本 Java 数据类型。

	- `PrintStream`: 继承自 FilterOutputStream 的打印输出流类，实现了 Appendable 和 Closeable 接口。使它们能够方便地打印各种数据值表示形式。PrintStream 永远不会抛出 IOException。PrintStream 提供了自动 flush 和 字符集设置功能。所谓自动 flush，就是往 PrintStream 写入的数据会立刻调用 flush() 函数。

	- `CheckedOutputStream`: 继承自 FilterOutputStream 的校验输出流类。

	- `CipherOutputStream`: 继承自 FilterOutputStream 的密钥输出流类。

	- `DigestOutputStream`: 继承自 FilterOutputStream 的摘要处理输出流类。

	- `InflaterOutputStream`: 继承自 FilterOutputStream 的解压缩处理输出流类。

	- `DeflaterOutputStream`: 继承自 FilterOutputStream 的解压缩数据输出流类。
		- `GZIPOutputStream`: 继承自 DeflaterOutputStream 的 gzip 文件解压缩输出流类。
		- `ZipOutputStream`: 继承自 DeflaterOutputStream 的 zip 文件解压缩输出流类。
		- `JarOutputStream`: 继承自 ZipOutputStream 的 zip 文件解压缩输出流类。

## 3. InputStream 抽象类

### 3.1. 基本概念

[InputStream](https://docs.oracle.com/javase/9/docs/api/java/io/InputStream.html) 是所有输入字节流的抽象基类。因为存在许多输入流（例如网络、文件等），这些都能为程序提供数据源，而不同的数据源则通过不同的 InputStream 子类来接收。

### 3.2. 类定义

```java
public abstract class InputStream
extends Object
implements Closeable
```

### 3.3. 常用 API

- `abstract int read()`: 从流中读取数据，读取一个字节，**返回值为所读得的字节所对应的 int 值 (0-255)**。
- `int read(byte b[])`: 从流中读取数据，最多读取 b.length 个字节的数据，放置到字节数组 b 中，**返回值为实际独取的字节的数量**。
- `int read(byte b[], int off, int len)`: 从流中读取数据，最多读取 len 个字节的数据，放置到以下标 off 开始字节数组 b 中，**返回值为实际读取的字节的数量**。

NOTE：其中 read() 返回的是读入的一个字节所对应的 int 值 (0-255), 而 read(byte[] b) 和 read(byte[] b, int off, int len) 返回的是读入的字节数。

- `long skip(long n)`: 读指针跳过 n 个字节不读，返回值为实际跳过的字节数量。
- `int available()`: 返回值为流中尚未读取的字节的数量。
- `void close()`: 关闭输入流。

	NOTE：与 JDBC 编程一样，程序中打开的文件 IO 资源不属于内存里的资源，因此 GC 无法将其回收，必须在程序中显式关闭文件 IO 资源。在 JDK7 中，所有 IO 资源类都实现了 AutoCloseable 接口，因此可通过 try 语句自动关闭 IO 资源，无须再进行显式的关闭。

	eg:
	```java
	try(
		// 使用自动关闭 IO 资源的 try 语句，可以保证输入流一定会被关闭
		FileInputStream fileInputStream = new FileInputStream("xxx.java");
	) {
		byte [] b = new byte[32];
		int hasRead = 0;
		while ((hasRead = fileInputStream.read(b)) > 0) {
			// xxxxxx
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	```

- `synchronized void mark(int readlimit)`: 记录当前指针的所在位置，readlimit 表示读指针读出的 readlimit 个字节后，所标记的指针位置才实效。
- `synchronized void reset()`: 把读指针重新指向用 mark 方法所记录的位置。
- `boolean markSupported()`: 当前的流是否支持读指针的记录功能。

## 4. OutputStream 抽象类

### 4.1. 基本概念

[OutputStream](https://docs.oracle.com/javase/9/docs/api/java/io/OutputStream.html) 是所有输出字节流的抽象基类。

### 4.2. 类定义

```java
public abstract class OutputStream
extends Object
implements Closeable, Flushable
```

### 4.3. 常用 API

- `abstract void write(int b)`: 输出数据，往流中写一个字节 b（参数 b 是 ASCII 表中的码值，不是普通数字）。
- `void write(byte b[])`: 输出数据，往流中写一个字节数组 b。
- `void write(byte b[], int off, int len)`: 输出数据，把字节数组 b 中从下标 off 开始，长度为 len 的字节写入到流中。

- `void flush()`: 刷空输出流，并输出所有被缓存的字节。由于某些流支持缓存功能，该方法将把缓存中所有内容强制输出到流中。
- `void close()`: 关闭输出流。

## 5. Refer Links

[Java IO 知识整理](http://blinkfox.com/java-io-zhi-shi-zheng-li/)

[JAVA IO 分析一：File 类、字节流、字符流、字节字符转换流](https://www.cnblogs.com/pony1223/p/8030200.html)

[Java IO（2）阻塞式输入输出（BIO） - OKevin - 博客园](https://www.cnblogs.com/yulinfeng/p/7995559.html)