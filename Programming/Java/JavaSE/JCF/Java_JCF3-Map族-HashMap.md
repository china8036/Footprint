- [Java 集合：Map 族 - HashMap 实现类](#java-集合map-族---hashmap-实现类)
    - [1. 基本概念](#1-基本概念)
    - [2. 常用 API](#2-常用-api)
    - [3. 源码分析](#3-源码分析)
        - [3.1. JDK 对 HashMap 的改进](#31-jdk-对-hashmap-的改进)
            - [3.1.1. 引入红黑树](#311-引入红黑树)
            - [3.1.2. 用位运算代替常规运算，以提升效率](#312-用位运算代替常规运算以提升效率)
            - [3.1.3. 扩容方法的改进](#313-扩容方法的改进)
        - [3.2. 存储结构](#32-存储结构)
        - [3.3. 类定义](#33-类定义)
        - [3.4. 关键属性](#34-关键属性)
        - [3.5. 关键内部类](#35-关键内部类)
        - [3.6. 初始化操作](#36-初始化操作)
        - [3.7. 添加元素](#37-添加元素)
            - [3.7.1. put() 方法](#371-put-方法)
            - [3.7.2. 哈希计算和扰动函数](#372-哈希计算和扰动函数)
            - [3.7.3. 扩容运算 resize()](#373-扩容运算-resize)
            - [3.7.4. putVal() 方法](#374-putval-方法)
            - [3.7.5. putIfAbsent() 方法](#375-putifabsent-方法)
        - [3.8. 删除元素](#38-删除元素)
        - [3.9. 获取元素](#39-获取元素)
        - [3.10. 遍历集合](#310-遍历集合)
        - [3.11. HashMap 的线程不安全](#311-hashmap-的线程不安全)
            - [3.11.1. resize 死循环](#3111-resize-死循环)
            - [3.11.2. Fast-fail](#3112-fast-fail)
    - [4. HashTable](#4-hashtable)
    - [6. Refer Links](#6-refer-links)

# Java 集合：Map 族 - HashMap 实现类

## 1. 基本概念

散列映射 [HashMap](https://docs.oracle.com/javase/9/docs/api/java/util/HashMap.html) 是 Map 接口的经典实现，大多数时候使用 Map 集合时都是使用 HashMap 这个实现类。

HashMap 特点：
- HashMap 是线程不安全的。若多个线程同时修改了 HashMap，则必须通过代码来保证其同步。
- HashMap 中元素的 Key 和 Value 都可以为 null。
- 遍历 HashMap 无序。

散列将元素分散在散列表的各个位置上，若元素的散列码发生了变化，则元素在数据结构中的位置也会发生变化。

## 2. 常用 API

- 构造器
  - `HashMap​()`: Constructs an empty HashMap with the default initial capacity (16) and the default load factor (0.75).
  - `HashMap​(int initialCapacity)`: Constructs an empty HashMap with the specified initial capacity and the default load factor (0.75).
  - `HashMap​(int initialCapacity, float loadFactor)`: Constructs an empty HashMap with the specified initial capacity and load factor.
  - `HashMap​(Map<? extends K,? extends V> m)`: Constructs a new HashMap with the same mappings as the specified Map.
- 添加 Key-Value 元素
  - `V put​(K key, V value)`: Associates the specified value with the specified key in this map.
  - `V putIfAbsent(K key, V value)`: If the specified key is not already associated with a value (or is mapped to null) associates it with the given value and returns null, else returns the current value.
  - `void putAll(Map<? extends K,? extends V> m)`: Copies all of the mappings from the specified map to this map.
- 删除 Key-Value 元素
  - `V remove(Object key)`: Removes the mapping for the specified key from this map if present.
  - `boolean remove(Object key, Object value)`: Removes the entry for the specified key only if it is currently mapped to the specified value.
- 根据 Key 获取 Key-Value 元素
  - `V get​(Object key)`: Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key.
  - `V getOrDefault​(Object key, V defaultValue)`: Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key.
- 遍历集合
  - `Set<K> keySet​()`: Returns a Set view of the keys contained in this map.
  - `Collection<V> values​()`: Returns a Collection view of the values contained in this map.
  - `Set<Map.Entry<K,V>> entrySet()`: Returns a Set view of the mappings contained in this map.
  - `void forEach(BiConsumer<? super K,? super V> action)`: Performs the given action for each entry in this map until all entries have been processed or the action throws an exception.

## 3. 源码分析

### 3.1. JDK 对 HashMap 的改进

在 JDK8 中对 HashMap 的实现进行了许多改进和优化。

#### 3.1.1. 引入红黑树

如果 Hash 算法不好，会使得 Hash 表退化为顺序查找，即使负载因子和 Hash 算法优化再多，也无法避免出现链表过长的情景（虽然这个概率很低），于是在 JDK1.8 中，对 HashMap 做了优化，引入了红黑树。

**具体做法就是当 hash 表中每个桶附带的链表长度默认超过 8 时，链表就转换为红黑树结构，从而提高 HashMap 的性能，因为红黑树的增删改是 O(logn)，而不是 O(n)**。

#### 3.1.2. 用位运算代替常规运算，以提升效率

- 定址方法（散列索引计算）的改进：用与运算替代模运算

	要想查找表中元素的位置，需要先计算其散列码，然后与桶的总数取余，所得到的结果就是保存这个元素的桶的索引。例，某个对象散列码为 76268，且有 128 个桶，则对象应保存在第 108 号桶中（``76268 % 128=108`）。

	而**从 JDK8 开始，利用了哈希表桶的数量总为 2 的幂的特点，使用与运算代替了复杂的模运算（用 `hashcode & (table.length-1)` 替代 `hashcode % (table.length)`），大大提高了效率**。

- 用 `if ((e.hash & oldCap) == 0)` 来判断扩容之后节点 e 处于低区还是高区。

#### 3.1.3. 扩容方法的改进

JDK 1.8 以前的扩容原理是使用新的（2 倍旧长度）的数组代替，把旧数组的内容放到新数组，需要重新计算 hash 和计算 hash 表的位置，非常耗时。因此 JDK 1.8 对 hashmap 的扩容方法有了改进。

假设 hash 表的长度是 16，那么最后一个索引 15 对应二进制是：
```
0000 0000， 0000 0000， 0000 0000， 0000 1111 = 15
```
扩容之前有两个 key，分别是 k1 和 k2：

k1 的 hash：
```
0000 0000， 0000 0000， 0000 0000， 0000 1111 = 15
```
k2 的 hash：
```
0000 0000， 0000 0000， 0000 0000， 0001 1111 = 15
```
hash 值和 15 模得到：
```
k1：0000 0000， 0000 0000， 0000 0000， 0000 1111 = 15

k2：0000 0000， 0000 0000， 0000 0000， 0000 1111 = 15
```
扩容之后表长对应为 32，则最后一个索引 31 的二进制表示为：
```
0000 0000， 0000 0000， 0000 0000， 0001 1111 = 31
```
重新 hash 之后得到：
```
k1：0000 0000， 0000 0000， 0000 0000， 0000 1111 = 15

k2：0000 0000， 0000 0000， 0000 0000， 0001 1111 = 31 = 15 + 16
```
观察发现：

**如果扩容后新增的位是 0，那么 rehash 索引不变，否则才会改变，并且变为 `原来的索引 + 旧 hash 表的长度`，故我们只需看原 hash 表长新增的 bit 是 1 还是 0，如果是 0，索引不变，如果是 1，索引变成原索引 + 旧表长，根本不用像 JDK 7 那样 rehash，省去了重新计算 hash 值的时间**。

而且新增的 bit 是 0 还是 1 可以认为是随机的，因此 resize 的过程，还能均匀的把之前的冲突节点分散。

### 3.2. 存储结构

在 Java 中，散列表一般使用链表数组实现，每个链表称为桶。

实现 HashMap 的存储结构同样是一个链表数组，即一个 Node<K, V>类型的一维数组，数组的每个槽位称为桶 (bin)，存放着 key 的 hash 值相等的元素。

由于 HashMap 使用链地址方法来解决哈希冲突，因此多个 hash 值相等的元素通过构建新的链表来组织。

**从 JDK 8 开始，当一个桶的链表长度达到阈值 8 时，会自动将链表转换为红黑树，以提升查询、插入效率**。

![image](http://img.cdn.firejq.com/jpg/2018/3/12/09c33c48612692ac86eb2e9880219389.jpg)

![image](http://img.cdn.firejq.com/jpg/2018/3/12/beb26cb90e5cd32b250b03020b43f1b8.jpg)

### 3.3. 类定义

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {/*...*/}
```

### 3.4. 关键属性

```java
// 默认容量，必须是 2 的幂，此处为 2^4
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
// 最大容量，必须是 2 的幂，此处为 2^30
static final int MAXIMUM_CAPACITY = 1 << 30;
// 默认加载因子
static final float DEFAULT_LOAD_FACTOR = 0.75f;
// 链表转成红黑树的阈值
static final int TREEIFY_THRESHOLD = 8;
// 红黑树转为链表的阈值
static final int UNTREEIFY_THRESHOLD = 6;
// 由链表转成红黑树的容量的最小阈值
static final int MIN_TREEIFY_CAPACITY = 64;
// HashMap 中存储的键值对的数量
transient int size;
// HashMap 中更改存储结构的累计次数，由于 HashMap 是线程不安全的，所以要保存 modCount，以用于 fail-fast 策略
transient int modCount;
// 扩容阈值，当 size >= threshold 时，就会自动扩容
int threshold;
// HashMap 的装填因子。当表太满时，就需要对散列表进行再散列，装填因子决定了何时对表进行再散列：当 size >= DEFAULT_INITIAL_CAPACITY*loadFactor 时，就会进行自动扩容
final float loadFactor;

// Key-Value 集合，没有重复元素
transient Set<Map.Entry<K,V>> entrySet;
// 继承自 AbstractMap 类，Key 集合，没有重复元素
transient Set<K> keySet;
// 继承自 AbstractMap 类，Value 集合，允许有重复元素
transient Collection<V> values;

// 用于实现 HashMap 的哈希表，实际上是元素类型为 Node<K,V>的单向链表（以数组实现，每个数组元素都存储 next 元素的引用）。必要时，该数组会自动扩容，数组长度总保持为 2 的幂（允许特定操作时长度为 0）
transient Node<K,V>[] table;
```

### 3.5. 关键内部类

- Node 类（用于表示一个哈希表的节点和发生哈希冲突时扩展链表的节点，是实际保存 key-value 的数据结构）
  ```java
  static class Node<K,V> implements Map.Entry<K,V> {
      final int hash;
      // 从这里可以知道，HashMap 底层实现中每个节点存储的是 key-value 的键值对，不只是存储了 value
      final K key;
      V value;
      // 指向下一个节点的引用
      Node<K,V> next;

      // 省略部分实现代码

      // 使用键值对计算 hashcode 的值：将 key 的 hashCode 和 value 的 hashCode 执行异或运算得到一个节点的 hashcode 值
      public final int hashCode() {
          return Objects.hashCode(key) ^ Objects.hashCode(value);
      }

       // 为该节点设置新的 value，返回旧的 value
      public final V setValue(V newValue) {
          V oldValue = value;
          value = newValue;
          return oldValue;
      }

      public final boolean equals(Object o) {
          if (o == this)
              return true;
          if (o instanceof Map.Entry) {
              Map.Entry<?,?> e = (Map.Entry<?,?>)o;
              if (Objects.equals(key, e.getKey()) &&
                  Objects.equals(value, e.getValue()))
                  return true;
          }
          return false;
      }
  }
  ```

- TreeNode 类（用于表示一个红黑树的节点）
  ```java
  static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;

        // 省略具体方法实现
  }
  ```

### 3.6. 初始化操作

```java
public HashMap() {
    // 默认构造函数，设置装填因子为默认的 0.75f
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
}
public HashMap(int initialCapacity) {
    // 指定初始化容量的构造函数
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}
// 同时指定初始化容量 以及 加载因子， 用的很少，一般不会修改 loadFactor
public HashMap(int initialCapacity, float loadFactor) {
    // 边界处理
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                            initialCapacity);
    // 初始容量最大不能超过 2 的 30 次方
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    // 显然加载因子不能为负数
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                            loadFactor);
    this.loadFactor = loadFactor;
    // 设置阈值为》= 初始化容量的 2 的 n 次方的值
    this.threshold = tableSizeFor(initialCapacity);
}
// 新建一个哈希表，同时将另一个 map m 里的所有元素加入表中
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}
```

```java
// 根据期望容量 cap，返回 2 的 n 次方形式的哈希桶的实际容量 length。返回值一般不小于 cap
static final int tableSizeFor(int cap) {
    // 经过下面的或运算和位移运算，n 最终各位都是 1
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    // 判断 n 是否越界，返回 2 的 n 次方作为 table（哈希桶）的阈值
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

### 3.7. 添加元素

#### 3.7.1. put() 方法

HashMap 中添加元素通过 put() 方法来实现：
```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

#### 3.7.2. 哈希计算和扰动函数

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
从源码可知，HashMap 中 hash 值的计算过程不应只是直接使用 key 的 hashCode() 返回值，还会经过扰动函数的扰动。

HashMap 中 Key 哈希的计算过程如下：
- 当 key=null 时，也是有 hash 值的，是 0，所以，HashMap 的 key 是可以为 null 的，对比 HashTable 源码我们可以知道，HashTable 的 key 直接进行了 hashCode，如果 key 为 null 时，会抛出异常，所以 HashTable 的 key 不可以是 null。

- 当 key!=null 时，首先会计算出 key 的 hashCode() 为 h，然后使用扰动函数对 h 进行扰动，即将 h 与 h 无条件右移 16 位后的二进制进行按位异或 (^)，才得到最终的 hash 值。这个 hash 值就是键值对存储在数组中的位置。

P.S. 为什么不直接使用 key 的 hashCode() 作为哈希计算结果，而要使用扰动计算进行混淆？

由于 hashCode() 是 int 类型，取值范围是 40 多亿，只要哈希函数映射的比较均匀松散，碰撞几率是很小的。但就算原本的 hashCode() 取得很好，每个 key 的 hashCode() 不同，但是由于 HashMap 的哈希桶的长度远比 hash 取值范围小，默认是 16，所以当对 hash 值以桶的长度取余，以找到存放该 key 的桶的下标时，由于取余是通过与操作完成的，会忽略 hash 值的高位。因此**只有 hashCode() 的低位参加运算，发生不同的 hash 值，但是得到的 index 相同的情况的几率（hash 碰撞碰撞率）会大大增加**。

**扰动函数就是为了解决 hash 碰撞的。它会综合 hash 值高位和低位的特征，并存放在低位，因此在与运算时，相当于高低位一起参与了运算，从而减少了 hash 碰撞的概率（在 JDK8 之前，扰动函数会扰动四次，JDK8 简化了这个操作，降低了混淆程度）**。

因此，HashMap 中 hash 值的计算过程不应只是直接使用 key 的 hashCode() 返回值，还会经过扰动函数的扰动，以使 hash 值分布更加均衡。

#### 3.7.3. 扩容运算 resize()

由于哈希桶的数据结构是数组，数组的大小必须在创建时分配完毕，所以自然会涉及到扩容的问题。

**当 HashMap 的容量达到 threshold 域值时，就会触发扩容操作 `final Node<K,V>[] resize()`。扩容前后，哈希桶的长度一定会是 2 的次方，从而在根据 key 的 hash 值寻找对应的哈希桶时，可以用位运算替代取余操作，更加高效**。

扩容操作时，会 new 一个新的 Node 数组作为哈希桶，然后将原哈希表中的所有数据 (Node 节点) 移动到新的哈希桶中，相当于对原哈希表中所有的数据重新做了一个 put 操作。所以性能消耗很大，可想而知，在哈希表的容量越大时，性能消耗越明显。

```java
final Node<K,V>[] resize() {
    // oldTab 为当前表的哈希桶
    Node<K,V>[] oldTab = table;
    // 当前哈希桶的容量 length
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    // 当前的阈值
    int oldThr = threshold;
    // 初始化新的容量和阈值为 0
    int newCap, newThr = 0;
    // 如果当前容量大于 0
    if (oldCap > 0) {
        // 如果当前容量已经到达上限
        if (oldCap >= MAXIMUM_CAPACITY) {
            // 则设置阈值是 2 的 31 次方 -1
            threshold = Integer.MAX_VALUE;
            // 同时返回当前的哈希桶，不再扩容
            return oldTab;
        }// 否则新的容量为旧的容量的两倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                  oldCap >= DEFAULT_INITIAL_CAPACITY)// 如果旧的容量大于等于默认初始容量 16
            // 那么新的阈值也等于旧的阈值的两倍
            newThr = oldThr << 1; // double threshold
    }// 如果当前表是空的，但是有阈值。代表是初始化时指定了容量、阈值的情况
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;// 那么新表的容量就等于旧的阈值
    else {}// 如果当前表是空的，而且也没有阈值。代表是初始化时没有任何容量 / 阈值参数的情况               // zero initial threshold signifies using defaults
        newCap = DEFAULT_INITIAL_CAPACITY;// 此时新表的容量为默认的容量 16
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);// 新的阈值为默认容量 16 * 默认加载因子 0.75f = 12
    }
    if (newThr == 0) {// 如果新的阈值是 0，对应的是  当前表是空的，但是有阈值的情况
        float ft = (float)newCap * loadFactor;// 根据新表容量 和 加载因子 求出新的阈值
        // 进行越界修复
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    // 更新阈值
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
    // 根据新的容量 构建新的哈希桶
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    // 更新哈希桶引用
    table = newTab;
    // 如果以前的哈希桶中有元素
    // 下面开始将当前哈希桶中的所有节点转移到新的哈希桶中
    if (oldTab != null) {
        // 遍历老的哈希桶
        for (int j = 0; j < oldCap; ++j) {
            // 取出当前的节点 e
            Node<K,V> e;
            // 如果当前桶中有元素，则将链表赋值给 e
            if ((e = oldTab[j]) != null) {
                // 将原哈希桶置空以便 GC
                oldTab[j] = null;
                // 如果当前链表中就一个元素，（没有发生哈希碰撞）
                if (e.next == null)
                    // 直接将这个元素放置在新的哈希桶里。
                    // 注意这里取下标 是用 哈希值 与 桶的长度 -1 。 由于桶的长度是 2 的 n 次方，这么做其实是等于 一个模运算。但是效率更高
                    newTab[e.hash & (newCap - 1)] = e;
                    // 如果发生过哈希碰撞 , 而且是节点数超过 8 个，转化成了红黑树（暂且不谈 避免过于复杂， 后续专门研究一下红黑树）
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                // 如果发生过哈希碰撞，节点数小于 8 个。则要根据链表上每个节点的哈希值，依次放入新哈希桶对应下标位置。
                else { // preserve order
                    // 因为扩容是容量翻倍，所以原链表上的每个节点，现在可能存放在原来的下标，即 low 位， 或者扩容后的下标，即 high 位。 high 位 =  low 位 + 原哈希桶容量
                    // 低位链表的头结点、尾节点
                    Node<K,V> loHead = null, loTail = null;
                    // 高位链表的头节点、尾节点
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;// 临时节点 存放 e 的下一个节点
                    do {
                        next = e.next;
                        // 这里又是一个利用位运算 代替常规运算的高效点： 利用哈希值 与 旧的容量，可以得到哈希值去模后，是大于等于 oldCap 还是小于 oldCap，等于 0 代表小于 oldCap，应该存放在低位，否则存放在高位
                        if ((e.hash & oldCap) == 0) {
                            // 给头尾节点指针赋值
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }// 高位也是相同的逻辑
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }// 循环直到链表结束
                    } while ((e = next) != null);
                    // 将低位链表存放在原 index 处，
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    // 将高位链表存放在新 index 处
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

#### 3.7.4. putVal() 方法

从 put() 方法的实现中可以发现，执行添加元素的具体逻辑封装在 `final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict)` 方法中。

参数说明：
- hash：表示 key 的 hash 值。
- key：待存储的 key 值。
- value：待存储的 value 值
- onlyIfAbsent：这个参数表示，是否需要替换相同的 value 值，如果为 true，表示不替换已经存在的 value。
- evict：如果为 false，表示数组是新增模式。

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    // tab 存放 当前的哈希桶，p 用作临时链表节点
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 首先判断当前 HashMap 的数组是否为空
    if ((tab = table) == null || (n = tab.length) == 0)
        // 如果为空，则调用 resize() 方法，对 HashMap 进行扩容，这次扩容的结果就是 HashMap 的初始化一个长度为 16 的数组。获取到数组的长度 n
        n = (tab = resize()).length;

    // 执行取下标运算：`hashCode & (length -1)`，由于桶的长度是 2 的 n 次方，这么做其实相当于 `hashCode % length`（严谨的证明请参考更多的资料），但代替了复杂的取模运算，效率更高。
    // 取下标运算所得到的 hash 值即对应于数组中的位置，从 tab 中将这个位置上面的内容取出，如果为 null，表示没有发生哈希碰撞，直接构建一个新节点 Node，挂载在 index 处即可
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    // 如果不为 null，表示发生了哈希冲突
    else {
        Node<K,V> e; K k;
        // 如果哈希值相等，key 也相等，则直接覆盖 value 即可
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;

        // 如果 tab[i] 的 key 不是我们传入的 key，首先要判断 p 这个 Node 是不是红黑树，如果是红黑树，则直接向红黑树新增一个数据即可
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);

        // 如果 tab[i] 的 key 不是我们传入的 key，且 p 这个 Node 不是红黑树，说明 p 是单向链表，因此遍历链表，找到链表的尾部，将节点新增到尾部即可
        else {
            // 遍历链表
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {// 遍历到尾部，追加新节点到尾部
                    p.next = newNode(hash, key, value, null);
                    // 如果追加节点后，链表长度达到转换为红黑树的阈值，则转化为红黑树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                // 如果在链表中还存在相同的 key，则直接跳出循环，在下边执行覆盖即可
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        // 如果 e 不是 null，说明有需要覆盖的节点，则覆盖节点值，并返回原 oldValue
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            // 这是一个空实现的函数，用作 LinkedHashMap 重写使用
            afterNodeAccess(e);
            return oldValue;
        }
    }
    // 如果执行到了这里，说明插入了一个新的节点，所以会修改 modCount，以及返回 null

    // modCount 执行自增
    ++modCount;
    // 更新 size，并判断是否需要扩容
    if (++size > threshold)
        resize();
    // 这是一个空实现的函数，用作 LinkedHashMap 重写使用
    afterNodeInsertion(evict);
    return null;
}

// 构建一个新的链表节点
Node<K,V> newNode(int hash, K key, V value, Node<K,V> next) {
    return new Node<>(hash, key, value, next);
}
```

流程图：

![image](http://img.cdn.firejq.com/jpg/2018/3/12/346b09ee79f8343df3d5287eeed67304.jpg)

若执行结果是插入了一个新的节点，返回 null；若执行结果是覆盖了旧 value，返回旧 value。

根据 hash 值计算的特点，我们不难得到以下规律：
- equals() 为 true 的两个 value，其 hash 值肯定相等。
- equals() 为 false 的两个 value，其 hash 值可能相等（此时即为 hash 冲突）、也可能不相等。

因此，可以总结出以下 HashMap 的插入规律：

| hashCode() | equals() | 是否插入 | 备注                                       |
| ---------- | -------- | ---- | ---------------------------------------- |
| 相等         | 相等       | 否    | 正常情况。                                    |
| 不相等        | 相等       | 是    | 异常情况。正常插入操作不会出现这种情况，只有当特别修改了 hash 值才会发生。 |
| 不相等        | 不相等      | 是    | 正常情况。                                    |
| 相等         | 不相等      | 是    | hash 冲突，通过链表法解决冲突。                       |

可以得出结论：
- 当 Key 的 equals() 相等且 hashCode() 相等时，元素在 HashMap 中的物理存储位置相同，此时会覆盖旧 value。
- 当 Key 的 equals() 不相等且 hashCode() 不相等时，元素在 HashMap 中的物理存储位置不相同。
- 当 Key 的 equals() 不相等而 hashCode() 相等时，元素在 HashMap 中的物理存储位置不相同。

#### 3.7.5. putIfAbsent() 方法

```java
// 往表中插入 key-value, 若 key 对应的 value 已存在，不会覆盖旧 value，而是丢弃新 value
@Override
public V putIfAbsent(K key, V value) {
    return putVal(hash(key), key, value, true, true);
}
```

### 3.8. 删除元素

```java
// 如果 key 存在，则删除这个键值对，并返回 value；如果不存在，返回 null
public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
}

// 如果 key-value 键值对存在，则删除这个键值对，并返回 value；如果不存在，返回 null
@Override
public boolean remove(Object key, Object value) {
    // 这里传入了 value 同时 matchValue 为 true
    return removeNode(hash(key), key, value, true, true) != null;
}
```

```java
// 从哈希表中删除某个节点
// 如果参数 matchValue 是 true，必须 key 和 value 都相等才删除
// 如果 movable 参数是 false，在删除节点时，不移动其他节点
final Node<K,V> removeNode(int hash, Object key, Object value,
                            boolean matchValue, boolean movable) {
    // p 是待删除节点的前置节点
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    // 如果哈希表不为空，则根据 hash 值算出的 index 下 有节点的话。
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        //node 是待删除节点
        Node<K,V> node = null, e; K k; V v;
        // 如果链表头的就是需要删除的节点
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;// 将待删除节点引用赋给 node
        else if ((e = p.next) != null) {// 否则循环遍历 找到待删除节点，赋值给 node
            if (p instanceof TreeNode)
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            else {
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                          (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        // 如果有待删除节点 node，  且 matchValue 为 false，或者值也相等
        if (node != null && (!matchValue || (v = node.value) == value ||
                              (value != null && value.equals(v)))) {
            if (node instanceof TreeNode)
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            else if (node == p)// 如果 node ==  p，说明是链表头是待删除节点
                tab[index] = node.next;
            else// 否则待删除节点在表中间
                p.next = node.next;
            ++modCount;// 修改 modCount
            --size;// 修改 size
            afterNodeRemoval(node);//LinkedHashMap 回调函数
            return node;
        }
    }
    return null;
}
```

### 3.9. 获取元素

获取元素通过 get() 方法实现。
```java
public V get(Object key) {
    Node<K,V> e;
    // 传入扰动后的哈希值和 key，找到目标节点 Node
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

```java
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    // 查找过程和删除基本相同，找到则返回节点，否则返回 null
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

### 3.10. 遍历集合

迭代器源码：
```java
abstract class HashIterator {
    Node<K,V> next;        // next entry to return
    Node<K,V> current;     // current entry
    int expectedModCount;  // for fast-fail
    int index;             // current slot

    HashIterator() {
        expectedModCount = modCount;
        Node<K,V>[] t = table;
        current = next = null;
        index = 0;
        // next 初始时，指向哈希桶上第一个不为 null 的链表头
        if (t != null && size > 0) { // advance to first entry
            do {} while (index < t.length && (next = t[index++]) == null);
        }
    }

    public final boolean hasNext() {
        return next != null;
    }

    // 由这个方法可以看出，HashMap 的遍历顺序与元素的添加顺序无关，属于无序遍历
    final Node<K,V> nextNode() {
        Node<K,V>[] t;
        Node<K,V> e = next;
        // fail-fast 策略
        if (modCount != expectedModCount)
            throw new ConcurrentModificationException();
        if (e == null)
            throw new NoSuchElementException();
        // 依次取链表下一个节点
        if ((next = (current = e).next) == null && (t = table) != null) {
            // 如果当前链表节点遍历完了，则取哈希桶下一个不为 null 的链表头
            do {} while (index < t.length && (next = t[index++]) == null);
        }
        return e;
    }

    public final void remove() {
        Node<K,V> p = current;
        if (p == null)
            throw new IllegalStateException();
        // fail-fast 策略
        if (modCount != expectedModCount)
            throw new ConcurrentModificationException();
        current = null;
        K key = p.key;
        // 最终还是利用 removeNode 删除节点
        removeNode(hash(key), key, null, false, false);
        expectedModCount = modCount;
    }
}
```

由迭代器的实现可以看出，遍历 HashMap 时，顺序是按照哈希桶从低到高，链表从前往后遍历的。因此，HashMap 的遍历顺序与元素的添加顺序无关，属于无序遍历。

P.S.

- 在遍历 HashMap 时（toString() 或 entrySet().iterator()），[为什么输出结果是有序的](https://www.zhihu.com/question/28414001)？

  > HashSet 的迭代器在输出时“不保证有序”，但也不是“保证无序”。也就是说，输出时有序也是允许的，但是你的程序不应该依赖这一点，所谓“有序”纯粹是个巧合的情况而已。

  1. 首先，Iterator 的实现是根据内部插入位置的顺序（布局）来进行遍历的。
  1. 其次，在 HashMap 中，**如果 Key 为 Integer**，由于 JDK 8 的 java.util.HashMap 内的 hash 算法比 JDK 7 的混淆程度低，导致 Key 在 [0, 2^32-1] 范围内经过 HashMap.hash() 之后还是得到 Key 本身的值，因此**只要 Key 的值落入 [0, 2^32-1] 这个范围内，且 load factor 让这个 HashMap 没有 hash 冲突**，那么 Key 在 HashMap 中的物理插入位置就会正好按照 Key 值的大小顺序插入到 HashMap 的 table 中。
  1. 于是，在使用迭代器遍历 HashMap 时，就会出现“有序遍历”的假象了。**如果更换 JDK 版本，或者更换 JVM 实现，结果就可能会完全不一样**。

### 3.11. HashMap 的线程不安全

HashMap 的线程不安全主要体现在 resize 时的死循环及使用迭代器时的 fast-fail 上。

#### 3.11.1. resize 死循环

<!-- TODO: -->

[疫苗：JAVA HASHMAP 的死循环](https://coolshell.cn/articles/9606.html)

当 HashMap 的 size 超过 `Capacity*loadFactor` 时，需要对 HashMap 进行扩容。具体方法是，创建一个新的，长度为原来 Capacity 两倍的数组，保证新的 Capacity 仍为 2 的 N 次方，从而保证上述寻址方式仍适用。同时需要通过如下 transfer 方法将原来的所有数据全部重新插入（rehash）到新的数组中。
```java
void transfer(Entry[] newTable, boolean rehash) {
  int newCapacity = newTable.length;
  for (Entry<K,V> e : table) {
    while(null != e) {
      Entry<K,V> next = e.next;
      if (rehash) {
        e.hash = null == e.key ? 0 : hash(e.key);
      }
      int i = indexFor(e.hash, newCapacity);
      e.next = newTable[i];
      newTable[i] = e;
      e = next;
    }
  }
}
```
而该方法并不保证线程安全，而且在多线程并发调用时，可能出现死循环。其执行过程如下：
1. 遍历原数组中的元素
1. 对链表上的每一个节点遍历：用 next 取得要转移那个元素的下一个，将 e 转移到新数组的头部，使用头插法插入节点
1. 循环 2，直到链表节点全部转移
1. 循环 1，直到所有元素全部转移
从步骤 2 可见，转移时链表顺序反转。

假设有两个线程同时执行了 put 操作并引发了 rehash，执行了 transfer 方法，并假设线程一进入 transfer 方法并执行完 next = e.next 后，因为线程调度所分配时间片用完而“暂停”，此时线程二完成了 transfer 方法的执行。接着线程 1 被唤醒，继续执行第一轮循环的剩余部分：
```java
e.next = newTable[1] = null
newTable[1] = e = key(5)
e = next = key(9)
```
继续执行扩容流程，就会形成循环链表，造成下一次访问该链表时的死循环。

#### 3.11.2. Fast-fail

当 HashMap 的 iterator() 方法被调用时，会构造并返回一个新的 EntryIterator 对象，并将 EntryIterator 的 expectedModCount 设置为 HashMap 的 modCount（该变量记录了 HashMap 被修改的次数）。
```java
HashIterator() {
  expectedModCount = modCount;
  if (size > 0) { // advance to first entry
  Entry[] t = table;
  while (index < t.length && (next = t[index++]) == null)
    ;
  }
}
```
在通过该 Iterator 的 next 方法访问下一个 Entry 时，它会先检查自己的 expectedModCount 与 HashMap 的 modCount 是否相等，如果不相等，说明 HashMap 被修改，直接抛出 ConcurrentModificationException。该 Iterator 的 remove 方法也会做类似的检查。该异常的抛出意在提醒用户及早意识到线程安全问题。

## 4. HashTable

[HashTable](https://docs.oracle.com/javase/9/docs/api/java/util/Hashtable.html) 比较古老，是 JDK1.0 就引入的类，而 HashMap 是 1.2 引进的 Map 的一个实现。

HashTable 继承于 Dictionary 类，实现了 `Map`, `Cloneable`,`Serializable` 接口，同样是基于哈希表实现的，其实类似 HashMap，只不过有些区别，HashTable 同样每个元素是一个 key-value 对，其内部也是通过单链表解决冲突问题，容量不足（超过了阀值）时，同样会自动增长。

## 6. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)

[JAVA 源码分析 - HashMap 源码分析（一)](https://www.jianshu.com/p/7dcff1fd05ad)

[JAVA 源码分析 - HashMap 源码分析（二)](https://www.jianshu.com/p/fd22f4965369)

[面试必备：HashMap 源码解析（JDK8）](http://blog.csdn.net/zxt0601/article/details/77413921)

[coolblog: HashMap 源码详细分析 (JDK1.8)](http://www.coolblog.xyz/2018/01/18/HashMap-%E6%BA%90%E7%A0%81%E8%AF%A6%E7%BB%86%E5%88%86%E6%9E%90-JDK1-8/)

[Java 集合专题总结（1）：HashMap 和 HashTable 源码学习和面试总结](https://www.cnblogs.com/kubixuesheng/p/6144535.html)

[Java 基础——HashTable 源码分析](http://blog.csdn.net/qq_30379689/article/details/72618060)

[Java 遍历 HashSet 为什么输出是有序的？](https://www.zhihu.com/question/28414001)