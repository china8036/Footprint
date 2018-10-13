- [Java 集合：Collection 族 - List](#java-集合collection-族---list)
	- [1. List 接口](#1-list-接口)
		- [1.1. 基本概念](#11-基本概念)
		- [1.2. 常用 API](#12-常用-api)
		- [1.3. 专用迭代器：ListIterator](#13-专用迭代器listiterator)
			- [1.3.1. 基本概念](#131-基本概念)
			- [1.3.2. 常用 API](#132-常用-api)
	- [2. ArrayList](#2-arraylist)
		- [2.1. 基本概念](#21-基本概念)
		- [2.2. 常用 API](#22-常用-api)
		- [2.3. 源码分析](#23-源码分析)
			- [2.3.1. 存储结构](#231-存储结构)
			- [2.3.2. 类定义](#232-类定义)
			- [2.3.3. 内部属性](#233-内部属性)
			- [2.3.4. 构造方法](#234-构造方法)
			- [2.3.5. 扩容机制](#235-扩容机制)
			- [2.3.6. 添加元素](#236-添加元素)
				- [2.3.6.1. 插入单个元素](#2361-插入单个元素)
				- [2.3.6.2. 批量插入元素](#2362-批量插入元素)
			- [2.3.7. 删除元素](#237-删除元素)
				- [2.3.7.1. 删除单个元素](#2371-删除单个元素)
				- [2.3.7.2. 批量删除元素](#2372-批量删除元素)
			- [2.3.8. 缩容机制](#238-缩容机制)
			- [2.3.9. 访问元素](#239-访问元素)
			- [2.3.10. 其它方法](#2310-其它方法)
	- [3. Vector](#3-vector)
	- [4. Stack](#4-stack)
	- [5. AbstractSequentialList 抽象类](#5-abstractsequentiallist-抽象类)
	- [6. LinkedList](#6-linkedlist)
		- [6.1. 基本概念](#61-基本概念)
		- [6.2. 常用 API](#62-常用-api)
		- [6.3. 源码分析](#63-源码分析)
			- [6.3.1. 类定义](#631-类定义)
			- [6.3.2. 内部属性](#632-内部属性)
			- [6.3.3. 构造方法](#633-构造方法)
			- [6.3.4. 添加元素](#634-添加元素)
				- [6.3.4.1. 添加单个元素](#6341-添加单个元素)
				- [6.3.4.2. 批量添加元素](#6342-批量添加元素)
			- [6.3.5. 删除元素](#635-删除元素)
			- [6.3.6. 访问元素](#636-访问元素)
			- [6.3.7. 集合遍历](#637-集合遍历)
			- [6.3.8. 修改元素](#638-修改元素)
	- [8. Refer Links](#8-refer-links)

# Java 集合：Collection 族 - List

![image](/resources/images/list-api-class-diagram.png)

## 1. List 接口

### 1.1. 基本概念

[List](https://docs.oracle.com/javase/9/docs/api/java/util/List.html) 接口是 Collection 接口的扩展，是一个元素有序、可重复的集合，集合中的每个元素都有其对应的顺序索引。List 集合默认按元素的添加顺序设置元素的索引，例如第一次添加的元素索引为 0，第二次添加的元素索引为 1 等，以此类推。

- 优点：
  - 元素按顺序排列。

- 缺点：
  - 搜索时需要遍历元素，相比哈希表，查找速度较慢。

### 1.2. 常用 API

List 接口作为 Collection 接口的扩展接口，在 Collecition 接口所有方法的基础上，由于 List 是有序集合，添加了一些根据索引来操作集合元素的方法：
- `void add(int index, E element)`: Inserts the specified element at the specified position in this list (optional operation).

- `boolean addAll(int index, Collection<? extends E> c)`: Inserts all of the elements in the specified collection into this list at the specified position (optional operation).

- `E get(int index)`: Returns the element at the specified position in this list.

- `int indexOf(Object o)`: Returns the index of the first occurrence of the specified element in this list, or -1 if this list does not contain the element.

- `int lastIndexOf(Object o)`: Returns the index of the last occurrence of the specified element in this list, or -1 if this list does not contain the element.

- `E remove(int index)`: Removes the element at the specified position in this list (optional operation).

- `E set(int index, E element)`: Replaces the element at the specified position in this list with the specified element (optional operation).

- `List<E> subList(int fromIndex, int toIndex)`: Returns a view of the portion of this list between the specified fromIndex, inclusive, and toIndex, exclusive.

- `default void replaceAll(UnaryOperator<E> operator)`:  (From JDK 8) Replaces each element of this list with the result of applying the operator to that element.
  
	eg:
	```java
	books.replaceAll(element -> ((String)element).length());
	```

- `default void sort(Comparator<? super E> c)`: (From JDK 8) Sorts this list according to the order induced by the specified Comparator.

  eg:
  ```java
  books.sort((o1, o2) -> ((String)o1).length() - ((String)o2).length());
  ```

NOTE:
- List 集合判断元素是否相等的标准是通过 equal() 方法进行比较，若返回 True 即判断为相等。

- 可以采用两种方式访问 List 元素：
  - 使用整数索引访问（随机访问）

    List 接口中提供了多个基于随机访问的 API：
    - `void add(int index, E element)`
    - `E remove(int index)`
    - `E get(int index)`
    - `E set(int index, E element)`
    - `boolean add(E e)`
    - `boolean remove(Object o)`

  - 使用迭代器访问（顺序访问）

    List 接口的顺序访问需要使用 ListIterator 接口。

P.S. 
- 《core Java 10th》
  > 集合框架的这个方面设计得很不好。实际中有两种有序集合，其性能有着很大差异。数组适合随机访问，但链表适合迭代器顺序访问。若能提供两个接口就会容易一些了。

- 可通过工具类 `RandomAccess` 来判断一个集合是否支持高效的随机访问：
  ```java
  if (c instanceof RandomAccess) {
    // 支持 
  } else {
    // 不支持
  }
  ```

### 1.3. 专用迭代器：ListIterator

#### 1.3.1. 基本概念

与 Set 集合只提供了 iterator() 方法返回一个 Iterator 迭代器不同，List 集合还额外提供了 listIterator() 方法，该方法返回一个 ListIterator 对象的迭代器。

[ListIterator](https://docs.oracle.com/javase/9/docs/api/java/util/ListIterator.html) 接口是 Iterator 的扩展接口，提供了专门操作 List 的方法。

TODO: **其实例对象是用于顺序访问的迭代器，主要用在链表结构中。**

#### 1.3.2. 常用 API

**ListIterator 接口在 Iterator 接口的基础上增加了向前迭代的功能（Iterator 接口只能向后迭代）和向集合中添加元素的功能（Iterator 接口只能删除元素）**。

P.S. 在 JCF 中，并非所有的集合都需要使用迭代器来插入中间位置的元素，比如无序的 Set 结构，其中元素完全无序。因此，Java 集合框架的设计者不在 Iterator 接口中定义 add 方法，而是在 Iterator 的子接口 ListIterator 中才定义了 add 方法，更具实际意义。

ListIterator 主要 API 如下：
- `void add​(E e)`：在指定位置（迭代器前）添加一个元素。

- `boolean hasPrevious​()`：返回该迭代器相关联的集合是否还有上一个元素。

- `boolean hasNext​()`: Returns true if this list iterator has more elements when traversing the list in the forward direction.

- `E previous​()`：返回该迭代器的上一个对象，即刚刚越过的对象。

  **调用 next 后再调用 remove 方法，会删除迭代器左侧的元素，但调用 previous 后再调用 remove 方法，会删除迭代器右侧的元素，并且同样不能连续调用两次 remove。**

- `E next​()`: Returns the next element in the list and advances the cursor position.

- `int previousIndex​()`：返回上一个元素的索引。

- `int nextIndex​()`：返回下一个元素的索引。

- `void set​(E e)`：Replaces the last element returned by next() or previous() with the specified element (optional operation).

- `void remove​()`: Removes from the list the last element that was returned by next() or previous() (optional operation).

## 2. ArrayList

### 2.1. 基本概念

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了动态数组 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是 List 接口的典型实现类，完全支持 List 接口的所有功能。ArrayLsit 基于数组实现，其封装了一个动态的、允许再分配的 Object [] 数组，它通过 initialCapacity 参数来设置该数组的长度，在添加或删除数组元素时，具有自动调节数组容量的功能，**initialCapacity 参数默认为 10**。

- 优点
  - 随机访问元素效率高
- 缺点
  - 在集合中间位置插入元素时需要移动该元素后的所有元素，效率低

### 2.2. 常用 API

- 构造器

  ArrayList 由三种构造方式：

  - `ArrayList​()`：构造一个空的数组列表。
  	```java
  	ArrayList<Employee> staff = new ArrayList<>(); 
  	```

  - `ArrayList​(int initialCapacity)`：构造一个具有初始容量的数组列表。
  	```java
  	ArrayList<Employee> staff = new ArrayList<>(100); 
  	```
  	NOTE：数组列表的容量与普通数组的大小是有区别的。
  	- 普通数组的大小，指的是数组初始化时已分配了 100 个元素的存储空间可供使用；
  	- 数组列表的容量，指的是该数组列表拥有保存 100 个元素的潜力（实际上还可以通过重新分配空间从而超过 100 个），但是在完成初始化时，数组列表根本不含有任何元素空间。

  - `ArrayList​(Collection<? extends E> c)`：以集合元素作为初始值构造一个数组列表。

  NOTE: 
  - **尖括号内不可使用基本数据类型**。
  - 一维 ArrayList 的初始化方法：
    - （常规方式）使用 add 方法
    	```java
    	ArrayList<T> obj = new ArrayList<>();
    	obj.add("o1");
    	obj.add("o2");
    	// ...
    	```
    - 使用 `Arrays.asList`
      ```java
      ArrayList<T> obj = new ArrayList<>(Arrays.asList(Object o1, Object o2, Object o3, ....));
      // 或者
      Integer [] arr = new Integer[10];
      List<Integer> list = new ArrayList<>(Arrays.asList(arr));
      ```
    - 使用 `Collections.ncopies`
    	```java
    	ArrayList<T> obj = new ArrayList<>(Collections.nCopies(count, element)); // 把 element 复制 count 次填入 ArrayList 中
    	```
    - 使用匿名内部类
    	```java
    	ArrayList<T> obj = new ArrayList<>() {{
    			add(Object o1);
    			add(Object o2);
    			// ...
    	}};
    	```
  - 多维 ArrayList 的初始化方法：
  	```java
  	List<List<Integer>> list = new ArrayList<List<Integer>>();
  	for (int i = 0; i < 3; i++)
  		list.add((List<Integer>)Arrays.asList(arr[i]));
  	```
  - ArrayList 数组的初始化方法：
  	```java
  	ArrayList<Integer> [] g = (ArrayList<Integer>[]) new ArrayList[n];
  	for (int i = 0; i < n; i ++)
  		g[i] = new ArrayList<>();
  	```

- 添加 & 删除元素

  - `void add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

  - `void add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

  - `E remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

  - `boolean remove(Object o)`：删除数组列表中值为 o 的元素。

  NOTE：

  **对数组进行插入或删除元素的操作效率都比较低**。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

- 访问元素

  数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 **Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的 `[]` 格式，而需要使用 get 和 set 方法进行访问**。

  - `E get(int index)`：获取数组列表指定位置的元素。

  - `E set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

- 转换为数组

  `Object[] toArray()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

- 容量控制

  - `int size()`：返回数组列表的元素个数。

  - `void ensureCapacity(int minCapacity)`：将 ArrayList 集合的 Object[] 数组长度增加大于或等于 minCapacity 的值。

     若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add() 方法，而不用重新分配空间。

  - `void trimToSize()`：调整 ArrayList 集合的 Object[] 数组长度为当前元素的个数，通过该方法可减少 ArrayList 集合对象占用的存储空间。

    当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储块，因此应该在确认不会再添加任何元素时，才调用该方法。

### 2.3. 源码分析

ArrayList 实现了 `List<E>`, `RandomAccess`, `Cloneable`, `java.io.Serializable` 接口，其中 `RandomAccess` 代表了其拥有随机快速访问的能力，ArrayList 可以以 O(1) 的时间复杂度去根据下标访问元素。

#### 2.3.1. 存储结构

ArrayList 的底层数据结构是数组，占据一块连续的内存空间，因此它也有数组的缺点：**空间效率不高**。

但由于数组的内存连续，可以根据下标以 O(1) 的时间复杂度读写（改查) 元素，因此**时间效率很高**。

#### 2.3.2. 类定义

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

#### 2.3.3. 内部属性

```java
// 默认数组容量
private static final int DEFAULT_CAPACITY = 10;

// 空数组
private static final Object[] EMPTY_ELEMENTDATA = {};

// 默认构造函数里的空数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

// 存储集合元素的底层实现：真正存放元素的数组
transient Object[] elementData; // non-private to simplify nested class access

// 当前集合中的元素数量
private int size;

/**
  * The maximum size of array to allocate.
  * Some VMs reserve some header words in an array.
  * Attempts to allocate larger arrays may result in
  * OutOfMemoryError: Requested array size exceeds VM limit
  */
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```

#### 2.3.4. 构造方法

```java
// 默认构造方法
public ArrayList() {
    // 将空数组赋值给 elementData
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

// 带初始容量的构造方法
public ArrayList(int initialCapacity) {
    // 如果初始容量大于 0，则新建一个长度为 initialCapacity 的 Object 数组。
    // 注意这里并没有修改 size（对比第三个构造函数)
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        // 如果容量为 0，直接将 EMPTY_ELEMENTDATA 赋值给 elementData
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        // 如果容量小于 0，直接抛出异常
        throw new IllegalArgumentException("Illegal Capacity: "+
                                            initialCapacity);
    }
}

// 利用别的集合类来构建 ArrayList 的构造函数
public ArrayList(Collection<? extends E> c) {
    // 直接利用 Collection.toArray() 方法得到一个对象数组，并赋值给 elementData 
    elementData = c.toArray();
    // 因为 size 代表的是集合元素数量，所以通过别的集合来构造 ArrayList 时，要给 size 赋值
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652) TODO:
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // 如果集合 c 元素数量为 0，则将空数组 EMPTY_ELEMENTDATA 赋值给 elementData 
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

其中：
- `Arrays.copyOf(elementData, size, Object[].class)` 方法是 Arrays 工具类中的静态方法，该方法将原数组拷贝到一个长度为 newLength 的新数组中，并返回该数组，适用于数组扩容或缩容：
  ```java
  public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
      @SuppressWarnings("unchecked")
      // 根据 class 的类型来决定是要通过 new 还是通过反射去构造一个泛型数组
      T[] copy = ((Object)newType == (Object)Object[].class)
          ? (T[]) new Object[newLength]
          : (T[]) Array.newInstance(newType.getComponentType(), newLength);
      // 高效地批量复制元素至新数组中
      System.arraycopy(original, 0, copy, 0,
                        Math.min(original.length, newLength));
      return copy;
  }
  ```
  参数说明：
  - original - 要复制的数组
  - newLength - 要返回的副本的长度
  - newType - 要返回的副本的类型

- `System.arraycopy()` 方法是一个 native 函数，该方法高效地批量复制元素至新数组中，由于是对内存直接进行复制，减少了 for 循环过程中的寻址时间，因此效率很高，没有返回值：
  ```java
  public static native void arraycopy(Object src,  int  srcPos,
                                      Object dest, int destPos,
                                      int length);
  ```
  参数说明：
  - src - 源数组
  - srcPos - 源数组中的起始位置
  - dest - 目标数组
  - destPos - 目标数据中的起始位置
  - length - 要复制的数组元素的数量

#### 2.3.5. 扩容机制

对于变长数据结构，当集合中的元素超出内部数组的容量，便会进行扩容操作。**在 ArrayList 中，当空间用完，其会按照原数组空间的 1.5 倍进行扩容**。扩容操作是 ArrayList 的一个核心操作，也是 ArrayList 的性能消耗比较大的地方。

```java
/** 计算最小容量 */
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

/** 扩容的入口方法 */
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

/** 扩容的核心方法 */
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    // newCapacity = oldCapacity + oldCapacity / 2 = oldCapacity * 1.5
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // 扩容
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    // 如果最小容量超过 MAX_ARRAY_SIZE，则将数组容量扩容至 Integer.MAX_VALUE
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

虽然 ArrayList 可以动态扩容，但扩容是一个性能消耗较大的操作。因此：
- 如果可以提前预知数据的规模，应该通过 `public ArrayList(int initialCapacity)` 构造方法在构建 ArrayList 实例时就指定数组的大小（即指定 initialCapacity 参数的大小），以避免扩容，提高效率。

- 如果需要一次添加大量的已知数量的元素，应该手动调用 `public void ensureCapacity(int minCapacity)` 方法一次性地分配数组大小，从而避免多次扩容操作。
  
  但由于该方法是 ArrayList 特有的 API，不是 List 接口里的，所以使用时需要使用类型强制转换：`((ArrayList)list).ensureCapacity(30);`。

#### 2.3.6. 添加元素

##### 2.3.6.1. 插入单个元素

对于数组（线性表）结构，插入操作分为两种情况：一种是在元素序列尾部插入，另一种是在元素序列的指定位置插入。ArrayList 的源码里也体现了这两种插入情况，如下：
```java
/** 在元素序列尾部插入 */
public boolean add(E e) {
    // 1. 检测是否需要扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    // 2. 将新元素插入序列尾部
    elementData[size++] = e;
    return true;
}

/** 在元素序列 index 位置处插入 */
public void add(int index, E element) {
    // 越界判断 如果越界直接抛出异常
    rangeCheckForAdd(index);

    // 1. 检测是否需要扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    // 2. 将 index 及其之后的所有元素都向后移一位
    System.arraycopy(elementData, index, elementData, index + 1,
                     size - index);
    // 3. 将新元素插入至 index 处
    elementData[index] = element;
    size++;
}
```
从源码可知：
- 对于在元素序列尾部插入，这种情况比较简单，只需两个步骤即可：
  1. 检测数组是否有足够的空间插入
  1. 将新元素插入至序列尾部

- 如果是在元素序列指定位置（假设该位置合理）插入，则情况稍微复杂一点，需要三个步骤：
  1. 检测数组是否有足够的空间
  1. 将 index 及其之后的所有元素向后移一位
  1. 将新元素插入至 index 处

容易发现，对于第二种指定位置的元素添加，需要先将该位置及其之后的元素都向后移动一位，为新元素腾出位置，这个操作的时间复杂度为 O(N)，频繁移动元素可能会导致效率问题，特别是集合中元素数量较多时。因此，**在日常开发中，若非所需，我们应当尽量避免在大集合中调用第二种插入方法**。

##### 2.3.6.2. 批量插入元素

除此之外，ArrayList 提供了批量添加元素的方法 addAll()，同样分为两种情况：在元素序列尾部插入，以及在元素序列的指定位置插入：
```java
public boolean addAll(Collection<? extends E> c) {
    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount // 确认是否需要扩容
    System.arraycopy(a, 0, elementData, size, numNew);// 复制数组完成复制
    size += numNew;
    return numNew != 0;
}

public boolean addAll(int index, Collection<? extends E> c) {
    rangeCheckForAdd(index);// 越界判断

    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount // 确认是否需要扩容

    int numMoved = size - index;
    if (numMoved > 0)
        System.arraycopy(elementData, index, elementData, index + numNew,
                         numMoved);// 移动（复制) 数组

    System.arraycopy(a, 0, elementData, index, numNew);// 复制数组完成批量赋值
    size += numNew;
    return numNew != 0;
}
```

#### 2.3.7. 删除元素

对于 ArrayList 的删除操作，除非从元素序列的尾部删除，否则无可避免的是元素的移动操作，因此效率较低。

##### 2.3.7.1. 删除单个元素

ArrayList 中提供了两种删除元素的方法类型：删除指定位置的元素和删除指定值的元素。

```java
/** 删除指定位置的元素 */
public E remove(int index) {
    // 判断是否越界
    rangeCheck(index);

    // 修改 modeCount，因为结构改变了
    modCount++;
    // 读出被删除的元素值，作为返回值
    E oldValue = elementData(index);

    int numMoved = size - index - 1;
    if (numMoved > 0)
        // 将 index + 1 及之后的元素向前移动一位，覆盖被删除值
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    // 将最后一个元素置空，并将 size 值减 1                
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}

/** 根据下标从数组取值，并进行类型的强制转换 */
E elementData(int index) {
    return (E) elementData[index];
}

/** 删除指定元素，若元素重复，则只删除下标最小的元素 */
public boolean remove(Object o) {
    if (o == null) {
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        // 遍历数组，查找要删除元素的位置
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

/** 快速删除操作，不做边界检查，也不返回删除的元素值，用于删除指定元素时内部调用 */
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work
}
```

##### 2.3.7.2. 批量删除元素

```java
// 批量删除
public boolean removeAll(Collection<?> c) {
    Objects.requireNonNull(c);// 判空
    return batchRemove(c, false);
}

// 批量移动
private boolean batchRemove(Collection<?> c, boolean complement) {
    final Object[] elementData = this.elementData;
    int r = 0, w = 0;//w 代表批量删除后 数组还剩多少元素
    boolean modified = false;
    try {
        // 高效的保存两个集合公有元素的算法
        for (; r < size; r++)
            if (c.contains(elementData[r]) == complement) // 如果 c 里不包含当前下标元素， 
                elementData[w++] = elementData[r];// 则保留
    } finally {
        // Preserve behavioral compatibility with AbstractCollection,
        // even if c.contains() throws.
        if (r != size) { // 出现异常会导致 r !=size , 则将出现异常处后面的数据全部复制覆盖到数组里。
            System.arraycopy(elementData, r,
                             elementData, w,
                             size - r);
            w += size - r;// 修改 w 数量
        }
        if (w != size) {// 置空数组后面的元素
            // clear to let GC do its work
            for (int i = w; i < size; i++)
                elementData[i] = null;
            modCount += size - w;// 修改 modCount
            size = w;// 修改 size
            modified = true;
        }
    }
    return modified;
}
```
从源码可知，当用来作为删除元素的集合里的元素多余被删除集合时，ArrayList 只会删除它们共同拥有的元素，因此可以用于保存两个集合中公有的元素。

#### 2.3.8. 缩容机制

考虑这样一种情况：往 ArrayList 插入大量元素后，又删除了很多元素，此时底层数组会空闲处大量的空间。因为 ArrayList 没有自动缩容机制，导致底层数组大量的空闲空间不能被释放，造成浪费。对于这种情况，ArrayList 也提供了相应的处理方法，如下：
```java
/** 将数组容量缩小至元素数量 */
public void trimToSize() {
    modCount++;
    if (size < elementData.length) {
        elementData = (size == 0)
          ? EMPTY_ELEMENTDATA
          : Arrays.copyOf(elementData, size);
    }
}
```
通过上面的方法，我们可以手动触发 ArrayList 的缩容机制。这样就可以释放多余的空间，提高空间利用率。

#### 2.3.9. 访问元素

由于 ArrayList 基于数组实现，支持高效的随机访问，因此 ArrayList 进行元素的访问和赋值都是非常高效的操作，时间效率为 O(1)。

- set()
  ```java
  public E set(int index, E element) {
      rangeCheck(index);// 越界检查
      E oldValue = elementData(index); // 取出元素 
      elementData[index] = element;// 覆盖元素
      return oldValue;// 返回元素
  }
  ```
  
- get()
  ```java
  public E get(int index) {
      rangeCheck(index);// 越界检查
      return elementData(index); // 下标取数据
  }

  E elementData(int index) {
      return (E) elementData[index];
  }
  ```
  ArrayList 基于数组实现，具有随机访问的能力，因此**在一些效率要求比较高的场景下，推荐通过 get() 方法来遍历 ArrayList**：
  ```java
  for (int i = 0; i < list.size(); i++) {
      list.get(i);
  }
  ```
  **若使用 foreach 循环遍历 ArrayList，foreach 最终会被编译转换成迭代器遍历的形式，也就是顺序访问形式，无法发挥数组的高效随机访问特性，因此不推荐**。

#### 2.3.10. 其它方法

- indexOf(Object o)
  ```java
  public int indexOf(Object o) {
      if (o == null) {
          for (int i = 0; i < size; i++)
              if (elementData[i]==null)
                  return i;
      } else {
          for (int i = 0; i < size; i++)
              if (o.equals(elementData[i]))
                  return i;
      }
      return -1;
  }
  ```
  从源码中可知，ArrayList 实现 indexOf() 方法时，从头遍历其内部数组，通过调用 equal() 方法来进行判断是否与目标值匹配。因此，若重写 equal() 方法，使该方法总返回 true，ArrayList.indexOf() 将始终返回第一个元素。

- lastIndexOf(Object o)
  ```java
  public int lastIndexOf(Object o) {
      if (o == null) {
          for (int i = size-1; i >= 0; i--)
              if (elementData[i]==null)
                  return i;
      } else {
          for (int i = size-1; i >= 0; i--)
              if (o.equals(elementData[i]))
                  return i;
      }
      return -1;
  }
  ```
  ArrayList.lastIndexOf() 方法的实现原理与 ArrayList.indexOf() 类似，不同之处在于 lastIndexOf() 方法从数组尾部开始向前遍历。

- toArray()
  ```java
  public Object[] toArray() {
      return Arrays.copyOf(elementData, size);
  }

  @SuppressWarnings("unchecked")
  public <T> T[] toArray(T[] a) {
      if (a.length < size)
          // Make a new array of a's runtime type, but my contents:
          return (T[]) Arrays.copyOf(elementData, size, a.getClass());
      System.arraycopy(elementData, 0, a, 0, size);
      if (a.length > size)
          a[size] = null;
      return a;
  }
  ```

- clear()
  ```java
  public void clear() {
      modCount++;// 修改 modCount
      // clear to let GC do its work
      for (int i = 0; i < size; i++)  // 将所有元素置 null
          elementData[i] = null;

      size = 0; // 修改 size 
  }
  ```

## 3. Vector

Vector 和 ArrayList 一样是 List 接口基于数组的的典型实现，完全支持 List 接口的全部功能。

Vector 的实现和用法大部分与 ArrayList 相同，但由于 Vector 是一个古老的集合类（从 JKD1.0 开始就有了，JDK1.2 出现了 JCF 后将 Vector 改为实现 List 接口），从而导致 Vector 中包含一些功能重复、命名冗长的方法。

Vector 与 ArrayList 的显著区别在于：**ArrayList 是非线程安全的，而 Vector 是线程安全的。这也是 Vector 的性能要弱于 ArrayList 的原因之一**。

实际上，Vector 具有很多缺点，因此通常在不要求线程安全的情况下，都推荐使用 ArrayList 而不是 Vector。若要求保证线程安全，同样不推荐使用 Vector 作为 List 实现类，可以使用 Collections 工具类提供的方法将 ArrayList 变成线程安全的。

## 4. Stack

Stack 类是 Vector 类的扩展类，因此同样也是一个很古老的集合实现类。Stack 用于实现栈的数据结构，是一种 LIFO 的容器。

**与 Vector 相同的是，它同样是线程安全、性能较差的，因此通常也不推荐使用 Stack 类作为栈的实现，可以使用 ArrayDeque 类来实现栈的结构**。

## 5. AbstractSequentialList 抽象类

从实现上，AbstractSequentialList 提供了一套**基于顺序访问的接口**。通过继承此类，子类仅需实现部分代码即可拥有完整的一套访问某种序列表（比如链表）的接口。

通过查看源码，可以发现：**AbstractSequentialList 提供的方法基本上都是通过 ListIterator 实现的**。
```java
public E get(int index) {
    try {
        return listIterator(index).next();
    } catch (NoSuchElementException exc) {
        throw new IndexOutOfBoundsException("Index: "+index);
    }
}

public void add(int index, E element) {
    try {
        listIterator(index).add(element);
    } catch (NoSuchElementException exc) {
        throw new IndexOutOfBoundsException("Index: "+index);
    }
}

// 留给子类实现
public abstract ListIterator<E> listIterator(int index);
```

## 6. LinkedList

### 6.1. 基本概念

[LinkedList](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedList.html) 与 ArrayList 一样是 List 接口的典型实现类，但不同的是，ArrayList 基于数组实现，而 LinkedList 基于链表实现，而且**是一种非线程安全的、允许元素为 null 的双向链表**。

除此之外，LinkedList 还实现了双端队列 Deque 接口，因此 **LinkedList 还可以作为栈和队列使用**。

- 优点
  - 便于在集合中间位置插入元素
- 缺点
  - 访问每个元素只能从头搜索，随机访问效率低

### 6.2. 常用 API

- 构造器
	- `LinkedList​()`：构造一个空链表。
	
	- `LinkedList​(Collection<? extends E> c)`：构造一个链表，并将所给集合中的所有元素添加到这个链表中。

- 添加元素
	- `add​(E e)`：将对象添加到链表的尾部
		
		LinkedList 提供了	`add​(E e)` 方法将对象添加到链表的尾部，但对于链表中间位置的元素添加，需要使用迭代器 ListIterator 中的 add 方法。

	- `ListIterator<E> listIterator​(int index)`：返回一个 ListIterator 迭代器。

- 访问元素

	LinkedList 提供了一个用于访问元素的 `get(int index)` 方法，但该方法的实现效率很低。

	**虽然 get 方法做了优化（索引大于 size/2 时，从链表尾部开始搜索元素），但是，若要访问第 n 个元素，就需要从链表头部开始，越过 n-1 个元素，没有任何捷径可走，且 LinkedList 对象访问元素时不做任何缓存位置信息的操作**。因此，若必须需要使用该方法访问元素，应考虑是否更换所用的数据结构。

	《core java 10th》
	> 我们建议避免使用以整数索引表示链表中位置的所有方法。若需要对集合进行随机访问，就使用数组或 ArrayList，不要使用链表。

### 6.3. 源码分析

LinkedList 的底层数据结构是链表，因此可想而知，它的增删只需要移动指针即可，故时间效率较高。由于不需要批量扩容，也不需要预留空间，所以空间效率比 ArrayList 高。但缺点就是随机访问时，需要进行链表的遍历，尽管 LinkedList 在实现时做了优化，但效率仍然较低。

#### 6.3.1. 类定义

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```

#### 6.3.2. 内部属性

```java
// 集合元素数量
transient int size = 0;
// 链表头节点
transient Node<E> first;
// 链表尾节点
transient Node<E> last;

// 双向链表的节点 Node 结构
private static class Node<E> {
    E item;// 元素值
    Node<E> next;// 后置节点
    Node<E> prev;// 前置节点

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

#### 6.3.3. 构造方法

```java
public LinkedList() {
}

public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```

#### 6.3.4. 添加元素

LinkedList 插入元素的过程实际上就是链表链入节点的过程，但由于 LinkedList 除了实现了 List 接口相关方法，还实现了 Deque 接口的很多方法，因此有很多种方式可用于插入元素。以下只分析实现自 List 接口的插入方法：

##### 6.3.4.1. 添加单个元素

```java
/** 在链表尾部插入元素 */
public boolean add(E e) {
    linkLast(e);
    return true;
}

/** 在链表指定位置插入元素 */
public void add(int index, E element) {
    checkPositionIndex(index);

    // 判断 index 是不是链表尾部位置，如果是，直接将元素节点插入链表尾部即可
    if (index == size)
        linkLast(element);
    else
        linkBefore(element, node(index));
}

/** 将元素节点插入到链表尾部 */
void linkLast(E e) {
    final Node<E> l = last;
    // 创建节点，并指定节点前驱为链表尾节点 last，后继引用为空
    final Node<E> newNode = new Node<>(l, e, null);
    // 将 last 引用指向新节点
    last = newNode;
    // 判断尾节点是否为空，为空表示当前链表还没有节点
    if (l == null)
        first = newNode;
    else
        // 让原尾节点后继引用 next 指向新的尾节点
        l.next = newNode;    
    size++;
    modCount++;
}

/** 将元素节点插入到 succ 之前的位置 */
void linkBefore(E e, Node<E> succ) {
    // assert succ != null;
    final Node<E> pred = succ.prev;
    // 1. 初始化节点，并指明前驱和后继节点
    final Node<E> newNode = new Node<>(pred, e, succ);
    // 2. 将 succ 节点前驱引用 prev 指向新节点
    succ.prev = newNode;
    // 判断尾节点是否为空，为空表示当前链表还没有节点    
    if (pred == null)
        first = newNode;
    else
        // 3. succ 节点前驱的后继引用指向新节点
        pred.next = newNode;   
    size++;
    modCount++;
}
```

##### 6.3.4.2. 批量添加元素

```java
// 在尾部批量增加
public boolean addAll(Collection<? extends E> c) {
    return addAll(size, c);// 以 size 为插入下标，插入集合 c 中所有元素
}

// 以 index 为插入下标，插入集合 c 中所有元素
public boolean addAll(int index, Collection<? extends E> c) {
    checkPositionIndex(index);// 检查越界 [0,size] 闭区间

    Object[] a = c.toArray();// 拿到目标集合数组
    int numNew = a.length;// 新增元素的数量
    if (numNew == 0)// 如果新增元素数量为 0，则不增加，并返回 false
        return false;

    Node<E> pred, succ;  //index 节点的前置节点，后置节点
    if (index == size) { // 在链表尾部追加数据
        succ = null;  //size 节点（队尾）的后置节点一定是 null
        pred = last;// 前置节点是队尾
    } else {
        succ = node(index);// 取出 index 节点，作为后置节点
        pred = succ.prev; // 前置节点是，index 节点的前一个节点
    }
    // 链表批量增加，是靠 for 循环遍历原数组，依次执行插入节点操作。对比 ArrayList 是通过 System.arraycopy 完成批量增加的
    for (Object o : a) {// 遍历要添加的节点。
        @SuppressWarnings("unchecked") E e = (E) o;
        Node<E> newNode = new Node<>(pred, e, null);// 以前置节点 和 元素值 e，构建 new 一个新节点，
        if (pred == null) // 如果前置节点是空，说明是头结点
            first = newNode;
        else// 否则 前置节点的后置节点设置问新节点
            pred.next = newNode;
        pred = newNode;// 步进，当前的节点为前置节点了，为下次添加节点做准备
    }

    if (succ == null) {// 循环结束后，判断，如果后置节点是 null。 说明此时是在队尾 append 的。
        last = pred; // 则设置尾节点
    } else {
        pred.next = succ; // 否则是在队中插入的节点 ，更新前置节点 后置节点
        succ.prev = pred; // 更新后置节点的前置节点
    }

    size += numNew;  // 修改数量 size
    modCount++;  // 修改 modCount
    return true;
}

private void checkPositionIndex(int index) {
    if (!isPositionIndex(index))
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

private boolean isPositionIndex(int index) {
    return index >= 0 && index <= size;  // 插入时的检查，下标可以是 size [0,size]
}
```

**链表的批量增加，是通过 for 循环遍历原数组，依次执行插入节点操作来实现的。对比 ArrayList，后者是通过 System.arraycopy 完成批量增加的**。

#### 6.3.5. 删除元素

```java
/** 删除指定元素值的元素 */
public boolean remove(Object o) {
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        // 遍历链表，找到要删除的节点
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);    // 将节点从链表中移除
                return true;
            }
        }
    }
    return false;
}

/** 删除指定位置的元素，并返回被删除元素 */
public E remove(int index) {
    checkElementIndex(index);
    // 通过 node 方法定位节点，并调用 unlink 将节点从链表中移除
    return unlink(node(index));
}

/** 将指定节点从链表中移除 */
E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;
    
    // prev 为空，表明删除的是头节点
    if (prev == null) {
        first = next;
    } else {
        // 将 x 的前驱的后继指向 x 的后继
        prev.next = next;
        // 将 x 的前驱引用置空，断开与前驱的链接
        x.prev = null;
    }

    // next 为空，表明删除的是尾节点
    if (next == null) {
        last = prev;
    } else {
        // 将 x 的后继的前驱指向 x 的前驱
        next.prev = prev;
        // 将 x 的后继引用置空，断开与后继的链接
        x.next = null;
    }

    // 将 item 置空，等待 GC 回收
    x.item = null;
    size--;
    modCount++;
    return element;
}
```

#### 6.3.6. 访问元素

LinkedList 底层基于链表结构，无法向 ArrayList 那样随机访问指定位置的元素。LinkedList 的查找实现需要从链表头结点（或尾节点）向后查找，时间复杂度为 O(N)。

```java
public E get(int index) {
    // 检查是否越界
    checkElementIndex(index);
    return node(index).item;
}

// Returns the (non-null) Node at the specified element index.
Node<E> node(int index) {
    /**
     * 查找位置 index 如果小于节点数量的一半，则从头节点开始查找；否则从尾节点查找
     */
    if (index < (size >> 1)) {
        Node<E> x = first;
        // 循环向后查找，直至 i == index
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

#### 6.3.7. 集合遍历

**LinkedList 可以通过两种方式进行遍历：通过迭代器进行遍历与通过 get() 方法进行遍历**。

- 通过迭代器进行遍历（foreach 循环在编译后最终会转换成迭代器形式）
  ```java
  List<Integet> list = new LinkedList<>();
  list.add(1)
  list.add(2)
  for (Integer i : list) {
    System.out.println(i);
  }
  ```
  通过查看 LinkedList 的迭代器源码可知，以上遍历方式的**时间效率为 O(N)**：
  ```java
  public ListIterator<E> listIterator(int index) {
      checkPositionIndex(index);
      return new ListItr(index);
  }

  private class ListItr implements ListIterator<E> {
      private Node<E> lastReturned;
      private Node<E> next;
      private int nextIndex;
      private int expectedModCount = modCount;

      /** 构造方法将 next 引用指向指定位置的节点 */
      ListItr(int index) {
          // assert isPositionIndex(index);
          next = (index == size) ? null : node(index);
          nextIndex = index;
      }

      public boolean hasNext() {
          return nextIndex < size;
      }

      public E next() {
          checkForComodification();
          if (!hasNext())
              throw new NoSuchElementException();

          lastReturned = next;
          next = next.next;    // 调用 next 方法后，next 引用都会指向他的后继节点
          nextIndex++;
          return lastReturned.item;
      }
      
      // 省略部分方法
  }
  ```
  
- 通过 get() 方法进行遍历
  ```java
  List<Integet> list = new LinkedList<>();
  list.add(1)
  list.add(2)
  for (int i = 0; i < list.size(); i++) {
      System.out.println(list.get(i));
  }
  ```
  通过查看 get() 方法的源码可知，通过上面的方式每获取一个元素，LinkedList 都需要从头节点（或尾节点）进行遍历，时间效率将会达到 O(N^2)，在大数据量情况下，效率极差。因此，应该尽量避免这种遍历方法。

#### 6.3.8. 修改元素

```java
public E set(int index, E element) {
    checkElementIndex(index); // 检查越界 [0,size)
    Node<E> x = node(index);// 取出对应的 Node
    E oldVal = x.item;// 保存旧值 供返回
    x.item = element;// 用新值覆盖旧值
    return oldVal;// 返回旧值
}
```

## 8. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)

[ArrayList 源码分析](http://www.coolblog.xyz/2018/01/28/ArrayList%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/)

[ArrayList 初始化的 4 种方法](https://blog.csdn.net/u011523796/article/details/79537055)

[面试必备：ArrayList 源码解析（JDK8）](http://blog.csdn.net/zxt0601/article/details/77281231)

[Java 中 System.arraycopy() 和 Arrays.copyOf() 的区别](https://www.jianshu.com/p/840976f14950)

[面试题：Java 中 ArrayList 循环遍历并删除元素的陷阱](http://blog.csdn.net/L_kanglin/article/details/70148043)

[面试必备：LinkedList 源码解析（JDK8）](http://blog.csdn.net/zxt0601/article/details/77341098)

[LinkedList 源码分析 (JDK 1.8)](http://www.coolblog.xyz/2018/01/31/LinkedList-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90-JDK-1-8/)