- [Java 反射 - 动态类加载](#java-%E5%8F%8D%E5%B0%84---%E5%8A%A8%E6%80%81%E7%B1%BB%E5%8A%A0%E8%BD%BD)
  - [1. 类加载机制](#1-%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6)
  - [2. 类加载器](#2-%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8)
  - [3. 动态类加载（自定义类加载器)](#3-%E5%8A%A8%E6%80%81%E7%B1%BB%E5%8A%A0%E8%BD%BD%EF%BC%88%E8%87%AA%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8)
    - [3.1. 步骤](#31-%E6%AD%A5%E9%AA%A4)
    - [3.2. 示例](#32-%E7%A4%BA%E4%BE%8B)
    - [3.3. 应用](#33-%E5%BA%94%E7%94%A8)
      - [3.3.1. 代码混淆器](#331-%E4%BB%A3%E7%A0%81%E6%B7%B7%E6%B7%86%E5%99%A8)
  - [4. Refer Links](#4-refer-links)

# Java 反射 - 动态类加载

当程序主动使用某个类时，如果该类还未被加载到内存中，则 JVM 会对该类进行**加载、连接、初始化**三个阶段的操作。

其中第一个阶段，**类加载**指的是将类的 class 文件读取到内存，并为之创建一个 java.lang.Class 实例对象的过程。也就是说，当程序中使用任何类时，系统都会为之建立一个 java.lang.Class 对象。因此，**类加载阶段也属于反射技术应用的一个部分**。

**类加载阶段的操作由类加载器完成，类加载器通常由 JVM 提供，这些类加载器是所有程序运行的基础。**

## 1. 类加载机制

类加载器通常无须等到首次使用时才加载该类，JVM 规范允许系统预先加载某些类，但 JVM 启动时，也不会一次性加载所有的 class 文件，而是根据需要去动态加载。这其中**具体的加载规则，就称为类加载机制**。

不同的类加载器可以从不同的来源加载类的二进制数据，通常包括以下来源：
- 从本地文件系统中加载 class 文件。这是绝大部分程序的类加载方式。
- 从 jar 包加载 class 文件。这种方式也是很常见的，如 JDBC 编程中数据库驱动类就放在 jar 包文件中，JVM 可以从 jar 文件中直接加载该 class 文件。
- 通过网络加载 class 文件。
- 动态编译 Java 源文件，并执行加载。

一旦一个类被加载到 JVM 中，同一类就不会再次被加载了。那么，怎么样才算是同一个类？
- 在 Java 中，一个类用其全限定类名（包名 + 类名）作为唯一标识。
- 而在 JVM 中，一个类用其全限定类名和其类加载器作为唯一标识。因此，不同类加载器加载的同名类是互不相同的。

JVM 的类加载机制主要有以下 3 种：
- 全盘负责。当一个类加载器负责加载某个 Class 时，该 Class 所依赖的和引用的其它 Class 也将全部由该类加载器负责载入，除非显式使用另外一个类加载器来载入。
- 父类委托。在执行类加载时，先让父类加载器尝试加载该 Class，只有在父类加载器无法加载该类时，才尝试从自己的类路径中加载该类。
- 缓存机制。当程序中需要使用某个 Class 时，类加载器先从缓存区中寻找该 Class，只有当缓存区中不存在该 Class 对象时，系统才会读取该类对应的二进制数据，并将其转化为 Class 对象存入缓存区中。

## 2. 类加载器

类加载器负责在类的加载阶段加载所需要的类，即将`.class`文件加载到内存中，并为之生成相应的 java.lang.Class 实例对象。

JVM 中包含了 3 个内置的类加载器，当 JVM 启动时，会形成由这 3 个类加载器组成的初始类加载器层次结构：
- Bootstrap CLassloder

  引导 / 根类加载器，是最顶层的类加载器。它负责加载 Java 的核心类（`%JRE_HOME%\lib` 下的 rt.jar、resources.jar、charsets.jar 和 classes 等）。可以通过启动 jvm 时指定 -Xbootclasspath 和路径来改变 Bootstrap ClassLoader 的加载目录。

  根类加载器非常特殊，它并不是 java.lang.ClassLoader 的子类，而是由 JVM 自身实现的，且是唯一使用非 Java 语言（C++）实现的类加载器，因此无法在 java 代码中获取它的引用。

- Extention ClassLoader

  扩展类加载器。它负责加载 JRE 的扩展目录（`%JRE_HOME%\lib\ext` 或由 `java.ext.dirs` 系统属性指定的目录）中的 jar 包和 class 文件。

- SystemAppClass(AppClassLoader)

  系统 / 应用类加载器。它负责在 JVM 进程启动时，加载来自当前 Java 程序的 java 命令的 `-classpath` 选项、`java.class.path` 系统属性、CLASSPATH 环境变量所指定的 jar 包和类。

## 3. 动态类加载（自定义类加载器)

<!-- TODO: 动态类加载 的定义是什么？动态类重载？ -->

对于 JVM 中内置的类加载器，不管 BootStrap ClassLoader、ExtClassLoader、AppClassLoader 都是加载指定路径下的 jar 包。**如果我们要突破 JDK 系统内置加载路径的限制，实现自己某些特殊的需求，我们就得自定义 classLoader**，指定加载的路径（可以是磁盘、内存、网络等）。

### 3.1. 步骤

1. 编写一个类，继承 [ClassLoader](https://docs.oracle.com/javase/9/docs/api/java/lang/ClassLoader.html) 抽象类。

    JVM 中除根类加载器之外，所有的类加载器都是 ClassLoader 类的子类实例，开发者可以通过扩展 ClassLoader 类来实现自定义的类加载器。

    NOTE: 如果自定义一个 ClassLoader，默认的 parent 父加载器是 AppClassLoader，因为这样就能够保证它能访问系统内置加载器加载成功的 class 文件。

1. 重写 ClassLoader 抽象类 的 findClass() 方法，并在 findClass() 方法中调用 defineClass() 将字节码文件转换成 Class 对象，然后返回这个 Class 对象。

    ClassLoader 类中有以下 3 个关键方法：
    - `protected Class<?>	loadClass​(String name, boolean resolve)`: Loads the class with the specified binary name.
    - `protected Class<?>	findClass​(String name)`: Finds the class with the specified binary name.
    - `protected Class<?>	defineClass​(String name, byte[] b, int off, int len)`: Converts an array of bytes into an instance of class Class. **Before the Class can be used it must be resolved**.

    通常情况下，loadClass​() 方法中会调用 findClass​() 方法，而 findClass​() 方法中会调用 defineClass​() 方法。
    
    若要实现自定义的类加载器，可以通过重写以上任意方法来实现。但**通常只推荐重写 findClass() 方法，而不是重写 loadClass() 方法或 defineClass​() 方法**。
    - 因为 loadClass() 方法中包含了默认类加载器的父类委托和缓存机制两种策略，若重写该方法，实现逻辑非常复杂；defineClass​() 方法将 class 二进制内容转换成 Class 对象，如果不符合要求的会抛出各种异常，实现逻辑更加复杂。

### 3.2. 示例

自定义 classloader，加载路径为`D:\lib`下的 jar 包和资源。

Test.java
```java
public class Test {
    public void say(){
        System.out.println("Say Hello");
    }
}
```
将其编译成 Test.class 放到 D:\lib 这个路径下。

自定义类加载器：
```java
public class DiskClassLoader extends ClassLoader {

    private String mLibPath;

    public DiskClassLoader(String path) {
        // TODO Auto-generated constructor stub
        mLibPath = path;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        // 查找 class 文件的逻辑
        String fileName = getFileName(name);
        File file = new File(mLibPath,fileName);
        try {
            FileInputStream is = new FileInputStream(file);
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            int len = 0;
            try {
                while ((len = is.read()) != -1) {
                    bos.write(len);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            byte[] data = bos.toByteArray();
            is.close();
            bos.close();

            // 将 class 文件转化为 Class 对象
            return defineClass(name, data, 0, data.length);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

        return super.findClass(name);
    }

    // 获取要加载 的 class 文件名
    private String getFileName(String name) {
        // TODO Auto-generated method stub
        int index = name.lastIndexOf('.');
        if(index == -1){ 
            return name+".class";
        }else{
            return name.substring(index)+".class";
        }
    }
}
```

测试代码：
```java
public class ClassLoaderTest {
    public static void main(String[] args) {
        // 创建自定义 classloader 对象。
        DiskClassLoader diskLoader = new DiskClassLoader("D:\\lib");
        try {
            // 加载 class 文件
            Class c = diskLoader.loadClass("com.frank.test.Test");

            if(c != null) {
                try {
                    Object obj = c.newInstance();
                    Method method = c.getDeclaredMethod("say",null);
                    // 通过反射调用 Test 类的 say 方法
                    method.invoke(obj, null);
                } catch (InstantiationException | IllegalAccessException | NoSuchMethodException | SecurityException | IllegalArgumentException | InvocationTargetException e) {
                    e.printStackTrace();
                }
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.3. 应用

使用自定义的类加载器，通常可以实现以下功能：
- 执行代码前自动验证数字签名。
- 根据用户提供的密码解密代码，从而实现代码混淆器来避免字节码文件被反编译。
- 根据用户需求来动态地加载类。
- 根据应用需求把其它数据以字节码形式加载到应用中。

#### 3.3.1. 代码混淆器

将 Class 文件按照某种加密手段进行加密，然后按照规则编写自定义的 ClassLoader 进行解密，这样我们就可以在程序中加载特定了类，并且**这个类只能被我们自定义的加载器进行加载，提高了程序的安全性**。

加密和解密的协议有很多种，具体怎么定看业务需要。以下为了便于演示，简单地将加密解密定义为异或运算。当一个文件进行异或运算后，产生了加密文件，再进行一次异或后，就进行了解密。

编写加密工具类：
```java
public class FileUtils {
    public static void test(String path){
        File file = new File(path);
        try {
            FileInputStream fis = new FileInputStream(file);
            // 加密后的字节码文件后缀名为 .classen
            FileOutputStream fos = new FileOutputStream(path+"en");
            int b = 0;
            int b1 = 0;
            try {
                while((b = fis.read()) != -1){
                    // 每一个 byte 异或一个数字 2
                    fos.write(b ^ 2);
                }
                fos.close();
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
对字节码文件进行加密，生成 Test.classen 文件
```java
FileUtils.test("D:\\lib\\Test.class");
```
自定义解密类加载器：
```java
public class DeClassLoader extends ClassLoader {

    private String mLibPath;

    public DeClassLoader(String path) {
        // TODO Auto-generated constructor stub
        mLibPath = path;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        String fileName = getFileName(name);
        File file = new File(mLibPath,fileName);
        try {
            FileInputStream is = new FileInputStream(file);
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            int len = 0;
            byte b = 0;
            try {
                while ((len = is.read()) != -1) {
                    // 将数据异或一个数字 2 进行解密
                    b = (byte) (len ^ 2);
                    bos.write(b);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            byte[] data = bos.toByteArray();
            is.close();
            bos.close();

            return defineClass(name,data,0,data.length);

        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

        return super.findClass(name);
    }

    // 获取要加载 的 class 文件名
    private String getFileName(String name) {
        // TODO Auto-generated method stub
        int index = name.lastIndexOf('.');
        if(index == -1){ 
            return name+".classen";
        }else{
            return name.substring(index+1)+".classen";
        }
    }
}
```
编写测试代码：
```java
public class ClassLoaderTest {
    public static void main(String [] args) {
        DeClassLoader diskLoader = new DeClassLoader("D:\\lib");
        try {
            // 加载 class 文件
            Class c = diskLoader.loadClass("com.frank.test.Test");
            if(c != null){
                try {
                    Object obj = c.newInstance();
                    Method method = c.getDeclaredMethod("say",null);
                    // 通过反射调用 Test 类的 say 方法
                    method.invoke(obj, null);
                } catch (InstantiationException | IllegalAccessException | NoSuchMethodException | SecurityException | IllegalArgumentException | InvocationTargetException e) {
                    e.printStackTrace();
                }
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## 4. Refer Links

[深入理解 Java 类加载器 (ClassLoader)](http://blog.csdn.net/javazejian/article/details/73413292)

[一看你就懂，超详细 java 中的 ClassLoader 详解](http://blog.csdn.net/briblue/article/details/54973413#comments)

[Java Reflection（十二): 动态类加载与重载](http://ifeve.com/dynamic-class-loading-reloading/)