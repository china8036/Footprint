- [Java Unsafe 类](#java-unsafe-%E7%B1%BB)
  - [1. Refer Links](#1-refer-links)

# Java Unsafe 类

Java 无法直接访问底层操作系统，而是通过本地（native）方法来访问。不过尽管如此，JVM 还是开了一个后门，JDK 中有一个类 Unsafe，它提供了硬件级别的原子操作。

这个类尽管里面的方法都是 public 的，但是并没有办法使用它们，JDK API 文档也没有提供任何关于这个类的方法的解释。总而言之，对于 Unsafe 类的使用都是受限制的，只有授信的代码才能获得该类的实例，当然 JDK 库里面的类是可以随意使用的。

从第一行的描述可以了解到 Unsafe 提供了硬件级别的操作，比如说获取某个属性在内存中的位置，比如说修改对象的字段值，即使它是私有的。不过 Java 本身就是为了屏蔽底层的差异，对于一般的开发而言也很少会有这样的需求。

eg:
```java
public native long staticFieldOffset(Field paramField);
```
这个方法可以用来获取给定的 paramField 的内存地址偏移量，这个值对于给定的 field 是唯一的且是固定不变的。

eg:
```java
public native int arrayBaseOffset(Class paramClass); 
public native int arrayIndexScale(Class paramClass);
```
前一个方法是用来获取数组第一个元素的偏移地址，后一个方法是用来获取数组的转换因子即数组中元素的增量地址的。

eg:
```java
allocateMemory()
reallocateMemory()
freeMemory()
```
分别用来分配内存，扩充内存和释放内存的。

eg: 
```java
public final native boolean compareAndSwapObject(Object paramObject1, long paramLong, Object paramObject2, Object paramObject3);
public final native boolean compareAndSwapInt(Object paramObject, long paramLong, int paramInt1, int paramInt2);
public final native boolean compareAndSwapLong(Object paramObject, long paramLong1, long paramLong2, long paramLong3);
```
原子操作 CAS 的实现，java.util.concurrent.atomic包下的原子操作类都是基于CAS实现的。

## 1. Refer Links