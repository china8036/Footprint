<!-- toc -->

# JavaScript Note - ECMAScript #

## 概述 ##
https://zh.wikipedia.org/wiki/JavaScript

> JavaScript，一种高级编程语言，通过解释执行，是一门动态类型，面向对象（基于原型）的直译语言。它已经由ECMA（欧洲电脑制造商协会）通过ECMAScript实现语言的标准化。
> JavaScript是一门基于原型、函数先行的语言[5]，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本、数组、日期以及正则表达式等，**不支持I/O**，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。
> 在客户端，JavaScript在传统意义上被实现为一种解释语言，但在最近，它已经可以被即时编译（JIT）执行。


- ECMAScript 是一种语言标准（常见的 Web 环境实际上只是 ECMAScript 实现可能的宿主环境之一），而 JavaScript 是网景公司对 ECMAScript 标准的一种实现（除此之外还有 ActionScript、ScriptEase 等），所谓的 JavaScript 的版本，实际上即是指它实现了 ECMAScript 标准的哪个版本；
为什么不直接把 JavaScript定为 标准呢？因为 JavaScript 是网景的注册商标。

- 一般来说，完整的JavaScript包括以下几个部分：   
	- ECMAScript（语言核心），描述了该语言的语法和基本对象
	- DOM（文档对象模型），描述处理网页内容的方法和接口
	- BOM（浏览器对象模型），描述与浏览器进行交互的方法和接口

## 基本语法 ##

JavaScript的语法和Java语言类似，每个语句以;结束，语句块用{...}；
注：
JavaScript并不强制要求在每个语句的结尾加 “;”，浏览器中负责执行 JavaScript 代码的引擎会自动在每个语句的结尾补上“;”；
若分号前面可以没有任何内容，JavaScript引擎将其视为空语句；

JavaScript程序的执行单位为行（line），也就是一行一行地执行。一般情况下，每一行就是一个语句。


### 区分大小写 ###
JavaScript 严格区分大小写。


### 注释 ###
ECMAScript 采用 C 风格的注释：

行注释：以//开头直到行末的字符，JavaScript引擎会自动忽略注释；

块注释：用/*...*/把多行字符包裹起来，把一大“块”视为一个注释；

### 标识符 ###
标识符指的是变量、函数、函数参数、属性的命名；

- 标识符命名规范 
	- 只能包含字母、下划线、美元符号、数字；
	- 首字符不能是数字；
	- ECMAScript 标识符采用驼峰命名法；
	- 标识符中支持使用扩展的 ASCII 或 Unicode 字符，但一般不推荐使用；
	- 不可使用保留字作为标识符（Infinity、NaN、undefined 不是保留字，但有特殊含义，也不应作为标识符）；
合法标识符：
```
arg0
_tmp
$elem
π
临时变量
```
非法标识符：
```
1a  // 第一个字符不能是数字
23  // 同上
***  // 标识符不能包含星号
a+b  // 标识符不能包含加号
-d  // 标识符不能包含减号或连词线
```

## 代码结构 ##

### 区块 ###
JavaScript使用大括号，将多个相关的语句组合在一起，称为“区块”（block）；

与大多数编程语言不一样，**JavaScript的区块不构成单独的作用域（scope）**。也就是说，区块中的变量与区块外的变量，属于同一个作用域；

### if 结构 ###
```javascript
if (expression) {
	statement;
} else if (expression) {
	statement;
} else {
	statement;
}
```


### switch 结构 ###
```
switch (fruit) {
  case "banana":
    // ...
    break;
  case "apple":
    // ...
    break;
  default:
    // ...
}
```

### while 循环 ###
```
while (expression) {
  statement;
}
```

### for 循环 ###
```
for (initialize; test; increment) {
  statement
}
```












## 变量 ##
### 变量定义 ###

