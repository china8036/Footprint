- [JavaScript 模块化](#javascript-%E6%A8%A1%E5%9D%97%E5%8C%96)
  - [CommonJS](#commonjs)
  - [AMD(Asynchronous Module Definition)](#amdasynchronous-module-definition)
    - [require.js](#requirejs)
      - [解决的问题：](#%E8%A7%A3%E5%86%B3%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A)
      - [requireJS 使用](#requirejs-%E4%BD%BF%E7%94%A8)
  - [UMD](#umd)
  - [CMD](#cmd)
  - [ES6 Module](#es6-module)
    - [静态加载](#%E9%9D%99%E6%80%81%E5%8A%A0%E8%BD%BD)
    - [优点](#%E4%BC%98%E7%82%B9)
    - [严格模式](#%E4%B8%A5%E6%A0%BC%E6%A8%A1%E5%BC%8F)
  - [Refer Links](#refer-links)

# JavaScript 模块化

## CommonJS

在 CommonJS 中，全局方法 module() 用于定义模块，module.export() 用于导出模块内容，require() 用于引入模块，

假定有一个数学模块 math.js，就可以像下面这样加载并调用模块提供的方法：
```javascript
var math = require('math');
math.add(2,3); // 5
```
node.js 的[模块系统](http://nodejs.org/docs/latest/api/modules.html)，就是参照 CommonJS 规范实现的。

这种方式存在一个问题：**CommonJS 规范采用同步加载模块**，在执行`require('math')`的时候，JavaScript 是被阻塞的，这对服务器端不是一个问题，因为所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间。但是，对于浏览器，这却是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于"假死"状态。因此，由于这个原因，**CommonJS 规范只适用于服务端，而不适用于浏览器端**。

## AMD(Asynchronous Module Definition)

**AMD 规范采用异步方式加载模块**，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

AMD 也采用 require() 语句加载模块，但是不同于 CommonJS，它要求两个参数：
```javascript
// 第一个参数 [module]，是一个数组，里面的成员就是要加载的模块；第二个参数 callback，则是加载成功之后的回调函数
require([module], callback);
```
例：
```javascript
require(['math'], function (math) {
　　math.add(2, 3);
});
```
显然，**AMD 更适合浏览器环境**。

### require.js

require.js 是 AMD 规范的一种实现。

#### 解决的问题：

一般情况下，多模块的 JavaScript 项目，我们需要依次加载所有的 JavaScript 文件：
```javascript
<script src="1.js"></script>
<script src="2.js"></script>
<script src="3.js"></script>
<script src="4.js"></script>
<script src="5.js"></script>
<script src="6.js"></script>
```
但这样的写法存在很多缺点：
- 首先，加载的时候，浏览器会停止网页渲染，加载文件越多，网页失去响应的时间就会越长；
- 其次，由于 js 文件之间存在依赖关系，因此必须严格保证加载顺序（比如上例的 1.js 要在 2.js 的前面），依赖性最大的模块一定要放到最后加载，当依赖关系很复杂的时候，代码的编写和维护都会变得困难。
require.js 解决了这两个问题：
- 实现 js 文件的异步加载，避免网页失去响应；
- 管理模块之间的依赖性，便于代码的编写和维护。

#### requireJS 使用

引入 require.js，并指定应用的主模块文件 main.js（也放在 js 目录下面)：
```html
<script src="js/require.js" data-main="js/main"></script>
<!-- data-main 属性的作用是，指定网页程序的主模块，即整个网页的入口代码，类似 C 语言的 main 函数。在上例中，就是 js 目录下面的 main.js，这个文件会第一个被 require.js 加载。由于 require.js 默认的文件后缀名是 js，所以可以把 main.js 简写成 main -->
```
- 加载符合 AMD 规范的模块

  main.js
  ```javascript
  require(['moduleA', 'moduleB', 'moduleC'], function (moduleA, moduleB, moduleC){
    // some code here
  });
  ```
  使用 require.config() 方法，我们可以对模块的加载行为进行自定义。require.config() 就写在主模块（main.js）的头部。参数就是一个对象，这个对象的 paths 属性指定各个模块的加载路径：
  ```javascript
  require.config({
    baseUrl: "js/lib",
    paths: {
        "jquery": "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min",
        "underscore": "underscore.min",
        "backbone": "backbone.min"
      }
  });
  ```
  require.js 要求，每个模块是一个单独的 js 文件，这样会导致发送许多 HTTP 请求，影响网页加载速度，因此可以使用 requirejs 提供的[优化工具](http://requirejs.org/docs/optimization.html)，当模块部署完毕以后，可以用这个工具将多个模块合并在一个文件中，减少 HTTP 请求数。

- 加载不符合 AMD 规范的模块

  这样的模块在用 require() 加载之前，要先用 require.config() 方法，定义它们的一些特征：
  ```javascript
  require.config({
    shim: {
        'underscore':{
          exports: '_'
        },
  　　  'backbone': {
          deps: ['underscore', 'jquery'],
          exports: 'Backbone'
  　　　}
  　}
  });
  ```
  require.config() 中的 shim 属性专门用来配置不兼容的模块，具体来说，每个模块要定义（1）exports 值（输出的变量名），表明这个模块外部调用时的名称；（2）deps 数组，表明该模块的依赖性。

- 定义模块

  根据 AMD 规范，模块必须采用特定的 define() 函数来定义：
  ```javascript
  // math.js
  define(function (){
  　　var add = function (x,y){
  　　　　return x+y;
  　　};
  　　return {
  　　　　add: add
  　　};
  });
  ```
  如果这个模块还依赖其他模块，那么 define() 函数的第一个参数，必须是一个数组，指明该模块的依赖性：
  ```javascript
  define(['myLib'], function(myLib){//...}
  ```
  当 require() 函数加载上面这个模块的时候，就会先加载 myLib.js 文件。

## UMD

UMD 是 AMD 和 CommonJS 的糅合，是跨平台的解决方案。UMD 先判断是否支持 Node.js 的模块，exports 是否存在，存在则使用 Node.js 模块模式。在判断是否支持 AMD(define 是否存在), 存在则使用 AMD 方式加载模块。

## CMD

CMD 规范是阿里的玉伯提出来的，实现 js 库为 sea.js。 它和 requirejs 非常类似，即一个 js 文件就是一个模块，但是 CMD 的加载方式更加优秀，是通过按需加载的方式，而不是必须在模块开始就加载所有的依赖。

定义模块：
```javascript
define(function(require,exports,module){
  var $ = require('jquery'); 
  var Spinning = require('./spinning'); 
  exports.doSomething = ... 
  module.exports = ...
});
```
将回调函数转为字符串，使用正则提取 require 语法获取依赖，根据配置文件获取实际路径，在 dom 中动态插入 script 脚本，加载完毕后触发 onLoad 事件通知加载完成，所有依赖都加载完毕后执行回调，并将回调中的 require 语句替换为依赖，如回调中存在 `var $ = require('jquery')`, 则回调执行时 $ 即为 jquery 实例。

这时我们就可以看出 AMD 和 CMD 的区别了，前者是对于依赖的模块提前执行，而后者是延迟执行。前者推崇依赖前置，而后者推崇依赖就近，即只在需要用到某个模块的时候再 require。
缺点：依赖 SPM 打包，模块的加载逻辑偏重。
    

## ES6 Module

ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

**在 ES6 中，我们可以使用 import 关键字引入模块，通过 export 关键字导出模块**，功能较之于前几个方案更为强大。


例：
```javascript
import store from '../store/index'
import {mapState, mapMutations, mapActions} from 'vuex'
import axios from '../assets/js/request'
import util from '../utils/js/util.js'

export default {
  created () {
    this.getClassify(); 

    this.RESET_VALUE();
    console.log('created' ,new Date().getTime());

  }
}
```

但是由于 ES6 目前无法在浏览器中执行，所以，我们只能通过 babel 将不被支持的 import 编译为当前受到广泛支持的 require。

### 静态加载

**ES6 模块是编译时加载的。ES6 中模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。**CommonJS 和 AMD 模块，都只能在运行时确定这些东西。例如，在CommonJS 中，模块就是对象；但在ES6中模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入：
```javascript
// ES6模块
import { stat, exists, readFile } from 'fs';
```
上面代码从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

### 优点

除了静态加载带来的各种好处，ES6 模块还有以下好处：
- 不再需要UMD模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。目前，通过各种工具库，其实已经做到了这一点。
- 将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者navigator对象的属性。
- 不再需要对象作为命名空间（比如Math对象），未来这些功能可以通过模块提供。


### 严格模式

ES6 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict"`;。

NOTE:
- ES6 模块之中，顶层的this指向undefined，即不应该在顶层代码使用this。
- 变量必须声明后再使用。



## Refer Links

[Javascript 模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

[Javascript 模块化编程（二）：AMD 规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)

[Javascript 模块化编程（三）：require.js 的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)

[AMD 规范和 RequireJS](http://javascript.ruanyifeng.com/tool/requirejs.html)

[JS 模块规范区别 (CommonJS,AMD,CMD,UMD,ES6)](https://blog.suisuijiang.com/javascript-module-standard-difference/)

[JavaScript 模块化 --- Commonjs、AMD、CMD、ES6 modules](http://www.imooc.com/article/20057)  

[ECMAScript 6 入门 Module 的语法](http://es6.ruanyifeng.com/#docs/module)