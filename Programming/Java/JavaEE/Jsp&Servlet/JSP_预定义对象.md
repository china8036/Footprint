- [JSP 预定义对象](#jsp-%E9%A2%84%E5%AE%9A%E4%B9%89%E5%AF%B9%E8%B1%A1)
  - [1. application](#1-application)
  - [2. config](#2-config)
  - [3. exception：](#3-exception%EF%BC%9A)
  - [4. out：](#4-out%EF%BC%9A)
  - [5. page](#5-page)
  - [6. pageContext](#6-pagecontext)
  - [7. request](#7-request)
  - [8. response](#8-response)
  - [9. session](#9-session)

# JSP 预定义对象

在 JSP 脚本中包含九个内置对象，这九个内置对象都是 Servlet API 接口的实例，在客户端向 jsp 页面发起请求时，由 jsp 页面对应 Servlet 类的_jspService 方法自动创建了这些实例；

在 x.jsp 转换成的 x_jsp.java 的_jspService 方法中，可以看到九大预定义对象的定义：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/82996b4a0364aec34e8c0feaf348aa23.jpg)

即：
```java
final javax.servlet.http.HttpServletRequest request;（作为_jspService() 的参数定义)
final javax.servlet.http.HttpServletResponse response; （作为_jspService() 的参数定义)
final javax.servlet.jsp.PageContext pageContext; （作为_jspService 方法的局部参数）
javax.servlet.http.HttpSession session = null; （作为_jspService 方法的局部参数）
final javax.servlet.ServletContext application; （作为_jspService 方法的局部参数）
final javax.servlet.ServletConfig config; （作为_jspService 方法的局部参数）
javax.servlet.jsp.JspWriter out = null; （作为_jspService 方法的局部参数）
final java.lang.Object page = this; （作为_jspService 方法的局部参数）
java.lang.Throwable Exception；
```
注意：正因为预定义对象都是在_jspService 方法中定义的，因此，在“<% %>”或“<%= %>”或 jsp 预定义的动作标签中可以使用预定义对象，而在<%@ %>中不可使用；

## 1. application

- 定义：
  ```java
  final javax.servlet.ServletContext application = pageContext.getServletContext(); 
  ```
  另：“interface ServletContext”，因此不能直接 new；

- 在每个 web 应用中只能有唯一一个 javax.servlet.ServletContext 类的对象，在某个 jsp 页面第一次被访问后 application 对象被创建，之后在任意 jsp 页面中可通过 application 访问该类的实例，在任意 java 文件中通过以下方式访问该类实例：
  ```java
  ServletContext sc = getServletConfig().getServletContext();
  //sc 即是 ServletContext 类的实例，且与 jsp 文件中的内置对象 application 是同一个对象；
  ```
	直到服务器关闭，该类实例才被销毁；可类比于 javase 中的静态类。

- 常用方法：
  
  - void setAttribute(String attrName, Object value)：创建全局属性并赋值（可将任意对象绑定为全局属性)；
    
    注：
    
    在 application 作用域中设置的属性如果不手动调用 removeAttribute 函数进行删除的话，那么 application 中的属性将永远不会删除，如果 Web 容器发生重启，此时 application 范围内的所有属性都将丢失；
  
  - Object getAttribute(String attrName)：获取指定的全局属性值；
    
    例：
    
    在 1.jsp 中：
    ```jsp
    <% application.setAttribute(“webinfo”, “I am test.”); %>
    ```
    在 2.jsp 中：
    ```jsp
    <%= application.getAttribute("Webinfo") %>
    ```
    在 3.java 中：
    ```jsp
    PrintWriter out = response.getWriter();
    ServletContext sc = getServletConfig().getServletContext();// 获取上下文
    out.println(sc.getAttribute("Webinfo")); // 获得参数值
    ```
    注意：若在访问 1.jsp 之前访问 2.jsp 或者在其它 jsp 或 Servlet 中访问 webinfo 的值，其值都为 null，因为 1.jsp 还未被访问过，全局属性“webinfo“还未设置；
  
  - Enumeration getAttributeNames()：返回所有可用的属性名的枚举；

  
  - Object getParameter(String attrName)：获取 Web 应用的配置参数（即 web.xml 中的 param 值）；

    例：在 web.xml 中配置全局参数：
    ```xml
    <context-param>
        <param-name>webinfo</param-name>
        <param-value>” I am test.” </param-value>
    </context-param>
    ```
    在 JSP 中可以通过如下代码获取全局配置参数：
    ```xml
    <%= application.getInitParameter("Webinfo") %>
    ```  
    在 Servlet 中：
    ```java
    ServletContext sc = getServletConfig().getServletContext();// 获取上下文
    String webinfo = sc.getInitParameter(“webinfo”);
    ```

  - Object getInitParameter(String name);
  
  - String getServletInfo()：返回 JSP/Servlet 引擎名和版本号；

- 应用：
  - application 对象代表 Web 应用本身，具有全局操作性，可以在整个 Web 应用的多个 JSP、Servlet 之间共享数据；
  - application 可以从 web.xml 中读取全局配置文件 web.xml，可以访问 Web 应用的全局配置参数；

## 2. config

- 定义：
  ```
  final javax.servlet.ServletConfig config = pageContext.getServletConfig();
  ```
  另，“interface ServletConfig“，因此不能直接 new；

- ServletConfig 类封装了当前类在 web.xml 中配置的初始化参数（在一个 Servlet 初始化时，JSP 引擎向它传递参数会被调用），包括了 Servlet 类初始化所需要的参数和服务器的有关信息；

- 在 JSP 中可以通过预定义的 config 对象来获取这些参数值；
  
  在 java 文件中没有预定义对象 config，但可以通过调用 getServletConfig 函数获得 ServletConfig 类的实例化对象：
  ```
  ServletConfig conf = getServletConfig();
  ```

- 常用方法：
  - ServletContext getServletContext()：返回含有服务器信息的 ServletContext 对象；
  - String getInitParameter(String attrName)：返回 web.xml 中当前 jsp 页面 /Servlet 类初始化参数的值；
    例 1：在 web.xml 中对 Jsp 文件所生成的 Servlet 进行配置：
    ```xml
    <web-app ...>
        <!-- 根据 JSP 页面，定义一个 servlet -->
        <servlet>
            <servlet-name>ConfigDemo</servlet-name>
            <jsp-file>/page1.jsp</jsp-file>

            <init-param>
                <param-name>Website</param-name>
                <param-value>http://www.test.com</param-value>
            </init-param>
        </servlet>

        <!-- JSP 与 URL 的映射，可以在浏览器地址栏输入 url-pattern 配置的值访问页面 -->
        <servlet-mapping>
            <servlet-name>ConfigDemo</servlet-name>
            <url-pattern>/config</url-pattern>
        </servlet-mapping>
    </web-app>
    ```
    ps：配置 JSP 页面与配置 servlet 的最大区别就在于<jsp-file>和<servlet-class>这两个标签：
    - 配置 JSP 页面通过<jsp-file>标签指定 JSP 页面的位置和名字；
    - 配置 servlet 则通过<servlet-class>标签来确定 servlet 类的包名和类名。
    在 page1.jsp 中：
    ```
    <%= config.getInitParameter("Website") %>
    ```
    在实际开发时，几乎不会在 web.xml 中配置 JSP 页面，因为 JSP 主要用来做表现层，几乎不会涉及到配置或后台数据的处理；更多时候都是在 servlet 中来读取配置数据，完成业务逻辑；

    例 2：在 web.xml 中配置 Servlet：
    ```xml
    <web-app ...>
        <servlet>
            <servlet-name>ConfigDemo</servlet-name>
            <servlet-class>com.test.TestConfig<servlet-class>

            <init-param>
                <param-name>Website</param-name>
                <param-value>http://www.test.com</param-value>
            </init-param>
        </servlet>

        <!-- Servlet 与 URL 的映射，可以在浏览器地址栏输入 url-pattern 配置的值将请求传递给 Servlet-->
        <servlet-mapping>
            <servlet-name>ConfigDemo</servlet-name>
            <url-pattern>/config</url-pattern>
        </servlet-mapping>
    </web-app>
    ```
    则在 TestConfig.java 中（在 servlet 没有内置的 config 对象，需要调用 getServletConfig 函数获得 config 对象）：
    ```java
    PrintWriter out = response.getWriter();
    ServletConfig conf = getServletConfig();
    out.println(conf.getInitParameter("Website"));
    ```
  - Enumeration<String> getInitParameterNames()：返回 web.xml 中当前 jsp 页面 /Servlet 初始化所需要的所有参数的枚举；
  - String getServletName()；

## 3. exception：

- 定义：`java.lang.Throwable Exception；`

- 在 Java 文件中开发者需要处理可能出现的异常，但在 JSP 脚本中无须处理异常，JSP 脚本包含的所有可能出现的异常都可以交给专门处理错误的页面进行处理；

- JSP 页面中出现异常的处理方式：
  
  在 JSP 编译生成的 Servlet 源文件中的_jspService 方法中：
  ```java
  try {
      // ......
  } catch (java.lang.Throwable t) {
      // ......

      if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
      else throw new ServletException(t);
  } finally {
      // ......
  }
  ```
  即：_jspService 方法的主要执行体都在 try{}catch{}finally{}结构中执行，一旦 jsp 文件抛出异常，便会被捕获，并将捕获的 java.lang.Throwable t 赋值给内置对象 exception 异常对象（因此 exception 对象仅在异常处理页面中才有效），捕获之后判断：若 page 指令的 errorPage 属性为 null，则使用系统页面莱输出异常信息；若 page 指令的 errorPage 属性不为 null，则 forward 到 errorPage 属性指定的页面；

- 常用方法：
  - String toString()：返回描述异常的信息；
    
    例：在 errorPage 中：
    ```
    <%=exception.toString() %>
    ```
  - String getMessage()：返回关于异常的简短描述信息；
  - void printStackTrace()：显示异常及其栈轨迹；
  - Throwable FillInStackTrace()：重写异常的执行栈轨迹； 

- 应用：在开发过程中，可以定义一个统一的异常处理页面，所有的 JSP 页面中将 errorPage 属性指定为统一的异常处理页面，这样方便页面风格的统一，也方便页面调试；

## 4. out：

- 定义：`javax.servlet.jsp.JspWriter out = pageContext.getOut();`
  
  另，“abstract class JspWriter extends java.io.Writer”，因此不能直接 new；

- 使用 out 对象向客户端输出字符类内容；
  
  PrintWriter 与 JspWriter 的区别；

- 常用方法：
  1)	void println()：向客户端打印字符串；
  2)	void clear()：清除缓冲区，在 flush 之后调用会抛出异常；
  3)	void clearBuffer()	：清除缓冲区，在 flush 之后调用不会抛出异常；
  4)	void flush()：将缓冲区内容全部输出到客户端，并清空缓冲区；
  5)	int getBufferSize()：返回缓存大小，单位 KB；
  6)	int getRemaining()	：返回缓存剩余大小，单位 KB；
  7)	boolean isAutoFlush()：返回缓冲区满时，是自动 flush 还是抛出异常；
  8)	void close()：关闭输出流；

