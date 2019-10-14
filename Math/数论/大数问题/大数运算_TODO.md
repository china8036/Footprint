
https://blog.csdn.net/lisp1995/article/details/52403507

https://blog.csdn.net/lisp1995/article/details/52316466

https://blog.csdn.net/lisp1995/article/details/52319348

https://blog.csdn.net/lisp1995/article/details/52347655

...

https://blog.csdn.net/u010983881/article/details/77503519

## 1. N!（阶乘）的末尾有多少个零？

[n! 阶乘末尾有多少个零](https://blog.csdn.net/TommyZht/article/details/46309563)

假如你把 1 × 2 ×3× 4 ×……×N 中每一个因数分解质因数，结果就像：

1 × 2 × 3 × (2 × 2) × 5 × (2 × 3) × 7 × (2 × 2 ×2) ×……

10 可以分解为 2 × 5——因此只有质数 2 和 5 相乘能产生 0，别的任何两个质数相乘都不能产生 0，而且 2，5 相乘只产生一个 0。所以，分解后的整个因数式中有多少对 (2, 5)，结果中就有多少个 0，而分解的结果中，2 的个数显然是多于 5 的，因此，有多少个 5，就有多少个 (2, 5) 对。

该问题的实质是要求出 1~N 含有多少个 5，由特殊推广到一般的论证过程可得：
1、 每隔 5 个，会产生一个 0，比如 5， 10 ，15，20.。
2、 每隔 5×5 个会多产生出一个 0，比如 25，50，75，100
3、 每隔 5×5×5 会多出一个 0，比如 125.

因此，N！的末尾有多少个零？

A: N/5^1 + N/5^2 + N/5^3 + ... +N/5^r (N <= 5^r)

e.g. 计算 2009! 的末尾有多少个 0:

2009! 的末尾有 401 + 80 + 16 + 3 = 500 个 0。

代码实现：
```java
int CountZero(int N) {
    int ret = 0;
    while (N) {
        ret += N / 5;
        N /= 5;
    }
    return ret;
}
```