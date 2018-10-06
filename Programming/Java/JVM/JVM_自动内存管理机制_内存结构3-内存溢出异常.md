- [JVM 内存溢出异常](#jvm-%E5%86%85%E5%AD%98%E6%BA%A2%E5%87%BA%E5%BC%82%E5%B8%B8)
  - [1. Java 堆溢出](#1-java-%E5%A0%86%E6%BA%A2%E5%87%BA)
  - [2. Java 虚拟机栈 / 本地方法溢出](#2-java-%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%A0%88-%E6%9C%AC%E5%9C%B0%E6%96%B9%E6%B3%95%E6%BA%A2%E5%87%BA)
  - [3. 方法区溢出](#3-%E6%96%B9%E6%B3%95%E5%8C%BA%E6%BA%A2%E5%87%BA)
    - [3.1. 运行时常量池溢出](#31-%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0%E6%BA%A2%E5%87%BA)
    - [3.2. 类信息溢出](#32-%E7%B1%BB%E4%BF%A1%E6%81%AF%E6%BA%A2%E5%87%BA)
  - [4. 本机直接内存溢出](#4-%E6%9C%AC%E6%9C%BA%E7%9B%B4%E6%8E%A5%E5%86%85%E5%AD%98%E6%BA%A2%E5%87%BA)
  - [5. Refer Links](#5-refer-links)

# JVM 内存溢出异常

在 Java SE 虚拟机规范的描述中，除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生 OutOfMemoryError 即 OOM 异常的可能。对于开发者，需要能在工作中遇到实际的 OOM 异常时，能够根据异常的信息快速判断是哪个内存区域的 OOM，知道什么样的代码可能会导致这些区域的 OOM，以及出现这些异常后应该如何处理和解决。

## 1. Java 堆溢出

Java 堆用于存储对象实例，因此**只要不断地创建对象，并且保证 GC Roots 到对象之间有可达路径来避免 GC 回收这些对象，那么在对象数量达到堆的最大容量限制后就会产生内存溢出异常**。

以下代码限制堆大小为 20MB 且不可扩展，并设置 JVM 在出现 OOM 异常时 Dump 出当前的内存堆转储快照以便事后进行分析：
```java
//java -Xms20m -Xmx20m -XX:+HeapDumpOnOutOfMemoryError
List<Object> list = new ArrayList<Object>();
while (true) {
    list.add(new HelloBean());
}
```

Java 堆内存的 OOM 异常是实际应用中非常常见的异常情况，当异常出现时，异常堆栈信息 `java.lang.OutOfMemoryError` 会跟着进一步提示 `Java heap space`。

要解决 Java 堆的 OOM 异常，一般的手段是闲通过内存映像分析工具堆 Dump 出来的堆转储快照进行分析，重点是确认内存中的对象是否是必要的，也就是要先弄清楚到底是出现了内存泄露 (Memory Leak) 还是内存溢出 (Memory Overflow)：
- 内存泄露
  
  内存泄露是指分配出去的内存没有被回收回来，由于失去了对该内存区域的控制，因而造成了资源的浪费。Java 中一般不会产生内存泄露，因为有垃圾回收器自动回收垃圾，但这也不绝对，**当我们 new 了对象，并保存了其引用，但是后面一直没用它，而垃圾回收器又不会去回收它，便会造成内存泄露**。

  如果是内存泄漏，可进一步通过工具查看泄漏对象到 GC Roots 的引用链。于是就能找到泄露对象是通过怎样的路径与 GC Roots 相关联并导致垃圾收集器无法自动回收它们的。掌握了泄露对象的类型信息及 GC Roots 引用链的信息，就可以比较准确地定位出泄露代码的位置。

- 内存溢出

  内存溢出是指程序所需要的内存超出了系统所能分配的内存（包括动态扩展）的上限。

  如果不存在泄露而却发生了内存溢出，换句话说，就是**内存中的对象确实都必须存活着但可用堆内存告罄**，那就应当检查虚拟机的堆参数（-Xmx 与 -Xms），与机器物理内存对比看是否还可以调大；从代码上检查是否存在某些对象生命期过长、持有状态时间过长的情况，尝试减少程序运行期的内存消耗。

## 2. Java 虚拟机栈 / 本地方法溢出

Java 栈中存放的是一个个的栈帧，每个栈帧对应一个被调用的方法。关于虚拟机栈和本地方法栈，在 Java 虚拟机规范中描述了两种异常：
- 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出 StackOverflowError 异常。
- 如果虚拟机允许动态扩展（当前大多数 VM 实现都支持），在动态扩展栈时无法申请到足够的内存空间，则抛出 OutOfMemoryError 异常。
这两种情况存在着一些互相重叠的地方：当栈空间无法继续分配时，到底是内存太小，还是已使用的栈空间太大，其本质上只是对同一件事情的两种描述而已。

NOTE: 由于在 HotSpot 虚拟机中并不区分虚拟机栈和本地方法栈，因此，对于 HotSpot 来说，虽然 -Xoss 参数（用于设置本地方法栈大小的参数）存在，但实际上是没有效果的，栈容量只由 -Xss 参数设置。

- 单线程测试

  在单线程的操作中，无论是定义大量的本地变量以大幅度增大栈帧，还是使用 -Xss 参数减少栈内存的容量，当栈空间无法分配时，虚拟机抛出的都是 StackOverflowError 异常，而无法得到 OutOfMemoryError 异常。
  
  以下代码使用 `-Xss` 参数减少栈内存的容量，并通过无限递归占满 Java 虚拟机栈：
  ```java
  // java -Xss128k
  public class VMStackSOF {
    private int stackLength = 1;

    // 无限递归
    private void stackLeak() {
      stackLength ++;
      stackLeak();
    }

    public static void main(Stirng [] args) throws Exception {
      VMStackSOF oom = new VMStackSOF();
      try {
        oom.stackLeak();
      } catch (Exception e) {
        System.out.println(oom.stackLength);
        throw e;
      }
    }
  }
  ```

- 多线程测试

  在多线程环境下，通过不断建立线程的方式就可以使得程序抛出 OutOfMemoryError 异常，但这样产生的 OOM 溢出与栈空间是否够大并没有任何关系。
  - 因为，**在多线程情况下，给每个线程的栈分配的内存越大，反而越容易产生内存溢出异常**。操作系统为每个进程分配的内存是有限制的，虚拟机提供了参数来控制 Java 堆和方法区这两部分内存的最大值，忽略掉程序计数器消耗的内存（很小），以及进程本身消耗的内存，剩下的内存便给了虚拟机栈和本地方法栈，**每个线程分配到的栈容量越大，可以建立的线程数量自然就越少**。

  因此，如果是建立过多的线程导致的内存溢出，在不能减少线程数或者更换为 64 位虚拟机的情况下，就只能通过减少最大堆和减少每个线程的栈容量来换取更多的线程。这种通过“减少内存”来解决 OOM 异常的方式通常较难想到。

## 3. 方法区溢出

方法区存放的是已经被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码（类方法）等数据。由于一个类要被垃圾器回收掉，判定条件是比较严苛的，因此方法区溢出也是一种常见的内存溢出异常。在经常动态产生大量 Class 的应用中，需要特别注意类的回收状况。这类场景常见的有：大量 JSP 或者动态产生 JSP 文件的应用（每个 JSP 文件都会被编译成 Servlet 类）、基于 OSGi 的应用（同一个类被不同加载器加载也会是为不同的类）等。

### 3.1. 运行时常量池溢出

以下代码通过 `-XX:PermSize` 和 `-XX:MaxPermSize` 限制方法区大小，从而间接限制其中的常量池的容量：
```java
// java -XX:PermSize=10M -XX:MaxPermSize=10M
List<String> list = new ArrayList<>();
int i = 0;
while(true) {
  list.add(String.valueOf(i++).intern());
}
```
在 JDK1.6 中，以上代码运行后抛出 `java.lang.OutOfMemoryError: PermGen space`，也说明了运行时常量池属于方法区（HotSpot VM 的永久代）的一部分。但在 JDK1.7 中，以上代码会一直运行下去。

关于这个差异的原因，见以下测试代码：
```java
public static void main(String[] args) { 
    String str1 = new StringBuilder("计算机").append("软件").toString();
    System.out.println(str1.intern() == str1);
    
    String str2 = new StringBuilder("ja").append("va").toString();
    System.out.println(str2.intern() == str2);
}
```
这段代码在 JDK1.6 中运行，会得到两个 false。而在 JDK1.7 中运行，会得到一个 true 和一个 false。

原因是：
- `String.intern()` 是一个 Native 方法，它的作用是：如果字符串常量池中已经包含了一个等于此 String 对象的字符串，则返回代表池中这个字符串的 String 对象；否则，将此 String 对象包含的字符串添加到常量池中，并且返回此字符串的引用。
- 在 JDK1.6 中 intern() 方法会把首次遇到的字符串实例复制到永久代中，返回的也是永久代中这个字符串实例的引用，而由 StringBuilder 创建的字符串实例在 Java 堆上，所以必然不是一个引用。
- 在 JDK1.7 中 intern() 方法不会复制实例，只是在常量池中记录首次出现的实例引用，因此 intern() 返回的引用和由 StringBuilder 创建的字符串实例是同一个。str2 返回 false 是因为 Java 这个字符串在执行 StringBuilder("ja").append("va").toString() 之前已经出现过，字符串常量池中已经有它的引用了，不符合首次出现的原则，而"计算机软件"这个字符串是首次出现的，因此返回 true。

### 3.2. 类信息溢出

方法区用于存放 Class 的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。对于这些区域的测试，基本的思路是运行时产生大量的类去填满方法区，直到溢出。

以下代码借助 CGLib 直接操作字节码运行时生成大量的动态类：
```java
// java -XX:PermSize=10M -XX:MaxPermSize=10M
public class JavaMethodAreaOOM {
    public static void main(final String[] args) {
        while(true) {
            Enhancer enhancer = new Enhancer();
            enhancer.setSuperclass(OOMObject.class);
            enhancer.setUseCache(false);
            enhancer.setCallback(new MethodInterceptor() {
                @Override
                public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                    return methodProxy.invokeSuper(o, args);
                }
            });
            enhancer.create();
        }
    }

    static class OOMObject {
    }
}
```
以上代码运行后抛出 `java.lang.OutOfMemoryError: PermGen space`。

## 4. 本机直接内存溢出

<!-- TODO: -->

DirectMemory 容量可以通过 `-XX:MaxDirectMemorySize` 指定，如果不指定，则默认与 Java 堆最大值（-Xmx 指定）一样。

以下代码越过了 DirectByteBuffer 类，直接通过反射获取 Unsafe 实例进行内存分配（Unsafe 类的 getUnsafe 方法限制了只有引导类加载器才会返回实例，也就是设计者希望只有 rt.jar 中的类才能使用 Unsafe 的功能）。因为，虽然使用 DirectByteBuffer 分配内存也会抛出内存异常，但它抛出异常时并没有真正向操作系统申请内存分配，而是通过计算得知内存无法分配，于是手动抛出异常，真正申请分配内存的方法是 unsafe.allocateMemory。
```java
/**
 * VM Args: -Xmx20M -XX:MaxDirectMemorySize=10M
 */
public class DirectMemoryOOM {
    private static final int _1MB = 1024*1024;
    public static void main(String[] args) throws IllegalAccessException {
        Field unsafeField = Unsafe.class.getDeclaredFields()[0];
        unsafeField.setAccessible(true);
        Unsafe unsafe = (Unsafe) unsafeField.get(null);
        while(true) {
            unsafe.allocateMemory(_1MB);
        }
    }
}
```
由 DirectMemory 导致的内存溢出，一个明显的特征是在 Heap Dump 文件中不会看见明显的异常，如果读者发现 OOM 之后 Dump 文件很小，而程序中又直接或者间接使用了 NIO，那就可以考虑检查一下是不是这方面的原因。

## 5. Refer Links

《深入理解 Java 虚拟机》 周志明 著

[Java 内存溢出 (OOM) 异常完全指南](https://www.jianshu.com/p/2fdee831ed03)