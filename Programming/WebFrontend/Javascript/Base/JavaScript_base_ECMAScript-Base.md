- [JavaScript Note - ECMAScript](#javascript-note---ecmascript)
	- [1. 概述](#1-%E6%A6%82%E8%BF%B0)
	- [2. 基本语法](#2-%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95)
		- [2.1. 区分大小写](#21-%E5%8C%BA%E5%88%86%E5%A4%A7%E5%B0%8F%E5%86%99)
		- [2.2. 注释](#22-%E6%B3%A8%E9%87%8A)
		- [2.3. 标识符](#23-%E6%A0%87%E8%AF%86%E7%AC%A6)
	- [3. 代码结构](#3-%E4%BB%A3%E7%A0%81%E7%BB%93%E6%9E%84)
		- [3.1. 区块](#31-%E5%8C%BA%E5%9D%97)
		- [3.2. if 结构](#32-if-%E7%BB%93%E6%9E%84)
		- [3.3. switch 结构](#33-switch-%E7%BB%93%E6%9E%84)
		- [3.4. while 循环](#34-while-%E5%BE%AA%E7%8E%AF)
		- [3.5. for 循环](#35-for-%E5%BE%AA%E7%8E%AF)
	- [4. 变量](#4-%E5%8F%98%E9%87%8F)
		- [4.1. 变量定义](#41-%E5%8F%98%E9%87%8F%E5%AE%9A%E4%B9%89)
		- [4.2. 作用域](#42-%E4%BD%9C%E7%94%A8%E5%9F%9F)
		- [4.3. 变量提升](#43-%E5%8F%98%E9%87%8F%E6%8F%90%E5%8D%87)
	- [5. 数据类型](#5-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
		- [5.1. 类型检测](#51-%E7%B1%BB%E5%9E%8B%E6%A3%80%E6%B5%8B)
			- [5.1.1. typeof](#511-typeof)
			- [5.1.2. instanceof](#512-instanceof)
		- [5.2. 原始类型](#52-%E5%8E%9F%E5%A7%8B%E7%B1%BB%E5%9E%8B)
			- [5.2.1. null](#521-null)
			- [5.2.2. undefined](#522-undefined)
			- [5.2.3. Boolean](#523-boolean)
			- [5.2.4. Number](#524-number)
			- [5.2.5. String](#525-string)
		- [5.3. Object](#53-object)
			- [5.3.1. 狭义 Object](#531-%E7%8B%AD%E4%B9%89-object)
			- [5.3.2. 数组 Array](#532-%E6%95%B0%E7%BB%84-array)
			- [5.3.3. 函数 Function](#533-%E5%87%BD%E6%95%B0-function)
				- [5.3.3.1. 函数参数](#5331-%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0)
				- [5.3.3.2. 原型对象](#5332-%E5%8E%9F%E5%9E%8B%E5%AF%B9%E8%B1%A1)
			- [5.3.4. 包装类型](#534-%E5%8C%85%E8%A3%85%E7%B1%BB%E5%9E%8B)
				- [5.3.4.1. Number](#5341-number)
				- [5.3.4.2. String](#5342-string)
				- [5.3.4.3. Boolean](#5343-boolean)
			- [5.3.5. Math](#535-math)
			- [5.3.6. Date](#536-date)
			- [5.3.7. RegExp](#537-regexp)
			- [5.3.8. JSON](#538-json)
			- [5.3.9. console](#539-console)
			- [5.3.10. 内置对象](#5310-%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)
		- [5.4. 数据类型转换](#54-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
	- [6. 异常处理](#6-%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
	- [7. 匿名函数](#7-%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0)
		- [7.1. 递归](#71-%E9%80%92%E5%BD%92)
		- [7.2. 闭包](#72-%E9%97%AD%E5%8C%85)
		- [7.3. 仿块级作用域](#73-%E4%BB%BF%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F)
		- [7.4. 私有变量](#74-%E7%A7%81%E6%9C%89%E5%8F%98%E9%87%8F)
		- [7.5. 立即调用的函数表达式 (IIFE)](#75-%E7%AB%8B%E5%8D%B3%E8%B0%83%E7%94%A8%E7%9A%84%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F-iife)
	- [8. Refer](#8-refer)

# JavaScript Note - ECMAScript

## 1. 概述
https://zh.wikipedia.org/wiki/JavaScript

> JavaScript，一种高级编程语言，通过解释执行，是一门动态类型，面向对象（基于原型）的直译语言。它已经由 ECMA（欧洲电脑制造商协会）通过 ECMAScript 实现语言的标准化。
> JavaScript 是一门基于原型、函数先行的语言 [5]，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本、数组、日期以及正则表达式等，**不支持 I/O**，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。
> 在客户端，JavaScript 在传统意义上被实现为一种解释语言，但在最近，它已经可以被即时编译（JIT）执行。

- ECMAScript 是一种语言标准（常见的 Web 环境实际上只是 ECMAScript 实现可能的宿主环境之一），而 JavaScript 是网景公司对 ECMAScript 标准的一种实现（除此之外还有 ActionScript、ScriptEase 等），所谓的 JavaScript 的版本，实际上即是指它实现了 ECMAScript 标准的哪个版本；
为什么不直接把 JavaScript 定为 标准呢？因为 JavaScript 是网景的注册商标。

- 一般来说，完整的 JavaScript 包括以下几个部分：
	- ECMAScript（语言核心），描述了该语言的语法和基本对象
	- DOM（文档对象模型），描述处理网页内容的方法和接口
	- BOM（浏览器对象模型），描述与浏览器进行交互的方法和接口

## 2. 基本语法

JavaScript 的语法和 Java 语言类似，每个语句以；结束，语句块用{...}；
注：
JavaScript 并不强制要求在每个语句的结尾加 “;”，浏览器中负责执行 JavaScript 代码的引擎会自动在每个语句的结尾补上“;”；
若分号前面可以没有任何内容，JavaScript 引擎将其视为空语句；

JavaScript 程序的执行单位为行（line），也就是一行一行地执行。一般情况下，每一行就是一个语句。

### 2.1. 区分大小写
JavaScript 严格区分大小写。

### 2.2. 注释
ECMAScript 采用 C 风格的注释：

行注释：以 // 开头直到行末的字符，JavaScript 引擎会自动忽略注释；

块注释：用 /*...*/ 把多行字符包裹起来，把一大“块”视为一个注释；

### 2.3. 标识符
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

## 3. 代码结构

### 3.1. 区块
JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）；

与大多数编程语言不一样，**JavaScript 的区块不构成单独的作用域（scope）**。也就是说，区块中的变量与区块外的变量，属于同一个作用域；

### 3.2. if 结构
```javascript
if (expression) {
	statement;
} else if (expression) {
	statement;
} else {
	statement;
}
```

### 3.3. switch 结构
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

ECMAScript 和 Java 中的 switch 语句有两点不同：
- 在 ECMAScript 中，能用不是常量的值说明 case：

	例：
	```javascript
	var BLUE = "blue", RED = "red", GREEN  = "green";

	switch (sColor) {
		case BLUE: alert("Blue");
			break;
		case RED: alert("Red");
			break;
		case GREEN: alert("Green");
			break;
		default: alert("Other");
	}
	```

- 在 ECMAScript 中，switch 语句可以用于字符串

### 3.4. while 循环
```
while (expression) {
  statement;
}
```

### 3.5. for 循环
```
for (initialize; test; increment) {
  statement
}
```

## 4. 变量
### 4.1. 变量定义

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
- 如果只是声明变量而没有赋值，则该变量的值是 undefined。undefined 是一个 JavaScript 关键字，表示“未定义”；

- 如果使用 var 重新声明一个已经存在的变量，是无效的，但若第二次声明的同时还赋值了，则会覆盖掉前面的值：
```javascript
	var x = 1;
	var x = 2;

	// 等同于

	var x = 1;
	var x;
	x = 2;
```

### 4.2. 作用域
Javascript 只有两种作用域：
- 一种是**全局作用域**，变量在整个程序中一直存在，所有地方都可以读取；
- 另一种是**函数作用域**，变量只在函数内部存在；

也就是说，在函数外部声明的变量全都是全局变量（global variable），在函数内部定义的变量，外部无法读取，称为“局部变量”（local variable）；

### 4.3. 变量提升
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

注意：变量提升**只对 var 操作符声明的变量有效**，如果一个变量不是用 var 命令声明的，就不会发生变量提升；

## 5. 数据类型
ECMAScript 中有 5 种基本数据类型，以及 1 种复杂数据类型 --Object；
![image](http://img.cdn.firejq.com/jpg/2017/9/23/86950679098abc59eb5f63113def8929.jpg)

### 5.1. 类型检测
http://www.imooc.com/video/5677

#### 5.1.1. typeof
typeof 操作符返回一个字符串，表示变量的数据类型，可能返回的类型有：number、string、boolean、function、undefined 以及 object（不满足其它类型的情况都返回 object）；

```javascript
var message = "hello";
alert(typeof message);// string
alert(typeof(messgae));// string
```

注意：
- 由于 JavaScript 的历史原因，typeof null 会返回 object，但本质上 null 是一个类似于 undefined 的特殊值；
- typeof 对数组（array）和对象（object）的显示结果都是 object，需要使用 instanceof 运算符区分；
```javascript
alert(typeof NaN);//number
alert(typeof NaN);//object
```

- typeof 一般适用于原始数据类型的判断；

#### 5.1.2. instanceof

instanceof 是基于原型链的类型判断运算符，一般用于 Object 类型的判断；

### 5.2. 原始类型

#### 5.2.1. null
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

#### 5.2.2. undefined
- undefined 表示"未定义"，就是此处应该有一个值，但是还没有定义。典型用法是：
	- 变量被声明了，但没有赋值时，就等于 undefined；
	- 调用函数时，应该提供的参数没有提供，该参数等于 undefined；
	- 对象没有赋值的属性，该属性的值为 undefined；
	- 函数没有返回值时，默认返回 undefined；
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

	alert(b);// 产生异常：Uncaught ReferenceError: a is not defined
	alert(typeof b);// undefined，对未声明的变量，只能执行 typeof 操作，且返回的是 undefined；
```

- undefined 与 null 区分：
	- undefined 派生自 null，因此：
	```
	alert(null == undefined) //true
	```
	- null 是一个表示”无”的对象，转为数值时为 0；undefined 是一个表示”无”的原始值，转为数值时为 NaN；
	```
	Number(undefined) // NaN
	5 + undefined // NaN
	```

#### 5.2.3. Boolean

- Boolean 只有两个字面值：true 和 false；
- Boolean 类型的字面值 true 和 false 是区分大小写的，True、False 等都不是 Boolean 值；
- 与大多数语言不同，在 JavaScript 中，true 不一定等于 1，false 不一定等于 0；

- 如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为 false，其他值都视为 true：
```
undefined
null
false
0
NaN
""或''（空字符串）
```
需要特别注意的是，**空数组（[]）和空对象（{}）对应的布尔值，都是 true**；

#### 5.2.4. Number

- Number 类型使用 IEEE754 标准来表示整数与浮点数值；
- Number 类型可表示多种进制的整数：
	- 十进制数表示
			var n = 55;
	- 八进制数表示
			var n = 070;//56
			var n = 079;// 超过八进制数的限制，将解析为 79
			var n = 080;// 超过八进制数的限制，将解析为 80
	- 十六进制数表示
			var n = 0xA;//10
			var n = 0x1f;//31
	- 进行数值计算时，所有八进制、十六进制数最终都会被转换成十六进制；
- 数值范围：
	- Number.MIN_VALUE 表示 ECMAScript 能表示的最小数值；
	- Number.MAX_VALUE 表示 ECMAScript 能表示的最大数值；
- NaN 表示一个错误的数值
	- 任何数除以 0 都会返回 NaN；
	- NaN 与任何值都不相等，包括 NaN 本身；

#### 5.2.5. String

- String 类型用于表示由零或多个 16 位 Unicode 字符组成的字符序列，即字符串；
- 字符串可以由双引号或单引号表示（在 ECMAScript 中双引号和单引号的解析方式基本完全相同）；
- 任何字符串的长度可以通过其 length 属性获得；
- String 类型的字面值是不可变的，也就是说要改变某个变量保存的字符串，必须先销毁原来的字符串，然后再用另一个包含心智的字符串填充该变量；
例：
```
var lang = "java";
lang = lang + "script"
```

先创建一个能容纳 10 个字符的新字符串，然后在这个字符串中填充 “java” 和 “script”，最后一步是销毁原来字符串 "java" 和字符串 “script”；

### 5.3. Object ###
- 对象（object）是 JavaScript 的核心概念，也是最重要的数据类型；

- 对象可分为三种类型：**狭义的对象（object）、数组（array）、函数（function）**；
其中，狭义的对象和数组是两种不同的数据组合方式，而函数其实是处理数据的方法；

- 需要明确的是，JavaScript 的所有数据，都可以视为广义的对象。不仅数组和函数属于对象，就连原始类型的数据（数值、字符串、布尔值）也可以用对象方式调用；

#### 5.3.1. 狭义 Object
- 对象是一种无序的数据集合，由若干个“键值对”（key-value）构成；

- 对象的创建通常有三种方法：
	- 直接使用大括号创建：
	```javascript
	var o = {
		p: 'Hello World'
	};
	```
	- 对象通过 new 操作符创建：
	```javascript
	var o = new Object();
	var 0 = new Object;// 若构造函数不带参数，可省略括号，但不推荐省略括号
	```

	- 使用 Object.create 方法创建（这种写法一般用在需要对象继承的场合）：
	```javascript
	var o = Object.create(Object.prototype);
	```

- 如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量；

- 类似 Java 中的 Object 类，JavaScript 中 Object 是所有对象类型实例的基础，每个对象类型的实例都有以下属性与方法：
	- constructor：对象构造函数；
	- hasOwnProperty(propertyName)：检查给定属性在当前对象中是否存在；
	- isPrototypeOf(Object)：检查传入的对象是否是另一个对象的原型；
	- propertyIsEnumerable(propertyName)：检查给定属性是否能够使用 for-in- 语句来枚举；
	- toString()：返回对象的字符串表示；
	- valueOf()：返回对象的字符串、数值、布尔值表示；

- 如果行首是一个大括号，它到底是表达式还是语句？
	```
	{ foo: 123 }
	```
	为了避免这种歧义，JavaScript 规定，如果行首是大括号，一律解释为语句（即代码块）；如果要解释为表达式（即对象），必须在大括号前加上圆括号；

- 属性
	- 对象的每一个 “键名” 又称为 “属性”（property），它的 “键值” 可以是任何数据类型；
	- 键名
		- 对象的所有键名都是字符串，所以加不加引号都可以；
		- 如果键名是数值，也会被自动转为字符串；
		- 如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），也不是数字，则必须加上引号，否则会报错；
		- JavaScript 的保留字可以不加引号当作键名；
	- 对象的属性之间用逗号分隔，最后一个属性后面可以加逗号（trailing comma），也可以不加；
	- 属性可以动态创建，不必在对象声明时就指定；
	- 读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符：
		```javascrpit
		var o = {
			p: 'Hello World'
		};

		o.p // "Hello World"
		o['p'] // "Hello World"
		```
		注意：数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符；
	- 查看一个对象本身的所有属性，可以使用 Object.keys 方法；
		```
		var o = {
			key1: 1,
			key2: 2
		};

	Object.keys(o);
	// ['key1', 'key2']
	```

- delete 操作符用于删除对象的属性，删除成功后返回 true；
	注意：delete 命令不能删除 var 命令声明的变量，只能用来删除属性；

- in 运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回 true，否则返回 false；
	例：浏览器环境中检查变量是否已被声明：
	```javascript
	if ('a' in window) {
		// 变量 a 声明过
	} else {
		// 变量 a 未声明
	}
	```
	注意：in 运算符无法区分继承的属性，对继承的属性也返回 true；

- for...in 循环用于遍历一个对象的全部属性；
	注意：
		- 遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性（如 toString 属性）；
		- 不仅遍历对象自身的属性，还遍历继承的属性；
		如果只想遍历对象本身的属性，可以使用 hasOwnProperty 方法，在循环内部判断一下是不是自身的属性；
		- 一般情况下，都是只想遍历对象自身的属性，所以不推荐直接使用 for...in 循环；

	JavaScript 中遍历对象的多种方法：https://huixisheng.github.io/object-loop/


#### 5.3.2. 数组 Array

- 数组（array）是按次序排列的一组值，每个值的位置都有编号（从 0 开始），整个数组用方括号表示；
- 数组的创建方式一般有两种：
	- 使用 Array 构造函数：
	```javascript
	var arr = new Array();
	var arr = new Array(20);// 创建长度为 20 的数组，其中每一项的初始值都是 undefined
	var arr = new Array("one", "two", "three");// 创建长度为 3，且每一项都是字符串的数组
	var arr = Array();// 可省略 new 操作符
	```
	- 使用方括号 []：
	```javascript
	var arr = ['a', 'b', 'c'];
	```
- 同一个数组中可以存放任何不同类型的数据；
- 在 JavaScript 中，数组的长度可以通过 length 属性获取，且 length 属性是可写的，可通过该属性动态调整数组长度；
- 本质上，数组属于一种特殊的对象。typeof 运算符会返回数组的类型是 object；
- 数组的键名是数值类型，但也可以是表示数值的字符串类型（会自动进行隐式类型转换）；
- 检查某个键名是否存在的运算符 in，适用于对象，也适用于数组（数组是一种特殊对象）；
```javascript
	var arr = [ 'a', 'b', 'c' ];
	2 in arr  // true
	'2' in arr // true
	4 in arr // false
```
- 数组的遍历：
```javascript
	var a = [1, 2, 3];

	// for 循环
	for(var i = 0; i < a.length; i++) {
	  console.log(a[i]);
	}

	// while 循环
	var i = 0;
	while (i < a.length) {
	  console.log(a[i]);
	  i++;
	}

	var l = a.length;
	while (l--) {
	  console.log(a[l]);
	}

	// 使用 Array 的 forEach 方法
	a.forEach(function (i) {
		console.log(i);
	})
```

#### 5.3.3. 函数 Function

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function

- 函数是一个特殊的对象（可被调用且有独立作用域）；

- JavaScript 中声明一个函数通常有三种方法：
	- 使用 function 命令
	这种写法将一个匿名函数赋值给变量。这时，这个匿名函数又称函数表达式（Function Expression）
	```javascript
	function print(s) {
		console.log(s);
	}
	```
	采用函数表达式声明函数时，function 命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效；
	- 使用函数表示式
	```javascript
	var print = function(s) {
		console.log(s);
	};
	```
	- 使用 Function 的构造函数
	```javascript
	var add = new Function(
	  'x',
	  'y',
	  'return x + y'
	);

	// 等同于

	function add(x, y) {
	  return x + y;
	}
	```

- 在 JavaScript 中不存在函数重载，因此若重复声明一个函数，后面的声明就会覆盖前面的声明；

- 函数中的 return 语句不是必需的，如果没有 return 的话，该函数就不返回任何值，或者说返回 undefined；

- JavaScript 支持函数的递归（recursion）调用
	```javascript
		function fib(num) {
			if (num === 0) return 0;
			if (num === 1) return 1;
			return fib(num - 2) + fib(num - 1);
		}

		fib(6) // 8
	```

- JavaScript 把函数当成一种数据类型，可以像其他类型的数据一样，进行赋值和传递，这为编程带来了很大的灵活性，体现了 JavaScript 作为 “**函数式语言**” 的本质，在 JavaScript 语言中又称函数为**第一等公民**；
	```javascript
		function add(x, y) {
			return x + y;
		}

		// 将函数赋值给一个变量
		var operator = add;

		// 将函数作为参数和返回值
		function a(op){
			return op;
		}
		a(add)(1, 1)
		// 2
	```

- **函数名提升**

	由于 JavaScript 引擎将函数名视同变量名，因此采用 function 命令声明函数时，整个函数会像变量声明的提升一样，被提升到代码头部；
	```javascript
		// 下面的代码不会报错
		f();
		function f() {}
		// 实际上等同于
		function f() {}
		f();
	```
	但若采用赋值语句定义函数，则效果会不同
	```javascript
		// 下面的代码会报错
		f();
		var f = function (){};
		// TypeError: undefined is not a function
		// 实际上等同于
		var f;
		f();
		f = function () {};
		// 调用 f 的时候，f 只是被声明了，还没有被赋值，等于 undefined，所以会报错
	```
	因此，如果同时采用 function 命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义：
	```javascript
		var f = function() {
			console.log('1');
		}

		function f() {
			console.log('2');
		}

		f() // 1
	```

- 根据 ECMAScript 的规范，不得在非函数的代码块中声明函数，最常见的非函数的代码块就是 if 和 try 语句；

- 函数内部的变量提升

	与全局作用域一样，函数作用域内部也会产生 “变量提升” 现象，var 命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部；
	```javascript
		function foo(x) {
			if (x > 100) {
				var tmp = x - 100;
			}
		}
	```
	等同于
	```javascript
		function foo(x) {
			var tmp;
			if (x > 100) {
				tmp = x - 100;
			};
		}
	```

##### 5.3.3.1. 函数参数

- 调用函数时，函数参数不是必需的，Javascript 允许省略参数，被省略的参数的值为 undefined
	```javascript
		function f(a, b) {
			return a;
		}

		f(1, 2, 3) // 1，运行时无论提供多少个参数（或者不提供参数），JavaScript 都不会报错
		f(1) // 1
		f() // undefined
		f(undefined, 2) // 没有办法只省略靠前的参数，而保留靠后的参数。如果一定要省略靠前的参数，只有显式传入 undefined

		f.length // 2
	```

- 函数参数默认值

	通过下面的方法，可以为函数的参数设置默认值：
	```javascript
		function f(a) {
			(a !== undefined && a !== null) ? a = a : a = 1;
			return a;
		}

		f() // 1
		f('') // ""
		f(0) // 0
	```

- 传参方式
	- 函数参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部；
	```javascript
	var p = 2;

	function f(p) {
	  p = 3;
	}
	f(p);

	p // 2
	```
	- 函数参数如果是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。这意味着，传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值；
	```javascript
	var obj = {p: 1};

	function f(o) {
	  o.p = 2;
	}
	f(obj);

	obj.p // 2
	```
	注意：若在函数内将参数对象整个赋值成另外一个对象，相当于切断了形式参数与实际参数的联系，对形式参数的修改不再影响到实际参数
	```javascript
	var obj = [1, 2, 3];

	function f(o){
	  o = [2, 3, 4];
	}
	f(obj);

	obj // [1, 2, 3]
	```

##### 5.3.3.2. 原型对象

- 属性

	- Function.arguments

		arguments 对象以数组形式包含了函数运行时的所有参数，这个对象只有在函数体内部，才可以使用；
			- arguments[0] 就是第一个参数，arguments[1] 就是第二个参数，以此类推；
			- arguments.length：函数调用时实际传递的参数；

	- Function.prototype.constructor

		声明函数的原型构造方法

- 方法

	- Function.prototype.toString()

		获取函数的实现源码的字符串。覆盖了 Object.prototype.toString 方法。



	- Function.prototype.call() 和 Function.prototype.apply()

		https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call

		https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

		https://www.zhihu.com/question/20289071

		call 和 apply 都是为了改变某个函数运行时的 context 即上下文而存在的，换句话说，就是为了**动态改变函数体内部 this 的指向，在一个对象的上下文中应用另一个对象的方法**。因为 JavaScript 的函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

		**二者的作用完全一样，只是接受参数的方式不太一样：call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里**。

		例如：

		有一个函数 func1 定义如下：
		```javascript
		var func1 = function(arg1, arg2) {};
		```
		就可以通过 `func1.call(this, arg1, arg2);` 或者 `func1.apply(this, [arg1, arg2]);` 来调用。

		其中 this 是你想指定的上下文，他可以是任何一个 JavaScript 对象 (JavaScript 中一切皆对象)。

		- call 和 apply 该用哪个？

			JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话
			- 当参数数量明确知道时，用 call;
			- 当参数数量不确定的时候，用 apply，然后把参数 push 进数组传递进去。若在函数内部也可以通过 arguments 这个数组来遍历所有的参数。

		- 应用实例

			通过 `document.getElementsByTagName` 选择的 dom 节点是一种类似 array 的 array，但它不能应用 Array 下的 push,pop 等方法。因此，我们可以通过：
			```javascript
			var domNodes =  Array.prototype.slice.call(document.getElementsByTagName("*"));
			```
			这样 domNodes 就可以应用 Array 下的所有方法了。



	- Function.prototype.bind()

		https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind

		bind() 方法会创建一个新函数，称为绑定函数。当调用这个绑定函数时，绑定函数会以创建它时传入 bind() 方法的第一个参数作为 this, 传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

		apply 、 call 、bind 三者都是用来改变函数的 this 对象的指向的；区别在于 bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用 。

		NOTE:
		```javascript
		var bar = function(){
				console.log(this.x);
		}
		var foo = {
				x:3
		}
		var sed = {
				x:4
		}
		var func = bar.bind(foo).bind(sed);
		func(); //?

		var fiv = {
				x:5
		}
		var func = bar.bind(foo).bind(sed).bind(fiv);
		func(); //?
		```
		答案是，两次都仍将输出 3 ，而非期待中的 4 和 5 。原因是，在 Javascript 中，多次 bind() 是无效的。更深层次的原因， bind() 的实现，相当于使用函数在内部包了一个 call / apply ，第二次 bind() 相当于再包住第一次 bind() , 故第二次以后的 bind 是无法生效的。



#### 5.3.4. 包装类型

ECMAScript 中每个原始数据类型都存在对应的包装类型：String、Number、Boolean；

例：
![image](http://img.cdn.firejq.com/jpg/2017/9/24/262f2e3b65b49ed14f28e707115801e0.jpg)

- 为什么原始类型可以有 length 属性？为什么动态赋值一个属性 t 后又变成了 undefined？
![image](http://img.cdn.firejq.com/jpg/2017/9/24/de3e87429742ba0aab805b2a4cc8b311.jpg)
当对一个原始数据类型的变量执行 “对象” 的操作时，ECMAScript 会自动创建一个对应的临时包装类型的对象并赋以相同的值，完成用户所需的 “对象” 操作后，又将该临时包装对象销毁。
因此，原始类型的字符串可以成功输出 length 属性并动态赋一个新的属性值，但赋值后该包装类即被销毁故为 undefined。

##### 5.3.4.1. Number

##### 5.3.4.2. String

##### 5.3.4.3. Boolean

#### 5.3.5. Math

#### 5.3.6. Date

#### 5.3.7. RegExp

#### 5.3.8. JSON

#### 5.3.9. console

#### 5.3.10. 内置对象

### 5.4. 数据类型转换

![image](http://img.cdn.firejq.com/jpg/2017/9/24/636660656ff60630d6afaa4e0b37fe8a.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/9/24/5bcb6e749ea0564b0a70c68778cc8500.jpg)

## 6. 异常处理

```javascript
console.log("a");    // 這是正確的
console.log("b");    // 這是正確的
console.logg("c");   // 这是错误的，并且到这里会停下来
console.log("d");    // 這是正確的
console.log("e");    // 這是正確的

/*解決辦法*/
try{console.log("a");}catch(e){}    // 這是正確的
try{console.log("b");}catch(e){}    // 這是正確的
try{console.logg("c");}catch(e){}   // 这是错误的，但是到这里不会停下来，而是跳过
try{console.log("d");}catch(e){}    // 這是正確的
try{console.log("e");}catch(e){}    // 這是正確的
```


## 7. 匿名函数

### 7.1. 递归

### 7.2. 闭包
闭包（closure）是 Javascript 语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现；

http://javascript.ruanyifeng.com/grammar/function.html

### 7.3. 仿块级作用域

### 7.4. 私有变量

### 7.5. 立即调用的函数表达式 (IIFE)

https://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。

例：
```javascript
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function (){
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```
上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。

## 8. Refer

阮一峰的 JavaScript 教程：
http://javascript.ruanyifeng.com/
http://www.ruanyifeng.com/blog/javascript/
http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html
http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html
http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html
http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html

廖雪峰的 JavaScript 教程：
https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000

W3School JavaScript 教程：
http://www.w3school.com.cn/js/

imooc JavaScript 深入浅出：
http://www.imooc.com/learn/277

COOLSHELL JavaScript 面向对象编程：
https://coolshell.cn/articles/6441.html
https://coolshell.cn/articles/6668.html
