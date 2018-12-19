- [JSP 其他知识点](#jsp-其他知识点)
  - [1. session 与 cookie](#1-session-与-cookie)
    - [1.1. cookie 与 session 的比较：](#11-cookie-与-session-的比较)
    - [1.2. cookie](#12-cookie)
    - [1.3. session](#13-session)
  - [2. 请求重定向和请求转发](#2-请求重定向和请求转发)
  - [3. 易混淆的几个类关系](#3-易混淆的几个类关系)
  - [4. 四种作用域范围](#4-四种作用域范围)
    - [4.1. page--PageContext 域](#41-page--pagecontext-域)
    - [4.2. request--Request 域](#42-request--request-域)
    - [4.3. session--Session 域](#43-session--session-域)
    - [4.4. application--ServletContext 域](#44-application--servletcontext-域)
  - [5. 几种 context 辨析](#5-几种-context-辨析)
    - [5.1. ServletContext：](#51-servletcontext)
    - [5.2. ActionContext：](#52-actioncontext)
    - [5.3. ServletActionContext](#53-servletactioncontext)
    - [5.4. ApplicationContext](#54-applicationcontext)
    - [5.5. PageContext](#55-pagecontext)
    - [5.6. SessionContext](#56-sessioncontext)
    - [5.7. JspContext：javax.serlvet.jsp.JspContext](#57-jspcontextjavaxserlvetjspjspcontext)
    - [5.8. ELContext](#58-elcontext)
  - [6. getParameter() 与 getAttribute()：](#6-getparameter-与-getattribute)
  - [7. javax.servlet.jsp.JspWriter 和 java.io.PrintWriter：](#7-javaxservletjspjspwriter-和-javaioprintwriter)
  - [8. JSP/Servlet 中的编码问题](#8-jspservlet-中的编码问题)
    - [8.1. JSP/Servlet 中四个编码设置的函数 / 指令：](#81-jspservlet-中四个编码设置的函数--指令)
    - [8.2. 编码过程](#82-编码过程)

# JSP 其他知识点

## 1. session 与 cookie

### 1.1. cookie 与 session 的比较：

session：
- 没有大小限制；
- 保存在服务器端；
- 保存的是 Object 类型；
- 随着会话结束而被销毁；
- 适合保存重要的信息；

cookie：
- 大小有限制；
- 保存在客户端；
- 保存的是 String 类型；
- 可以长期保存在客户端；
- 只适合保存不隐秘的不重要信息；

### 1.2. cookie

cookie 是存储在客户机的文本文件，保存了大量的用户会话信息；服务器通过指定一个唯一的 sessionID 作为 cookie 来代表每个客户端，用以识别该客户端的连贯会话请求；

Cookie 分为两种，一种可以叫做 persistent cookie，设置有效期后保存在客户端硬盘中，就是我们通常意义上所说的 cookie；而另一种可以叫做 session cookie，没有设置有效期且只保存在客户端内存中，浏览器关闭就会丢失，一般服务器端的 session 是借助于 seesion cookie 来和客户端交互的（如传递 sessionID）（也有其它方法）；

- javax.servlet.http.Cookie 类：

  JSP 中 cookie 信息封装在 javax.servlet.http.Cookie 类的实例对象中，一个 cookie 对象存放一对 cookie 键值；

  javax.servlet.http.Cookie 类常用方法：
  1)	Cookie(String name, String value)：类的构造方法，指定 cookie 名和 cookie 值；
  2)	void setMaxAge(int expiry)：设置 cookie 有效期，以秒为单位（cookie 在客户端硬盘的存放时间）；默认有效期为当前 session 的存活时间（即客户端会将 cookie 加载至内存中，随浏览器的关闭而失效）；
  3)	int getMaxAge()：获取 cookie 有效期，以秒为单位，默认为 -1 ，表明 cookie 会存活到浏览器关闭为止；
  4)	String getName()：返回该 cookie 的名称，名称创建后将不能被修改；
  5)	String getValue()：获取 cookie 的值；
  6)	void setValue(String newValue)：设置 cookie 的值；
  7)	void setPath(String uri)：设置 cookie 的路径，默认为当前页面目录下的所有 URL，还有此目录下的所有子目录；
  8)	String getPath()：获取 cookie 的路径；

  9)	boolean getSecure()：返回当前 cookie 是否加入了 secure 属性（即只在 https 加密传输时才允许使用 cookie）；
  10)	void setSecure(boolean flag)：设定 cookie 是否要加上 secure 属性，默认是 false；
  11)	boolean isHttpOnly()：返回当前 cookie 是否加入了 HttpOnly 属性（禁止客户端通过 JavaScript 获取 cookie，防止 xss 攻击）；
  12)	void setHttpOnly(boolean httpOnly)：设定当前 cookie 是否要加上 HttpOnly 属性，默认是 false；

      注：为防止 XSS，一般要在 cookie 中加入 httponly 属性，以禁止客户端使用 JavaScript 获取 cookie；同时加入 secure 属性，使得只在 https 下才使用 cookie；

  13)	void setComment(String purpose)：设置注释描述 cookie 的目的。当浏览器将 cookie 展现给用户时，注释将会变得非常有用；
  14)	String getComment()：返回描述 cookie 目的的注释，若没有则返回 null；

- 使用 JSP 在响应中设置 cookie：

  步骤：
  1)	使用 Cookie(String name, String value) 构造函数，创建 Cookie 实例；
  名称和值中都不能包含空格和这些字符：[ ] ( ) = , " / ? @ : ;
  2)	设置 Cookie 的生命期限（以秒为单位），即该 Cookie 可以“存活”多长时间；
  3)	调用 response 对象的 addCookie(Cookie cookie) 向客户端写 Cookie；

  实现：
  ```jsp
  <%
      String name = java.net.URLEncoder.encode(request.getParameter(“name”),  “utf-8”);
      String password = request.getParameter(“password”);
  // 创建 Cookie 对象
      Cookie cUserName = new Cookie("USERNAME", name);
      Cookie cPassword = new Cookie("PASSWORD", password);
  // 设置有效时间 24 小时
  // 若不设置有效时间，客户端仅将 cookie 加载至内存，浏览器关闭即失效（也叫 session cookie）；设置有效时间后，才能将 cookie 保存至硬盘（也叫 persistent cookie）；
      cUserName.setMaxAge(60*60*24);
      cPassword.setMaxAge(60*60*24);
  // 向客户端响应中添加 Cookie
      response.addCookie(cUserName);
      response.addCookie(cPassword);
  %>
  ```

- 使用 JSP 在请求中读取 cookie：

  1)	调用 request.getCookies() 方法来获得一个 javax.servlet.http.Cookie 对象的数组；
  2)	遍历这个数组，使用 getName() 方法和 getValue() 方法来获取每一个 cookie 的名称和值；
  ```jsp
  <%
    Cookie cookie = null;
    Cookie[] cookies = null;
    // 获取 cookie 数组
    cookies = request.getCookies();
    if( cookies != null ) {
        for (cookie : cookies) {
          out.print("参数名 : " + cookie.getName() + “<br>”);
          out.print("参数值：" + java.net.URLDecoder.decode(cookie.getValue(), "utf-8") +" <br>");
          out.print("------------------------------------<br>");
        }
      } else {
        out.println("<h2>没有发现 Cookie</h2>");
      }
  %>
  ```

- 使用 JSP 删除客户端 cookie：

  cookie 一旦在客户端设置后，无法在服务器端直接删除，但可通过覆盖 / 有效期过期来间接删除；

  步骤：
  1)	获取一个已经存在的 cookie 然后存储在 Cookie 对象中；
  2)	将 cookie 的有效期设置为 0；
  3)	将这个 cookie 重新添加进响应头中；
  ```jsp
  <%
    Cookie cookie = null;
    Cookie[] cookies = request.getCookies();
    if( cookies != null ){
        for (cookie : cookies) {
          if((cookie.getName( )).compareTo("name") == 0 ) {
              cookie.setMaxAge(0);
              response.addCookie(cookie);
              out.print("删除 Cookie: " + cookie.getName( ) + "<br/>");
          }
        }
      } else {
      out.println("<h2>没有发现 Cookie</h2>");
      }
  %>
  ```

### 1.3. session

## 2. 请求重定向和请求转发

- 请求重定向：response.sendRedirect(String URL)，客户端行为，要求客户端对另外一个 URL 重新发起请求，相当于两次请求，且第一次请求的对象不会保存，客户端浏览器地址栏的 URL 会改变；

- 请求转发：request.getRequestDispatcher(String URL).forward(request, response)，服务器端行为，将一次请求完整转发到服务端另一个 Jsp 页面 /Servlet 处理，转发请求后请求对象会保存，相当于一次请求，客户端浏览器地址栏的 URL 不会改变；

## 3. 易混淆的几个类关系

interface 层：ServletContext，ServletConfig，Servlet，JspPage；

abstract 类层：GenericServlet，HttpServlet，HttpJspPage；

- Servlet
  ```
  void init(ServletConfig config)；
  ServletConfig getServletConfig()；// 因此在所有 JSP 页面和 Servlet 中都可用
  void service(ServletRequest req, ServletResponse res)；
  String getServletInfo()；
  void destroy()；
  ```

- ServletConfig
  ```
  public String getServletName()；
  ServletContext getServletContext()；
  String getInitParameter(String name)；// 获得的是 <init-param></init-param>配置的参数信息，取得是 servlet 的初始值
  Enumeration<String> getInitParameterNames()；
  ```

- ServletContext
  ```
  // 在每个 web 项目中只能有唯一一个 ServletContext 实例对象，且作用域是全局；
  String getContextPath()；
  ServletContext getContext(String uripath)；
  String getMimeType(String file)；
  RequestDispatcher getRequestDispatcher(String path)；
  RequestDispatcher getNamedDispatcher(String name)；
  Servlet getServlet(String name)；
  String getRealPath(String path)；
  String getServerInfo()；
  String getInitParameter(String name)；// 获得的是 <context-param> </context-param>配置的参数信息，取得是 application 中的初始值
  Enumeration<String> getInitParameterNames()；
  boolean setInitParameter(String name, String value)；
  Object getAttribute(String name)；
  void setAttribute(String name, Object object)；
  void removeAttribute(String name)；
  ```
- GenericServlet

  ```
  void init(ServletConfig config)；// 初始化时传入 ServletConfig 对象
  abstract void service(ServletRequest req, ServletResponse res)；
  void destroy()；
  String getInitParameter(String name)；
  Enumeration<String> getInitParameterNames()；
  ServletConfig getServletConfig()；
  ServletContext getServletContext()；

  //Servlet 使用 getServletConfig() 得到 ServletConfig 对象
  //        使用 getServletContext() 得到 ServletContext 对象
  //JSP 使用 getServletConfig() 得到 ServletConfig 对象
  //    使用 getServletConfig().getServletContext() 得到 ServletContext 对象

  ```
- HttpServlet

  ```
  void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException；
  void doPost……；void doHead……；void doPut……；
  void doDelete……；void doTrace……；void doOpitions……；
  void service(HttpServletRequest req, HttpServletResponse resp)throws ServletException, IOException；
  ```

![image](http://img.cdn.firejq.com/jpg/2018/1/24/d5240373983a2ad5a5e5a79f2419a1d7.jpg)

## 4. 四种作用域范围

http://www.5ycode.com/article/63.html

### 4.1. page--PageContext 域

具有 page 范围的对象被绑定到 javax.servlet.jsp.PageContext 对象中；

可以调用 pageContext 这个隐含对象的 getAttribute() 方法来访问具有这种范围类型的对象（pageContext 对象还提供了访问其他范围对象的 getAttribute 方法），pageContext 对象本身也属于 page 范围。当 Servlet 类的_jspService() 方法执行完毕，属于 page 范围的对象的引用将被丢弃。page 范围内的对象，在客户端每次请求 JSP 页面时创建，在页面向客户端发送回响应或请求被转发（forward）到其他的资源后被删除；

### 4.2. request--Request 域

具有 request 范围的对象被绑定到 javax.servlet.ServletRequest 对象中；

可以调用 request 这个隐含对象的 getAttribute() 方法来访问具有这种范围类型的对象。在调用 forward() 方法转向的页面或者调用 include() 方法包含的页面中，都可以访问这个范围内的对象。要注意的是，因为请求对象对于每一个客户请求都是不同的，所以对于每一个新的请求，都要重新创建和删除这个范围内的对象；在完成客户端的请求之前，request 对象一直有效；

### 4.3. session--Session 域

具有 session 范围的对象被绑定到 javax.servlet.http.HttpSession 对象中；

可以调用 session 这个隐含对象的 getAttribute() 方法来访问具有这种范围类型的对象。JSP 容器为每一次会话，创建一个 HttpSession 对象，在会话期间，可以访问 session 范围内的对象；

### 4.4. application--ServletContext 域

具有 application 范围的对象被绑定到 javax.servlet.ServletContext 中；

可以调用 application 这个隐含对象的 getAttribute() 方法来访问具有这种范围类型的对象。在 Web 应用程序运行期间，所有的页面都可以访问在这个范围内的对象；

## 5. 几种 context 辨析

http://www.blogjava.net/fancydeepin/archive/2015/03/27/397185.html

“Context“指上下文、环境。

### 5.1. ServletContext：

一个 WEB 运用程序只有一个 ServletContext 实例，它是在容器（包括 JBoss, Tomcat 等) 完全启动 WEB 项目之前被创建，生命周期伴随整个 WEB 运用；

当在编写一个 Servlet 类的时候，首先是要去继承一个抽象类 HttpServlet, 然后可以直接通过 getServletContext() 方法来获得 ServletContext 对象（这是因为 HttpServlet 类中实现了 ServletConfig 接口，而 ServletConfig 接口中维护了一个 ServletContext 的对象的引用）；

利用 ServletContext 能够获得 WEB 运用的配置信息，实现在多个 Servlet 之间共享数据等；

ServletContext 对象获得几种方式：
```java
javax.servlet.http.HttpSession.getServletContext()
javax.servlet.jsp.PageContext.getServletContext()
javax.servlet.ServletConfig.getServletContext()
```

### 5.2. ActionContext：

ActionContext 是当前 Action 执行时的上下文环境，ActionContext 中维护了一些与当前 Action 相关的对象的引用，如：Parameters （参数), Session （会话), ValueStack （值栈), Locale （本地化信息) 等；

