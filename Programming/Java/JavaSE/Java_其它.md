- [Java 零碎知识点整理](#java-%E9%9B%B6%E7%A2%8E%E7%9F%A5%E8%AF%86%E7%82%B9%E6%95%B4%E7%90%86)
  - [取余运算的实现以及奇偶判断的一个细节](#%E5%8F%96%E4%BD%99%E8%BF%90%E7%AE%97%E7%9A%84%E5%AE%9E%E7%8E%B0%E4%BB%A5%E5%8F%8A%E5%A5%87%E5%81%B6%E5%88%A4%E6%96%AD%E7%9A%84%E4%B8%80%E4%B8%AA%E7%BB%86%E8%8A%82)

# Java 零碎知识点整理

## 取余运算的实现以及奇偶判断的一个细节

```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 1) {      //奇数
    /*
     * 处理.....
     */
  } else {        　　　  //偶数
    /*
     * 处理......
     */
  }
}
```
这个ftcs是需要经过一系列的运算得到的结果，然后再做奇偶判断，为奇数做相应处理，否则做偶数处理。对于正数可正常执行，但对于负数，所有负数都会被判定为偶像。

通过查找资料发现，java的取余的实现大致如下：
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
//如：10 % 3 == 1，运算过程：10 – 10 / 3 * 3 = 1（注意都是int，没有小数部分）
```
当ftcs = -11时， -11 – (-11 / 2 * 2) = -1;（不等于1）

当ftcs = -10时， -10 – (-10 / 2 * 2) = 0;

……

因此，修正如下：
```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 0) {       //偶数
    /*
     * 处理.....
     */
  } else {        　　　   //奇数
    /*
     * 处理......
     */
  }
}
```
因此：使用取余运算判断奇偶数时，推荐用偶数判断，不要用奇数判断。若用奇数判断，应对`1`加上绝对值。

