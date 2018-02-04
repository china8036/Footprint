- [JavaScript Web Workers](#javascript-web-workers)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [专用线程 Dedicated Worker](#%E4%B8%93%E7%94%A8%E7%BA%BF%E7%A8%8B-dedicated-worker)
    - [兼容性检测](#%E5%85%BC%E5%AE%B9%E6%80%A7%E6%A3%80%E6%B5%8B)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [NOTE](#note)
    - [应用](#%E5%BA%94%E7%94%A8)
    - [实例：Worker 进程完成论询](#%E5%AE%9E%E4%BE%8B%EF%BC%9Aworker-%E8%BF%9B%E7%A8%8B%E5%AE%8C%E6%88%90%E8%AE%BA%E8%AF%A2)
  - [共享线程 Shared Worker](#%E5%85%B1%E4%BA%AB%E7%BA%BF%E7%A8%8B-shared-worker)
  - [服务线程 Service Worker](#%E6%9C%8D%E5%8A%A1%E7%BA%BF%E7%A8%8B-service-worker)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [NOTE](#note)
    - [应用](#%E5%BA%94%E7%94%A8)
  - [Refer Links](#refer-links)

# JavaScript Web Workers

## 概述

Javascript 运行在一个单线程环境中，对 setTimeout/setInterval、ajax 和 dom 事件的异步处理是依赖事件循环实现的。

Web Worker 是 HTML5 标准的一部分，这一规范定义了一套 API，它允许一段 JavaScript 程序运行在主线程之外的另外一个线程中。Web Worker 的目的，就是为 JavaScript 创造多线程环境，允许主线程将一些任务分配给子线程。在主线程运行的同时，子线程在后台运行，两者互不干扰。等到子线程完成计算任务，再把结果返回给主线程。因此，每一个子线程就好像一个“工人”（worker），默默地完成自己的工作。这样做的好处是，一些高计算量或高延迟的工作，被 worker 线程负担了，所以主进程（通常是 UI 进程）就会很流畅，不会被阻塞或拖慢。

一个 worker 是使用一个构造函数创建的一个对象 (e.g. Worker()) 运行一个命名的 JavaScript 文件 - 这个文件包含将在工作线程中运行的代码。

Web Worker 有以下几个特点：
- 同域限制。子线程加载的脚本文件，必须与主线程的脚本文件在同一个域。
- DOM 限制。子线程所在的全局对象，与主进程不一样，它无法读取网页的 DOM 对象，即 document、window、parent 这些对象，子线程都无法得到。（但是，navigator 对象和 location 对象可以获得。）
- 脚本限制。子线程无法读取网页的全局变量和函数，也不能执行 alert 和 confirm 方法，不过可以执行 setInterval 和 setTimeout，以及使用 XMLHttpRequest 对象发出 AJAX 请求。
- 文件限制。子线程无法读取本地文件，即子线程无法打开本机的文件系统（file://），它所加载的脚本，必须来自网络。

Worker 线程包括以下几种：
- 专用线程 Dedicated Worker
- 共享线程 Shared Worker
- 服务线程 Service Worker

## 专用线程 Dedicated Worker

一个专用 worker 仅仅能被生成它的脚本所使用。

### 兼容性检测

查浏览器是否支持这个 API：
```javascript
if (window.Worker) {
  // 支持
} else {
  // 不支持
}
```

### 基本使用

WEB 主线程中：
- 通过 `var worker = new Worker(url)` 加载一个 js 文件来创建一个 worker，同时返回一个 worker 实例。

  ```javascript
  var worker = new Worker('work.js');
  ```
  Worker 构造函数的参数是一个脚本文件，这个文件就是子线程所要完成的任务，上面代码中是 work.js。由于子线程不能读取本地文件系统，所以这个脚本文件必须来自网络端。如果下载没有成功，比如出现 404 错误，这个子线程就会默默地失败。

  事实上，只要符合父线程的同源政策，Worker 线程自己也能新建 Worker 线程。Worker 线程可以使用 XMLHttpRequest 进行网络 I/O，但是 XMLHttpRequest 对象的 responseXML 和 channel 属性总是返回 null。

- 通过 `worker.postMessage(data)` 方法来向 worker 发送数据。

  子线程新建之后，并没有启动，必需等待主线程调用 postMessage 方法，即发出信号之后才会启动。postMessage 方法的 data 参数，就是主线程传给子线程的信号。它可以是一个字符串，也可以是一个对象。

- 绑定 `message` 事件来接收 worker 发送过来的数据。

  ```javascript
  worker.addEventListener('message', function(e) {
    console.log(e.data);
  }, false);
  ```

- 绑定 `error` 事件监听子线程是否发生错误。如果发生错误，会触发主线程的 error 事件。

  ```javascript
  worker.addEventListener('error', function(event) {
    console.log(event);
  });
  ```

- 通过 `worker.terminate()` 来终止一个 worker 的执行。使用完毕之后，为了节省系统资源，我们必须在主线程手动关闭子线程。
  ```javascript
  worker.terminate();
  ```

worker 新线程中：
- 绑定 `message` 事件来接收主线程发送过来的数据。

  ```javascript
  self.addEventListener('message', function(e) {
    self.postMessage('You said: ' + e.data);
  }, false);
  ```

- 通过 `postMessage(data)` 方法来向主线程发送数据。

- 通过 `self.close()` 在子线程内部终止一个 worker 的执行。

### NOTE

- worker 线程的执行流程：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/3/3f332e1c988cf99c4937743936721332.jpg)

- 在主线程中使用时，onmessage 和 postMessage() 必须挂在 worker 对象上，而在 worker 中使用时不用这样做。原因是，在 worker 内部，worker 是有效的全局作用域。

- workers 运行在另一个全局上下文中，不同于当前的 window. 因此，使用 window 快捷方式获取当前全局的范围 （而不是 self）在一个 Worker 内将返回错误。

- Web Worker 中可以访问以下对象：
  - 最小化 的 navigator 对象，包括 onLine、appName、appVersion、userAgent 和 platform 属性
  - 只读的 location 对象
  - setTimeout()、setInterval()、clearTimeout()、clearInterval() 方法
  - XMLHttpRequest 构造函数
  Web Worker 中不可访问以下对象：
  - DOM 对象
  - window 对象
  - document 对象
  - parent 对象

- 主线程与子线程之间的通信内容，可以是文本，也可以是对象。需要注意的是，这种通信是拷贝关系，即是传值而不是传址，子线程对通信内容的修改，不会影响到主线程。事实上，浏览器内部的运行机制是，先将通信内容串行化，然后把串行化后的字符串发给子线程，后者再将它还原。

  但是，用拷贝方式发送二进制数据，会造成性能问题。为了解决这个问题，JavaScript 允许主线程把二进制数据直接转移给子线程，但是一旦转移，主线程就无法再使用这些二进制数据了，这是为了防止出现多个线程同时修改数据的麻烦局面。这种转移数据的方法，叫做 Transferable Objects。如果要使用该方法，postMessage 方法的最后一个参数必须是一个数组，用来指定前面发送的哪些值可以被转移给子线程。
  ```javascript
  worker.postMessage(arrayBuffer, [arrayBuffer]);
  window.postMessage(arrayBuffer, targetOrigin, [arrayBuffer]);
  ```
### 应用

既然 Web Worker 为浏览器端 Javascript 带来了后台计算能力，我们便可利用这一能力，将无限循环中第一项“更新数据和对象状态”的耗时部分交由 Web Worker 执行，提升页面性能。

部分典型的应用场景如下

- 使用专用线程进行数学运算

  Web Worker 最简单的应用就是用来做后台计算，而这种计算并不会中断前台用户的操作

- 图像处理

  通过使用从<canvas>或者<video>元素中获取的数据，可以把图像分割成几个不同的区域并且把它们推送给并行的不同 Workers 来做计算

- 大量数据的检索

  当需要在调用 ajax 后处理大量的数据，如果处理这些数据所需的时间长短非常重要，可以在 Web Worker 中来做这些，避免冻结 UI 线程。

- 背景数据分析

  由于在使用 Web Worker 的时候，我们有更多潜在的 CPU 可用时间，我们现在可以考虑一下 JavaScript 中的新应用场景。例如，我们可以想像在不影响 UI 体验的情况下实时处理用户输入。利用这样一种可能，我们可以想像一个像 Word（Office Web Apps 套装）一样的应用：当用户打字时后台在词典中进行查找，帮助用户自动纠错等等。

### 实例：Worker 进程完成论询

有时，浏览器需要论询服务器状态，以便第一时间得知状态改变。这个工作可以在 Worker 线程里完成：
```javascript
var pollingWorker = createWorker(function (e) {
  var cache;

  function compare(new, old) { ... };

  var myRequest = new Request('/my-api-endpoint');

  setInterval(function () {
    fetch('/my-api-endpoint').then(function (res) {
      var data = res.json();

      if(!compare(res.json(), cache)) {
        cache = data;

        self.postMessage(data);
      }
    })
  }, 1000)
});

pollingWorker.onmessage = function () {
  // render data
}

pollingWorker.postMessage('init');
```

## 共享线程 Shared Worker

一个共享 worker 可以被多个脚本使用——即使这些脚本正在被不同的 window、iframe 或者 worker 访问。适用场合较专有 worker 而言比较少。

创建一个共享 worker：
```javascript
var myWorker = new SharedWorker('worker.js');
myWorker.port.start();  // 父级线程中的调用

port.start(); // worker 线程中的调用，假设 port 变量代表一个端口
```

## 服务线程 Service Worker

### 概述

在 2014 年，W3C 公布了 service worker 的草案，service worker 提供了很多新的能力，使得 web app 拥有与 native app 相同的离线体验、消息推送体验。

service worker 是一段脚本，与 web worker 一样，也是在后台运行。作为一个独立的线程，运行环境与普通脚本不同，所以不能直接参与 web 交互行为。native app 可以做到离线使用、消息推送、后台自动更新，service worker 的出现是正是为了使得 web app 也可以具有类似的能力。

Service Worker 有以下特点：
- 属于 JavaScript Worker，不能直接接触 DOM，通过 postMessage 接口与页面通信。
- 不需要任何页面，就能执行。
- 不用的时候会终止执行，需要的时候又重新执行，即它是事件驱动的。
- 有一个精心定义的升级策略。
- 只在 HTTPs 协议下可用，这是因为它能拦截网络请求，所以必须保证请求是安全的。
- 可以拦截发出的网络请求，从而控制页面的网路通信。
- 内部大量使用 Promise。

### 基本使用

向浏览器注册 Service Worker
```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
  //.register 方法返回一个 Promise 对象
    .then(function(registration) {
    // 登记成功
    console.log('ServiceWorker 登记成功，范围为', registration.scope);
    }).catch(function(err) {
    // 登记失败
      console.log('ServiceWorker 登记失败：', err);
    });
}
```
一旦注册完成，这段脚本就会用户的浏览器之中长期存在，不会随着用户离开你的网站而消失。

注册成功后，浏览器执行下面步骤：
1. 下载资源（Download）
1. 安装（Install）
1. 激活（Activate）

安装和激活，主要通过事件来判断：
```javascript
self.addEventListener('install', function(event) {
  event.waitUntil(
    fetchStuffAndInitDatabases()
  );
});

self.addEventListener('activate', function(event) {
  // You're good to go!
});
```
激活成功之后，打开 chrome://inspect/#service-workers 可以查看到当前运行的 service worker。

Service worker 一旦激活，就开始控制页面，可以监听 fetch 和 message 事件：
```javascript
self.addEventListener('fetch', function(event) {
  console.log(event.request);
});
```
fetch 事件会在两种情况下触发：
- 用户访问 Service worker 范围内的网页。
- 这些网页发出的任何网络请求（页面本身、CSS、JS、图像、XHR 等等），即使这些请求是发向另一个域。但是，iframe 和<object>标签发出的请求不会被拦截。

页面和 serviceWorker 之间可以通过 posetMessage() 方法发送消息，发送的消息可以通过 message 事件接收到。

这是一个双向的过程，页面可以发消息给 service worker，service worker 也可以发送消息给页面，由于这个特性，可以将 service worker 作为中间纽带，使得一个域名或者子域名下的多个页面可以自由通信。

### NOTE

service worker 并不是一直在后台运行的。在页面关闭后，浏览器可以继续保持 service worker 运行，也可以关闭 service worker，这取决与浏览器自己的行为。所以不要定义一些全局变量。

当 service worker 监听 fetch 事件以后，对应的请求都会经过 service worker。通过 chrome 的 network 工具，可以看到此类请求会标注：from service worker。如果 service worker 中出现了问题，会导致所有请求失败，包括普通的 html 文件。所以 service worker 的代码质量、容错性一定要很好才能保证 web app 正常运行。

### 应用

Service workers 可以用来做这些事情：
- 后台数据同步
- 响应来自其它源的资源请求
- 集中接收计算成本高的数据更新，比如地理位置和陀螺仪信息，这样多个页面就可以利用同一组数据
- 在客户端进行 CoffeeScript，LESS，CJS/AMD 等模块编译和依赖管理（用于开发目的）
- 后台服务钩子
- 自定义模板用于特定 URL 模式
- 性能增强，比如预取用户可能需要的资源，比如相册中的后面数张图片
- 后台同步：启动一个 service worker 即使没有用户访问特定站点，也可以更新缓存
- 响应推送：启动一个 service worker 向用户发送一条信息通知新的内容可用
- 对时间或日期作出响应
- 进入地理栅栏

## Refer Links

[【转向 Javascript 系列】深入理解 Web Worker](http://www.alloyteam.com/2015/11/deep-in-web-worker/)

[MDN 使用 Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)

[MDN 服务工作者 (Service worker) 概念和用法](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

[深入 HTML5 Web Worker 应用实践：多线程编程](https://www.ibm.com/developerworks/cn/web/1112_sunch_webworker/)

[阮一峰 JavaScript 标准参考教程：Web Worker](http://javascript.ruanyifeng.com/htmlapi/webworker.html)

[Service Worker 初体验](http://www.alloyteam.com/2016/01/9274/)