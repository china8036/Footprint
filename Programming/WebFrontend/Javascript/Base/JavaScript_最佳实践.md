- [JavaScript 最佳实践](#javascript-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [1. 命名规范](#1-%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
  - [2. 高可维护性](#2-%E9%AB%98%E5%8F%AF%E7%BB%B4%E6%8A%A4%E6%80%A7)
  - [3. Refer Links](#3-refer-links)

# JavaScript 最佳实践

## 1. 命名规范

- JavaScript 局部变量命名采用首字母小写，其它单词首字母大写的方式；

- 命名应采用有意义的单词命名，不推荐使用标识变量类型的前缀命名，如 int、str 等；

- 在 JavaScript 面向对象变成中，公有接口的命名为首字母大写，私有接口的命名为首字母小写；

- 在使用 jQuery 的项目中，推荐给 jQuery 类型变量添加“$”为前缀，如：`var $tocTitle = $('.reader-toc-title');`；

- JavaScript 中单引号与双引号几乎没有区别，但推荐在 JavaScript 中使用单引号，便于在 JavaScript 中包含惯用双引号的 HTML 代码；

- 若注释为未占多行，建议使用 //，不推荐使用 /**/；注释应单独占一行，而不是在代码同一行直接添加；

## 2. 高可维护性

- 避免定义全局变量和全局函数
  推荐的方案是使用代码模块化的思想，即利用立即执行函数，将全局变量和函数包含在一个局部作用域中，在该作用域中完成这些变量的定义和操作逻辑，最大限度的防止了代码之间的污染，同时巧妙实现了代码代码逻辑的封装和公共接口的公开；   
  例：
  ```javascript
  var myAction = (function() {
    var length = 0;
    var init = function() {
      ...
    };
    var action1 = function() {
      ...
    };
    var action2 = function() {
      ...
    };
    return {
      init: init,
      action1: action1
    };
  })();
  ```
  在定时变量时应一直使用 var 关键字；

- 使用简化的编码方式
  ```javascript
  // 创建对象，无需使用 new Object()
  person = {
    age: 25,
    name: 'Jack'
  };

  // 创建数组，无需使用 new Array()
  list = [12, 20, 24];
  ```

- 使用`===`和`!==`而不是`==`和`!=`

- 避免使用 with 语句

- 避免使用 eval 语句
  与 eval 函数类似的还有 setTimeout 和 setInterval 函数，当接收字符串参数时，这些函数会做类似 eval 函数的处理，将字符串作为代码直接执行，因此，此类函数应避免使用字符串参数；   
  此外，Function 构造器与 eval 函数的功能类似，也应避免使用；

- 不要编写检测浏览器的代码
  最好的做法是直接检测浏览器是否支持某个特定功能（可以利用 Modernizr 框架进行检测）；

- 使用严格模式
  在严格模式下的编码有助于更早的发现可能存在的问题，因此应使用 JavaScript 的严格模式进行开发；   
  但不应在全局中启用严格模式，因为无法保证其它模块的代码也是符合严格模式的；

## 3. Refer Links

https://zoomzhao.github.io/code-guide/

《Web 前端开发最佳实践》 党建 著

http://yincheng.site/less-coupling