- [HTTP Method](#http-method)
  - [1. URL & URI](#1-url--uri)
  - [2. Web Services](#2-web-services)
  - [3. Refer Links](#3-refer-links)

# HTTP Method

## 1. URL & URI

- URL(Uniform Resource Locator)：统一资源定位符。URI(Uniform Resource Identifier)：统一资源标识符。
- **URL 是 URI 的子集**。
- 绝对 URI 格式：
  ```
  http://user:passwd@www.example.com:80/dir/index.html?uid=1#ch1
  （协议方案）（认证信息）   （服务器地址）（端口）（带层次的文件路径）（查询字符串）（片段标识符）
  ```

## 2. Web Services

- SOAP

  一个基于 XML 的可扩展消息信封格式，需同时绑定一个网络传输协议。这个协议通常是 HTTP 或 HTTPS，但也可能是 SMTP 或 XMPP。

- WSDL

  一个 XML 格式文档，用以描述服务端口访问方式和使用协议的细节。通常用来辅助生成服务器和客户端代码及配置信息。

- UDDI

  一个用来发布和搜索 WEB 服务的协议，应用程序可借由此协议在设计或运行时找到目标 WEB 服务。
  这些标准由这些组织制订：W3C 负责 XML、SOAP 及 WSDL；OASIS 负责 UDDI。

Http 协议定义了客户端与服务器交互的不同方法，最基本的方法有 4 种，分别是 GET，POST，PUT，DELETE。URL 定位了这个资源，而 HTTP 中的 GET，POST，PUT，DELETE 就是对应着对这个资源的查，改，增，删 4 个操作；

## 3. Refer Links

[W3School Web Services](http://www.w3school.com.cn/webservices/)
