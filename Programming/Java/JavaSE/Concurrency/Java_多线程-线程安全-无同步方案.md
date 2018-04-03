- [Java 多线程 - 线程安全 - 无同步方案](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---%E6%97%A0%E5%90%8C%E6%AD%A5%E6%96%B9%E6%A1%88)
  - [1. 可重入代码 (Reentrant Code)](#1-%E5%8F%AF%E9%87%8D%E5%85%A5%E4%BB%A3%E7%A0%81-reentrant-code)
  - [2. 线程局部变量 (ThreadLocal)](#2-%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F-threadlocal)
    - [2.1. 适用场景](#21-%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF)
    - [2.2. 常用 API](#22-%E5%B8%B8%E7%94%A8-api)
    - [2.3. 实现原理](#23-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
      - [2.3.1. threadLocalHashCode](#231-threadlocalhashcode)
      - [2.3.2. ThreadLocalMap 内部类](#232-threadlocalmap-%E5%86%85%E9%83%A8%E7%B1%BB)
      - [2.3.3. 设置实例](#233-%E8%AE%BE%E7%BD%AE%E5%AE%9E%E4%BE%8B)
      - [2.3.4. 读取实例](#234-%E8%AF%BB%E5%8F%96%E5%AE%9E%E4%BE%8B)
  - [3. Refer Linked](#3-refer-linked)

# Java 多线程 - 线程安全 - 无同步方案

要保证线程安全，并不是一定要进行同步，两者没有因果关系。同步只是保证是共享数据争用时的正确性的手段，但若一个方法本来就不涉及共享数据，那么自然就无须进行使用同步措施去保证并发正确性。

## 1. 可重入代码 (Reentrant Code)

可重入代码 (Reentrant Code) 也称为纯代码 (Pure Code)，可以在代码执行的任何时刻被中断，转而执行另一段代码（包括递归调用他本身）。而在控制权返回后，原来的程序不会出现任何错误。

可重入性是更基本的特性，所有的可重入代码都是线程安全的，但并非所有线程安全的代码都是可重入的。

可重入代码都有一些共同的特征，例如不依赖存储在堆上的数据和公用的系统资源，用到的状态量都由参数中传入、不调用非可重入的方法等。

可以通过以下原则来判断代码是否具有可重入性：如果一个方法的返回结果是可预测的，只要输入了相同的数据，就都能返回相同的结果，那么这个方法就是可重入的，自然也就是线程安全的。

## 2. 线程局部变量 (ThreadLocal)

当多个线程同时访问共享变量时，会出现并发安全问题。因此，我们可以避免使用共享变量，转而把共享数据的可见范围限制在同一个线程内，自然也就不会出现数据争用的问题了。

Java 提供了 java.lang.[ThreadLocal](https://docs.oracle.com/javase/9/docs/api/java/lang/ThreadLocal.html) 类，为每个线程提供各自的变量实例，也就是每个线程都保存有一个变量副本，在不同的 Thread 中有不同的副本，即线程局部变量。

NOTE:
- ThreadLocal 并不解决线程间共享数据的问题，它相当于提供了一种线程隔离，将变量与线程相绑定，每个使用该变量的线程都会初始化一个完全独立的实例副本，因此完全不涉及数据共享。

- ThreadLocal 通常定义为 private static 类型。

- ThreadLocalMap 的 Entry 对 ThreadLocal 的引用为弱引用，避免了 ThreadLocal 对象无法被回收的问题。当一个线程结束时，它所使用的所有 ThreadLocal 相应的实例副本都可被回收。

### 2.1. 适用场景

ThreadLocal 适用于如下两种场景：
- 每个线程需要有自己单独的实例。
- 实例需要在多个方法中共享，但不希望被多线程共享。

当然，在以上场景下，并非必须使用 ThreadLocal，其它方式完全可以实现同样的效果，只是 ThreadLocal 使得实现更简洁。

### 2.2. 常用 API

- `void	set​(T value)`：为当前的线程的局部变量设置一个新值。
- `T	get​()`：获取当前线程的变量局部实例。首次调用 get 时，会调用 initialize 来得到初始值。
- `protected T	initialValue​()`：应覆盖该方法来提供一个初始值。默认返回 null。
- `void	remove​()`：删除对应这个线程的值。
- `static <S> ThreadLocal<S>	withInitial​(Supplier<? extends S> supplier)`：创建一个线程局部变量，初始值通过调用给定的 supplier 生成。
  ```java
  ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd"));
  String date = dateFormat.get().format(new Date());
  ```

例：DAO 的数据库连接，由于 DAO 是单例的，那么其属性 Connection 就不是一个线程安全的变量。而我们每个线程都需要使用他，并且各自使用各自的。这种情况，ThreadLocal 就比较好的解决了这个问题。
```java
public final class ConnectionUtil {

    private ConnectionUtil() {}

    private static final ThreadLocal<Connection> conn = new ThreadLocal<>();

    public static Connection getConn() {
        Connection con = conn.get();
        if (con == null) {
            try {
                Class.forName("com.mysql.jdbc.Driver");
                con = DriverManager.getConnection("url", "userName", "password");
                conn.set(con);
            } catch (ClassNotFoundException | SQLException e) {
                // ...
            }
        }
        return con;
    }

}
```

### 2.3. 实现原理

每一个线程的 Thread 对象中都有一个 ThreadLocalMap 对象，这个对象存储了一组以 ThreadLocal.threadLocalHashCode 为键，以本地线程变量为值的 KV 对。ThreadLocal 对象就是当前线程的 ThreadLocalMap 访问入口，每一个 ThreadLocal 对象都包含了一个独一无二的 threadLocalHashCode 值，使用这个值就可以在线程 KV 对中找回对应的本地线程变量。

对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而 ThreadLocal 采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问；而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。

#### 2.3.1. threadLocalHashCode

ThreadLocal 通过 threadLocalHashCode 来标识每一个 ThreadLocal 的唯一性。threadLocalHashCode 通过 CAS 操作进行更新，每次 hash 操作的增量为 0x61c88647：
```java
private final int threadLocalHashCode = nextHashCode();
/**
 * The next hash code to be given out. Updated atomically. Starts at
 * zero.
 */
private static AtomicInteger nextHashCode =
    new AtomicInteger();
/**
 * The difference between successively generated hash codes - turns
 * implicit sequential thread-local IDs into near-optimally spread
 * multiplicative hash values for power-of-two-sized tables.
 */
private static final int HASH_INCREMENT = 0x61c88647;
/**
 * Returns the next hash code.
 */
private static int nextHashCode() {
    return nextHashCode.getAndAdd(HASH_INCREMENT);
}
```

#### 2.3.2. ThreadLocalMap 内部类

ThreadLocalMap 是 ThreadLocal 的静态内部类，它有一个常量和三个成员变量：
```java
/**
 * The initial capacity -- MUST be a power of two.
 */
private static final int INITIAL_CAPACITY = 16;
/**
 * The table, resized as necessary.
 * table.length MUST always be a power of two.
 */
private Entry[] table;
/**
 * The number of entries in the table.
 */
private int size = 0;
/**
 * The next size value at which to resize.
 */
private int threshold; // Default to 0
```

Entry 类是 ThreadLocalMap 的静态内部类，用于存储数据。Entry 类继承了 WeakReference<ThreadLocal<?>>，即每个 Entry 对象都有一个 ThreadLocal 的弱引用（作为 key），这是为了防止内存泄露。一旦线程结束，key 变为一个不可达的对象，这个 Entry 就可以被 GC 了。
```java
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;
    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```

ThreadLocalMap 类有两个构造函数，其中常用的是 ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue)：
```java
ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
    table = new Entry[INITIAL_CAPACITY];
    // 相当于取模运算 hashCode % size 的一个更高效的实现（和 HashMap 中的思路相同）
    // 正是因为这种算法，我们要求 size 必须是 2 的指数，因为这可以使得 hash 发生冲突的次数减小
    int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
    table[i] = new Entry(firstKey, firstValue);
    size = 1;
    setThreshold(INITIAL_CAPACITY);
}
```

ThreadLocalMap 解决冲突的方法是线性探测法（不断加 1），而不是 HashMap 的链地址法，这一点也能从 Entry 的结构上推断出来。

对于已经不再被使用且已被回收的 ThreadLocal 对象，它在每个线程内对应的实例由于被线程的 ThreadLocalMap 的 Entry 强引用，无法被回收，可能会造成内存泄漏。针对该问题，ThreadLocalMap 的 set 方法中，通过 replaceStaleEntry 方法将所有键为 null 的 Entry 的值设置为 null，从而使得该值可被回收。另外，会在 rehash 方法中通过 expungeStaleEntry 方法将键和值为 null 的 Entry 设置为 null 从而使得该 Entry 可被回收。

因此，当满足以下条件之一时，无用的 Entry 会被 GC：
- Thread 结束的时候
- 插入元素时，发现 staled entry，则会进行替换并清理
- 插入元素时，ThreadLocalMap 的 size 达到 threshold，并且没有任何 staled entries 的时候，会调用 rehash 方法清理并扩容
- 调用 ThreadLocalMap 的 remove 方法或 set(null) 时
尽管不会造成内存泄露，但是可以看到无用的 Entry 只会在以上四种情况下才会被清理，这就可能导致一些 Entry 虽然无用但还占内存的情况。因此，我们在使用完 ThreadLocal 后一定要 remove 一下，保证及时回收掉无用的 Entry。

#### 2.3.3. 设置实例

每个 Thread 里面都有一个 ThreadLocal.ThreadLocalMap 成员变量，也就是说每个线程通过 ThreadLocal.ThreadLocalMap 与 ThreadLocal 相绑定，这样可以确保每个线程访问到的 thread-local variable 都是本线程的。

```java
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
}

// 直接返回 Thread 实例的成员变量 threadLocals，它的定义在 Thread 内部，访问级别为 package 级别
ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}

void createMap(Thread t, T firstValue) {
    t.threadLocals = new ThreadLocalMap(this, firstValue);
}
```

该方法先获取当前线程的 ThreadLocalMap 对象，然后直接将 ThreadLocal 对象（即代码中的 this）与目标实例的映射添加进 ThreadLocalMap 中。

当然，如果映射已经存在，就直接覆盖。另外，如果获取到的 ThreadLocalMap 为 null，则先创建该 ThreadLocalMap 对象。

#### 2.3.4. 读取实例

```java
public T get() {
  Thread t = Thread.currentThread();
  ThreadLocalMap map = getMap(t);
  if (map != null) {
    ThreadLocalMap.Entry e = map.getEntry(this);
    if (e != null) {
      @SuppressWarnings("unchecked")
      T result = (T)e.value;
      return result;
    }
  }
  return setInitialValue();
}

ThreadLocalMap getMap(Thread t) {
  return t.threadLocals;
}

private T setInitialValue() {
  T value = initialValue();
  Thread t = Thread.currentThread();
  ThreadLocalMap map = getMap(t);
  if (map != null)
    map.set(this, value);
  else
    createMap(t, value);
  return value;
}
```
读取实例时，线程首先通过 getMap(t) 方法获取自身的 ThreadLocalMap。从如下该方法的定义可见，该 ThreadLocalMap 的实例是 Thread 类的一个字段，即由 Thread 维护 ThreadLocal 对象与具体实例的映射。

获取到 ThreadLocalMap 后，通过 map.getEntry(this) 方法获取该 ThreadLocal 在当前线程的 ThreadLocalMap 中对应的 Entry。该方法中的 this 即当前访问的 ThreadLocal 对象。

如果获取到的 Entry 不为 null，从 Entry 中取出值即为所需访问的本线程对应的实例。如果获取到的 Entry 为 null，则通过 setInitialValue() 方法设置该 ThreadLocal 变量在该线程中对应的具体实例的初始值。

## 3. Refer Linked

[并发编程 | ThreadLocal 源码深入分析](http://www.sczyh30.com/posts/Java/java-concurrent-threadlocal/)

[Java 进阶（七）正确理解 Thread Local 的原理与适用场景](http://www.jasongj.com/java/threadlocal/)