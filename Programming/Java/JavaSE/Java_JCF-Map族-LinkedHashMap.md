- [Java 集合：Map 族 - LinkedHashMap 实现类](#java-%E9%9B%86%E5%90%88%EF%BC%9Amap-%E6%97%8F---linkedhashmap-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 常用 API](#2-%E5%B8%B8%E7%94%A8-api)
	- [3. 源码分析](#3-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
		- [3.1. 类定义](#31-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.2. 存储结构](#32-%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84)
		- [3.3. 关键属性](#33-%E5%85%B3%E9%94%AE%E5%B1%9E%E6%80%A7)
		- [3.4. 关键内部类](#34-%E5%85%B3%E9%94%AE%E5%86%85%E9%83%A8%E7%B1%BB)
		- [3.5. 初始化](#35-%E5%88%9D%E5%A7%8B%E5%8C%96)
		- [3.6. 添加元素](#36-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
		- [3.7. 删除元素](#37-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
		- [3.8. 获取元素](#38-%E8%8E%B7%E5%8F%96%E5%85%83%E7%B4%A0)
		- [3.9. containsValue() 方法](#39-containsvalue-%E6%96%B9%E6%B3%95)
	- [4. Refer Links](#4-refer-links)
    
# Java 集合：Map 族 - LinkedHashMap 实现类

## 1. 基本概念

[LinkedHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedHashMap.html) 是 HashMap 的扩展类。

由于继承自 HashMap，因此 HashMap 所具有的特性 LinkedHashMap 都有，比如扩容的策略，哈希桶长度一定是 2 的 N 次方等。**相比较 HashMap，LinkedHashMap 最大的区别在于，它使用双向链表根据 Key 来维护 Key-Value 的次序，使得遍历 LinkedHashMap 时，元素的访问顺序与元素的插入顺序相同。**

LinkedHashMap 可以避免对 HashMap/HashTable 中 Key-Value 进行排序，只要在插入时按顺序插入即可，同时又避免使用 TreeMap 所增加的成本。

由于 LinkedHashMap 需要维护元素的插入顺序，因此性能略低于 HashMap，但由于它以双向链表维护着内部顺序，所以在迭代访问 Map 中的全部元素时将会比 HashMap 有更快的迭代速度。

## 2. 常用 API

LinkedHashMap 所提供的 API 与 HashMap 基本上完全相同，但在实现上按照其特殊需求，对部分方法进行了重写。

示例：
```java
public static void main(String [] args) {
		Map<String, String> map = new LinkedHashMap<>();
		map.put("1", "a");
		map.put("2", "b");
		map.put("3", "c");
		map.put("4", "d");
		Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
		while (iterator.hasNext()) {
				System.out.println(iterator.next());
		}

		System.out.println("以下是 accessOrder=true 的情况：");

		// 设置 accessOrder 为 true
		map = new LinkedHashMap<String, String>(10, 0.75f, true);
		map.put("1", "a");
		map.put("2", "b");
		map.put("3", "c");
		map.put("4", "d");
		map.get("2");// 2 移动到了内部的链表末尾
		map.get("4");// 4 移动到了内部的链表末尾
		map.put("3", "e");// 3 移动到了内部的链表末尾
		map.put(null, null);// 插入新的节点 null
		map.put("5", null);// 插入新的节点 5
		iterator = map.entrySet().iterator();
		while (iterator.hasNext()) {
				System.out.println(iterator.next());
		}
}
```
运行结果
```
1=a
2=b
3=c
4=d
以下是 accessOrder=true 的情况：
1=a
2=b
4=d
3=e
null=null
5=null
```

## 3. 源码分析

### 3.1. 类定义

```java
public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>
```

### 3.2. 存储结构

LinkedHashMap 继承自 HashMap，在其基础上，内部还额外维护了一个双向链表，在每次插入数据，或者访问、修改数据时，会增加节点、或调整链表的节点顺序，使得在迭代时可以按照插入元素的顺序进行输出。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/12/44d6de572f1898afae01c9859ca65e8e.jpg)

上图中，淡蓝色的箭头表示前驱引用，红色箭头表示后继引用。

当有新的节点插入时，执行完 HashMap 的元素插入逻辑后，在 LinkedHashMap 中会将 tail 节点的后继引用指向新插入的节点，然后将 tail 指向新节点的引用，即可实现链表的更新。

### 3.3. 关键属性

```java
// 双向链表的头节点 (eldest)
transient LinkedHashMap.Entry<K,V> head;

// 双向链表的尾结点 (youngest)
transient LinkedHashMap.Entry<K,V> tail;

// 若为 false，则迭代时输出的顺序是插入节点的顺序；若为 true，则迭代时输出的顺序是按照访问节点的顺序。
// 初始化时会默认设置为 false
final boolean accessOrder;
```

### 3.4. 关键内部类

- Entry<K,V>：表示 LinkedHashMap 中双向链表的的每一个节点（相当于 HashMap 中的 Node<K,V>）
	```java
	static class Entry<K,V> extends HashMap.Node<K,V> {
			Entry<K,V> before, after;
			Entry(int hash, K key, V value, Node<K,V> next) {
					super(hash, key, value, next);
			}
	}
	```

	继承体系：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/12/a0e290531e37cfd27c4709b28c9da5e2.jpg)

### 3.5. 初始化

LinkedHashMap 的构造函数和 HashMap 相比，就是增加了一个 accessOrder 参数，用于控制迭代时的节点顺序。
```java
public LinkedHashMap() {
		super();
		accessOrder = false;
}

// 指定初始化时的容量
public LinkedHashMap(int initialCapacity) {
		super(initialCapacity);
		accessOrder = false;
}

// 指定初始化时的容量，和扩容的加载因子
public LinkedHashMap(int initialCapacity, float loadFactor) {
		super(initialCapacity, loadFactor);
		accessOrder = false;
}

// 指定初始化时的容量，和扩容的加载因子，以及迭代输出节点的顺序
public LinkedHashMap(int initialCapacity,
											float loadFactor,
											boolean accessOrder) {
		super(initialCapacity, loadFactor);
		this.accessOrder = accessOrder;
}

// 利用另一个 Map 来构建
public LinkedHashMap(Map<? extends K, ? extends V> m) {
		super();
		accessOrder = false;
		// 批量插入一个 map 中的所有数据到本集合中
		putMapEntries(m, false);
}
```

### 3.6. 添加元素

LinkedHashMap 并没有重写 HashMap 的 put() 方法、 putVal() 方法或 putMapEntries() 方法，但它重写了 HashMap 内部构建新节点的 newNode() 方法和 HashMap 专门预留给 LinkedHashMap 的 afterNodeInsertion() 方法。

- newNode()
	
	newNode() 方法会在 putVal() 方法中被调用，而 putVal() 方法会在 put() 方法和 putMapEntries() 方法中被调用。在 LinkedHashMap 重写后的 newNode() 方法中，每次构建新节点时，都会通过 `linkNodeLast(p)` 将新节点链接在内部双向链表的尾部。

	```java
	Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
			// 在构建新节点时，构建的是`LinkedHashMap.Entry` 不再是`Node`	
			LinkedHashMap.Entry<K,V> p =
					new LinkedHashMap.Entry<K,V>(hash, key, value, e);
			// 将新增的节点，连接在链表的尾部
			linkNodeLast(p);
			return p;
	}

	private void linkNodeLast(LinkedHashMap.Entry<K,V> p) {
			LinkedHashMap.Entry<K,V> last = tail;
			tail = p;
			// 集合之前是空的
			if (last == null)
					head = p;
			else {
					// 将新节点连接在链表的尾部
					p.before = last;
					last.after = p;
			}
	}
	```

- HashMap 中预留的 afterNodeInsertion() 方法
	```java
	// 回调函数，新节点插入之后回调，根据条件判断是否移除最近最少被访问的节点
	void afterNodeInsertion(boolean evict) { // possibly remove eldest
			LinkedHashMap.Entry<K,V> first;
			// 
			// LinkedHashMap 默认返回 false 则不删除节点
			if (evict && (first = head) != null && removeEldestEntry(first)) {
					K key = first.key;
					removeNode(hash(key), key, null, false, true);
			}
	}

	// 移除最近最少被访问条件之一，通过覆盖此方法，可实现不同策略的缓存
	// LinkedHashMap 默认返回 false 则不删除节点，返回 true 代表要删除最早的节点
	protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
			return false;
	}
	```

	例：覆写 LinkedHashMap 的 removeEldestEntry() 方法，实现 LRU 缓存策略。
	```java
	public class SimpleCache<K, V> extends LinkedHashMap<K, V> {
			private static final int MAX_NODE_NUM = 100;
			private int limit;
			public SimpleCache() {
					this(MAX_NODE_NUM);
			}
			public SimpleCache(int limit) {
					super(limit, 0.75f, true);
					this.limit = limit;
			}
			public V save(K key, V val) {
					return put(key, val);
			}
			public V getOne(K key) {
					return get(key);
			}
			public boolean exists(K key) {
					return containsKey(key);
			}
			
			/**
			* 判断节点数是否超限，若超限，则开始删除最近最少被访问的节点
			* @param eldest
			* @return 超限返回 true，否则返回 false
			*/
			@Override
			protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
					return size() > limit;
			}
	}
	```
	测试代码：
	```java
	public class SimpleCacheTest {
			@Test
			public void test() throws Exception {
					SimpleCache<Integer, Integer> cache = new SimpleCache<>(3);
					for (int i = 0; i < 10; i++) {
							cache.save(i, i * i);
					}
					System.out.println("插入 10 个键值对后，缓存内容：");
					System.out.println(cache + "\n");

					System.out.println("访问键值为 7 的节点后，缓存内容：");
					cache.getOne(7);
					System.out.println(cache + "\n");

					System.out.println("插入键值为 1 的键值对后，缓存内容：");
					cache.save(1, 1);
					System.out.println(cache);
			}
	}
	```
	在测试代码中，设定缓存大小为 3。在向缓存中插入 10 个键值对后，只有最后 3 个被保存下来了，其他的都被移除了。然后通过访问键值为 7 的节点，使得该节点被移到双向链表的最后位置。当我们再次插入一个键值对时，键值为 7 的节点就不会被移除。

### 3.7. 删除元素

由于 LinkedHashMap 的删除逻辑与 HashMap 的删除逻辑基本相同，因此 LinkedHashMap 也没有重写 HashMap 的 remove() 方法，但它重写了 HashMap 中专门预留给 LinkedHashMap 的 afterNodeRemoval() 方法，该方法会在 removeNode() 方法中回调，而 removeNode() 会在所有涉及到删除节点的方法中被调用。

```java
// 在删除节点 e 时，同步将 e 从双向链表上删除
void afterNodeRemoval(Node<K,V> e) { // unlink
		LinkedHashMap.Entry<K,V> p =
				(LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
		// 待删除节点 p 的前置后置节点都置空
		p.before = p.after = null;
		// 如果前置节点是 null，则现在的头结点应该是后置节点 a
		if (b == null)
				head = a;
		else// 否则将前置节点 b 的后置节点指向 a
				b.after = a;
		// 同理如果后置节点时 null，则尾节点应是 b
		if (a == null)
				tail = b;
		else// 否则更新后置节点 a 的前置节点为 b
				a.before = b;
}
```

### 3.8. 获取元素

LinkedHashMap 重写了 HashMap 的 get() 和 getOrDefault() 方法：

```java
// 增加了在成员变量（构造函数时赋值)accessOrder 为 true 的情况下，要去回调 afterNodeAccess() 函数
public V get(Object key) {
		Node<K,V> e;
		if ((e = getNode(hash(key), key)) == null)
				return null;
		if (accessOrder)
				afterNodeAccess(e);
		return e.value;
}

public V getOrDefault(Object key, V defaultValue) {
		Node<K,V> e;
		if ((e = getNode(hash(key), key)) == null)
				return defaultValue;
		if (accessOrder)
				afterNodeAccess(e);
		return e.value;
}

// 将指定节点移动到链表的尾部
void afterNodeAccess(Node<K,V> e) { // move node to last
		LinkedHashMap.Entry<K,V> last;// 原尾节点
		// 如果 accessOrder 是 true ，且原尾节点不等于 e
		if (accessOrder && (last = tail) != e) {
				// 节点 e 强转成双向链表节点 p
				LinkedHashMap.Entry<K,V> p =
						(LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
				// p 现在是尾节点， 后置节点一定是 null
				p.after = null;
				// 如果 p 的前置节点是 null，则 p 以前是头结点，所以更新现在的头结点是 p 的后置节点 a
				if (b == null)
						head = a;
				else// 否则更新 p 的前直接点 b 的后置节点为 a
						b.after = a;
				// 如果 p 的后置节点不是 null，则更新后置节点 a 的前置节点为 b
				if (a != null)
						a.before = b;
				else// 如果原本 p 的后置节点是 null，则 p 就是尾节点。 此时 更新 last 的引用为 p 的前置节点 b
						last = b;
				if (last == null) // 若原本尾节点是 null，则说明链表中就一个节点
						head = p;
				else {// 否则 更新 当前节点 p 的前置节点为 原尾节点 last， last 的后置节点是 p
						p.before = last;
						last.after = p;
				}
				// 尾节点的引用赋值成 p
				tail = p;
				// 修改 modCount，因此当处在 accessOrder=true 的模式下时，迭代 LinkedHashMap 时，如果同时查询访问数据，也会导致 fail-fast 异常，因为迭代的顺序已经改变
				++modCount;
		}
}
```

### 3.9. containsValue() 方法

LinkedHashMap 重写了 containsValue() 方法，相比 HashMap 的实现，它利用双向链表使得操作更为高效。

```java
public boolean containsValue(Object value) {
		// 遍历一遍链表，去比较有没有 value 相等的节点，并返回
		for (LinkedHashMap.Entry<K,V> e = head; e != null; e = e.after) {
				V v = e.value;
				if (v == value || (value != null && value.equals(v)))
						return true;
		}
		return false;
}
```

## 4. Refer Links

[面试必备：LinkedHashMap 源码解析（JDK8）](http://blog.csdn.net/zxt0601/article/details/77429150)

[LinkedHashMap 源码详细分析（JDK1.8）](http://www.coolblog.xyz/2018/01/24/LinkedHashMap-%E6%BA%90%E7%A0%81%E8%AF%A6%E7%BB%86%E5%88%86%E6%9E%90%EF%BC%88JDK1-8%EF%BC%89/)