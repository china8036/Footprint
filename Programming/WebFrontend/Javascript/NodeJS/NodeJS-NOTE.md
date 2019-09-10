- [NodeJS 模块](#nodejs-%E6%A8%A1%E5%9D%97)
  - [1. require](#1-require)
    - [1.1. 常用用法](#11-%E5%B8%B8%E7%94%A8%E7%94%A8%E6%B3%95)
    - [1.2. require的模块查找策略](#12-require%E7%9A%84%E6%A8%A1%E5%9D%97%E6%9F%A5%E6%89%BE%E7%AD%96%E7%95%A5)
  - [2. export 和 module.export](#2-export-%E5%92%8C-moduleexport)
  - [3. Refer Links](#3-refer-links)

# NodeJS 模块


Node 使用 CommonJs 模块系统。

NodeJS 自身实现了 require 方法作为其引入模块的方法。同时 NPM 也基于 CommonJS 定义的包规范，实现了依赖管理和模块自动安装等功能。

## 1. require

### 1.1. 常用用法
- 内置模块： require('http')
- ./ 表示当前路径，后面跟的是相对路径 ： require('./server')
- ../ 表示上一级目录，后面跟的也是相对路径：require("../lib/server")

### 1.2. require的模块查找策略

![image](http://img.cdn.firejq.com/jpg/2017/12/10/76c3c8b20780215de16802483558d0ef.jpg)

对于原生模块：优先级仅次于文件模块缓存的优先级。

以http模块为例，尽管在目录下存在一个http/http.js/http.node/http.json文件，require(“http”)都不会从这些文件中加载，而是从原生模块中加载。学过 Java 的可能知道，有点 ClassLoader 的味道。

require 方法接受以下 4 种参数的传递：
- http、fs、path等，原生模块。
- ./mod或../mod，相对路径的文件模块。
- /pathtomodule/mod，绝对路径的文件模块。
- mod，非原生模块的文件模块。


当文件模块缓存区没有，并且不是原生模块的时候：
- 如果 require 的参数带 / 或者 ./ 或者 ../ 那么直接从绝对路径或者相对路径查找。
- 如果 require 的不带 / 或者 ./ 或者 ../ 那么首先从当前文件目录开始查找`node_modules`目录；然后依次进入父目录，查找父目录下的`node_modules`目录；依次迭代，直到根目录下的`node_modules`目录。


Ps：如果 require 的时候没有写明文件后缀，那么默认按照 .js / .json / .node 进行查找。

## 2. export 和 module.export

在 node 中，一个文件就是一个模块。实际上，为了让各个文件里的变量互不干扰，node 让每个模块都放在一个闭包中执行，这样实现的模块的隔离。而要让模块间相互联系，就需要暴露变量。

在 node 源码里，定义了一个 Module 构造函数，每个模块都是 Module 的实例，而 Module 有一个属性为 Module.export，初始化的值是 {}。

```javascritp
function Module(id, parent) {
  this.id = id;
  this.exports = {};
  ...
}
module.exports = Module;
```

**exports 对象是指向的 module.exports 的引用，module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {}，require() 返回的是 module.exports 而不是 exports。**


模块想要对外暴露变量只需要对 exports 赋值：
```javascript
function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

exports.foo = foo;
exports.bar = bar;
```
也可以对 module.exports 赋值：
```javascript
module.exports = {
  foo: foo,
  bar: bar
}
```
但是不能直接对 exports 赋值，因为这样做仅仅改变了 exports 的引用，而没有改变 module.exports：
```javascript
// 错误
exports = {
  foo: foo,
  bar: bar
}
```

区别：
- export
  ```javascript
  //test.js
  var Student=function(){
      var name='';

      this.setName=function(n){
          name=n;
      };

      this.printName=function(){
          console.log(name)    ;
      };
  };

  exports.Student=Student;

  //main.js
  var Student = require('./test').Student;
  var student = new Student();
  student.setName('Byron');
  student.printName();
  ```
- module.export：require语句可以更优雅一些
  ```javascript
  //test.js
  var Student=function(){
    var name='';

    this.setName=function(n){
        name=n;
    };

    this.printName=function(){
        console.log(name);
    };
  };

  module.exports=Student;

  //main.js
  var Student=require('./test');
  var student=new Student();
  student.setName('Byron');
  student.printName();
  ```




## 3. Refer Links

[NodeJs：require，exports，module 概念](https://github.com/pzxwhc/MineKnowContainer/issues/39)

[Node.js 模块里 exports 与 module.exports 的区别？](https://www.zhihu.com/question/26621212)

[Node.js Document export](https://nodejs.org/docs/latest/api/modules.html#modules_exports)