在 Struts1 时期，Struts1 的 Action 与 Servlet API 和 JSP 技术的耦合度都很紧密，属于一个侵入式框架：
```java
public ActionForward execute(ActionMapping mapping,ActionForm form,HttpServletRequest request,HttpServletResponse response){
    // TODO Auto-generated method stub
    return null;
}
```
在 Struts2, 则是通过 WebWork 来将与 Servlet 相关的数据信息转换成了与 Servlet API 无关的对象，即 ActionContext 对象；这样就使得了业务逻辑控制器能够与 Servlet API 分离开来。另外，由于 Struts2 的 Action 是每一次用户请求都产生一个新的实例，因此，ActionContext 不存在线程安全问题，可以放心使用；

### 5.3. ServletActionContext

ServletActionContext 是 ActionContext 的一个子类。ServletActionContext 从名字上来看，意味着它与 Servlet API 紧密耦合；

ServletActionContext 的构造子是私有的，主要是提供了一些静态的方法，可以用来获取：ActionContext, ActionMapping, PageContext, HttpServletRequest, HttpServletResponse, ServletContext, ValueStack, HttpSession 对象的引用；

### 5.4. ApplicationContext

### 5.5. PageContext

### 5.6. SessionContext

HttpSession 类有方法：HttpSessionContext getSessionContext()