- 应用：
  - 由于在使用内置 out 对象的地方都可以使用更为简洁的输出表达式`<%= ... %>`来代替，所以在实际的 JSP 页面中，通常很少使用内置的 out 对象；
  - 输出表达式`<%= ... %>`的实现就是 out.print(...)；

## 5. page

- 定义：`final java.lang.Object page = this;`

- 代表当前的 JSP 页面，也表示当前 JSP 编译后的 Servlet 类的对象，就好比普通 Java 类中的 this；

- 常用方法：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/592b7b2f2ec681d9b1e2d1bc4e0e0e66.jpg)

  （page 对象的方法都是 Object 类的方法）

## 6. pageContext

- 定义：
  ```
  final javax.servlet.jsp.PageContext pageContext = _jspxFactory.getPageContext(this, request, response, null, true, 8192, true);
  ```
  另，“abstract class PageContext extends JspContext”，因此不能直接 new；

- javax.servlet.jsp.PageContext 的对象代表着页面上下文，也就是当前页面所在的环境，这个环境中包含了变量等数信息，该对象提供了对 jsp 页面内所有对象及名字空间的访问，相当于页面中所有功能的集大成者；

- 常用方法：
  
  1)	Object getAttribute(String name, int scope)：取得指定范围内的 name 属性；
  2)	void setAttribute(String name, Object value, int scope)：设置指定范围内的 name 属性；

  注：上述方法中的参数 scope 取值（scope 默认为 page）：
  ```
  public static final int PAGE_SCOPE         = 1; // 对应于 page 范围
  public static final int REQUEST_SCOPE      = 2; // 对应于 request 范围
  public static final int SESSION_SCOPE      = 3; // 对应于 session 范围
  public static final int APPLICATION_SCOPE  = 4; // 对应于 application 范围
  ```
      例：
      page1.jsp：
      ```
      <% pageContext.setAttribute("Website", "http://www.test.com", PageContext.SESSION_SCOPE); %>
      ```
      page2.jsp：
      ```
      <%= pageContext.getAttribute("defaultWebsite", PageContext.SESSION_SCOPE) %>
      ```
  由于 page 指令中的 session 属性默认值为 true，scope 指定为`SESSION_SCOPE`，当我们在不关闭测试浏览器的情况下，访问完页面一，再去访问页面二就可以成功的获取到 SESSION_SCOPE 范围下 Website 的值；

