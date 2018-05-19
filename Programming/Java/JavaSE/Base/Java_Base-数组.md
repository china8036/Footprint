- [Java 基础：数组](#java)
  - [1. 创建与初始化](#1)
  - [2. 多维数组](#2)
  - [3. 数组拷贝](#3)
  - [4. 数组填充](#4)
  - [5. 快速排序](#5)
  - [6. 二分查找](#6)

# Java 基础：数组

数组是一种用于存储同一类型的值集合的数据结构。

Java 中对数组的操作主要通过工具类 [Arrays](https://docs.oracle.com/javase/9/docs/api/java/util/Arrays.html) 完成。

## 1. 创建与初始化

数组的声明 / 创建：
```java
int [] a;
int a[];
```

数组的初始化：
```java
a = new int[100]; // 固定长度初始化
a = {2,3,4,56,7,78,57}; // 直接赋值初始化
a= new int[] {2,3,4,56,7,78,57}; // 使用匿名数组进行初始化
```

NOTE：
- 在 Java 中不要求数组长度是常量，`new int[n]`是完全合法的。
- 在 Java 中允许数组长度为 0。
- 初始化数组后，数值数值所有元素默认值为 0，boolean 数组所有元素默认值为 false，对象数组所有元素默认值为 null。
- 数组创建初始化后，便无法再改变长度。若需要在运行过程中扩展数组的大小，应使用数组列表 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html)。
- 数组的声明 / 创建可与初始化同时完成，如：`int [] a = new int[n];`。

## 2. 多维数组

```java
int [][] a = new int[X](Y);
int [][] b = new int[X][];// 先创建一维数组
int [][] b = new int[][Y];//error，必须从低维到高维创建
int [][] c = {// 创建不规则数组
  {1,2,3},
  {4,5},
  {6,7,8,9}
};
```
- NOTE: 不同于 C++ 中存在真正的多维数组，在 Java 中实际上没有真正的多维数组，只有一维数组。

  ```java
  //java
  int [][] a = new int[10](6);
  ```
  在 C++ 中相当于创建了一个包含 10 个指针的数组，然后再为每个数组元素填充一个包含 6 个数字的数组：
  ```cpp
  //cpp
  // 不等价
  int a[10](6);
  // 不等价
  int (*a)[6] = new int[10](6); 
  // 等价
  int **a = new int*[10];
  for (i = 0; i < 10; i++) {
    a[i] = new int[6];
  }
  ```

## 3. 数组拷贝

在 Java 中，数组的拷贝主要通过`Arrays.copyOf​(U [] original, int newLength)`完成，其中 original 为要复制的原数组，newLength 为要返回的数组副本的长度（若长度小于原数组长度，则只拷贝前部分的元素；若长度大小原数组的长度，则多出的部分赋默认值）。

`Arrays.copyOf`常用于数组扩容：
```java
int [] originalArray = {1,2,3,4,5};
int [] newArray = Arrays.copyOf(originalArray, 2 * originalArray.length);
```

NOTE：使用`System.arraycopy()`方法同样可完成数组的拷贝，两者的不同在于：
- `System.arraycopy()` 调用其它语言编写的底层函数，完成数组拷贝。
- `Arrays.copyOf` 实际上是对 `System.arraycopy()` 的二次封装，可以选择拷贝的起点和长度以及放入新数组中的位置。

## 4. 数组填充

通过`Arrays.fill​(type [] a, type val)`方法，可将数组的所有元素设置为 val 值。

## 5. 快速排序

通过`Arrays.sort​(type [] a, int fromIndex, int toIndex)`方法，可对数值型数组进行优化的快速排序，其中 type 可为 byte、char、double、float、int、long、short。

## 6. 二分查找

通过`Arrays.binarySearch​(type [] a, int fromIndex, int toIndex, type key)`方法，可对数组进行二分查找。若查找成功，返回相应下标值；否则，返回一个负数值 r（`-r-1` 是为了保持原数组 a 有序，key 值应插入的位置）。
