- [RESTful API 设计与实现](#restful-api-%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0)
  - [1. 如何评价 API](#1-%E5%A6%82%E4%BD%95%E8%AF%84%E4%BB%B7-api)
    - [1.1. 名称及参数](#11-%E5%90%8D%E7%A7%B0%E5%8F%8A%E5%8F%82%E6%95%B0)
    - [1.2. 协议及行为](#12-%E5%8D%8F%E8%AE%AE%E5%8F%8A%E8%A1%8C%E4%B8%BA)
    - [1.3. 可理解及一致性](#13-%E5%8F%AF%E7%90%86%E8%A7%A3%E5%8F%8A%E4%B8%80%E8%87%B4%E6%80%A7)
  - [2. 设计原则](#2-%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99)
    - [2.1. 只公开必须的内容](#21-%E5%8F%AA%E5%85%AC%E5%BC%80%E5%BF%85%E9%A1%BB%E7%9A%84%E5%86%85%E5%AE%B9)
    - [2.2. 面向接口编程](#22-%E9%9D%A2%E5%90%91%E6%8E%A5%E5%8F%A3%E7%BC%96%E7%A8%8B)
    - [2.3. 方法重载](#23-%E6%96%B9%E6%B3%95%E9%87%8D%E8%BD%BD)
    - [2.4. 返回值](#24-%E8%BF%94%E5%9B%9E%E5%80%BC)
    - [2.5. 协议](#25-%E5%8D%8F%E8%AE%AE)
    - [2.6. 域名](#26-%E5%9F%9F%E5%90%8D)
    - [2.7. 版本（Versioning）](#27-%E7%89%88%E6%9C%AC%EF%BC%88versioning%EF%BC%89)
    - [2.8. 路径（Endpoint）](#28-%E8%B7%AF%E5%BE%84%EF%BC%88endpoint%EF%BC%89)
    - [2.9. HTTP 动词](#29-http-%E5%8A%A8%E8%AF%8D)
    - [2.10. 过滤信息（Filtering）](#210-%E8%BF%87%E6%BB%A4%E4%BF%A1%E6%81%AF%EF%BC%88filtering%EF%BC%89)
    - [2.11. 状态码（Status Codes）](#211-%E7%8A%B6%E6%80%81%E7%A0%81%EF%BC%88status-codes%EF%BC%89)
    - [2.12. 错误处理（Error handling）](#212-%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86%EF%BC%88error-handling%EF%BC%89)
    - [2.13. 返回结果类型](#213-%E8%BF%94%E5%9B%9E%E7%BB%93%E6%9E%9C%E7%B1%BB%E5%9E%8B)
    - [2.14. 其它](#214-%E5%85%B6%E5%AE%83)
  - [3. 身份认证](#3-%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81)
    - [3.1. 基于 session 的身份认证](#31-%E5%9F%BA%E4%BA%8E-session-%E7%9A%84%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81)
    - [3.2. 基于 Token 的身份认证](#32-%E5%9F%BA%E4%BA%8E-token-%E7%9A%84%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81)
      - [3.2.1. 交互流程](#321-%E4%BA%A4%E4%BA%92%E6%B5%81%E7%A8%8B)
  - [4. Refer Links](#4-refer-links)

# RESTful API 设计与实现

## 1. 如何评价 API

### 1.1. 名称及参数

方法名称是否可以自描述，即看到方法的名字就能知道方法的作用、看到参数名就知道需要传递什么样的数据（比如 getUserById(String userId)）。

### 1.2. 协议及行为

一个方法的输入、输出就是协议，什么样的输入是合法的、什么样的输入是非法的，针对合法的输入在处理后返回什么样的结果、针对非法的输入返回什么样的提示以便调用方能快速知道问题的症结所在。

同时协议也要保证在不同版本间的兼容，新版本要兼容老版本，保证之前的调用方不会因为 API 的内部修改而不能得到正确的结果。

所谓行为就是 API 内部实现逻辑，有些时候，虽然我们提供给用户的是 API，但是内部逻辑也在用户的可见范围内，这时候用户会查看内部实现并根据 API 内部实现编写自己的代码，因此内部行为的稳定同样很重要。

### 1.3. 可理解及一致性

API 一定要便于使用者理解，这样才是广泛传播的基础。如果有些 API 需要用户掌握特定的概念、定义，那么就要保持这个 API 的一致性，不能轻易的改变 API，否则会给使用者带来很大的麻烦。

## 2. 设计原则


### 2.1. 只公开必须的内容

在编写 API 时，肯定会有这样一种思考方式：是不是要多加一个参数呢？可能会有这种用法吧？参数要不要做到很灵活呢？接收一个 Object 数组吧（而且是不带泛型的）。有这样的想法能说明我们在设计 API 的时候思考了很多，但是这么实现的 API 是要命的、坑人的。

那么我们应该怎么做呢？精简、明确、直接。只公开必要的内容，不要为了“以后可能会有”而大伤脑筋，考虑未来的扩展固然是好事，但凡事总要有个度，过犹不及。

### 2.2. 面向接口编程

面向接口编程，就是在参数的传递和返回时使用接口而不是实现类，这样可以方便的替换实现方，从而达到程序容易扩展、实现更加灵活的目的。

比如我们在设计一个 API 的时候，传入参数使用接口 Map 就会比使用具体的类如 HashMap、TreeMap 更加灵活。比如 JDBC，我们使用的都是 Connection、ResultSet 这样的一些接口，具体的实现则由数据库厂商提供，MySql、Oracle 等厂商都提供了 JDBC 的实现。

### 2.3. 方法重载

方法重载在很多 API、SDK 中有着广泛的使用，之所有使用重载是因为它为程序实现带来了极大的便利。一个明显的例子是：在不同的地方做相同或类似的事情，重载这种方式的优越性就凸显出来了。

例如，在 JDK 的 java.util 包下的 Collections 类有两个排序的静态方法：
```java
sort(List<T> list)；
sort(List<T> list, Comparator<? super T> c);
```
同样的方法名不会让人疑惑，参数的丰富让人一眼就能看到重载方法的区别以及自己该调用哪个。

### 2.4. 返回值

当 API 具有返回值时，应该在一组（或同一种类）的 API 中使用同样规则的返回值。

如果返回的结果是单个对象，当没有找到符合条件的结果时，应返回 null；如果返回值是 List、Map 或数组，那么应该返回空的 List、Map、数组，而不是 null。

当然这种方式不一定是适合所有场景的，也可以使用另外的一种通用规则替代，比如当查询不到符合条件的数据时抛出异常，这也是一种通用的方式，具体使用哪种，根据实际情况而定。

### 2.5. 协议

API 与用户的通信协议，总是使用 HTTPS 协议。

### 2.6. 域名

应该尽量将 API 部署在专用域名之下，如 https://api.example.com；

如果确定 API 很简单，不会有进一步扩展，可以考虑放在主域名下，如 https://example.org/api/；

### 2.7. 版本（Versioning）

应该将 API 的版本号放入 URL，如 https://api.example.com/v1/；

还有另一种做法是，将版本号放在 HTTP 头信息中，但不如放入 URL 方便和直观。Github 采用这种做法。

### 2.8. 路径（Endpoint）

路径又称"终点"（endpoint），表示 API 的具体网址。

在 RESTful 架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以 API 中的名词也应该使用复数。

例：有一个 API 提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。
```
https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
```

### 2.9. HTTP 动词

对于资源的具体操作类型，由 HTTP 动词表示。

常用的 HTTP 动词有下面五个（括号里是对应的 SQL 命令）。
```
GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
```
还有两个不常用的 HTTP 动词。
```
HEAD：获取资源的元数据。
OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。
```

例：
```
GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
```

### 2.10. 过滤信息（Filtering）

如果记录数量很多，服务器不可能都将它们返回给用户。API 应该提供参数，过滤返回结果。

例：
```
?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件
```
**参数的设计允许存在冗余**，即允许 API 路径和 URL 参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

### 2.11. 状态码（Status Codes）

服务器向用户返回的状态码和提示信息，完整状态吗列表参见：https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html

常见的有以下一些（方括号中是该状态码对应的 HTTP 动词）:
```
200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与 401 错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求 JSON 格式，但是只有 XML 格式）。
410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
```

### 2.12. 错误处理（Error handling）

如果状态码是`4xx`，就应该向用户返回出错信息。一般来说，返回的信息中将 error 作为键名，出错信息作为键值即可。

例：
```json
{
    error: "Invalid API key"
}
```

### 2.13. 返回结果类型

针对不同操作，服务器向用户返回的结果应该符合以下规范：
```
GET /collection：返回资源对象的列表（数组）
GET /collection/resource：返回单个资源对象
POST /collection：返回新生成的资源对象
PUT /collection/resource：返回完整的资源对象
PATCH /collection/resource：返回完整的资源对象
DELETE /collection/resource：返回一个空文档
```### Hypermedia API

RESTful API 最好做到 Hypermedia，即返回结果中提供链接，连向其他 API 方法，使得用户不查文档，也知道下一步应该做什么。

例：当用户向 api.example.com 的根目录发出请求，会得到这样一个文档：
```json
{
  "link": {
    "rel":   "collection https://www.example.com/zoos",
    "href":  "https://api.example.com/zoos",
    "title": "List of zoos",
    "type":  "application/vnd.yourformat+json"
  }
}
```
上面代码表示，文档中有一个 link 属性，用户读取这个属性就知道下一步该调用什么 API 了。rel 表示这个 API 与当前网址的关系（collection 关系，并给出该 collection 的网址），href 表示 API 的路径，title 表示 API 的标题，type 表示返回类型。

Hypermedia API 的设计被称为 [HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)。Github 的 API 就是这种设计，访问 api.github.com 会得到一个所有可用 API 的网址列表：
```json
{
  "current_user_url": "https://api.github.com/user",
  "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
```
从上面可以看到，如果想获取当前用户的信息，应该去访问 api.github.com/user，然后就得到了下面结果：
```json
{
  "message": "Requires authentication",
  "documentation_url": "https://developer.github.com/v3"
}
```
上面代码表示，服务器给出了提示信息，以及文档的网址。

### 2.14. 其它

（1）API 的身份认证应该使用 OAuth 2.0 框架。

（2）服务器返回的数据格式，应该尽量使用 JSON，避免使用 XML。

## 3. 身份认证

### 3.1. 基于 session 的身份认证

- session 的缺点的在上文中也提到了，但是假如采取 session 外置存储的策略，上边提到的问题都可以解决。既然这样，那**为什么要使用基于 token 的 sso 来替代 session 呢**？
  
  Session 外置已经可以解决集群模式的复杂性问题。基于 Token 的安全机制是新时代下新问题（RESTful API，微服务，Cloud 等）的解决方案。

  另一方面，在实际场景中，不可避免的会遇到和已有的 SSO 集成的问题，事实上很多时候集成是和 Seesion 外置一起做的。有 Session 之后就需要对 Session 对应的节点进行管理，需要有高可用的配置，需要在多个环境中维护该节点，而且在开发时还需要做详尽的测试。

  微服务架构下，服务本身更倾向于无状态，压根不使用 Session。客户通过 SSO，或者其他形式的鉴权获得一个 Token，每次请求时会带着这个 Token 来访问资源（通常是在 HTTP Header 上，比如 X-Auth-Token）。这个 Token 保证了所有状态其实是在客户端来维护了（有点像 Cookie，不过又避开了 CSRF 的问题）。这样后台服务更加专注业务本身，而不再考虑状态，鉴权问题。

### 3.2. 基于 Token 的身份认证

RESTful 登录设计（基于 Spring 及 Redis 的 Token 鉴权）：http://www.scienjus.com/restful-token-authorization/

**保护你的 API（上）**：http://icodeit.org/2016/05/about-session-and-security-api-1/

**保护你的 API（下）**：http://icodeit.org/2016/05/about-session-and-security-api-2/

编写 AccessToken 拦截器实现 token 认证： https://www.bysocket.com/?p=1080

#### 3.2.1. 交互流程

1. 客户端通过登录请求提交用户名和密码，服务端验证通过后生成一个 Token 与该用户进行关联，并将 Token 返回给客户端。

2. 客户端在接下来的请求中都会携带 Token，服务端通过解析 Token 检查登录状态。

3. 当用户退出登录、其他终端登录同一账号（被顶号）、长时间未进行操作时 Token 会失效，这时用户需要重新登录。

## 4. Refer Links

阮一峰：RESTful API 设计指南 http://www.ruanyifeng.com/blog/2014/05/restful_api.html

RESTful 登录设计（基于 Spring 及 Redis 的 Token 鉴权）：http://www.scienjus.com/restful-token-authorization/

RESTful Api 身份认证中的安全性设计探讨：http://www.jianshu.com/p/000c0b8628b9

如何实现 RESTful Web API 的身份验证 https://www.cnblogs.com/guogangj/archive/2013/01/18/2866537.html

**保护你的 API（上）**：http://icodeit.org/2016/05/about-session-and-security-api-1/

**保护你的 API（下）**：http://icodeit.org/2016/05/about-session-and-security-api-2/

浅谈如何设计 API：https://www.imooc.com/article/15611