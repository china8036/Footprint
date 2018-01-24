- [JSP 生命周期](#jsp-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [JSP 文件与 Servlet 的关系](#jsp-%E6%96%87%E4%BB%B6%E4%B8%8E-servlet-%E7%9A%84%E5%85%B3%E7%B3%BB)
  - [JSP 文件转换为 Servlet 的过程](#jsp-%E6%96%87%E4%BB%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA-servlet-%E7%9A%84%E8%BF%87%E7%A8%8B)
  - [服务端处理 JSP 请求的过程 /JSP 生命周期](#%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%A4%84%E7%90%86-jsp-%E8%AF%B7%E6%B1%82%E7%9A%84%E8%BF%87%E7%A8%8B-jsp-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)

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

P.S. 如何在Tomcat下指定Jsp生成的Java文件路径？

当需要自定义Jsp生成的Java文件位置时，可以通过如下两种方法来实现：

- 方法1：在tomcat的配置文件server.xml（路径：tomcat路径＼conf下面）里，找到：`<Context docBase="D:\workspace\icinfo\trunk\web" path="" reloadable="false" debug="0" crossContext="true" workDir="D:\workspace\icinfo\trunk\web\WEB-INF\lib\CommonPKI\META-INF\work"/>`，添加如上的workDir
## 服务端处理 JSP 请求的过程 /JSP 生命周期
