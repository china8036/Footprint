- [服务端推送技术](#%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A8%E9%80%81%E6%8A%80%E6%9C%AF)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [长连接 (persistent connection)](#%E9%95%BF%E8%BF%9E%E6%8E%A5-persistent-connection)
  - [Comet](#comet)
    - [基于 AJAX 的长轮询 (long-polling)](#%E5%9F%BA%E4%BA%8E-ajax-%E7%9A%84%E9%95%BF%E8%BD%AE%E8%AF%A2-long-polling)
      - [传统轮询 /AJAX 轮询](#%E4%BC%A0%E7%BB%9F%E8%BD%AE%E8%AF%A2-ajax-%E8%BD%AE%E8%AF%A2)
      - [长轮询 (long-polling)](#%E9%95%BF%E8%BD%AE%E8%AF%A2-long-polling)
      - [比较](#%E6%AF%94%E8%BE%83)
    - [基于 Iframe 及 htmlfile 的流方式 (streaming)](#%E5%9F%BA%E4%BA%8E-iframe-%E5%8F%8A-htmlfile-%E7%9A%84%E6%B5%81%E6%96%B9%E5%BC%8F-streaming)
  - [客户端套接口](#%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%A5%97%E6%8E%A5%E5%8F%A3)
    - [Flash XMLSocket](#flash-xmlsocket)
    - [Java Applet 套接口](#java-applet-%E5%A5%97%E6%8E%A5%E5%8F%A3)
  - [Websocket](#websocket)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [数据帧](#%E6%95%B0%E6%8D%AE%E5%B8%A7)
    - [握手协议](#%E6%8F%A1%E6%89%8B%E5%8D%8F%E8%AE%AE)
    - [浏览器支持](#%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81)
    - [安全](#%E5%AE%89%E5%85%A8)
    - [意义](#%E6%84%8F%E4%B9%89)
    - [优缺点](#%E4%BC%98%E7%BC%BA%E7%82%B9)
    - [使用](#%E4%BD%BF%E7%94%A8)
      - [Font End](#font-end)
        - [W3C API](#w3c-api)
          - [基本用法](#%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
      - [Server End](#server-end)
        - [Node](#node)
        - [Bash](#bash)
        - [Python](#python)
        - [Java:JSR 356](#javajsr-356)
      - [使用 websocket 传输图片和语音](#%E4%BD%BF%E7%94%A8-websocket-%E4%BC%A0%E8%BE%93%E5%9B%BE%E7%89%87%E5%92%8C%E8%AF%AD%E9%9F%B3)
  - [Server-sent events(SSE)](#server-sent-eventssse)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [工作原理](#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
    - [浏览器支持](#%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81)
    - [与 websocket 的比较](#%E4%B8%8E-websocket-%E7%9A%84%E6%AF%94%E8%BE%83)
    - [使用](#%E4%BD%BF%E7%94%A8)
      - [Font End](#font-end)
      - [Server End](#server-end)
  - [Refer Links](#refer-links)

# 服务端推送技术

## 概述

推送技术，一种基于 Internet 通信方式的服务器推送，要求通信的请求是由发布者或中央服务器发起；

一般的 pull/get 通信模型中，信息传输的相应一般由接收者或客户端发起；而在 publish/subscribe 通信模型中，客户通过订阅由服务器提供各种信息的频道，不论何时都可以在其中一个频道得到新的内容，同样服务器通过推送把信息传递给相应的客户端。

应用场景：
1)	即时监控系统：后台硬件热插拔、LED、温度、电压发生变化；
2)	即时通信系统：其它用户登录、发送信息；
3)	即时报价系统：后台数据库内容发生变化；

这些应用的解决方案可分为两类：
1)	一类需要在浏览器端安装插件，基于套接口传送信息，或是使用 RMI、CORBA 进行远程调用；
2)	一类则无须浏览器安装任何插件、基于 HTTP 长连接；

应用到 WEB 中，首先考虑的是如何在功能有限的浏览器端接收、处理信息：
1)	客户端如何接收、处理信息，是否需要使用套接口或是使用远程调用。客户端呈现给用户的是 HTML 页面还是 Java applet 或 Flash 窗口。如果使用套接口和远程调用，怎么和 JavaScript 结合修改 HTML 的显示。
2)	客户与服务器端通信的信息格式，采取怎样的出错处理机制。
3)	客户端是否需要支持不同类型的浏览器如 IE、Firefox，是否需要同时支持 Windows 和 Linux 平台。

## 长连接 (persistent connection)

基于长连接的服务端推送主要通过 Comet 技术实现。

## Comet

Comet 是一种基于 HTTP 长连接、无须在浏览器端安装插件的 web 服务端推送技术，能使服务器实时地将更新的信息传送到客户端，而无须客户端发出请求，目前有两种实现方式，基于 AJAX 的长轮询和基于 Iframe 及 htmlfile 的流方式。

### 基于 AJAX 的长轮询 (long-polling)

#### 传统轮询 /AJAX 轮询

客户端定时向服务器发送 http(ajax) 请求，服务器接到请求后马上返回响应信息并关闭连接。

优点：后端程序编写比较容易。

缺点：请求中有大半是无用，浪费带宽和服务器资源。

实例：适于小型应用。

#### 长轮询 (long-polling)

客户端像传统轮询一样从服务器请求数据。然而，如果服务器没有可以立即返回给客户端的数据，则不会立刻返回一个空结果，而是保持这个请求等待数据到来（或者恰当的超时），之后将数据作为结果返回给客户端。

优点：在无消息的情况下不会频繁的请求。

缺点：服务器保持连接会消耗资源。

实例：WebQQ、Hi 网页版、Facebook IM。

#### 比较

基于 AJAX 的长轮询与传统的 AJAX 应用都使用了 AJAX，它们的不同之处在于：

1)	服务器端会阻塞请求直到有数据传递或超时才返回。

2)	客户端 JavaScript 响应处理函数会在处理完服务器返回的信息后，再次发出请求，重新建立连接。

3)	当客户端处理接收的数据、重新建立连接时，服务器端可能有新的数据到达；这些信息会被服务器端保存直到客户端重新建立连接，客户端会一次把当前服务器端所有的信息取回。

### 基于 Iframe 及 htmlfile 的流方式 (streaming)

iframe 流方式是在页面中插入一个隐藏的 iframe，利用其 src 属性在服务器和客户端之间建立一条长链接，服务器向 iframe 传输数据（通常是 HTML，内有负责插入信息的 javascript），来实时更新页面。

优点：消息即时到达，不发无用请求；浏览器兼容好（兼容 IE）；

缺点：服务器维护一个长连接会增加开销；

实例：Google Talk，Gmail；

实现：http://liuwanlin.info/shi-shi-kua-yu-tong-xin-iframe/ 

## 客户端套接口

基于客户端套接口的“服务器推送”技术主要有 Flash XMLSocket 与 Java Applet 套接口；

### Flash XMLSocket

http://en.wikipedia.org/wiki/XMLSocket 

这种方案实现的基础是：
1)	Flash 提供了 XMLSocket 类。
2)	JavaScript 和 Flash 的紧密结合：在 JavaScript 可以直接调用 Flash 程序提供的接口。

在 JavaScript 控制下，客户端建立一个 TCP 连接到服务器上的单向中继。中继服务器不会从这个套接字读取任何内容，相反它会向客户端发送一个唯一标识符。接下来客户机向 web 服务器发送 HTTP 请求，包括这个标识符。Web 应用程序可以将向客户机发送的消息发送到中继服务器的本地接口，然后中继服务器通过 Flash 套接字传递给客户机。这种方法的优点是，它能鉴别许多 web 应用程序的典型的读写不对称，包括聊天，它还提供了更高的效率。由于它不接受传出套接字的数据，所以中继服务器根本不需要轮着传出 TCP 连接，使其保持数万个并发连接成为可能。在这个模型中扩展的限制是底层服务器操作系统的 TCP 堆栈。

实现：在页面中内嵌入一个使用了 Socket 类的 Flash 程序，JavaScript 通过调用此 Flash 程序提供的 Socket 接口与服务器端的 Socket 接口进行通信，JavaScript 在收到服务器端传送的信息后控制页面的显示。

优点：实现真正的即时通信，而不是伪即时。

实例：在一些网络聊天室，网络互动游戏中已得到广泛使用。

不足：
1)	客户端必须安装 Flash 播放器；
2)	因为 XMLSocket 没有 HTTP 隧道功能，XMLSocket 类不能自动穿过防火墙；
3)	因为是使用套接口，需要设置一个通信端口，防火墙、代理服务器也可能对非 HTTP 通道端口进行限制；

### Java Applet 套接口

在客户端使用 Java Applet，通过 java.net.Socket 或 java.net.DatagramSocket 或 java.net.MulticastSocket 建立与服务器端的套接口连接，从而实现“服务器推送”。

这种方案最大的不足在于 Java Applet 在收到服务器端返回的信息后，无法通过 JavaScript 去更新 HTML 页面的内容。

## Websocket

http://blog.csdn.net/fenglibing/article/details/7108982 

http://liuwanlin.info/shi-shi-kua-yu-tong-xin-websocket/ 

https://www.html5rocks.com/zh/tutorials/websockets/basics/#toc-serverside 

[websocket 详细解释系列教程](https://blog.lyz810.com/article/2016/06/websocket-api-introduce/)

[WebSocket 数据帧的介绍](http://blog.csdn.net/fenglibing/article/details/6852497)

[Web Sockets 与代理服务器交互的问题](http://www.infoq.com/cn/articles/Web-Sockets-Proxy-Servers)

[RFC6455](https://datatracker.ietf.org/doc/rfc6455/)

### 概述

WebSocket 一种建立在单个 TCP 连接上进行全双工通讯的协议。WebSocket 通信协议于 2011 年被 IETF 定为标准 RFC 6455，并被 RFC7936 所补充规范。HTML5 中的 WebSocket API 也被 W3C 定为标准。

[维基百科](https://zh.wikipedia.org/wiki/Comet_(web%E6%8A%80%E6%9C%AF))：
>	在 HTML5 标准中，定义了客户端和服务器通讯的 WebSocket 方式，在得到浏览器支持以后，WebSocket 将会取代 Comet 成为服务器推送的方法，目前 chrome、Firefox、Opera、Safari 等主流版本均支持，Internet Explorer 从 10 开始支持。

Websocket 借用了 HTTP 协议来完成一部分握手过程，因此，Websocket 和 HTTP 有关系，但是关系不大，可以用交集来表示：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/b27507ecca2e347c12655c11e9ce1f6a.jpg)

Websocket 使用 ws 或 wss 的统一资源标志符，其中 wss 表示在 TLS 之上的 Websocket（类似 https)。如：
```
ws://example.com/wsapi
wss://secure.example.com/
```

WebSocket 使用标准的对防火墙友好的 80 及 443 端口。建立连接时，使用 HTTP Upgrade 机制升级到 Web Socket 协议，有着兼容 HTTP 的握手机制，因此 HTTP 服务器可以与 WebSocket 服务器共享默认的 HTTP 与 HTTPS 端（80 和 443）。

### 数据帧

官方文档（RFC-6455）提供的一个结构图：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/d872367140877c571356ab47b491b63f.jpg)

```
FIN      1bit 表示信息的最后一帧，flag，也就是标记符  
RSV 1-3  1bit each 以后备用的 默认都为 0  
Opcode   4bit 帧类型  
Mask     1bit 掩码，是否加密数据，默认必须置为 1 （这里很蛋疼）  
Payload  7bit 数据的长度  
Masking-key      1 or 4 bit 掩码  
Payload data     (x + y) bytes 数据  
Extension data   x bytes  扩展数据  
Application data y bytes  程序数据  
```

### 握手协议

[websocket 三种握手方式](http://blog.csdn.net/fenglibing/article/details/7100070)

为了建立 Websocket 连接，需要通过浏览器发出请求，之后服务器进行回应，这个过程通常称为“握手”（handshaking）。

在 WebSocket API 中，浏览器和服务器通过 HTTP/1.1 协议的 101 状态码进行握手，只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输，直到客户端或者服务器端的某一方主动的关闭连接。

- 典型的 Websocket 握手示例： 
  
  一个 WebSocket 连接是在客户端与服务器之间 HTTP 协议的初始握手阶段将其升级到 Web Socket 协议来建立的，其**底层仍是 TCP/IP 连接**。

  客户端请求
  ```
  GET / HTTP/1.1
  Upgrade: websocket
  Connection: Upgrade
  Host: example.com
  Origin: http://example.com
  Sec-WebSocket-Key: sN9cRrP/n9NdMgdcy2VJFQ==
  Sec-WebSocket-Version: 13
  ```
  服务器回应
  ```
  HTTP/1.1 101 Switching Protocols
  Upgrade: websocket
  Connection: Upgrade
  Sec-WebSocket-Accept: fFBooB7FAkLlXgRSz0BT3v4hq5s=
  Sec-WebSocket-Location: ws://example.com/
  ```
  字段说明：
  1)	Connection 必须设置 Upgrade，表示客户端希望连接升级；
  2)	Upgrade 字段必须设置 Websocket，表示希望升级到 Websocket 协议；
  3)	Sec-WebSocket-Key 是随机的字符串，服务器端会用这些数据来构造出一个 SHA-1 的信息摘要。把“Sec-WebSocket-Key”加上一个特殊字符串“258EAFA5-E914-47DA-95CA-C5AB0DC85B11”，然后计算 SHA-1 摘要，之后进行 BASE-64 编码，将结果做为“Sec-WebSocket-Accept”头的值，返回给客户端。如此操作，可以尽量避免普通 HTTP 请求被误认为 Websocket 协议；
  4)	Sec-WebSocket-Version 表示支持的 Websocket 版本。RFC6455 要求使用的版本是 13，之前草案的版本均应当被弃用；
  5)	Origin 字段是可选的，通常用来表示在浏览器中发起此 Websocket 连接所在的页面，类似于 Referer。但是，于 Referer 不同的是，Origin 只包含了协议和主机名称；
  6)	其他一些定义在 HTTP 协议中的字段，如 Cookie 等，也可以在 Websocket 中使用；

  一旦连接被创建，客户端和服务端可以互相以全双工的方式发送 WebSocket 数据或者文本帧。WebSocket 传输也成为消息，一个单独的消息可以被任意分成多个数据帧。

### 浏览器支持

各大浏览器支持情况：Firefox6，Safari6，Chrome4，Opera12.10，IE10 都实现了一个安全版本的 WebSocket 协议。

https://caniuse.com/#search=websockets 

2017/07/23
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/82e5f7807232117d111433f8ebde195f.jpg)

客户端 JavaScript 检测是否支持 websocket：
```javascript
// 注意：在 Android 中，即使浏览器不支持 WebSocket，但浏览器 window 对象仍存在 WebSocket 对象，因此不能简单通过判断全局对象中是否包含 WebSocket 对象来判断；
$(document).ready(function() {
  if (!window.WebSocket || !window.WebSocket.prototype.send) {
    $('body').html("<h1>Error</h1><p>Your browser does not support HTML5 Web Sockets. Try Google Chrome instead.</p>");
  }
});
```

### 安全

作为现代协议，**跨源通信已内置在 WebSocket 中。WebSocket 可实现任何域上多方之间的通信**。服务器将决定是向所有客户端，还是只向驻留在一组指定域上的客户端提供服务。

从安全角度来说，在服务端建立连接的过程中校验 Origin 头是很重要的，这样可以避免跨站点的 WebSocket 劫持攻击，当连接经过 Cookie 或者 HTTP 认证校验之后，这种攻击是可能的。发送敏感数据时，最好使用 token 或者类似的保护机制来校验 WebSocket 连接。

### 意义

https://cnodejs.org/topic/5680fa00952147b71ea37144 

- Q：既然 HTTP 通过 keep-alive 实现了长连接，产生 websocket 协议的意义是什么？

  在 HTTP 协议中，keep-alive connection 是指在一次 TCP 连接中完成多个 HTTP 请求即复用 TCP 连接，但是对每个请求仍然要单独发 header；而在 polling 中，需要从客户端不断主动的向服务器发 HTTP 请求查询是否有新数据。这两种模式有一个共同的缺点，就是除了真正的数据部分外，服务器和客户端还要大量交换 HTTP header，信息交换效率很低。因此，它们建立的“长连接”都是“伪长连接”，优点是不需要对现有的 HTTP server 和浏览器架构做修改就能实现。

  HTTP 长连接使用的 keep-alive 只是一种为了达到复用 TCP 连接的“协商”行为，双方并没有建立正真的连接会话，**任何一方都可以不认可，可以随时（在任何一次请求完成后）关闭**；

  HTTP 协议决定了浏览器端总是主动发起方，而服务端总是被动的接受、响应请求，从不主动，不利于复杂业务的实现；

因此，websocket 协议的意义如下：

1)	WebSocket 中，通过第一个 HTTP request 建立了 TCP 连接之后，之后的交换数据都不需要再发 HTTP request，从而使得这个长连接变成了一个“真正的长连接”。也正因为 websocket 是一个与 HTTP 协议不同的协议，所以它需要对服务器和客户端都进行升级才能实现；
2)	WebSocket 建立了一个双通道的真实会话连接，且两边都必须要维持住连接的状态；
3)	WebSocket 在连接建立之后，客户端、服务端是完全平等的，不存在主动、被动之说；
4)	此外还有 multiplexing 功能，几个不同的 URI 可以复用同一个 WebSocket 连接。这些都是原来的 HTTP 不能做到的；

### 优缺点

- 优点
  
  - 较少的控制开销。在连接创建后，服务器和客户端之间交换数据时，用于协议控制的数据包头部相对较小。在不包含扩展的情况下，对于服务器到客户端的内容，此头部大小只有 2 至 10 字节（和数据包长度有关）；对于客户端到服务器的内容，此头部还需要加上额外的 4 字节的掩码。相对于 HTTP 请求每次都要携带完整的头部，此项开销显著减少了。
  
  - 更强的实时性。由于协议是全双工的，所以服务器可以随时主动给客户端下发数据。相对于 HTTP 请求需要等待客户端发起请求服务端才能响应，延迟明显更少；即使是和 Comet 等类似的长轮询比较，其也能在短时间内更多次地传递数据。
  
  - 保持连接状态。与 HTTP 不同的是，Websocket 需要先创建连接，这就使得其成为一种有状态的协议，之后通信时可以省略部分状态信息，而 HTTP 请求可能需要在每个请求都携带状态信息（如身份认证等）。
  
  - 更好的二进制支持。Websocket 定义了二进制帧，相对 HTTP，可以更轻松地处理二进制内容。
  
  - 可以支持扩展。Websocket 定义了扩展，用户可以扩展协议、实现部分自定义的子协议。如部分浏览器支持压缩等。
  
  - 更好的压缩效果。相对于 HTTP 压缩，Websocket 在适当的扩展支持下，可以沿用之前内容的上下文，在传递类似的数据时，可以显著地提高压缩率。
  
  - 支持跨源通信，没有同源限制，客户端可以与任意服务器通信。服务器可决定是向所有客户端，还是只向驻留在一组指定域上的客户端提供服务。

- 缺点

  - 最大的问题就是浏览器兼容性问题。低版本 IE 浏览器不支持该技术，直到 IE10 才开始支持 WebSocket 技术。解决方案是对于低版本浏览器可以使用 Flash 来模拟 WebSocket。

  - WebSocket 在用于双向传输、推送消息方面能够做到灵活、简便、高效，但在普通的 Request-Response 过程中并没有太大用武之地，比起普通的 HTTP 请求来反倒麻烦了许多，甚至更为低效。

### 使用

#### Font End

##### W3C API

[W3C API 定义](http://dev.w3.org/html5/websockets/ )

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/c58ebf498c8170f9a4a6c76e5238a764.jpg)

```
enum BinaryType { "blob", "arraybuffer" };
[Constructor(DOMString url, optional (DOMString or DOMString[]) protocols)]
interface WebSocket : EventTarget {
  readonly attribute DOMString url;
  // ready state
  const unsigned short CONNECTING = 0;
  const unsigned short OPEN = 1;
  const unsigned short CLOSING = 2;
  const unsigned short CLOSED = 3;<br>
  readonly attribute unsigned short readyState;
  readonly attribute unsigned long bufferedAmount;
  // networking
          attribute EventHandler onopen;
          attribute EventHandler onerror;
          attribute EventHandler onclose;
  readonly attribute DOMString extensions;
  readonly attribute DOMString protocol;
  void close([Clamp] optional unsigned shortcode, optional DOMString reason);
  // messaging
          attribute EventHandler onmessage;
          attribute BinaryType binaryType;
  void send(DOMString data);
  void send(Blob data);
  void send(ArrayBuffer data);
  void send(ArrayBufferView data);
};
```

###### 基本用法

https://www.cnblogs.com/shijiaqi1066/p/3795075.html 

https://developer.mozilla.org/en-US/docs/Web/API/WebSocket 

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据：
- WebSocket 构造函数
  ```javascript
  var Socket = new WebSocket(url, [protocol] );
  ```
  其中的第一个参数 url, 指定连接的 URL；
  第二个参数 protocol 是可选的，指定了可接受的子协议，既可以是字符串，也可以是字符串数组，如“['soap', 'xmpp']“。每个字符串都应代表一个子协议名称，而服务器只能接受数组中通过的一个子协议。访问 WebSocket 对象的 protocol 属性可确定接受的子协议。；

  对象构造后，客户端就会与服务器进行连接；
  例：
  ```javascript
  var wsUri ="ws://echo.websocket.org/";
  websocket = new WebSocket(wsUri);
  ```

- WebSocket 属性

  - readyState

    只读属性 readyState 表示连接状态，取值：
    - CONNECTING / 0：表示连接尚未建立。
    - OPEN / 1：表示连接已建立，可以进行通信。
    - CLOSING / 2：表示连接正在进行关闭。
    - CLOSED / 3：表示连接已经关闭或者连接不能打开。

  - bufferedAmount

    只读属性 bufferedAmount 表示已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数，用来判断发送是否结束。

    e.g.
    ```javascript
    var data = new ArrayBuffer(10000000);
    socket.send(data);

    if (socket.bufferedAmount === 0) {
      // 发送完毕
    } else {
      // 发送还没结束
    }
    ```
- WebSocket 事件
  
  WebSocket 对象一共支持四个事件响应方法 onopen, onmessage, onclose 和 onerror（这样不会阻塞 UI，得到更好的用户体验）：
  1)	当浏览器和服务器连接成功后，会触发**onopen 事件**
  ```javascript
  websocket.onopen = function(evt) {};
  ```
  2)	如果连接失败，发送、接收数据失败或者处理数据出现错误，会触发**onerror 事件**
  ```javascript
  websocket.onerror = function(evt) {};
  ```
  3)	当浏览器接收到服务器发送过来的数据时，就会触发**onmessage 事件**，参数 evt 中包含服务器传输过来的数据
  ```javascript
  websocket.onmessage = function(evt) {};
  ```
  
  注意，服务器数据可能是文本，也可能是二进制数据（blob 对象或 Arraybuffer 对象）： 
  ```javascript
  ws.onmessage = function(event){
      // 动态判断收到的数据类型
  if(typeof event.data === String) {
        console.log("Received data string");
      }

      if(event.data instanceof ArrayBuffer){
        var buffer = event.data;
        console.log("Received arraybuffer");
      }
  }
  // 除了动态判断收到的数据类型，也可以使用 binaryType 属性，显式指定收到的二进制数据类型，可将 binaryType 属性设为“blob”或“arraybuffer”，默认格式为“blob”
  ```

  4)	当浏览器接收到服务器发送的关闭连接请求时，就会触发**onclose 事件**
  ```javascript
  websocket.onclose = function(evt) {};
  ```

- 如果要指定多个回调函数，可以使用 addEventListener 方法：
  ```javascript
  ws.addEventListener('open', function (event) {
    ws.send('Hello Server!');
  });

  ws.addEventListener("close", function(event) {
    var code = event.code;
    var reason = event.reason;
    var wasClean = event.wasClean;
    // handle close event
  });

  ws.addEventListener(“error”, function(event) {
    //handle error
  })

  ws.addEventListener("message", function(event) {
    var data = event.data;
    // 处理数据
  });

  ```

- WebSocket 方法

  1)	通过 send() 方法在连接过程中，向服务器发送消息

      发送字符串：
      ```javascript
      ws.send('your message');
      ```
      发送 Blob 对象：
      ```javascript
      var file = document
        .querySelector('input[type="file"]')
        .files[0];
      ws.send(file);
      ```
      发送 ArrayBuffer 对象：
      ```javascript
      // Sending canvas ImageData as ArrayBuffer
      var img = canvas_context.getImageData(0, 0, 400, 320);
      var binary = new Uint8Array(img.data.length);
      for (var i = 0; i < img.data.length; i++) {
        binary[i] = img.data[i];
      }
      ws.send(binary.buffer);
      ```

  2)	通过 close() 方法从客户端关闭连接

- 实例

  ```javascript
  window.onload = function () {
      var websocket = null;

      // websocket = new SockJS("http://127.0.0.1:8080/recordHandle/sockjs");

      if ('WebSocket' in window) {
          websocket = new WebSocket("ws://127.0.0.1:8080/recordHandle");
      } else if ('MozWebSocket' in window) {
          websocket = new MozWebSocket("ws://127.0.0.1:8080/recordHandle");
      } else {
          // websocket = new SockJS("http://127.0.0.1:8080/recordHandle/sockjs");
      }

      websocket.onopen = function (event) {
          document.getElementById("info").innerHTML = "连接成功";
          console.log("Connected to WebSocket server.");

      };

      websocket.onclose = function (event) {
          document.getElementById("info").innerHTML = "连接关闭";
          console.log("Disconnected from WebSocket server.");

      };

      websocket.onmessage = function(event) {
          obj = JSON.parse(event.data);
          document.getElementById("id_v").innerHTML       = obj.id;
          document.getElementById("time_v").innerHTML     = obj.time;
          document.getElementById("var1_v").innerHTML     = obj.autoChangeVar1;
          document.getElementById("var2_v").innerHTML     = obj.autoChangeVar2;
          document.getElementById("var3_v").innerHTML     = obj.autoChangeVar3;
      };

      websocket.onerror = function (event) {
          document.getElementById("info").innerHTML = "连接出错";
          console.log('Error occured: ' + event.data);

      };

      // 监听窗口关闭事件，窗口关闭前，主动关闭 websocket 连接，防止连接还没断开就关闭窗口，server 端会抛异常
      window.onbeforeunload = function () {
          websocket.close();
      }

  };

  ```

#### Server End

##### Node

常用的 WebSocket 服务端 Node 实现有以下三种：

a)	µWebSockets

b)	Socket.IO

c)	WebSocket-Node