### 5.7. JspContext：javax.serlvet.jsp.JspContext

> Official API: “JspContext serves as the base class for the PageContext class and abstracts all information that is not specific to servlets. This allows for Simple Tag Extensions to be used outside of the context of a request/response Servlet.”

每个 JSP 页面都有 JspContext 的对象，专为自定义标签而设计；

### 5.8. ELContext

## 6. getParameter() 与 getAttribute()：

- getParameter()：

  - parameter 得到的是 string；

  - 该方法传递的数据，会从 Web 客户端传到 Web 服务器端（如获取 POST/GET 传递的参数值），用于客户端重定向时的数据传递，即点击了链接或提交按扭时传值用，即用于在用表单或 url 重定向传值时接收数据用；

- getAttribute()

  - attribute 得到的是 object；

  - 该方法传递的数据，只会存在于 Web 容器内部，在具有转发关系的 Web 组件之间共享（如获取 SESSION 参数值），用于服务器端转发时的数据传递，即在 sevlet 中使用了 forward 函数，或 struts 中使用了 mapping.findForward；

HttpServletRequest 类有 setAttribute() 方法，而没有 setParameter() 方法；

parameter 与 attribute 的区别可以类比为 request 和 session 的区别；

## 7. javax.servlet.jsp.JspWriter 和 java.io.PrintWriter：

