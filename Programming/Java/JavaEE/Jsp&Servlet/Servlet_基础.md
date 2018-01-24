- [Servlet 基础](#servlet-%E5%9F%BA%E7%A1%80)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [优势](#%E4%BC%98%E5%8A%BF)
    - [Servlet 类谱图](#servlet-%E7%B1%BB%E8%B0%B1%E5%9B%BE)
    - [生命周期](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [Servlet 编写](#servlet-%E7%BC%96%E5%86%99)
    - [步骤](#%E6%AD%A5%E9%AA%A4)
    - [内置对象](#%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)
  - [过滤器](#%E8%BF%87%E6%BB%A4%E5%99%A8)
  - [监听器](#%E7%9B%91%E5%90%AC%E5%99%A8)
  - [其它问题](#%E5%85%B6%E5%AE%83%E9%97%AE%E9%A2%98)
    - [路径问题](#%E8%B7%AF%E5%BE%84%E9%97%AE%E9%A2%98)
    - [关于斜线“/”的总结：](#%E5%85%B3%E4%BA%8E%E6%96%9C%E7%BA%BF%E2%80%9C%E2%80%9D%E7%9A%84%E6%80%BB%E7%BB%93%EF%BC%9A)
  - [Refer Links](#refer-links)

# Servlet 基础

## 概述

Java Servlet 是运行在带有支持 Java Servlet 规范的解释器的 web 服务器上的 Java 类，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层，可以通过“请求 - 响应”的编程模型来访问这个驻留在服务器内存中的 Servlet 程序；

Servlet 可以使用 javax.servlet 和 javax.servlet.http 包创建，它是 Java 企业版的标准组成部分；

### 优势

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果。但是相比于 CGI，Servlet 有以下几点优势：

- 性能明显更好；

- Servlet 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求；

- Servlet 是独立于平台的，因为它们是用 Java 编写的；

- 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的；

- Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互；

### Servlet 类谱图

Servlet 是服务 HTTP 请求并实现 javax.servlet.Servlet 接口的 Java 类。Web 应用程序开发人员通常编写 Servlet 来扩展 javax.servlet.http.HttpServlet，并实现 Servlet 接口的抽象类专门用来处理 HTTP 请求；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/057b09ff710d3c7c669c23c79ab7614b.jpg)

附：Servlet 类主要接口：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/e5302925b3ad9e0f53738ca69b485279.jpg)

### 生命周期

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/80e6f7d3e862237d2370d80316ce4cec.jpg)

1. 加载
    
    调用构造方法（从 Servlet 类构造方法开始依次调用各个父类的构造方法），创建实例对象；
    
    Servlet 容器开始加载 servlet 的时刻：
    - 服务器启动时，自动加载和初始化在 WEB.xml 中定义自动加载的 servlet；
    
    - 当被客户端第一次请求时，加载被请求的 servlet；
    - 已被加载过的 servlet 的类文件源代码被更新后，会重新加载 servlet；

1. 初始化
    
    调用 init() 方法（定义于 javax.servlet.Servlet 接口中），将 servlet 程序加载到内存中，此后常驻内存直至服务器关闭，因此初始化方法 init() 只调用一次；

1. 服务
    
    对于每一次向 servlet 提交的 request，在 web.xml 中的<url-pattern>标签找到请求 url 对应的<servlet-name>，再找到<servlet-name>对应的<servlet-class>，调用 HttpServlet 类方法
    ```java
    void service(HttpServletRequest req, HttpServletResponse resp) 
    throws ServletException, IOException
    // 定义于 javax.servlet.Servlet 接口中
    ```
    来判断 request method，进而选择调用指定 servlet-class 中的 doGet(request, response) /doPost(request, response) /doHead(request, response) /doPut(request, response) /doDelete(request, response) /doOptions(request, response) /doTrace(request, response) 方法，在这一系列调用过程中将参数 request 和 response 依次传递；
    
    流程图如下：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/5133369a83f0687abfcf59dd96cdbf09.jpg)

1. 销毁
    
    服务器关闭时，调用 destroy() 方法；

## Servlet 编写

### 步骤

1. 编写 java 类：
    - 继承 javax.servlet.http.HttpServlet；
    - override doGet() 或 doPost() 方法；
    
    例：
    ```java
    import java.io.*;
    import javax.servlet.*;
    import javax.servlet.http.*;

    public class HelloWorld extends HttpServlet {
    private String message;

        public void init() throws ServletException {
            message = "Hello World";
            // 自定义初始化操作
        }

    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 注意：doXxx 方法都需要抛出 ServletException 和 IOException，而不需要抛出 HttpServletException
            // 设置响应内容类型
    response.setContentType("text/html");
            // 实际的逻辑是在这里
            PrintWriter out = response.getWriter();
            out.println("<h1>" + message + "</h1>");
    }
      
    public void destroy() {
          // 自定义销毁操作，此处什么也不做
        }
    }
    ```
    当采用 POST 方法请求时，通常在 doGet() 方法中只写 doPost(request,response)，然后把业务逻辑的处理写在 doPost 方法中；

1. 编译 java 类，并将编译得到的 class 字节码文件放在 WEB-INF/classes/package 结构 /class 文件中；（若使用 Eclipse 之类的 IDE，这一步由 IDE 自动完成）

1. 在 web.xml 中注册 Servlet：
    
    在`<web-app>`的标签体中添加（非加粗内容为可选项）：
    ```xml
    <servlet>
      <description>This is the description of my J2EE component</description>
      <display-name>This is the display name of my J2EE component</display-name>
      
    <servlet-name>HelloWorld</servlet-name>// 定义 servlet 名，在 web.xml 中是 servlet 的唯一标识
    <servlet-class>HelloWorld</servlet-class>// 指定实现该 servlet 的 java 类

    <init-param>
      <param-name>username</param-name>
      <param-value>admin</param-value>
    </init-param>
    <init-param>
      <param-name>password</param-name>
      <param-value>123456</param-value>
    </init-param>
    // 配置初始化参数，在 servlet 中可通过 ServletConifg 接口提供的方法 getInitParameter(“xxx”) 取得这些参数
    </servlet>
    <servlet-mapping>
    <servlet-name>HelloWorld</servlet-name>// 必须与上边定义的 servlet 名相对应
    <url-pattern>/HelloWorld/</url-pattern>
    // 指定访问该 servlet 的 url，注意：
    // 此处的 url-pattern 地址中最前边需要加“/” （表示项目根目录），最后边也要加“/”（表示路径结束），否则 tomcat 服务器无法正常启动；
    // 合法的 url-pattern：/test.abcd，/test/*
    // 不合法的 url-pattern：/test，test/，test
    </servlet-mapping>
    ```

### 内置对象

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/8137a19614dcc4a48342ee58af7a2fe7.jpg)

## 过滤器
http://www.runoob.com/jsp/jsp-writing-filters.html

http://www.jellythink.com/archives/1409

过滤器是一种服务器端组件，可以截取用户端的请求和响应信息，并处理这些信息实现过滤的目的；

过滤器是可用于 Servlet 编程的 Java 类，可以实现以下目的：
- 在客户端的请求访问后端资源之前，拦截这些请求。
- 在服务器的响应发送回客户端之前，处理这些响应。
- 根据规范建议的各种类型的过滤器：
- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

过滤器通过 Web 部署描述符（web.xml）中的 XML 标签来声明，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为您在部署描述符中声明的每一个过滤器创建一个实例。

Filter 的执行顺序与在 web.xml 配置文件中的配置顺序一致，一般把 Filter 配置在所有的 Servlet 之前

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/89dc131ac6beec3c385ec3b9000467e2.jpg)

启动 tomcat 容器时，自动调用 init 方法，加载 filter；

当客户端每次对 filter 绑定的 url 发起请求时，调用 doFilter 方法；

关闭服务器时，调用 destroy 方法；

创建过滤器：
1. 在 src 中新建类，实现 filter 接口；
1. 在 web.xml 中添加刚刚编写的 filter 类，并绑定 url；
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/04ecb4d38a4074db0ba91facedeeda9f.jpg)

过滤器可改变用户请求的资源，即可改变客户端请求的 url；

过滤器不能直接处理用户请求并返回响应 /web 资源，只能重定向到其它 web 资源，由其它资源处理；

可定义多个 filter，若多个 filter 绑定的 url 互不相同，则各自工作；若有多个 filter 绑定的 url 是相同的，则会组成过滤器链：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/45e58e3a26c1047d6c3301c82a31dec2.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/d62a05c9e8aaf14a5f835940cb6f18ad.jpg)（Chain.doFilter() 即放行）

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/2791f3a6cc3bb41776bfd3a26c5be21f.jpg)

当请求过滤器 request filter 使得访问可能发生死循环时：
- 若采用得是重定向 sendRedirect 方法，走的是 request 路线，在客户端处理，因此会造成死循环；若采用的是转发 getRedirectDispatch(“main.jsp”).forward(req,res) 方法，则走是的 forward 路线，在服务器端处理，因此不会造成死循环；
- 若采用的是 forward 过滤器，则会；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/a9350edfeb8c94d20933e31b62f3a2ba.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/2beebf4d357657067135745191b67b62.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/488871fa579465a819256a1e8cbc0359.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/4e88e9a31e0239f901b9e137474a4e49.jpg)

## 监听器

http://www.jellythink.com/archives/1414

监听器是一种上下文对象，web 应用创建时创建（调用 contextInitialized 方法），web 应用销毁时销毁（调用 contextDestory 方法）；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/128143f9f7d7a16f344d4efc0abd348e.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/745d9f6e76b36e114357b8a9cdefb6d7.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/e590f09b66fc26ebd08d98219a13b220.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/a73726cbb58a4ae48a5bcc03cb9581ea.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/f9a7fa307c3eb4886f57dce13090317c.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/46d1905c2430635c147f4113c5fa5cc2.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/09ae0ed8326f842857f49ee5cf1fb4cc.jpg)

## 其它问题

### 路径问题

JSP 文件与 Servlet 的相互访问：
- 在 jsp 文件中访问 servlet：
  
  例：在 index.jsp 中访问 src 中的 servlet——Test 类：
  - 使用相对路径
    ```html
    <a href=”servlet/Test”>点击访问</a>
    ```
    注意：
    - “servlet/Test“是在 WEB.xml 中与 Test 绑定的虚拟目录（URL），此时实际访问的完整 url 为：http://localhost:8080/ 项目名 /servlet/Test；
    - 若写成“/servlet/Test”，则实际访问 url 变成：http://localhost:8080/servlet/Test，无法成功访问，因为“/”表示服务器的根目录；
  - 使用绝对路径：
    ```jsp
    <% String path = request.getContextPath(); %>// path 得到当前的上下文地址，即当前项目的根目录
    <a href=”<%=path%>/servlet/Test”>点击访问</a>// 实际访问 url 为：http://localhost:8080/ 项目名 /servlet/Test，注意这种表示方法 servlet 前要用“/”与项目根目录连接
    ```

- 在 servlet 中访问 jsp 文件：
  
  例：在 package/Test 这个 servlet 中访问 index.jsp：
  - 采用重定向的方式：
    ```java
    request.sendRedirect(request.getContextPath()+“/index.jsp”);
    ```
    注意：
    - 若写成 request.sendRedirect(“index.jsp”) 是错误的，此时该方法会在当前目录下寻找 index.jsp 文件，而当前目录是 package/Test（注意是在 WEB.xml 中与当前 servlet 绑定好的虚拟目录，而不是实际物理目录 src / package / Test.java ），当然找不到 index.jsp 文件，404 错误；
    - 在请求重定向中，“/”表示服务器根目录，因此也可写成：request.sendRedirect(“/ 项目名 /index.jsp”)；<!-- TODO:——未验证 -->
    
  - 采用请求转发（服务器内部跳转）的方式：
    - 采用绝对路径：
      ```java
      request.getRequestDispatcher(“/index.jsp”).forward(request,response);
      ```
      注意：
      - 在请求转发中，“/”表示项目根目录；
      - 若写成”index.jsp”，只在当前目录下寻找 index.jsp 文件，因此返回 404 错误；
    - 采用相对路径：
      ```java
      request.getRequestDispatcher(“../index.jsp”).forward(request,response);
      ```
      注意：当前目录为 package/Test，因此。./ 回到上一级目录，即项目根目录；<!-- TODO:——未验证 -->

### 关于斜线“/”的总结：

- jsp 页面中的 / 代表服务器根目录；

- WEB.xml 中 / 代表项目根目录；

- java 文件中请求重定向的 / 代表服务器根目录；

- java 文件中请求转发的 / 代表项目根目录

## Refer Links

官方 document: https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/Servlet.html