##### Bash

实例：

Bash 脚本读取客户端输入的例子
```shell
#!/bin/bash

while read LINE
do
	echo "Hello $LINE!"
done
```
Bash 五行代码实现一个最简单的聊天服务器
```shell
#!/bin/bash

# Copyright 2013 Jeroen Janssens
# All rights reserved.

echo "Please enter your name:"; read USER
echo "[$(date)] ${USER} joined the chat" >> chat.log
echo "[$(date)] Welcome to the chat ${USER}!"
tail -n 0 -f chat.log --pid=$$ | grep --line-buffered -v "] ${USER}>" &
while read MSG; do echo "[$(date)] ${USER}> ${MSG}" >> chat.log; done
```

##### Python

实例项目：

websocketd：命令行的 WebSocket 代理，可以通过它使浏览器与服务器命令行进行 WebSocket 通信；

回声服务 greeter.js；

##### Java:JSR 356

[JSR 356 (Java API for WebSocket)](http://www.oracle.com/technetwork/cn/articles/java/jsr356-1937161-zhs.html)

http://colobu.com/2015/02/27/WebSockets-tutorial-on-Wildfly-8/ 

Java EE 对 websocket 的实现主要封装在 JEE JSR356 标准规范 API 中，即 javax.websocket.* 与 javax.websocket.server.* 中；

- 导入依赖 (maven)
  ```xml
  <dependency>
      <groupId>javax.websocket</groupId>
      <artifactId>javax.websocket-api</artifactId>
      <version>1.1</version>
      <scope>provided</scope>
  </dependency>
  ```

- 实现接口方式 (Interface-driven)

  通过实现 WebSocketServlet 接口，实际上就是实现了一个 Servlet，开发者可以实现周期的各个方法；
  ```java
  public class EchoServlet extends WebSocketServlet {
    @Override
    protected StreamInbound createWebSocketInbound(String subProtocol,
      HttpServletRequest request) {
        // 以下代码省略。...
      return new MessageInbound() {
        // 以下代码省略。...
    }
    protected void onBinaryMessage(ByteBuffer buffer) throws IOException {
        // 以下代码省略。..
    }
    protected void onTextMessage(CharBuffer buffer) throws IOException {
        getWsOutbound().writeTextMessage(buffer);
        // 以下代码省略。..
    }
  }
  ```

- 注解方式 (Annotation-driven)

  通过在 POJO 加上 @ServerEndpoint 注解，告诉容器此类应该作为 WebSocket 服务器的端点 (Endpoint)，开发者就在此端点中可以处理 WebSocket 生命周期事件（推荐使用）；
  ```java
  import javax.websocket.*;
  import javax.websocket.server.ServerEndpoint;

  @ServerEndpoint("/echo") 
  // VALUE 值为 EndPoint 的 path
  // 表示将 WebSocket 服务端运行在 ws://[Server 端 IP 或域名』:[Server 端口』/echo 的访问端点，客户端浏览器可对此 URL 发起连接请求
  // 此路径可包括后续方法调用中所用的路径参数；例如，/hello/{userid} 是一个有效路径，其中的 {userid} 值可以使用 @PathParam 批注在生命周期方法调用中获取
  public class EchoEndpoint {
    public void EchoEndPoint() {}
    // 使用 ServerEndpoint 注释的类必须有一个公共的无参数构造函数

    @OnOpen
    public void onOpen(Session session) throws IOException {
      // 一旦建立 WebSocket 连接，将创建一个 Session，且将调用 @OnOpen 批注的方法，该方法可以包含参数：
      // javax.websocket.Session 参数，指定创建的 Session
      // EndpointConfig 实例，包含有关端点配置的信息
      // 零个或更多个使用 @PathParam 批注的字符串参数，指向端点路径上的路径参数
      System.out.println("WebSocket opened: " + session.getId());
    }
    
    @OnMessage
    public String onMessage(String message, Session session) {
      // 当 WebSocket 端点收到消息时，将调用 @OnMessage 批注的方法，该方法可以包含参数：
      // 1. javax.websocket.Session ；
      // 2. 零个或更多个使用 @PathParam 批注的字符串参数，指向端点路径上的路径参数；
      // 3. 消息本身，这个信息可以是文本格式，也可以是二进制格式；

      // 向客户端推送消息 -- 方式一
      // 如果以 @OnMessage 批注的方法的返回类型不是 void，则 WebSocket 将返回值发送给发送方
      System.out.println("Received : "+ message);
      return message.toUpperCase();

      // 向客户端推送消息 -- 方式二
      // Session 实例上的 getBasicRemote() 方法返回 WebSocket 另一个连接端：RemoteEndpoint
      // 该 RemoteEndpoint 实例可用于发送文本或其他类型的消息
      RemoteEndpoint.Basic remoteEndpoint= session.getBasicRemote();
      remoteEndpoint.sendText ("Hello, world");
    }

    @Message(maxMessageSize=6)
    // @Message 注释可对消息传输进行高级定制
    // MaxMessageSize 属性定义了消息字节最大限制，如果超过 6 个字节的信息被接收，就报告错误和连接关闭
    public void receiveMessage(String s) {
      // 以下代码省略。..
    } 

    @OnError
    public void onError(Throwable t) {
      // 以下代码省略。..
    }
    
    @OnClose
    public void onClose(Session session, CloseReason reason) {
      // 在连接被终止时调用，参数 closeReason 可封装更多细节，如为什么一个 WebSocket 连接关闭
      System.out.println("Closing a WebSocket due to " + reason.getReasonPhrase());
    } 
  }

  ```

- 编码器和解码器

  当需要传递更复杂类型的数据时，就需要使用编码器和解码器。

  使用批注驱动的模型时，对于每种不同类型的消息，允许一种 @onMessage 批注方法。允许在批注方法中指定消息内容的参数依赖于消息的类型。

-	消息类型

  消息类型主要分为三种：
  - 基于文本的消息（如 XML、JSON 等）；
  - 二进制消息（如图片、文件等）；
  - Pong 消息，关于 WebSocket 连接本身；

- 编码 / 解码器实现

  编码器定义为 javax.websocket.Encoder 接口的实现，解码器是 javax.websocket.Decoder 接口的实现：

  Encoder 接口有四个子接口：
  - Encoder.Text，用于将 Java 对象转换成文本消息
  - Encoder.TextStream，用于向字符流添加 Java 对象
  - Encoder.Binary，用于将 Java 对象转换成二进制消息
  - Encoder.BinaryStream，用于向二进制流添加 Java 对象

  Decoder 接口有四个子接口：
  - Decoder.Text，用于将文本消息转换成 Java 对象
  - Decoder.TextStream，用于从字符流读取 Java 对象
  - Decoder.Binary，用于将二进制消息转换成 Java 对象
  - Decoder.BinaryStream，用于从二进制流读取 Java 对象

  编码器实现规范
  ```java
  class MessageEncoder implements Encoder.Text<MyJavaObject> {
    @override
    public String encode(MyJavaObject obj) throws EncodingException {
        ...
    }
  }
  ```

  解码器实现规范
  ```java
  class MessageDecoder implements Decoder.Text<MyJavaObject> {
    @override 
    public MyJavaObject decode (String src) throws DecodeException {
        ...
    }

    @override 
    public boolean willDecode (String src) {
        // return true if we want to decode this String into a MyJavaObject instance
    }
  }
  ```

  实例：

  - 将 XML 转换为 POJO
    ```java
    public class MessageDecoder implements Decoder.Text<Person> {
    
        @Override
        public Person decode(String s) {
            System.out.println("Incoming XML " + s);
            Person person = null;
            JAXBContext jaxbContext;
            try {
                jaxbContext = JAXBContext.newInstance(Person.class);
    
                Unmarshaller unmarshaller = jaxbContext.createUnmarshaller();
    
                StringReader reader = new StringReader(s);
                person = (Person) unmarshaller.unmarshal(reader);
            } catch (Exception ex) {
                ex.printStackTrace();
            }
            return person;
        }
    
        @Override
        public boolean willDecode(String s) {
            return (s != null);
        }
    
        @Override
        public void init(EndpointConfig endpointConfig) {
            // do nothing.
        }
    
        @Override
        public void destroy() {
            // do nothing.
        }
    }
    ```
  - 将 POJO 转换为 XML
    ```java
    public class MessageEncoder implements Encoder.Text<Person> {
    
        @Override
        public String encode(Person object) throws EncodeException {
    
            JAXBContext jaxbContext = null;
            StringWriter st = null;
            try {
                jaxbContext = JAXBContext.newInstance(Person.class);
    
                Marshaller marshaller = jaxbContext.createMarshaller();
                st = new StringWriter();
                marshaller.marshal(object, st);
                System.out.println("OutGoing XML " + st.toString());
    
            } catch (Exception ex) {
                ex.printStackTrace();
            }
            return st.toString();
        }
    
        @Override
        public void init(EndpointConfig endpointConfig) {
            // do nothing.
        }
    
        @Override
        public void destroy() {
            // do nothing.
        }
    }
    ```

- 信息传输实现

  当 Websocket EndPoint 匹配的 URL 接收到从客户端传来的信息时，Websocket 会自动使用 Decoder 将文本消息转换成 Java 对象，然后传给 @OnMessage 方法处理；

  当要给客户端传递信息时，Websocket 会将 POJO 对象写入到 session 中，自动使用 Encoder 将 Java 对象转换成文本，再发送给客户端。

  实例：
  ```java
  // 注册 Encoder/Decoder 只需在 @ServerEndpoint 注解中增加 encoder/decoder 设置
  @ServerEndpoint(value = "/hello",
          decoders = {
              MessageDecoder.class,},
          encoders = {
              MessageEncoder.class
          })
  public class HelloWorldEndpoint {
  
      @OnMessage
      public Person hello(Person person, Session session) {
          if (person.getName().equals("john")) {
              person.setName("Mr. John");
          }
          try {
        // 直接发送 POJO 对象，Encoder 会自动在发送前将 POJO 转换为 string
              session.getBasicRemote().sendObject(person);
              System.out.println("sent ");
          } catch (Exception ex) {
              Logger.getLogger(HelloWorldEndpoint.class.getName()).log(Level.SEVERE, null, ex);
          }
          return person;
  
      }
  
      @OnOpen
      public void myOnOpen(Session session) {
      }
  
  }
  ```

  [使用消息队列，将每一个 session 都按顺序保存，之后通过 sessionid 来精准推送特定消息](http://www.voidcn.com/blog/yingxiake/article/p-5783312.html)
  ```java
  @ServerEndpoint(value = "/echo")
  public class PriceServer {
    private Set<Session> sessions = Collections.synchronizedSet(new HashSet<Session>());
    @OnOpen
    public void onOpen(Session session) {
      sessions.add(session);
    }
    @OnClose
    public void onClose(Session session) {
      sessions.remove(session);
    }
    @OnMessage
    public void onMessage(String message, Session session) {
      System.out.println("Message Received: " + message);
      for (Session remote : sessions) {
        System.out.println("Sending to " + remote.getId());
        remote.getAsyncRemote().sendText(message);
      }
    }
  }
  ```

####	使用 websocket 传输图片和语音

http://www.alloyteam.com/2015/12/websockets-ability-to-explore-it-with-voice-pictures/ 

## Server-sent events(SSE)

### 概述

http://www.ruanyifeng.com/blog/2017/05/server-sent_events.html 

https://liangbizhi.github.io/websocket-sse-use-diff-1/ 

维基百科：
> Server-sent events (SSE) is a technology where a browser receives automatic updates from a server via HTTP connection. The Server-Sent Events EventSource API is standardized as part of HTML5 by the W3C.

### 工作原理

- 客户端通过 new EventSource(url) 向服务端发起 HTTP Request：
  ```
  Accept:text/event-stream
  Connection:keep-alive
  ```

- 服务器接收到请求后，与客户端建立连接，但不一定会立即返回（此时浏览器里请求状态是 pending），而是进行业务逻辑处理，直到需要返回时才携带 data 向客户端返回 HTTP Response：
  ```
  Content-Type:text/event-stream;charset=UTF-8
  Cache-Control: no-cache
  Connection: keep-alive
  ```

  其中：
  - event-stream 是一个简单的文本流，必须使用 UTF-8 进行编码；若不是 UTF-8，会导致连接被 abandon；

  - 每一个消息由若干个 message 组成，每个 message 之间用、n\n 分隔，即一次传输的 data 由一行或多行文本组成，它们列出了消息的字段，每一个字段由字段名，一个冒号（:），以及该字段的文本数据表示，以下的字段名都是规范中定义的：
    - event：事件的类型。如果该字段被指定，监听该事件的浏览器监听器将会被调用。浏览器可以使用 addEventListener() 来监听命名事件。如果消息的 event 字段没有被指定，onmessage 处理器就会被调用，即默认 event 为 message。
    - data：消息的数据字段。当 EventSource 接收到众多连续以 data: 开始的行时，会通过在每一行之间插入一个换行符来把它们连接起来。尾部的换行符会被移除掉。
    - id：设置 EventSource 对象最后事件 ID 值。浏览器用 lastEventId 属性读取这个值。一旦连接断线，浏览器会发送一个 HTTP 头，里面包含一个特殊的 Last-Event-ID 头信息，将这个值发送回来，用来帮助服务器端重建连接。因此，这个头信息可以被视为一种同步机制。
    - retry：指定若连接出错，浏览器重新发起连接的时间间隔。必须为整数，单位为毫秒。如果该值为非整数，该字段将被忽略。
    
    除了上面的其他字段名将被忽略。

  - 通常，注释行可以用来避免连接超时，服务器可以周期性地发送注释行来维持连接；

  - 若某一行中没有包含冒号（:），那么整行将会被当成字段名，它的字段值将会是空白字符串；

  例：传输 json 信息
  ```
  event: usermessage
  data: {"username": "bobby", "time": "02:34:11", "text": "Hi everyone."}
  ```

- 客户端一旦收到 response，会触发回调函数，同时会再次向服务端发起与第一次相同的 HTTP Request……直到客户端调用 close() 关闭连接，则客户端不会再持续发起请求。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/2e7a6726de4b2d746a79f4cea47d9cf8.jpg)

- 因此，SSE 传输的过程实际上包括了很多个 HTTP 请求，但效果上看，相当于客户端与服务器一直保持连接，服务端可以随时根据需要向客户端发送 data.

### 浏览器支持

除了 IE 和 Edge，各大浏览器基本都支持 SSE:
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/5a5cd52fd3bf94e90a6012465303c3d7.jpg)

