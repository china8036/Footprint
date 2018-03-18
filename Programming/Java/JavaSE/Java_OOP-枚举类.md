- [Java 枚举类](#java-%E6%9E%9A%E4%B8%BE%E7%B1%BB)
    - [1. 基本用法](#1-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
    - [2. 常用方法](#2-%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
        - [2.1. values()](#21-values)
        - [2.2. switch()](#22-switch)
    - [3. 自定义属性](#3-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
    - [4. 自定义方法](#4-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%96%B9%E6%B3%95)
    - [5. enum 与静态常量的比较](#5-enum-%E4%B8%8E%E9%9D%99%E6%80%81%E5%B8%B8%E9%87%8F%E7%9A%84%E6%AF%94%E8%BE%83)
    - [6. Refer Links](#6-refer-links)

# Java 枚举类

java 枚举类是 Java 5 新增的类型，存放在 java.lang 包中。

枚举类是一组预定义常量的集合，全部都以类型安全的形式来表示，使用 enum 关键字声明这个类，常量名称官方建议大写。

枚举类的名称一般以 Enum 结尾，比如 ColorEnum 等。如果你写个枚举类，取名为 Color，那么没人能快速知道它是一个枚举类。

## 1. 基本用法

创建枚举类型要使用 enum 关键字，隐含了所创建的类型都是 java.lang.Enum 类的子类（java.lang.Enum 是一个抽象类）。枚举类型符合通用模式 Class Enum<E extends Enum<E>>，而 E 表示枚举类型的名称。枚举类型的每一个值都将映射到 protected Enum(String name, int ordinal) 构造函数中。在这里每个值的名称都被转换成一个字符串，并且序数设置表示了此设置被创建的顺序。

格式：
```
enum 类名 {

  // 枚举值 1, 枚举值 2, ...

}
```

例：
```java
public enum Day {
    SUNDAY, 
    MONDAY, 
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY 
}

public class EnumTest {
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("周一各种不在状态");
                break;

            case FRIDAY:
                System.out.println("周五感觉还不错");
                break;

            case SATURDAY: case SUNDAY:
                System.out.println("周末给人的感觉是最棒的");
                break;

            default:
                System.out.println("周内感觉就那样吧。");
                break;
        }
    }
}
```

NOTE:

- 需要的数据不能是任意的，而必须是一定范围内的值
- 枚举类也是一个特殊的类，构造方法默认的修饰符是 private 的
- 枚举类不能使用extends关键字继承其他类，因为enum已经继承了java.lang.Enum（java是单一继承）
- 枚举值默认的修饰符是 public static final，必须要位于枚举类的第一个语句
- 枚举类可以定义自己的成员变量、成员函数和带参构造方法，自定义带参构造方法时，枚举值需要传参
- 枚举类可以存在抽象的方法，但是枚举值必须要实现抽象的方法
- 枚举最大的缺点是：相对于普通的常量会占用更多的内存。所以，我还是不建议大面积的使用枚举来替代整形常量。但是如果这些常量还有关联属性或者行为等，那么强烈推荐使用枚举类型。使用枚举类型的性能几乎是使用静态类的16倍。
- 枚举类型对象之间的值比较，可以使用==直接来比较值是否相等的，不是必须使用equals方法。
- 推荐使用枚举单例来实现单例模式，可以使用枚举策略来简化策略模式。

## 2. 常用方法

- int ordinal()：返回枚举常量的序数（它在枚举声明中的位置，其中初始常量序数为零）。

- String name()：返回此枚举常量的名称。

- String toString()：返回覆盖枚举常量的 toString() 方法的值。

- int compareTo(E o)：比较此枚举与指定对象的顺序。

- `Class<E> getDeclaringClass()`：返回与此枚举常量的枚举类型相对应的 Class 对象。

- `static <T extends Enum<T>> T valueOf(Class<T> enumType, String name)`：返回指定名称的枚举常量指定的 enumtype 的方法。如：ColorEnum color = ColorEnum.valueOf("RED");

### 2.1. values()

Java 中使用values()方法将枚举所有元素item转换成一个数组。这样就可以通过foreach语法来遍历枚举中的所有元素了。

```java
for (ColorEnum color: ColorEnum.values()) {  
    log.info("ordinal:{}, name:{}", color.ordinal(), color.name());
}
```

### 2.2. switch()

Java 为枚举提供了switch语法的支持:
```java
// 客户端传来的枚举item
ColorEnum color = ColorEnum.GREEN;

switch (color) {  
    case RED: log.info("进入了 RED 的分支");
        break;
    case GREEN: log.info("进入了 GREEN 的分支");
        break;
    case BLUE: log.info("进入了 BLUE 的分支");
        break;
    default: log.info("进入了 default 的分支");
}
```

注意：

- switch后已经指定了枚举的类型，case后无须再使用全名ColorEnum。

- 在使用 Java 的枚举时往往会结合Switch来进行判断以实现不同值的处理，但是我们知道多用switch不是一种很好的代码风格，不利用维护和适应变化，因为这不符合开闭原则。为此一种方法是用策略模式来重构原有的枚举实现。在《Effective Java》一书中提出了一种枚举策略模式很好的解决了这个问题。

## 3. 自定义属性

可以赋予每一个枚举值若干个属性：
```java
public enum Day {
    MONDAY (1, "星期一", "星期一各种不在状态"),
    TUESDAY (2, "星期二", "星期二依旧犯困"),
    WEDNESDAY (3, "星期三", "星期三感觉半周终于过去了"),
    THURSDAY (4, "星期四", "星期四期待这星期五"),
    FRIDAY (5, "星期五", "星期五感觉还不错"),
    SATURDAY (6, "星期六", "星期六感觉非常好"),
    SUNDAY (7, "星期日", "星期日感觉周末还没过够。");

    Day (int index, String name, String value) {
        this.index = index;
        this.name = name;
        this.value = value;
    }

    private int index;
    private String name;
    private String value;

    //...setter and getter
}
```
测试：

```java
public class EnumTest {
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println(day.getName()+day.getValue());
                break;

            case FRIDAY:
                System.out.println(day.getName()+day.getValue());
                break;

            case SATURDAY: case SUNDAY:
                System.out.println(day.getName()+day.getValue());
                break;

            default:
                System.out.println(day.getName()+day.getValue());
                break;
        }
    }

    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs();
        EnumTest fifthDay = new EnumTest(Day.FRIDAY);
        fifthDay.tellItLikeItIs();
        EnumTest sixthDay = new EnumTest(Day.SATURDAY);
        sixthDay.tellItLikeItIs();
        EnumTest seventhDay = new EnumTest(Day.SUNDAY);
        seventhDay.tellItLikeItIs();
    }
}
```
## 4. 自定义方法

```java
public enum Day {
    MONDAY(1, "星期一", "各种不在状态"){
        @Override
        public Day getNext() {
            return TUESDAY;
        }
    },
    TUESDAY(2, "星期二", "依旧犯困") {
        @Override
        public Day getNext() {
            return WEDNESDAY;
        }
    },
    WEDNESDAY(3, "星期三", "感觉半周终于过去了") {
        @Override
        public Day getNext() {
            return THURSDAY;
        }
    },
    THURSDAY(4, "星期四", "期待这星期五") {
        @Override
        public Day getNext() {
            return FRIDAY;
        }
    },
    FRIDAY(5, "星期五", "感觉还不错"){
        @Override
        public Day getNext() {
            return SATURDAY;
        }
    },
    SATURDAY(6, "星期六", "感觉非常好"){
        @Override
        public Day getNext() {
            return SUNDAY;
        }
    },
    SUNDAY(7, "星期日", "感觉周末还没过够。"){
        @Override
        public Day getNext() {
            return MONDAY;
        }
    };

    Day(int index, String name, String value) {
        this.index = index;
        this.name = name;
        this.value = value;
    }

    private int index;
    private String name;
    private String value;
    public abstract Day getNext();

    //setter and getter
}
```

测试：
```java
public class EnumTest {
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println(day.getName()+day.getValue());
                System.out.println(day.getName()+"的下一天是"+day.getNext().getName());
                break;

            case FRIDAY:
                System.out.println(day.getName()+day.getValue());
                System.out.println(day.getName()+"的下一天是"+day.getNext().getName());
                break;

            case SATURDAY: case SUNDAY:
                System.out.println(day.getName()+day.getValue());
                System.out.println(day.getName()+"的下一天是"+day.getNext().getName());
                break;

            default:
                System.out.println(day.getName()+day.getValue());
                System.out.println(day.getName()+"的下一天是"+day.getNext().getName());
                break;
        }
    }

    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs();
        EnumTest fifthDay = new EnumTest(Day.FRIDAY);
        fifthDay.tellItLikeItIs();
        EnumTest sixthDay = new EnumTest(Day.SATURDAY);
        sixthDay.tellItLikeItIs();
        EnumTest seventhDay = new EnumTest(Day.SUNDAY);
        seventhDay.tellItLikeItIs();
    }
}
```

## 5. enum 与静态常量的比较

很多情况下，enum 都可以用 Java 的静态常量实现相同的效果：
```java
public class EnumTest2 {
    public static final int MONDAY= 1;
    public static final int WEDNESDAY= 3;
    public static final int FRIDAY= 5;
    public static final int SATURDAY= 6;
    public static final int SUNDAY= 7;

    public void tellItLikeItIs(int day) {
        switch (day) {
            case 1:
                System.out.println("周一各种不在状态");
                break;

            case 5:
                System.out.println("周五感觉还不错");
                break;

            case 6: case 7:
                System.out.println("周末给人的感觉是最棒的");
                break;

            default:
                System.out.println("周内感觉就那样吧。");
                break;
        }
    }
}
```

那我们为什么要选择使用 enum 枚举类呢？

- static 方式的静态变量类型不安全，我们可以在调用的时候传入其他值，导致错误。例如： seventhDay.tellItLikeItIs(999);

- static 方式的静态变量不支持属性扩展，每一个 key 对应一个值，而 enum 的每一个 key 可以拥有自己的属性

## 6. Refer Links

https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html

http://www.jianshu.com/p/d2cb1355653c

http://blinkfox.com/java-mei-ju-zhi-shi-zheng-li/
