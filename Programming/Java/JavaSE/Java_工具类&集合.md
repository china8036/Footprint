- [Java 常用工具类 & 集合](#java-%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7%E7%B1%BB-%E9%9B%86%E5%90%88)
  - [Math](#math)

# Java 常用工具类 & 集合

## Math

在 [Math](https://docs.oracle.com/javase/9/docs/api/java/lang/Math.html) 类中，包含了各种各样的数学函数和数学常量值。

- `double	sqrt​(double a)`：返回数值的平方根。

- `double	pow​(double a, double b)`：幂运算。

- 三角函数：
  - `double	sin​(double a)`

  - `double	cos​(double a)`

  - `double	tan(double a)`

- `double	log​(double a)`：自然对数。

- `double	log10​(double a)`：以 10 为底的对数。

- `double	floor​(double a)`：四舍五入（向下取整）。

- `double	ceil​(double a)`：四舍五入（向上取整）。

- `public static final double PI`：π 常量。

- `public static final double E`：e 常量。

例：
```java
public class Test {  
  public static void main (String []args) {  
    System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  
    System.out.println("0度的余弦值：" + Math.cos(0));  
    System.out.println("60度的正切值：" + Math.tan(Math.PI/3));  
    System.out.println("1的反正切值： " + Math.atan(1));  
    System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));  
    System.out.println(Math.PI);  
  }  
}
```