JavaScript 检测是否支持 SSE：
```javascript
if ('EventSource' in window) {
  // ...
}
```

### 与 websocket 的比较

对于 Web 的实时应用来说，SSE 与 WebSocket 是最为可行的技术方案。

WebSockets 连接可以同时发送数据给浏览器，也可以接收来自浏览器的数据。一个可能使用 WebSockets 的例子就是聊天程序了。

SSE 连接仅仅可以推送数据给浏览器。在线股票行情，或者 twitter 更新都是比较好的例子，它们都可以使用 SSE
实践中，由于 SSE 可以做的 WebSockets 都可以做，所以 WebSockets 正获得更多的关注和青睐，浏览器支持 WebSockets 更多于 SSE。

然而，这对于一些应用来说有点矫枉过正了，并且后端可以更容易实现 SSE。

而且 SSE 在较老的那些不支持该特性的浏览器中更容易实现，仅仅是使用 JavaScript。

SSE 之所以会被 WebSockets 掩盖了光芒的原因之一是 WebSockets API 提供了丰富的协议去处理双向的，全双工的通信。因为 WebSockets 拥有两个信道，所以它对于游戏、聊天 app 和那些需要双方实时更新的应用更具有吸引力。然而，在某些场景中，客户端不需要发送数据。我们只是简单地获取服务器的更新数据。例如朋友状态的更新、股票代号、新闻提要更新或者其他一些自动数据推送机制。如果需要向服务器发送数据，使用 XMLHttpRequest 是个好办法。

