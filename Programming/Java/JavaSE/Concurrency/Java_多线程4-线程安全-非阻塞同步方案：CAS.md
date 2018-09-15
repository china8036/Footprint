- [Java 多线程 - 线程安全 - 非阻塞 / 无锁同步方案](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---%E9%9D%9E%E9%98%BB%E5%A1%9E-%E6%97%A0%E9%94%81%E5%90%8C%E6%AD%A5%E6%96%B9%E6%A1%88)
  - [1. CAS](#1-cas)
    - [1.1. CPU 指令支持](#11-cpu-%E6%8C%87%E4%BB%A4%E6%94%AF%E6%8C%81)
    - [1.2. 运算过程](#12-%E8%BF%90%E7%AE%97%E8%BF%87%E7%A8%8B)
  - [2. Unsafe 类](#2-unsafe-%E7%B1%BB)
    - [2.1. 获取实例](#21-%E8%8E%B7%E5%8F%96%E5%AE%9E%E4%BE%8B)
    - [2.2. 常用 API](#22-%E5%B8%B8%E7%94%A8-api)
  - [3. 原子操作类](#3-%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C%E7%B1%BB)
    - [3.1. 原子基本类型](#31-%E5%8E%9F%E5%AD%90%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B)
      - [3.1.1. 常用 API](#311-%E5%B8%B8%E7%94%A8-api)
      - [3.1.2. 实现原理](#312-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
    - [3.2. 原子引用类](#32-%E5%8E%9F%E5%AD%90%E5%BC%95%E7%94%A8%E7%B1%BB)
      - [3.2.1. 常用 API](#321-%E5%B8%B8%E7%94%A8-api)
      - [3.2.2. 实现原理](#322-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
    - [3.3. 原子数组类型](#33-%E5%8E%9F%E5%AD%90%E6%95%B0%E7%BB%84%E7%B1%BB%E5%9E%8B)
      - [3.3.1. 常用 API](#331-%E5%B8%B8%E7%94%A8-api)
      - [3.3.2. 实现原理](#332-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
    - [3.4. 原子属性更新类型](#34-%E5%8E%9F%E5%AD%90%E5%B1%9E%E6%80%A7%E6%9B%B4%E6%96%B0%E7%B1%BB%E5%9E%8B)
      - [3.4.1. 常用 API](#341-%E5%B8%B8%E7%94%A8-api)
      - [3.4.2. 实现原理](#342-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
  - [4. ABA 问题](#4-aba-%E9%97%AE%E9%A2%98)
    - [4.1. AtomicStampedReference 类](#41-atomicstampedreference-%E7%B1%BB)
    - [4.2. AtomicMarkableReference 类](#42-atomicmarkablereference-%E7%B1%BB)
  - [5. Refer Links](#5-refer-links)

# Java 多线程 - 线程安全 - 非阻塞 / 无锁同步方案

**无锁同步方案**是实现线程安全的另一种手段，它是一种基于冲突检测的乐观并发策略，总是认为**对共享资源的访问不会发生冲突，线程可以不停执行，无需加锁，无需等待；万一发现冲突，无锁策略就采用一种称为 CAS 的技术来保证线程执行的安全性**，这项 CAS 技术就是无锁策略实现的关键。

无锁同步方法在保证同步时无须将线程挂起等待，因此也称为**非阻塞同步方案** (Non-Blocking Synchronization)。

## 1. CAS

### 1.1. CPU 指令支持

在并发安全控制时，我们需要操作和冲突检测这两个步骤具备原子性，且只能靠硬件来完成，即通过硬件保证一个语义上看起来需要多次操作的行为只通过一条处理器指令就能完成。这类常用的指令有：
- 测试并设置（Test-and-Set）
- 获取并增加（Fetch-and-Increment）
- 交互（swap）
- 比较并交换（Compare-and-Swap, CAS）
- 加载链接 / 条件存储（Load-Linked/Store-Conditional, LL/SC）

其中，前 3 条指令是 20 世纪就已存在于大多数指令集中的处理器指令，随着硬件指令集的发展，在现代处理器中也新增后边 2 条指令，而 CAS 指令的就是实现是无锁策略实现的关键。

### 1.2. 运算过程

CAS 操作的运算过程如下：
> CAS(V,E,N)
参数说明：
- V 表示要更新的目标变量
- E 表示预期的旧值
- N 表示要设置的新值
如果 V 值等于 E 值，则将 V 的值设为 N，然后返回旧值。若 V 值和 E 值不同，则说明已经有其他线程做了更新，则当前线程什么都不做，但还是会返回旧值。

也就是说，CAS 操作需要我们提供一个期望值，当期望值与当前线程的变量值相同时，说明还没线程修改该值，当前线程可以进行修改，也就是执行 CAS 操作，但如果期望值与当前线程不符，则说明该值已被其他线程修改，此时不执行更新操作，但可以选择重新读取该变量再尝试再次修改该变量，也可以放弃操作。

基于这样的原理，CAS 操作即使没有锁，同样知道其他线程对共享资源操作影响，并执行相应的处理措施。同时从这点也可以看出，**由于无锁操作中没有锁的存在，因此不可能出现死锁的情况，也就是说无锁操作天生免疫死锁**。

## 2. Unsafe 类

sun.misc.Unsafe 至少从 2004 年 JDK1.4 开始就存在于 Java 中了，但在 Java9 中，为了提高 JVM 的可维护性和安全性，Unsafe 类被作为内部使用类隐藏了起来，即将原本的 Unsafe 类定义转移到了 jdk.internal.misc 包下，再在对外提供的 sun.misc.Unsafe 类中使用 jdk.internal.misc.Unsafe 对象。

### 2.1. 获取实例

查看 sun.misc.Unsafe 与 jdk.internal.misc.Unsafe 的源码：
```java
// sun.misc.Unsafe
public final class Unsafe {

    static {
        Reflection.registerMethodsToFilter(Unsafe.class, "getUnsafe");
    }

    private Unsafe() {}

    private static final Unsafe theUnsafe = new Unsafe();
    private static final jdk.internal.misc.Unsafe theInternalUnsafe = jdk.internal.misc.Unsafe.getUnsafe();

    @CallerSensitive
    public static Unsafe getUnsafe() {
        Class<?> caller = Reflection.getCallerClass();
        if (!VM.isSystemDomainLoader(caller.getClassLoader()))
            throw new SecurityException("Unsafe");
        return theUnsafe;
    }
}
```
```java
// jdk.internal.misc.Unsafe
public final class Unsafe {

    private static native void registerNatives();
    static {
        registerNatives();
    }

    private Unsafe() {}

    private static final Unsafe theUnsafe = new Unsafe();

    public static Unsafe getUnsafe() {
        return theUnsafe;
    }
}
```
可知，sun.misc.Unsafe 与 jdk.internal.misc.Unsafe 都只包含了一个私有构造方法，且在 sun.misc.Unsafe 的 getUnsafe() 方法中，限制了只有启动类加载器 (bootstrap class loader) 加载的 Class 才能够获取其实例对象。因此，**只能通过反射的方法来获取获取到该成员属性 theUnsafe 对应的 Field 对象，并将其设置为可访问，从而得到 theUnsafe 具体对象**。
```java
// import sun.misc.Unsafe;
public static void main(String[] args) throws NoSuchFieldException,
        SecurityException, IllegalArgumentException, IllegalAccessException {
    // 通过反射得到 theUnsafe 对应的 Field 对象
    Field field = Unsafe.class.getDeclaredField("theUnsafe");
    // 设置该 Field 为可访问
    field.setAccessible(true);
    // 通过 Field 得到该 Field 对应的具体对象，传入 null 是因为该 Field 是 static 的
    Unsafe unsafe = (Unsafe) field.get(null);
    System.out.println(unsafe);
}
```

事实上，JDK 提供的许多类实现都是对 Unsafe 类方法的二次封装，因此，我们还可以通过其它 Java API 来间接使用 Unsafe。例如，J.U.C 包下 AtomicInteger 类的 compareAndSet() 和 compareAndIncrement() 等方法其实现都使用了 Unsafe 类中提供的 CAS 操作。

### 2.2. 常用 API

jdk.internal.misc.Unsafe 类中几乎所有方法都是 native 修饰的，也就是说 Unsafe 类中的方法都直接调用操作系统底层资源执行相应任务，其提供了以下特性：
- CAS 操作
  ```java
  // 第一个参数 o 为给定对象，offset 为对象内存的偏移量，通过这个偏移量迅速定位字段并设置或获取该字段的值
  //expected 表示期望值，x 表示要设置的值，下面 3 个方法都通过 CAS 原子指令执行操作
  public final native boolean compareAndSwapObject(Object o, long offset,Object expected, Object x);                                                                                                  
  public final native boolean compareAndSwapInt(Object o, long offset,int expected,int x);

  public final native boolean compareAndSwapLong(Object o, long offset,long expected,long x);
  ```
- 对变量和数组内容的原子访问（基于 CAS 操作实现）
  ```java
  //1.8 新增，给定对象 o，根据获取内存偏移量指向的字段，将其增加 delta，
  // 这是一个 CAS 操作过程，直到设置成功方能退出循环，返回旧值
  public final int getAndAddInt(Object o, long offset, int delta) {
      int v;
      do {
          // 获取内存中最新值
          v = getIntVolatile(o, offset);
        // 通过 CAS 操作
      } while (!compareAndSwapInt(o, offset, v, v + delta));
      return v;
  }

  //1.8 新增，方法作用同上，只不过这里操作的 long 类型数据
  public final long getAndAddLong(Object o, long offset, long delta) {
      long v;
      do {
          v = getLongVolatile(o, offset);
      } while (!compareAndSwapLong(o, offset, v, v + delta));
      return v;
  }

  //1.8 新增，给定对象 o，根据获取内存偏移量对于字段，将其 设置为新值 newValue，
  // 这是一个 CAS 操作过程，直到设置成功方能退出循环，返回旧值
  public final int getAndSetInt(Object o, long offset, int newValue) {
      int v;
      do {
          v = getIntVolatile(o, offset);
      } while (!compareAndSwapInt(o, offset, v, newValue));
      return v;
  }

  // 1.8 新增，同上，操作的是 long 类型
  public final long getAndSetLong(Object o, long offset, long newValue) {
      long v;
      do {
          v = getLongVolatile(o, offset);
      } while (!compareAndSwapLong(o, offset, v, newValue));
      return v;
  }

  //1.8 新增，同上，操作的是引用类型数据
  public final Object getAndSetObject(Object o, long offset, Object newValue) {
      Object v;
      do {
          v = getObjectVolatile(o, offset);
      } while (!compareAndSwapObject(o, offset, v, newValue));
      return v;
  }
  // 获取数组第一个元素的偏移地址
  public native int arrayBaseOffset(Class arrayClass);

  // 数组中一个元素占据的内存空间，arrayBaseOffset 与 arrayIndexScale 配合使用，可定位数组中每个元素在内存中的位置
  public native int arrayIndexScale(Class arrayClass);
  ```
- 自定义内存屏障
  ```java
  // 在该方法之前的所有读操作，一定在 load 屏障之前执行完成
  public native void loadFence();
  // 在该方法之前的所有写操作，一定在 store 屏障之前执行完成
  public native void storeFence();
  // 在该方法之前的所有读写操作，一定在 full 屏障之前执行完成，这个内存屏障相当于上面两个的合体功能
  public native void fullFence();
  ```
- 对序列化的支持
- 自定义内存管理 / 高效的内存布局
  ```java
  // 分配内存指定大小的内存
  public native long allocateMemory(long bytes);
  // 根据给定的内存地址 address 设置重新分配指定大小的内存
  public native long reallocateMemory(long address, long bytes);
  // 用于释放 allocateMemory 和 reallocateMemory 申请的内存
  public native void freeMemory(long address);
  // 将指定对象的给定 offset 偏移量内存块中的所有字节设置为固定值
  public native void setMemory(Object o, long offset, long bytes, byte value);
  // 设置给定内存地址的值
  public native void putAddress(long address, long x);
  // 获取指定内存地址的值
  public native long getAddress(long address);

  // 设置给定内存地址的 long 值
  public native void putLong(long address, long x);
  // 获取指定内存地址的 long 值
  public native long getLong(long address);
  // 设置或获取指定内存的 byte 值
  public native byte  getByte(long address);
  public native void  putByte(long address, byte x);
  // 其他基本数据类型 (long,char,float,double,short 等) 的操作与 putByte 及 getByte 相同

  // 操作系统的内存页大小
  public native int pageSize();
  ```
- 与原生代码和其他 JVM 进行互操作
  ```java
  // 获取本机内存的页数，这个值永远都是 2 的幂次方  
  public native int pageSize();  

  // 告诉虚拟机定义了一个没有安全检查的类，默认情况下这个类加载器和保护域来着调用者类  
  public native Class defineClass(String name, byte[] b, int off, int len, ClassLoader loader, ProtectionDomain protectionDomain);  

  // 加载一个匿名类
  public native Class defineAnonymousClass(Class hostClass, byte[] data, Object[] cpPatches);
  // 判断是否需要加载一个类
  public native boolean shouldBeInitialized(Class<?> c);
  // 确保类一定被加载 
  public native  void ensureClassInitialized(Class<?> c);
  // 传入一个对象的 class 并创建该实例对象，但不会调用构造方法
  public native Object allocateInstance(Class cls) throws InstantiationException;
  // 获取字段 f 在实例对象中的偏移量
  public native long objectFieldOffset(Field f);
  // 静态属性的偏移量，用于在对应的 Class 对象中读写静态属性
  public native long staticFieldOffset(Field f);
  // 返回值就是 f.getDeclaringClass()
  public native Object staticFieldBase(Field f);

  // 获得给定对象偏移量上的 int 值，所谓的偏移量可以简单理解为指针指向该变量的内存地址，
  // 通过偏移量便可得到该对象的变量，进行各种操作
  public native int getInt(Object o, long offset);
  // 设置给定对象上偏移量的 int 值
  public native void putInt(Object o, long offset, int x);

  // 获得给定对象偏移量上的引用类型的值
  public native Object getObject(Object o, long offset);
  // 设置给定对象偏移量上的引用类型的值
  public native void putObject(Object o, long offset, Object x);
  // 其他基本数据类型 (long,char,byte,float,double) 的操作与 getInthe 及 putInt 相同

  // 设置给定对象的 int 值，使用 volatile 语义，即设置后立马更新到内存对其他线程可见
  public native void  putIntVolatile(Object o, long offset, int x);
  // 获得给定对象的指定偏移量 offset 的 int 值，使用 volatile 语义，总能获取到最新的 int 值。
  public native int getIntVolatile(Object o, long offset);

  // 其他基本数据类型 (long,char,byte,float,double) 的操作与 putIntVolatile 及 getIntVolatile 相同，引用类型 putObjectVolatile 也一样。

  // 与 putIntVolatile 一样，但要求被操作字段必须有 volatile 修饰
  public native void putOrderedInt(Object o,long offset,int x);
  ```
- 对高级锁的支持（线程挂起与恢复）
  ```java
  // Java 对线程的挂起操作被封装在 java.util.concurrent.locks.LockSupport 类中，LockSupport 类中有各种版本 pack 方法，其底层实现最终还是使用 Unsafe.park() 方法和 Unsafe.unpark() 方法
  // 线程调用该方法，线程将一直阻塞直到超时，或者是中断条件出现。  
  public native void park(boolean isAbsolute, long time);  

  // 终止挂起的线程，恢复正常 
  public native void unpark(Object thread); 
  ```

## 3. 原子操作类

从 JDK 1.5 开始，在 java.util.concurrent.atomatic 包中提供了很多基于 CAS 实现的原子性操作，保证了操作不会被中断，且用法方便，性能高效，主要分以下 4 种类型。

### 3.1. 原子基本类型

原子基本类型主要包括 3 个类：
- [AtomicBoolean](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicBoolean.html)：原子布尔类型
- [AtomicInteger](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicInteger.html)：原子整型
- [AtomicLong](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicLong.html)：原子长整型

AtomicBoolean、AtomicInteger 与 AtomicLong 的实现原理和使用方式几乎是一样的，因此以下以 AtomicInteger 为例进行分析。

#### 3.1.1. 常用 API

构造方法：
- `AtomicInteger​()`: Creates a new AtomicInteger with initial value 0.
- `AtomicInteger​(int initialValue)`: Creates a new AtomicInteger with the given initial value.

AtomicInteger 提供了原子自增方法、原子自减方法以及原子赋值方法：
- `public final int get()`: 获取当前的值。
- `public final int getAndSet(int newValue)`: 获取当前的值，并设置新的值。
- `public final int getAndIncrement()`: 获取当前的值，并自增。
- `public final int getAndDecrement()`: 获取当前的值，并自减。
- `public final int getAndAdd(int delta)`: 获取当前的值，并加上预期的值。

例：
```java
public class AtomicIntegerDemo {
    // 创建 AtomicInteger, 用于自增操作
    static AtomicInteger i=new AtomicInteger();

    public static class AddThread implements Runnable{
        public void run(){
           for (int k=0;k<10000;k++)
               i.incrementAndGet();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Thread[] ts=new Thread[10];
        // 开启 10 条线程同时执行 i 的自增操作
        for (int k=0;k<10;k++)
            ts[k]=new Thread(new AddThread());
        
        // 启动线程
        for (int k=0;k<10;k++)
            ts[k].start();
        for (int k=0;k<10;k++)
            ts[k].join();

        System.out.println(i);// 输出结果：100000
    }
}
```

#### 3.1.2. 实现原理

查看 AtomicInteger 源码：
```java
public class AtomicInteger extends Number implements java.io.Serializable {
    private static final long serialVersionUID = 6214790243416807050L;

    // 获取指针类 Unsafe
    private static final Unsafe unsafe = Unsafe.getUnsafe();

    // 当前 AtomicInteger 封装的 int 变量 value 在 AtomicInteger 实例对象内的内存偏移量
    private static final long valueOffset;

    static {
        try {
           // 通过 unsafe 类的 objectFieldOffset() 方法，获取 value 变量在对象内存中的偏移
           // 通过该偏移量 valueOffset，unsafe 类的内部方法可以获取到变量 value 对其进行取值或赋值操作
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }

    // 当前 AtomicInteger 封装的 int 变量 value
    private volatile int value;

    public AtomicInteger(int initialValue) {
        value = initialValue;
    }
    public AtomicInteger() {
    }
    
    // 获取当前最新值，
    public final int get() {
        return value;
    }
    // 设置当前值，具备 volatile 效果，方法用 final 修饰是为了更进一步的保证线程安全。
    public final void set(int newValue) {
        value = newValue;
    }
    // 最终会设置成 newValue，使用该方法后可能导致其他线程在之后的一小段时间内可以获取到旧值，有点类似于延迟加载
    public final void lazySet(int newValue) {
        unsafe.putOrderedInt(this, valueOffset, newValue);
    }
    // 设置新值并获取旧值，底层调用的是 CAS 操作即 unsafe.compareAndSwapInt() 方法
    public final int getAndSet(int newValue) {
        return unsafe.getAndSetInt(this, valueOffset, newValue);
    }
    // 如果当前值为 expect，则设置为 update（当前值指的是 value 变量)
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
    // 当前值加 1 返回旧值，底层 CAS 操作
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
    // 当前值减 1，返回旧值，底层 CAS 操作
    public final int getAndDecrement() {
        return unsafe.getAndAddInt(this, valueOffset, -1);
    }
    // 当前值增加 delta，返回旧值，底层 CAS 操作
    public final int getAndAdd(int delta) {
        return unsafe.getAndAddInt(this, valueOffset, delta);
    }
    // 当前值加 1，返回新值，底层 CAS 操作
    public final int incrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    }
    // 当前值减 1，返回新值，底层 CAS 操作
    public final int decrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, -1) - 1;
    }
    // 当前值增加 delta，返回新值，底层 CAS 操作
    public final int addAndGet(int delta) {
        return unsafe.getAndAddInt(this, valueOffset, delta) + delta;
    }
    // 省略一些不常用的方法。...
}
```
可以发现 AtomicInteger 原子类的内部几乎是基于 Unsafe 类中的 CAS 相关操作的方法实现的。

### 3.2. 原子引用类

原子引用类指的是 [AtomicReference](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicReference.html) 原子类。

#### 3.2.1. 常用 API

例：
```java
public class AtomicReferenceDemo2 {

    public static AtomicReference<User> atomicUserRef = new AtomicReference<User>();

    public static void main(String[] args) {
        User user = new User("zejian", 18);
        atomicUserRef.set(user);
        User updateUser = new User("Shine", 25);
        atomicUserRef.compareAndSet(user, updateUser);
        // 执行结果：User{name='Shine', age=25}
      	System.out.println(atomicUserRef.get().toString());  
    }

    static class User {
        public String name;
        private int age;

        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
}
```

#### 3.2.2. 实现原理

查看源码：
```java
public class AtomicReference<V> implements java.io.Serializable {
    AtomicReference<V> implements java private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;

    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicReference.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
    // 内部变量 value，Unsafe 类通过 valueOffset 内存偏移量即可获取该变量
    private volatile V value;

    // CAS 方法，间接调用 unsafe.compareAndSwapObject(), 它是一个实现了 CAS 操作的 native 方法
    public final boolean compareAndSet(V expect, V update) {
        return unsafe.compareAndSwapObject(this, valueOffset, expect, update);
    }

    // 设置并获取旧值
    public final V getAndSet(V newValue) {
        return (V)unsafe.getAndSetObject(this, valueOffset, newValue);
    }
    // 省略其他代码。.....
}

//Unsafe 类中的 getAndSetObject 方法，实际调用还是 CAS 操作
public final Object getAndSetObject(Object o, long offset, Object newValue) {
    Object v;
    do {
        v = getObjectVolatile(o, offset);
    } while (!compareAndSwapObject(o, offset, v, newValue));
    return v;
}
```
从源码看来，AtomicReference 与 AtomicInteger 的实现原理基本是一样的，最终执行的还是 Unsafe 类。

### 3.3. 原子数组类型

原子数组类型允许通过原子的方式更新数组里的某个元素，主要有以下 3 个类：
- [AtomicIntegerArray](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicIntegerArray.html)：原子更新整数数组里的元素
- [AtomicLongArray](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicLongArray.html)：原子更新长整数数组里的元素
- [AtomicReferenceArray](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicReferenceArray.html)：原子更新引用类型数组里的元素

这 3 个类的使用方式和实现原理基本一样，因此以下以 AtomicIntegerArray 为例进行分析。

#### 3.3.1. 常用 API

例：
```java
public class AtomicIntegerArrayDemo {
    static AtomicIntegerArray arr = new AtomicIntegerArray(10);

    public static class AddThread implements Runnable{
        public void run() {
           for (int k=0;k<10000;k++)
               // 执行数组中元素自增操作，参数为 index, 即数组下标
               arr.getAndIncrement(k%arr.length());
        }
    }
    public static void main(String[] args) throws InterruptedException {

        Thread[] ts=new Thread[10];
        // 创建 10 条线程
        for(int k=0;k<10;k++)
            ts[k]=new Thread(new AddThread());
        
        // 启动 10 条线程
        for(int k=0;k<10;k++)
            ts[k].start();
        for(int k=0;k<10;k++)
            ts[k].join();
        // 执行结果
        // [10000, 10000, 10000, 10000, 10000, 10000, 10000, 10000, 10000, 10000]
        System.out.println(arr);
    }
}
```

#### 3.3.2. 实现原理

查看 AtomicIntegerArray 源码：
```java
public class AtomicIntegerArray implements java.io.Serializable {
    // 获取 unsafe 类的实例对象
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    // 获取数组的第一个元素内存起始地址
    private static final int base = unsafe.arrayBaseOffset(int[].class);

    private static final int shift;
    // 内部数组
    private final int[] array;

    static {
        // 获取数组中一个元素占据的内存空间
        int scale = unsafe.arrayIndexScale(int[].class);
        // 判断是否为 2 的次幂，一般为 2 的次幂否则抛异常
        if ((scale & (scale - 1)) != 0)
            throw new Error("data type scale not a power of two");
        shift = 31 - Integer.numberOfLeadingZeros(scale);
    }

    private long checkedByteOffset(int i) {
        if (i < 0 || i >= array.length)
            throw new IndexOutOfBoundsException("index " + i);

        return byteOffset(i);
    }

    // 计算数组中每个元素的的内存地址
    private static long byteOffset(int i) {
        return ((long) i << shift) + base;
    }

    // 执行自增操作，返回旧值，i 是指数组元素下标
    public final int getAndIncrement(int i) {
          return getAndAdd(i, 1);
    }
    // 指定下标元素执行自增操作，并返回新值
    public final int incrementAndGet(int i) {
        return getAndAdd(i, 1) + 1;
    }

    // 指定下标元素执行自减操作，并返回新值
    public final int decrementAndGet(int i) {
        return getAndAdd(i, -1) - 1;
    }
    // 间接调用 unsafe.getAndAddInt() 方法
    public final int getAndAdd(int i, int delta) {
        return unsafe.getAndAddInt(array, checkedByteOffset(i), delta);
    }
    
    // 省略其他代码。.....    
}
```

其中
- `byteOffset(int i)` 方法的计算原理：

  arrayBaseOffset 方法可以获取数组的第一个元素起始地址，而 arrayIndexScale 方法可以获取每个数组元素占用的内存空间，由于这里是 Int 类型，而 Java 中一个 int 类型占用 4 个字节，也就是 scale 的值为 4，那么如何根据数组下标值计算每个元素的内存地址呢？
  > 每个数组元素的内存地址 = 起始地址 + 元素下标 * 每个元素所占用的内存空间
  与该原理相同，首先来计算出 shift 的值：
  > shift = 31 - Integer.numberOfLeadingZeros(scale);
  其中 Integer.numberOfLeadingZeros(scale) 是计算出 scale 的前导零个数（必须是连续的)，scale=4，转成二进制为 `00000000 00000000 00000000 00000100`。

  即前导零数为 29，也就是 shift=2，然后利用 shift 来定位数组中的内存位置，在数组不越界时，计算出前 3 个数组元素内存地址
  ```java
  // 第一个数组元素，index=0 ， 其中 base 为起始地址，4 代表 int 类型占用的字节数 
  address = base + 0 * 4 即 address= base + 0 << 2
  // 第二个数组元素，index=1
  address = base + 1 * 4 即 address= base + 1 << 2
  // 第三个数组元素，index=2
  address = base + 2 * 4 即 address= base + 2 << 2
  //........
  ```
  显然 shift=2，替换去就是 `address= base + i << shift`。

- 至于其他方法就比较简单了，都是间接调用 Unsafe 类的 CAS 原子操作方法。

### 3.4. 原子属性更新类型

如果我们只需要某个类里的某个字段，也就是说让普通的变量使用原子操作，就可以使用原子属性更新类型。Atomic 并发包提供了以下三个原子属性更新类型：
- [AtomicIntegerFieldUpdater](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicIntegerFieldUpdater.html)：原子更新整型的字段的更新器。
- [AtomicLongFieldUpdater](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicLongFieldUpdater.html)：原子更新长整型字段的更新器。
- [AtomicReferenceFieldUpdater](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/atomic/AtomicReferenceFieldUpdater.html)：原子更新引用类型里的字段。

#### 3.4.1. 常用 API

原子属性更新类型的使用存在以下要求：
- 操作的字段不能是 static 类型。
- 操作的字段不能是 final 类型的，因为 final 无法修改。
- 操作的字段必须是 volatile 修饰的，也就是数据本身是读一致的。
- 操作的字段必须对当前的 Updater 所在的区域是可见的。即：如果不是当前类内部进行原子更新器操作不能使用 private，protected 子类操作父类时修饰符必须是 protect 权限及以上，如果在同一个 package 下则必须是 default 权限及以上，也就是说无论何时都应该保证操作类与被操作类间的可见性。

例：使用 AtomicIntegerFieldUpdater 更新候选人 (Candidate) 的分数 score，开启了 10000 条线程投票，当随机值大于 0.4 时算一票，分数自增一次。其中 allScore 用于验证分数是否正确（其实用于验证 AtomicIntegerFieldUpdater 更新的字段是否线程安全)。当 allScore 与 score 相同时，则说明投票结果无误，也代表 AtomicIntegerFieldUpdater 能正确更新字段 score 的值，是线程安全的。
```java
public class AtomicIntegerFieldUpdaterDemo {
    public static class Candidate{
        int id;
        volatile int score;
    }

    public static class Game{
        int id;
        volatile String name;

        public Game(int id, String name) {
            this.id = id;
            this.name = name;
        }

        // toString...
    }

    static AtomicIntegerFieldUpdater<Candidate> atIntegerUpdater
        = AtomicIntegerFieldUpdater.newUpdater(Candidate.class, "score");

    static AtomicReferenceFieldUpdater<Game, String> atRefUpdate =
            AtomicReferenceFieldUpdater.newUpdater(Game.class,String.class,"name");

    // 用于验证分数是否正确
    public static AtomicInteger allScore=new AtomicInteger(0);

    public static void main(String[] args) throws InterruptedException {
        final Candidate stu=new Candidate();
        Thread[] t=new Thread[10000];
        // 开启 10000 个线程
        for(int i = 0 ; i < 10000 ; i++) {
            t[i]=new Thread() {
                public void run() {
                    if(Math.random()>0.4) {
                        atIntegerUpdater.incrementAndGet(stu);
                        allScore.incrementAndGet();
                    }
                }
            };
            t[i].start();
        }

        for(int i = 0 ; i < 10000 ; i++) {  t[i].join();}
        System.out.println("最终分数 score="+stu.score);
        System.out.println("校验分数 allScore="+allScore);

        //AtomicReferenceFieldUpdater 简单的使用
        Game game = new Game(2,"zh");
        atRefUpdate.compareAndSet(game,game.name,"JAVA-HHH");
        System.out.println(game.toString());
    }
}
```
输出结果：
```
最终分数 score=5976
校验分数 allScore=5976
Game{id=2, name='JAVA-HHH'}
```

#### 3.4.2. 实现原理

AtomicIntegerFieldUpdater 的实现原理，实际就是反射和 Unsafe 类结合，AtomicIntegerFieldUpdater 是个抽象类，实际实现类为 AtomicIntegerFieldUpdater 的内部类 AtomicIntegerFieldUpdaterImpl:
```java
public abstract class AtomicIntegerFieldUpdater<T> {
    @CallerSensitive
    public static <U> AtomicIntegerFieldUpdater<U> newUpdater(Class<U> tclass,
                                                              String fieldName) {
         // 实际实现类 AtomicIntegerFieldUpdaterImpl                                          
        return new AtomicIntegerFieldUpdaterImpl<U>
            (tclass, fieldName, Reflection.getCallerClass());
    }

    // ...

    /**
     * Standard hotspot implementation using intrinsics.
     */
    private static class AtomicIntegerFieldUpdaterImpl<T>
            extends AtomicIntegerFieldUpdater<T> {
        private static final Unsafe unsafe = Unsafe.getUnsafe();
        private final long offset;// 内存偏移量
        private final Class<T> tclass;
        private final Class<?> cclass;

        AtomicIntegerFieldUpdaterImpl(final Class<T> tclass,
                                      final String fieldName,
                                      final Class<?> caller) {
            final Field field;// 要修改的字段
            final int modifiers;// 字段修饰符
            try {
                field = AccessController.doPrivileged(
                    new PrivilegedExceptionAction<Field>() {
                        public Field run() throws NoSuchFieldException {
                            return tclass.getDeclaredField(fieldName);// 反射获取字段对象
                        }
                    }
                );
                // 获取字段修饰符
                modifiers = field.getModifiers();
                // 对字段的访问权限进行检查，不在访问范围内抛异常
                sun.reflect.misc.ReflectUtil.ensureMemberAccess(
                    caller, tclass, null, modifiers);
                ClassLoader cl = tclass.getClassLoader();
                ClassLoader ccl = caller.getClassLoader();
                if ((ccl != null) && (ccl != cl) &&
                    ((cl == null) || !isAncestor(cl, ccl))) {
                    sun.reflect.misc.ReflectUtil.checkPackageAccess(tclass);
                }
            } catch (PrivilegedActionException pae) {
                throw new RuntimeException(pae.getException());
            } catch (Exception ex) {
                throw new RuntimeException(ex);
            }

            Class<?> fieldt = field.getType();
            // 判断是否为 int 类型
            if (fieldt != int.class)
                throw new IllegalArgumentException("Must be integer type");
            // 判断是否被 volatile 修饰
            if (!Modifier.isVolatile(modifiers))
                throw new IllegalArgumentException("Must be volatile type");

            this.cclass = (Modifier.isProtected(modifiers) &&
                           caller != tclass) ? caller : null;
            this.tclass = tclass;
            // 获取该字段的在对象内存的偏移量，通过内存偏移量可以获取或者修改该字段的值
            offset = unsafe.objectFieldOffset(field);
        }
    }
}
```
同样的，其 CAS 操作也是通过 Unsafe 类完成的。

## 4. ABA 问题

CAS 操作虽然简洁有效，但显然这种操作无法涵盖同步的所有使用场景，从语义上来说，CAS 存在一个逻辑漏洞：

> 假设这样一种场景，当第一个线程执行 CAS(V,E,U) 操作，在获取到当前变量 V，准备修改为新值 U 前，另外两个线程已连续修改了两次变量 V 的值，使得该值从旧值 A 变为新值 B 又恢复为旧值 A，这样的话，我们就无法正确判断这个变量是否已被修改过。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/2/643ef64a67cc1cc48c939f1b46f5abce.jpg)

这就是典型的 CAS 的 **ABA 问题**。

大部分情况下这种情况发生的概率比较小，且就算发生了也不会影响并发程序的正确性（比如说我们对某个做加减法，不关心数字的过程，那么发生 ABA 问题也没啥关系）。但是**在某些情况下还是需要防止 ABA 问题的发生，J.U.C 包为解决这个问题，提供了 AtomicStampedReference 和 AtomicMarkableReference 类**。

但实际上，若需要解决 ABA 问题，改为采用传统的阻塞同步互斥方案，会比原子类更加高效。

### 4.1. AtomicStampedReference 类

AtomicStampedReference 类是一个带有时间戳标记的原子引用类，可通过控制变量值的版本来保证 CAS 的正确性。

在每次修改后，AtomicStampedReference 不仅会设置新值而且还会记录更改的时间。当 AtomicStampedReference 设置对象值时，对象值以及时间戳都必须满足期望值才能写入成功，这也就解决了反复读写时，无法预知值是否已被修改的窘境。

例：
```java
public class ABADemo {
    static AtomicInteger atIn = new AtomicInteger(100);

    // 初始化时需要传入一个初始值和初始时间
    static AtomicStampedReference<Integer> atomicStampedR =
            new AtomicStampedReference<Integer>(200,0);

    static Thread t1 = new Thread(new Runnable() {
        @Override
        public void run() {
            // 更新为 200
            atIn.compareAndSet(100, 200);
            // 更新为 100
            atIn.compareAndSet(200, 100);
        }
    });

    static Thread t2 = new Thread(new Runnable() {
        @Override
        public void run() {
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            boolean flag=atIn.compareAndSet(100,500);
            System.out.println("flag:"+flag+",newValue:"+atIn);
        }
    });

    static Thread t3 = new Thread(new Runnable() {
        @Override
        public void run() {
            int time=atomicStampedR.getStamp();
            // 更新为 200
            atomicStampedR.compareAndSet(100, 200,time,time+1);
            // 更新为 100
            int time2=atomicStampedR.getStamp();
            atomicStampedR.compareAndSet(200, 100,time2,time2+1);
        }
    });

    static Thread t4 = new Thread(new Runnable() {
        @Override
        public void run() {
            int time = atomicStampedR.getStamp();
            System.out.println("sleep 前 t4 time:"+time);
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            boolean flag=atomicStampedR.compareAndSet(100,500,time,time+1);
            System.out.println("flag:"+flag+",newValue:"+atomicStampedR.getReference());
        }
    });

    public static  void  main(String[] args) throws InterruptedException {
        t1.start();
        t2.start();
        t1.join();
        t2.join();

        t3.start();
        t4.start();
        /**
         * 输出结果：
         flag:true,newValue:500
         sleep 前 t4 time:0
         flag:false,newValue:200
         */
    }
}
```

### 4.2. AtomicMarkableReference 类

AtomicMarkableReference 与 AtomicStampedReference 不同的是，AtomicMarkableReference 维护的是一个 boolean 值的标识，也就是说至于 true 和 false 两种切换状态，但这种方式并不能完全防止 ABA 问题的发生，只能减少 ABA 问题发生的概率。

例：
```java
public class ABADemo {
    static AtomicMarkableReference<Integer> atMarkRef =
              new AtomicMarkableReference<Integer>(100,false);

 static Thread t5 = new Thread(new Runnable() {
        @Override
        public void run() {
            boolean mark=atMarkRef.isMarked();
            System.out.println("mark:"+mark);
            // 更新为 200
            System.out.println("t5 result:"+atMarkRef.compareAndSet(atMarkRef.getReference(), 200,mark,!mark));
        }
    });

    static Thread t6 = new Thread(new Runnable() {
        @Override
        public void run() {
            boolean mark2=atMarkRef.isMarked();
            System.out.println("mark2:"+mark2);
            System.out.println("t6 result:"+atMarkRef.compareAndSet(atMarkRef.getReference(), 100,mark2,!mark2));
        }
    });

    static Thread t7 = new Thread(new Runnable() {
        @Override
        public void run() {
            boolean mark=atMarkRef.isMarked();
            System.out.println("sleep 前 t7 mark:"+mark);
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            boolean flag=atMarkRef.compareAndSet(100,500,mark,!mark);
            System.out.println("flag:"+flag+",newValue:"+atMarkRef.getReference());
        }
    });

    public static  void  main(String[] args) throws InterruptedException {        
        t5.start();t5.join();
        t6.start();t6.join();
        t7.start();

        /**
         * 输出结果：
         mark:false
         t5 result:true
         mark2:true
         t6 result:true
         sleep 前 t5 mark:false
         flag:true,newValue:500 ---->成功了。.... 说明还是发生 ABA 问题
         */
    }
}
```

## 5. Refer Links

[Java 并发编程 - 无锁 CAS 与 Unsafe 类及其并发包 Atomic](https://blog.csdn.net/javazejian/article/details/72772470)