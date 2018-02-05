- [JavaScript this 关键字](#javascript-this-%E5%85%B3%E9%94%AE%E5%AD%97)
  - [1. 从函数调用说起](#1-%E4%BB%8E%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E8%AF%B4%E8%B5%B7)
  - [2. func() 形式](#2-func-%E5%BD%A2%E5%BC%8F)
  - [3. obj.child.method() 形式](#3-objchildmethod-%E5%BD%A2%E5%BC%8F)
  - [4. Refer Links](#4-refer-links)

# JavaScript this 关键字

一道经典的 JavaScript 面试题：
```javascript
var obj = {
  foo: function(){
    console.log(this)
  }
}

var bar = obj.foo
obj.foo() // 打印出的 this 是 obj
bar() // 打印出的 this 是 window
// 请解释最后两行函数的值为什么不一样？
```

## 1. 从函数调用说起

JS（ES5）里面有三种函数调用形式：
```javascript
func(p1, p2) 
obj.child.method(p1, p2)
func.call(context, p1, p2) // 先不讲 apply
```

对于这三种形式，其中第三种调用形式才是原生的函数调用形式，其他两种都是语法糖，可以等价地变为 call 形式：
```javascript
func(p1, p2) 等价于
func.call(undefined, p1, p2)

obj.child.method(p1, p2) 等价于
obj.child.method.call(obj.child, p1, p2)
```
而 this，就是上面代码中 call 一个函数时传的 context 参数。

## 2. func() 形式

```javascript
function func(){
  console.log(this)
}

func()
```
等价于：
```javascript
function func(){
  console.log(this)
}

func.call(undefined)
```
但在浏览器中有一条规则：如果你传的 context 就 null 或者 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 为 undefined）。

在浏览器中全局函数都是 window 对象的方法，因此 func() 等与 window.func() 等于 window.func.call(window)。

因此上面的打印结果是 window。

如果你希望这里的 this 不是 window：
```javscript
func.call(obj) // 那么里面的 this 就是 obj 对象了
```

## 3. obj.child.method() 形式

```javascript
var obj = {
  foo: function(){
    console.log(this)
  }
}

obj.foo() 
```
等价于：
```javascript
var obj = {
  foo: function(){
    console.log(this)
  }
}

obj.foo.call(obj)
```

## 4. Refer Links

https://zhuanlan.zhihu.com/p/23804247

http://huang-jerryc.com/2017/07/15/understand-this-of-javascript/
