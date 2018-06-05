- [Java IO File & RandomAccessFile](#java-io-file-randomaccessfile)
  - [1. File 类](#1-file)
    - [1.1. 基本概念](#11)
    - [1.2. 常用 API](#12--api)
      - [1.2.1. 文件 / 路径分隔符](#121)
      - [1.2.2. 构造方法](#122)
      - [1.2.3. 文件名 / 路径名方法](#123)
      - [1.2.4. 文件属性操作方法](#124)
      - [1.2.5. 文件信息获取方法](#125)
      - [1.2.6. 文件操作方法](#126)
      - [1.2.7. 目录操作方法](#127)
  - [2. RandomAccessFile 类](#2-randomaccessfile)
    - [2.1. 基本概念](#21)
    - [2.2. 常用 API](#22--api)
      - [2.2.1. 构造方法](#221)
      - [2.2.2. 指针操作](#222)
      - [2.2.3. 读写操作](#223)
      - [2.2.4. 其它操作](#224)
    - [2.3. 应用](#23)
      - [2.3.1. 多线程断点网络下载工具](#231)
  - [3. Refer Links](#3-refer-links)

# Java IO File & RandomAccessFile

## 1. File 类

### 1.1. 基本概念

在整个 java.io 包中，唯一表示与文件本身有关的类就是 [File](https://docs.oracle.com/javase/9/docs/api/java/io/File.html) 类，它代表了与平台无关的文件和目录。

File 类是对文件系统中文件与目录进行封装的对象，可以通过对象的思想来操作文件和文件夹。File 类保存文件或目录的各种元数据信息，其包含了文件名、文件长度、最后修改时间、是否可读、获取当前文件的路径名，判断指定文件是否存在、获得当前目录中的文件列表，创建、删除文件和目录等方法。  

不管是文件还是目录都是使用 File 来操作的，但需要注意的是 **File 类不能访问文件内容本身。若需要访问文件内容本身，则需要使用 I/O 流或 RandomAccessFile 进行操作**。

### 1.2. 常用 API

类定义：
```java
public class File
extends Object
implements Serializable, Comparable<File>
```

#### 1.2.1. 文件 / 路径分隔符

- `static String	pathSeparator`: The system-dependent path-separator character, represented as a string for convenience (windows：‘；’).
- `static char	pathSeparatorChar`: The system-dependent path-separator character (windows：‘；’).
- `static String	separator`: The system-dependent default name-separator character, represented as a string for convenience (windows：‘\’).
- `static char	separatorChar`: The system-dependent default name-separator character (windows：‘\’).

#### 1.2.2. 构造方法

- `File​(File parent, String child)`: 通过给定的父抽象路径名和子路径名字符串创建一个新的 File 实例。
- `File​(String pathname)`: 通过将给定路径名字符串转换成抽象路径名来创建一个新 File 实例。
- `File​(String parent, String child)`: 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
- `File​(URI uri)`: 通过将给定的 file: URI 转换成一个抽象路径名来创建一个新的 File 实例。

#### 1.2.3. 文件名 / 路径名方法

- `public String getName()`: 返回由此抽象路径名表示的文件或目录的名称。
- `public String getPath()`: 将此抽象路径名转换为一个路径名字符串。
- `public String getAbsolutePath()`: 返回抽象路径名的绝对路径名字符串。
- `public String getParent()`: 返回此抽象路径名的父路径名的路径名字符串，如果此路径名没有指定父目录，则返回 null。
- `public boolean renameTo(File dest)`: 重新命名此抽象路径名表示的文件。

#### 1.2.4. 文件属性操作方法

- `public boolean exists()`: 测试此抽象路径名表示的文件或目录是否存在。
- `public boolean canRead()`: 测试应用程序是否可以读取此抽象路径名表示的文件。
- `public boolean canWrite()`: 测试应用程序是否可以修改此抽象路径名表示的文件。
- `public boolean isFile()`: 测试此抽象路径名表示的文件是否是一个标准文件。
- `public boolean isDirectory()`: 测试此抽象路径名表示的文件是否是一个目录。
- `public boolean isAbsolute()`: 测试此抽象路径名是否为绝对路径名。
- `public boolean setLastModified(long time)`: 设置由此抽象路径名所指定的文件或目录的最后一次修改时间。
- `public boolean setReadOnly()`: 标记此抽象路径名指定的文件或目录，以便只可对其进行读操作。

#### 1.2.5. 文件信息获取方法

- `public long lastModified()`: 返回此抽象路径名表示的文件最后一次被修改的时间。
- `public long length()`: 返回由此抽象路径名表示的文件的长度。

#### 1.2.6. 文件操作方法

- `public boolean createNewFile() throws IOException`: 当且仅当不存在具有此抽象路径名指定的名称的文件时，原子地创建由此抽象路径名指定的一个新的空文件。
- `public boolean delete()`:  删除此抽象路径名表示的文件或目录。
- `public void deleteOnExit()`: 在虚拟机终止时，请求删除此抽象路径名表示的文件或目录。
- `public static File createTempFile(String prefix, String suffix, File directory) throws IOException`: 在指定目录中创建一个新的空文件，使用给定的前缀和后缀字符串生成其名称。
- `public static File createTempFile(String prefix, String suffix) throws IOException`: 在默认临时文件目录中创建一个空文件，使用给定前缀和后缀生成其名称。

  NOTE: 当我们创建一个新的 File 对象，所给的路径不是根盘路径时，文件会自动放在项目文件夹下；但是使用静态方法创建一个临时文件，所给路径不是根盘路径时，文件是放在 C 盘下的某文件夹下的。

#### 1.2.7. 目录操作方法

- `public boolean mkdir()`: 创建此抽象路径名指定的目录。
- `public String[] list()`: 返回由此抽象路径名所表示的目录中的文件和目录的名称所组成字符串数组。
- `public String[] list(FilenameFilter filter)`: 返回由包含在目录中的文件和目录的名称所组成的字符串数组，这一目录是通过满足指定过滤器的抽象路径名来表示的。

  此方法可用于文件的过滤，通过 FilenameFilter 参数可以只列出符合条件的文件。

  FilenameFilter 接口是一个函数式接口，其包含了一个 accept(File dir, String name) 方法，该方法会依次对指定 File 的所有子目录或文件进行迭代，如果该方法返回 true，则 list() 方法会列出该子目录或文件。
  ```java
  File file = new File(".");
  String [] nameList = file.list((dir, name) -> name.endsWith(".java") || new File(name).isDirectory());
  Arrays.toString(nameList);
  ```

- `public File[] listFiles()`: 返回一个抽象路径名数组，这些路径名表示此抽象路径名所表示目录中的文件。
- `public File[] listFiles(FileFilter filter)`: 返回表示此抽象路径名所表示目录中的文件和目录的抽象路径名数组，这些路径名满足特定过滤器。

## 2. RandomAccessFile 类

### 2.1. 基本概念

[RandomAccessFile](https://docs.oracle.com/javase/9/docs/api/java/io/RandomAccessFile.html) 是 Java 输入输出流体系中功能最丰富的文件内容访问类，它封装了字节流，同时还封装了一个缓冲区（字符数组），通过内部的指针来操作字符数组中的数据，提供了众多方法来访问文件内容。

特点：
- RandomAccessFile 位于 java.io 包下，虽然它不是 InputStream 或 OutputStream 的子类，但 RandomAccessFile 包含 InputStream 的三个 read 方法，也包含 OutputStream 的三个 write 方法。

- FileInputStream 只能对文件进行读操作，FileOutputStream 只能对文件进行写操作，而 RandomAccessFile 同时支持对文件内容的读取和写入。

- 且与普通 InputStream 或 OutputStream 不同的是，RandomAccessFile 支持以**随机访问**的方式读写文件的任意位置。因此，若只需要访问文件部分内容，而不是从头读到尾，RandomAccessFile 会是更好的选择。

- RandomAccessFile 允许自定义文件记录指针的位置，因此可利用 RandomAccessFile 向已存在的文件后追加内容，用于多线程下载或多个线程同时写数据到文件。

- RandomAccessFile 最大的局限在于它只能读写文件，而无法读写其它 I/O 节点，因此其构造函数只接收两种类型的参数：文件路径字符串和 File 对象。

- RandomAccessFile 对象在实例化时，如果要操作的文件不存在，会自动创建；如果文件存在，写数据时未指定指针位置，会默认从文件头（指针位 0）开始写，即覆盖原有的内容。当读 / 写了 n 个字节后，文件记录指针将会自动向后移动 n 个字节。

### 2.2. 常用 API

类定义：
```java
public class RandomAccessFile
extends Object
implements DataOutput, DataInput, Closeable
```

#### 2.2.1. 构造方法

- `RandomAccessFile​(File file, String mode)`: Creates a random access file stream to read from, and optionally to write to, the file specified by the File argument.
- `RandomAccessFile​(String name, String mode)`: Creates a random access file stream to read from, and optionally to write to, a file with the specified name.

构造方法中的 mode 参数，用于指定 RandomAccessFile​ 的访问模式，该参数有如下 4 种取值：
- `r`: 代表以只读方式打开指定文件。
- `rw`: 以读写方式打开指定文件。
- `rws`: 以读写方式打开指定文件，并将对文件内容或元数据的更新都同步写入底层存储设备。
- `rwd`: 以读写方式打开指定文件，对将对文件内容的更新同步写入底层存储设备。

#### 2.2.2. 指针操作

RandomAccessFile 包含了一个对象记录的指针，用于标识当前流的读写位置 RandomAccessFile 包含两个方法来操作文件记录指针。文件指针可以通过 getFilePointer 方法读取，并由 seek 方法设置：
- `long	getFilePointer​()`: Returns the current offset in this file.
- `void	seek​(long pos)`: Sets the file-pointer offset, measured from the beginning of this file, at which the next read or write occurs.

```java
RandomAccessFile raf = new RandomAccessFile("out.txt", "rw");
// 将文件指针移动到离文件头 300 字节处
raf.seek(300);
raf.write("覆盖的内容".geBytes());
// 将文件指针移动到文件末尾
raf.seek(raf.length());
raf.write("追加的内容".geBytes());
```

#### 2.2.3. 读写操作

RandomAccessFile 包含 InputStream 的 3 个 read 方法，也包含 OutputStream 的 3 个 write 方法：
- read()
  - `int read()`: 从此文件中读取一个字节。
  - `int read(byte[] b)`: 将最多 b.length 个字节从此文件读入 byte 数组。
  - `int read(byte[] b, int off, int len)`: 将最多 len 个数据字节从此文件读入 byte 数组。
- write()
  - `void write(byte[] b)`: 将 b.length 个字节从指定 byte 数组写入到此文件，并从当前文件指针开始。
  - `void write(byte[] b, int off, int len)`: 将 len 个字节从指定 byte 数组写入到此文件，并从偏移量 off 处开始。
  - `void write(int b)`: 向此文件写入指定的字节。

除此之外，RandomAccessFile 还包含一系列的 readXxx 和 writeXxx 方法完成输入输出：
- readXxx()
  - `boolean readBoolean()`: 从此文件读取一个 boolean。
  - `byte readByte()`: 从此文件读取一个有符号的八位值。
  - `char readChar()`: 从此文件读取一个字符。
  - `double readDouble()`: 从此文件读取一个 double。
  - `float readFloat()`: 从此文件读取一个 float。
  - `void readFully(byte[] b)`: 将 b.length 个字节从此文件读入 byte 数组，并从当前文件指针开始。
  - `void readFully(byte[] b, int off, int len)`: 将正好 len 个字节从此文件读入 byte 数组，并从当前文件指针开始。
  - `int readInt()`：从此文件读取一个有符号的 32 位整数。
  - `String readLine()`: 从此文件读取文本的下一行。
  - `long readLong()`:  从此文件读取一个有符号的 64 位整数。
  - `short readShort()`: 从此文件读取一个有符号的 16 位数。
  - `int readUnsignedByte()`: 从此文件读取一个无符号的八位数。
  - `int readUnsignedShort()`: 从此文件读取一个无符号的 16 位数。
  - `String readUTF()`: 从此文件读取一个字符串。
- writeXxx()
  - `void writeBoolean(boolean v)`: 按单字节值将 boolean 写入该文件。
  - `void writeByte(int v)`: 按单字节值将 byte 写入该文件。
  - `void writeBytes(String s)`: 按字节序列将该字符串写入该文件。
  - `void writeChar(int v)`: 按双字节值将 char 写入该文件，先写高字节。
  - `void writeChars(String s)`: 按字符序列将一个字符串写入该文件。
  - `void writeDouble(double v)`: 使用 Double 类中的 doubleToLongBits 方法将双精度参数转换为一个 long，然后按八字节数量将该 long 值写入该文件，先定高字节。
  - `void writeFloat(float v)`: 使用 Float 类中的 floatToIntBits 方法将浮点参数转换为一个 int，然后按四字节数量将该 int 值写入该文件，先写高字节。
  - `void writeInt(int v)`: 按四个字节将 int 写入该文件，先写高字节。
  - `void writeLong(long v)`: 按八个字节将 long 写入该文件，先写高字节。
  - `void writeShort(int v)`: 按两个字节将 short 写入该文件，先写高字节。
  - `void writeUTF(String str)`: 使用 modified UTF-8 编码以与机器无关的方式将一个字符串写入该文件。

#### 2.2.4. 其它操作

- `void setLength(long newLength)`：设置此文件的长度。

- `int skipBytes(int n)` ：尝试跳过输入的 n 个字节以丢弃跳过的字节。

- `void close()`：关闭此随机访问文件流并释放与该流关联的所有系统资源。

- `long length()`：返回此文件的长度。

### 2.3. 应用

#### 2.3.1. 多线程断点网络下载工具

多线程断点网络下载工具（如 FlashGet）就可通过 RandomAccessFile 来实现。

这些下载工具再下载开始时会创建两个文件：一个与被下载文件大小相同的空文件，一个是记录文件指针位置的位置文件。

下载工具通过多个线程启动输入流来读取网络数据，并使用 RandomAccessFile 将从网络上读取的数据写入到前边创建的空文件中，每写一些数据后，记录文件指针的文件就分别记下每个 RandomAccessFile 当前的文件指针位置。若网络断开，当再次开始下载时，每个 RandomAccessFile 都会根据记录文件指针的文件中记录的位置继续向下写数据。

## 3. Refer Links

[java 中 java.io.RandomAccessFile 的应用场景及使用实例](http://blog.csdn.net/qq496013218/article/details/69397380)