- 其他方法

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/86f662565fe72997b234b7e694471053.jpg)

- 应用：可以通过 pageContext 对象来访问 page、request、session 和 application 作用域下的变量。

## 7. request

- 定义：
  ```
  final javax.servlet.http.HttpServletRequest request；
  ```
  另，“interface HttpServletRequest extends ServletRequest”， 因此不能直接 new；

- 当客户端浏览器向服务器发送请求时，浏览器会将请求信息都包装在一个请求中，发送给服务器端；服务器得到这个请求以后，会将从这个请求中得到的所有请求信息，封装成一个 HttpServletRequest 实例，即内置对象 request；

- 常用方法：
  1)	Enumeration<String> getHeaderNames()：获取本次请求的所有首部字段的枚举；
  2)	String getHeader(headerName)：获取本次请求中指定首部字段的值；
  
      例：
      ```
      <%
          // 获得所有的请求头
          Enumeration<String> headerNames = request.getHeaderNames();
          while (headerNames.hasMoreElements())
          {
              String headerName = headerNames.nextElement();
              // 获得请求头对应的参数
              out.println("[" + headerName + "] => [" + request.getHeader(headerName) + "]<br
      />");
          }
      %>
      ```

  3)	String getParameter(String name)：获取 POST 表单数据 /GET 数据中属性"name"的 value 值；
  4)	Enumeration<String> getParameterNames()：获取 POST 表单数据 /GET 数据中所有属性的枚举；
  5)	String[] getParameterValues(String name)：返回指定名称的参数的所有值的数组，若不存在则返回 null；

  6)	InputStream getInputStream()：调用此方法来读取来自客户端的二进制数据流；

  7)	Cookie[] getCookies()：返回客户端所有的 Cookie 对象的数组；

  8)	void setAttribute(String name, Object o)：设置本次请求关联的属性值；
  9)	void removeAttribute(String name)：删除本次请求关联的属性值；
  10)	Object getAttribute(String name)： 取本次请求关联属性值；

      注意：HttpServletRequest 类有 setAttribute() 方法，而没有 setParameter() 方法（Parameter 只能来自于客户端， attribute 可在服务端设置）；

  11)	String getProtocol()：返回请求所用的协议类型和版本号；
  12)	String getContentType()： 返回本次请求体的 MIME 类型；
  13)	String getServerName()：返回接收请求的服务器主机名；
  14)	int getContextLength()：返回请求体的长度（以字节为单位）；
  15)	String getMethod()：返回此 request 中的 HTTP 方法，比如 GET,，POST，或 PUT；
  16)	HttpSession getSession(boolean create)：返回当前请求所绑定的 session 对象，若没有 session，根据布尔值 create 决定是否创建一个新的 session 并返回（默认是 true），否则返回 null；

  17)	void setCharacterEncoding(String charset)：指定对浏览器发送来的数据进行重新编码（或者称为解码）时，使用的编码集（MIME 字符集），例如 UTF-8；
  18)	String getCharacterEncoding()：返回客户端请求所使用的编码方式（请求报文中 Content-Type 的 charset），若请求首部中没有指定，则返回 null；
  19)	int getServletPort()：返回服务器接收此请求的端口号；
  20)	String getRemoveAddr()：返回此请求的客户端 IP 地址；

      路径问题相关：http://mlaaalm.iteye.com/blog/671915
  21)	String getRealPath(String path)：返回指定虚拟路径的真实路径；
  22)	String getQueryString()：& 之后 GET 方法的参数部分；
  23)	String getPathInfo()：返回 Servlet 访问路径之后，QueryString 之前的中间部分；返回的字符串是由 Servlet 服务器解码 (decode) 过的；
  24)	String getServletPath()：返回 web.xml 中定义的 Servlet 访问路径；
  25)	String getContextPath()：返回上下文路径（Context 路径前缀）；
  26)	String getRequestURI()：等于 getContextPath() + getServletPath() + getPathInfo()；返回的字符串没有被 Servlet 服务器 decoded 过；
  27)	String getRequestURL()：等于 getScheme() + "://" + getServerName() + ":" + getServerPort() + getRequestURI()；
  28)	String getPathTranslated()：等于 getServletContext().getRealPath("/") + getPathInfo()；
  
  例 1：
  ```xml
  <servlet>  
    <servlet-name>test</servlet-name>  
    <servlet-class>coresun.TestServlet</servlet-class>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>test</servlet-name>  
    <url-pattern>*.do</url-pattern>  
  </servlet-mapping>  
  ```
  *.do 表示只要是以。do 结尾的地址，都可以访问此 Servlet；
  
  访问 http://localhost:8080/FilterWeb/update.do?userName=zhangsan&age=20  
  
  页面返回的结果如下：  
  ```
  request.getRequestURL() = http://localhost:8080/FilterWeb/update.do  
  request.getRequestURI() = /FilterWeb/update.do  
  request.getContextPath() = /FilterWeb  
  request.getServletPath() = /update.do  
  request.getQueryString() = userName=zhangsan&age=20  
  request.getPathInfo() = null 
  request.getPathTranslated() = null  
  request.getProtocol() = HTTP/1.1  
  request.getMethod() = GET  
  request.getScheme() = http  
  request.getRequestedSessionId() = 0D5219B7FF11D47EBE95B2E6A31076B5  
  request.isRequestedSessionIdFromCookie() = true  
  request.isRequestedSessionIdFromURL() = false  
  request.isRequestedSessionIdValid() = true  
  request.getAuthType() = null
  ```
  例 2：
  ```xml
  <servlet>  
    <servlet-name>test</servlet-name>  
    <servlet-class>coresun.TestServlet</servlet-class>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>test</servlet-name>  
    <url-pattern>/faces/*</url-pattern>  
  </servlet-mapping>  
  ```
  以上表示只要是以 /faces/ 开头的地址，此 Servlet 都可以访问；
  
  访问 http://localhost:8080/FilterWeb/faces/update?userName=zhangsan&age=20  
  
  页面返回的结果如下：  
  ```
  request.getRequestURL() = http://localhost:8080/FilterWeb/faces/update  
  request.getRequestURI() = /FilterWeb/faces/update  
  request.getContextPath() = /FilterWeb  
  request.getServletPath() = /faces  
  request.getQueryString() = userName=zhangsan&age=20  
  request.getPathInfo() = /update  
  request.getPathTranslated() = D:\FilterProject\FilterWeb\update  
  request.getProtocol() = HTTP/1.1  
  request.getMethod() = GET  
  request.getScheme() = http  
  request.getRequestedSessionId() = 0D5219B7FF11D47EBE95B2E6A31076B5  
  request.isRequestedSessionIdFromCookie() = true  
  request.isRequestedSessionIdFromURL() = false  
  request.isRequestedSessionIdValid() = true  
  request.getAuthType() = null  
  ```

