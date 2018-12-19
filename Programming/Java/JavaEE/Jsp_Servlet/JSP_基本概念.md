- [JSP 基本概念](#jsp-基本概念)
  - [1. 优势](#1-优势)
  - [2. Eclipse web 项目目录结构](#2-eclipse-web-项目目录结构)
  - [3. Refer Links](#3-refer-links)

# JSP 基本概念

JSP（全称 Java Server Pages）是由 Sun Microsystems 公司倡导和许多公司参与共同创建的一种使软件开发者可以响应客户端请求，而动态生成 HTML、XML 或其他格式文档的 Web 网页的技术标准；

**JSP 与 PHP、ASP、ASP.NET 等语言类似，是运行在服务端的语言，它使用 JSP 标签在 HTML 网页中插入 Java 代码，标签通常以<% 开头以 %>结束**;

JSP 是以 Java 语言作为脚本语言的，JSP 网页为整个服务器端的 Java 库单元提供了一个接口来服务于 HTTP 的应用程序；

JSP 开发的 WEB 应用可以**跨平台**使用，既可以运行在 Linux 上也能运行在 Window 上；

## 1. 优势

JSP 程序与 CGI 程序有着相似的功能，但和 CGI 程序相比，JSP 程序有如下优势：

- 性能更加优越，因为 JSP 可以直接在 HTML 网页中动态嵌入元素而不需要单独引用 CGI 文件；

- 服务器调用的是已经编译好的 JSP 文件，而不像 CGI/Perl 那样必须先载入解释器和目标脚本；

- JSP 基于 Java Servlets API，因此，JSP 拥有各种强大的企业级 Java API，包括 JDBC，JNDI，EJB，JAXP 等等；

- JSP 页面可以与处理业务逻辑的 servlets 一起使用，这种模式被 Java servlet 模板引擎所支持；

P.S.

![image](http://img.cdn.firejq.com/jpg/2018/1/24/3b3d51785d1a3279372ddd57ff3c1f7a.jpg)

## 2. Eclipse web 项目目录结构

![image](http://img.cdn.firejq.com/jpg/2018/1/24/26f11c50399d42c7132021674210b218.jpg)

- Deployment Descriptor：部署的描述；

- Java Resources/Libraries/Web App Libraries：项目中需要使用的外部 jar 包可以放在里面；

- build：放入编译之后的 class 文件；

- WebContent：网站根目录；
  - META-INF
  - WEB-INF：Java 的 web 应用的安全目录（所谓安全即客户端无法访问该文件夹中的任何文件，只有服务器端才能访问）
    - lib：用于存放项目所需的外部 jar 包
    - classes：用于存放项目所有 java 源文件编译得到的 class 文件
    - web.xml：项目的配置文件

    （对于需要管理员权限才运行访问的管理者界面，可创建 WEB-INF/page/ 文件夹，然后将管理界面存放于该目录下，然后在 Servlet 中验证管理员身份成功后利用服务器端转发来访问 /WEB-INF/page 下的页面）

## 3. Refer Links

http://www.jellythink.com/archives/category/javaframework/page/2

http://www.runoob.com/jsp/jsp-implicit-objects.html
