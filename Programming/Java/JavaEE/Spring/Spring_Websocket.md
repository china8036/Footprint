- [Spring websocket 实现](#spring-websocket-实现)
  - [1. 介绍](#1-介绍)
  - [2. 三个部分](#2-三个部分)
    - [2.1. 处理器](#21-处理器)
    - [2.2. 拦截器](#22-拦截器)
    - [2.3. 配置](#23-配置)
  - [3. 重要类](#3-重要类)
    - [3.1. WebSocketSession](#31-websocketsession)
    - [3.2. WebSocketMessage](#32-websocketmessage)
  - [4. origins 配置](#4-origins-配置)
  - [5. SockJS](#5-sockjs)
    - [5.1. 服务端中开启 SockJS](#51-服务端中开启-sockjs)
    - [5.2. 客户端使用 sockJS](#52-客户端使用-sockjs)
  - [6. Stomp](#6-stomp)
    - [6.1. 介绍](#61-介绍)
    - [6.2. STOMP 帧](#62-stomp-帧)
    - [6.3. 客户端 API](#63-客户端-api)
  - [7. 实例：Spring MVC + websocket](#7-实例spring-mvc--websocket)
  - [8. 实例：SpringBoot + websocket + STOMP + SockJS](#8-实例springboot--websocket--stomp--sockjs)
    - [8.1. 服务器端](#81-服务器端)
    - [8.2. 客户端](#82-客户端)
    - [8.3. 广播消息推送](#83-广播消息推送)
    - [8.4. 一对一消息推送的几种方法（即服务器收到客户端消息后仅回复给该客户端而非广播）](#84-一对一消息推送的几种方法即服务器收到客户端消息后仅回复给该客户端而非广播)
    - [8.5. 异常信息推送](#85-异常信息推送)
  - [9. Refer Links](#9-refer-links)

# Spring websocket 实现

## 1. 介绍

Spring 从 4.0 开始加入了 spring-websocket 这个模块，并能够全面支持 WebSocket，它与 Java WebSocket API 标准（JSR-356）保持一致，同时提供了额外的服务。

在 maven 中引入 spring-websocket 模块：
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-websocket</artifactId>
    <version>4.3.10.RELEASE</version>
</dependency>
```

在 spring 中引入 websocket 模块的代码，在 spring 的配置文件中配置对此类的自动扫描
```xml
<context:component-scan base-package="xxx.xxx.websocket"/>
```

## 2. 三个部分

使用 websocket 模块，主要包含 3 个部分：

- 配置类 WebSocketConfig（注册 websocket 接口）
- 拦截器 WebSocketInterceptor（进行握手连接）
- 处理类 SocketHandler（处理通信业务）	

### 2.1. 处理器

```java
/**
 * WebSocket 处理器，用于处理通信业务
*/
public class MyWebSocketHandler implements WebSocketHandler {

@Override
// JSR365:@OnOpen
/** 
* webscoket 建立好链接之后的处理函数 
* @param session 当前 websocket 的会话 id，打开一个 websocket 通过都会生成唯一的一个会话，可以通过该 id 进行发送消息到浏览器客户端 
*/
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        System.out.println("ConnectionEstablished");
        //TODO
    }

@Override
// JSR365:@OnMessage
/** 
* 客户端发送服务器的消息时，的处理函数，在这里收到消息之后可以分发消息 
*/
    public void handleMessage(WebSocketSession session, WebSocketMessage<?> message) throws Exception {
        //TODO
    }

@Override
// JSR365:@OnError
/** 
* 消息传输过程中出现的异常处理函数
*/
    public void handleTransportError(WebSocketSession session, Throwable exception) throws Exception {
        //TODO
    }

@Override
// JSR365:@OnClosed
/** 
* websocket 链接关闭的回调 
* http://docs.oracle.com/javaee/7/api/javax/websocket/CloseReason.CloseCodes.html 
*/
    public void afterConnectionClosed(WebSocketSession session, CloseStatus closeStatus) throws Exception {
        System.out.println("ConnectionClosed");
        //TODO
    }

@Override
/** 
* 是否支持处理拆分消息，返回 true 返回拆分消息 
*/
    public boolean supportsPartialMessages() {
        return false;
    }

}

```

### 2.2. 拦截器

```java
/**
 * WebSocket 拦截器，用于建立连接（握手）和断开
*/
public class MyHandShake implements HandshakeInterceptor {
    /** 
     * 初次握手前，若返回 false，则不建立链接 
     * 可在此处将 HttpSession 中对象放入 WebSocketSession 中，便于之后 Handler 使用 HttpSession
     * 也可直接使用内建拦截器 HttpSessionHandshakeInterceptor，它可以传递 HTTP session attributes 到 WebSocket session 中
     */
    public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Map<String, Object> attributes) throws Exception {
        // ...
        return true;
    }
 
    /** 初次握手访问后 */
    public void afterHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Exception exception) {
    }
 
}

```
更高级的做法是继承 DefaultHandshakeHandler，它操作了 WebSocket 握手的步骤，包括验证 client origin、协商一个 sub-protocol 等。

### 2.3. 配置

```java
/**
* WebSocket 配置类，用于注册 websocket 接口
*/
@Configuration
@EnableWebMvc
@EnableWebSocket
public class WebSocketConfig extends WebMvcConfigurerAdapter implements WebSocketConfigurer {
 
    @Bean
    public MyWebSocketHandler myHandler() {
        return new MyWebSocketHandler();
    }

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(myWebSocketHandler, "/ws").addInterceptors(new MyHandShake ());// 注册 websocket 接口，并配置处理器和拦截器
        //registry.addHandler(socketHandler, "/sockjs/socketServer").addInterceptors(new WebSocketInterceptor()).withSockJS();
    }

    // 配置 Tomcat 服务器的 WebSocket 引擎
    @Bean
    public ServletServerContainerFactoryBean createWebSocketContainer() {
        ServletServerContainerFactoryBean container = new ServletServerContainerFactoryBean();
        container.setMaxTextMessageBufferSize(8192);
        container.setMaxBinaryMessageBufferSize(8192);
        return container;
    }

}

```
或在 XML 文件中配置：
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:websocket="http://www.springframework.org/schema/websocket"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/websocket
        http://www.springframework.org/schema/websocket/spring-websocket.xsd">

    <websocket:handlers>
        <websocket:mapping path="/myHandler" handler="myHandler"/>
        <websocket:handshake-interceptors>
            <bean class="xxx.xxx.xxx.MyHandShake"/>
        </websocket:handshake-interceptors>
</websocket:handlers>

<bean class="org.springframework...ServletServerContainerFactoryBean">
    <property name="maxTextMessageBufferSize" value="8192"/>
    <property name="maxBinaryMessageBufferSize" value="8192"/>
</bean>

    <bean id="myHandler" class="org.springframework.samples.MyHandler"/>

</beans>
```

## 3. 重要类

### 3.1. WebSocketSession

http://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/socket/WebSocketSession.html 

http://www.jianshu.com/p/8355df989b81 

WebSocketSession 对象表示与一个客户端的一个会话，每个客户端与服务器的连接都会生成一个 WebSocketSession 对象，可通过该对象获取客户端的信息以及向客户端发送信息；

- void sendMessage(Message<?> message) throws IOException：向客户端推送文本 / 二进制信息；
- String getId()：返回唯一的 session id；
- URI getUri()：返回开启 WebSocekt 连接的 URI；
- HttpHeaders getHandshakeHeaders()：返回握手请求的请求头；
- boolean isOpen()：返回连接是否打开；
- void close() throws IOException：关闭 WebSocket 连接，状态码 1000；

  注：客户端直接调用 close 方法并不会关闭连接，而是发送请求（操作码为 8 的帧）到服务器请求对方，服务器接收请求后断开连接，才会触发客户端的 close 事件；因此，在断开之前，客户端若发送一个同样的断连请求，并包含状态码和原因描述，也可以起到相同的效果。

- void close(CloseStatus status) throws IOException：关闭 WebSocket 连接，指定状态码 status；

### 3.2. WebSocketMessage

Interface WebSocketMessage<T>

公共方法：
- T getPayload()；
- int getPayloadLength()；
- boolean isLast()；

实现类：AbstractWebSocketMessage, BinaryMessage, PingMessage, PongMessage, TextMessage

[TextMessage](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/socket/TextMessage.html)
构造：`new TextMessage(“some string”) `

[BinaryMessage](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/socket/BinaryMessage.html)
构造：`new BinaryMessage(byte[] payload)`

## 4. origins 配置

https://docs.spring.io/spring/docs/current/spring-framework-reference/html/websocket.html#websocket-server-allowed-origins 

三种可能的设置：

1)	仅允许同源请求（默认）：在该模式下，当启用 SockJS 时，Iframe HTTP response header X-Frame-Options 被设置成 SAMEORIGIN，然后 JSONP 传输会被禁止 -- 因其不允许检查请求的 origin。因此，启用该模式时，不支持 IE6/7。
2)	运行指定的 origins 列表：每个提供的 allowed origin 必须以 http:// 或 https:// 开头。在这种模式下，当启用 SockJS 时，IFrame 和 JSONP 都被禁止！因此不支持 IE6/7/8/9。
3)	允许所有 origins：设置为*即可。

配置 ORIGIN:
```java
@Override
public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
    registry.addHandler(myHandler(), "/myHandler").setAllowedOrigins("http://mydomain.com");
}
```
或
```xml
<websocket:handlers allowed-origins="http://mydomain.com">
<websocket:mapping path="/myHandler" handler="myHandler" />
</websocket:handlers>
```

## 5. SockJS

SockJS 是一个浏览器上运行的 JavaScript 库，如果浏览器不支持 WebSocket，该库可以模拟对 WebSocket 的支持，实现浏览器和 Web 服务器之间低延迟、全双工、跨域的通讯通道；

由于 WebSocket 并不是所有浏览器都支持，因此 spring 推荐使用 sockjs 来适配各种浏览器；SockJS 的会让应用优先使用 WebSocket API，但会在必要的时候回滚到 non-WebSocket，自动选择 HTTP Streaming、HTTP Long Polling 中的最优方案替代；

https://github.com/sockjs/sockjs-client#supported-transports-by-browser-html-served-from-http-or-https 

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/807aea0b83e51dd2b7c218448a96cf49.jpg)

SockJS client 以发送 "GET /info" 从服务器获取基本的信息开始。之后，它必须决定使用什么样的传输。如果可以，会使用 WebSocket。如果不可用，在多数浏览器中至少有一个 HTTP Streaming 选项，如果还不行，那就将使用 HTTP (long) polling 了；

所有传输请求都有下面的 URL 结构：
```
http://host:port/myApp/myEndpoint/{server-id}/{session-id}/{transport} 
```
1)	{server-id} - useful for routing requests in a cluster but not used otherwise.
2)	{session-id} - correlates HTTP requests belonging to a SockJS session.
3)	{transport} - indicates the transport type, e.g. "websocket", "xhr-streaming", etc.

WebSocket transport 需要的是仅仅一个单一的 HTTP 请求来进行 WebSocket 握手。此后所有的消息都在那个 socket 上面交换。

HTTP transport 则需要更多请求。例如，Ajax/XHR streaming 依赖于一个长期运行的请求来完成服务器到客户端的消息，额外的 HTTP POST 请求来完成客户端到服务器的消息。Long polling 是类似的 -- 只是 它会在每次服务器到客户端的发送之后结束当前请求。

### 5.1. 服务端中开启 SockJS

由于 spring 内置了 SockJS 服务端，因此只需在注册 websocket handle 时添加`.withSockJS()`即可；

若出现错误：`Incompatibile SockJS!` 使用 setClientLibraryUrl，指定客户端与服务器的 sockjs 为同一版本即可；
```java
@Override
public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
   registry.addHandler(myWebSocketHandler, "/ws").addInterceptors(new MyHandShake ());
   registry.addHandler(socketHandler, "/sockjs/socketServer").addInterceptors(new WebSocketInterceptor())       .withSockJS()
    .setClientLibraryUrl("//cdn.bootcss.com/sockjs-client/1.1.4/sockjs.min.js");
}
```
或者 XML：
```xml
<websocket:handlers>
   <websocket:mapping path="/myHandler" handler="myHandler"/>
   <websocket:sockjs/>
</websocket:handlers>
```

### 5.2. 客户端使用 sockJS

[SockJS-client API](https://github.com/sockjs/sockjs-client)

加载 sockJS：
```html
<script src="https://cdn.jsdelivr.net/sockjs/1/sockjs.min.js"></script>
```
使用 sockJS：

创建连接：
```javascript
var sockjs = new SockJS(url, _reserved, options);
// URL 可包括查询字符串
// SockJS 所处理的 URL 是 "http://" 或 "https://" 模式，而不是 "ws://" or  "wss://" ；
// 其他的函数如 onopen, onmessage, and  onclose ，SockJS 客户端与 WebSocket 一样；
```
例：
```javascript
var websocket;
if ('WebSocket' in window) {
  websocket = new WebSocket("ws://" + path + "/ws?uid="+uid);
} else if ('MozWebSocket' in window) {
  websocket = new MozWebSocket("ws://" + path + "/ws"+uid);
} else {
  websocket = new SockJS("http://" + path + "/ws/sockjs"+uid);
}

websocket.onopen = function(event) {
  console.log("WebSocket: 已连接");
  console.log(event);
};

websocket.onmessage = function(event) {
  var data=JSON.parse(event.data);
  console.log("WebSocket: 收到一条消息",data);
  var textCss=data.from==-1?"sfmsg_text":"fmsg_text";
  $("#content").append("<div><label>"+data.fromName+"&nbsp;"+data.date+"</label><div class='"+textCss+"'>"+data.text+"</div></div>");
  scrollToBottom();
};

websocket.onerror = function(event) {
  console.log("WebSocket: 发生错误 ");
  console.log(event);
};
websocket.onclose = function(event) {
  console.log("WebSocket: 已关闭");
  console.log(event);
}
```

## 6. Stomp

使用方法教程：

http://blog.csdn.net/daniel7443/article/details/54377326 

https://my.oschina.net/feinik/blog/853875 

https://my.oschina.net/u/1590027/blog/879629

http://blog.csdn.net/yingxiake/article/details/51213060

http://blog.csdn.net/yingxiake/article/details/51224569

http://blog.csdn.net/yingxiake/article/details/51224929

（按下边的做初步成功了）

https://www.cnblogs.com/winkey4986/p/5622758.html

https://my.oschina.net/feinik/blog/853875

STOMP 1.2 协议说明

http://blog.csdn.net/zsomsom/article/details/27076249?utm_source=tuicool 

### 6.1. 介绍

STOMP(Simple Text-Orientated Messaging Protocol) 面向消息的简单文本协议。

WebSocket 是一个消息架构，不强制使用任何特定的消息协议，它依赖于应用层解释消息的含义；与处在应用层的 HTTP 不同，WebSocket 处在 TCP 上非常薄的一层，会将字节流转换为文本 / 二进制消息，**对于实际应用来说，WebSocket 的通信形式层级过低，因此，可以在 WebSocket 之上使用 STOMP 协议，来为浏览器 和 server 间的通信增加适当的消息语义**。

如何理解 STOMP 与 WebSocket 的关系：
- HTTP 协议解决了 web 浏览器发起请求以及 web 服务器响应请求的细节，假设 HTTP 协议 并不存在，只能使用 TCP 套接字来编写 web 应用，你可能认为这是一件疯狂的事情。
- 直接使用 WebSocket（SockJS） 就很类似于使用 TCP 套接字来编写 web 应用，因为没有高层协议，就需要我们定义应用间所发送消息的语义，还需要确保连接的两端都能遵循这些语义。
- 同 HTTP 在 TCP 套接字上添加请求 - 响应模型层一样，STOMP 在 WebSocket 之上提供了一个基于帧的线路格式层，用来定义消息语义。

### 6.2. STOMP 帧

STOMP 帧由命令，一个或多个头信息、一个空行及负载（文本或字节）所组成；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/d7bfa9cdbd329d7e8867a00765945b17.jpg)

其中可用的 COMMAND 包括：
```
CONNECT、SEND、SUBSCRIBE、UNSUBSCRIBE、BEGIN、COMMIT、ABORT、ACK、NACK、DISCONNECT；
```

例：

- 发送消息
  ```
  SEND
  destination:/queue/trade
  content-type:application/json
  content-length:44
  {"action":"BUY","ticker":"MMM","shares",44}^@
  ```
- 订阅消息
  ```
  SUBSCRIBE
  id:sub-1
  destination:/topic/price.stock.*
  ^@
  ```
- 服务器进行广播消息
  ```
  MESSAGE
  message-id:nxahklf6-1
  subscription:sub-1
  destination:/topic/price.stock.MMM
  {"ticker":"MMM","price":129.45}^@
  ```

### 6.3. 客户端 API

官方教程：http://jmesnil.net/stomp-websocket/doc/ 

引入 stomp.js
```html
<script type="application/javascript" src="http://cdn.bootcss.com/stomp.js/2.3.3/stomp.min.js"></script>
```

-  发起连接

  客户端可以通过使用 Stomp.js 和 sockjs-client 连接
  ```javascript
  // 建立连接对象（还未发起连接）
  var socket=new SockJS("/spring-websocket-portfolio/portfolio");

  // 获取 STOMP 子协议的客户端对象
  var stompClient = Stomp.over(socket);

  // 向服务器发起 websocket 连接并发送 CONNECT 帧
  stompClient.connect(
      {},
  function connectCallback (frame) {
    // 连接成功时（服务器响应 CONNECTED 帧）的回调方法
          document.getElementById("state-info").innerHTML = "连接成功";
          console.log('已连接【' + frame + '】');
          stompClient.subscribe('/topic/getResponse', function (response) {
              showResponse(response.body);
          });
      },
  function errorCallBack (error) {
    // 连接失败时（服务器响应 ERROR 帧）的回调方法
          document.getElementById("state-info").innerHTML = "连接失败";
          console.log('连接失败【' + error + '】');
      }
  );
  ```
  说明：
  - socket 连接对象也可通过 WebSocket（不通过 SockJS) 连接

    ```javscript
    var socket=new WebSocket("/spring-websocket-portfolio/portfolio");
    ```

  - stompClient.connect() 方法签名：
    ```javscript
    client.connect(headers, connectCallback, errorCallback);
    ```
    其中
    headers 表示客户端的认证信息，如：
    ```javascript
    var headers = {
      login: 'mylogin',
      passcode: 'mypasscode',
      // additional header
      'client-id': 'my-client-id'
    };
    ```
    若无需认证，直接使用空对象 “{}” 即可；

    - connectCallback 表示连接成功时（服务器响应 CONNECTED 帧）的回调方法；
    - errorCallback 表示连接失败时（服务器响应 ERROR 帧）的回调方法，非必须；

- 断开连接

  若要从客户端主动断开连接，可调用 disconnect() 方法
  ```javascript
  client.disconnect(function () {
    alert("See you next time!");
  };
  ```
  该方法为异步进行，因此包含了回调参数，操作完成时自动回调；

- 心跳机制

    若使用 STOMP 1.1 版本，默认开启了心跳检测机制，可通过 client 对象的 heartbeat field 进行配置（默认值都是 10000 ms）：
    ```javascript
    client.heartbeat.outgoing = 20000; 	// client will send heartbeats every 20000ms
    client.heartbeat.incoming = 0;     	// client does not want to receive heartbeats from the server TODO:
    // The heart-beating is using window.setInterval() to regularly send heart-beats and/or check server heart-beats
    ```

- 发送信息

  连接成功后，客户端可使用 send() 方法向服务器发送信息：
  ```javascript
  client.send(destination url[, headers[, body]]);
  ```
  其中
  - destination url 为服务器 controller 中 @MessageMapping 中匹配的 URL，字符串，必须参数；
  - headers 为发送信息的 header，JavaScript 对象，可选参数；
  - body 为发送信息的 body，字符串，可选参数；

  例：
  ```javascript
  client.send("/queue/test", {priority: 9}, "Hello, STOMP");
  client.send("/queue/test", {}, "Hello, STOMP");
  ```

- 订阅、接收信息

  STOMP 客户端要想接收来自服务器推送的消息，必须先订阅相应的 URL，即发送一个 SUBSCRIBE 帧，然后才能不断接收来自服务器的推送消息；

  订阅和接收消息通过 subscribe() 方法实现：
  ```javscript
  subscribe(destination url, callback[, headers])
  ```
  其中
  - destination url 为服务器 @SendTo 匹配的 URL，字符串；
  - callback 为每次收到服务器推送的消息时的回调方法，该方法包含参数 message；
  - headers 为附加的 headers，JavaScript 对象；<!-- TODO: 什么作用？ -->
  - 该方法返回一个包含了 id 属性的 JavaScript 对象，可作为 unsubscribe() 方法的参数；

  例：
  ```javascript
  var headers = {ack: 'client', 'selector': "location = 'Europe'"};
  var  callback = function(message) {
    if (message.body) {
      alert("got message with body " + message.body)
    } else {
      alert("got empty message");
    }
  });
  var subscription = client.subscribe("/queue/test", callback, headers);
  ```

- 取消订阅
  ```javascript
  var subscription = client.subscribe(...);

  subscription.unsubscribe();
  ```

- JSON 支持

  STOMP 帧的 body 必须是 string 类型，若希望接收 / 发送 json 对象，可通过 JSON.stringify() and JSON.parse() 实现；

  例：
  ```javascript
  var quote = {symbol: 'APPL', value: 195.46};
  client.send("/topic/stocks", {}, JSON.stringify(quote));

  client.subcribe("/topic/stocks", function(message) {
    var quote = JSON.parse(message.body);
    alert(quote.symbol + " is at " + quote.value);
  });
  ```

- 事务支持

  STOMP 客户端支持在发送消息时用事务进行处理：

  举例说明：
  ```javascript
  // start the transaction
  // 该方法返回一个包含了事务 id、commit()、abort() 的 JavaScript 对象
  var tx = client.begin();
  // send the message in a transaction
  // 最关键的在于要在 headers 对象中加入事务 id，若没有添加，则会直接发送消息，不会以事务进行处理
  client.send("/queue/test", {transaction: tx.id}, "message in a transaction");
  // commit the transaction to effectively send the message
  tx.commit();
  // tx.abort();
  ```

- Debug 信息

  STOMP 客户端默认将传输过程中的所有 debug 信息以 console.log() 形式输出到客户端浏览器中，也可通过以下方式输出到 DOM 中：
  ```javascript
  client.debug = function(str) {
    // str 参数即为 debug 信息
    // append the debug log to a #debug div somewhere in the page using JQuery:
    $("#debug").append(str + "\n");
  };
  ```

- 认证

<!-- TODO: -->

By default, STOMP messages will be automatically acknowledged by the server before the message is delivered to the client.

The client can chose instead to handle message acknowledgement by subscribing to a destination and specify a ack header set to client or client-individual.

In that case, the client must use the message.ack() method to inform the server that it has acknowledge the message.
```javascript
var subscription = client.subscribe("/queue/test",
  function(message) {
    // do something with the message
    ...
    // and acknowledge it
    message.ack();
  },
  {ack: 'client'}
);
```
The ack() method accepts a headers argument for additional headers to acknowledge the message. For example, it is possible to acknowledge a message as part of a transaction and ask for a receipt when the ACK STOMP frame has effectively be processed by the broker:
```javascript
var tx = client.begin();
message.ack({ transaction: tx.id, receipt: 'my-receipt' });
tx.commit();
```
The nack() method can also be used to inform STOMP 1.1 brokers that the client did not consume the message. It takes the same arguments than the ack() method.

## 7. 实例：Spring MVC + websocket

- websocket 配置

  类配置：
  ```java
  @Configuration
  @EnableWebMvc
  @EnableWebSocket
  public class WebSocketConfig extends WebMvcConfigurerAdapter implements WebSocketConfigurer {

    public WebSocketConfig() {
    }

    @Bean
    public RecordHandle recordHandle() {
      return new RecordHandle();
    }

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry webSocketHandlerRegistry) {
      webSocketHandlerRegistry.addHandler(recordHandle(), "/recordHandle")
          .addInterceptors(new RecordHandshake()).setAllowedOrigins("*");

      webSocketHandlerRegistry.addHandler(recordHandle(), "/recordHandle/sockjs")
          .addInterceptors(new RecordHandshake()).setAllowedOrigins("*").withSockJS()
          .setClientLibraryUrl("//cdn.bootcss.com/sockjs-client/1.1.4/sockjs.min.js");

      System.out.println("注册 spring websocket 成功！");
    }

    @Bean
    public ServletServerContainerFactoryBean createWebSocketContainer() {
      ServletServerContainerFactoryBean container = new ServletServerContainerFactoryBean();
      container.setMaxTextMessageBufferSize(8192);
      container.setMaxBinaryMessageBufferSize(8192);
      return container;
    }

  }
  ```
  XML 配置
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:websocket="http://www.springframework.org/schema/websocket"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/websocket http://www.springframework.org/schema/websocket/spring-websocket.xsd">

      <context:component-scan base-package="com.firejq.websocket"/>
      <websocket:handlers allowed-origins="*">
          <websocket:mapping path="/recordHandle" handler="recordHandler"/>
          <websocket:handshake-interceptors>
              <bean class="com.firejq.websocket.RecordHandshake"/>
          </websocket:handshake-interceptors>
          <!--<websocket:sockjs/>-->
      </websocket:handlers>

      <bean id="recordHandler" class="com.firejq.websocket.RecordHandle"></bean>

      <bean class="org.springframework.web.socket.server.standard.ServletServerContainerFactoryBean">
          <property name="maxTextMessageBufferSize" value="8192"/>
          <property name="maxBinaryMessageBufferSize" value="8192"/>
      </bean>
  </beans>
  ```

- Handler
  ```java
  public class RecordHandle implements WebSocketHandler {

    // 连接队列
    private static final List<WebSocketSession> sessions = new ArrayList<WebSocketSession>();

    @Autowired
    private RecordService recordService;

    @Override
    public void afterConnectionEstablished(WebSocketSession webSocketSession) throws Exception {
      // 加入连接队列
      sessions.add(webSocketSession);
      System.out.printf("webSocketSessionId:" + webSocketSession.getId() + "连接成功！服务器准备开始发送信息……");
      webSocketSession.sendMessage(new TextMessage("连接成功了！服务器准备开始发送信息……"));
      while ( true ) {
        // 监控具体逻辑代码
        Record record = recordService.queryLastest();
        if (webSocketSession.isOpen()) {

          webSocketSession.sendMessage(new TextMessage(new GsonBuilder()
              .create().toJson(record)));//TODO
          System.out.printf(" 已发送给 session id：" + webSocketSession.getId());
        } else {
          break;
        }

        // 设置轮询的间隔时间
        Thread.sleep(1000);

      }
    }

    @Override
    public void handleMessage(WebSocketSession webSocketSession, WebSocketMessage<?> webSocketMessage) throws Exception {
  //		Record record = new Gson().fromJson(webSocketMessage.getPayload().toString(), Record.class);

      TextMessage returnMessage = new TextMessage("用户" + webSocketSession.getId() + "说："
          + webSocketMessage.getPayload());
      System.out.println(returnMessage.toString());
      for (WebSocketSession session : sessions) {
        session.sendMessage(returnMessage);
      }
    }

    @Override
    public void handleTransportError(WebSocketSession webSocketSession, Throwable throwable) throws Exception {
      System.out.printf("连接出错！");
      System.out.println("WebsocketSessionId: " + webSocketSession.getId() + "已经关闭");

    }

    @Override
    public void afterConnectionClosed(WebSocketSession webSocketSession, CloseStatus closeStatus) throws Exception {

      System.out.printf("连接关闭！");
      System.out.println("WebsocketSessionId: " + webSocketSession.getId() + "已经关闭");
    }

    @Override
    public boolean supportsPartialMessages() {
      return false;
    }
  }
  ```

- HandShake
  ```java
  public class RecordHandshake implements HandshakeInterceptor {
    @Override
    public boolean beforeHandshake(ServerHttpRequest serverHttpRequest, ServerHttpResponse serverHttpResponse, WebSocketHandler webSocketHandler, Map<String, Object> map) throws Exception {
      System.out.printf("准备进行连接握手！");
      return true;
    }

    @Override
    public void afterHandshake(ServerHttpRequest serverHttpRequest, ServerHttpResponse serverHttpResponse, WebSocketHandler webSocketHandler, Exception e) {
      System.out.printf("握手结束！");
    }
  }
  ```

## 8. 实例：SpringBoot + websocket + STOMP + SockJS

使用 SpringBoot + websocket + STOMP + SockJS 实现的[多人网页聊天室](https://github.com/firejq/web-chatroom-stomp)

### 8.1. 服务器端

服务器端需要编写的有两个地方：配置类和控制器：

- 配置类

  ```java
  @Configuration
  @EnableWebSocketMessageBroker
  public class WebSocketConfig extends AbstractWebSocketMessageBrokerConfigurer {
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
      registry.enableSimpleBroker("/topic", "/user");// “/topic“ 用于给客户端订阅广播信息，”/user“用于给客户端订阅点对点消息
      
      //registry.setApplicationDestinationPrefixes("/app");// 客户端向服务器发送消息时 URL 为：【/app + controller 中 @MessageMapping 的地址】，此处暂时不使用该配置
      //registry.setUserDestinationPrefix("/user/");// 客户端向指定用户发送（一对一）信息时 URL 前缀是“/user/”，即使不配置默认也是“/user/“
    }
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
      registry.addEndpoint("/endpoint").withSockJS();
    }
  }
  ```

- controller

  - 注解

    - 方法注解
      - @MessageMapping(value = "/URL") 匹配客户端 send 消息时的 URL；
      - @sendTo 和 @sendToUser 分别用于给客户端订阅广播消息和点对点消息；
    - 参数注解
      - @Payload：使用客户端 STOMP 帧的 body 赋值；
      - @Header(“xxx”)：使用客户端 STOMP 帧的 headers 中的 xxx 赋值；

  - SimpMessagingTemplate
    
    SimpMessagingTemplate 是 Spring-WebSocket 内置的一个消息发送工具，可以将消息发送到指定的客户端。
    ```java
    this.simpMessagingTemplate.convertAndSend("/topic/notice", value)
    ```
    将给定的对象进行序列化，使用‘MessageConverter’进行包装转化成一条消息，发送到指定的目标”，通俗点讲就是我们使用这个方法进行消息的转发发送；
    ```java
    @MessageMapping("/change-notice")    
    public void greeting(String value){
        this.simpMessagingTemplate.convertAndSend("/topic/notice", value);    
    }
    ```
    等同于
    ```java
    @MessageMapping("/change-notice")
    @SendTo("/topic/notice")
    public String greeting(String value) {
        return value;
    }
    ```
    @SendTo 定义了消息的目的地，即接收 /app/change-notice 发来的 value，然后将 value 转发到 /topic/notice 客户端

    具体代码见下边的实例；

### 8.2. 客户端

```javascript
var socket = new SockJS('/endpoint');
stompClient = Stomp.over(socket);

stompClient.connect(
    {},
    function connectCallback (frame) {
        // 获取 websocket 连接的 sessionId
        var sessionId = /\/([^\/]+)\/websocket/.exec(socket._transport.url)[1];
        console.log("connected, session id: " + sessionId);

        // 订阅广播消息
        var subscription_broadcast = stompClient.subscribe('/topic/getResponse', function callBack (response) {
            if (response.body) {
                printToScreen("【广播】" + response.body);
            } else {
                printToScreen("收到一个空消息");
            }
        });

        // 订阅私人消息
        var subscription_personal = stompClient.subscribe('/user/' + sessionId + '/personal', function callBack (response) {
            if (response.body) {
                printToScreen("【私人消息】" + response.body);
            } else {
                printToScreen("收到一个空消息");
            }
        });
    },
    function errorCallBack (error) {
        console.log('连接断开【' + error + '】');
    }
);

```

### 8.3. 广播消息推送

Controller
```java
@MessageMapping(value = "/chat")
@SendTo("/topic/getResponse")
public String talk(@Payload String text, @Header("simpSessionId") String sessionId) throws Exception {
    System.out.println("收到来自 sessionId:【" + sessionId + "】的广播消息：【" + text + "】");
    return "【" + sessionId + "】说：【" + text + "】";
}
```
Client
```javascript
......
// 订阅广播消息
var subscription_broadcast = stompClient.subscribe('/topic/getResponse', function callBack (response) {
    if (response.body) {
        printToScreen("【广播】" + response.body);
    } else {
        printToScreen("收到一个空消息");
    }
});
......
// 发送广播消息
stompClient.send("/chat", headers, JSON.stringify(body));
```

### 8.4. 一对一消息推送的几种方法（即服务器收到客户端消息后仅回复给该客户端而非广播）

- 在 @MessageMapping 匹配的 URL 中携带参数（客户端 id），以区分不同的客户端

    Controller
    ```java
    @Autowired
    private SimpMessagingTemplate simpMessagingTemplate;

    @MessageMapping(value = "/speak/{userId}")
    public void speak(Message message, @Payload String text, @DestinationVariable String userId) throws Exception {
        System.out.println("收到私人消息：" + text);
        this.simpMessagingTemplate.convertAndSend(‘/user/’ + userId + ‘/personal’, text);
    }
    ```
    client
    ```javascript
    // 随机生成一个全局变量 userId
    userId = Math.random().toString(36).substr(2);
    ......
    // 订阅私人消息
    var subscription_personal = stompClient.subscribe('/user/' + userId + '/personal', function callBack (response) {
            if (response.body) {
                    printToScreen("【私人消息】" + response.body);
            } else {
                    printToScreen("收到一个空消息");
            }
    }
    ......
    // 发送消息
    stompClient.send("/speak/" + userId, headers, JSON.stringify(body));
    ```

- 与第一种方法基本相同，但用 websocket 连接产生的 sessionId 代替自己生成的 userId
    
    Controller
    ```java
    @Autowired
    private SimpMessagingTemplate simpMessagingTemplate;

    @MessageMapping(value = "/speak")
    public void speak(Message message, @Payload String text, @Header("simpSessionId") String sessionId) throws Exception {
        System.out.println("收到私人消息：" + text);
        this.simpMessagingTemplate.convertAndSend(‘/user/’ + sessionId + ‘/personal’, text);
    }
    ```
    client
    ```javascript
    // 获取 socket 连接的 sessionId，即从 socket._transport.url 中使用正则截取
    var sessionId = /\/([^\/]+)\/websocket/.exec(socket._transport.url)[1];
    console.log("connected, session id: " + sessionId);
    ......
    // 订阅私人消息
    var subscription_personal = stompClient.subscribe('/user/' + sessionId + '/personal', function callBack (response) {
            if (response.body) {
                    printToScreen("【私人消息】" + response.body);
            } else {
                    printToScreen("收到一个空消息");
            }
    }
    ......
    // 发送消息
    stompClient.send("/speak", headers, JSON.stringify(body));
    ```

- 使用 @sendToUser 代替 SimpMessagingTemplate，客户端与第二种方法相同

    Controller
    ```java
    @MessageMapping(value = "/speak")
    @SendToUser(value = "/personal")
    // @sendToUser(value = “/URL”) 匹配的 URL 等同于 '/user/' + sessionId + ‘/URL’；
    public String speak(Message message, @Payload String text, @Header("simpSessionId") String sessionId) throws Exception {
        System.out.println("收到私人消息：" + text);
        return text;
    }
    ```
        
    client
    ```javascript
    // 获取 socket 连接的 sessionId，即从 socket._transport.url 中使用正则截取
    var sessionId = /\/([^\/]+)\/websocket/.exec(socket._transport.url)[1];
    console.log("connected, session id: " + sessionId);
    ......
    // 订阅私人消息
    var subscription_personal = stompClient.subscribe('/user/' + sessionId + '/personal', function callBack (response) {
            if (response.body) {
                    printToScreen("【私人消息】" + response.body);
            } else {
                    printToScreen("收到一个空消息");
            }
    }
    ......
    // 发送消息
    stompClient.send("/speak", headers, JSON.stringify(body));
    ```

注意：以上几种方法，订阅的 URL 都应使用 WebSocketConfig 类中的 enableSimpleBroker 配置为前缀，上例中：
```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig extends AbstractWebSocketMessageBrokerConfigurer {

    /**
     * 广播消息代理
     * @param registry
     */
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic", "/user");
    }
}
```
因此，订阅的 URL 都应使用 “/user” 作为前缀，否则会出错；

### 8.5. 异常信息推送

Controller
```java
@MessageExceptionHandler
@SendToUser(value = "/errors")
public String handleException(Throwable exception) {
    return exception.getMessage();
}
```
Client
```javascript
// 获取 websocket 连接的 sessionId
var sessionId = /\/([^\/]+)\/websocket/.exec(socket._transport.url)[1];
// 订阅异常消息
var subscription_errors = stompClient.subscribe('/user/' + sessionId + '/errors', function callBack (response) {
    if (response.body) {
        printToScreen("【异常消息】" + response.body);
    } else {
        printToScreen("收到一个空消息");
    }
});
```

## 9. Refer Links

[Spring 4.x WebSocket 文档（English）](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/websocket.html )

[Spring 5.0 WebSocket 文档（English）](http://docs.spring.io/spring/docs/5.0.0.M5/spring-framework-reference/html/websocket.html)

[Spring WebSocket 文档（中文）](https://my.oschina.net/u/1590027/blog/875273?utm_source=debugrun&utm_medium=referral)

https://my.oschina.net/u/1590027/blog/879629

http://www.voidcn.com/blog/yingxiake/article/p-5783312.html 

[多人聊天实例](http://www.xdemo.org/spring-websocket-comet/)