- ECMAScript 中，使用** var 操作符**定义的变量将成为定义该变量的作用域中的**局部变量**，若不使用 var 操作符，将创建一个**全局变量**；
例：   
使用 var 操作符定义局部变量：
```javascript
function test() {
	var i = "hello"
}
test();
alert(i);	// undefined
```
不使用 var 操作符定义全局变量：
```javascript
function test() {
	i = "hello"
}
test();
alert(i);	// "hello"
```

- ECMAScript 中变量是松散类型的，即可用于保存任何类型的数据，每个变量仅仅引用 “值” 的占位符，可以在修改变量值的同时修改值的类型（但不建议这么做）：
```javascript
var a = 1;
a = "hello";
```

- 变量是对“值”的引用，使用变量等同于引用一个值；
- 如果只是声明变量而没有赋值，则该变量的值是undefined。undefined是一个JavaScript关键字，表示“未定义”；

- 如果使用 var 重新声明一个已经存在的变量，是无效的，但若第二次声明的同时还赋值了，则会覆盖掉前面的值：
```javascript
	var x = 1;
	var x = 2;
	
	// 等同于
	
	var x = 1;
	var x;
	x = 2;
```


### 作用域 ###
**JavaScript 中只有函数作用域**，即只有在函数体中存在独立的作用域。




### 变量提升 ###
在解析 JavaScript 脚本时，JavaScript 引擎的工作方式是：先解析所有使用 var 操作符声明的变量，然后再一行一行地运行其它代码。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做**变量提升（hoisting）**；

例：
```javascript
	console.log(a);
	var a = 1;
	
	// 等同于
	
	var a;
	console.log(a);
	a = 1;
```

注意：变量提升**只对 var 操作符声明的变量有效**，如果一个变量不是用var命令声明的，就不会发生变量提升；

## 数据类型 ##
ECMAScript 中有5种基本数据类型，以及1种复杂数据类型--Object；
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/23/86950679098abc59eb5f63113def8929.jpg)

### typeof ###

typeof 操作符返回变量的类型：
```
var message = "hello";
alert(typeof message);// string
alert(typeof(messgae));// string
```

### 原始类型 ###

#### null ####
- null 表示一个空对象指针，即 “没有对象”，该处不应该有值。典型用法是：
	- 作为函数的参数，表示该函数的参数不是对象；
	- 作为对象原型链的终点；
	```javascript
	Object.getPrototypeOf(Object.prototype)
	// null
	```
	- 用于检查某个标识符是否已保存了对象的引用
	```javascript
	if (car != null) {
		// ...
	}
	```

- 只要在保存对象的变量还没有真正保存对象之前，都应该明确地将该变量赋值为 null；

#### undefined ####
- undefined表示"未定义"，就是此处应该有一个值，但是还没有定义。典型用法是：
	- 变量被声明了，但没有赋值时，就等于undefined；
	- 调用函数时，应该提供的参数没有提供，该参数等于undefined；
	- 对象没有赋值的属性，该属性的值为undefined；
	- 函数没有返回值时，默认返回undefined；
```javascript
	var i;
	i // undefined
	
	function f(x) {
		console.log(x)
	}
	f() // undefined
	
	var  o = new Object();
	o.p // undefined
	
	var x = f();
	x // undefined
```


- 一般情况下不存在需要显示将一个变量赋值为 undefined 的情况，字面值 undefined 主要用于确定变量是否已被赋值，以及区分空对象指针与未经初始化的变量：
```javascript
	var a;
	alert(a);// undefined
	alert(typeof a);// undefined
	
	alert(b);//产生异常：Uncaught ReferenceError: a is not defined
	alert(typeof b);// undefined，对未声明的变量，只能执行 typeof 操作，且返回的是 undefined；
```




- 实际上，undefined 派生自 null，因此：
```
alert(null == undefined) //true
```

#### Boolean ####

- Boolean 只有两个字面值：true 和 false；
- Boolean 类型的字面值 true 和 false 是区分大小写的，True、False 等都不是 Boolean 值；
- 与大多数语言不同，在 JavaScript 中，true 不一定等于1，false 不一定等于0；




