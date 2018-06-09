- [Java IO NIO: Channel](#java-io-nio--channel)
	- [1. 类谱图](#1)
	- [2. 初始化](#2)
	- [3. 文件通道](#3)
		- [3.1. 读写操作](#31)
		- [3.2. 数据转移](#32)
			- [3.2.1. API](#321-api)
			- [3.2.2. 实现分析](#322)
		- [3.3. 内存映射](#33)
			- [3.3.1. API](#331-api)
			- [3.3.2. 实现分析](#332)
		- [3.4. 文件锁](#34)
	- [4. 套接字通道](#4)
		- [4.1. SelectableChannel 抽象类](#41-selectablechannel)
		- [4.2. ServerSocketChannel 抽象类](#42-serversocketchannel)
			- [4.2.1. 初始化](#421)
			- [4.2.2. 主要操作](#422)
		- [4.3. SocketChannel 抽象类](#43-socketchannel)
			- [4.3.1. 初始化](#431)
			- [4.3.2. 读写操作](#432)
			- [4.3.3. 非阻塞模式](#433)
		- [4.4. DatagramChannel 抽象类](#44-datagramchannel)
			- [4.4.1. 初始化](#441)
			- [4.4.2. 收发数据](#442)
	- [5. 管道通道](#5)
	- [6. Refer Links](#6-refer-links)

# Java IO NIO: Channel

通道 Channel 是 Java NIO 的核心内容之一，它类似于传统的流对象，但与传统流对象存在以下区别：
- 传统的流式 IO 数据只能单向流动，而通道中的数据可以双向流动，既可以读，也可以写。
- 传统的流式 IO 数据只能按字节进行处理，而通道可以直接将指定文件的部分或全部直接映射成 Buffer 进行处理。
- 在使用上，通道需和 Buffer 配合完成读写等操作，Channel 只能和 Buffer 进行交互，程序不能直接访问 Channel 中的数据。

## 1. 类谱图

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/3/1567af06fe34a963056f434e893649b6.jpg)

java.nio.channels.[Channel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Channel.html) 是所有通道的公共接口：
```java
public interface Channel extends Closeable {
    public boolean isOpen();
    public void close() throws IOException;
}
```

Java NIO 在 java.nio.channels 包下为 Channel 接口提供了一系列通道实现类，这些实现类可按照功能进行划分：
- 用于线程间通信的管道 Channel
  - [Pipe.SinkChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Pipe.SinkChannel.html)
  - [Pipe.SourceChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Pipe.SourceChannel.html)
- 用于 TCP 网络通信的 Channel
  - [ServerSocketChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/ServerSocketChannel.html)
  - [SocketChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/SocketChannel.html)
- 用于 UDP 网络通信的 Channel
  - [DatagramChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/DatagramChannel.html)
- 用于文件操作的 Channel
  - [FileChannel]((https://docs.oracle.com/javase/9/docs/api/java/nio/channels/FileChannel.html)

## 2. 初始化

所有的 Channel 都没有提供公有构造器，但可通过其它方法创建 Channel 实例对象：

- 在 InputStream/OutputStream/RandomAccessFile 中提供了 getChannel() 方法来返回对应的 FileChannel 实例：
  ```java
  // FileInputStream
  public FileChannel getChannel() {
      synchronized (this) {
          if (channel == null) {
              // FileChannelImpl 是 sun.misc 提供的 FileChannel 扩展类
              channel = FileChannelImpl.open(fd, path, true, false, this);
          }
          return channel;
      }
  }
  // FileOutputStream
  public FileChannel getChannel() {
      synchronized (this) {
          if (channel == null) {
              channel = FileChannelImpl.open(fd, path, false, true, append, this);
          }
          return channel;
      }
  }
  // RandomAccessFile
  public final FileChannel getChannel() {
      synchronized (this) {
          if (channel == null) {
              channel = FileChannelImpl.open(fd, path, true, rw, this);
          }
          return channel;
      }
  }
  ```

- 所有 Channel 抽象类都提供了静态方法 open()，用于创建一个没有绑定具体资源的 XxxChannel 实例：
  ```java
  // ServerSocketChannel
  public static ServerSocketChannel open() throws IOException {
      return SelectorProvider.provider().openServerSocketChannel();
  }
  // SocketChannel
  public static SocketChannel open() throws IOException {
      return SelectorProvider.provider().openSocketChannel();
  }
  // DatagramChannel
  public static DatagramChannel open() throws IOException {
      return SelectorProvider.provider().openDatagramChannel();
  }
  // Pipe
  public static Pipe open() throws IOException {
      return SelectorProvider.provider().openPipe();
  }
  // FileChannel
  public static FileChannel open(Path path,
                                  Set<? extends OpenOption> options,
                                  FileAttribute<?>... attrs) throws IOException {
      FileSystemProvider provider = path.getFileSystem().provider();
      return provider.newFileChannel(path, options, attrs);
  }
  ```

## 3. 文件通道

Java NIO 用 [FileChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/FileChannel.html) 表示与 FileInputStream/FileOutputStream 对应的文件通道。

### 3.1. 读写操作

- read: read() 方法有一系列的重载形式，用于从 Channel 中读取数据到 ByteBuffer 中。
  - `abstract int	read​(ByteBuffer dst)`: Reads a sequence of bytes from this channel into the given buffer.
  - `long	read​(ByteBuffer[] dsts)`: Reads a sequence of bytes from this channel into the given buffers.
  - `abstract long	read​(ByteBuffer[] dsts, int offset, int length)`: Reads a sequence of bytes from this channel into a subsequence of the given buffers.
  - `abstract int	read​(ByteBuffer dst, long position)`: Reads a sequence of bytes from this channel into the given buffer, starting at the given file position.

- write: write() 方法同样有一系列的重载形式，用于将 ByteBuffer 数据写入到 Channel 中。
  - `abstract int	write​(ByteBuffer src)`: Writes a sequence of bytes to this channel from the given buffer.
  - `long	write​(ByteBuffer[] srcs)`: Writes a sequence of bytes to this channel from the given buffers.
  - `abstract long	write​(ByteBuffer[] srcs, int offset, int length)`: Writes a sequence of bytes to this channel from a subsequence of the given buffers.
  - `abstract int	write​(ByteBuffer src, long position)`: Writes a sequence of bytes to this channel from the given buffer, starting at the given file position.

eg:
```java
// 获取管道
RandomAccessFile raf = new RandomAccessFile(FILE_PATH, "rw");
FileChannel rafChannel = raf.getChannel();

// 准备数据
String data = "新数据，时间： " + System.currentTimeMillis();
System.out.println("原数据：\n" + "   " + data);
ByteBuffer buffer = ByteBuffer.allocate(128);
buffer.clear();
buffer.put(data.getBytes());
buffer.flip();

// 写入数据
rafChannel.write(buffer);

rafChannel.close();
raf.close();

// 重新打开管道
raf = new RandomAccessFile(FILE_PATH, "rw");
rafChannel = raf.getChannel();

// 读取刚刚写入的数据
buffer.clear();
rafChannel.read(buffer);

// 打印读取出的数据
buffer.flip();
byte[] bytes = new byte[buffer.limit()];
buffer.get(bytes);
System.out.println("读取到的数据：\n" + "   " + new String(bytes));

rafChannel.close();
raf.close();
```

### 3.2. 数据转移

当需要将一个文件中的内容复制到另一个文件中去时，最容易想到的做法是利用传统的 IO 将源文件中的内容读取到内存中，然后再往目标文件中写入。而在 Java NIO 中，FileChannel 提供了一对数据转移方法，通过使用这两个方法，可大幅度地简化文件复制操作。

#### 3.2.1. API

- `long	transferFrom​(ReadableByteChannel src, long position, long count)`: Transfers bytes into this channel's file from the given readable byte channel.
- `long	transferTo​(long position, long count, WritableByteChannel target)`: Transfers bytes from this channel's file to the given writable byte channel.

eg:
```java
try ( FileChannel fromChannel = new RandomAccessFile("fromFile.txt", "rw").getChannel();
      FileChannel toChannel = new RandomAccessFile("toFile.txt", "rw").getChannel()) {
    long position = 0;
    long count = fromChannel.size();
    fromChannel.transferTo(position, count, toChannel);
    // toChannel.transferFrom​(toChannel, position, count);
}    
```

#### 3.2.2. 实现分析

```java
// openjdk/jdk/src/share/classes/sun/nio/ch/FileChannelImpl.java
public long transferTo(long position, long count,
                           WritableByteChannel target)
        throws IOException
{
    // 省略一些代码
    int icount = (int)Math.min(count, Integer.MAX_VALUE);
    if ((sz - position) < icount)
        icount = (int)(sz - position);

    long n;

    // Attempt a direct transfer, if the kernel supports it
    if ((n = transferToDirectly(position, icount, target)) >= 0)
        return n;

    // Attempt a mapped transfer, but only to trusted channel types
    if ((n = transferToTrustedChannel(position, icount, target)) >= 0)
        return n;

    // Slow path for untrusted targets
    return transferToArbitraryChannel(position, icount, target);
}
    
private long transferToDirectly(long position, int icount,
                                WritableByteChannel target)
    throws IOException
{
    // 省略一些代码
    long n = -1;
    int ti = -1;
    try {
        begin();
        ti = threads.add();
        if (!isOpen())
            return -1;
        do {
            n = transferTo0(thisFDVal, position, icount, targetFDVal); // native method
        } while ((n == IOStatus.INTERRUPTED) && isOpen());
        
        // 省略一些代码
        
        return IOStatus.normalize(n);
    } finally {
        threads.remove(ti);
        end (n > -1);
    }
}
```
```c
// openjdk/jdk/src/solaris/native/sun/nio/ch/FileChannelImpl.c
JNIEXPORT jlong JNICALL
Java_sun_nio_ch_FileChannelImpl_transferTo0(JNIEnv *env, jobject this,
                                            jint srcFD,
                                            jlong position, jlong count,
                                            jint dstFD)
{
#if defined(__linux__)
    off64_t offset = (off64_t)position;
    
    jlong n = sendfile64(dstFD, srcFD, &offset, (size_t)count); // transferTo0 最终调用了 sendfile64 函数
    if (n < 0) {
        if (errno == EAGAIN)
            return IOS_UNAVAILABLE;
        if ((errno == EINVAL) && ((ssize_t)count >= 0))
            return IOS_UNSUPPORTED_CASE;
        if (errno == EINTR) {
            return IOS_INTERRUPTED;
        }
        JNU_ThrowIOExceptionWithLastError(env, "Transfer failed");
        return IOS_THROWN;
    }
    return n;

// 其他平台的代码省略
#endif
}
```
在传统的文件拷贝操作中，Java 程序需要通过系统调用 read，先将文件数据从磁盘拷贝到内核态内存空间中，再进一步从内核态内存拷贝到用户态内存，同样，写入数据也是如此。也就是说，Java 程序与磁盘文件的 IO 交互都需要通过内核态内存进行数据中转。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/3/baf0b77df2980afa11f4731568cb4bc2.jpg)

从上述代码可知，Java NIO 中 FileChannel 的 transferTo 方法与传统文件 IO 方式不同，该方法底层基于 sendfile64（Linux 平台下）系统调用实现。sendfile64 会直接在内核空间内进行数据拷贝，免去了内核往用户空间拷贝，用户空间再往内核空间拷贝这两步操作，因此提高了效率。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/3/5e744ede243c4df816d6d29f648878aa.jpg)

### 3.3. 内存映射

**内存映射文件 (memory map)**是使部分或整个磁盘文件与内存空间中的一个缓冲区建立映射关系，然后当从缓冲区中取数据，就相当于读文件中的相应字节；而将数据存入缓冲区，就相当于写文件中的相应字节。虽然最终内存里的数据还是要写到文件里，但这一步操作交给了操作系统去控制，从而提高了 IO 效率。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/4/b1cdad416bf78da0927d153e1e529856.jpg)

#### 3.3.1. API

Java NIO 在 FileChannel 中提供了内存映射操作的支持：
- `MappedByteBuffer	map​(FileChannel.MapMode mode, long position, long size)`: 将 Channel 对应的部分或全部数据映射到内存中的 MappedByteBuffer 中。
  - 参数 FileChannel.MapMode mode 用于指定执行映射时的模式：
    - `static FileChannel.MapMode	READ_ONLY`: Mode for a read-only mapping. 试图修改得到的缓冲区将导致抛出 ReadOnlyBufferException。
    - `static FileChannel.MapMode	READ_WRITE`: Mode for a read/write mapping. 对得到的缓冲区的更改最终将传播到文件；该更改对映射到同一文件的其他程序不一定是可见的。
    - `static FileChannel.MapMode	PRIVATE`: Mode for a private (copy-on-write) mapping.  对得到的缓冲区的更改不会传播到文件，并且该更改对映射到同一文件的其他程序也不是可见的。
  - 参数 position 和 size 用于指定将 Channel 的哪些数据映射成 ByteBuffer。

eg: 从标准输入中获取数据，然后将数据通过内存映射缓存写入到文件中
```java
// 从标准输入获取数据
Scanner sc = new Scanner(System.in);
System.out.println("请输入：");
String str = sc.nextLine();
byte[] bytes = str.getBytes();
try (FileChannel channel = new RandomAccessFile("map.txt", "rw").getChannel()) {
    // 获取内存映射缓冲区，并向缓冲区写入数据
    MappedByteBuffer mappedBuffer = channel.map(MapMode.READ_WRITE, 0, bytes.length);
    mappedBuffer.put(bytes);
}
```

eg: 利用内存映射实现进程间通信

我们可以在两个 Java 进程中各使用一次 map 将文件映射到内存，这样两个进程就可以直接通过这个共享内存来实现进程间的数据通信了。
```java
// 进程 1
public static void main(String args[]) {
		try (FileChannel fc = new RandomAccessFile("hello.txt", "rw").getChannel()) {
				MappedByteBuffer buf = fc.map(FileChannel.MapMode.READ_WRITE, 0, 20);
				buf.put("how are you?".getBytes());
				Thread.sleep(10000);
		} catch (Exception e) {
				e.printStackTrace();
		}
}
// 进程 2：在进程 2 中可以读到进程 1 往 ByteBuffer 里写的内容
public static void main(String[] args) throws Exception {
		try (FileChannel fc = new RandomAccessFile("hello.txt", "rw").getChannel()) {
				MappedByteBuffer buf = fc.map(FileChannel.MapMode.READ_WRITE, 0, fc.size());
				// 输出 how are you?
				while (buf.hasRemaining()) {
						System.out.print((char)buf.get());
				}
		}
}
```

#### 3.3.2. 实现分析

```java
// sun.nio.ch.FileChannelImpl.java
try {
		// If no exception was thrown from map0, the address is valid
		addr = map0(imode, mapPosition, mapSize);
} // ...

private native long map0(int prot, long position, long length)
```
```c
// solaris/native/sun/nio/ch/FileChannelImpl.c
JNIEXPORT jlong JNICALL
Java_sun_nio_ch_FileChannelImpl_map0(JNIEnv *env, jobject this,
                                     jint prot, jlong off, jlong len)
{
    void *mapAddress = 0;
    jobject fdo = (*env)->GetObjectField(env, this, chan_fd);
    jint fd = fdval(env, fdo);
    int protections = 0;
    int flags = 0;

    if (prot == sun_nio_ch_FileChannelImpl_MAP_RO) {
        protections = PROT_READ;
        flags = MAP_SHARED;
    } else if (prot == sun_nio_ch_FileChannelImpl_MAP_RW) {
        protections = PROT_WRITE | PROT_READ;
        flags = MAP_SHARED;
    } else if (prot == sun_nio_ch_FileChannelImpl_MAP_PV) {
        protections =  PROT_WRITE | PROT_READ;
        flags = MAP_PRIVATE;
    }
		// 系统调用 mmap64
    mapAddress = mmap64(
        0,                    /* Let OS decide location */
        len,                  /* Number of bytes to map */
        protections,          /* File permissions */
        flags,                /* Changes are shared */
        fd,                   /* File descriptor of mapped file */
        off);                 /* Offset into file */

    if (mapAddress == MAP_FAILED) {
        if (errno == ENOMEM) {
            JNU_ThrowOutOfMemoryError(env, "Map failed");
            return IOS_THROWN;
        }
        return handle(env, -1, "Map failed");
    }

    return ((jlong) (unsigned long) mapAddress);
}
```
可见，Java NIO 中提供了 map() 方法，在 JNI 中实际上是使用了 `void *mmap(void *start, size_t length, int prot, int flags, int fd, off_t offset)` 这个系统调用实现的。

### 3.4. 文件锁

Java 在 NIO 中使用 FileLock 类来提高对文件锁的支持，通过 FileChannel 对象的以下方法即可获得 FileLock 实例：
- `FileLock	lock​()`: 锁定整个文件，若无法获得文件锁，该方法会一直阻塞。
- `FileLock	lock​(long position, long size, boolean shared)`: 只锁定文件指定的部分内容而不是整个文件，若无法获得文件锁，该方法会一直阻塞。
- `FileLock	tryLock​()`: 锁定整个文件，若无法获得文件锁，该方法会立即返回 null。
- `FileLock	tryLock​(long position, long size, boolean shared)`: 只锁定文件指定的部分内容而不是整个文件，若无法获得文件锁，该方法会立即返回 null。

当参数 shared 为 true 时，获取的锁为共享锁，否则为排他锁（默认情况）。

当文件处理完毕后，可通过 FileLock 对象的 release() 方法释放文件锁。

NOTE：
- 与操作系统提供的文件锁不同，FileLock 代表的文件锁是由 JVM 所持有的。因此，若 2 个 Java 程序在不同的 JVM 中运行，则 2 个程序可能会对同一个文件同时加锁。
- 在某些平台上，文件锁是建议性而不是强制性的，即一个程序即使没有获取文件锁依旧能对文件进行操作。

## 4. 套接字通道

为开发高性能的网络服务器，Java NIO 在 java.nio.channel 包下提供了基于 IO 多路复用的非阻塞式 API，可以让服务器端使用一个或有限的几个线程来同时处理所有连接到服务器的大量客户端。其中，主要包括了在传统套接字基础上进行改造的套接字通道：基于 TCP 协议的 ServerSocketChannel 抽象类和 SocketChannel 抽象类、基于 UDP 协议的 DatagramChannel 抽象类。

### 4.1. SelectableChannel 抽象类

> A channel that can be multiplexed via a Selector.

[SelectableChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/SelectableChannel.html) 抽象类是所有可用于 IO 多路复用器 Selector 的基类，它提供了多个用于实现基于 IO 多路复用的非阻塞 IO 的方法：
- `SelectableChannel	configureBlocking​(boolean block)`: 设置 Channel 是否运行在阻塞模式下，若设置为 false，则 Channel 调用 connect()，read() 和 write() 等方法时都会立即返回而不会阻塞线程。
- `boolean	isBlocking​()`: Tells whether or not every I/O operation on this channel will block until it completes.
- `SelectionKey	register​(Selector sel, int ops)`: 将 Channel 注册到指定的 IO 复用器 Selector 上，并返回注册后的 SelectorKey。
  - 参数 ops 指定了需要 Selector 监听的 IO 操作，在 SelectionKey 类中用静态变量定义了 4 种 IO 操作：
    ```java
    // Operation-set bit for read operations.
    public static final int OP_READ = 1 << 0;
    // Operation-set bit for write operations.
    public static final int OP_WRITE = 1 << 2;
    // Operation-set bit for socket-connect operations.
    public static final int OP_CONNECT = 1 << 3;
    // Operation-set bit for socket-accept operations.
    public static final int OP_ACCEPT = 1 << 4;
    ```
    可见，这 4 种操作以 int 整数存在，值分别为 1、4、8、16，且存在以下规律：
    - 任意 2 个、3 个、4 个进行按位或的结果和相加的结果相等。
    - 任意 2 个、3 个、4 个进行相加的结果互不相等。
    因此，系统可以根据 validOps() 方法返回的相加值来确定该 SelectableChannel 所支持的操作类型，如返回 5 就代表了支持读（1）和写（4）操作。
- `int	validOps​()`: Returns an operation set identifying this channel's supported operations.
- `boolean	isRegistered​()`: Tells whether or not this channel is currently registered with any selectors.

### 4.2. ServerSocketChannel 抽象类

> A selectable channel for stream-oriented listening sockets.

[ServerSocketChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/ServerSocketChannel.html) 继承了 AbstractSelectableChannel 抽象类，在传统 ServerSocket 的基础上增加了非阻塞 IO 的支持。
```java
public abstract class ServerSocketChannel
extends AbstractSelectableChannel
implements NetworkChannel
```

#### 4.2.1. 初始化

ServerSocketChannel 是抽象类，不能直接通过构造方法创建。Java 程序需要通过其静态方法创建一个没有与具体端口绑定的实例，再使用对象方法 bind() 指定它在某个端口进行工作：
```java
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
serverSocketChannel.bind(new InetSocketAddress("127.0.0.1", 30000));
```

#### 4.2.2. 主要操作

- `SocketChannel	accept​()`: 监听客户端连接，并返回一个包含新进来的连接的 SocketChannel。在阻塞模式下，accept() 方法会一直阻塞到有新连接到达；在非阻塞模式下，accept() 方法会立刻返回，如果还没有新进来的连接，返回的将是 null。

- `ServerSocket	socket​()`: Retrieves a server socket associated with this channel.

### 4.3. SocketChannel 抽象类

> A selectable channel for stream-oriented connecting sockets.

[SocketChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/SocketChannel.html) 继承了 AbstractSelectableChannel 抽象类，在传统 Socket 的基础上增加了非阻塞 IO 的支持。
```java
public abstract class SocketChannel
extends AbstractSelectableChannel
implements ByteChannel, ScatteringByteChannel, GatheringByteChannel, NetworkChannel
```

#### 4.3.1. 初始化

SocketChannel 也是抽象类，不能直接通过构造方法创建，但有以下方式来获取一个 SocketChannel 实例：
- 一个新连接到达 ServerSocketChannel 时，会返回一个与客户端绑定的 SocketChannel 对象。

- 通过其静态方法创建一个没有与具体端口绑定的实例，再使用对象方法 bind() 与某个端口绑定，并连接到互联网上的某台服务器：
  ```java
  SocketChannel socketChannel = SocketChannel.open();
  socketChannel.bind(new InetSocketAddress("127.0.0.1", 40000));
  socketChannel.connect(new InetSocketAddress("192.168.1.100", 30000));
  ```

#### 4.3.2. 读写操作

通过使用 SocketChannel 的 read 方法，并配合 ByteBuffer 字节缓冲区，即可以从 SocketChannel 中读取数据。read() 方法返回的 int 值表示读了多少字节进 Buffer 里。如果返回的是 -1，表示已经读到了流的末尾（连接关闭了）。
- `int	read​(ByteBuffer dst)`: Reads a sequence of bytes from this channel into the given buffer.
- `long	read​(ByteBuffer[] dsts)`: Reads a sequence of bytes from this channel into the given buffers.
- `long	read​(ByteBuffer[] dsts, int offset, int length)`: Reads a sequence of bytes from this channel into a subsequence of the given buffers.
eg:
```java
ByteBuffer buffer = ByteBuffer.allocate(32);
int num = socketChannel.read(buffer);
```

写数据到 SocketChannel 用的是 SocketChannel.write() 方法，该方法以一个 Buffer 作为参数。NIO 通道是面向缓冲的，所以向管道中写入数据也需要和缓冲区配合才行。
- `int write​(ByteBuffer src)`: Writes a sequence of bytes to this channel from the given buffer.
- `long	write​(ByteBuffer[] srcs)`: Writes a sequence of bytes to this channel from the given buffers.
- `long	write​(ByteBuffer[] srcs, int offset, int length)`: Writes a sequence of bytes to this channel from a subsequence of the given buffers.
eg:
```java
ByteBuffer buffer = ByteBuffer.allocate(32);
buffer.clear();
buffer.put("Test data...".getBytes());
bbuffer.flip();
channel.write(buffer);
```

#### 4.3.3. 非阻塞模式

当 SocketChannel 运行在非阻塞模式下时，调用 connect()、 read() 和 write() 方法都会立即返回而不阻塞当前线程。

eg:
```java
socketChannel.configureBlocking(false);
socketChannel.connect(new InetSocketAddress("192.168.1.100", 80));

while(! socketChannel.finishConnect() ){
    //wait, or do something else...
}
```
由于在非阻塞模式下，调用 connect 方法会立即返回。如果在连接未建立起来的情况下，从管道中读取，或向管道写入数据，会触发 NotYetConnectedException 异常。所以要进行循环检测，以保证连接完成建立。

但这会引发另外一个问题：非阻塞模式虽然不会阻塞线程，但是在方法返回后，还要进行循环检测，线程实际上还是被阻塞。因此，非阻塞模式需要与 IO 多路复用选择器搭配进行工作，通过将一或多个 SocketChannel 注册到 Selector，可以询问选择器哪个通道已经准备好了读取、写入等，从而实现真正的非阻塞。

### 4.4. DatagramChannel 抽象类

> A selectable channel for datagram-oriented sockets.

[DatagramChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/DatagramChannel.html) 继承了 AbstractSelectableChannel 抽象类，在传统 DatagramSocket 的基础上增加了非阻塞 IO 的支持。因为 UDP 是无连接的网络协议，所以不能像其它通道那样读取和写入，它发送和接收的是数据包。

```java
public abstract class DatagramChannel
extends AbstractSelectableChannel
implements ByteChannel, ScatteringByteChannel, GatheringByteChannel, MulticastChannel
```

#### 4.4.1. 初始化

- `static DatagramChannel	open​()`: Opens a datagram channel.
- `static DatagramChannel	open​(ProtocolFamily family)`: Opens a datagram channel.
- `DatagramChannel	bind​(SocketAddress local)`: Binds the channel's socket to a local address.

eg:
```java
DatagramChannel channel = DatagramChannel.open();
channel.bind(new InetSocketAddress(9999));
```

#### 4.4.2. 收发数据

- `SocketAddress	receive​(ByteBuffer dst)`: Receives a datagram via this channel. receive() 方法会将接收到的数据包内容复制到指定的 Buffer. 如果 Buffer 容不下收到的数据，多出的数据将被丢弃。

  eg:
  ```java
  ByteBuffer buf = ByteBuffer.allocate(48);
  buf.clear();
  channel.receive(buf);
  ```

- `int	send​(ByteBuffer src, SocketAddress target)`: Sends a datagram via this channel.

  eg:
  ```java
  String newData = "New String to write to file..." + System.currentTimeMillis();

  ByteBuffer buf = ByteBuffer.allocate(48);
  buf.clear();
  buf.put(newData.getBytes());
  buf.flip();

  int bytesSent = channel.send(buf, new InetSocketAddress("jenkov.com", 80));
  ```

- DatagramChannel 可以通过 connect() 方法与特定地址进行绑定（不是真正的连接），从而能够像其它通道一样使用 read() 和 write() 方法进行数据的传送：
  - `DatagramChannel	connect​(SocketAddress remote)`: Connects this channel's socket.
  - `int	read​(ByteBuffer dst)`: Reads a datagram from this channel.
  - `int	write​(ByteBuffer src)`: Writes a datagram to this channel.
  
  eg:
  ```java
  channel.connect(new InetSocketAddress("192.168.1.100", 80));
  int bytesRead = channel.read(buf);
  int bytesWritten = channel.write(but);
  ```

## 5. 管道通道

> A pair of channels that implements a unidirectional pipe.

管道是 2 个线程之间的单向数据连接，Java NIO 中使用 [Pipe](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Pipe.html) 抽象类提供对管道的支持，Pipe 有一个 [SourceChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Pipe.SourceChannel.html) 和一个 [SinkChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Pipe.SinkChannel.html)，数据会被写到 sink 通道，从 source 通道读取。

```java
public abstract class Pipe
extends Object
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/4/e5ba69734d38231f105ed88c34d0d99b.jpg)

eg:
```java
// 打开管道
Pipe pipe = Pipe.open();

// 向管道写数据
Pipe.SinkChannel sinkChannel = pipe.sink();
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(("New String to write to file..." + System.currentTimeMillis()).getBytes());
buf.flip();
while(buf.hasRemaining()) {
    sinkChannel.write(buf);
}

// 从管道读取数据 
Pipe.SourceChannel sourceChannel = pipe.source();
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = sourceChannel.read(buf);
```

## 6. Refer Links

[JAVA NIO 之文件通道](http://www.coolblog.xyz/2018/03/24/JAVA-NIO%E4%B9%8B%E6%96%87%E4%BB%B6%E9%80%9A%E9%81%93/)

[进击的 Java 新人：内存映射 (memory map)](https://zhuanlan.zhihu.com/p/27679281)

[进击的 Java 新人：FileChannel 的 map](https://zhuanlan.zhihu.com/p/27698585)

[Java NIO 之套接字通道](http://www.coolblog.xyz/2018/03/25/Java-NIO%E4%B9%8B%E5%A5%97%E6%8E%A5%E5%AD%97%E9%80%9A%E9%81%93/)

[Java NIO 系列教程（八） SocketChannel](http://ifeve.com/socket-channel/)

[Java NIO 系列教程（九） ServerSocketChannel](http://ifeve.com/server-socket-channel/)

[Java NIO 系列教程（十） Java NIO DatagramChannel](http://ifeve.com/datagram-channel/)

[Java NIO 系列教程（十一） Pipe](http://ifeve.com/pipe/)