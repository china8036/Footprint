- [Java NIO.2 文件访问支持](#java-nio2-文件访问支持)
  - [1. Path 接口](#1-path-接口)
  - [2. Paths 工具类](#2-paths-工具类)
  - [3. Files 工具类](#3-files-工具类)
  - [4. FileVisitor 接口](#4-filevisitor-接口)
  - [5. WatchService 接口](#5-watchservice-接口)
  - [6. Refer Links](#6-refer-links)

# Java NIO.2 文件访问支持

早期的 Java 只提供了一个 java.io.File 类来提供对文件系统访问的支持，但 File 类功能有限，无法利用特定文件系统的特性，提供的 API 性能也不高，且大多数方法在出错时仅返回失败，而不会提供异常信息。

NIO 为弥补这一不足，在 Java 7 NIO.2 中新增了 java.nio.file 包，其中提供了全面的文件 IO 和文件系统访问支持。NIO.2 引入了一个代表平台无关路径的 Path 接口、Files 工具类和 Paths 工具类，其中 Files 工具类包含了大量的静态工具方法来操作文件，Paths 则包含了 2 个返回 Path 对象的静态工厂方法。

## 1. Path 接口

> An object that may be used to locate a file in a file system. It will typically represent a system dependent file path.
> 
> A Path represents a path that is hierarchical and composed of a sequence of directory and file name elements separated by a special separator or delimiter.

```java
public interface Path
    extends Comparable<Path>, Iterable<Path>, Watchable
```

[Path](https://docs.oracle.com/javase/9/docs/api/java/nio/file/Path.html) 接口的类对象用于表示路径和文件对象，旨在取代传统 IO 中的 File 类。

- 构造器

  Path 接口没有提供公有构造器，需要通过 Paths 工厂类来创建 Path 对象。

- 常用 API
  - `File	toFile​()`: 将该 Path 对象转换为 File 对象。
  - `URI	toUri​()`: 将该 Path 对象转换为 Uri 对象。
  - `Path	toAbsolutePath​()`: Returns a Path object representing the absolute path of this path.
  - `Path	subpath​(int beginIndex, int endIndex)`: Returns a relative Path that is a subsequence of the name elements of this path.
  - `int	getNameCount​()`: 返回 Path 对象所包含的路径名的数量，如 `"c:\\user\\codes` 会返回 3。
  - `Path	getRoot​()`: Returns the root component of this path as a Path object, or null if this path does not have a root component.
  - `Path	getParent​()`: Returns the parent path, or null if this path does not have a parent.
  - `Path	getName​(int index)`: Returns a name element of this path as a Path object.

## 2. Paths 工具类

```java
public final class Paths 
```

[Paths](https://docs.oracle.com/javase/9/docs/api/java/nio/file/Paths.html) 工具类中只包含了 2 个用于创建 Path 对象的静态工厂方法：
- `static Path	get​(URI uri)`: Converts the given URI to a Path object.
- `static Path	get​(String first, String... more)`: Converts a path string, or a sequence of strings that when joined form a path string, to a Path.

eg:
```java
Path path1 = Paths.get("."); // 使用相对路径
Path path2 = Paths.get("c:\\user\\codes"); // 使用绝对路径
Path path3 = Paths.get("c:", "user", "codes"); // 相当于 c:\user\codes
```

## 3. Files 工具类

> This class consists exclusively of static methods that operate on files, directories, or other types of files.
> 
> In most cases, the methods defined here will delegate to the associated file system provider to perform the file operations.

```java
public final class Files
```

[Files](https://docs.oracle.com/javase/9/docs/api/java/nio/file/Files.html) 工具类是一个高度封装的工具类，它提供了大量的工具方法来完成平台无关的文件复制、文件读取、文件写入等操作。在 Java 8 中，进一步增强了 Files 工具类的功能，允许开发者使用 Stream API 来操作文件目录和文件内容。

常用 API:
- 文件复制
  - `static long	copy​(InputStream in, Path target, CopyOption... options)`: Copies all bytes from an input stream to a file.
  - `static long	copy​(Path source, OutputStream out)`: Copies all bytes from a file to an output stream.
  - `static Path	copy​(Path source, Path target, CopyOption... options)`: Copy a file to a target file.
- 文件属性
  - `static boolean	isDirectory​(Path path, LinkOption... options)`: Tests whether a file is a directory.
  - `static boolean	isExecutable​(Path path)`: Tests whether a file is executable.
  - `static boolean	isHidden​(Path path)`: Tells whether or not a file is considered hidden.
  - `static boolean	isReadable​(Path path)`: Tests whether a file is readable.
  - `static boolean	isRegularFile​(Path path, LinkOption... options)`: Tests whether a file is a regular file with opaque content.
  - `static boolean	isSameFile​(Path path, Path path2)`: Tests if two paths locate the same file.
  - `static boolean	isSymbolicLink​(Path path)`: Tests whether a file is a symbolic link.
  - `static boolean	isWritable​(Path path)`: Tests whether a file is writable.
  - `static long	size​(Path path)`: Returns the size of a file (in bytes).
- 文件删除
  - `static void	delete​(Path path)`: Deletes a file.
  - `static boolean	deleteIfExists​(Path path)`: Deletes a file if it exists.
- 文件创建
  - `static Path	createDirectory​(Path dir, FileAttribute<?>... attrs)`: Creates a new directory.
  - `static Path	createFile​(Path path, FileAttribute<?>... attrs)`: Creates a new and empty file, failing if the file already exists.
- 文件移动 / 重命名
  - `static Path	move​(Path source, Path target, CopyOption... options)`: Move or rename a file to a target file.
- 文件是否存在
  - `static boolean	exists​(Path path, LinkOption... options)`: Tests whether a file exists.
- 文件读取
  - `static byte[]	readAllBytes​(Path path)`: Reads all the bytes from a file.
  - `static List<String>	readAllLines​(Path path)`: Read all lines from a file.
- 文件写入
  - `static Path	write​(Path path, byte[] bytes, OpenOption... options)`: Writes bytes to a file.
  - `static Path	write​(Path path, Iterable<? extends CharSequence> lines, Charset cs, OpenOption... options)`: Write lines of text to a file.
  - `static Path	write​(Path path, Iterable<? extends CharSequence> lines, OpenOption... options)`: Write lines of text to a file.
- 遍历文件和目录
  - `static Path	walkFileTree​(Path start, FileVisitor<? super Path> visitor)`: 遍历 start 目录下的所有文件和子目录。
  - `static Path	walkFileTree​(Path start, Set<FileVisitOption> options, int maxDepth, FileVisitor<? super Path> visitor)`: 遍历 start 目录下的所有文件和子目录，最多遍历 maxDepth 深度的文件。

- 对目录下所有文件进行流式操作
  - `static Stream<Path>	list​(Path dir)`: Return a lazily populated Stream, the elements of which are the entries in the directory.
    
    eg:
    ```java
    Files.list(Paths.get(".")).forEach(path -> System.out.print(path));
    ```
- 文件监听（注册监听器）
  - `WatchKey	register​(WatchService watcher, WatchEvent.Kind<?>... events)`: Registers the file located by this path with a watch service.
  - `WatchKey	register​(WatchService watcher, WatchEvent.Kind<?>[] events, WatchEvent.Modifier... modifiers)`: Registers the file located by this path with a watch service.

## 4. FileVisitor 接口

```java
public interface FileVisitor<T> 
```

[FileVisitor](https://docs.oracle.com/javase/9/docs/api/java/nio/file/FileVisitor.html) 接口的类对象代表一个文件访问器，用于 Files 工具类的 `walkFileTree​` 方法中时，每遍历一个文件或目录都会触发 FileVisitor 对象中的相应方法：
- `FileVisitResult	postVisitDirectory​(T dir, IOException exc)`: Invoked for a directory after entries in the directory, and all of their descendants, have been visited.
- `FileVisitResult	preVisitDirectory​(T dir, BasicFileAttributes attrs)`: Invoked for a directory before entries in the directory are visited.
- `FileVisitResult	visitFile​(T file, BasicFileAttributes attrs)`: Invoked for a file in a directory.
- `FileVisitResult	visitFileFailed​(T file, IOException exc)`: Invoked for a file that could not be visited.

其中，返回的 [FileVisitResult](https://docs.oracle.com/javase/9/docs/api/java/nio/file/FileVisitResult.html) 对象是一个枚举类，代表了访问之后的后续行为：
- `CONTINUE`: Continue.
- `SKIP_SIBLINGS`: Continue without visiting the siblings of this file or directory.
- `SKIP_SUBTREE`: Continue without visiting the entries in this directory.
- `TERMINATE`: Terminate.

实际编程时，若没必要为 FileVisitor 接口的所有方法提供实现，可通过继承 [SimpleFileVisitor](https://docs.oracle.com/javase/9/docs/api/java/nio/file/SimpleFileVisitor.html) 来实现一个 FileVisitor 接口对象，再可根据自己的需要重写指定方法即可。

## 5. WatchService 接口

```java
public interface WatchService
    extends Closeable
```

在 Java 7 之前，若程序需要监控文件的变化，则需要启动一个后台线程，轮询指定目录的文件来检查是否发生了改变，这种方式实现繁琐且性能不佳。

NIO.2 提供了 [WatchService](https://docs.oracle.com/javase/9/docs/api/java/nio/file/WatchService.html) 接口，该接口的类对象代表了一个文件系统监听服务。一旦在 Path 对象中调用 register 方法注册了 WatchService 对象后，即可调用 WatchService 中的 API 来获取被监听目录的文件变化事件。
- `WatchKey	poll​()`: Retrieves and removes the next watch key, or null if none are present.
- `WatchKey	poll​(long timeout, TimeUnit unit)`: Retrieves and removes the next watch key, waiting if necessary up to the specified wait time if none are yet present.
- `WatchKey	take​()`: Retrieves and removes next watch key, waiting if none are yet present.

## 6. Refer Links

[java Files 类和 Paths 类的用法](https://blog.csdn.net/u010889616/article/details/52694061)