SSE 工作在传统的 HTTP 协议上，也就是说它不需要一个特定的协议或者服务器实现来支持。而 WebSockets 需要全双工连接和新的 Web Socket 服务器来处理此协议。另外，SSE 还有很多 WebSockets 缺少的特点，比如自动重连，事件 ID 和发送任意事件的能力。

总结
- SSE 相对于 WebSockets 的优点：
  
  - 使用简单的 HTTP 传输，而不是独立的协议。
  
  - 默认开启断线重连（缺省设置是连接出错后每隔 3 秒发起一次重新连接，可通过 retry 字段修改浏览器重新发起连接的时间间隔），重连次数没有上限；
  
  - 支持定义事件 ID。
  
  - SSE 可以通过 javascript 在那些不支持它的浏览器上面实现。
  
  - 更简单的协议，更容易在服务端实现。

- WebSockets 相对于 SSE 的优点：

  - 实时，双向通信。
  
  - 更多的浏览器本地支持。

- 一些使用 SSE 的理想情况：
  
  - 股票行情。
  
  - twitter 信息更新。
  
  - 浏览器提醒。

### 使用

#### Font End

- 构造
  
  SSE 的客户端 API 部署在 EventSource 对象上，实例化一个 EventSource 对象，即向服务器发起 SSE 的 HTTP 请求：
  ```javascript
  var source = new EventSource(url);
  ```
  注意：url 可以与当前网址同域，也可以跨域。跨域时，可以指定第二个参数，打开 withCredentials 属性，表示是否一起发送 Cookie：
  ```javascript
  var source = new EventSource(url, { withCredentials: true });
  ```

