- [Java JCF Interview FAQ](#java-jcf-interview-faq)
	- [1. 关于 ArrayList](#1-关于-arraylist)
		- [1.1. ArrayList 的大小是如何自动增加的？](#11-arraylist-的大小是如何自动增加的)
		- [1.2. 如何复制某个 ArrayList 到另一个 ArrayList 中去？写出你的代码？](#12-如何复制某个-arraylist-到另一个-arraylist-中去写出你的代码)
		- [1.3. 说一下 ArrayList 在循环遍历中删除元素的陷阱？](#13-说一下-arraylist-在循环遍历中删除元素的陷阱)
		- [1.4. 手写 ArrayList 的简易实现](#14-手写-arraylist-的简易实现)
	- [2. 关于 LinkedList](#2-关于-linkedlist)
	- [3. 关于 ArrayList 与 LinkedList 的关系](#3-关于-arraylist-与-linkedlist-的关系)
		- [3.1. ArrayList 与 LinkedList 的异同？](#31-arraylist-与-linkedlist-的异同)
		- [3.2. 什么情况下你会使用 ArrayList？什么时候你会选择 LinkedList？](#32-什么情况下你会使用-arraylist什么时候你会选择-linkedlist)
		- [3.3. 如果不考虑任何性能问题，能否将所有 ArrayList 都替换为 LinkedList？](#33-如果不考虑任何性能问题能否将所有-arraylist-都替换为-linkedlist)
	- [4. 关于 Set](#4-关于-set)
		- [4.1. 为什么 JCF 中不提供 WeakHashSet 类？](#41-为什么-jcf-中不提供-weakhashset-类)
	- [5. 关于 HashMap](#5-关于-hashmap)
		- [5.1. 为什么 HashMap 继承了 AbstractMap 还要实现 Map 接口？](#51-为什么-hashmap-继承了-abstractmap-还要实现-map-接口)
		- [5.2. 关于 Hashmap 的 扩容机制](#52-关于-hashmap-的-扩容机制)
			- [5.2.1. Hashmap 什么时候需要增加容量呢？](#521-hashmap-什么时候需要增加容量呢)
			- [5.2.2. Hashmap 是怎么扩容的？](#522-hashmap-是怎么扩容的)
			- [5.2.3. initialCapacity 和 loadFactor 参数应该设为什么样的值？](#523-initialcapacity-和-loadfactor-参数应该设为什么样的值)
		- [5.3. 关于 HashMap 的非线程安全](#53-关于-hashmap-的非线程安全)
			- [5.3.1. 为什么 HashMap 是线程不安全的，实际会如何体现？](#531-为什么-hashmap-是线程不安全的实际会如何体现)
			- [5.3.2. 能否让 HashMap 实现线程安全，如何做？](#532-能否让-hashmap-实现线程安全如何做)
			- [5.3.3. Collections.synchronizedMap(hashMap) 是如何保证 HashMap 线程安全的？](#533-collectionssynchronizedmaphashmap-是如何保证-hashmap-线程安全的)
		- [5.4. 关于 HashMap 与 HashTable 的区别](#54-关于-hashmap-与-hashtable-的区别)
			- [5.4.1. HashMap 和 HashTable 的区别是什么？](#541-hashmap-和-hashtable-的区别是什么)
			- [5.4.2. 为什么 HashTable 的默认大小和 HashMap 不一样？](#542-为什么-hashtable-的默认大小和-hashmap-不一样)
		- [5.5. JDK Ⅷ 对 HashMap 有了什么改进？说说你对红黑树的理解？](#55-jdk-Ⅷ-对-hashmap-有了什么改进说说你对红黑树的理解)
	- [6. Refer Links](#6-refer-links)

# Java JCF Interview FAQ

## 1. 关于 ArrayList

### 1.1. ArrayList 的大小是如何自动增加的？

当试图在 arraylist 中增加一个对象的时候，Java 会去检查 Arraylist，以确保已存在的数组中有足够的容量来存储这个新的对象。如果没有足够容量的话，那么就会执行扩容逻辑：

新建一个长度为原来 1.5 倍的数组，旧的数组会使用 `Arrays.copyOf()` 方法复制到新的数组中去，再将现有的数组引用指向新的数组。

### 1.2. 如何复制某个 ArrayList 到另一个 ArrayList 中去？写出你的代码？

- 使用 Cloneable 接口的 clone() 方法，如 `ArrayList newArray = oldArray.clone();`。

- 使用 ArrayList 构造方法，如：`ArrayList myObject = new ArrayList(myTempObject);`。

- 使用 Collections 的 copy 方法。

NOTE: 方法 1 和 2 是浅拷贝 (shallow copy)，方法 3 才是深拷贝。

### 1.3. 说一下 ArrayList 在循环遍历中删除元素的陷阱？

ArrayList 在循环遍历中删除元素的陷阱分成两种情况：基于 get() 方法随机访问时的遍历 & 基于迭代器 Iterator 顺序访问时的遍历。

- 基于 get() 方法随机访问时的遍历
  
  - 问题描述
    ```java 
    List<String> a = new ArrayList<>();
    a.add("1"); a.add("1"); a.add("2"); a.add("3");
    for (int i = 0; i < a.size(); i++)
      if (a.get(i).equals("1"))
        a.remove(i);

    for (int i = 0; i < a.size(); i++)
      System.out.println(a.get(i));
    ```
    运行结果：
    ```
    1
    2
    3
    ```
    从运行结果上可以发现第二个元素“1”本应被删除却并没有被删除，这是为什么呢？

    通过查看 ArrayList 类中 remove() 方法的源码，我们可以知道执行 remove() 方法的过程中，会调用 fastRemove(index) 方法进行**元素的移动**。在上述例子中，**删除第一个“1”后，ArrayList 类中的数组整体向前移动了一个索引的位置，因此紧挨在被删除元素后边的元素（也就说第二个“1”）被移动到了被删除元素的位置，便不会再在此遍历循环中被访问到了**，也就导致了第二个元素“1”本应被删除却并没有被删除的结果。
  
  - 解决方法
    
    导致问题发生的原因可归纳为**remove() 方法中元素的移动**。为规避这样的问题，我们可以采取**倒序删除**的方式，即从数组的末尾开始向前遍历，这样即使发生元素删除也不会影响后序元素遍历：
    ```java 
    List<String> a = new ArrayList<>();
    a.add("1"); a.add("1"); a.add("2"); a.add("3");
    for (int i = a.size() - 1; i >= 0; i--)
      if (a.get(i).equals("1"))
        a.remove(i);

    for (int i = 0; i < a.size(); i++)
      System.out.println(a.get(i));
    ```
    运行结果：
    ```
    2
    3
    ```

- 基于迭代器 Iterator 顺序访问时的遍历
  - 问题描述
    ```java 
    List<String> a = new ArrayList<>();
    a.add("1"); a.add("1"); a.add("2"); a.add("3");
    for (String s : a) 
      if (s.equals("1")) 
        a.remove(s);

    for (int i = 0; i < a.size(); i++) 
      System.out.println(a.get(i));
    ```
    运行结果：抛出并发修改异常 `java.util.ConcurrentModificationException`，为什么？

    由于 Java 中的 foreach 循环是个语法糖，实际上编译成字节码后会被转成用迭代器的方式进行遍历，因此上述代码等同于：
    ```java
    List<String> a = new ArrayList<>();
    a.add("1"); a.add("1"); a.add("2"); a.add("3");
    Iterator<String> it = a.iterator();
    while (it.hasNext()) {
        String temp = it.next();
        if("1".equals(temp)){
            a.remove(temp);
        }
    }

    for (int i = 0; i < a.size(); i++) 
      System.out.println(a.get(i));
    ```
    查看 ArrayList 类中迭代器和 remove() 方法的源码：
    ```java
    private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            // 并发修改检测，检测不通过则抛出异常
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }
        
        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
        
        // 省略不相关的代码
    }
    ```
    从迭代器源码中我们可以知道，每次执行迭代器的 next() 方法时，都会先进行并发修改检测，**若检测到 ArrayList 对象中的 modCount 属性值与内部类 Itr 中 expectedModCount 属性值不相等，则判断为发生了并发修改，直接抛出 ConcurrentModificationException 异常**。

    在上述例子中，在完成第一个匹配的元素“1”的 remove() 操作时，会进行元素的移动操作，然后将 modCount 属性值自增，但此时 ArrayList 的内部类 Itr 中的 expectedModCount 属性值并未同步进行自增。因此，当进行下一次循环的 `it.next()` 时，就会检测到 ArrayList 对象中的 modCount 属性值与内部类 Itr 中 expectedModCount 属性值不相等，误判为发生了并发修改操作，并抛出并发修改异常 ConcurrentModificationException。

  - 特殊情景
    ```java 
    List<String> a = new ArrayList<>();
    a.add("1"); 
    a.add("2");
    for (String s : a) {
      if (s.equals("1") || s.equals("2")) {
        a.remove(s);
      }
    }

    for (int i = 0; i < a.size(); i++)
      System.out.println(a.get(i));
    ```
    运行结果：不会抛出任何异常，但会输出本应该被删除的 `2`，为什么一样的操作不会抛出并发修改异常？为什么会元素“2”没有被删除？

    同样查看 ArrayList 类中迭代器的源码可知，hasNext() 方法判定的标准是 `cursor != size`，而由于此集合中元素个数刚好为 2，当执行完第 1 次删除操作后，size 变为 1，刚好等于此时的 cursor，导致 hasNext() 方法返回 false，因此上边例子中的 foreach 循环实际上只进行了一次。
    - 由于执行 ArrayList 类的 remove() 方法后没有再执行迭代器的 next() 方法，因此不会抛出并发修改异常。
    - 由于没有进行第二次循环，因此元素“2”没有被删除。

  - 解决方法

    导致问题发生的原因可归纳为**在通过集合迭代器的 next() 方法进行访问时，调用了非迭代器的 remove() 方法**。为规避这样的问题，**在通过迭代器遍历访问集合时，若需要删除元素，应使用迭代器的 remove() 方法**，而不是 ArrayList 类的 remove() 方法。

    实际上，关于这个问题，在《阿里巴巴 Java 开发手册》里也有所规定：
    > 【强制】不要在 foreach 循环里进行元素的 remove/add 操作。remove 元素请使用 Iterator 方式，如果并发操作，需要对 Iterator 对象加锁。

### 1.4. 手写 ArrayList 的简易实现

TODO: 

实现要点：
- 扩容机制
- 添加、删除元素的实现（特别是元素移动操作）
- 访问、修改元素的实现
- 边界判断

示例：
```java
public class MyArrayList {  
  
    private Object[] elementData; // 底层数组  
    private int size; // 数组大小  
  
    // 获得数组大小  
    public int size() {  
        return size;  
    }  
  
    // 无参构造函数  
    public MyArrayList() {  
        this(10);  
    }  
  
    // 含参构造函数  
    public MyArrayList(int initialCapacity) {  
        if (initialCapacity < 0) {  
            try {  
                throw new Exception();  
            } catch (Exception e) {  
                e.printStackTrace();  
            }  
        }  
        elementData = new Object[initialCapacity];  
    }  
  
    // 判断是否为空  
    public boolean isEmpty() {  
        return size == 0;  
    }  
  
    // 根据下标获取对象  
    public Object get(int index) {  
        rangeCheck(index);  
        return elementData[index];  
    }  
  
    public boolean add(Object obj) {  
        ensureCapacity();  
        elementData[size] = obj;  
        size++;  
        return true;  
    }  
  
    // 插入操作  
    public void add(int index, Object obj) {  
        rangeCheck(index);  
        ensureCapacity();  
        System.arraycopy(elementData, index, elementData, index + 1, size - index);  
        elementData[index] = obj;  
        size++;  
    }  
  
    // 删除操作  
    public Object remove(int index) {  
        rangeCheck(index);  
        int arrnums = size - index - 1;  
        Object oldValue = elementData[index];  
        if (arrnums > 0) {  
            System.arraycopy(elementData, index, elementData, index, arrnums);  
        }  
        elementData[--size] = null;  
        return oldValue;  
    }  
  
    public boolean remove(Object obj) {  
        for (int i = 0; i < size; i++) {  
            if (get(i).equals(obj)) {  
                remove(i);  
            }  
            break;  
        }  
        return true;  
    }  
  
    public Object set(int index, Object obj) {  
        rangeCheck(index);  
        Object oldValue = elementData[index];  
        elementData[index] = obj;  
        return oldValue;  
    }  
  
    // 检查容量，必要时扩容处理  
    private void ensureCapacity() {  
        if (size == elementData.length) {  
            Object[] newArray = new Object[size + (size >> 1)];  
            System.arraycopy(elementData, 0, newArray, 0, size);  
            elementData = newArray;  
        }  
    }  
  
    // 检查下标是否合法  
    public void rangeCheck(int index) {  
        if (index < 0 || index >= size) {  
            try {  
                throw new Exception();  
            } catch (Exception e) {  
                e.printStackTrace();  
            }  
        }  
    }  
  
    public static void main(String[] args) {  
        MyArrayList mylist = new MyArrayList();  
        mylist.add("哈哈");  
        mylist.add("呵呵");  
        mylist.add("哦哦");  
        mylist.add("哈哈");  
        mylist.add(1, "手写代码");  
        String old = (String) mylist.remove(3); // 返回的是旧值  
        System.out.println(old);  
        System.out.println(mylist.remove("哦哦"));  
        System.out.println(mylist.isEmpty());  
        System.out.println(mylist.get(1));  
        System.out.println(mylist.size());  
        System.out.println(mylist.set(2, "你好啊")); // 返回旧值  
        System.out.println(mylist.get(2));  
    }  
  
}  
```

## 2. 关于 LinkedList

## 3. 关于 ArrayList 与 LinkedList 的关系

### 3.1. ArrayList 与 LinkedList 的异同？

- 相同
  - 都实现了 List 接口。
  - 都是非线程安全的。
  - 都允许元素为 null。

- 区别
  - ArrayList 是基于数组实现的线性表，增删效率低，改查效率高。LinkedList 是基于链表实现的线性表，增删效率高，改查效率低。

### 3.2. 什么情况下你会使用 ArrayList？什么时候你会选择 LinkedList？

多数情况下，当你遇到访问元素比插入 / 删除元素更加频繁的时候，你应该使用 ArrayList。另外一方面，当你在某个特别的索引中，插入或者是删除元素更加频繁，或者你压根就不需要访问元素的时候，你应该选择 LinkedList。

这里的主要原因是，在 ArrayList 中访问元素的最糟糕的时间复杂度是”1″，而在 LinkedList 中可能就是”n”了。在 ArrayList 中增加或者删除某个元素，通常会调用 `System.arraycopy` 方法，这是一种极为消耗资源的操作，因此，在频繁的插入或者是删除元素的情况下，LinkedList 的性能会更加好一点。

### 3.3. 如果不考虑任何性能问题，能否将所有 ArrayList 都替换为 LinkedList？

1. 可以。ArrayList 与 LinkedList 都实现了 List 接口，且 LinkedList 还额外实现了 Deque 接口，包含的方法比 ArrayList 中多得多，也基本覆盖了 ArrayList 中提供的 API 的功能。
1. 但不可在代码层面直接替换。因为 ArrayList 包含了 3 种构造方法（无参、指定容量、指定集合），而 LinkedList 只包含了 2 种构造方法（无参、指定集合），若直接替换肯定会出现问题。

## 4. 关于 Set

### 4.1. 为什么 JCF 中不提供 WeakHashSet 类？

https://stackoverflow.com/questions/4062919/why-does-exist-weakhashmap-but-absent-weakset

在 JCF 中，基本上大部分的 Map 接口的实现类都一个与之对应的 Set 实现类，其基于 Map 接口实现。但为什么 WeakHashMap 没有与之对应的 WeakHashSet 呢？

事实上，可以通过以下方法创建一个 WeakHashSet 实例：
```java
Set<Object> weakHashSet = Collections.newSetFromMap(
		new WeakHashMap<Object, Boolean>());
```

> Why there is no specific class for such stuff? 
> It's easy to imagine why the maintainers of java.util might have wanted to stop having to provide dual Map and Set versions of everything they do, and opted to just provide newSetFromMap() instead.

## 5. 关于 HashMap

**凡 Java 相关的面试，都会问及 Java 集合框架；凡问及 Java 集合框架，都会问及与哈希表相关的实现类**。以下总结与此相关的常见面试题。

### 5.1. 为什么 HashMap 继承了 AbstractMap 还要实现 Map 接口？

https://blog.csdn.net/u011392897/article/details/60141739

添加 Map 接口声明是为了 `Class` 类的 `getInterfaces()` 这个方法能够直接获取到 Map 接口。

### 5.2. 关于 Hashmap 的 扩容机制

#### 5.2.1. Hashmap 什么时候需要增加容量呢？

JDK 采用预处理法。

Hashmap 的构造器里指明了两个对于理解 HashMap 比较重要的两个参数 `int initialCapacity` 和 `float loadFactor`，当 `size > initialCapacity * loadFactor` 时，Hashmap 内部的 `resize()` 方法就被调用。

#### 5.2.2. Hashmap 是怎么扩容的？

HashMap 底层采用的散列数组实现，利用 initialCapacity 这个参数我们可以设置这个数组的大小，也就是散列桶的数量，但是如果需要 Map 的数据过多，在不断的 add 之后，这些桶可能都会被占满。

这种情况下一般有两种策略：
- 一种是不改变 Capacity，因为即使桶占满了，我们还是可以利用每个桶附带的链表增加元素。但是这有个缺点，此时 HaspMap 就退化成为了 LinkedList，使 get 和 put 方法的时间开销上升。
- 另一种方法是增加 Hash 桶的数量，这样 get 和 put 的时间开销又回退到近于常数复杂度上。**Hashmap 就是采用的该方法，也就是扩增散列数组的大小。**

```java
// 重新调整 HashMap 的大小，newCapacity 是调整后的单位
void resize(int newCapacity) {
		Entry[] oldTable = table;
		int oldCapacity = oldTable.length;
		if (oldCapacity == MAXIMUM_CAPACITY) {
				threshold = Integer.MAX_VALUE;
				return;
		}

		// 新建一个 HashMap，将“旧 HashMap”的全部元素添加到“新 HashMap”中，
		// 然后，将“新 HashMap”赋值给“旧 HashMap”。
		Entry[] newTable = new Entry[newCapacity];
		transfer(newTable);
		table = newTable;
		threshold = (int) (newCapacity * loadFactor);
}

// 将 HashMap 中的全部元素都添加到 newTable 中
void transfer(Entry[] newTable) {
		Entry[] src = table;
		int newCapacity = newTable.length;
		for (int j = 0; j < src.length; j++) {
				Entry<K, V> e = src[j];
				if (e != null) {
						src[j] = null;
						do {
								Entry<K, V> next = e.next;
								int i = indexFor(e.hash, newCapacity);
								e.next = newTable[i];
								newTable[i] = e;
								e = next;
						} while (e != null);
				}
		}
}
```
从源码可知，扩容时，会新建一个 HashMap 的底层数组，长度为原来的两倍，然后再调用 transfer() 方法，将旧 HashMap 的全部元素添加到新的 HashMap 中（要重新计算元素在新的数组中的索引位置）。因此，扩容是一个相当耗时的操作，我们在用 HashMap 时，最好能提前预估下 HashMap 中元素的个数，这样有助于提高 HashMap 的性能。

#### 5.2.3. initialCapacity 和 loadFactor 参数应该设为什么样的值？

initialCapacity 的默认值是 16。有些人可能会想如果内存足够，是不是可以将 initialCapacity 设大一些，即使用不了这么大，就可避免扩容导致的效率的下降，反正无论 initialCapacity 大小，我们使用的 get 和 put 方法都是常数复杂度的。这么说没什么不对，但是可能会忽略一点，实际的程序可能不仅仅使用 get 和 put 方法，也有可能使用迭代器，如果 initialCapacity 容量较大，那么会使迭代器效率降低。所以**理想的情况还是在使用 HashMap 前估计一下数据量**。

加载因子默认值是 0.75，这是 JDK**权衡时间和空间效率**之后得到的一个相对优良的数值。如果这个值过大，虽然空间利用率是高了，但是对于 HashMap 中的一些方法的效率就下降了，包括 get 和 put 方法，会导致每个 hash 桶所附加的链表增长，影响存取效率。如果比较小，除了导致空间利用率较低外没有什么坏处，只要有的是内存，毕竟现在大多数人把时间看的比空间重要。但是实际中还是很少有人会将这个值设置的低于 0.5。

### 5.3. 关于 HashMap 的非线程安全

#### 5.3.1. 为什么 HashMap 是线程不安全的，实际会如何体现？

具体细节上的原因，可以参考：[不正当使用 HashMap 导致 cpu 100% 的问题追究](http://ifeve.com/hashmap-infinite-loop/)

- 如果多个线程同时使用 put 方法添加元素

	假设正好存在两个 put 的 key 发生了碰撞 (hash 值一样)，那么根据 HashMap 的实现，这两个 key 会添加到数组的同一个位置，这样最终就会发生其中一个线程的 put 的数据被覆盖。

- 如果多个线程同时检测到元素个数超过数组大小*loadFactor

	这样会发生多个线程同时对 hash 数组进行扩容，都在重新计算元素位置以及复制数据，但是最终只有一个线程扩容后的数组会赋给 table，也就是说其他线程的都会丢失，并且各自线程 put 的数据也丢失。且会引起死循环的错误。

#### 5.3.2. 能否让 HashMap 实现线程安全，如何做？

- 使用线程安全的 Hashtable。
	
	但当一个线程访问 HashTable 的同步方法时，其他线程如果也要访问同步方法，会被阻塞住。举个例子，当一个线程使用 put 方法时，另一个线程不但不可以使用 put 方法，连 get 方法都不可以，效率很低，因此现在基本不会选择它了。

- 可以通过下面的语句进行 HashMap 的同步：
	```java
	hashmap = Collections.synchronizedMap(hashMap);
	```

- 若 JDK 版本在 5 之后，应直接使用 ConcurrentHashMap。

#### 5.3.3. Collections.synchronizedMap(hashMap) 是如何保证 HashMap 线程安全的？

```java
// synchronizedMap 方法
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m) {
		return new SynchronizedMap<>(m);
}
// SynchronizedMap 类
private static class SynchronizedMap<K,V>
		implements Map<K,V>, Serializable {
		private static final long serialVersionUID = 1978198479659022715L;

		private final Map<K,V> m;       // Backing Map
		final Object      mutex;        // Object on which to synchronize

		SynchronizedMap(Map<K,V> m) {
				this.m = Objects.requireNonNull(m);
				mutex = this;
		}

		SynchronizedMap(Map<K,V> m, Object mutex) {
				this.m = m;
				this.mutex = mutex;
		}

		public int size() {
				synchronized (mutex) {return m.size();}
		}
		public boolean isEmpty() {
				synchronized (mutex) {return m.isEmpty();}
		}
		public boolean containsKey(Object key) {
				synchronized (mutex) {return m.containsKey(key);}
		}
		public boolean containsValue(Object value) {
				synchronized (mutex) {return m.containsValue(value);}
		}
		public V get(Object key) {
				synchronized (mutex) {return m.get(key);}
		}

		public V put(K key, V value) {
				synchronized (mutex) {return m.put(key, value);}
		}
		public V remove(Object key) {
				synchronized (mutex) {return m.remove(key);}
		}
		// 省略其他方法
}
```
从源码中看出 synchronizedMap() 方法返回一个 SynchronizedMap 类的对象，而在 SynchronizedMap 类中使用了 synchronized 对 Map 的方法加层同步锁，保证对 Map 的操作是线程安全的，故效率其实也不高。

### 5.4. 关于 HashMap 与 HashTable 的区别

#### 5.4.1. HashMap 和 HashTable 的区别是什么？

Hashtable 和 HashMap 最主要的不同在于线程安全和速度。

- HashTable 是线程安全的，且不允许 key、value 是 null；HashMap 是非线程安全的，且允许 key、value 是 null。

- HashTable 默认容量是 11，HashMap 默认容量是 16，但默认加载因子都是 0.75。

- HashTable 是直接使用 key 的 `hashCode(key.hashCode())` 作为 hash 值，而 HashMap 内部使用 `static final int hash(Object key)` 扰动函数对 key 的 hashCode 进行扰动后才作为 hash 值。

- HashTable 取哈希桶下标是直接用模运算 %（因为其默认容量不要求是 2 的 n 次方，所以也无法用位运算替代模运算），而 HashMap 通过位运算代替模运算执行取下标运算，提高了效率（因此要求底层数组的容量一定要为 2 的整数次幂）。

- HashTable 扩容时，`int newCapacity = (oldCapacity << 1) + 1`，即新容量是原来的 2 倍加上 1；HashMap 扩容时，新容量是原来的 2 倍。

- Hashtable 是 Dictionary 的扩展类并实现了 Map 接口；HashMap 是 AbstractMap 的扩展类同样也实现了 Map 接口。

#### 5.4.2. 为什么 HashTable 的默认大小和 HashMap 不一样？

Hashtable 的扩容方法是乘 2 再加一，而不是简单的乘 2，故 hashtable 保证了容量永远是奇数，结合之前分析 hashmap 的重算 hash 值的逻辑，就明白了：

因为在数据分布在等差数据集合（如偶数) 上时，如果公差与桶容量有公约数 n，则至少有 (n-1)/n 数量的桶是利用不到的。因此 HashMap 会在取模（使用位与运算代替）哈希前先做一次哈希运算，调整 hash 值。而 Hashtable 比较古老，直接使用了除留余数法，那么就需要设置容量起码不是偶数（除（近似）质数求余的分散效果好），而 JDK 开发者选了 11。

### 5.5. JDK Ⅷ 对 HashMap 有了什么改进？说说你对红黑树的理解？

## 6. Refer Links