- 应用：

## 8. response

- 定义
  ```
  final javax.servlet.http.HttpServletResponse response；
  ```
  另，“interface HttpServletResponse extends ServletResponse”， 因此不能直接 new；

- response 对象定义了处理创建 HTTP 信息头的接口。通过使用这个对象，开发者们可以添加新的 cookie 或时间戳，还有 HTTP 状态码等；

-	常用方法：
  
  1)	void addCookie(Cookie cookie)：添加指定的 cookie 至响应中；
  2)	void addDateHeader(String name, long date)：添加指定名称的响应头和日期值；
  3)	void addHeader(String name, String value)：添加指定名称的响应头和 string 值；
  4)	void addIntHeader(String name, int value)：添加指定名称的响应头和 int 值；

  5)	void setHeader(String name, String value)：使用指定名称和值设置响应头的名称和 string 值；
  6)	void setIntHeader(String name, int value)：使用指定名称和值设置响应头的名称和 int 值；
  7)	void setStatus(int sc)：设置响应的状态码；
  8)	void setCharacterEncoding(String charset)：指定服务器在将响应数据发送到客户端浏览器前，对数据进行重新编码时使用的编码集（MIME 字符集），例如 UTF-8，默认是 ISO-8859-1，该方法必须在调用 response.getWriter() 之前进行设置；
  9)	String getCharacterEncoding()：返回对响应重新编码采用的编码；

      注意区别：
      void setHeader(name, value)：如果 Header 中没有定义则添加，如果已定义则用新的 value 覆盖原用 value 值；
      void addHeader(name, value)：如果 Header 中没有定义则添加，如果已定义则保持原有 value 不改变；

  10)	void sendError(int sc, String msg)：使用指定的状态码和短消息向客户端发送一个出错响应；
  11)	void sendRedirect(String location)：使用指定的 URL 向客户端发送一个临时的重定向响应，状态码 302；

  12)	OutputStream getOutputStream()：向客户端输出二进制数据，获取输出流；
  13)	PrintWriter getWrite()：返回一个 PrintWriter 对象，PrintWriter 与 JspWriter 的区别；

