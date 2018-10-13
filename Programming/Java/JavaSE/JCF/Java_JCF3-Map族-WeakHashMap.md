- [Java 集合：Map 族 - WeakHashMap 实现类](#java-集合map-族---weakhashmap-实现类)
	- [1. 基本概念](#1-基本概念)
		- [1.1. 强引用、软引用、弱引用](#11-强引用软引用弱引用)
		- [1.2. weakhashmap 的弱引用](#12-weakhashmap-的弱引用)
	- [2. 常用 API](#2-常用-api)
	- [3. 应用](#3-应用)
		- [3.1. 利用 WeakHashMap 存储大对象](#31-利用-weakhashmap-存储大对象)
	- [4. 源码分析](#4-源码分析)
		- [4.1. 类定义](#41-类定义)
		- [4.2. 构造方法](#42-构造方法)
	- [5. Refer Links](#5-refer-links)
    
# Java 集合：Map 族 - WeakHashMap 实现类

对于 HashMap，存在这样的问题：由于 GC 跟踪活动的对象，只要映射对象是活动的，其中所有的桶也是活动的，因此**即使一条映射的键值已不再使用了，GC 也不会回收，这导致了开发者需要负责从长期存活的映射表中删除无用的值**。 

为解决这个问题，Java 集合框架提供了 [WeakHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/WeakHashMap.html)。**WeakHashMap 是基于 Java 弱引用实现的 HashMap，当对键的唯一引用来自散列条目时，WeakHashMap 将与 GC 协同工作一起删除键值对**。

## 1. 基本概念

### 1.1. 强引用、软引用、弱引用

- 强引用

	被强引用指向的对象，绝对不会被垃圾收集器回收。
	
	`Integer prime = 1;`，这个语句中 prime 对象就有一个强引用。

- 软引用

	被 SoftReference 指向的对象可能会被垃圾收集器回收，但是只有在 JVM 内存不够的情况下才会回收。

	如下代码可以创建一个软引用：
	```java
	Integer prime = 1;  
	SoftReference<Integer> soft = new SoftReference<Integer>(prime);
	prime = null; // 移除强引用
	```
	当把 prime 赋值为 null 的时候，已经没有强引用指向它而只有 SoftReference 引用它，因此原 prime 对象会在 JVM 内存不够的情况下被回收。

- 弱引用

	当一个对象仅仅被 WeakReference 引用时，在下个垃圾收集周期时候该对象就会被回收。

	通过下面代码创建一个 WeakReference：
	```java
	Integer prime = 1;  
	WeakReference<Integer> soft = new WeakReference<Integer>(prime);
	prime = null; // 移除强引用
	```
	当把 prime 赋值为 null 的时候，已经没有强引用指向它而只有 WeakReference 引用它，因此原 prime 对象会在下一个垃圾收集周期中被回收。
	
### 1.2. weakhashmap 的弱引用

WeakHashMap 实现使用弱引用保存键，这个“弱键”主要是通过 WeakReference 和 ReferenceQueue 实现的。在 WeakHashMap 中，key 的类型是 WeakReference，**如果除了自身有对 Key 的引用外，如果此 Key 没有其他引用，那么 WeakHashMap 会自动丢弃该值，不会造成内存空间的浪费**。

WeakReference 对象将引用保存到另外一个对象中，即散列键。若某个对象只能由 WeakReference 引用，GC 就会回收它，但要先将引用该对象的 WeakReference 放入队列 ReferenceQueue 中。WeakHashMap 会周期性检查 ReferenceQueue 队列，找出新添加的弱引用并删除其对应的条目。

例：
```java
public class Test {
    public static void main(String[] args) {
        String a = new String("A");
        String b = new String("B");

        // 向 HashMap 中放入 A、B 对象
        HashMap<String, String> hashMap = new HashMap<String, String>();
        hashMap.put(a, "AAA");
        hashMap.put(b, "BBB");

        // 向 WeakHashMap 中放入 A、B 对象
        WeakHashMap<String, String> weakHashMap = new WeakHashMap<String, String>();
        weakHashMap.put(a, "AAA");
        weakHashMap.put(b, "BBB");

				// 当从 HashMap 中删除 A，并且将 a 指向 Null 时，WeakHashMap 中的 A 将自动被回收
				// 而对于 b 对象虽然也指向了 null，但 HashMap 中还有指向 B 的指针，所以 WeakHashMap 将会保留 B 对象
        hashMap.remove(a);
        a = null;
        b = null;
        System.gc();

        Iterator<Entry<String, String>> ite = hashMap.entrySet().iterator();
        System.out.println("***输出 HashMap***");

        while (ite.hasNext()) {
            Entry<String, String> entry = ite.next();
            System.out.println(entry.getKey() + " --> " + entry.getValue());
        }

        Iterator<Entry<String, String>> ite2 = weakHashMap.entrySet().iterator();
        System.out.println("***输出 WeakHashMap***");

        while (ite2.hasNext()) {
            Entry<String, String> entry = ite2.next();
            System.out.println(entry.getKey() + " --> " + entry.getValue());
        }
    }
}
```
输出结果：
```
***输出 HashMap***
B --> BBB
***输出 WeakHashMap***
B --> BBB
```

NOTE：
- **由于 WeakHashMap 的行为一定程度上基于垃圾收集器的行为，因此 WeakHashMap 跟普通的 HashMap 不同，Map 数据结构对应的常识在 WeakHashMap 上会失效，如 `size()` 方法的返回值会随着程序的运行变小，`isEmpty()` 方法的返回值会从 false 变成 true 等**。

- **WeakHashMap 并不是你什么也不干它就能自动释放内部不用的对象的，而是在你访问它内容的时候才调用内部 expungeStaleEntries() 方法，来释放内部不用的对象。因此，如果初始化好 WeakHashMap 中相关数据后，一直不调用 put、get、remove、size 等相关方法，是不能够正常回收的，依然会导致内存泄漏**。

## 2. 常用 API

WeakHashMap 的 API 与 HashMap 的 API 基本相同：

- `V get​(Object key)`: Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key.
- `V put​(K key, V value)`: Associates the specified value with the specified key in this map.
- `V remove​(Object key)`: Removes the mapping for a key from this weak hash map if it is present.

## 3. 应用

### 3.1. 利用 WeakHashMap 存储大对象

假设一种应用场景：我们需要保存一批大的图片对象，其中 values 是图片的内容，key 是图片的名字，这里我们需要选择一种合适的容器保存这些对象。

使用普通的 HashMap 并不是好的选择，这些大对象将会占用很多内存，并且还不会被 GC 回收，除非我们在对应的 key 废弃之前主动 remove 掉这些元素。WeakHashMap 非常适合使用在这种场景下，下面的代码演示了具体的实现：
```java
// 创建一个 WeakHashMap 对象来存储 BigImage 实例，对应的 key 是 UniqueImageName 对象
WeakHashMap<UniqueImageName, BigImage> map = new WeakHashMap<>();
BigImage bigImage = new BigImage("image_id");
UniqueImageName imageName = new UniqueImageName("name_of_big_image"); // 强引用

map.put(imageName, bigImage);
assertTrue(map.containsKey(imageName));

// map 中的 values 对象成为弱引用对象，在下一次 GC 周期中会回收 bigImage 对象
imageName = null; 
// 主动触发一次 GC
System.gc(); 

// 可以发现 WeakHashMap 成为空的
await().atMost(10, TimeUnit.SECONDS).until(map::isEmpty);
```

利用 WeakHashMap 的这种特性，可以实现类似本地、堆内缓存的存储机制，即缓存的失效依赖于 GC 收集器的行为。

## 4. 源码分析

TODO:

### 4.1. 类定义

```java
public class WeakHashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>
```

在类定义方面与 HashMap 并没有什么不同，而且在结构上也基本和 HashMap 一致。

**在 Java 8 中，与 HashMap 唯一存储上的不同点就是，当冲突的 key 变多时，HashMap 引入了二叉树（红黑树）进行存储，而 WeakHashMap 则一直使用链表进行存储**。

### 4.2. 构造方法

```java
public WeakHashMap(int initialCapacity, float loadFactor) {
		if (initialCapacity < 0)
				throw new IllegalArgumentException("Illegal Initial Capacity: "+
																						initialCapacity);
		if (initialCapacity > MAXIMUM_CAPACITY)
				initialCapacity = MAXIMUM_CAPACITY;

		if (loadFactor <= 0 || Float.isNaN(loadFactor))
				throw new IllegalArgumentException("Illegal Load factor: "+
																						loadFactor);
		int capacity = 1;
		while (capacity < initialCapacity)
				capacity <<= 1;
		table = newTable(capacity);
		this.loadFactor = loadFactor;
		threshold = (int)(capacity * loadFactor);
}

public WeakHashMap(int initialCapacity) {
		this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

public WeakHashMap() {
		this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR);
}

public WeakHashMap(Map<? extends K, ? extends V> m) {
		this(Math.max((int) (m.size() / DEFAULT_LOAD_FACTOR) + 1,
						DEFAULT_INITIAL_CAPACITY),
					DEFAULT_LOAD_FACTOR);
		putAll(m);
}
```

## 5. Refer Links

[Java 中的 WeakHashMap](http://duqicauc.gitee.io/2017/11/10/weakhashmap-in-java/)

[WeakHashMap 实现原理及源码分析](http://blog.csdn.net/itmyhome1990/article/details/77651165)

[Java8 中的 WeakHashMap](http://ifeve.com/weakhashmap/)

[WeakHashMap 垃圾回收原理](https://www.jianshu.com/p/6904d632549f)