#### Number ####

- Number 类型使用 IEEE754 标准来表示整数与浮点数值；
- Number 类型可表示多种进制的整数：
	- 十进制数表示
			var n = 55;
	- 八进制数表示
			var n = 070;//56
			var n = 079;//超过八进制数的限制，将解析为79
			var n = 080;//超过八进制数的限制，将解析为80
	- 十六进制数表示
			var n = 0xA;//10
			var n = 0x1f;//31
	- 进行数值计算时，所有八进制、十六进制数最终都会被转换成十六进制；
- 数值范围：
	- Number.MIN_VALUE 表示 ECMAScript 能表示的最小数值；
	- Number.MAX_VALUE 表示 ECMAScript 能表示的最大数值；
- NaN 表示一个错误的数值
	- 任何数除以0都会返回 NaN；
	- NaN 与任何值都不相等，包括 NaN 本身；

#### String ####

- String 类型用于表示由零或多个16位 Unicode 字符组成的字符序列，即字符串；
- 字符串可以由双引号或单引号表示（在 ECMAScript 中双引号和单引号的解析方式基本完全相同）；
- 任何字符串的长度可以通过其 length 属性获得；
- String 类型的字面值是不可变的，也就是说要改变某个变量保存的字符串，必须先销毁原来的字符串，然后再用另一个包含心智的字符串填充该变量；
例：   
```
var lang = "java";
lang = lang + "script"
```
先创建一个能容纳10个字符的新字符串，然后在这个字符串中填充 “java” 和 “script”，最后一步是销毁原来字符串 "java" 和字符串 “script”；
- 




#### Object ####
- Object 即对象类型，在 JavaScript 中函数、数组、日期等都是对象，其中函数是一个特殊的对象（可被调用且有独立作用域）；
- 对象是一组数据和功能的集合，通过 new 操作符创建：
```javascript
var o = new Object();
var 0 = new Object;//若构造函数不带参数，可省略括号，但不推荐省略括号
```
- 类似 Java 中的 Object 类，JavaScript 中 Object 是所有对象类型实例的基础，每个对象类型的实例都有以下属性与方法：
	- constructor：对象构造函数；
	- hasOwnProperty(propertyName)：检查给定属性在当前对象中是否存在；
	- isPrototypeOf(Object)：检查传入的对象是否是另一个对象的原型；
	- propertyIsEnumerable(propertyName)：检查给定属性是否能够使用 for-in- 语句来枚举；
	- toString()：返回对象的字符串表示；
	- valueOf()：返回对象的字符串、数值、布尔值表示；


### 数据类型转换 ###




### 引用数据类型 ###









## 函数 ##




## 内置对象 ##



## 异常处理 ##


```javascript
console.log("a");    //這是正確的
console.log("b");    //這是正確的
console.logg("c");   //这是错误的，并且到这里会停下来
console.log("d");    //這是正確的
console.log("e");    //這是正確的

/*解決辦法*/
try{console.log("a");}catch(e){}    //這是正確的
try{console.log("b");}catch(e){}    //這是正確的
try{console.logg("c");}catch(e){}   //这是错误的，但是到这里不会停下来，而是跳过
try{console.log("d");}catch(e){}    //這是正確的
try{console.log("e");}catch(e){}    //這是正確的
```





## 面向对象 ##
Javascript继承机制的设计思想 http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html 











## 立即调用的函数表达式 ##
https://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html 











## Refer ##
阮一峰的JavaScript教程：   
http://javascript.ruanyifeng.com/    
http://www.ruanyifeng.com/blog/javascript/    

廖雪峰的 JavaScript 教程：   
https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000   

W3School JavaScript 教程：   
http://www.w3school.com.cn/js/    

imooc JavaScript 深入浅出：    
http://www.imooc.com/learn/277    




