- JspWriter 和 PrintWriter 都是继承于 Java.io.Writer，都是向客户端响应中输出的工具；
ps：System.out.println() 是向本地（服务器）终端输出，注意区分；

- JspWriter 是抽象类而 PrintWriter 不是，即可以通过 new 操作来直接新建一个 PrintWriter 的对象而 JspWriter 必须通过其子类来实例化：
  - 获得 JspWriter 对象：
    - JSP 页面的预定义对象 out；
    - JspWriter jw = (new javax.servlet.jsp.PageContext()).getOut()；
    - JspWriter jw = javax.serlvet.jsp.JspContext（抽象类）的实例对象的 getOut()
  - 获得 PrintWriter 对象：
    - 通过 JSP 页面的预定义对象的方法 response.getWriter()：
      ```
      PrintWriter pw = response.getWriter();
      ```
    - 直接 new 创建对象：
      ```
      PrintWriter pw = new PrintWriter();
      ```

- PrintWriter 的 print 方法中不会抛出 IOException，而 JspWriter 的 println 方法会抛出 IOException；

- JspWriter 对 PrintWriter 有依赖：初始化一个 JspWriter 对象的时候要关联一个 PrintWriter 对象（因为 JspWriter 对象的输出方法需要调用 PrintWriter 对象的 print 方法实现；

