<!-- <style>img {box-shadow: 0 0 15px #111;}</style> -->

- [Spring Note](#spring-note)
    - [1. Spring 概述](#1-spring-%E6%A6%82%E8%BF%B0)
    - [2. Maven 依赖](#2-maven-%E4%BE%9D%E8%B5%96)
    - [3. Spring 容器](#3-spring-%E5%AE%B9%E5%99%A8)
    - [4. 依赖注入](#4-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
        - [4.1. 注入方式](#41-%E6%B3%A8%E5%85%A5%E6%96%B9%E5%BC%8F)
        - [4.2. IoC Service Provider](#42-ioc-service-provider)
        - [4.3. 资源访问](#43-%E8%B5%84%E6%BA%90%E8%AE%BF%E9%97%AE)
        - [4.4. IoC 容器 BeanFactory](#44-ioc-%E5%AE%B9%E5%99%A8-beanfactory)
        - [4.5. 应用上下文 ApplicationContext](#45-%E5%BA%94%E7%94%A8%E4%B8%8A%E4%B8%8B%E6%96%87-applicationcontext)
    - [5. AOP](#5-aop)
    - [6. SpEL](#6-spel)
    - [7. 配置文件](#7-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
        - [7.1. web.xml](#71-webxml)

# Spring Note

## 1. Spring 概述

Spring 设计的根本理念：简化 java 开发。为降低 java 开发的复杂性，将开发者的关注点便放到了需要实现的业务逻辑上 spring 采取了以下几种关键策略：
- 基于 POJO 的轻量级和最小侵入式编程；
- 通过依赖注入和面向接口实现松耦合；
- 基于切面和惯例进行声明式编程；
- 通过切面和模板减少样板式代码；

Spring 框架实现的核心技术：反射。

## 2. Maven 依赖

使用 maven 创建 SSM 项目 pom.xml 需要配置的依赖：
- spring-core
- spring-beans
- spring-context
- spring-jdbc
- spring-tx
- spring-web
- spring-webmvc
- spring-test
- mybatis
- mybatis-spring

```xml
  <dependencies>
      <!-- 单元测试依赖 -->
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
      <!-- 日志依赖 -->
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.5</version>
      </dependency>
      <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-core</artifactId>
          <version>1.2.1</version>
      </dependency>
      <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>1.2.1</version>
      </dependency>
      <!-- 数据库依赖 -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>6.0.5</version>
          <scope>runtime</scope>
      </dependency>
      <dependency>
          <groupId>c3p0</groupId>
          <artifactId>c3p0</artifactId>
          <version>0.9.1.2</version>
      </dependency>
      <!-- DAO 层依赖 -->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.4.2</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.1</version>
      </dependency>
      <!-- Servlet Web 依赖 -->
      <dependency>
          <groupId>taglibs</groupId>
          <artifactId>standard</artifactId>
          <version>1.1.2</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet.jsp.jstl</groupId>
          <artifactId>jstl-api</artifactId>
          <version>1.2</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
      </dependency>
      <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0.pr1</version>
      </dependency>
      <!-- spring 依赖 -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-beans</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-tx</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>4.3.7.RELEASE</version>
      </dependency>

  </dependencies>
```

使用 ssm 框架的 web 应用 pom.xml 示例：
```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>cn.adibio.wechat</groupId>
    <artifactId>wechat</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>wechat Maven Webapp</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <!-- 单元测试依赖 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- 日志依赖
        <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.5</version>
        </dependency>
        <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-core</artifactId>
          <version>1.2.1</version>
        </dependency>
        <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>1.2.1</version>
        </dependency>-->
        <!--<dependency>-->
        <!--<groupId>commons-logging</groupId>-->
        <!--<artifactId>commons-logging</artifactId>-->
        <!--<version>1.2</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>org.slf4j</groupId>-->
        <!--<artifactId>jcl-over-slf4j</artifactId>-->
        <!--<version>1.7.24</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>ch.qos.logback</groupId>-->
        <!--<artifactId>logback-classic</artifactId>-->
        <!--<version>1.2.1</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>ch.qos.logback</groupId>-->
        <!--<artifactId>logback-core</artifactId>-->
        <!--<version>1.2.1</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>org.logback-extensions</groupId>-->
        <!--<artifactId>logback-ext-spring</artifactId>-->
        <!--<version>0.1.4</version>-->
        <!--</dependency>-->

        <!-- 数据库依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.5</version>
            <scope>runtime</scope>
        </dependency>
        <!--<dependency>-->
        <!--<groupId>c3p0</groupId>-->
        <!--<artifactId>c3p0</artifactId>-->
        <!--<version>0.9.1.2</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>org.apache.commons</groupId>-->
        <!--<artifactId>commons-dbcp2</artifactId>-->
        <!--<version>2.1.1</version>-->
        <!--</dependency>-->
        <!--<dependency>-->
        <!--<groupId>com.jolbox</groupId>-->
        <!--<artifactId>bonecp</artifactId>-->
        <!--<version>0.8.0.RELEASE</version>-->
        <!--</dependency>-->
        <dependency>
            <groupId>com.zaxxer</groupId>
            <artifactId>HikariCP</artifactId>
            <version>2.6.0</version>
        </dependency>

        <!-- DAO 层依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.1</version>
        </dependency>
        <!-- Servlet Web 依赖 -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.0.pr1</version>
        </dependency>
        <!-- json,xml 处理 -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.8.7</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.5</version>
        </dependency>
        <dependency>
            <groupId>dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>1.6.1</version>
        </dependency>
        <dependency>
            <groupId>com.thoughtworks.xstream</groupId>
            <artifactId>xstream</artifactId>
            <version>1.4.9</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.0</version>
        </dependency>

        <!-- spring 依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

    </dependencies>

    <build>
        <finalName>wechat</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.5</version>
            </plugin>
        </plugins>

    </build>
  </project>
```

## 3. Spring 容器

Spring 容器是 Spring 的核心，一切 Spring bean 都存储在 Spring 容器内，并由其通过 IoC 技术管理。Spring 容器也就是一个 bean 工厂（BeanFactory）。应用中 bean 的实例化，获取，销毁等都是由这个 bean 工厂管理的。

org.springframework.context.ApplicationContext 接口用于完成容器的配置，初始化，管理 bean。一个 Spring 容器就是某个实现了 ApplicationContext 接口的类的实例。也就是说，从代码层面，Spring 容器其实就是一个 ApplicationContext。

初始化 spring 容器：
- 在普通的 JAVA 工程中，我们可以通过代码显式 new 一个 ClassPathXmlApplicationContext 或者 FileSystemXmlApplicationContext 来初始化一个 Spring 容器；
  - 通过 ClassPathApplicationContext 初始化 Spring 容器：
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});
    ```
  - 用 FileSystemApplicationContext 来初始化 Spring 容器：
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});
    ```

- 在 Web 工程中，我们一般是通过配置 web.xml 的方式来初始化 Spring 容器：
  - 通过 org.springframework.web.servlet.DispatcherServlet：

  - 通过 org.springframework.web.context.ContextLoaderListener：
    ```xml
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml classpath:resources/services.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    ```
    contextConfigLocation：contextConfigLocation 指的是 Spring 该去哪里读取配置文件，ContextLoaderListener 用于启动的 web 容器（如 tomcat）时，去读取配置文件并完成 Spring 容器的初始化（包括加载 bean 等）。

    关于 contextConfigLocation 的配置方式也是可以非常丰富的，还可以使用通配符 * ，这里简单举几个例子说明：
    - classpath:resources/services.xml：表示到 web 工程的 classes/resources 文件夹中查找配置文件；
    - classpath*:resources/services.xml：这种方式除了会到 classes/resources 文件夹查找，还会到 lib 下面的 jar 包中查找，查找路径是 jar 包内 /resources/services.xml；
    - classpath:resouces/**/*services.xml：这种方式表示到 classpath 的 resources 文件夹下所有文件夹（不限层级，可以在第 N 层子文件夹中）中查找文件名以 services.xml 结尾的文件；
    - 多个路径配置可以用空格分开；
    
    classpath 知识补充：
    - web 工程部署后，对应 war 包下的 WEB-INF 下会有一个 classes 文件夹和一个 lib 文件。其中 classes 文件夹中的内容是从工程中的源码文件夹（对应右键工程，Properties - Java Build Path - Source 页签中看到的文件夹）中编译过来，lib 文件夹即工程中引用的 jar 包。这个 classes 文件夹和 lib 中的 jar 都属于 classpath。

    ContextLoaderListener：
    - 这个 Listener 就是在标准 Spring Web 工程中 Spring 开始干活的切入点，ContextLoaderListener 实现了 ServletContextListener，所以在 web 容器启动时，ContextLoaderListener 就悄悄开始工作了。

## 4. 依赖注入

依赖注入 DI/IOC 本质就是要抛弃 new 的方法取得对象，通过配置来取得对象。

### 4.1. 注入方式

- 构造函数注入
- 属性注入（通过 setter 方法）
- 接口注入（弃用）

### 4.2. IoC Service Provider

### 4.3. 资源访问

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/27/fd7e2fd6a27db2a4b44a5c2fcc1d9e0d.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/27/55b25012f6f1b165b3cada047e5bcf19.jpg)

### 4.4. IoC 容器 BeanFactory

### 4.5. 应用上下文 ApplicationContext

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/27/941a9f063460dc39653ff52d58bf3e24.jpg)

## 5. AOP

## 6. SpEL
SpEL 的目的是通过计算来获取某个值，#{}标记会提示 Spring 这个标记内的是 SpEL 表达式。

通过注解，可以将 SpEL 从 xml 文件中分离，在基于注解驱动的装配中使用 SpEL。

- 使用 Bean 的 id 来引用某个 Bean
- 调用方法和访问对象的 field
- 对值进行算术、关系和逻辑运算
- 正则表达式匹配
- 集合操作

## 7. 配置文件

按照官方文档，spring web 项目的配置文件应有：http://jiage17.iteye.com/blog/2312456 
- WEB-INF/web.xml
- WEB-INF/<dispatcherServlet_name>-servlet.xml（配置 spring mvc）
- WEB-INF/applicationContext.xml（配置 spring framework）

### 7.1. web.xml

- 加载 spring 配置文件
  
  Web 容器上下文通过指定 spring 配置文件的地址来加载 spring 配置文件。多个配置文件用逗号或空格分隔（建议采用逗号）；
  若使用日志管理，需要将 loh4j.properties 配置文件放在类路径下，以便日志引擎自动生效；
  ```xml
  <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>
          classpath:spring/spring-*.xml// 若不指定 spring 配置文件命名，默认加载 classpath 根目录下的 ApplicationContext.xml
      </param-value>
  </context-param>
  <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  ```

- 配置 SpringMVC 的前端控制器 DispatcherController
  ```xml
  <servlet>
      <servlet-name>mvc-dispatcher</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <load-on-startup>1</load-on-startup>
      <init-param>// 若不指定此参数，默认加载 classpath 根目录下的 xxx-dispatcher-servlet.xml
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:xxxx.xml</param-value>
      </init-param>
  </servlet>
  <servlet-mapping>
      <servlet-name>mvc-dispatcher</servlet-name>
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```

- 关于<url-pattern>的规则问题：

  https://www.cnblogs.com/fangjian0423/p/servletContainer-tomcat-urlPattern.html 

  http://www.voidcn.com/blog/javaloveiphone/article/p-6190045.html 

  - 可以看到路径分成 4 类：
    - 以 /* 结尾的
      
      会匹配所有 url：路径型的和后缀型的 url（包括 /login,.jsp,.js 和*.html 等)

    - 以 *. 开头的

    - /

      会匹配到 /login 这样的路径型 url，不会匹配到模式为*.jsp 这样的后缀型 url

    - 以上 3 种之外的

  - `/`：访问的地址为 localhost:8088/login , /login 返回 login.jsp ，访问去 Controller 下的 /login 跳转到相应的视图 login.jsp 
  - `/*`：访问的地址为 localhost:8088/login/ ，/login 返回 login.jsp ，访问去 Controller 下的 /login, 跳转到 login.jsp ，然后进过 dispatchservlet 的时候，由于是 /* ，又会以 localhost:8088/login/login.jsp 去请求 Controller , 那么如果 Controller 没有 /login/login.jsp 的 Mapping 映射，则会报 404 错误！
  
  <!-- TODO: 使用 /*是错的？-- 官网很多例子都是用 /*？ -->

当有客户端向服务器发起请求，服务器进行 url 匹配的时候是有优先级的。 以下从上到下以优先级的高低进行说明：
- 规则 1：精确匹配，使用 contextVersion 的 exactWrappers；
- 规则 2：前缀匹配，使用 contextVersion 的 wildcardWrappers；
- 规则 3：扩展名匹配，使用 contextVersion 的 extensionWrappers；
- 规则 4：使用资源文件来处理 servlet，使用 contextVersion 的 welcomeResources 属性，这个属性是个字符串数组；
- 规则 7：使用默认的 servlet，使用 contextVersion 的 defaultWrapper；

/*与 / 的区别在于：
- / 不会拦截 jsp 视图，而 /*会拦截。

- /*的时候无法加载默认首页 index；/ 可以；