- 属性
  
  readyState：表明连接的当前状态。该属性只读，取值：
  - 0：相当于常量 EventSource.CONNECTING，表示连接还未建立，或者断线正在重连。
  - 1：相当于常量 EventSource.OPEN，表示连接已经建立，可以接受数据。
  - 2：相当于常量 EventSource.CLOSED，表示连接已断，且不会重连

- 标准事件

  onopen：连接一旦建立，就会触发 open 事件；
  ```javascript
  source.onopen = function (event) {
    // ...
  };

  // 另一种写法
  source.addEventListener('open', function (event) {
    // ...
  }, false);
  onmessage：当接收到消息；
  source.onmessage = function (event) {
    var data = event.data;
    // handle message
  };

  // 另一种写法
  source.addEventListener('message', function (event) {
    var data = event.data;
    // handle message
  }, false);
  onerror	：当发生错误；
  source.onerror = function (event) {
    // handle error event
  };

  // 另一种写法
  source.addEventListener('error', function (event) {
    // handle error event
  }, false);
  ```

- 自定义事件

  默认情况下，服务器发来的数据，总是触发浏览器 EventSource 实例的 message 事件。开发者还可以自定义 SSE 事件，这种情况下，发送回来的数据只会触发相应的事件类型，而不会触发 message 事件；

  例：监听 foo 事件
  ```javascript
  source.addEventListener('foo', function (event) {
    var data = event.data;
    // handle message
  }, false);
  ```

