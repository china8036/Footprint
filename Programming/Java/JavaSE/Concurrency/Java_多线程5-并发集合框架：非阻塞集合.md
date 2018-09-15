- [Java 多线程 并发集合框架：非阻塞集合](#java-多线程-并发集合框架非阻塞集合)
  - [1. 非阻塞 Map: ConcurrentHashMap](#1-非阻塞-map-concurrenthashmap)
    - [1.1. 常用 API](#11-常用-api)
    - [1.2. 实现原理](#12-实现原理)
      - [1.2.1. 基于分段锁的 ConcurrentHashMap](#121-基于分段锁的-concurrenthashmap)
        - [1.2.1.1. 寻址方式](#1211-寻址方式)
        - [1.2.1.2. 同步方式](#1212-同步方式)
      - [1.2.2. 基于 CAS 的 ConcurrentHashMap](#122-基于-cas-的-concurrenthashmap)
        - [1.2.2.1. 寻址方式](#1221-寻址方式)
        - [1.2.2.2. 同步方式](#1222-同步方式)
    - [1.3. ConcurrentHashMap 与 HashMap 比较](#13-concurrenthashmap-与-hashmap-比较)
  - [2. 非阻塞 Queue: ConcurrentLinkedQueue](#2-非阻塞-queue-concurrentlinkedqueue)
  - [3. 非阻塞 List: CopyOnWriteArrayList](#3-非阻塞-list-copyonwritearraylist)
  - [4. 非阻塞 Set: CopyOnWriteArraySet](#4-非阻塞-set-copyonwritearrayset)
  - [5. Refer Links](#5-refer-links)

# Java 多线程 并发集合框架：非阻塞集合

非阻塞集合 (Non-Blocking Collection) 包括添加和移除数据的方法。当集合已满或为空时，在调用添加或者移除方法时会返回 null 或者抛出异常，但线程不会被阻塞。

在 JDK 的并发包中，常见的非阻塞集合有以下几个： 
- `ConcurrentHashMap` 
- `ConcurrentSkipListMap` 
- `ConcurrentSkipListSet` 
- `ConcurrentLinkedQueue` 
- `ConcurrentLinkedDeque` 
- `CopyOnWriteArrayList` 
- `CopyOnWriteArraySet`

## 1. 非阻塞 Map: ConcurrentHashMap

HashMap 是线程不安全的，在并发环境下，可能会形成环状链表，导致 get 操作时 CPU 空转，因此在并发环境中使用 HashMap 是非常危险的。

HashTable 和 HashMap 的实现原理几乎一样，但 HashTable 是线程安全的。然而 HashTable 为实现线程安全，将 get/put 的所有相关操作都加上了 synchronized 修饰，相当于给整个哈希表加了一把全表的大锁，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。

[ConcurrentHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ConcurrentHashMap.html) 是 J.U.C(java.util.concurrent 包) 的重要成员，也是 HashMap 的一个线程安全的、支持高效并发的版本。**在默认理想状态下，ConcurrentHashMap 可以支持 16 个线程执行并发写操作及任意数量线程的读操作**。

### 1.1. 常用 API
TODO:
### 1.2. 实现原理

#### 1.2.1. 基于分段锁的 ConcurrentHashMap

Java 7 中 ConcurrentHashMap 的底层数据结构仍然是数组和链表。但与 HashMap 不同的是，ConcurrentHashMap 最外层不是一个大的数组，而是一个 Segment 的数组，每个 Segment 包含一个与 HashMap 数据结构差不多的链表数组。

整体数据结构如下图所示：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/7/25/8aa8315514d1f8a0c66f1588a6d7cf94.jpg)

ConcurrentHashMap 采用了非常精妙的**"分段锁"策略**，每一把锁锁一段数据，从而在多线程访问时不同段的数据时，就不会存在锁竞争了，有效地提高了并发效率。

**根据分段锁策略，对于同一个 Segment 的操作才需考虑线程同步，不同的 Segment 则无需考虑。**由于 ConcurrentHashMap 默认将哈希表分为 16 个 Segment，理论上就允许 16 个线程并发执行。

##### 1.2.1.1. 寻址方式

在读写某个 Key 时，先取该 Key 的哈希值。并将哈希值的高 N 位对 Segment 个数取模从而得到该 Key 应该属于哪个 Segment，接着如同操作 HashMap 一样操作这个 Segment。为了保证不同的值均匀分布到不同的 Segment，需要通过如下方法计算哈希值：
```java
private int hash(Object k) {
    int h = hashSeed;
    if ((0 != h) && (k instanceof String)) {
      return sun.misc.Hashing.stringHash32((String) k);
    }
    h ^= k.hashCode();
    h += (h <<  15) ^ 0xffffcd7d;
    h ^= (h >>> 10);
    h += (h <<   3);
    h ^= (h >>>  6);
    h += (h <<   2) + (h << 14);
    return h ^ (h >>> 16);
}
```
同样为了提高取模运算效率，通过如下计算，ssize 即为大于 concurrencyLevel 的最小的 2 的 N 次方，同时 segmentMask 为 2^N-1。对于某一个 Key 的哈希值，只需要向右移 segmentShift 位以取高 sshift 位，再与 segmentMask 取与操作即可得到它在 Segment 数组上的索引。
```java
int sshift = 0;
int ssize = 1;
while (ssize < concurrencyLevel) {
    ++sshift;
    ssize <<= 1;
}
this.segmentShift = 32 - sshift;
this.segmentMask = ssize - 1;
Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];
```

##### 1.2.1.2. 同步方式

Segment 继承自 ReentrantLock，所以我们可以很方便的对每一个 Segment 上锁。

- 对于读操作
  
  获取 Key 所在的 Segment 时，需要保证可见性。具体实现上可以使用 volatile 关键字，也可使用锁。但使用锁开销太大，而使用 volatile 时每次写操作都会让所有 CPU 内缓存无效，也有一定开销。因此，ConcurrentHashMap 使用如下方法保证可见性，取得最新的 Segment。
  ```java
  Segment<K,V> s = (Segment<K,V>)UNSAFE.getObjectVolatile(segments, u)
  ```

- 对于写操作
  
  写操作不要求同时获取所有 Segment 的锁，因为那样相当于锁住了整个 Map。它会先获取该 Key-Value 对所在的 Segment 的锁，获取成功后就可以像操作一个普通的 HashMap 一样操作该 Segment，并保证该 Segment 的安全性。

  同时由于其它 Segment 的锁并未被获取，因此理论上可支持 concurrencyLevel（等于 Segment 的个数）个线程安全的并发读写。

  获取锁时，并不直接使用 lock 来获取，因为该方法获取锁失败时会挂起（参考可重入锁）。**事实上，它使用了自旋锁，如果 tryLock 获取锁失败，说明锁被其它线程占用，此时通过循环再次以 tryLock 的方式申请锁**。如果在循环过程中该 Key 所对应的链表头被修改，则重置 retry 次数。如果 retry 次数超过一定值，则使用 lock 方法申请锁。但由于自旋操作消耗 CPU 资源比较多，因此在自旋次数超过阈值时切换为互斥锁。

#### 1.2.2. 基于 CAS 的 ConcurrentHashMap

Java 7 为实现并行访问，引入了 Segment 这一结构，实现了分段锁，理论上最大并发度与 Segment 个数相等。Java 8 为进一步提高并发性，摒弃了分段锁的方案，直接使用一个大的数组。并且与 Java 8 的 HashMap 一样，在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为 O(N)）转换为红黑树（寻址时间复杂度为 O(long(N))）。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/7/25/b89c34eda0b2d1308cf4f7cbb724c94e.jpg)

##### 1.2.2.1. 寻址方式

Java 8 的 ConcurrentHashMap 同样是通过 Key 的哈希值与数组长度取模确定该 Key 在数组中的索引。同样为了避免不太好的 Key 的 hashCode 设计，它通过如下方法计算得到 Key 的最终哈希值。不同的是，Java 8 的 ConcurrentHashMap 作者认为引入红黑树后，即使哈希冲突比较严重，寻址效率也足够高，所以作者并未在哈希值的计算上做过多设计，只是将 Key 的 hashCode 值与其高 16 位作异或并保证最高位为 0（从而保证最终结果为正整数）。
```java
static final int spread(int h) {
    return (h ^ (h >>> 16)) & HASH_BITS;
}
```

##### 1.2.2.2. 同步方式

- 对于读操作

  由于数组被 volatile 关键字修饰，因此不用担心数组的可见性问题。
  
  同时每个元素是一个 Node 实例（Java 7 中每个元素是一个 HashEntry），它的 Key 值和 hash 值都由 final 修饰，不可变更，无须关心它们被修改后的可见性问题，而其 Value 及对下一个元素的引用由 volatile 修饰，可见性也有保障。
  ```java
  static class Node<K,V> implements Map.Entry<K,V> {
      final int hash;
      final K key;
      volatile V val;
      volatile Node<K,V> next;
  }
  ```
  对于 Key 对应的数组元素的可见性，则由 Unsafe 的 getObjectVolatile 方法保证。
  ```java
  static final <K,V> Node<K,V> tabAt(Node<K,V>[] tab, int i) {
      return (Node<K,V>)U.getObjectVolatile(tab, ((long)i << ASHIFT) + ABASE);
  }
  ```

- 对于写操作

  如果 Key 对应的数组元素为 null，则通过 CAS 操作将其设置为当前值。如果 Key 对应的数组元素（也即链表表头或者树的根元素）不为 null，则对该元素使用 synchronized 关键字申请锁，然后进行操作。如果该 put 操作使得当前链表长度超过一定阈值，则将该链表转换为树，从而提高寻址效率。

### 1.3. ConcurrentHashMap 与 HashMap 比较

- ConcurrentHashMap 线程安全，而 HashMap 非线程安全。
- **HashMap 允许 Key 和 Value 为 null，而 ConcurrentHashMap 不允许**。
- HashMap 不允许通过 Iterator 遍历的同时通过 HashMap 修改，而 ConcurrentHashMap 允许该行为，并且该更新对后续的遍历可见。

## 2. 非阻塞 Queue: ConcurrentLinkedQueue

ArrayBlockingQueue 和 LinkedBlockingQueue 都是使用 lock 来实现的阻塞队列，而 ConcurrentLinkedQueue 基于 CAS 操作来实现，是非阻塞式的“lock-free”实现。

与其它队列一样，ConcurrentLinkedQueue 按照 FIFO（先进先出）原则对元素进行排序，队列的头部是队列中时间最长的元素，队列的尾部是队列中时间最短的元素。新的元素插入到队列的尾部，队列检索操作从队列头部获得元素。

ConcurrentLinkedQueue 源代码的实现有点复杂，具体的可看[这篇文章](http://www.infoq.com/cn/articles/ConcurrentLinkedQueue)的分析

## 3. 非阻塞 List: CopyOnWriteArrayList

CopyOnWriteArrayList 是一种即写时复制的容器。**所有可变操作（添加、设置等）都是通过对基础数组进行一次新的复制来实现的，往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行 Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器**。这样做的好处是可以对 CopyOnWrite 容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以 CopyOnWrite 容器也是一种读写分离的思想，读和写不同的容器。**这一般需要很大的开销，但是当遍历操作的数量大大超过可变操作的数量时，这种方法可能比其他替代方法更有效**。

在不能或不想进行同步遍历，但又需要从并发线程中排除冲突时，它也很有用。“快照”风格的迭代器方法在创建迭代器时使用了对数组状态的引用。此数组在迭代器的生存期内绝不会更改，因此不可能发生冲突，并且迭代器保证不会抛出 ConcurrentModificationException。

自创建迭代器以后，迭代器就不会反映列表的添加、移除或者更改。不支持迭代器上更改元素的操作（移除、设置和添加）。这些方法将抛出 UnsupportedOperationException。

## 4. 非阻塞 Set: CopyOnWriteArraySet

CopyOnWriteArraySet 是对其所有操作使用 CopyOnWriteArrayList 的 Set。因此，它共享以下相同的基本属性：
- 它**最适合于 set 大小通常保持很小、只读操作远多于可变操作以及需要在遍历期间防止线程间冲突的应用程序**。
- 它是线程安全的。
- **因为通常需要复制整个基础数组，所以可变操作（添加、设置、移除，等等）的开销巨大**。
- 迭代器不支持可变移除操作。
- 使用迭代器进行遍历的速度很快，并且不会与其他线程发生冲突。在构造迭代器时，迭代器依赖于不变的数组快照。 

## 5. Refer Links

[ConcurrentHashMap 实现原理及源码分析](https://www.cnblogs.com/chengxiao/p/6842045.html)

[Java 进阶（六）从 ConcurrentHashMap 的演进看 Java 多线程核心技术](http://www.jasongj.com/java/concurrenthashmap/)

[Map 综述（三）：彻头彻尾理解 ConcurrentHashMap](https://blog.csdn.net/justloveyou_/article/details/72783008)