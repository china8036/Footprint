- [Java 零碎知识点整理](#java)
  - [1. 取余运算的实现以及奇偶判断的一个细节](#1)
  - [2. 实现一个 Swap 函数](#2--swap)

# Java 零碎知识点整理

## 1. 取余运算的实现以及奇偶判断的一个细节

```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 1) {      // 奇数
    /*
     * 处理。....
     */
  } else {        　　　  // 偶数
    /*
     * 处理。.....
     */
  }
}
```
这个 ftcs 是需要经过一系列的运算得到的结果，然后再做奇偶判断，为奇数做相应处理，否则做偶数处理。对于正数可正常执行，但对于负数，所有负数都会被判定为偶像。

通过查找资料发现，java 的取余的实现大致如下：
```java
/**
  * @desc 取余模拟算法
  * @param dividend 被除数
  * @param divisor 除数
  * @return
  * @return int
  */
public static int remainder(int dividend, int divisor) {
  return dividend - dividend / divisor * divisor;
}
// 如：10 % 3 == 1，运算过程：10 – 10 / 3 * 3 = 1（注意都是 int，没有小数部分）
```
当 ftcs = -11 时， -11 – (-11 / 2 * 2) = -1;（不等于 1）

当 ftcs = -10 时， -10 – (-10 / 2 * 2) = 0;

……

因此，修正如下：
```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 0) {       // 偶数
    /*
     * 处理。....
     */
  } else {        　　　   // 奇数
    /*
     * 处理。.....
     */
  }
}
```
因此：使用取余运算判断奇偶数时，推荐用偶数判断，不要用奇数判断。若用奇数判断，应对`1`加上绝对值。

## 2. 实现一个 Swap 函数

[怎么用 Java 实现一个 swap 函数？](https://www.zhihu.com/question/54443277)

在 Java 中无法像 C/C++ 语言一样使用指针或者引用实现只有 2 个参数的 swap 函数，这也是 Java 放弃指针带来的副作用：
```cpp
// cpp
void swap(int & a, int & b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

在 Java 中，以下 2 个 swap 函数都不会对实参有任何效果：
```java
public void swap(Object obj1, Object obj2) {
    Object tmp = obj1;
    obj1 = obj2;
    obj2 = tmp;
}

public void swap(int a, int b) {
    int tmp = a;
    a = b;
    b = tmp;
}
```

但在 Java 可以通过其它方式实现 swap 函数，如 Collections 工具类中：
```java
// 方式一：利用数组进行参数传递
private static void swap(Object[] arr, int i, int j) {
    Object tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

// 方式二：利用对象的 get/set 方法
@SuppressWarnings({"rawtypes", "unchecked"})
public static void swap(List<?> list, int i, int j) {
    // instead of using a raw type here, it's possible to capture
    // the wildcard but it will require a call to a supplementary
    // private method
    final List l = list;
    l.set(i, l.set(j, l.get(i)));
}
```

此外，对于 swap 逻辑的实现，一般有以下 3 种方法：
- 利用中间变量
  ```java
  public void swap(Object[] arr, int i, int j) {
      Object tmp = arr[i];
      arr[i] = arr[j];
      arr[j] = tmp;
  }
  ```

- 利用异或位运算
  ```java
  public void swap(Object[] arr, int i, int j) {
      arr[i] ^= arr[j];
      arr[j] ^= arr[i];
      arr[i] ^= arr[j];
  }
  ```

- 利用加减运算
  ```java
  public void swap(Object[] arr, int i, int j) {
      arr[i] = arr[i] + arr[j];
      arr[j] = arr[i] - arr[j];
      arr[i] = arr[i] - arr[j];
  }
  ```
