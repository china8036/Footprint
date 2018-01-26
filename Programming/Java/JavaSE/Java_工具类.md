- [Java 工具类](#java-%E5%B7%A5%E5%85%B7%E7%B1%BB)
  - [Math](#math)
  - [Date & SimpleDateFormat](#date-simpledateformat)
  - [包装器](#%E5%8C%85%E8%A3%85%E5%99%A8)
    - [特性](#%E7%89%B9%E6%80%A7)
    - [常用 API](#%E5%B8%B8%E7%94%A8-api)

# Java 工具类

Java 标准库中提供了一系列可供使用的工具类，便利了开发工作，以下列出常用的工具类：

## Math

在 [Math](https://docs.oracle.com/javase/9/docs/api/java/lang/Math.html) 类中，包含了各种各样的数学函数和数学常量值。

- `static double	sqrt​(double a)`：返回数值的平方根。

- `static double	pow​(double a, double b)`：幂运算。

- 三角函数：
  - `static double	sin​(double a)`

  - `static double	cos​(double a)`

  - `static double	tan(double a)`

- `static double	log​(double a)`：自然对数。

- `static double	log10​(double a)`：以 10 为底的对数。

- `static double	floor​(double a)`：四舍五入（向下取整）。

- `static double	ceil​(double a)`：四舍五入（向上取整）。

- `public static final double PI`：π 常量。

- `public static final double E`：e 常量。

例：
```java
public class Test {  
  public static void main (String []args) {  
    System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  
    System.out.println("0 度的余弦值：" + Math.cos(0));  
    System.out.println("60 度的正切值：" + Math.tan(Math.PI/3));  
    System.out.println("1 的反正切值： " + Math.atan(1));  
    System.out.println("π/2 的角度值：" + Math.toDegrees(Math.PI/2));  
    System.out.println(Math.PI);  
  }  
}
```

## Date & SimpleDateFormat

返回 unix 时间戳：TODO:

## 包装器

在 Java 中，所有的基本类型都有一个与之对应的包装器：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/26/9794f9e803ad3ca6231096ee89d16443.jpg)

其中，除了 Character 和 Boolean 类，其它 6 个类都派生与公共超类 Number。

将基本类型包装成对象以后，扩大了基本类型所具有的操作，也是 Java 面向对象的体现。

### 特性

- 不可变

  包装器是不可变的。包装器一旦使用基本数据类型构造后，就不允许更改包装在其中的值。并且包装器类是用 final 定义的，因此不能定义它们的子类。

  若想编写一个需要修改数值参数值的方法，可使用在 org.omg.CORBA 中定义的 holder 类型，包括 IntHolder、Double 等。每个 holder 类对象都包含一个共有（!）域值，通过它可以访问存储在其中的值。
  ```java
  public static void triple(IntHolder x) {
    x.value = 3 * x.value;
  }
  ```

- 自动装箱 & 自动拆箱

  - 自动装箱：
    ```java
    ArrayList<Integer> list = new ArrayList<>();
    list.add(3);// 传入基本数据类型
    //list.add(Integer.valueOf(3));// 将自动包装到包装器中
    ```
  - 自动拆箱：
    ```java
    int n = list.get(i);// 用包装器为基本数据类型赋值
    //int n = list.get(i).intValue();// 将自动拆包装成基本数据类型
    ```
    ```java
    Integer n = 3;
    n ++;// 自动拆包装，执行自增运算，然后再将结果自动装箱成包装器对象
    ```
  NOTE
  - 自动装箱和自动拆箱是由编译器完成的，而不是虚拟机。编译器再生成类的字节码时，插入必要的方法调用。虚拟机只是执行这些字节码。
  - 若包装器对象引用 null，自动装箱可能会抛出 NullPointerException 异常。
  - 若在一个条件表达式中混合使用 Integer 和 Double，Integer 值会自动拆箱，转换为 double 类型后，再自动装箱为 Double 对象。
    ```java
    Integer a = 1;
    Double b = 2.0;
    System.out.println(true?a:b);// 输出 1.0
    ```

### 常用 API

- 构造器

  除了 Character，所有包装器类型的构造器都接收 2 种参数：要包装的基本数据类型、表示数值的 String 类型。
  ```java
  Integer i1 = new Integer(42); 
  Integer i2 = new Integer("123");
  Character c1 = new Character('a');//Character 只有一个构造函数
  ```

- parseXxx()：将表示数值的 String 类型值转化为基本数据类型值，可指定转换使用的进制（默认十进制）。
  
  源码：
  ```java
   // 直接转换，获得 int 值
  public static int parseInt(String s) throws NumberFormatException {
    return parseInt(s,10);
  }
  ```
  实例：
  ```java
  int a = Integer.parseInt("1234");
  // 以 16 进制转换。
  int b = Integer.parseInt("3333",16);
  ```

- valueOf()：将表示数值的 String 类型值转化为基本数据类型的包装器，可指定转换使用的进制（默认十进制）。
  
  源码：
  ```java
  // 先调用 parseInt 获得 int 值，然后封装成 Integer 对象，注意封装的逻辑，有缓存
  public static Integer valueOf(String s) throws NumberFormatException {
    return Integer.valueOf(parseInt(s, 10));
  }
  public static Integer valueOf(int i) {
    assert IntegerCache.high >= 127;
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
  }
  ```
  实例：
  ```java
  Integer i1 = Integer.valueOf("123");
  Integer i2 = Integer.valueOf("11101", 2);// 以二进制转换
  ```
  **NOTE**：valueOf() 与 parseXxx() 的区别：
  - parseXxx 将表示数值的字符串转化为基本数据类型。
  - valueOf 将表示数值类型的字符串转化为基本数据类型对于的包装器，内部调用了 parseXxx。

- xxxValue()：将包装器对象转换为对应的基本数据类型值。
  ```java
  int a = i1.intValue();
  char b = c1.charValue();
  ```