- [JSP 生命周期](#jsp-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [JSP 文件与 Servlet 的关系](#jsp-%E6%96%87%E4%BB%B6%E4%B8%8E-servlet-%E7%9A%84%E5%85%B3%E7%B3%BB)
  - [JSP 文件转换为 Servlet 的过程](#jsp-%E6%96%87%E4%BB%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA-servlet-%E7%9A%84%E8%BF%87%E7%A8%8B)
  - [服务端处理 JSP 请求的过程 /JSP 生命周期](#%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%A4%84%E7%90%86-jsp-%E8%AF%B7%E6%B1%82%E7%9A%84%E8%BF%87%E7%A8%8B-jsp-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [编译阶段](#%E7%BC%96%E8%AF%91%E9%98%B6%E6%AE%B5)
    - [初始化阶段](#%E5%88%9D%E5%A7%8B%E5%8C%96%E9%98%B6%E6%AE%B5)
    - [执行阶段](#%E6%89%A7%E8%A1%8C%E9%98%B6%E6%AE%B5)
    - [销毁阶段](#%E9%94%80%E6%AF%81%E9%98%B6%E6%AE%B5)
    - [验证实验](#%E9%AA%8C%E8%AF%81%E5%AE%9E%E9%AA%8C)

# JSP 生命周期

## JSP 文件与 Servlet 的关系

http://www.webkaka.com/blog/archives/the-location-of-jsp-java-file.html

- **JSP 是 Servlet 的扩展，在没有 JSP 之前，就已经出现了 Servlet 技术**。**Servlet 是利用输出流动态生成 HTML 页面**，包括每一个 HTML 标签和每个在 HTML 页面中出现的内容；但在 Servlet 中嵌入大量的静态文本格式，导致 Servlet 的开发效率低，代码难维护，而 JSP 就是为了解决这个问题而出现的：**JSP 通过在标准的 HTML 页面中插入 Java 代码，其静态的部分无须 Java 程序控制，只有那些需要从数据库读取并根据程序动态生成信息时，才使用 Java 脚本控制**；

- **JSP 是 Servlet 的一种特殊形式，每个 JSP 页面就是一个 Servlet 实例——JSP 页面由系统编译成 Servlet，Servlet 再负责响应用户请求**。在运行时 jsp 接受请求开始编译，如果 jsp 文件已经更新或者被生成的 servlet 文件被删除，则会重新编译生成；，如果 jsp 文件没有发生变化，则直接使用编译好的 class 文件；

  验证实验：在 XX.jsp 生成的 xx_jsp.java 中可以看到：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/96fc7afcb118813959731b31278f55af.jpg)
  
  `xx_jsp`继承自`org.apahce.jasper.runtime.HttpJspBase`类，而 HttpJspBase 类继承自 HttpServlet 类（所有 Servlet 都需要继承 HttpServlet 类），因此，`hello_jsp`类本质上也是一个 servlet；

- 每个 JSP 页面的第一个访问者速度很慢，因为必须等待 JSP 编译成 Servlet；

- JSP 页面的访问者无须安装任何客户端，甚至也不需要可以运行 Java 的运行环境，因为 JSP 页面输送到客户端的是标准 HTML 页面；

- JSP 编译生成的 java 文件和 class 文件默认位置在：
  
  `{Tomcat 根目录}\work\Catalina\localhost\ 项目名、org\apache\jsp\`；
  
  test.jsp 会编译生成`test1_jsp.java`和`test1_jsp.class`；

## JSP 文件转换为 Servlet 的过程

转换过程示意图：

[![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/d432066f6f800bd95fbe332c22acb76f.jpg)](https://pic1.zhimg.com/57894e9d5fb4f55b0f13506dcc69fb34_b.png)

- JSP 页面的静态内容（HTML 代码） —— 以 out.write(“xxxx”) 插入到类 xx_jsp 的_jspService 方法（类似于自行创建 Servlet 时 service() 方法） 中；
  
  例：由 x.jsp 生成的 x_jsp.java 中的 `_jspService`方法的部分代码：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/b194cab60da82cb3b89d2bdd53ed6231.jpg)

- JSP 声明（即<%! %>中）的变量和方法 —— 以 field 或 method 插入到类 xx_jsp 中；
  
  因此 JSP 声明可以使用 private,protected,public,static 等修饰符，其他地方则不行；

- JSP 脚本（即<% %>）中的代码 —— 以源代码的形式插入到类 xx_jsp 的_jspService 方法的实现中（JSP 脚本里声明的变量作为_jspService 方法的局部变量）；
  
  因此<%   %>里不能声明方法，不能使用 private,protected,public,static 等修饰符； 

- JSP 输出表达式 （即<%= %>） —— 以 out.print(xxx) 插入到类 xx_jsp 的_jspService 方法中；
  
  例：<%=5+5%>，则在类 xx_jsp 的_jspService 方法的对应位置中插入一行 out.print(5+5);

- JSP 指令（即<%@ %>） —— 以属性、方法、赋值的形式设置到类 xx_jsp 中；

- JSP 动作标签（tablib） —— 

- JSP 注释 —— 
  
  <%-- --%>中的注释只在 JSP 文件中有效，编译成 java 文件后便会被抛弃；
  
  <% //xxxx %>中的注释在 JSP 文件编译成 java 文件后会作为注释插入到_JspService 方法中：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/1a0761d26ab3d8e20b17fa720a9740d6.jpg)

P.S. 

- 如何在 Tomcat 下指定 Jsp 生成的 Java 文件路径？

  当需要自定义 Jsp 生成的 Java 文件位置时，可以通过如下两种方法来实现：

  - 方法 1
    
    在 tomcat 的配置文件 server.xml（路径：tomcat 路径＼conf 下面）里，找到：
    ```xml
    <Context docBase="D:\workspace\icinfo\trunk\web" path="" reloadable="false" debug="0" crossContext="true" workDir="D:\workspace\icinfo\trunk\web\WEB-INF\lib\CommonPKI\META-INF\work"/>
    ``` 
    添加如上的 workDir＝" "属性，＂＂里写你的要看到。java/.class 的路径。

  - 方法 2
    
    到 conf\Catalina\localhost 下找到你项目的．xml 配置文件，找到方法 1 中的代码，后续操作同方法 1。

- 怎样保留 Weblogic 中 Jsp 编译后生成的 Java 文件？

  运行自己配置的 web 应用，往往只能看见 weblogic 编译之后的 class 文件。而看不见编译前的 java 的文件。为了调试方便，我们有时候是想看编译前的 java 文件的。
  
  在 weblogic.xml 中加入：
  ```xml
  <jsp-descriptor>
    <jsp-param>
      <param-name>keepgenerated</param-name> 
      <param-value>true</param-value>
    </jsp-param>
  </jsp-descriptor>
  ```
  如果没有 weblogic.xml 文件，在 WEB-INF 中建立一个 weblogic.xml 文件：
  ```xml
  <?xml version="1.0" encoding="UTF-8"?> 
  <!DOCTYPE weblogic-web-app 
    PUBLIC "-//BEA Systems, Inc.//DTD Web Application 8.1//EN" 
    "http://www.bea.com/servers/wls810/dtd/weblogic810-web-jar.dtd" >
  <weblogic-web-app>
      <jsp-descriptor>
          <jsp-param>
              <param-name>keepgenerated</param-name>
              <param-value>true</param-value>
          </jsp-param>
      </jsp-descriptor>
  </weblogic-web-app>
  ```
  在 Weblogic 中生成临时文件中即可看到编译前的 java 文件。

## 服务端处理 JSP 请求的过程 /JSP 生命周期

JSP 生命周期的几个阶段：

### 编译阶段

由 servlet 容器（如 tomcat）编译 JSP 文件成 servlet 源文件（java 文件），生成 servlet 类（class 文件），在编译过程中如果发生错误，会立刻中止，同时向服务器端和客户端发送错误信息报告；

当客户端浏览器请求 JSP 页面时，JSP 引擎会首先去检查是否需要编译这个文件。如果这个文件没有被编译过，或者在上次编译后被更改过，则编译这个 JSP 文件；

### 初始化阶段

初始化过程在 jsp 页面第一次被访问时执行，且只执行一次，JSP 引擎会按以下顺序调用初始化方法；

1. 调用 xx_jsp 类的父类（HttpJspBase）的父类（HttpServlet 类）的 init 方法（所有 Servlet 都会调用），加载与 JSP 对应的 servlet 类，创建其实例；

1. 调用用户自定义的初始化方法 jspInit()（通常情况下在初始化中初始化数据库连接、打开文件和创建查询表）；
    ```java
    public void jspInit () {// 用户自定义的初始化代码}
    ```

1. 调用服务器端自动生成的初始化方法_jspInit()；
    ```java
    public void _jspInit () {// 自动生成的初始化代码，不可 override}
    ```

**注意：一般以下划线“_”开头命名的方法，都是系统自动生成不可 override 的。**

### 执行阶段

当 JSP 网页完成初始化后，JSP 引擎会将编译的都的 Servlet 程序加载到内存（并常驻内存），对客户端的每一个请求，会开启一个线程调用_jspService() 方法，将响应以静态形式（HTML）返回；

```java
public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException { 
  // 服务器根据 JSP 文件和配置自动生成的服务端处理代码，不可 override
}
```
_jspService() 方法在每个 request 中被调用一次并且负责产生与之相对应的 response，并且它还负责产生所有 7 个 HTTP 方法的回应，比如 GET、POST、DELETE 等；

对每一个请求 Servlet 程序都会创建一个线程去调用_jspService() 方法，如果同时有多个请求需要处理的话就会创建多个线程，以多线程的方式降低对系统的资源需求，提高系统并发量及响应时间，但也应注意多线程带来的同步问题。由于 servlet 长期贮存与内存中，所以执行速度比较快；

这一阶段描述了 JSP 生命周期中一切与请求相关的交互行为，直到被销毁；

### 销毁阶段

<!-- TODO: 什么时候会执行销毁？）由于某种原因导致 jsp 网页关闭或者销毁的话 / 服务器关闭？ -->

调用与 JSP 对应的 servlet 实例的销毁方法，销毁 servlet 实例，JSP 引擎会按以下顺序调用销毁方法：

1. 调用用户自定义的 jspDestory 方法（通常情况下在销毁 JSP 实例时释放数据库连接或者关闭文件夹等）：
		```java
    public void jspDestroy() {// 自定义清理代码}
    ```
1. 调用服务器端自动生成的销毁方法_jspDestory()；
    ```java
    public void _jspDestroy () {// 自动生成的销毁代码，不可 override}
    ```

该阶段描述了当一个 JSP 网页从容器中被移除时所发生的一切；

### 验证实验

http://www.xlgps.com/article/386296.html 

- xx.jsp 编译生成 xx_jsp.java 和 xx_jsp.class；
- 继承关系：
  ```java
  public final class xx_jsp extends org.apache.jasper.runtime.HttpJspBase；
  public abstract class HttpJspBase extends HttpServlet implements HttpJspPage；
  ```
- 证明材料：org.apache.jasper.runtime.HttpJspBase 类的部分定义：
  ```java
  @Override
  public final void init(ServletConfig config) throws ServletException //init 为 final，不允许被覆写
  {
      super.init(config); 
      // 调用父类 HttpServlet 的 init，设置 ServletConfig，这也是 xx_jsp.class 的_jspInit 中能够调用 getServletConfig 的原因
      jspInit();
      _jspInit();
  }
  @Override
  public final void destroy() { //destroy 为 final，不允许被覆写
      jspDestroy();
      _jspDestroy();
  }
  @Override
  public final void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { //service 为 final，不允许被覆写
      _jspService(request, response);
  }

  @Override
  public void jspInit() { // 负责 jsp 的初始化
  }
  public void _jspInit() { // 由 tomcat 自动生成，jsp 文件中不允许被覆写
  }
  @Override
  public void jspDestroy() { // 负责 jsp 的销毁
  }
  protected void _jspDestroy() { // 由 tomcat 自动生成，jsp 文件中不允许被覆写
  }
  @Override
  public abstract void _jspService(HttpServletRequest request, HttpServletResponse response) 
      throws ServletException, IOException; // 由 tomcat 自动生成，抽象方法，jsp 文件中不允许被覆写
  ```
