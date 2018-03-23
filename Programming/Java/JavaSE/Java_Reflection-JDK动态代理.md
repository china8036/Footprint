- [Java 反射 JDK 动态代理](#java-%E5%8F%8D%E5%B0%84-jdk-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)
    - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [1.1. 代理模式](#11-%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F)
        - [1.2. 静态代理](#12-%E9%9D%99%E6%80%81%E4%BB%A3%E7%90%86)
        - [1.3. 动态代理](#13-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)
    - [2. JDK 动态代理实现方法](#2-jdk-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E5%AE%9E%E7%8E%B0%E6%96%B9%E6%B3%95)
    - [3. 动态代理与 AOP](#3-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E4%B8%8E-aop)
    - [4. Refer Links](#4-refer-links)

# Java 反射 JDK 动态代理

利用 Java 反射机制你可以在运行期动态的创建接口的实现。java.lang.reflect.Proxy 类就可以实现这一功能。动态的代理的用途十分广泛，比如数据库连接和事物管理（transaction management）还有单元测试时用到的动态 mock 对象以及 AOP 中的方法拦截功能等等都使用到了动态代理。

## 1. 基本概念

### 1.1. 代理模式

代理模式是面向对象编程中比较常见的设计模式。

举例来说，顾客可以直接从厂家购买产品，但是现实生活中，很少有这样的销售模式。一般都是厂家委托给代理商进行销售，顾客跟代理商打交道，而不直接与产品实际生产者进行关联。在这个过程中，代理商就起着代理的作用。

代理模式可以在不修改被代理对象的基础上，通过扩展代理类，进行一些功能的附加与增强。值得注意的是，代理类和被代理类应该共同实现一个接口，或者是共同继承某个类。

代理可以分为静态代理和动态代理两种。

### 1.2. 静态代理

举例说明：我们平常去电影院看电影的时候，在电影开始的阶段经常会放广告。电影是电影公司委托给影院进行播放的，但是影院可以在播放电影的时候，产生一些自己的经济收益，比如卖爆米花、可乐等，然后在影片开始结束时播放一些广告。这个过程中，电影公司是被代理者，电影院是代理。

```java
// 通用的接口是代理模式实现的基础
// Movie，代表电影播放的能力
public interface Movie {
    void play();
}

// 表示一部真正的影片。它实现了 Movie 接口，play() 方法调用时，影片就开始播放
public class RealMovie implements Movie {
    @Override
    public void play() {
        // TODO Auto-generated method stub
        System.out.println("您正在观看电影 《肖申克的救赎》");
    }
}

// Cinema 就是 Proxy 代理对象，它有一个 play() 方法。不过调用 play() 方法时，它进行了一些相关利益的处理，那就是广告
public class Cinema implements Movie {
    RealMovie movie;

    public Cinema(RealMovie movie) {
        super();
        this.movie = movie;
    }

    @Override
    public void play() {
        guanggao(true);
        movie.play();
        guanggao(false);
    }

    public void guanggao(boolean isStart){
        if ( isStart ) {
            System.out.println("电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！");
        } else {
            System.out.println("电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！");
        }
    }
}

// 编写测试代码
public class ProxyTest {
    public static void main(String[] args) {
        RealMovie realmovie = new RealMovie();
        Movie movie = new Cinema(realmovie);
        movie.play();
    }
}
```
运行结果：
```
电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！
您正在观看电影 《肖申克的救赎》
电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！
```

上面介绍的是静态代理的内容，为什么叫做静态代理呢？因为它的类型是事先预定好，硬编码到代码中去的，比如上面代码中的 Cinema 这个类。

### 1.3. 动态代理

在静态代理的示例代码中，Cinema 类是代理，我们需要手动编写代码让 Cinema 实现 Movie 接口，而在动态代理中，我们可以让程序在运行的时候自动在内存中创建一个实现 Movie 接口的代理，而不需要去定义 Cinema 这个类。这就是它被称为动态的原因。

## 2. JDK 动态代理实现方法

在 Java 的 java.lang.reflect 包下提供了一个 [Proxy](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Proxy.html) 类和一个 [InvocationHandler](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InvocationHandler.html) 接口，通过使用这个类和接口，就可以生成 JDK 动态代理类或动态代理对象。

Proxy 类是所有动态代理类的父类，它提供了用于创建动态代理类和动态代理对象的静态方法：
- `static Class<?>	getProxyClass​(ClassLoader loader, Class<?>... interfaces)`: 创建一个动态代理类，返回类所对应的 Class 对象，但该方法官方文档不推荐使用（Constructor.newInstance will throw IllegalAccessException when it is called on an inaccessible proxy class. Use newProxyInstance(ClassLoader, Class[], InvocationHandler) to create a proxy instance instead）。

- `static Object	newProxyInstance​(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)`: 直接创建一个动态代理对象。
  
  参数说明：
  - 第一个参数指定该代理对象的类加载器。
  - 第二个参数指定该代理对象要实现的多个接口，JDK 动态代理只能为接口创建动态代理。
  - 第三个参数指定该代理对象的 InvocationHandler 对象，执行代理对象的每个方法时都会被替换成执行 InvocationHandler 对象的 invoke 方法。
    - `Object invoke​(Object proxy, Method method, Object[] args) throws Throwable`: 调用代理对象的所有方法时都会被替换成调用该 invoke 方法并返回结果。
      
      参数说明：
      - proxy：动态代理的对象实例。
      - method：正在调用的方法。
      - args：正在调用的方法所传入的实参数组。

例：假设有一个大商场，商场有很多的柜台，有一个柜台卖茅台酒。
```java
// SellWine 是一个接口，你可以理解它为卖酒的许可证
public interface SellWine {
     void mainJiu();
}

// 茅台酒
public class MaotaiJiu implements SellWine {
    @Override
    public void mainJiu() {
        // TODO Auto-generated method stub
        System.out.println("我卖得是茅台酒。");
    }
}

// 五粮液
public class Wuliangye implements SellWine {
    @Override
    public void mainJiu() {
        // TODO Auto-generated method stub
        System.out.println("我卖得是五粮液。");
    }
}

// 我们还需要一个柜台来卖酒，这个柜台就是动态代理
public class GuitaiA implements InvocationHandler {

    private Object pingpai;
    
    public GuitaiA(Object pingpai) {
        this.pingpai = pingpai;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        // TODO Auto-generated method stub
        System.out.println("销售开始  柜台是： "+this.getClass().getSimpleName());
        method.invoke(pingpai, args);
        System.out.println("销售结束");
        return null;
    }
}

// 编写测试代码
public class Test {
    public static void main(String[] args) {
        // TODO Auto-generated method stub

        MaotaiJiu maotaijiu = new MaotaiJiu();
        Wuliangye wu = new Wuliangye();

        InvocationHandler jingxiao1 = new GuitaiA(maotaijiu);
        InvocationHandler jingxiao2 = new GuitaiA(wu);

        SellWine dynamicProxy = (SellWine) Proxy.newProxyInstance(MaotaiJiu.class.getClassLoader(),
                MaotaiJiu.class.getInterfaces(), jingxiao1);
        SellWine dynamicProxy1 = (SellWine) Proxy.newProxyInstance(MaotaiJiu.class.getClassLoader(),
                MaotaiJiu.class.getInterfaces(), jingxiao2);
        dynamicProxy.mainJiu();
        dynamicProxy1.mainJiu();
    }

}
```
运行结果：
```
销售开始  柜台是： GuitaiA
我卖得是茅台酒。
销售结束
销售开始  柜台是： GuitaiA
我卖得是五粮液。
销售结束
```

## 3. 动态代理与 AOP

实际上，在普通编程中，确实很少会使用到动态代理，但在编写框架或底层基础代码时，动态代理的作用十分强大。

开发软件时，经常会遇到存在相同代码段重复出现的情况，在这种情况下，可通过将这些代码不断地复制粘贴，快速实现软件功能。但采用这样的方法，若在维护时需要修改这部分重复的代码，就需要在每一处粘贴的地方进行修改，工作量巨大。

因此，可以将这部分重复代码封装成一个方法，再在每处用到该代码段的地方调用该方法即可。通过这种方式，大大降低了后期维护的复杂度，但在每处用到该代码段的地方硬编码调用特定的方法，又产生了新的耦合。**最理想的效果应是：在每处需要用到该代码段的地方即可以执行该代码段，又无需在程序中以硬编码的方式直接调用特定的封装方法。**这时就可以使用动态代理来实现。

例：
```java
public interface Dog {
  void info();
  void run();
}

public class GunDog implements Dog {
  @Override
  public void info() {
    System.out.println("我是一只猎狗");
  }

  @Override
  public void run() {
    System.out.println("我奔跑迅速");
  }
}

// 通用工具类，包含了通用方法
public class DogUtil {
  public static void method1() {
    System.out.println("执行第一个通用方法");
  }

  public static void method2() {
    System.out.println("执行第二个通用方法");
  }
}
```
现在的要实现的功能是：当程序执行 info() 和 run() 方法时，能够调用 method1() 和 method2() 两个通用方法，即自动将 method1() 和 method2() 两个通用方法插入到 info() 和 run() 方法中执行，但又不能以硬编码的方式在 info() 和 run() 的实现代码中调用这两个方法。
```java
public class MyInvocationHandler implements InvocationHandler {
  // 需要被代理的对象
  private Object target;

  public void setTarget(Objecr target) {
    this.target = target;
  }

  @Override
  public object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    DogUtil.method1();
    // 通过反射以 target 作为主调来执行 target 对象的原有的方法 method
    Object result = method.invoke(this.target, args);
    DogUtil.method2();
    
    return result;
  }
}

// 代理工厂类，专为指定的 target 生成动态代理实例
public class MyProxyFactory {
  // 为指定的 target 生成动态代理实例
  public static Object getProxy(Object target) throws Exception {
    MyInvocationHandler handler = new MyInvocationHandler();
    handler.setTarget(target);
    // 创建并返回一个动态代理对象
    return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), handler);
  }
}

// 编写测试代码
public class Test {
  public static void main(String [] args) throws Exception {
    Dog gunDog = new GunDog();
    Dog proxyDog = (Dog)MyProxyFactory.getProxy(gunDog);
    proxyDog.info();
    proxyDog.run();
  }
}
```

通过动态代理，可以非常灵活的实现解耦。一般情况下，使用 Proxy 生成一个动态代理时，往往并不会凭空产生一个动态代理，这样没有太大的实际意义，通常都是为指定的目标对象生成动态代理。

这种动态代理在 AOP 中也称为 AOP 代理，AOP 代理里的方法可以在执行目标方法之前、之后插入一些通用的处理代码。

## 4. Refer Links

[轻松学，Java 中的代理模式及动态代理](http://blog.csdn.net/briblue/article/details/73928350)

[Java Reflection（十一): 动态代理](http://ifeve.com/java-reflection-11-dynamic-proxies/)