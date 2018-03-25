- [Java IO BIO 缓冲操作处理流](#java-io-bio-%E7%BC%93%E5%86%B2%E6%93%8D%E4%BD%9C%E5%A4%84%E7%90%86%E6%B5%81)
	- [1. 为什么要有缓冲处理流？](#1-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9C%89%E7%BC%93%E5%86%B2%E5%A4%84%E7%90%86%E6%B5%81%EF%BC%9F)
	- [2. BufferedInputStream](#2-bufferedinputstream)
		- [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [2.2. 类定义](#22-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
	- [3. BufferedOutputStream](#3-bufferedoutputstream)
		- [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
		- [3.4. 应用](#34-%E5%BA%94%E7%94%A8)
	- [4. BufferedReader](#4-bufferedreader)
		- [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
	- [5. BufferedWriter](#5-bufferedwriter)
		- [5.1. 基本概念](#51-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [5.2. 类定义](#52-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [5.3. 常用 API](#53-%E5%B8%B8%E7%94%A8-api)
		- [5.4. 应用](#54-%E5%BA%94%E7%94%A8)
	- [6. Refer Links](#6-refer-links)
    
# Java IO BIO 缓冲操作处理流

## 1. 为什么要有缓冲处理流？

不带缓冲的操作，每读一个字节就要写入一个字节，由于涉及磁盘的 IO 操作相比内存的操作要慢很多，所以不带缓冲的流效率很低。带缓冲的流，可以一次读很多字节，但不向磁盘中写入，只是先放到内存里。等凑够了缓冲区大小的时候一次性写入磁盘，这种方式可以减少磁盘操作次数，读写读写的操作速度。

在 Java 中，BufferedInputStream/BufferedOutputStream、BufferedReader/BufferedWriter 就是实现了缓冲功能的输入流 / 输出流，使用它们可以防止每次读取 / 发送数据时进行实际的写操作，效率更高，速度更快。因此在实际开发时，对于较大文件的复制，推荐使用缓冲流而不是普通输入输出流。

## 2. BufferedInputStream

### 2.1. 基本概念

[BufferedInputStream](https://docs.oracle.com/javase/9/docs/api/java/io/BufferedInputStream.html) 是 Java IO 中的缓冲输出字节处理流，继承于 FilterInputStream。

### 2.2. 类定义

BufferedInputStream 本质上是通过一个内部缓冲区数组实现的。例如，在新建某输入流对应的 BufferedInputStream 后，当我们通过 read() 读取输入流的数据时，BufferedInputStream 会将该输入流的数据分批的填入到缓冲区中。每当缓冲区中的数据被读完之后，输入流会再次填充数据缓冲区；如此反复，直到我们读完输入流数据位置。

类定义：
```java
public class BufferedInputStream
extends FilterInputStream
```

内部属性：
```java
// 内置缓存字节数组的大小 8KB
private static int defaultBufferSize = 8192;
// 内置缓存字节数组
protected volatile byte buf[];  
// 当前 buf 中的字节总数，注意不是底层字节输入流的源中字节总数
protected int count;    
// 当前 buf 中下一个被读取的字节下标
protected int pos;      
// 最后一次调用 mark(int readLimit) 方法记录的 buf 中下一个被读取的字节的位置
protected int markpos = -1; 
// 调用 mark 后、在后续调用 reset() 方法失败之前云寻的从 in 中读取的最大数据量、用于限制被标记后 buffer 的最大值
protected int marklimit;    
```

### 2.3. 常用 API

构造方法：
- `BufferedInputStream(InputStream in)`: 使用默认 buf 大小 8kb，底层字节输入流构建 BufferedInputStream。
- `BufferedInputStream(InputStream in, int size)`: 使用指定 buf 大小、底层字节输入流构建 BufferedInputStream。

常用方法：
- `int available()`: 返回底层流对应的源中有效可供读取的字节数。      

- `void close()`: 关闭此流、释放与此流有关的所有资源。  

- `boolean markSupport()`: 查看此流是否支持 mark。

- `void mark(int readLimit)`: 标记当前 buf 中读取下一个字节的下标。  

- `int read()`: 读取 buf 中下一个字节。  

- `int read(byte[] b, int off, int len)`: 读取 buf 中下一个字节。

- `void reset()`: 重置最后一次调用 mark 标记的 buf 中的位子。  

- `long skip(long n)`: 跳过 n 个字节、 不仅仅是 buf 中的有效字节、也包括 in 的源中的字节。 

## 3. BufferedOutputStream

### 3.1. 基本概念

BufferedOutputStream 是 Java IO 中的缓冲输出字节处理流，继承于 FilterOutputStream。通过建立这样一个缓冲输出流，应用程序可以将字节写入底层输出流，而不必为每一个字节编写一个对底层系统的调用，从而提高文件的写入效率。

### 3.2. 类定义

类定义：
```java
public class BufferedOutputStream
extends FilterOutputStream
```

内部属性：
```java
// 内部缓冲字节数组
protected byte[]	buf;	
// The number of valid bytes in the buffer.
protected int	count;	
```

### 3.3. 常用 API

构造方法：
- `BufferedOutputStream​(OutputStream out)`: 使用默认大小 8kb，底层字节输出流构造 BufferedOutputStream。
- `BufferedOutputStream​(OutputStream out, int size)`: 使用指定大小，底层字节输出流构造 BufferedOutputStream。

常用方法：
- `void	flush​()`: Flushes this buffered output stream.
- `void	write​(byte[] b, int off, int len)`: Writes len bytes from the specified byte array starting at offset off to this buffered output stream.
- `void	write​(int b)`: Writes the specified byte to this buffered output stream.

NOTE: 
- `BufferedOutputStream`没有自己的`close()`方法，当他调用父类`FilterOutputStrem`的方法关闭时，会间接调用自己实现的`flush()`方法将 buf 中残存的字节 flush 到 out 中，再`out.flush()`到目的地中。
- 满足以下条件之一，会触发内存缓存区向磁盘写入：
	- 调用了 BufferedOutputStream 的 close() 方法，在关闭 IO 资源时自动触发磁盘写入。
	- BufferedOutputStream 内部的缓存区数组容量已满（默认大小为 8192 字节即 8kb），自动触发磁盘写入。
	- 调用了 BufferedOutputStream 的 flush() 方法，手动触发磁盘写入。

### 3.4. 应用

例：使用缓冲字节流进行文件拷贝
```java
public static void copyFile( File oldFile , File newFile){
        try (
            BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(oldFile));
            BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(newFile));
				) {
            byte[] b=new byte[1024];   // 代表一次最多读取 1KB 的内容
            int length = 0 ; // 代表实际读取的字节数
            while((length = bufferedInputStream.read(b))!= -1) {
                bufferedOutputStream.write(b, 0, length);
            }
            // 将缓冲区的内容写入到文件
            bufferedOutputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
				}
}
```

## 4. BufferedReader

### 4.1. 基本概念

[BufferedReader](https://docs.oracle.com/javase/9/docs/api/java/io/BufferedReader.html)

### 4.2. 类定义

```java
public class BufferedReader
extends Reader
```

### 4.3. 常用 API

构造方法：
- `BufferedReader​(Reader in)`: 创建一个使用指定大小输入缓冲区的缓冲字符输入流。 
- `BufferedReader​(Reader in, int sz)`: 创建一个使用默认大小输入缓冲区的缓冲字符输入流。

常用方法：
- `int  read()`: 读取单个字符。
- `int  read(char[] cbuf, int off, int len)`: 将字符读入数组的某一部分。
- `String  readLine()`: 读取一个文本行。
- `boolean  ready()`: 判断此流是否已准备好被读取。
- `void  reset()`: 将流重置到最新的标记。
- `long  skip(long n)`: 跳过字符。
- `void  close()`: 关闭该流并释放与之关联的所有资源。
- `void  mark(int readAheadLimit)`: 标记流中的当前位置。
- `boolean  markSupported()`: 判断此流是否支持 mark() 操作（它一定支持）。

## 5. BufferedWriter

### 5.1. 基本概念

[BufferedWriter](https://docs.oracle.com/javase/9/docs/api/java/io/BufferedWriter.html)

### 5.2. 类定义

```java
public class BufferedWriter
extends Writer
```

### 5.3. 常用 API

构造方法：
- `BufferedWriter(Writer out, int sz)`: 创建一个使用给定大小输出缓冲区的新缓冲字符输出流。
- `BufferedWriter(Writer out)`: 建一个使用默认大小输出缓冲区的缓冲字符输出流。

常用方法：
- `void  close()`: 关闭此流，但要先刷新它。
- `void  flush()`: 刷新该流的缓冲。
- `void  newLine()`: 写入一个行分隔符。
- `void  write(char[] cbuf, int off, int len)`: 写入字符数组的某一部分。
- `void  write(int c)`: 写入单个字符。
- `void  write(String s, int off, int len)`: 写入字符串的某一部分。

### 5.4. 应用

例：用缓冲字符流进行文本文件拷贝
```java
private static void copyFile(File oldFile , File newFile) {
		try (
				BufferedReader bufferedReader = new BufferedReader(new FileReader(oldFile));
				BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(newFile));
		) {
				String result = null ; // 每次读取一行的内容
				while ((result = bufferedReader.readLine()) != null ) {
						bufferedWriter.write(result);  // 把内容写入文件
						bufferedWriter.newLine();  // 换行，result 是一行数据，所以没写一行就要换行 
				}
				bufferedWriter.flush();  // 将缓冲区内容写入文件
		} catch (IOException e) {
				e.printStackTrace();
		}
}
```

## 6. Refer Links

[Java IO 流学习总结三：缓冲流 - BufferedInputStream、BufferedOutputStream](https://blog.csdn.net/zhaoyanjun6/article/details/54894451)

[Java IO 流学习总结四：缓冲流 - BufferedReader、BufferedWriter](https://blog.csdn.net/zhaoyanjun6/article/details/54911237)