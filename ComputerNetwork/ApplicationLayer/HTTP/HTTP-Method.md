- [HTTP Method NOTE](#http-method-note)
  - [1. GET](#1-get)
    - [1.1. 编码](#11-编码)
  - [2. POST](#2-post)
    - [2.1. 编码](#21-编码)
    - [2.2. GET 与 POST 的区别：](#22-get-与-post-的区别)
  - [3. PUT](#3-put)
  - [4. HEAD](#4-head)
  - [5. DELETE](#5-delete)
  - [6. OPTIONS](#6-options)
  - [7. TRACE](#7-trace)
  - [8. CONNECT](#8-connect)

# HTTP Method NOTE

http://www.jellythink.com/archives/806

webservice： http://www.w3school.com.cn/webservices/

- SOAP

  一个基于 XML 的可扩展消息信封格式，需同时绑定一个网络传输协议。这个协议通常是 HTTP 或 HTTPS，但也可能是 SMTP 或 XMPP。

- WSDL

  一个 XML 格式文档，用以描述服务端口访问方式和使用协议的细节。通常用来辅助生成服务器和客户端代码及配置信息。

- UDDI

  一个用来发布和搜索 WEB 服务的协议，应用程序可借由此协议在设计或运行时找到目标 WEB 服务。
  这些标准由这些组织制订：W3C 负责 XML、SOAP 及 WSDL；OASIS 负责 UDDI。

Http 协议定义了客户端与服务器交互的不同方法，最基本的方法有 4 种，分别是 GET，POST，PUT，DELETE。URL 定位了这个资源，而 HTTP 中的 GET，POST，PUT，DELETE 就是对应着对这个资源的查，改，增，删 4 个操作；

## 1. GET

get 方法用于请求访问已被 URI 识别的资源，指定的资源经服务器端解析后返回响应内容；

GET 方法将请求的编码信息添加在网址后面，网址与编码信息通过"?"号分隔。如下所示：
http://www.runoob.com/hello?key1=value1&key2=value2

GET 方法是浏览器默认传递参数的方法，一些敏感信息，如密码等建议不使用 GET 方法。

GET 方法传输数据的大小有限制；实际上，URL 不存在参数上限的问题，HTTP 协议规范没有对 URL 长度进行限制。这个限制是特定的浏览器及服务器对它的限制。IE 对 URL 长度的限制是 2083 字节 (2K+35)。对于其他浏览器，如 Netscape、FireFox 等，理论上没有长度限制，其限制取决于操作系统的支持。

### 1.1. 编码

http://blog.csdn.net/yanwushu/article/details/8088260

客户端：
- 对原始 URL（含有各种字符）进行 URLEncode（使得 URL 的所有字符都在 ASCII 字符范围内），得到类型“%XY”形式的 URL；
- 将请求头以 utf-8/iso-8859-1 等编码方式转换为二进制 ；
- 发送请求；

服务器端：
- 把数据用 iso-8859-1/utf-8 等编码方式进行解码，得到类似“%XY”形式的 URL；
- URLdecode，得到原始 URL（含有各种字符）；

  PS：使用 request.getParameter("name") 获取到参数数据时，实际上已经完成了第一步解码，且解码过程中程序里是无法指定，tomcat 默认缺省用的是 iso-8859-1，因此，在客户端一般都是用 UTF-8 或 GBK 对数据 URL encode，而 tomcat 服务器用 iso-8859-1 方式 URL decoder，导致了 GET 请求带中文参数在 tomcat 服务器端就会得到乱码，解决办法：
  - new String(request.getParameter("name").getBytes("iso-8859-1"),"客户端指定的 URL encode 编码方式")// 先还原回字节码，然后用正确的方式解码数据
  - 修改 web.xml->使 tomcat 获取数据后用指定的方式 URL decoder：
    ```XML
    <Connector port="8080" protocol="HTTP/1.1" maxThreads="150" connectionTimeout="20000" redirectPort="8443" URIEncoding="GBK"/>   // 或 utf-8
    ```

## 2. POST

post 方法用于传输实体的主体；

一些敏感信息，如密码等我们可以通过 POST 方法传递，POST 提交数据是隐式的。

POST 提交数据在浏览器中是不可见的，但可被抓包工具获取。

JSP 使用 getParameter() 来获得传递的参数，getInputStream() 方法用来处理客户端的二进制数据流的请求。

POST 是没有大小限制的，HTTP 协议规范也没有进行大小限制。POST 数据是没有限制的；起限制作用的顶多是服务器的处理程序的处理能力，而这个限制是针对所有 HTTP 请求的，与 GET、POST 没有多少关系；

### 2.1. 编码

- 客户端（浏览器）编码
 
  在 form 所在的 html 文件里如果有段`<meta http-equiv="Content-Type" content="text/html; charset= 字符集（GBK，utf-8 等）"/>`，那么 post 就会用此处指定的编码方式编码，指定 form 表单的 post 方法提交数据的 URL encode 编码方式；

  从这里可以看出对于 get 方法来说，URL encode 的编码方式是由浏览器设置来决定，（可以用 js 做统一指定），而 post 方法，开发人员可以指定。

- 服务器端解码
  
  如果用 tomcat 默认缺省设置，也没做过滤器等编码设置，那么他也是用 iso-8859-1 解码的，但是 request.setCharacterEncoding("字符集") 可以派上用场

### 2.2. GET 与 POST 的区别：

1、Get 是用来从服务器上获得数据（没有请求体)，而 Post 是用来向服务器上传递数据（包含请求体)。 

2、Get 将表单中数据的按照 variable=value 的形式，添加到 action（服务）所指向的 URL 后面，并且两者使用“?”连接，而各个变量之间使用“&”连接；Post 是将表单中的数据放在 form 的数据体中，按照变量和值相对应的方式，传递到 action 所指向 URL。 

3、Get 是不安全的，因为在传输过程，数据被放在请求的 URL 中，而如今现有的很多服务器、代理服务器或者用户代理都会将请求 URL 记录到日志文件中，然后放在某个地方，这样就可能会有一些隐私的信息被第三方看到。另外，用户也可以在浏览器上直接看到提交的数据，一些系统内部消息将会一同显示在用户面前。Post 的所有操作对用户来说都是不可见的。 

4、Get 传输的数据量小，因为受 URL 长度限制；Post 可以传输大量的数据，所以在上传文件只能使用 Post（当然还有一个原因，将在后面的提到）。 

5、Get 限制 Form 表单的数据集的值必须为 ASCII 字符；而 Post 支持整个 ISO10646 字符集。默认是用 ISO-8859-1 编码 

6、Get 是 Form 的默认方法。

7、使用 GET 方法时，浏览器可能会缓存你的地址等信息，还会留下历史记录，而对于 POST 方法呢，则不会进行缓存；

## 3. PUT

put 方法用于传输文件，要求在请求报文的主体中包含文件内容，然后保存到请求 URI 指定的位置；

## 4. HEAD

head 方法等同于 get 方法，区别在于要求响应不返回报文主体部分，只返回请求头部；

## 5. DELETE

delete 方法用于删除文件，是与 put 方法相反的方法；

## 6. OPTIONS

options 方法用于查询针对指定 URI 服务器支持的所有 HTTP 方法；

## 7. TRACE

trace 方法要求 web 服务器端将之前的请求通信环回给客户端；

## 8. CONNECT

connect 方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行 TCP 通信；
