- [Java 反射 - 其它应用](#java-%E5%8F%8D%E5%B0%84---%E5%85%B6%E5%AE%83%E5%BA%94%E7%94%A8)
  - [1. 绕过安全机制](#1-%E7%BB%95%E8%BF%87%E5%AE%89%E5%85%A8%E6%9C%BA%E5%88%B6)
    - [1.1. 成员权限检查绕过](#11-%E6%88%90%E5%91%98%E6%9D%83%E9%99%90%E6%A3%80%E6%9F%A5%E7%BB%95%E8%BF%87)
    - [1.2. final 检查绕过](#12-final-%E6%A3%80%E6%9F%A5%E7%BB%95%E8%BF%87)
    - [1.3. 集合泛型检查绕过](#13-%E9%9B%86%E5%90%88%E6%B3%9B%E5%9E%8B%E6%A3%80%E6%9F%A5%E7%BB%95%E8%BF%87)
  - [2. 使用泛型 Class 避免强制类型转换](#2-%E4%BD%BF%E7%94%A8%E6%B3%9B%E5%9E%8B-class-%E9%81%BF%E5%85%8D%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [3. Refer Links](#3-refer-links)

# Java 反射 - 其它应用

## 1. 绕过安全机制

### 1.1. 成员权限检查绕过

利用反射可以获取 private 修饰的 Field、Method、Constructor。

例：
```java
public class PrivateObject {

    private String privateString = null;

    public PrivateObject(String privateString) {
        this.privateString = privateString;
    }
}
```
```java
public static void main(String [] args) {
    Field privateStringField = new PrivateObject("The Private Value")
                                      .class.getDeclaredField("privateString");
    // 关闭指定类实例的反射访问检查
    privateStringField.setAccessible(true);
    // 使用反射获取私有属性值
    String fieldValue = (String) privateStringField.get(privateObject);
    System.out.println("fieldValue = " + fieldValue);
}
```

### 1.2. final 检查绕过
 
利用反射可以改变 final 属性的 Field 的值。

### 1.3. 集合泛型检查绕过
  
集合的泛型只是用于防止错误输入，即只在编译阶段有效；若使用方法的反射操作绕过编译检查，可以使集合的泛型限制失效。

```java
ArrayList<String> al = new ArrayList<>();
Class c = al.getClass();
Method m = c.getMethod("add", Object.class);
m.invoke(al, 20);// 绕过集合泛型的编译时检查，添加非 String 元素
System.out.println(al);
```

## 2. 使用泛型 Class 避免强制类型转换

JDK 5 之后，Java 允许使用泛型来限制 Class 类，例如`String.class`的类型实际上是`Class<String>`。通过在反射中使用泛型，可以避免使用反射生成的对象时需要进行不安全的强制类型转换。

```java
public class MyObjectFactory {
  public static Object getInstance(String clsName) {
    try {
      Class cls = Class.forName(clsName);
      return cls.newInstance();
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }

  public static void main(String [] args) {
    Date d = (Date)MyObjectFactory.getInstance("java.util.Date");//1
    JFrame j = (JFrame)MyObjectFactory.getInstance("java.util.Date");//2
  }
}
```
上述代码在编译时都不会产生任何问题，但在运行时第 2 行代码将会抛出 ClassCastException 异常。

如果将上边代码改写为使用泛型后的 Class，就可以在编译阶段进行严格的类型检查，从而无需使用强制类型转换，避免发生这种运行时异常：
```java
public class MyObjectFactory {
  public static <T> T getInstance(Class<T> cls) {
    try {
      return cls.newInstance();
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }

  public static void main(String [] args) {
    Date d = (Date)MyObjectFactory.getInstance(Date.class);// 正常运行
    JFrame j1 = (JFrame)MyObjectFactory.getInstance(JFrame.class);// 正常运行
    JFrame j2 = (JFrame)MyObjectFactory.getInstance(Date.class);// 编译时就会报错
  }
}
```

## 3. Refer Links