- 应用：

  在实际开发中，当我们需要向客户端传回字符流内容的时候，要么使用内置的 out 对象，要么就使用输出表达式，而这个内置的 response 对象却非常少用，但内置的 out 是 JspWriter 的实例（输出表达式本质上也是 out），而 JspWriter 又是 Writer 的子类，由于 Writer 是字符流，所以 out 只能控制字符的响应，对于图片、视频、cookie、重定向等响应，out 无法处理；
  - response 生成验证码
    ```jsp
    <%@ page language="java" contentType="image/jpeg"%>
    <%@ page pageEncoding="UTF-8" %>
    <%@ page import="java.awt.image.*, java.io.*, java.awt.*, java.awt.Color, java.util.Random" %>
    <%@ page import="com.sun.image.codec.jpeg.JPEGCodec, com.sun.image.codec.jpeg.JPEGImageEncoder" %>

    <!DOCTYPE>
    <html>
    <head>
    <title>页面一</title>
    </head>
    <body>
        <%!
        // 随机字典，去掉了 O,0,I,1 这些难分辨的字符
        public static final char[] CHARS = {'2', '3', '4', '5', '6', '7', '8', '9', 
            'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'V', 'U', 'W', 'X', 'Y', 'Z'};

        public static Random random = new Random();

        // 获取随机 6 位随机数
        public static String getRandomString()
        {
            StringBuilder buffer = new StringBuilder();
            for (int i = 0; i < 6; ++i)
            {
                buffer.append(CHARS[random.nextInt(CHARS.length)]);
            }

            return buffer.toString();
        }

        // 获得随机的颜色
        public static Color getRandomColor()
        {
            return new Color(random.nextInt(255), random.nextInt(255), random.nextInt(255));
        }

        // 获得某个颜色的反颜色
        public static Color getReverseColor(Color c)
        {
            return new Color(255 - c.getRed(), 255 - c.getGreen(), 255 - c.getBlue());
        }
        %>

        <%
        String randomString = getRandomString();
        request.getSession(true).setAttribute("randomstring", randomString); // 将随机字符串放到 Session 中存起来

        int width = 100;
        int height = 30;

        Color bgColor = getRandomColor(); // 背景颜色
        Color frontColor = getReverseColor(bgColor);

        BufferedImage bi = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        Graphics2D g = bi.createGraphics(); // 获取绘图对象
        g.setFont(new Font(Font.SANS_SERIF, Font.BOLD, 16)); // 设置字体
        g.setColor(bgColor);
        g.fillRect(0, 0, width, height);
        g.setColor(frontColor);
        g.drawString(randomString, 18, 20);

        // 绘制噪声点
        for (int i = 0, n = random.nextInt(100); i < n; ++i)
        {
            g.drawRect(random.nextInt(width), random.nextInt(height), 1, 1);
        }
        ServletOutputStream identifyPic = response.getOutputStream();

        // 编码器，转成 JPEG 格式
        JPEGImageEncoder encoder = JPEGCodec.createJPEGEncoder(identifyPic);

        // 对图片进行编码
        encoder.encode(bi);
        identifyPic.flush();
        %>
    </body>
    </html>
    ```
  - response 实现重定向

    重定向是利用服务器返回的状态码来实现的，客户端向服务器发送请求的时候，服务器通过 response 的 setStatus 方法设置一个状态码（如果不设置，系统会默认设置），然后服务器会向客户端返回这个状态码。如果服务器返回了 301 或者 302，则浏览器会到新的网址重新请求；
    ```jsp
    <%
    response.setStatus(301); // 设置返回码
    response.setHeader("Location", "http://www.test.com");// 设置新的 Location
    %>
    ```
    通常直接使用封装好的方法：
    ```jsp
    <%
    // 返回码是 302
    response.sendRedirect("http://www.jellythink.com");
    %>
    ```
  - response 增加 Cookie（另 cookie）

    步骤：
    1)	使用 Cookie(String name, String value) 构造函数，创建 Cookie 实例；
    2)	设置 Cookie 的生命期限，即该 Cookie 可以“存活”多长时间；
    3)	调用 response 对象的 addCookie(Cookie cookie) 向客户端写 Cookie；
    实现：
    ```jsp
    <%
        String userName = request.getParameter("username");
        String password = request.getParameter("password");
    // 创建 Cookie 对象
        Cookie cUserName = new Cookie("USERNAME", userName);
        Cookie cPassword = new Cookie("PASSWORD", password);
    // 设置有效时间 3600 秒
    // 若不设置有效时间，客户端仅将 cookie 加载至内存，浏览器关闭即失效；设置有效时间后，才能将 cookie 保存至硬盘；
        cUserName.setMaxAge(3600);
        cPassword.setMaxAge(3600);
    // 向客户端设置 Cookie
        response.addCookie(cUserName);
        response.addCookie(cPassword);
    %>
    ```
  - response 设置页面自动刷新时间
    ```
    <%
      // 设置每隔 5 秒自动刷新
      response.setIntHeader("Refresh", 5);
    >%
    ```

