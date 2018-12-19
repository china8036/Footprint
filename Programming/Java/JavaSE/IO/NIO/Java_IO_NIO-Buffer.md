- [Java IO NIO: Buffer](#java-io-nio--buffer)
  - [1. 类谱图](#1)
  - [2. Buffer 抽象类](#2-buffer)
  - [3. XxxBuffer 抽象类](#3-xxxbuffer)
    - [3.1. 初始化](#31)
    - [3.2. 主要操作](#32)
  - [4. HeapXxxBuffer 类](#4-heapxxxbuffer)
  - [5. DirectByteBuffer 类](#5-directbytebuffer)
    - [5.1. 基本概念](#51)
    - [5.2. DirectByteBuffer 和 HeapByteBuffer 的区别](#52-directbytebuffer--heapbytebuffer)
    - [5.3. 初始化](#53)
    - [5.4. 主要操作](#54)
  - [6. Refer Links](#6-refer-links)

# Java IO NIO: Buffer

缓冲区 Buffer 可以理解为一个容器，所有发送到 Channel、从 Channel 中读取的数据都必须首先放在 Buffer 中。而从内部结构上看，Buffer 本质就是一个数组，只不过在数组的基础上进行适当的封装，方便使用。

## 1. 类谱图

![image](http://img.cdn.firejq.com/jpg/2018/6/3/8c3ebc7143e399d945e528523fea38d9.jpg)

（注意抽象类和类的区别）

可以看到：
- Buffer 是所有 XxxBuffer 抽象类的抽象基类，对应于每一种基本数据类型都在 java.nio 下都提供了其扩展抽象类 XxxBuffer。
- 对于每一个 XxxBuffer 抽象类都提供了其扩展类 HeapXxxBuffer，这也是最终被使用的类。
- 特别地，对于 ByteBuffer 还提供了 MappedByteBuffer 抽象类及其扩展类 DirectByteBuffer，用于给予 Java 程序直接读取 JVM 堆外内存的能力。

## 2. Buffer 抽象类

[Buffer](https://docs.oracle.com/javase/9/docs/api/java/nio/Buffer.html) 是一个抽象类：
```java
public abstract class Buffer
```

Buffer 抽象类中有几个重要的属性，通过这几个属性来显示数据存储的信息：
```java
// Invariants: mark <= position <= limit <= capacity
private int mark = -1;
private int position = 0;
private int limit;
private int capacity;
```
- capacity 容量：Buffer 所能容纳数据元素的最大数量，也就是底层数组的容量值。在创建时被指定，且不可更改。
- position 位置：用于指明下一个被读或被写的缓冲区位置索引（类似于 IO 流中的记录指针）。当一个 Buffer 对象刚被创建时，position 为 0；当从 Channel 中读取了 2 个数据到该 Buffer 中时，position 为 2，指定 Buffer 中的第 3 个位置。
- limit 上界：可供读写的最大位置，用于限制 position，position < limit。
- mark 标记：位置标记，用于记录某一次的读写位置，类似于 IO 流中的 mark，可以通过 reset 重新回到这个位置。

![image](http://img.cdn.firejq.com/jpg/2018/6/2/8437155f61dd6f249c375d5fc064dca4.jpg)

## 3. XxxBuffer 抽象类

对应于每一种基本数据类型都为 Buffer 抽象基类提供了其扩展抽象类 XxxBuffer，用于针对具体数据类型的 Buffer 操作。

### 3.1. 初始化

Buffer 抽象类没有提供公有构造器，但 Buffer 抽象类的每个子类 XxxBuffer 都包含了静态创建实例的方法，用于获取一个 HeapXxxBuffer 类对象：
- `static XxxBuffer allocate(int capacity)`: 创建一个容量为 capacity 的 XxxBuffer 对象。
  ```java
  public static XxxBuffer allocate(int capacity) {
      if (capacity < 0)
          throw new IllegalArgumentException();
      return new HeapXxxBuffer(capacity, capacity);
  }
  ```

- `static XxxBuffer wrap(Xxx [] array)`: 以一个 Xxx 类型的数组创建一个 XxxBuffer 对象。
  ```java
  public static XxxBuffer wrap(byte[] array,
                                  int offset, int length)
  {
      try {
          return new HeapXxxBuffer(array, offset, length);
      } catch (IllegalArgumentException x) {
          throw new IndexOutOfBoundsException();
      }
  }
  ```

### 3.2. 主要操作

Buffer 对象初始化完毕后，其底层数组的结构信息如下：

![image](http://img.cdn.firejq.com/jpg/2018/6/3/a51dbbb61ce682d0f1c6c0db8a95169a.jpg)

XxxBuffer 读写操作通过 get 和 put 完成，这两个方法都有重载，既支持进行单个数据的 IO，也支持批量数据的 IO（以数组为参数）；既支持相对位置的 IO（position 按处理元素的个数向后移动），也支持绝对位置的 IO（以 index 为参数，position 不变）。
- `ByteBuffer	put​(byte b)`: Relative put method  (optional operation).
- `ByteBuffer	put​(byte[] src)`: Relative bulk put method  (optional operation).
- `ByteBuffer	put​(byte[] src, int offset, int length)`: Relative bulk put method  (optional operation).
- `ByteBuffer	put​(int index, byte b)`: Absolute put method  (optional operation).
- `...`
- `byte	get​()`: Relative get method.
- `ByteBuffer	get​(byte[] dst)`: Relative bulk get method.
- `ByteBuffer	get​(byte[] dst, int offset, int length)`: Relative bulk get method.
- `byte	get​(int index)`: Absolute get method.
- `...`

当 Buffer 的数据装填操作完成后，position 位于最后一次写入数据的位置。为进行 Buffer 的数据获取，需要先调用 flip() 方法：
- `ByteBuffer flip​()`: 该方法将 limit 设置为 position 所在的位置，然后将 position 归零，为数据输出做好了准备。

当 Buffer 的数据输出操作完成后，可调用 clear() 方法：
- `ByteBuffer	clear​()`: 该方法不会情况 Buffer 中的数据，而是将 position 归零，并将 limit 置为 capacity，即数组的末尾，为下一次数据装填做好了准备（下一次写入会直接覆盖原数据）。

除此之外，Buffer 中包含了以下常用的方法：
- `int	capacity​()`: Returns this buffer's capacity.
- `boolean	hasRemaining​()`: Tells whether there are any elements between the current position and the limit.
- `int	limit​()`: Returns this buffer's limit.
- `Buffer	limit​(int newLimit)`: Sets this buffer's limit.
- `Buffer	mark​()`: Sets this buffer's mark at its position.
- `int	position​()`: Returns this buffer's position.
- `Buffer	position​(int newPosition)`: Sets this buffer's position.
- `int	remaining​()`: Returns the number of elements between the current position and the limit.
- `Buffer	reset​()`: Resets this buffer's position to the previously-marked position.
- `Buffer	rewind​()`: Rewinds this buffer.
- `abstract Buffer	slice​()`: Creates a new buffer whose content is a shared subsequence of this buffer's content.

## 4. HeapXxxBuffer 类

HeapXxxBuffer 类是所有 XxxBuffer 抽象类都有的扩展类，也是静态初始化方法 allocate() 和 wrap() 默认创建对象的所属类。

## 5. DirectByteBuffer 类

### 5.1. 基本概念

在所有 XxxBuffer 中，[ByteBuffer](https://docs.oracle.com/javase/9/docs/api/java/nio/ByteBuffer.html) 是最常用的 Buffer 子类，它可以用于对底层字节数组进行 get/set 操作。
```java
public abstract class ByteBuffer
    extends Buffer
    implements Comparable<ByteBuffer>
```
而 ByteBuffer 的特殊之处在于，不同于其它 XxxBuffer，NIO 为 ByteBuffer 提供了 MappedByteBuffer 抽象类及其扩展 DirectByteBuffer 类。

[MappedByteBuffer](https://docs.oracle.com/javase/9/docs/api/java/nio/MappedByteBuffer.html) 是 ByteBuffer 的扩展抽象类，用于表示 Channel 将磁盘文件的部分或全部内容映射到内存中后得到的结果，通过 MappedByteBuffer 对象由 Channel 的 map() 方法返回。
```java
public abstract class MappedByteBuffer
    extends ByteBuffer
```

DirectByteBuffer 是 MappedByteBuffer 抽象类的扩展类，用于表示一个存放在 JVM Heap 外内存中的 Buffer。

> Given a direct byte buffer, the Java virtual machine will make a best effort to perform native I/O operations directly upon it. That is, it will attempt to avoid copying the buffer's content to (or from) an intermediate buffer before (or after) each invocation of one of the underlying operating system's native I/O operations.

```java
class DirectByteBuffer
    extends MappedByteBuffer
    implements DirectBuffer
```

### 5.2. DirectByteBuffer 和 HeapByteBuffer 的区别

- HeapByteBuffer:
  - 创建快：

    直接在 JVM Heap 中申请一块内存即可，创建成本小。

  - IO 效率低：
    ```java
    // share/classes/sun/nio/ch/IOUtil.java
    static int write(FileDescriptor fd, ByteBuffer src, long position,
                     NativeDispatcher nd)
        throws IOException
    {   // 如果 src 是 DirectBuffer，就直接调用 writeFromNativeBuffer
        if (src instanceof DirectBuffer)
            return writeFromNativeBuffer(fd, src, position, nd);
        // 如果不是 DirectBuffer，则要先创建一个临时的 DirectBuffer，把 src 拷进去，然后再调用真正的写操作
        // Substitute a native buffer
        int pos = src.position();
        int lim = src.limit();
        assert (pos <= lim);
        int rem = (pos <= lim ? lim - pos : 0);
        ByteBuffer bb = Util.getTemporaryDirectBuffer(rem);
        try {
            bb.put(src);
            bb.flip();
            // Do not update src until we see how many bytes were written
            src.position(pos);

            int n = writeFromNativeBuffer(fd, bb, position, nd);
            if (n > 0) {
                // now update src
                src.position(pos + n);
            }
            return n;
        } finally {
            Util.offerFirstTemporaryDirectBuffer(bb);
        }
    }
    static int read(FileDescriptor fd, ByteBuffer dst, long position,
                    NativeDispatcher nd)
        throws IOException
    {
        if (dst.isReadOnly())
            throw new IllegalArgumentException("Read-only buffer");
        if (dst instanceof DirectBuffer)
            return readIntoNativeBuffer(fd, dst, position, nd);

        // Substitute a native buffer
        ByteBuffer bb = Util.getTemporaryDirectBuffer(dst.remaining());
        try {
            int n = readIntoNativeBuffer(fd, bb, position, nd);
            bb.flip();
            if (n > 0)
                dst.put(bb);
            return n;
        } finally {
            Util.offerFirstTemporaryDirectBuffer(bb);
        }
    }
    ```
    存放在 JVM Heap 内存中，由于受到 GC 的影响，内存地址随时可能会变化，而与操作系统 kernel 进行 IO 交互时要求内存地址不能变化（系统调用 write、read、pwrite，pread 等函数都要求传入 buffer 的起始地址和 buffer count 作为参数），因此在读写到相应的 Channel 时，需要先将 Java Heap 的 buffer 内容拷贝至 JVM Heap 外内存中（地址相对稳定的内存，实际上是 direct buffer 即 C Heap，或 intermediate buffer 即 C stack），然后再把这个地址发给操作系统 kernel。

  - 额外操作效率高：

    例如编码、过滤等操作，Java 程序直接对 JVM Heap 中的 Buffer 进行操作即可，效率较高。

- DirectByteBuffer:
  - 创建慢：
    ```java
    DirectByteBuffer(int cap) {                   // package-private
        // ...
        long base = 0;
        try {
            base = unsafe.allocateMemory(size);  // 申请大小为 size 的堆外内存空间
        } catch (OutOfMemoryError x) {
            Bits.unreserveMemory(size, cap);
            throw x;
        }
        unsafe.setMemory(base, size, (byte) 0);  // 空间初始化为 0
        if (pa && (base % ps != 0)) {            // 空间内存对齐
            // Round up to page boundary
            address = base + ps - (base & (ps - 1));
        } else {
            address = base;
        }
        // 注册 cleaner ，方便堆外内存的回收
        cleaner = Cleaner.create(this, new Deallocator(base, size, cap));
        att = null;
    }
    ```
    unsafe.allocateMemory(size) 的 JNI 实现：(openjdk\hotspot\src\share\vm\memory\allocation.cpp)
    ```cpp
    // allocate using malloc; will fail if no memory available
    inline char* AllocateHeap(size_t size, MEMFLAGS flags, address pc = 0,
        AllocFailType alloc_failmode = AllocFailStrategy::EXIT_OOM) {
      // ...
      char* p = (char*) os::malloc(size, flags, pc); // 分配在 C_HEAP 上并返回指向内存区域的指针
      // ...
      return p;
    }
    ```
    需要通过 `unsafe.allocateMemory` 调用 C Runtime 的 malloc 方法在 JVM Heap 外（direct buffer 即 C Heap）申请一块内存，创建成本较大。可见，DirectByteBuffer 只适用于长生存期的 Buffer，而不适用于短生存期、一次用完就丢弃的 Buffer。

  - IO 效率高：

    存放在 JVM Heap 外内存（C Heap）中，由于 C Heap 仅受 Full GC 的影响，地址相对稳定，因此直接把这个内存地址发给操作系统 kernel 即可。

    可见，DirectBuffer 并没有做什么特别的内存优化，只是因为 HeapBuffer 必须多做一次拷贝，才显得 DirectBuffer 更快而已。但在大量的网络交互下，一般速度会比 HeapByteBuffer 要快好几倍。

  - 额外操作效率低：

    例如编码、过滤等操作，Java 程序需要先将 C Heap 中的 buffer 拷贝到 JVM Heap 中才能进行操作，因此效率较低。可见，只有当你不需要对数据进行这些额外操作，只是从一个文件复制数据到另一个文件、一个 Socket 到另一个的时候才使用 DirectByteBuffer。

### 5.3. 初始化

由于 DirectBuffer 存在一定的安全风险，因此所有 XxxBuffer 抽象类包含的 allocate() 方法和 wrap() 方法创建的类对象都属于 HeapXxxBuffer。但在 ByteBuffer 中，还是提供了一个 allocateDirect() 方法可用于创建 DirectByteBuffer 类对象：
```java
public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
}
```
因此，DirectBuffer 只能在 ByteBuffer 级别上直接使用。如果希望使用其它类型的 DirectBuffer，则需要将 ByteBuffer 转化为其它类型的 Buffer（如 ByteBufferAsShortBuffer）。

### 5.4. 主要操作

DirectBuffer 的主要操作与 HeapBuffer 基本相同，故不再赘述。

## 6. Refer Links

[Java NIO 之缓冲区](http://www.coolblog.xyz/2018/03/04/Java-NIO%E4%B9%8B%E7%BC%93%E5%86%B2%E5%8C%BA/)

[进击的 Java 新人：Direct Buffer](https://zhuanlan.zhihu.com/p/27625923)

[Java NIO direct buffer 的优势在哪儿？](https://www.zhihu.com/question/60892134)