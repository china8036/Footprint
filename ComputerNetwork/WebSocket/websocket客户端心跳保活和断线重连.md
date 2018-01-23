- [websocket客户端心跳保活和断线重连](#websocket%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BF%83%E8%B7%B3%E4%BF%9D%E6%B4%BB%E5%92%8C%E6%96%AD%E7%BA%BF%E9%87%8D%E8%BF%9E)
  - [重连：断线重连 / 心跳检测重连](#%E9%87%8D%E8%BF%9E%EF%BC%9A%E6%96%AD%E7%BA%BF%E9%87%8D%E8%BF%9E-%E5%BF%83%E8%B7%B3%E6%A3%80%E6%B5%8B%E9%87%8D%E8%BF%9E)
    - [断线重连（连接出错并触发onclose）](#%E6%96%AD%E7%BA%BF%E9%87%8D%E8%BF%9E%EF%BC%88%E8%BF%9E%E6%8E%A5%E5%87%BA%E9%94%99%E5%B9%B6%E8%A7%A6%E5%8F%91onclose%EF%BC%89)
    - [心跳检测重连（连接出错但不触发onclose）](#%E5%BF%83%E8%B7%B3%E6%A3%80%E6%B5%8B%E9%87%8D%E8%BF%9E%EF%BC%88%E8%BF%9E%E6%8E%A5%E5%87%BA%E9%94%99%E4%BD%86%E4%B8%8D%E8%A7%A6%E5%8F%91onclose%EF%BC%89)
  - [保活：心跳保活](#%E4%BF%9D%E6%B4%BB%EF%BC%9A%E5%BF%83%E8%B7%B3%E4%BF%9D%E6%B4%BB)
    - [需求](#%E9%9C%80%E6%B1%82)
    - [疑问](#%E7%96%91%E9%97%AE)
    - [心跳保活机制设计](#%E5%BF%83%E8%B7%B3%E4%BF%9D%E6%B4%BB%E6%9C%BA%E5%88%B6%E8%AE%BE%E8%AE%A1)
    - [总结](#%E6%80%BB%E7%BB%93)
    - [疑问](#%E7%96%91%E9%97%AE)

# websocket客户端心跳保活和断线重连

## 重连：断线重连 / 心跳检测重连



### 断线重连（连接出错并触发onclose）

https://my.oschina.net/codingBingo/blog/633985 

断线指的是websocket连接中断，网络失去连接的情况，比如信号不好，网络临时性关闭，而不是物理断网（如直接拔网线）；

一般断线时都会触发websocket的onclose方法，因此，只需在此方法中重新发起一个websocket连接即可；

注意：
- **不要在onerror事件中进行重连，若此时进行重连，原来的连接还来不及close，随着时间积累会有越来越多未close的连接，一旦网络通信恢复了正常，该页面会同时存在很多个websocket连接，占用大量服务器资源**；
- **在onclose事件中才进行重连，因为原连接已经真正close**；

例：
```javascript
window.onload = function () {
    document.getElementById("info").innerHTML = "正在准备连接服务器……";
    var connect = function (url) {
        var websocket = null;

        // websocket = new SockJS("http://127.0.0.1:8080/recordHandle/sockjs");

        if ('WebSocket' in window) {
            websocket = new WebSocket(url);
        } else if ('MozWebSocket' in window) {
            websocket = new MozWebSocket(url);
        } else {
            // websocket = new SockJS("http://127.0.0.1:8080/recordHandle/sockjs");
        }

        websocket.onopen = function (event) {
            document.getElementById("info").innerHTML = "连接成功";
            console.log("Connected to WebSocket server.");

        };

        websocket.onclose = function (event) {
            document.getElementById("info").innerHTML = "连接关闭，10秒后重新连接……";
            console.log("Disconnected from WebSocket server. It will reconnect after 10 seconds...");
            // 10秒后重新连接，实际效果：每10秒重连一次，直到连接成功
            setTimeout(function () {
                connect(url);
            }, 10000);
        };

        websocket.onmessage = function(event) {
            obj = JSON.parse(event.data);
            document.getElementById("id_v").innerHTML = obj.id;
        };

        websocket.onerror = function (event) {
            document.getElementById("info").innerHTML = "连接出错";
            console.log('Error occured: ' + event.data);

        };

        //监听窗口关闭事件，窗口关闭前，主动关闭websocket连接
        window.onbeforeunload = function () {
            websocket.close();
        }
    };


    var url = "ws://127.0.0.1:8080/recordHandle";
    connect(url);


};
```

### 心跳检测重连（连接出错但不触发onclose）

https://my.oschina.net/codingBingo/blog/634947 

websocket ,ping pong heardbeat心跳机制 http://blog.techbeta.me/2015/12/websocket/ 

websocket下js发送和接受byte array信息http://wander-bird.iteye.com/blog/2187103 

websocket ping/pong frame from browser讨论  https://stackoverflow.com/questions/10585355/sending-websocket-ping-pong-frame-from-browser 

http://blog.csdn.net/ttdevs/article/details/62887058 

在使用websocket过程中，可能会出现物理性的断网（如拔网线），这时候websocket的连接已经断开，却**不会触发浏览器执行onclose方法**，我们无法知道是否断开连接，也就无法进行重连操作。因此，需要用到心跳检测重连。

websocket规范定义了心跳机制，一方可以通过发送ping（**协议头opcode 0x9**）消息给另一方，另一方收到ping后应该按照协议要求，尽可能快的返回pong（**协议头opcode 0xA**）；

各个浏览器并没有为websocket的发送/接收ping/pong消息提供JavaScript API，但各个浏览器都有各自的缺省检测方案和处理方法，如chrome一旦收到ping包，会马上自动答复一个相同的包作为pong，再如某些浏览器在一段时间没有发送数据传输时，会自动发送一个ping包到服务器检测是否还在线/保持连接；


实现发送ping/pong：

- Java服务端向客户端发送一条pingMessage：
  ```java
  byte[] bs = new byte[1];
  bs[0] = 'i';
  ByteBuffer byteBuffer = ByteBuffer.wrap(bs);
  PingMessage pingMessage = new PingMessage(byteBuffer);
  webSocketSession.sendMessage(pingMessage);
  System.out.println("已发送一个ping包：【" + pingMessage.toString() + "】");
  ```
  则chrome会自动答复一个【java.nio.HeapByteBuffer[pos=0 lim=1 cap=1]】，且关于收到这条pingMessage的信息不会显示在控制台中；其它浏览器未测试；

- 手动从客户端向服务器发送一个ping消息：
  ```javascript
  var pingToServer = function () {
      String.prototype.getBytes = function() {
          var bytes = [];
          for (var i = 0; i < this.length; i++) {
              var charCode = this.charCodeAt(i);
              var cLen = Math.ceil(Math.log(charCode)/Math.log(256));
              for (var j = 0; j < cLen; j++) {
                  bytes.push((charCode << (j*8)) & 0xFF);
              }
          }
          return bytes;
      };

      var payload = 'i';
      var buffer = new ArrayBuffer(payload.length);
      var intView = new Int8Array(buffer);
      for(var i = 0; i < intView.length; i++){
          intView[i] = payload.getBytes()[i];
      }
      websocket.send(intView);
      console.log("客户端已发送一个ping消息：【" + intView.toString() + "】");
  };
  ```

- 方案设计：

  **心跳包检测的方案基本可分为两种类型：客户端主动发包和服务端主动发包；**

  **客户端主动发包：客户端可以检测到服务器的状态，服务器一旦出现连接问题（指无法触发onclose的情况），客户端随即可以发现；但若客户端出现问题，服务器无法获知；**

  **服务端主动发包：服务器可以检测到客户端的状态，客户端一旦出现连接问题，服务器随即可以发现；但若服务端出现问题（指不触发onclose的情况），客户端无法获知；**

  **因此，完善的心跳检测方案，因由服务端主动发包和客户端主动发包两者搭配构成，即双边的心跳检测。**

- 方案一：（客户端主动）

  https://www.cnblogs.com/1wen/p/5808276.html 

  https://my.oschina.net/xkay/blog/846573 

  开始websocket传输时，启动 heartCheck机制，每次收到服务器的推送（说明此时连接正常），将计时器归零。当计时器到达timeout时，主动向服务器发起一个heartbeat，若一定时间内得不到答复，则说明连接已经断开，采取相应措施：重连或者关闭连接。

  适用于连接过程中服务器一直有向客户端进行数据传输的情形。
  ```javascript
  var heartCheck = {
      timeout: 60000,//60ms
      timeoutObj: null,
      serverTimeoutObj: null,
      reset: function(){
          clearTimeout(this.timeoutObj);
          clearTimeout(this.serverTimeoutObj);
  　　　　 this.start();
      },
      start: function(){
          var self = this;
          this.timeoutObj = setTimeout(function(){
              ws.send("HeartBeat");
              self.serverTimeoutObj = setTimeout(function(){
                  ws.close();//如果onclose会执行reconnect，我们执行ws.close()就行了.如果直接执行reconnect 会触发onclose导致重连两次
              }, self.timeout)
          }, this.timeout)
      },
  }

  ws.onopen = function () {
    heartCheck.start();
  };
  ws.onmessage = function (event) {
  //如果获取到消息，心跳检测重置
  //拿到任何消息都说明当前连接是正常的
      heartCheck.reset();
  }
  ws.onclose = function () {
      reconnect();
  };
  ws.onerror = function () {
      reconnect();
  };
  ```


- 方案二：（客户端主动）

  https://my.oschina.net/codingBingo/blog/634947 

  由客户端在指定时间间隔发送心跳包数据给服务器，服务器在一定时间内发回心跳包响应，对比超时限定，如果超过设定的超时时间，则认为当前与服务器的websocket连接已经断开，采取相应措施：重连/关闭连接。

  适用于客户端与服务器之间没有确定的数据传输（即无法预测什么时候会开始传），但需要一直保持连接/检测连接状态的情况。
  ```javascript
  heartbeatLive = (function(_this) {
    return function(conn, nowTime) {
      var nowtime, hbt;
      nowtime = new Date();
      if ((nowTime.add({
        minutes: 1
      })).isBefore(androidNowtime)) {
        clearTimeout(hbt);
        return newConnection();
      }
      return hbt = setTimeout(function() {
        return conn.send('heartbeat');
      }, 60000);
    };
  })(this);

  if (jsonGotData.hasOwnProperty('id')) {
    timestampVal = new Date(jsonGotData.now_time);
    heartbeatLive(webSocket, timestampVal);
  }
  if (jsonGotData.hasOwnProperty('heartbeat')) {
    timestampVal = new Date(jsonGotData.heartbeat);
    return heartbeatLive(webSocket, timestampVal);
  }
  ```

- 方案三：（服务端主动）
  <!-- TODO: 待实现？？？？？？？？？？？ -->

  由服务器向客户端发送心跳包，客户端收到后在一定时间内响应，若响应超时，则认为该客户端已经掉线，关闭连接，收回资源。

  发送心跳包要等网络空闭时才能发， 因为发送消息到后台也是需要消费网络带宽的，是需要时间的，如果发太多，或者正准备发送你的业务数据到后台之前就先发送了一堆心跳包到后台，则你的业务包要等待一段长时间，不是好的体验。

  那么怎以样才算网络空闭呢？

  这里要用到消息队列一个数组， 你每次需要向后台发送数据的时候都把数据封装成一个发送任务，放在你的消息队列里，你有一个线程，设每秒运行一次，每次运行时扫描队列是否有任务，如果有则发送到后台，没有则表示网络空闲，你的心跳计时器还发启动，计时到20秒时，插一个心跳任务到你的队列

  怕的是服务器空闲不用发包的时候，出现了断连却无法获知；若不空闲，发包失败马上能触发hadleerror函数，不用特别检测；

## 保活：心跳保活

### 需求

websocket长连接有默认的超时时间（1分钟），也就是说，超过一定的时间客户端和服务器之间没有发生任何消息传输，连接会自动断开；除此之外，服务器或防火墙一般也会在一段时间不活动并超时之后终止外部的长连接。因此，若需要使客户端一直保持连接，就需要设置心跳保活机制了。

### 疑问

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/5bca2f0f6938be1eae46a06ed5b2893a.jpg)

（图片来自https://www.cnblogs.com/wilber2013/p/4792788.html）

- 既然有TCP的keep-alive保活机制，为什么还需要我们再去实现？为什么websocket还会超过一分钟就断开？

  TCP的keep-alive保活机制是为了确认端到端的连接是否都仍然可达，用以通知浏览器和服务器这个连接链路出了问题了，从而触发onclose/onerror和服务器的相应函数；

  而在相对高层的websocket中还定义了超时时间，就不是为了检测该连接是否还可达，而是为了不要让无用连接占用带宽--某些连接虽然两端的连接没问题，但事实上在应用层暂时/已经没有作用了，完全可以先关闭掉。

  也因为超时断开是定义在websocket中的，而websocket处于TCP的上层，所以websocket完全可以断开TCP的连接。

### 心跳保活机制设计

  回到应用层websocket心跳保活机制的设计：

  首先想到的可能是在此连接被关闭后，在onclose中进行重连，但这就是上一节的内容了，本节的目的是保活，也就是要不让连接关闭。

  因此，最直接的保活机制可以这样：

  让浏览器每隔一定时间（要小于超时时间）发送一个心跳信息，也就是说让此websocket连接通路上有消息传输发生即可：
  ```javascript
  window.setInterval(function(){ //每隔5秒钟发送一次心跳，避免websocket连接因超时而自动断开
    if (ws.readyState == WebSocket.OPEN) {
      ws.send(‘ ’);
    }
  },5000);
  ```
  这样的做法优点是逻辑简单，但缺点是可能会导致心跳消息频繁发送甚至淹没了真正要传输的消息（如堵塞、丢包？），同时也浪费了大量的网络流量，对移动设备太不友好。

  因此，可以设计另一种对设客户端状态进行检测，决定发心跳包时机的保活机制：（加入了心跳重连）
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/23/cf96142bce7a8b3e62cb4f949933dfe8.jpg)

### 总结

需要辨析几个概念：

保活是针对websocket无传输超时自动断开的情况才有的需求，这个需求的解决可以交给客户端，也可以交给服务端。但如果交给服务端，并发量大时服务器需有巨大的保活任务量，因此交给客户端实现。

因此，**客户端发心跳包保活 + 断线自动重连**，是最终的方案。

### 疑问

服务器可以做的：

- 服务器踢除“死连接”机制

- 服务器另开一个线程运行以下逻辑：  
  - 设置计时器，计时器超时则检测session池是否为空：
      - 若为空，将计时器归零后重新启动   
      - 若不为空，则进行下一步   
  - 检测任务队列是否空闲： 
      - 若不空闲，将session池中没任务的连接提取到另一个session集合newSessions中，若newSessions为空，则将计时器归零后重新启动
      - 若空闲，将所有session加入到newSessions中   
  - 发送心跳包到newSessions的每个客户端，检测回复：
      - 有回复的session连接，将其全部放回session池
      - 没有回复的session连接，将其连接关闭   
  - 计时器归零后重新启动

- 是不是不应该与客户端的保活机制同时存在（需求相反）?

- 服务器需要开发者自己实现这种机制吗？在服务器底层有没有实现？

- 是否真的存在所谓的“死连接“?