- 使用 JspWriter 对象的输出时，会先输出到缓冲区，当满足以下任一条件时，才调用 PrintWriter 对象的 print 方法输出到客户端：
  - 缓冲区已满；
  - 当前 JSP 页面的其它输出任务全部结束；
  - page 指令的 buffer 属性关闭了 out 对象的缓存功能；

- 在同一 JSP 页面中，输出顺序有以下特征（由 JspWriter 对 PrintWriter 的依赖性所决定）：
  - PrintWriter 输出全部结束后，才开始 JspWriter 的输出；
  - 若在 JspWriter 输出后调用 out.flush()，强制输出缓冲区内容并清空，则可实现 JspWriter 的输出先于 PrintWriter；

## 8. JSP/Servlet 中的编码问题

http://developer.51cto.com/art/200906/132667.htm

https://www.cnblogs.com/caowei/p/2013-12-11_request-response.html

### 8.1. JSP/Servlet 中四个编码设置的函数 / 指令：

- page 指令的属性 pageEncoding="UTF-8"：
  只能用于 JSP 中，告诉 JSP 编译器在将 JSP 文件编译成 Servlet 时要使用的编码 / 字符集；

  但应该注意，文件真正的编码格式在创建文件的时候就已经指定（如 windows 记事本创建的为 ANSI），pageEncoding 的作用仅仅是“告知”JSP 编译器，若所告知的情况与实际不符，则可能导致乱码；

  另外，该参数还有一个功能，就是在 JSP 中不指定 contentType 参数，也不使用 response.setCharacterEncoding 方法时，指定对服务器响应进行重新编码的编码 / 字符集；

- page 指令的属性 contentType="text/html;charset=UTF-8"：

  只能用于 JSP 中，服务器在将响应数据发送到浏览器前，指定对数据进行重新编码所使用的编码 / 字符集；

- 内置对象 response.setCharacterEncoding("UTF-8")：

  可以用于 JSP 和 Servlet 中，服务器在将响应数据发送到浏览器前，指定对数据进行重新编码所使用的编码 / 字符集，必须在 response.getWriter() 前调用才有效；

- 内置对象 request.setCharacterEncoding("UTF-8")：

  可以用于 JSP 和 Servlet 中， 服务器接收到客户端请求后，指定对请求进行重新编码（解码）所使用的编码 / 字符集；

注：服务器发送数据时，服务器按照 response.setCharacterEncoding—contentType—pageEncoding 的优先顺序，对要发送的数据进行编码；

### 8.2. 编码过程

浏览器向服务器发送请求，因为浏览器与服务器之间的通信实质上是 socket 流（字节流），所以要先将请求参数编码（字符转换成字节），再发送；服务器接收到请求参数后需进行解码（字节转换成字符），然后才封装到 request 对象中；

- 客户端浏览器编码

  浏览器在接收服务器数据和发送数据到服务器时所使用的编码是相同的，默认情况下均为 JSP 页面的 response.setCharacterEncoding 参数（或者 contentType 或者 pageEncoding 参数），我们称其为浏览器编码（当然，浏览器编码可以修改，如在 IE 的菜单中选择"查看"——"编码 “进行修改）；

  浏览器显示网页时，可采用不同的编码 / 字符集来显示网页，默认是根据服务器响应的首部字段 Content-Type 中指定的编码进行显示，在 JSP 中，即是服务端 response.setCharacterEncoding(“xxx”) 指定的编码；

  浏览器发送请求数据时，若包含非 ASCII 字符，需要对请求进行 URL 编码，此时使用 浏览器编码 来进行 URL 编码；

