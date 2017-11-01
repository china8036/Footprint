- [RESTful 架构 NOTE](#restful-%E6%9E%B6%E6%9E%84-note)
  - [概念](#%E6%A6%82%E5%BF%B5)
  - [RESTful 优点](#restful-%E4%BC%98%E7%82%B9)
  - [RESTful API](#restful-api)
  - [URI 设计](#uri-%E8%AE%BE%E8%AE%A1)
  - [Refer Links](#refer-links)

# RESTful 架构 NOTE

## 概念

REST（英文：Representational State Transfer，又称具象状态传输）是 Roy Thomas Fielding 博士于 2000 年在他的博士论文 [1] 中提出来的一种万维网软件架构风格，目的是便于不同软件 / 程序在网络（例如互联网）中互相传递信息。

目前在三种主流的 Web 服务实现方案中，因为 REST 模式与复杂的 SOAP 和 XML-RPC 相比更加简洁，越来越多的 web 服务开始采用 REST 风格设计和实现。例如，Amazon.com 提供接近 REST 风格的 Web 服务执行图书查询；雅虎提供的 Web 服务也是 REST 风格的。

REST(Representational State Transfer) 是网络服务接口的一种风格，并不是一个标准，就 web service 而言，REST 要比 SOAP（SOAP 是标准，不是风格）轻量得多，容易得多；虽然目前 SOAP 在企业级的 web service 中还有一席之地，但是在公共的 Internet 上，不是 REST 的服务实在不好意思和人打招呼，我们经常可以看到评价某某服务是 RESTful 的，但是从来没有听说某某服务是 SOAPful 的；

## RESTful 优点

- 可更高效利用缓存来提高响应速度

- 通讯本身的无状态性可以让不同的服务器的处理一系列请求中的不同请求，提高服务器的扩展性

- 浏览器即可作为客户端，简化软件需求

- 相对于其他叠加在 HTTP 协议之上的机制，REST 的软件依赖性更小

- 不需要额外的资源发现机制

- 在软件技术演进中的长期的兼容性更好

## RESTful API

符合 REST 设计风格的 Web API 称为 RESTful API，它从以下三个方面资源进行定义：

1)	直观简短的资源地址：URI，比如：http://example.com/resources/。

2)	传输的资源：Web 服务接受与返回的互联网媒体类型，比如：JSON，XML，YAML 等。

3)	对资源的操作：Web 服务在该资源上所支持的一系列请求方法（比如：POST，GET，PUT 或 DELETE）。

## URI 设计

RESTful 架构有一些典型的设计误区：

-	最常见的一种设计错误，就是 URI 包含动词。因为"资源"表示一种实体，所以应该是名词，URI 不应该有动词，动词应该放在 HTTP 协议中。

-	另一个设计误区，就是在 URI 中加入版本号。因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个 URI。版本号可以在 HTTP 请求头信息的 Accept 字段中进行区分。

## Refer Links

RESTful 架构 http://www.ruanyifeng.com/blog/2011/09/restful.html https://www.cnblogs.com/shanyou/archive/2011/10/17/2215930.html

RESTful API 设计指南 http://www.ruanyifeng.com/blog/2014/05/restful_api.html

REST 当中为什么要使用 HTTP PUT http://www.cnblogs.com/shanyou/archive/2011/10/17/2215930.html