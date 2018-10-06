- [Design Pattern 结构型模式 - 代理模式](#design-pattern-结构型模式---代理模式)
    - [1. 基本概念](#1-基本概念)
    - [2. 代理类型](#2-代理类型)
    - [3. 静态代理](#3-静态代理)
        - [3.1. 继承方式](#31-继承方式)
        - [3.2. 聚合方式](#32-聚合方式)
        - [3.3. 比较](#33-比较)
    - [4. 动态代理](#4-动态代理)
        - [4.1. JDK 动态代理](#41-jdk-动态代理)
            - [4.1.1. API 说明](#411-api-说明)
            - [4.1.2. 使用示例](#412-使用示例)
            - [4.1.3. 优缺点](#413-优缺点)
        - [4.2. Cglib 动态代理](#42-cglib-动态代理)
            - [4.2.1. 基本概念](#421-基本概念)
            - [4.2.2. 原理分析](#422-原理分析)
            - [4.2.3. API 说明](#423-api-说明)
            - [4.2.4. 使用示例](#424-使用示例)
            - [4.2.5. 与 JDK 动态代理的比较](#425-与-jdk-动态代理的比较)
            - [4.2.6. 应用](#426-应用)
        - [4.3. 模拟 JDK Proxy 实现动态代理](#43-模拟-jdk-proxy-实现动态代理)
        - [4.4. JAVAASSIST 动态代理](#44-javaassist-动态代理)
        - [4.5. AspectJ 动态代理](#45-aspectj-动态代理)
        - [4.6. 动态代理应用](#46-动态代理应用)
            - [4.6.1. AOP](#461-aop)
            - [4.6.2. 数据库连接池](#462-数据库连接池)
            - [4.6.3. 事务管理](#463-事务管理)
            - [4.6.4. 单元测试动态 mock 对象](#464-单元测试动态-mock-对象)
    - [5. 适用场景](#5-适用场景)
    - [6. Refer Links](#6-refer-links)

# Design Pattern 结构型模式 - 代理模式

## 1. 基本概念

代理模式是面向对象编程中比较常见的结构型设计模式。代理模式通过**为其他对象提供一种代理对象（中间层）以控制对这个对象的访问**，可以在不修改被代理对象的基础上，通过扩展代理类，进行一些功能的附加与增强。代理对象在客户端对象和目标对象之间起到中介的作用，它去掉客户不能看到的内容和服务或者增添客户需要的额外的新服务。

举例来说，现实生活中顾客可以直接从厂家购买产品，但实际上很少有这样的销售模式，一般都是厂家委托给代理商进行销售，顾客跟代理商打交道，而不直接与产品实际生产者进行关联。在这个过程中，代理商就起着代理的作用。

为了保证客户端使用的透明性，通常所访问的真实对象与代理对象需要实现相同的接口或继承相同的抽象基类。

**代理模式的实现方式可以分为静态代理和动态代理两种**。

## 2. 代理类型

在实际开发过程中，代理类的实现相当复杂，代理模式根据其目的和实现方式不同可分为很多种类，其中常用的几种代理模式简要说明如下：
- 远程代理 (Remote Proxy)
  
  为一个位于不同的地址空间的对象提供一个本地的代理对象，这个不同的地址空间可以是在同一台主机中，也可是在另一台主机中，远程代理又称为大使 (Ambassador)。

- 虚拟代理 (Virtual Proxy)
  
  如果需要创建一个资源消耗较大的对象，先创建一个消耗相对较小的对象来表示，真实对象只在需要时才会被真正创建。

- 保护代理 (Protect Proxy)
  
  控制对一个对象的访问，可以给不同的用户提供不同级别的使用权限。

- 缓冲代理 (Cache Proxy)
  
  为某一个目标操作的结果提供临时的存储空间，以便多个客户端可以共享这些结果。

- 智能引用代理 (Smart Reference Proxy)
  
  当一个对象被引用时，提供一些额外的操作，例如将对象被调用的次数记录下来等。

在这些常用的代理模式中，有些代理类的设计非常复杂，例如远程代理类，它封装了底层网络通信和对远程对象的调用，其实现较为复杂。代理模式类型较多，其中远程代理、虚拟代理、保护代理等在软件开发中应用非常广泛。

## 3. 静态代理

静态代理指的是**代理和被代理对象在代理运行之前就是以硬编码方式确定好的，它们都实现了相同的接口或继承了相同的抽象基类**。

静态代理的实现有两种方式：继承方式和聚合方式。

### 3.1. 继承方式

继承方式是指代理对象继承了被代理对象，通过重写被代理的方法，并在重写的方法中调用父类方法同时加上代理对象的业务处理，实现静态代理。

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
        System.out.println("您正在观看电影 《肖申克的救赎》");
    }
}

// CinemaProxy 就是代理对象，它继承了被代理对象 RealMovie 类
public class CinemaProxy extends RealMovie {
    public CinemaProxy() {
        super();
    }

    // 在重写的 play() 方法中调用父类的 play() 方法，但在调用前后添加了广告操作
    @Override
    public void play() {
        ad(true);
        super.play();
        ad(false);
    }

    public void ad(boolean isStart){
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
        Movie movie = new CinemaProxy();
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

### 3.2. 聚合方式

聚合方式：代理对象与被代理对象实现相同的接口，且被代理对象以属性变量的形式聚合在代理对象中，从而实现在调用代理对象的相关方法时，能够调用被代理对象的相应方法同时加上自己的业务处理，实现静态代理。

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
        System.out.println("您正在观看电影 《肖申克的救赎》");
    }
}

// CinemaProxy 就是代理对象，它与 RealMovie 实现了相同的接口
public class CinemaProxy implements Movie {
    // 将被代理对象 (RealMovie 对象) 以聚合方式放在代理对象 Proxy 中
    private RealMovie movie;

    public CinemaProxy(RealMovie movie) {
        super();
        this.movie = movie;
    }

    // 它有一个 play() 方法。不过调用 play() 方法时，它添加了一些相关利益的广告操作
    @Override
    public void play() {
        ad(true);
        movie.play();
        ad(false);
    }

    public void ad(boolean isStart){
        if (isStart) {
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
        Movie movie = new CinemaProxy(realmovie);
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

### 3.3. 比较

假如我们希望在播放电影前后，除了广告操作，还能添加其它操作（如广播、环境操作等）：

- 使用继承方式来实现代理功能的叠加，我们需要进行对代理类进行以下修改：
  ```java
  public class CinemaProxy extends RealMovie {
      public CinemaProxy() {
          super();
      }

      // 它在重写的 play() 方法中调用父类的 play() 方法，但在调用前后添加了广播、广告、环境操作
      @Override
      public void play() {
          broacast(true);
          ad(true);
          envOpt(true);
          super.play();
          envOpt(false);
          ad(false);
          broacast(false);
      }

      public void ad(boolean isStart){
          if (isStart) {
              System.out.println("电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！");
          } else {
              System.out.println("电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！");
          }
      }

      public void broacast(boolean isStart) {
          if (isStart) {
              System.out.println("请有序进场");
          } else {
              System.out.println("请有序退场");
          }
      }

      public envOpt(boolean isStart) {
        if (isStart) {
          System.out.println("关灯");
        } else {
          System.out.println("开灯");
        }
      }
  }
  ```
- 使用聚合方式来实现代理功能的叠加，则可将每个代理功能独立成一个新的类：
  ```java
  public class CinemaAdProxy extends RealMovie {
      public CinemaAdProxy() {
          super();
      }

      @Override
      public void play() {
          ad(true);
          super.play();
          ad(false);
      }

      public void ad(boolean isStart){
          if (isStart) {
              System.out.println("电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！");
          } else {
              System.out.println("电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！");
          }
      }
  }
  public class CinemaBrocastProxy extends RealMovie {
      public CinemaBrocastProxy() {
          super();
      }

      @Override
      public void play() {
          broacast(true);
          super.play();
          broacast(false);
      }

      public void broacast(boolean isStart) {
          if (isStart) {
              System.out.println("请有序进场");
          } else {
              System.out.println("请有序退场");
          }
      }
  }
  public class CinemaEnvOptProxy extends RealMovie {
      public CinemaEnvOptProxy() {
          super();
      }

      @Override
      public void play() {
          envOpt(true);
          super.play();
          envOpt(false);
      }

      public envOpt(boolean isStart) {
        if (isStart) {
          System.out.println("关灯");
        } else {
          System.out.println("开灯");
        }
      }
  }
  // 编写测试代码
  public class ProxyTest {
      public static void main(String[] args) {
          Movie movie = new RealMovie();
          CinemaBrocastProxy bp = new CinemaBrocastProxy(movie);
          CinemaAdProxy ap = new CinemaAdProxy(bp);
          CinemaEnvOptProxy ep = new CinemaEnvOptProxy(ap);

          ep.play();
      }
  } 
  ```

如果我们需要能够在代理时多样化各个动作执行的顺序（如广播广告环境 / 广告广播环境 / 环境广告广播 ...）
- 如果继承方式来实现：若有 3 个代理动作我们就需要添加 7 个新的代理类。因此，若使用继承方式来实现代理功能的叠加，必定会导致代理类数量的无限膨胀下去。
- 如果采用聚合方式来实现：若有 3 个代理动作我们无须对原来的 3 个代理类进行任何改动，只需在创建实例时，根据需要的顺序改变代理类的创建顺序即可：
  ```java
  public static void main(String[] args) {
      Movie movie = new RealMovie();
      CinemaAdProxy ap = new CinemaAdProxy(movie);
      CinemaBrocastProxy bp = new CinemaBrocastProxy(ap);
      CinemaEnvOptProxy ep = new CinemaEnvOptProxy(bp);

      ep.play();
  }
  ```

因此，**使用聚合的方式实现静态代理，多个代理类可以相互传递互相组合，相比继承方式的实现更加灵活，这也是更为推荐的方式**。

## 4. 动态代理

在静态代理的示例中，如果现在除了电影院 Cinema 要代理播放这部电影，有一些线上网站 Website、电影软件 App 也要代理播放这个电影，根据静态代理的实现方式，我们需要建立 WebsiteAdProxy、WebsiteBroacastProxy、WebsiteEnvOptProxy 和 AppAdProxy、AppBroacastProxy、AppEnvOptProxy 这些代理类。

可见，如果要想为多个类进行代理，则需要建立多个代理类。由于静态代理模式中代理对象和被代理对象以硬编码的形式在程序运行之前就确定好了，非常不利于代理对象的横向扩展。因此，出现了在运行时才动态确定代理对象和被代理对象的动态代理模式。在静态代理中，Cinema 类是代理对象，我们需要手动编写代码让 Cinema 实现 Movie 接口。而在动态代理中，我们可以通过 Java 的反射机制让程序在运行期自动在内存中创建一个实现 Movie 接口的代理实现，而不需要去定义 Cinema 这个类。这就是它被称为动态的原因。

动态代理是代理模式实现的一种，它的用途十分广泛，比如数据库连接池、事务管理（transaction management）、单元测试时用到的动态 mock 对象以及 AOP 中的方法拦截功能等都使用到了动态代理。

### 4.1. JDK 动态代理

从 JDK 1.3 版本开始，在 `java.lang.reflect` 包下提供了一个 [Proxy](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Proxy.html) 类和一个 [InvocationHandler](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InvocationHandler.html) 接口，它们对动态代理机制进行了封装和实现，可供程序员调用。通过使用这个类和接口，就可以生成 JDK 动态代理类或动态代理对象。

#### 4.1.1. API 说明

Proxy 类是所有动态代理类的父类，它提供了用于创建动态代理类和动态代理对象的静态方法：
- `static Class<?> getProxyClass​(ClassLoader loader, Class<?>... interfaces)`
	
	创建一个动态代理类，返回类所对应的 Class 对象，但该方法官方文档不推荐使用。
	
	> Constructor.newInstance will throw IllegalAccessException when it is called on an inaccessible proxy class. Use newProxyInstance(ClassLoader, Class[], InvocationHandler) to create a proxy instance instead.

- `static Object newProxyInstance​(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)`

	直接创建一个动态代理对象，官方推荐使用此方法。
  
  参数说明：
  - 第一个参数指定该代理对象的类加载器。
  - 第二个参数指定该代理对象要实现的多个**interface**，JDK 动态代理只能为**interface**（不可以是 class）创建动态代理。
  - 第三个参数指定该代理对象的 InvocationHandler 对象，执行代理对象的每个方法时都会被替换成执行 InvocationHandler 对象的 invoke 方法。
  
InvocationHandler 接口是由代理实例的调用处理程序实现的接口，调用代理对象的所有方法时都会被替换成调用该接口的 invoke 方法并返回结果：
- `Object invoke​(Object proxy, Method method, Object[] args) throws Throwable`
	- proxy：动态代理的对象实例。
	- method：正在调用的方法。
	- args：正在调用的方法所传入的实参数组。

#### 4.1.2. 使用示例

假设有一个大商场，商场有很多的柜台，有一个柜台卖茅台酒，那么这个柜台就是动态代理
```java
// SellWine 是一个接口，你可以理解它为卖酒的许可证
public interface SellWine {
     void mainJiu();
}

// 茅台酒
public class MaotaiJiu implements SellWine {
    @Override
    public void mainJiu() {
        System.out.println("我卖得是茅台酒。");
    }
}

// 五粮液
public class Wuliangye implements SellWine {
    @Override
    public void mainJiu() {
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
        System.out.println("销售开始  柜台是： "+this.getClass().getSimpleName());
        Object res = method.invoke(pingpai, args);
        System.out.println("销售结束");
        return res;
    }
}

// 编写测试代码
public class Test {
    public static void main(String[] args) {
        MaotaiJiu maotaijiu = new MaotaiJiu();
        Wuliangye wu = new Wuliangye();

        InvocationHandler jingxiao1 = new GuitaiA(maotaijiu);
        InvocationHandler jingxiao2 = new GuitaiA(wu);

        SellWine dynamicProxy1 = (SellWine) Proxy.newProxyInstance(MaotaiJiu.class.getClassLoader(),
                MaotaiJiu.class.getInterfaces(), jingxiao1);
        SellWine dynamicProxy2 = (SellWine) Proxy.newProxyInstance(Wuliangye.class.getClassLoader(),
                Wuliangye.class.getInterfaces(), jingxiao2);
        dynamicProxy1.mainJiu();
        dynamicProxy2.mainJiu();
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

#### 4.1.3. 优缺点

优点：
- 实现无侵入式的代理实现。

缺点：
- 代理类和委托类需要都实现同一个接口，也就是说**只有实现了某个接口的类可以使用 JDK 提供的动态代理机制**。但是，事实上使用中并不是遇到的所有类都会给你实现一个接口。因此，对于没有实现接口的类，就不能使用该机制。

	如果想代理没有实现接口的继承的类该怎么办？可以使用 Cglib。

### 4.2. Cglib 动态代理

#### 4.2.1. 基本概念

> cglib is a powerful, high performance and quality Code Generation Library, It is used to extend JAVA classes and implements interfaces at runtime. 

[cglib](https://github.com/cglib/cglib)，即 Code Generation Library，是一个强大、高性能、高质量的字节码生成类库，它可以在运行期扩展 Java 类与创建 Java 接口的实现。

Cglib 可以为没有实现接口的类提供代理，为 JDK 的动态代理提供了很好的补充，当要代理的类没有实现接口或者为了更好的性能，cglib 是一个更好的选择。

#### 4.2.2. 原理分析

Cglib 是针对类来实现代理的，它需要指定父类和回调方法，然后动态生成一个实现了 MethodInterceptor 接口、继承自被代理类的代理类，并在代理类中覆盖被代理类所有非 final 的方法，实现 MethodInterceptor 接口的 intercept 方法。当调用父类方法时，在子类中采用方法拦截的技术拦截所有父类方法的调用，织入横切逻辑，从而实现动态代理。

**Cglib 底层使用一个小而快的字节码处理框架 [ASM](http://asm.ow2.io/)，来转换字节码并生成新的类**。除了 Cglib 包，许多 JVM 脚本语言（如 Groovy 和 BeanShell）也是使用 ASM 来生成 Java 的字节码（但不鼓励直接使用 ASM，因为它要求你必须对 JVM 内部结构包括 class 文件的格式和指令集都很熟悉）。

由于采用的是继承，所以**不能对 final 修饰的类进行继承方式的代理**，但 Cglib 也可以与 Java 动态代理一样面向接口，因为实现接口的本质也是继承。

#### 4.2.3. API 说明

Cglib 库的代码量不多，但是由于缺乏文档导致学习起来比较困难。2.1.2 版本的 Cglib 库源码组织如下所示：
- `net.sf.cglib.core`: 底层字节码操作类；大部分与 ASP 相关。
- `net.sf.cglib.transform`: 编译期、运行期的 class 文件转换类。
- `net.sf.cglib.proxy`: 代理创建类、方法拦截类。
- `net.sf.cglib.reflect`: 更快的反射类、C# 风格的代理类。
- `net.sf.cglib.util`: 集合排序工具类。
- `net.sf.cglib.beans`: JavaBean 相关的工具类。
对于创建动态代理，大部分情况下你只需要使用 proxy 包的一部分 API 即可。

在 Cglib 中实现动态代理，主要用到的相关 API 如下：
- `Enhancer`类：用于指定被代理类、回调拦截器对象（或对象数组）和拦截过滤器，然后动态创建代理类实例。
 - `setSuperclass()`: 指定代理类的父类（即被代理类），传入被代理类的 Class 对象。
 - `setCallback()`: 指定回调拦截器对象，传入一个实现 MethodInterceptor 接口的对象。当调用代理类的方法时，会触发该拦截器中`intercept()`方法的回调。
 - `setCallbacks()`: 指定回调拦截器的对象数组，传入一个包含实现 MethodInterceptor 接口的对象的数组。当调用代理类的方法时，会通过拦截过滤器，选择触发指定拦截器中`intercept()`方法的回调。
 - `setCallbackFilter()`: 指定拦截过滤器，传入一个实现 CallbackFilter 接口的对象。
 - `create()`: 动态创建代理对象，返回一个 Object 对象，需要进行强制类型转换。

- `Callback`接口：所有回调拦截器都会继承该接口。
	- `MethodInterceptor`接口：扩展自 Callback 接口，用于实现一个回调拦截器，需要实现其中的 intercept() 方法，调用被代理类的任意方法时，会触发该方法的执行。
		- `Object intercept(Object obj, Method method, Object[] params,  MethodProxy proxy) throws Throwable`
			- Object obj: 由 Cglib 动态生成的代理类实例对象。
			- Method method: 被调用的被代理方法引用对象。
			- Object[] params: 被调用方法的参数值列表。
			- MethodProxy proxy: 代理类对方法的代理引用对象，原始类里每一个方法都会在动态的子类里有一个对应的 MethodProxy。
				
				我们一般使用 `proxy.invokeSuper(obj,args)` 方法，这个很好理解，就是执行原始类的方法。还有一个方法 `proxy.invoke(obj,args)`，这是执行生成子类的方法，但如果传入的 obj 就是子类的话，会发生内存溢出，因为子类的方法不停地进入 intercept 方法，而这个方法又去调用子类的方法，两个方法直接循环调用了。

				一个 MethodProxy 又对应了两个动态生成的 FastClass 类，一个是对应原始方法，一个对应新生成的子类，MethodProxy.invokeSuper 就是交给对应原始方法那个 FastClass，MethodProxy.invoke 交给另一个。
	
	- `FixedValue`接口：扩展自 Callback 接口，用于实现一个锁定方法返回值的回调过滤器，即无论被代理类的方法返回什么值，回调方法都返回固定值。实现该接口需要实现 loadObject() 方法：
		- `Object loadObject() throws Exception`

	- [`NoOp.INSTANCE`对象](http://cglib.sourceforge.net/apidocs/net/sf/cglib/proxy/NoOp.html)：扩展自 Callback 接口，NoOp.INSTANCE 对象表示一个没有任何操作的拦截器。NoOp 表示 no operator，即什么操作也不做。 
	  > Methods using this Enhancer callback will delegate directly to the default (super) implementation in the base class.

- `CallbackFilter` 接口：用于实现一个回调过滤器，可以在回调时指定对不同方法使用不同的回调拦截器，或者根本不执行回调。

	P.S. 在 JDK 动态代理中并没有类似的功能，对 InvocationHandler 接口方法的调用对代理类内的所以方法都有效。

	实现一个回调过滤器，需要实现该接口的`accept()`方法：
	- `int accept(Method method)`: 参数 Method 对象为被调用的被代理对象方法，返回一个 int 数值，表示调用回调拦截器对象的数组中的位置索引。

#### 4.2.4. 使用示例

```java
// 被代理类
public class TargetObject {  
    public String method1(String paramName) {  
        return paramName;  
    }  
    public int method2(int count) {  
        return count;  
    }  
    public int method3(int count) {  
        return count;  
    }  
}

// 实现一个回调拦截器
public class TargetInterceptor implements MethodInterceptor {  
    // 重写方法拦截在方法前和方法后加入业务  
    @Override  
    public Object intercept(Object obj, Method method, Object[] params,  
            MethodProxy proxy) throws Throwable {  
        System.out.println("调用前");  
        Object result = proxy.invokeSuper(obj, params);  // 执行原始类的方法
        // Object result = method.invoke(conn, params); // 执行生成子类的方法，obj 是调用该方法的对象
        System.out.println(" 调用后"+result);  
        return result;  
    }  
}  

// 实现一个锁定方法返回值的回调拦截器
public class TargetResultFixed implements FixedValue{  
    /**  
     * 锁定回调值为 999  
     * CallbackFilter 中定义的使用 FixedValue 型回调的方法为 getConcreteMethodFixedValue，该方法返回值为整型
     */  
    @Override  
    public Object loadObject() throws Exception {  
        System.out.println("锁定结果");  
        Object obj = 999;  
        return obj;  
    }  
  
}  

// 实现一个拦截过滤器
public class TargetMethodCallbackFilter implements CallbackFilter {  
    /** 
     * 过滤方法 
     * 返回的值为数字，代表了 Callback 数组中的索引位置所对于的 Callback 对象
     */  
    @Override  
    public int accept(Method method) {  
        if(method.getName().equals("method1")){  
            System.out.println("filter method1 ==0");  
            return 0;  
        }  
        if(method.getName().equals("method2")){  
            System.out.println("filter method2 ==1");  
            return 1;  
        }  
        if(method.getName().equals("method3")){  
            System.out.println("filter method3 ==2");  
            return 2;  
        }  
        return 0;  
    }  
}  

// 测试代码
public static void main(String args[]) {  
		Enhancer enhancer = new Enhancer();  
		enhancer.setSuperclass(TargetObject.class);  
		enhancer.setCallbacks(new Callback[] {
				new TargetInterceptor(), 
				NoOp.INSTANCE, 
				new TargetResultFixed()
		});  
		enhancer.setCallbackFilter(new TargetMethodCallbackFilter());  

		TargetObject targetProxy = (TargetObject)enhancer.create();  
		System.out.println(targetObject2.method1("mmm1"));  
		System.out.println(targetObject2.method2(100));  
		System.out.println(targetObject2.method3(100));  
		System.out.println(targetObject2.method3(200));  
} 
```

#### 4.2.5. 与 JDK 动态代理的比较

在动态生成代理对象这一 feature 上，cglib 与 JDK 动态代理的区别在于：
- 实现方式
	- JDK 动态代理是由 Java 内部的反射机制生成一个实现代理接口的匿名类来实现的。
	- Cglib 动态代理是借助 ASM 生成字节码文件创建一个被代理类的子类并覆盖其中的方法来实现的。

- 执行效率
	- 反射机制在生成类的过程中更加高效。
	- ASM 在生成类之后的相关执行过程中比较高效（但可以通过将 ASM 生成的类进行缓存，这样解决 ASM 生成类过程低效问题）。

	参考这里的 [性能测试](https://www.jianshu.com/p/1aaacf92e2cd) 可知：
	- 相同情况下，Cglib 两种实现方式，`invokeSuper` 和 `setSuperClass` 组合实现方式永远比 `invoke` 和 `setInterfaces` 组合实现方式**慢**。
	- Cglib 中的 `invoke` 和 `setInterfaces` 组合实现方式在代理方法数量较少、函数平均调用的情况下，执行速度比 JDK Proxy 快。但随着代理方法的数量增多，优势越来越不明显，到达某个数量级速度比 JDK Proxy 慢。
	- Cglib 中的 `invoke` 和 `setInterfaces` 组合实现方式在调用特定函数（在 switch 中靠后的 case) 会比 JDK Proxy 慢。
	
- 使用限制
	- JDK 动态代理的使用前提，必须是目标类基于统一的接口。如果没有上述前提，JDK 动态代理不能应用。由此可以看出，JDK 动态代理有一定的局限性。
	- 相比之下 Cglib 由于采用字节码生成代理对象，实现的动态代理没有多余限制，应用更加广泛。

#### 4.2.6. 应用

Cglib 广泛的被许多 AOP 的框架使用，例如：
- Spring AOP 和 dynaop，为他们提供方法的 interception（拦截）。
- 最流行的 ORM 工具 hibernate 使用 Cglib 来代理单端 single-ended（多对一和一对一) 关联（对集合的延迟抓取，是采用其他机制实现的）。
- EasyMock 和 jMock 通过使用模仿（mock）对象来测试 Java 代码的包，它们都通过使用 Cglib 来为那些没有接口的类创建模仿（mock）对象。

### 4.3. 模拟 JDK Proxy 实现动态代理

https://www.imooc.com/video/4903

### 4.4. JAVAASSIST 动态代理

[数据库连接池 HikariCP 中就采用了 JAVAASSIST 实现连接对象的动态代理](https://www.javafm.com/issue/141)。

JAVAASSIST 的字节码生成方式比 ASM 方便，JAVAASSIST 只需用字符串拼接出 Java 源码，便可生成相应字节码，而 ASM 需要手工写字节码。

### 4.5. AspectJ 动态代理

### 4.6. 动态代理应用

实际上，在普通编程中，确实很少会使用到动态代理，但在编写框架或底层基础代码时，动态代理的作用十分强大。

#### 4.6.1. AOP

开发软件时，经常会遇到存在相同代码段重复出现的情况，在这种情况下，可通过将这些代码不断地复制粘贴，快速实现软件功能。但采用这样的方法，若在维护时需要修改这部分重复的代码，就需要在每一处粘贴的地方进行修改，工作量巨大。因此，可以将这部分重复代码封装成一个方法，再在每处用到该代码段的地方调用该方法即可。通过这种方式，大大降低了后期维护的复杂度，但在每处用到该代码段的地方硬编码调用特定的方法，又产生了新的耦合。

**最理想的效果应是：在每处需要用到该代码段的地方即可以执行该代码段，又无需在程序中以硬编码的方式直接调用特定的封装方法。**这时就可以使用动态代理来实现。

例：使用 JDK 动态代理实现 AOP。
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

Spring AOP 中封装了 JDK 和 CGLIB 的动态代理实现，同时引入了 AspectJ 的编程方式和注解，使得可以使用标准的 AOP 编程规范来编写代码外，还提供了多种代理方式选择，可以根据需求来选择最合适的代理模式。而且，Spring 也提供了 XML 配置的方式实现 AOP 配置。可以说是把所有想要的都做出来了，因此，Spring 是在平时编程中使用动态代理的不二选择。

#### 4.6.2. 数据库连接池

#### 4.6.3. 事务管理

#### 4.6.4. 单元测试动态 mock 对象

## 5. 适用场景

代理模式的共同优点如下：
- 能够协调调用者和被调用者，在一定程度上降低了系统的耦合度。
- 客户端可以针对抽象主题角色进行编程，增加和更换代理类无须修改源代码，符合开闭原则，系统具有较好的灵活性和可扩展性。

代理模式的主要缺点如下：
- 由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢，例如保护代理。
- 实现代理模式需要额外的工作，而且有些代理模式的实现过程较为复杂，例如远程代理。

代理模式的类型较多，不同类型的代理模式有不同的优缺点，它们应用于不同的场合：
- 当客户端对象需要访问远程主机中的对象时，可以使用**远程代理**。
- 当需要用一个消耗资源较少的对象来代表一个消耗资源较多的对象（例如一个对象需要很长时间才能完成加载），从而降低系统开销、缩短运行时间时，可以使用**虚拟代理**。
- 当需要为某一个被频繁访问的操作结果提供一个临时存储空间，以供多个客户端共享访问这些结果时，可以使用**缓冲代理**。通过使用缓冲代理，系统无须在客户端每一次访问时都重新执行操作，只需直接从临时缓冲区获取操作结果即可。
- 当需要控制对一个对象的访问，为不同用户提供不同级别的访问权限时，可以使用**保护代理**。
- 当需要为一个对象的访问（引用）提供一些额外的操作时，可以使用**智能引用代理**。

## 6. Refer Links

[慕课网：模式的秘密 --- 代理模式](https://www.imooc.com/learn/214)

[轻松学，Java 中的代理模式及动态代理](http://blog.csdn.net/briblue/article/details/73928350)

[Java Reflection（十一): 动态代理](http://ifeve.com/java-reflection-11-dynamic-proxies/)

[cglib 官方教程](https://mydailyjava.blogspot.no/2013/11/cglib-missing-manual.html)

[CGLIB 介绍与原理](https://blog.csdn.net/zghwaicsdn/article/details/50957474)

[AOP 的底层实现 -CGLIB 动态代理和 JDK 动态代理](https://blog.csdn.net/dreamrealised/article/details/12885739)

[代理 10 cglib 和 jdk 动态代理 调用性能测试](https://www.jianshu.com/p/1aaacf92e2cd)

[jdk 和 cglib 简单理解](http://www.cnblogs.com/onlywujun/p/3524690.html)