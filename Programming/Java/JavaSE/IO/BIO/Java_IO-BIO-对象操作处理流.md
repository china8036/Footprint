- [Java IO BIO 对象操作处理流](#java-io-bio-%E5%AF%B9%E8%B1%A1%E6%93%8D%E4%BD%9C%E5%A4%84%E7%90%86%E6%B5%81)
  - [1. 序列化机制](#1-%E5%BA%8F%E5%88%97%E5%8C%96%E6%9C%BA%E5%88%B6)
  - [2. 序列化接口](#2-%E5%BA%8F%E5%88%97%E5%8C%96%E6%8E%A5%E5%8F%A3)
    - [2.1. Serializable 接口](#21-serializable-%E6%8E%A5%E5%8F%A3)
    - [2.2. Externalizable 接口](#22-externalizable-%E6%8E%A5%E5%8F%A3)
  - [3. 序列化编号](#3-%E5%BA%8F%E5%88%97%E5%8C%96%E7%BC%96%E5%8F%B7)
  - [4. 序列化版本](#4-%E5%BA%8F%E5%88%97%E5%8C%96%E7%89%88%E6%9C%AC)
  - [5. 对象操作处理流](#5-%E5%AF%B9%E8%B1%A1%E6%93%8D%E4%BD%9C%E5%A4%84%E7%90%86%E6%B5%81)
    - [5.1. 对象序列化：ObjectInputStream](#51-%E5%AF%B9%E8%B1%A1%E5%BA%8F%E5%88%97%E5%8C%96%EF%BC%9Aobjectinputstream)
      - [5.1.1. 基本概念](#511-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
      - [5.1.2. 常用 API](#512-%E5%B8%B8%E7%94%A8-api)
      - [5.1.3. 序列化 NOTE](#513-%E5%BA%8F%E5%88%97%E5%8C%96-note)
    - [5.2. 对象反序列化：ObjectOutputStream](#52-%E5%AF%B9%E8%B1%A1%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%EF%BC%9Aobjectoutputstream)
      - [5.2.1. 基本概念](#521-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
      - [5.2.2. 常用 API](#522-%E5%B8%B8%E7%94%A8-api)
      - [5.2.3. 反序列化 NOTE](#523-%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96-note)
    - [5.3. 实例](#53-%E5%AE%9E%E4%BE%8B)
  - [6. 自定义序列化](#6-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BA%8F%E5%88%97%E5%8C%96)
    - [6.1. 使用 transient 关键字](#61-%E4%BD%BF%E7%94%A8-transient-%E5%85%B3%E9%94%AE%E5%AD%97)
    - [6.2. 提供特殊签名的方法](#62-%E6%8F%90%E4%BE%9B%E7%89%B9%E6%AE%8A%E7%AD%BE%E5%90%8D%E7%9A%84%E6%96%B9%E6%B3%95)
    - [6.3. 实现 Externalizable 接口](#63-%E5%AE%9E%E7%8E%B0-externalizable-%E6%8E%A5%E5%8F%A3)
  - [7. Refer Links](#7-refer-links)

# Java IO BIO 对象操作处理流

## 1. 序列化机制

在 Java 中，对象序列化机制允许将内存中的 Java 对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上或通过网络进行二进制流的传输。其它程序一旦获取了这种二进制流，都可以将其恢复成原来的 Java 对象。序列化机制使得对象可以脱离程序的运行而独立存在。

在序列化机制中，序列化是指把 Java 对象保存为二进制字节码写入 IO 流的过程，而反序列化是指从 IO 流中把二进制码重新转换成 Java 对象的过程。

## 2. 序列化接口

如果要让某个对象支持序列化机制，则必须使得该对象所属的类是可序列化的，即必须使该类实现以下接口之一：
- Serializable 接口
- Externalizable 接口

### 2.1. Serializable 接口

Serializable 接口是一个标记接口，其源码中没有包含任何内容，因此，实现该接口无须实现任何方法，它仅仅用于在执行对象序列化时向序列化处理流表明该类的实例是可序列化的。

由于序列化机制是 Java EE 平台的基础，因此**通常建议 Java EE 中创建的每一个 JavaBean 都应实现 Serializable 接口**。

### 2.2. Externalizable 接口

Externalizable 接口是 Serializable 接口的子类，有时我们不希望序列化那么多，可以使用这个接口，这个接口的 writeExternal() 和 readExternal() 方法可以指定序列化哪些属性。

## 3. 序列化编号

为避免内存中同一对象的重复序列化，Java 序列化机制采用了一种特殊的序列化算法，即为所有保存到磁盘中的对象都设置一个序列化编号。

当程序视图序列化一个对象时，程序会先检测该对象是否已被序列化过，只有该对象从未在本次虚拟机中被序列化过，系统才会将该对象转换成字节序列并输出。如果该对象已被序列化过，则程序只直接输出首次序列化时所设置的序列化编号，而不是重复序列化。

但这种机制可能引起一个潜在的问题：只有第一次序列化时才会将对象转换成字节序列输出，若之后该对象的实例变量值发生改变，再次序列化时也不会将改变的实例变量值进行输出。

## 4. 序列化版本

随着系统的升级，系统的 class 文件也会升级，那么 Java 如何保证两个 class 文件的兼容一致性呢？

Java 序列化机制为序列化类提供了一个 serialVersionUID 值，用于标识该 Java 类的序列化版本。通过在运行时判断 serialVersionUID 来验证版本的一致性，如果一个类升级后，只要它的 serialVersionUID 保持不变，序列化机制会将它们当成同一个序列化版本。

在进行反序列化时，JVM 会把传过来的字节流中的 serialVersionUID 与本地相应的实体（类）的 serialVersionUID 进行对比，只要其 serialVersionUID 值相同，序列化机制就会将其当成同一序列化版本，否则就会抛出异常 InvalidClassException。

serialVersionUID 有两种生成方式：默认生成和显示指定。
- 默认生成
  
  默认生成的 UID 计算方式来源于类的几个方面：
  
  类名（class name）、类及其属性的修饰符 (class modifiers)、 接口及接口顺序（interfaces）、属性（fields）、静态初始化（static initializer）， 构造器（constructors）。
  
  也就是说默认的 serialVersionUID 对类的详细信息具有较高的敏感性，这其中任何一个的改变都会影响 UID 的值，导致不兼容性。

- 显式指定
  ```java
  private static final long serialVersionUID = 1L;
  ```

那两种方式使用的情景是什么呢？应该把握一个判断原则：是否允许向下兼容。
- 默认方式使用情景：一旦创建则不允许改变。
- 显式方式使用情景：对类有一定的向下兼容性，当不允许兼容时，可以通过改变 UID 的值在实现。

**因此，强烈建议使用显式指定的方式，以防范潜在的不兼容根源，且可以带来小小的性能提升。**

## 5. 对象操作处理流

### 5.1. 对象序列化：ObjectInputStream

#### 5.1.1. 基本概念

一旦某个类实现了 Serializable 接口，该类的对象就是可序列化的，程序可以通过对象操作输出处理流 [ObjectInputStream](https://docs.oracle.com/javase/9/docs/api/java/io/ObjectInputStream.html) 来对该对象执行序列化操作。

#### 5.1.2. 常用 API

构造方法：
- `	ObjectOutputStream​(OutputStream out)`: Creates an ObjectOutputStream that writes to the specified OutputStream.

常用方法：
- `void	writeObject​(Object obj)`: Write the specified object to the ObjectOutputStream.
- `void	write​(byte[] buf)`: Writes an array of bytes.
- `void	write​(byte[] buf, int off, int len)`: Writes a sub array of bytes.
- `void	write​(int val)`: Writes a byte.
- `void	writeBoolean​(boolean val)`: Writes a boolean.
- `void	writeByte​(int val)`: Writes an 8 bit byte.
- `void	writeBytes​(String str)`: Writes a String as a sequence of bytes.
- `void	writeChar​(int val)`: Writes a 16 bit char.
- `void	writeChars​(String str)`: Writes a String as a sequence of chars.
- `void	writeDouble​(double val)`: Writes a 64 bit double.
- `void	writeFields​()`: Write the buffered fields to the stream.
- `void	writeFloat​(float val)`: Writes a 32 bit float.
- `void	writeInt​(int val)`: Writes a 32 bit int.
- `void	writeLong​(long val)`: Writes a 64 bit long.
- `void	writeShort​(int val)`: Writes a 16 bit short.
- `protected void	writeStreamHeader​()`: The writeStreamHeader method is provided so subclasses can append or prepend their own header to the stream.
- `void	writeUnshared​(Object obj)`: Writes an "unshared" object to the ObjectOutputStream.
- `void	writeUTF​(String str)`: Primitive data write of this String in modified UTF-8 format.
- `void	close​()`: Closes the stream.
- `void	flush​()`: Flushes the stream.

NOTE:

- 若一个类实现了序列化接口，那么其子类都可以进行序列化。

- 若一个实现了序列化接口的类有多个父类：
  - 若某个父类是不可序列化的，但带有无参构造器，则该父类中定义的成员变量值不会被序列化到二进制流中。
  - 若某个父类是不可序列化的，且没有无参构造器，则反序列化时会抛出 InvalidClassException 异常。

- **若一个实现了序列化接口的类中包含了引用类型的属性，则只有该属性的类是可序列化的，该类才是可序列化的**，否则会导致异常。

- **只有对象中的类名、实例变量（包括基本数据类型、数组、引用类型）才会被序列化**，类中的其它成分（如方法、静态变量、transient 变量等）都不会被序列化。

### 5.2. 对象反序列化：ObjectOutputStream

#### 5.2.1. 基本概念

如果希望从二进制流中恢复 Java 对象，程序可以通过对象操作输入处理流 [ObjectOutputStream](https://docs.oracle.com/javase/9/docs/api/java/io/ObjectOutputStream.html) 进行反序列化操作。

#### 5.2.2. 常用 API

构造方法：
- `ObjectInputStream​(InputStream in)`: Creates an ObjectInputStream that reads from the specified InputStream.

常用方法：
- `Object	readObject​()`: Read an object from the ObjectInputStream.
- `int	read​()`: Reads a byte of data.
- `int	read​(byte[] buf, int off, int len)`: Reads into an array of bytes.
- `boolean	readBoolean​()`: Reads in a boolean.
- `byte	readByte​()`: Reads an 8 bit byte.
- `char	readChar​()`: Reads a 16 bit char.
- `double	readDouble​()`: Reads a 64 bit double.
- `ObjectInputStream.GetField	readFields​()`: Reads the persistent fields from the stream and makes them available by name.
- `float	readFloat​()`: Reads a 32 bit float.
- `void	readFully​(byte[] buf)`: Reads bytes, blocking until all bytes are read.
- `void	readFully​(byte[] buf, int off, int len)`: Reads bytes, blocking until all bytes are read.
- `int	readInt​()`: Reads a 32 bit int.
- `long	readLong​()`: Reads a 64 bit long.
- `short	readShort​()`: Reads a 16 bit short.
- `Object	readUnshared​()`: Reads an "unshared" object from the ObjectInputStream.
- `int	readUnsignedByte​()`: Reads an unsigned 8 bit byte.
- `int	readUnsignedShort​()`: Reads an unsigned 16 bit short.
- `String	readUTF​()`: Reads a String in modified UTF-8 format.

NOTE:

- 若调用`readObject​()`方法读取二进制流中的对象，该方法只返回一个 Object 类型的对象，需要将对象强制类型转换成真实的类型。

- 反序列化读取的仅仅是 Java 对象的数据，而不包含 Java 类的定义。因此，进行反序列化操作时，必须提供该对象所属类的 class 文件，否则会抛出 ClassNotFoundException 异常。

- 如果执行序列化时向同一个文件中写入了多个 Java 对象，则进行反序列化恢复对象时必须按照实际写入的顺序进行读取。

- 对子类对象进行反序列化操作时，如果其父类没有实现序列化接口，那么其父类的无参构造函数会被调用；如果其父类实现了序列化接口，则可直接生成该对象而无需调用任何构造函数。

### 5.3. 实例

```java
public class Student implements Serializable {
    private String name;
    private char sex;
    private int year;
    public Student() {}
    public Student(String name, char sex, int year) {
        this.name = name;
        this.sex = sex;
        this.year = year;
    }
    // setter and getter and toString
}
```
```java
public class UseStudent {
    public static void main(String[] args) {
        Student st = new Student("Tom", 'M', 20);
        File file = new File("D:\\home\\IO\\student.txt");
        try {
            file.createNewFile();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try ( // 用于 Student 对象序列化
              ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file));
              // 用于 Student 对象反序列化
              ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file))) {
            // 对象序列化
            oos.writeObject(st);
            oos.flush();
            // 对象反序列化
            Student st1 = (Student) ois.readObject();
            System.out.println(st1.toString());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 6. 自定义序列化

当对某个对象进行序列化时，系统会自动把该对象的所有实例变量依次进行序列化。但存在这样的情景：如果一个类中包含某些涉及敏感信息的实例变量，或某个实例变量的类型是不可序列化的，我们希望系统在对该类的对象进行序列化时，不将这些实例变量进行序列化，此时就需要自定义序列化了。

### 6.1. 使用 transient 关键字

通过在实例变量前使用 transient 关键字修饰，可以指定 Java 序列化时不对该实例变量进行操作。

NOTE:
- transient 关键字只能用于修饰实例变量，不可修饰 Java 类中的其它成分。
- 事实上，使用 static 和 transient 关键字修饰的属性，都不会被序列化，但 transient 是专用于自定义序列化的关键字。

### 6.2. 提供特殊签名的方法

Java 还提供了一种自定义序列化机制，只要在需要自定义序列化 / 反序列化的类中提供以下特殊签名的方法，在系统进行序列化操作时就会**优先**按照自定义的规则进行序列化：
- `private void writeObject(java.io.ObjectOutputStream out) throws IOException`
- `private void readObject(java.io.ObjectInputStream in) throws IOException, ClassNotFoundException`

NOTE: writeObject 方法和 readObject 方法中存储和恢复的实例变量的顺序必须一致，否则不能正常的恢复该 Java 对象。

例：
```java
public class Student implements Serializable {
	private String stuno;
	private String stuname;

	// 该元素不会进行 jvm 默认的序列化，可以自定义该元素的序列化
	private transient int stuage;  
	
	public Student(String stuno, String stuname, int stuage) {
		super();
		this.stuno = stuno;
		this.stuname = stuname;
		this.stuage = stuage;
	}

  // setter and getter and toString

	private void writeObject(java.io.ObjectOutputStream s)
		        throws java.io.IOException {
		 s.defaultWriteObject();// 把 jvm 能默认序列化的元素进行序列化操作
		 s.writeInt(stuage);// 自己完成 stuage 的序列化
	}

	private void readObject(java.io.ObjectInputStream s)
		        throws java.io.IOException, ClassNotFoundException {
		  s.defaultReadObject();// 把 jvm 能默认反序列化的元素进行反序列化操作
		  this.stuage = s.readInt();// 自己完成 stuage 的反序列化操作
	}
}
```

### 6.3. 实现 Externalizable 接口

Java 还提供了一种完全由程序员决定存储和恢复对象数据的自定义序列化机制，要实现该机制，Java 类需要实现 Externalizable 接口，并实现该接口中的以下方法：
- `void readExternal(ObjectInput in) throws IOException, ClassNotFoundException`
- `void writeExternal(ObjectOutput out) throws IOException`

实际上，这种自定义序列化的方式与其它方式非常相似，区别在于实现 Serializable 接口的类不强制要求自定义序列化，而实现 Externalizable 接口后强制要求自定义序列化。

虽然实现 Externalizable 接口能带来一定的性能提升，但由于实现 Externalizable 接口后导致编程复杂性的增加，因此大部分时候还是采用实现 Serializable 接口的方法来实现序列化。

## 7. Refer Links

[深入学习 Java 序列化](http://beautyboss.farbox.com/post/study/shen-ru-xue-xi-javaxu-lie-hua)

[Java 序列化](https://www.cnblogs.com/xzwblog/p/7263170.html)

[Effective Java -- 序列化 -- 你以为只要实现 Serializable 接口就行了吗](https://blog.csdn.net/smileiam/article/details/75315005)

[深度解析 JAVA 序列化](https://juejin.im/post/590bfc00ac502e006cdd2886#heading-17)