- 方法

  close()：关闭 SSE 连接
  ```javascript
  source.close();
  ```

- 实例
```javascript
document.getElementById("info").innerHTML = "正在准备连接服务器……";

if (!"EventSource" in window) {
    alert("你的浏览器不支持 SSE");
    window.close();
} else {
    var source = new EventSource("http://127.0.0.1:8080/sse");
    source.onopen = function (event) {
        document.getElementById("info").innerHTML = "连接成功！";
    };

    source.onclose = function (event) {
        if (source.readyState === EventSource.CLOSED) {
            document.getElementById("info").innerHTML = "连接断开！";
        }

    };

    source.onmessage = function (event) {
        document.getElementById("sse").innerHTML += event.data + "<br/>";
    };
}
```

#### Server End

```java
@Controller
public class SSEController {
	@RequestMapping(value = "/sse", produces = "text/event-stream;charset=UTF-8")
	@ResponseBody
	public String push() {
		Random r = new Random();
		try {
			Thread.sleep(5000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		return "data:Testing 1,2,3 " + r.nextInt() +"\n"
				+ "data:dsadasd\n"
				+ ":eeeeeeee\n\n"
				+ "rreeeeeeee\n\n"
				+ "data:tttttttttttt\n\n";
	}
}

```

## Refer Links

https://www.ibm.com/developerworks/cn/web/wa-lo-comet/ 

http://blog.csdn.net/fenglibing/article/details/7108982

http://www.52im.net/thread-336-1-1.html 