## 9. session

- 定义：
  ```
  javax.servlet.http.HttpSession session = pageContext.getSession()；
  ```
  另，“interface HttpSession”，因此不能直接 new；

- session 信息保存在服务器端的内存中，服务器内存中保存了多个客户端的 session 信息，每个 session 都表示一个客户端与服务器的一次会话，不同 session 根据唯一的 sessionID 来辨别。

- 默认情况下，JSP 允许会话跟踪（禁止会话跟踪需要显式地关掉它，通过将 page 指令中 session 属性值设为 false 来实现），在客户端访问服务器第一个 jsp 页面时，一个新的 HttpSession 对象会自动为新的客户端实例化，实现会话管理，直到会话结束该对象才被销毁；

- 常用方法：
  
  1)	void setAttribute(String name, Object value)：使用指定的名称和值来产生一个对象并绑定到 session 中；
  2)	Object getAttribute(String name)：返回 session 对象中与指定名称绑定的对象，如果不存在则返回 null；
  3)	Enumeration<String> getAttributeNames()：返回 session 对象中所有绑定对象的枚举；
  4)	void removeAttribute(String name)：移除 session 中指定名称的对象；
  5)	String getId()：返回服务器创建 session 对象时设置的唯一 ID； 

  6)	long getCreationTime()：返回 session 对象被创建的时间， 以毫秒为单位，从 1970 年 1 月 1 号凌晨开始算起；
  7)	long getLastAccessedTime()：返回客户端最后访问的时间，以毫秒为单位，从 1970 年 1 月 1 号凌晨开始算起；
  8)	int getMaxInactiveInterval()：返回 session 超时时间，以秒为单位，servlet 容器将会在这段时间内保持会话打开；
  9)	void setMaxInactiveInterval(int interval)：用来指定 session 超时时间，以秒为单位，servlet 容器将会在这段时间内保持会话有效；
      
      注：tomcat 服务器的 MaxInactiveInterval 即默认超时时间为 30 分钟，可在 web.xml 中修改默认超时时间，以分钟为单位：
      ```xml  
      <session-config>
        <session-timeout>15</session-timeout>
      </session-config>
      ```
      若将 MaxInactiveInterval 设置为 -1，则 session 永远不会超时，只有主动调用 invalidate 方法或关闭服务器才能销毁 session；

  10)	boolean isNew()：返回是否为一个新的客户端，或者客户端是否拒绝加入 session；
      例：
      ```
      // 检测网页是否由新的用户访问
      if (session.isNew()){
        // 对第一次访问的用户的操作
      } else {
          // 对正在会话的用户操作
      }
      ```
  11)	void invalidate()：将 session 无效化，解绑任何与该 session 绑定的对象，删除整个会话；