- 服务器编码

  - 对于发送数据，服务器按照 response.setCharacterEncoding—contentType—pageEncoding 的优先顺序，对要发送的数据进行编码（字符转为字节）；默认是按照 ISO-8859-1 编码；

    实现代码如下
    - 方法一：
      ```java
      response.setCharacterEncoding("utf-8”);// 设置服务器端响应的编码（实际起“设置“作用）
      response.setHeader("contentType", "text/html; charset=utf-8”);// 通知浏览器服务器发送的数据格式是 text/html，并要求浏览器使用 utf-8 进行解码（仅仅是“通知“作用）
      ```
    - 方法二：
      ```java
      response.setCharacterEncoding("utf-8”);// 设置服务器端响应的编码为 utf-8
      response.getWriter().println("<meta http-equiv='Content-Type' content='text/html; charset=utf-8'>”);// 通过 html 代码设置响应首部字段，要求浏览器使用 utf-8 进行解码
      ```

  - 对于接收数据，分为 GET 和 POST 两种情况：

    各种 WEB 服务器对这两种情况的处理方式也各有不同，以下以 tomcat 为例：
    - 对于 POST 表单提交的请求：

      只要在接收数据的 JSP 中正确 request.setCharacterEncoding 参数，即将对客户端请求进行重新编码的编码设置成浏览器编码，就可以保证得到的参数编码正确；因为在默认请情况下，浏览器编码就是你在响应该请求的 JSP 页面中 response.setCharacterEncoding 设置的值。所以对于 POST 表单提交的数据，在获得数据的 JSP 页面中 request.setCharacterEncoding 要和生成提交该表单的 JSP 页面的 response.setCharacterEncoding 设置成相同的值；

    - 对于 URL 提交的数据和表单中 GET 方式提交的请求：

      request.setCharacterEncoding() 只对请求实体有效，对请求头部的 URL 无能为力；

      在 tomcat 中，默认情况下使用 ISO-8859-1 对 URL 提交的数据和表单中 GET 方式提交的数据进行重新编码（解码），在服务端使用 request.getParameter() 获取 GET 参数时，实际上此时已经完成了以 iso-8859-1 为字符集的重新编码（解码）；

      要解决该问题，一般有两种方法：
      - 方法一：修改服务器端对 uri 参数的默认编码

        在 Tomcat 的服务器配置文件 server.xml 的 Connector 标签中设置 useBodyEncodingForURI 或者 URIEncoding 属性：

        useBodyEncodingForURI 参数表示是否用 request.setCharacterEncoding 参数对 URL 提交的数据和表单中 GET 方式提交的数据进行重新编码，在默认情况下，该参数为 false（Tomcat4.0 中该参数默认为 true）；启用之后，可根据响应该请求的页面的 request.setCharacterEncoding 参数对数据进行的重新编码（解码），不同的页面可以有不同的重新编码（解码）的编码；

        URIEncoding 参数则指定对所有 GET 方式请求（包括 URL 提交的数据和表单中 GET 方式提交的数据）进行统一的重新编码（解码）的编码；

        修改配置文件后需重启服务器才能生效；

      - 方法二：逆向操作

        例：参数从浏览器到服务器，经过客户端浏览器 utf-8 编码，服务器端 iso-8859-1 解码，最终成为乱码。那么只需将乱码进行相反的编解码，即可得到正常的参数值；
        ```java
        String name = request.getParameter("name”);// 得到乱码，此时服务器已经完全 iso-8859-1 的解码
        name = new String(name.getBytes("iso-8859-1"),"utf-8”);// 按 iso-8859-1 提取字节码后，得到原始请求，再进行 utf-8 重新编码，即可得到正常的 name 值
        ```
        注意：`name.getBytes()；`如果不指定编码，默认按照 gb2312 进行编码；

      **在实际项目中，一切需要重启服务器的解决问题的方法在实际中都是需要谨慎操作的，因此应该采用方法二更为合适。**

总结：以 Tomcat5.0 为 WEB 服务器时，如何防止中文乱码：
- 对于同一个应用，最好统一编码，推荐为 UTF-8。
- 正确设置 JSP 的 pageEncoding 参数。
- 在所有的 JSP 和 Servlet 中设置 contentType="text/html;charset=UTF-8"或 response.setCharacterEncoding("UTF-8")，从而间接实现对浏览器编码的设置。
- 对于请求，可以使用过滤器或者在每个 JSP 和 Servlet 中设置 request.setCharacterEncoding("UTF-8")。同时，要修改 Tomcat 的默认配置，推荐将 useBodyEncodingForURI 参数设置为 true，也可以将 URIEncoding 参数设置为 UTF-8（有可能影响其他应用，所以不推荐）。
