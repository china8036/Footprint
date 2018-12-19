- [JSP 页面构成](#jsp-页面构成)
  - [1. 静态部分](#1-静态部分)
  - [2. 动态部分](#2-动态部分)
    - [2.1. JSP 注释](#21-jsp-注释)
    - [2.2. JSP 声明](#22-jsp-声明)
    - [2.3. JSP 输出表达式](#23-jsp-输出表达式)
    - [2.4. JSP 脚本](#24-jsp-脚本)
    - [2.5. JSP 指令](#25-jsp-指令)
      - [2.5.1. page](#251-page)
      - [2.5.2. include](#252-include)
      - [2.5.3. taglib](#253-taglib)
    - [2.6. JSP 动作（标签）](#26-jsp-动作标签)
    - [2.7. JSP 自定义标签](#27-jsp-自定义标签)
    - [2.8. JSTL](#28-jstl)
      - [2.8.1. 安装](#281-安装)
      - [2.8.2. 使用](#282-使用)
    - [2.9. JSP 表达式语言（EL）](#29-jsp-表达式语言el)

# JSP 页面构成

## 1. 静态部分

标准的 HTML 标签、静态的页面内容，这些内容与静态 HTML 页面相同；

## 2. 动态部分

受 Java 程序控制的内容，这些内容由 Java 程序来动态生成，如下：

### 2.1. JSP 注释

- 方式一： `<%-- 这里是 JSP 注释 --%>`；

- 方式二：
  ```jsp
  <%
      /**
        *这个是多行注释
        */
  %>
  ```
  注：不同于 HTML，JSP 是只运行在服务器端的语言，因此 JSP 注释不会被发送到客户端，也就是说 HTML 的注释可以在客户端通过源码查看到，但 JSP 的注释是无法通过网页源码查看到的；

### 2.2. JSP 声明

- JSP 声明用于声明变量和方法，供 JSP 页面的其它部分调用；

- 在 JSP 中，似乎不会看到的类的直接定义，方法貌似可以独立存在，这好像有点脱离了 Java 一切皆为对象的完全面向对象的思想。但实际上，JSP 声明将会被转换成对应的 Servlet 类的成员变量或成员方法，所以 JSP 声明依然是遵循着 Java 语法的；

- 语法：
  - JSP 形式：<%! 声明部分 %>

    eg：
    ```jsp
    <%!
    int i = 0;
    String getDefaultWebsite()
    {
        return "http://www.baidu.com";
    }
    %>
    ```
  - XML 形式：
    ```jsp
    <jsp:declaration>
        声明部分
    </jsp:declaration>
    ```

### 2.3. JSP 输出表达式

- JSP 输出表达式会先计算出表达式的值，再将值转换为 String 格式在表达式出现的地方替换该表达式；

- 表达式可以是任何符合 Java 语言规范的表达式，但是不能使用分号来结束表达式，表达式的前后也不需要空格；

- 语法：
  - JSP 形式：<%= 表达式 %>
  - XML 形式：
    ```jsp
    <jsp:expression>
        表达式
    </jsp:expression>
    ```

### 2.4. JSP 脚本

- 在 JSP 页面中可以包含任何可以执行的 Java 代码，所有可执行 Java 代码都可以通过如下方式嵌入 JSP 页面中：
  - JSP 形式
    ```jsp
    <% Java 代码 %>
    ```
  - XML 形式
    ```jsp
    <jsp:scriptlet>
      Java 代码
    </jsp:scriptlet>
    ```

- 鉴于应用程序服务器对 jsp 文件的处理流程，可以实现 HTML 语句（静态部分）与 JSP 脚本语句（动态部分）的交叉编写与灵活组合，如：
  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <%! int fontSize; %>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="utf-8">
  <title>测试 title>
  </head>
  <body>
  <h3>For 循环实例</h3>
  <%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
    <font color="green" size="<%= fontSize %>">
      测试
    </font><br />
  <%}%>
  </body>
  </html>
  ```

### 2.5. JSP 指令

- JSP 指令在编译 JSP 页面时执行，用来设置与整个 JSP 页面相关的属性，例如编码格式、文档类型等。这些指令用来告知 JSP 引擎如何处理该 JSP 页面；

- 语法：
  - jsp 形式：`<%@ 指令名 属性名 =“属性值” ……%>` （注意：一个<%@ %>中只能编写一个指令，若有多个指令，应在多个<%@ %>中编写)；
  - XML 形式：`<jsp:directive. 指令名 属性名 =“属性值” …… />`

常用编译指令有以下三个：

#### 2.5.1. page

该指令是针对当前页面属性的指令，位于 jsp 页面的顶端，同一个 jsp 页面可包含多个 page 指令；

| 属性名                | 取值范围                                  | 说明                                       |
| ------------------ | ------------------------------------- | ---------------------------------------- |
| **language**       | java                                  | 指定解释该 JSP 文件时采用的语言，默认为 java；                |
| extends            | 任何类的全名（包含包名）                          | 指定 JSP 页面编译所产生的 Java 类所继承的父类，或所实现的接口；        |
| **import**         | 任何类名、包名                               | 引入该 JSP 中用到的类、包等；  JSP 会默认导入四个包：java.lang.*、javax.servlet.*、javax.servlet.jsp.*、javax.servlet.http；  **import 是唯一一个可以在同一个 jsp 文件中出现多次的属性；** |
| session            | true、false                            | 指明该 JSP 页面是否内置 / 使用了 Session 对象；  默认为 true；     |
| autoFlush          | true、false                            | 是否使用缓冲区。如果为 true，则使用 out.println() 等方法输出的字符串并不是立刻到达客户端的，而是输出到缓冲区；当缓冲区满、程序执行完毕或者执行 out.flush() 操作时才发送到客户端；  默认为 true； |
| buffer             | none、数字 +kb                            | 指定缓存大小。当 autoFlush 为 true 时才有效；              |
| isThreadSafe       | true、false                            | 用来设置 JSP 页面是否可以多线程访问。当设置为 true 时，JSP 页面能同时响应多个客户的请求；当设置为 false 时，JSP 页面同一时刻只能响应一个客户的请求，其他客户需要排队等待。默认值为 true； |
| **info**           | 任意字符串                                 | 指明 JSP 的信息。该信息可以通过 Servlet.getServletInfo() 方法获得； |
| errorPage          | 某个 JSP 页面的**相对路径**                      | 指明一个错误显示页面。如果该 JSP 程序抛出了一个未捕获的异常，则转到 errorPage 指定的页面。errorPage 指定的页面通常 isErrorPage 属性为 true，且内置的 exception 对象为未捕获的异常； |
| **contentType**    | 合法的文档类型（一般为”text/html,charset=utf-8”） | 在响应中告知客户端返回响应的文档类型（**MIME**）和使用的字符集（但未必是真实情况）；  所有媒体类型参见 http://www.iana.org/assignments/media-types/media-types.xhtml； |
| **pageEncoding**   | 指定生成网页的编码字符集                          | JSP 文件本身使用的编码，将 JSP 翻译成 Java 源码时，就是根据 pageEncoding 的编码格式读取的； |
| isErrorPage        | true、false                            | 设置 JSP 页面是否为错误处理页面；如果该页面本身已是错误处理页面，则通常无须指定 errorPage 属性； |
| isELIgnored        | true、false                            | 指定是否执行 EL 表达式，默认为 true；                     |
| isScriptingEnabled | true、false                            | 确定脚本元素能否被使用，默认为 true；                     |

eg：
```jsp
<%@ page language="java" contentType="text/html; charset="utf-8" pageEncoding=“utf-8”%>
```
注意：任何 page 允许的属性都只能出现一次，否则会出现编译错误；import 属性除外，import 可以多次出现；

#### 2.5.2. include

用于将一个外部文件的所有源代码嵌入到当前的 jsp 文件中，在 jsp 文件编译时期执行，适合不经常变化的内容的导入（每次变化都要重写编译）；
语法格式：
1.	jsp 形式：<%@ include file="文件相对 url 地址" %>；
2.	XML 形式：<jsp:directive.include file="文件相对 url 地址" />；
（注意：如果没有给文件关联一个路径，JSP 编译器默认在当前路径下寻找）
·	eg: <%@ include file=”2.jsp” %>
·	注：
若在 1.jsp 中<%@ include file=”2.jsp” %>，运行该程序后，只能找到 1_jsp.java 和 1_jsp.class，找不到 2_jsp.java 和 2_jsp.class，因为 2.jsp 直接被导入到了 1.jsp 中，没有被编译；
导入时是将源代码全部导入（包括 html 的<head>、<body>，jsp 的所有元素），导入之后若发生冲突，在编译时解决（如新的覆盖旧的）；

#### 2.5.3. taglib

用于引入一个自定义标签集合的定义，包括库路径、自定义标签，在 JSP 页面中启用定制行为；

语法格式：
- jsp 形式：`<%@ taglib uri="uri" prefix="prefixOfTag" %>`；

  （uri 属性确定标签库的位置，prefix 属性指定标签库的前缀）

- XML 形式：`<jsp:directive.taglib uri="uri" prefix="prefixOfTag" />`；

### 2.6. JSP 动作（标签）

- JSP 动作标签实际上是一些预定义好的函数，使用 XML 语法结构来控制 servlet 引擎。它能够动态插入一个文件，重用 JavaBean 组件，引导用户去另一个页面，为 Java 插件产生相关的 HTML 等；

- JSP 动作标签语法格式：
  - 双标签形式：`<jsp:action_name attribute="value" ></jsp:action_name>`
  - 单标签形式：`<jsp:action_name attribute=“value”/>`
	（只有 param 和与 JavaBean 有关的三个标签是单标签，其它都是双标签）

- JSP 动作与 JSP 指令的区别：
  - JSP 指令是告知 Servlet 引擎如何编译当前 JSP 页面，而 JSP 动作只是运行时的动作；
  - JSP 动作是对常用的 JSP 功能的抽象与封装，它是 JSP 脚本的标准化写法；
  - JSP 指令影响编译，JSP 动作影响运行；

- JSP 规范定义了一系列的标准动作，用 JSP 作为前缀，常用的 JSP 动作如下：

- jsp:forward：

  - 执行请求转发，将客户端的请求完整转发到另一个页面（请求参数、请求属性都不变），并由另一个页面负责返回响应（完全采用了新页面代替原有页面来对用户生成响应），且客户端浏览器的 URL 不会改变；
  - 语法格式：
    ```jsp
    <jsp:forward page="relativeURL">
        <jsp:param name="xxx" value="xxx" />
    </jsp:forward>（双标签，需要闭合）
    ```
    （<jsp:param>用来增加请求参数，在目的页面可使用 HttpServletRequest 类的 getParameter(“name”) 方法获取）

- jsp:param：

  - 用于传递参数；
  - 语法格式：
    ```jsp
    <jsp:param name="xxx" value="xxx" />
    ```
    （单标签）
  - 由于单独的 jsp:param 动作没有实际意义，所以这个动作本身不能单独使用，一般与以下三哥动作结合使用：
    - jsp:include（jsp:param 动作用于将参数值传入被导入的页面）；
    - jsp:forward（jsp:param 动作用于将参数传入被转向的新页面）；
    - *jsp:plugin（jsp:param 用于将参数传入页面的 JavaBean 实例或 Applet 实例）；

- jsp:include：

  - 用于动态引入一个 JSP 页面，在执行请求处理时执行导入操作，导入的是新页面代码的输出结果，注意与 include 指令进行区别；
  - 语法格式：
```jsp
<jsp:include page="relativeURL" flush="true">
    <jsp:param name="" value="" />
</jsp:include>（双标签，需要闭合）
```
flush 属性用于指定输出缓存是否转移到被导入的文件中；如果指定为 true，则包含在被导入文件中；如果为 false，则包含在原文件中；
- 注意：
  - 若在 1.jsp 中`<jsp:include page=”2.jsp” %>`，运行该程序后，可以找到 1_jsp.java、1_jsp.class 和 2_jsp.java、2_jsp.class，说明 jsp:include 是在编译成 class 以后再进行 include 的；
  - 查看上述例子中的 1_jsp.java，可以看到：

    ![image](http://img.cdn.firejq.com/jpg/2018/1/24/a56c41d8651657064e4d20dd0924d64a.jpg)

    说明 jsp:include 动作是通过调用 include 方法来包含新页面的；
- cf：include 指令与 include 动作：

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/12d947babbde953163f223ffb4c57a06.jpg)
  - include 指令：
    - 在编译时期就执行导入操作；
    - 包含的是所有源代码；
    - 适合页面内容不经常变化的包含（弱点在编译阶段，每次变化都需重新编译）；
  - jsp:include 动作：
    - 在页面请求期间才执行导入操作；
    - 包含的是代码的输出结果；
    - 适合页面内容经常变化的包含（弱点在运行阶段，多次重新编译对其影响不大）；

- jsp:plugin：

  - 用于下载 Applet 到客户端执行，现在已经基本放弃这种用法了；

- jsp:useBean：

  - JavaBean：符合 JavaBean 规范的普通 Java 类，也叫 POJO（Plain Ordinary Java Object）；

    设计目的是解决代码重复编写，减少代码冗余，明确区分功能，提高代码重用性和可维护性；

    设计原则：
    - 该类为公有类 public；
    - 含有无参的公有构造方法；
    - 属性都为私有 private；
    - 含有针对每个属性的 getFiled 方法和 setField 方法；
  - 会在 WEB-INF/classes/ 目录下寻找指定的类，然后在 JSP 中实例化一个 Java Bean 对象和初始化这个 Java 实例；
  - 语法格式：
    ```jsp
    <jsp:useBean id="beanId" class="className" scope="Value" />
    ```
    （单标签）

    | 属性名   | 取值范围                             | 说明                                       |
    | ----- | -------------------------------- | ---------------------------------------- |
    | id    | 合法的 Java 变量名称                      | 指定 Java Bean 对象的名称；  在 JSP 中可以使用该名称引用该 Java Bean 对象； |
    | class | Java Bean 类的全名                    | Java Bean 类的全名，包含包名；                      |
    | scope | page、request、session、application | page：该 JavaBean 实例仅在该页面有效；  request：该 JavaBean 实例在本次请求有效；  session：该 JavaBean 实例在本次会话内有效；  application：该 JavaBean 实例在本应用内一直有效 |

- jsp:setProperty：

  - 调用类的 setField 方法，设置 JavaBean 实例的属性值；
  - 语法格式：
    - `<jsp:setProperty name="Bean Name" property="propertyName" value="value" />`：标准写法；
    - `<jsp:setProperty name="Bean Name" property="*" />`：当使用 POST 提交表单时，自动将实例的属性与表单的属性对应，全部赋值；
    - `<jsp:setProperty name="Bean Name" property="propertyName“/>`：当使用 POST 提交表单时，自动将实例的属性与表单的属性对应，只为指定的属性赋值；
    - `<jsp:setProperty name="Bean Name" property="propertyName“ param=”URL 中的参数名“/>`：当是使用 GET 即 URL 传参提交请求时，将 URL 中的 param 参数值赋给实例的指定属性；

  - 执行赋值操作时，若 value 值是字符串数据，会在目标类中通过标准的 valueOf 方法自动转换成数字、boolean、Boolean、 byte、Byte、char、Character；

- jsp:getProperty：

  - 调用类的 getField 方法，获取 JavaBean 实例的属性值；
  - 语法格式：
    ```jsp
    <jsp:getProperty name="Bean Name" property="propertyName" />
    ```
    （单标签）

> See also：https://stackoverflow.com/questions/3177733/how-to-avoid-java-code-in-jsp-files. “The use of scriptlets (those <% %> things) in JSP is indeed highly discouraged since the birth of taglibs (like JSTL) and EL (Expression Language, those ${} things) over a decade ago. “

### 2.7. JSP 自定义标签

自定义标签库是一种非常优秀的表现层组件技术。通过使用自定义标签库，可以在简单的标签中封装复杂的功能；

自定义标签开发：
- 开发自定义标签处理类（自定义标签最终都得通过 Java 类来进行处理）；
  - 自定义标签类都继承一个父类：javax.servlet.jsp.tagext.SimpleTagSupport；
    ```java
    public class SimpleTagSupport implements SimpleTag {
        private JspTag parentTag;
        private JspContext jspContext; // getJspContext 返回 this.jspContext
        private JspFragment jspBody; //getJspBody 返回 this.jspbody
      ……
    }
    ```
  - 如果标签类包含属性，每个属性都需要有对应的 getter 和 setter 方法；
  - 需要重写 doTag() 方法，用于生成页面内容，即使用该标签的地方将使用 doTag() 输出的内容替代：
    ```java
    import java.io.IOException;
    import javax.servlet.jsp.JspException;
    import javax.servlet.jsp.tagext.SimpleTagSupport;

    public class TestTag extends SimpleTagSupport {
        public void doTag() throws JspException, IOException {
            getJspContext().getOut().write("测试");
        }
    }
    ```
    在自定义标签处理类中：
    - 输出到页面：
      `getJspContext().getOut().println(“xxx”)（不要 try{}catch{}）`
    - 处理标签体内容：
      ```java
      getJspBody().invoke(null)// 使用 JspWriter 对象输出标签体内容到客户端页面
      getJspBody().invoke(Writer out)// 将标签体内容传给指定的 Writer 对象，由该对象处理输出
      ```

- 在 WEB-INF 目录下（或其任意子目录下）建立一个`*.tld`文件（标签库定义 / 配置文件：用于将自定义标签和对应的标签类关联起来，实质上是 xml 文件，Java Web 会自动加载该文件），每个*.tld 文件对应一个标签库，每个标签库可包含多个标签；

  标签库定义文件（tld 文件）的编写规范：
  - 标签库定义文件的根元素是 taglib，它可以包含多个 tag 子元素，每个 tag 子元素都定义一个标签；
  - taglib 标签主要有以下几个子元素：
    - description：对该 tld 文件的一些描述性的信息；
    - tlib-version：指定该标签库实现的版本，这是一个作为标识的内部版本号，对程序没有太大的作用；
    - short-name：标签库的默认短名，没有太大的作用；
    - uri：这个属性非常重要，它指定该标签库的 URI，相当于指定该标签库的唯一标识。JSP 页面中使用标签库时就是根据该 URI 属性来定位标签库的；
    - tag：tag 元素主要有以下子元素：
      - name，指定标签的名字，JSP 页面中就是根据该名称来使用此标签的，所以该子元素非常重要
      - tag-class，指定标签的处理类，它指定了标签由哪个标签处理类来处理，该子元素极其重要
      - body-content，指定标签体的内容（如<xxx> This is message body</xxx>，“This is message body”即为标签体），该子元素非常重要，它主要可以取以下几个值：
      - tagdependent，指定标签处理类自己负责处理标签体；
      - empty，该标签只能作为空标签（即不含标签体的标签）来使用，如<jt:test />；
      - scriptless，指定该标签的标签体可以是静态 HTML 元素、表达式语言，但不能是 JSP 脚本
      - dynamic-attributes，指明该标签是否支持动态属性。只有定义动态属性标签时才需要该子元素；
      - attribute：指定标签的参数，在定义 attribute 元素时，需要为其指定以下几个子元素：
      - name：设置属性名；在使用自定义标签时，就是通过该名指定属性名；
      - required：设置该属性是否为必须属性，取值为 true 或 false；
      - fragment：设置该属性是否支持 JSP 脚本、表达式等动态内容，取值为 true 或 false；

  例：xx.tld
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>

  <taglib xmlns="http://java.sun.com/xml/ns/j2ee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
    version="2.0">

    <description>Test 1.0</description>
    <display-name>Test TLD</display-name>
    <tlib-version>1.0</tlib-version>
    <short-name>jtlib</short-name>
    <uri>http://www.jellythink.com/jtlib/</uri>

    <!-- 定义一个标签 -->
    <tag>
      <!-- 定义标签名 -->
      <name>jtTag</name>
      <!-- 定义标签处理类 -->
      <tag-class>com.jellythink.practise.TestTag</tag-class>
      <!-- 定义标签体内容为空 -->
      <body-content>empty</body-content>

      <!-- 配置 name 属性 -->
      <attribute>
          <name>name</name>
          <required>true</required>
          <fragment>true</fragment>
      </attribute>

      <!-- 配置 price 属性 -->
      <attribute>
          <name>price</name>
          <required>true</required>
          <fragment>true</fragment>
      </attribute>
    </tag>
  </taglib>
  ```
- 在 JSP 文件中使用自定义标签；
  - 导入标签库；使用 taglib 编译指令导入标签库，就是将标签库和指定前缀关联起来；

    例：

    `<%@ taglib uri="http://www.jellythink.com/jtlib/" prefix="jt" %>//prefix 即“<jt: 标签名》“中的 jt`

  - 使用标签；在 JSP 页面中使用自定义标签；

### 2.8. JSTL

JSP Standard Tag Library（JSP 标准标签库），由 JCP（Java Community Process）制订，是一个 JSP 标签集合，它封装了 JSP 应用的通用核心功能；

根据 JSTL 标签所提供的功能，可以将其分为 5 个类别。
- 核心标签；
- 格式化标签；
- SQL 标签；
- XML 标签；
- JSTL 函数；

JSTL 中支持 EL(Expression Language) 语法，也支持使用<%= %>，但都要包含在双引号中，如：
```jsp
<c:out value="<%=userList.getUser( ).getPhoneNumber( ) %>" />
<c:out value="${userList.user.phoneNumber}" />
```

#### 2.8.1. 安装

在 https://tomcat.apache.org/download-taglibs.cgi 下载 4 个 jar 包：

![image](http://img.cdn.firejq.com/jpg/2018/1/24/63a69721cd9fac475d96de5cd2d6154c.jpg)

放到项目下 WEB-INF/lib 中；（若使用 Eclipse 开发，需要 copy 到 Eclipse 界面下的 lib 中，否则 Eclipse 会自动清空该目录下的 jar 包）

![image](http://img.cdn.firejq.com/jpg/2018/1/24/4de6072943471049026516985835ea88.jpg)

在需要使用 JSTL 的每个 jsp 页面中加入 tablib 指令：（需要什么类型的标签加哪个标签库即可)
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
<%@ taglib prefix="fm" uri="http://java.sun.com/jsp/jstl/functions" %>
```

#### 2.8.2. 使用

https://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/

- 核心标签

  https://www.cnblogs.com/lihuiyy/archive/2012/02/24/2366806.html

  http://peiquan.blog.51cto.com/7518552/1314707

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/768841832512ac5024edd6c9e2a4f773.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/1cb05ad4831990be81de577b8f624e3d.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/d50f3f719c5f4673dd6604d51c64d31a.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/4f14f749bc038ad2c38a57e56c8f37a6.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/3ab3c4ad376322d6ebfa7204e860d890.jpg)

- 格式化标签

  | `<fmt:formatNumber>`    | 使用指定的格式或精度格式化数字      |
  | --------------------- | -------------------- |
  | `<fmt:parseNumber>`     | 解析一个代表着数字，货币或百分比的字符串 |
  | `<fmt:formatDate>`      | 使用指定的风格或模式格式化日期和时间   |
  | `<fmt:parseDate>`       | 解析一个代表着日期或时间的字符串     |
  | `<fmt:bundle>`          | 绑定资源                 |
  | `<fmt:setLocale>`       | 指定地区                 |
  | `<fmt:setBundle>`       | 绑定资源                 |
  | `<fmt:timeZone>`        | 指定时区                 |
  | `<fmt:setTimeZone>`     | 指定时区                 |
  | `<fmt:message>`         | 显示资源配置文件信息           |
  | `<fmt:requestEncoding>` | 设置 request 的字符编码       |

- SQL 标签

  | `<sql:setDataSource>` | 指定数据源                                  |
  | ------------------- | -------------------------------------- |
  | `<sql:query>`         | 运行 SQL 查询语句                              |
  | `<sql:update>`        | 运行 SQL 更新语句                              |
  | `<sql:param>`         | 将 SQL 语句中的参数设为指定值                        |
  | `<sql:dateParam>`     | 将 SQL 语句中的日期参数设为指定的 java.util.Date 对象值    |
  | `<sql:transaction>`   | 在共享数据库连接中提供嵌套的数据库行为元素，将所有语句以一个事务的形式来运行 |

- XML 标签

  http://wiki.jikexueyuan.com/project/jsp/xml-data.html

  在使用 xml 标签前，你必须将 XML 和 XPath 的相关包拷贝至你的<Tomcat 安装目录》\lib 下：
  - XercesImpl.jar，下载地址： http://www.apache.org/dist/xerces/j/
  - xalan.jar，下载地址： http://xml.apache.org/xalan-j/index.html

  | `<x:out>`       | 与<%= ... >, 类似，不过只用于 XPath 表达式          |
  | ------------- | ------------------------------------ |
  | `<x:parse>`     | 解析 XML 数据                            |
  | `<x:set>`       | 设置 XPath 表达式                           |
  | `<x:if>`        | 判断 XPath 表达式，若为真，则执行本体中的内容，否则跳过本体      |
  | `<x:forEach>`   | 迭代 XML 文档中的节点                          |
  | `<x:choose>`    | <x:when>和<x:otherwise>的父标签           |
  | `<x:when>`      | <x:choose>的子标签，用来进行条件判断              |
  | `<x:otherwise>` | <x:choose>的子标签，当<x:when>判断为 false 时被执行 |
  | `<x:transform>` | 将 XSL 转换应用在 XML 文档中                      |
  | `<x:param>`     | 与<x:transform>共同使用，用于设置 XSL 样式表        |

- JSTL 函数

  | fn:contains()           | 测试输入的字符串是否包含指定的子串            |
  | ----------------------- | ---------------------------- |
  | fn:containsIgnoreCase() | 测试输入的字符串是否包含指定的子串，大小写不敏感     |
  | fn:endsWith()           | 测试输入的字符串是否以指定的后缀结尾           |
  | fn:escapeXml()          | 跳过可以作为 XML 标记的字符               |
  | fn:indexOf()            | 返回指定字符串在输入字符串中出现的位置          |
  | fn:join()               | 将数组中的元素合成一个字符串然后输出           |
  | fn:length()             | 返回字符串长度                      |
  | fn:replace()            | 将输入字符串中指定的位置替换为指定的字符串然后返回    |
  | fn:split()              | 将字符串用指定的分隔符分隔然后组成一个子字符串数组并返回 |
  | fn:startsWith()         | 测试输入字符串是否以指定的前缀开始            |
  | fn:substring()          | 返回字符串的子集                     |
  | fn:substringAfter()     | 返回字符串在指定子串之后的子集              |
  | fn:substringBefore()    | 返回字符串在指定子串之前的子集              |
  | fn:toLowerCase()        | 将字符串中的字符转为小写                 |
  | fn:toUpperCase()        | 将字符串中的字符转为大写                 |
  | fn:trim()               | 移除首位的空白符                     |

### 2.9. JSP 表达式语言（EL）

表达式语言 (Expression Language) ，EL 表达式是 Java 中的一种特殊的通用编程语言，借鉴于 JavaScript 和 XPath；主要作用是在 Java Web 应用程序嵌入到网页（如 JSP）中，用以访问页面的上下文以及不同作用域中的对象 ，取得对象属性的值，或执行简单的运算或判断操作。EL 在得到某个数据时，会自动进行数据类型的转换；

- 目的：使用 EL 的目的是简化在 JSP 中访问变量的方式，简单静态 HTML 与 Java 代码的耦合；

- 设置在当前 JSP 页面是否禁用 EL 语言：<%@ page isELIgnored ="true|false" %>,TRUE 表示禁止。FALSE 表示不禁止。JSP2.0 中默认的启用 EL 语言；

- 语法格式：${EL 表达式}
// 当 JSP 编译器在属性中见到"${}"格式后，它会产生代码来计算这个表达式，并且产生一个替代品来代替表达式的值；

- 注意点：
1）	仅能在 jsp 页面的静态部分中适用，在 java 代码中或在 jsp <% %>中无法被解析；
2）	JSTL 标签中可使用 EL 表达式，如<c:out value=”${xxxx}”></c:out>；
3）

- 用途：

  - 访问 Bean 对象的属性并输出（必须是标准 Bean 对象才可使用，即要保证要取得对象的那个的属性有相应的 setXxx() 和 getXxx() 方法）
    - ${对象名。属性名}：没有指定范围时，容器会依次从 pageContext,request,session,application 范围查找该对象名的对象，假如途中找到 username，就直接回传，不再继续找下去，然后调用该对象的 getXxx 方法访问指定的属性并输出；如果值为 null, 会转换成" "输出（如果是 jsp 输出，<%= %>，会输出“null“）；假如全部的范围都没有找到时，就回传 null，也会转换成“”输出；
    - ${对象名 [“属性名”]}：作用同第一种表示方法，但当一下情况时，只能用这种表示方法而不能用第一种：
      - 该对象是一个数组或者 List，[index] 用来访问其中的元素，index 为数字；
      - 该对象是一个 map 对象，[“key”] 用来访问指定键（key）的值；
      - 属性名中包含特殊字符，如“.“ 、“-”等，则 ${user.My-Name}应当改为 ${user["My-Name"]}；
      - 属性名为一个变量，需要进行动态取值时，如 ${sessionScope.user[data]}，其中 data 是一个变量；

  - 进行简单计算并输出结果
    - 算术运算："+","-","*","/","%"，注意"+"只能进行加法操作，不能进行连接操作；

      例：利用 java Bean 对象 box 的已有属性 width 和 height，为新的属性 perimeter 赋值：
      ```jsp
      <jsp:setProperty name="box" property="perimeter" value="${2*box.width+2*box.height}"/>//“”内亦可以被解析
      ```
    - 关系运算：">",">=","<","<=","==","!="；
    - 逻辑运算："&&","||","!"；
    - empty 运算：当空字符串、空的集合、值为 null、找不到对应的值时，运算结果为 true；

  - 访问请求的参数值并输出（POST/GET）
    - `${param.age}:` 等价与 request.getParameter("age");
    - `${paramValues.city}` : 等价与 request.getParameterValues("citys");

  - 访问使用 setAttribute 方法设置的普通变量

    如：`pageContext.setAttribute(“name”, “123test”);`

    则 ${pageScope.name} -> 123test

    **注意：无法访问在<% %>或<%! %>中定义的普通局部变量**；

- 操作符

  | 类型   | 操作符                                      |
  | ---- | ---------------------------------------- |
  | 算术型  | +、-（二元）、*、/、div、%、mod、-（一元）              |
  | 逻辑型  | and、&&、or、\|\|、!、not                     |
  | 关系型  | ==、eq、!=、ne、<、lt、>、gt、<=、le、>=、ge。可以与其他值进行比较，或与布尔型、字符串型、整型或浮点型文字进行比较。 |
  | 空    | empty 空操作符是前缀操作，可用于确定值是否为空。              |
  | 条件型  | A ?B :C。根据 A 赋值的结果来赋值 B 或 C。             |

- 隐式对象

  ![image](http://img.cdn.firejq.com/jpg/2018/1/24/d2774e0523c3c334f1e38811f86ecb09.jpg)

  | 隐式对象             | 说明                                       | 常用示例                                     |
  | ---------------- | ---------------------------------------- | ---------------------------------------- |
  | pageScope        | page 作用域                                 | 在<% %>中，pageContext.setAttribute(“name”,  value)，则在静态代码中，可用 ${pageScope.name}访问该变量并输出；  如果指定值为一个对象，可通过 ${pageScope .objectName.  attributeName} 访问对象的属性； |
  | requestScope     | request 作用域                              | 在<% %>中，request.setAttribute(“name”,  value)，则在静态代码中，可用 ${requestScope.name}访问该变量并输出；  如果指定值为一个对象，可通过 ${requestScope.objectName.  attributeName} 访问对象的属性； |
  | sessionScope     | session 作用域                              | 在<% %>中，session.setAttribute(“name”,  value)，则在静态代码中，可用 ${sessionScope.name}访问该变量并输出；  如果指定值为一个对象，可通过 ${sessionScope.objectName.  attributeName} 访问对象的属性； |
  | applicationScope | application 作用域，该隐式对象允许访问应用程序范围的对象       | 在<% %>中，application.setAttribute(“name”,  value)，则在静态代码中，可用 ${applicationScope.name}访问该变量并输出；  如果指定值为一个对象，可通过 ${applicationScope.objectName.  attributeName} 访问对象的属性； |
  | param            | Request 对象的参数，将请求参数名称映射到单个字符串参数值（通过调用  request.getParameter 获得）。 | **${param.name}或 ${param[“name”]}相当于  request.getParameter(“name”)；** |
  | paramValues      | Request 对象的参数，将请求参数名称映射到一个数值数组（通过调用  request.getParameterValues 获得）。它与  param 隐式对象非常类似，但它检索一个字符串数组而不是单个值。 | ${paramvalues. name} 相当于 request.getParamterValues(name)； |
  | header           | HTTP 信息头，将请求头名称映射到单个字符串头值（通过调用 ServletRequest.getHeader(String name) 获得）。  表达式 ${header.name} 相当于 request.getHeader(“name”)； | 使用 ${header["user-agent"]} 来访问请求报文的 user-agent 字段（因为 User-Agent 中包含“-”这个特殊字符，所以必须使用“[]”，而不能写成 ${header.User-Agent}）； |
  | headerValues     | HTTP 信息头，将请求头名称映射到一个数值数组（通过调用 ServletRequest.getHeaders(String) 获得）。  表达式 ${headerValues.name} 相当于 request.getHeaderValues(name)； |                                          |
  | initParam        | 上下文初始化参数，将上下文初始化参数名称映射到单个值（通过调用  ServletContext.getInitparameter(String name) 获得）； | 在 web.xml 中定义：  `<context-param>   <param-name>userid</param-name>    <param-value>mike</param-value>    </context-param>`  则在 jsp 中，可以直接使用 ${initParam.userid}来取得名称为 userid，其值为 mike 的参数，相当于 String userid  =(String)application.getInitParameter("userid"); |
  | cookie           | Cookie 值，将  cookie 名称映射到单个 cookie 对象。向服务器发出的客户端请求可以获得一个或多个 cookie。表达式 ${cookie. name  .value} 返回带有特定名称的第一个 cookie 值。如果请求包含多个同名的 cookie，则应该使用  ${headerValues. name} 表达式； |                                          |
  | pageContext      | 当前 JSP 页的上下文。它可以用于访问 JSP 隐式对象，如请求、响应、会话、输出、servletContext  等。 |                                          |

  其中，对于 pageContext，访问：http://jq.tunnel.qydev.com/weixin/page/receive_info.jsp?openid=aaabbbccc&test=1234
  - ${pageContext.request} |取得请求对象（调用 toString 方法）： org.apache.catalina.connector.RequestFacade@650f0c5a
  - ${pageContext.session} |取得 session 对象 （调用 toString 方法）：org.apache.catalina.session.StandardSessionFacade@7e291207
  - ${pageContext.request.queryString} |取得请求的参数字符串（即 URL 中“？“之后的）：openid=aaabbbccc&test=1234
  - ${pageContext.request.requestURL} |取得请求的 URL，但不包括请求的参数字符串（即 URL 中“？“之前的）：http://jq.tunnel.qydev.com/weixin/page/receive_info.jsp
  - ${pageContext.request.contextPath} |服务的 web application 的名称：/weixin
  - ${pageContext.request.method} |取得 HTTP 的方法 (GET、POST)：GET
  - ${pageContext.request.protocol} |取得使用的协议 (HTTP/1.1、HTTP/1.0)：HTTP/1.1
  - ${pageContext.request.remoteUser} |取得用户名称： （空）
  - ${pageContext.request.remoteAddr } |取得用户的 IP 地址：127.0.0.1
  - ${pageContext.session.id} |取得 session 的 ID：3BB5ABA7FCCEAFC3298C649768A97A90
  - ${pageContext.servletContext.serverInfo}|取得主机端的服务信息：Apache Tomcat/9.0.0.M17

- 函数

  JSP EL 允许在表达式中使用函数。这些函数必须被定义在自定义标签库中。函数的使用语法如下：
  ```
  ${ns:func(param1, param2, ...)}
  ```
  ns 指的是命名空间（namespace），func 指的是函数的名称，param1 指的是第一个参数，param2 指的是第二个参数，以此类推。比如，有函数 fn:length，在 JSTL 库中定义；

  如可以这样来获取一个字符串的长度：`${fn:length("Get my length")}`

- https://www.cnblogs.com/java-zhao/p/5633881.html