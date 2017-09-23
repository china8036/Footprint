<!-- toc -->
# JavaScript 实践 TIPS #

## 12种不宜使用的 Javascript 语法 ##
http://www.ruanyifeng.com/blog/2010/01/12_javascript_syntax_structures_you_should_not_use.html 

1.	Javascript有两组相等运算符，一组是==和!=，另一组是===和!==。前者只比较值的相等，后者除了值以外，还比较类型是否相同。请尽量不要使用前一组，永远只使用===和!==。因为==默认会进行类型转换，规则十分难记。
例：
```javascript
false == 'false' (false)
false == undefined (false)
false == null (false)
null == undefined (true)
0 == '' (true)
```
2.	with的本意是减少键盘输入。但在实际运行时导致了低效率，而且可能会导致意外，因此最好不要使用with语句。

3.	eval用来直接执行一个字符串。这条语句也是不应该使用的，因为它有性能和安全性的问题，并且使得代码更难阅读。eval能够做到的事情，不用它也能做到。
4.	continue的作用是返回到循环的头部，但是循环本来就会返回到头部。所以通过适当的构造，完全可以避免使用这条命令，使得效率得到改善。

5.	switch结构中凡是有case的地方，一律加上break。
6.	if、while、do和for的块结构语句不管是否只有一行命令，都一律加上大括号。

7.	递增运算符++和递减运算符--，直接来自C语言，表面上可以让代码变得很紧凑，但是实际上会让代码看上去更复杂和更晦涩。因此为了代码的整洁性和易读性，不用为好。

8.	Javascript完全套用了Java的位运算符，包括按位与&、按位或|、按位异或^、按位非~、左移<<、带符号的右移>>和用0补足的右移>>>。这套运算符针对的是整数，所以对Javascript完全无用，因为Javascript内部，所有数字都保存为双精度浮点数。如果使用它们的话，Javascript不得不将运算数先转为整数，然后再进行运算，效率极低，因此JavaScript中尽量不用位运算。

9.	在Javascript中定义一个函数，有两种写法：
```javascript
//第一种
function foo() { }
//第二种
var foo = function () { }
```
两种写法完全等价。但是在解析的时候，前一种写法会被解析器自动提升到代码的头部，因此违背了函数应该先定义后使用的要求，所以建议定义函数时，全部采用后一种写法。
10.	如果不是必要，不用基本数据类型的包装对象来定义变量，new Object和new Array也不建议使用，可以用{}和[]代替。
11.	JavaScript中利用函数生成类、利用new生成对象的语法，其实非常奇怪，一点都不符合直觉，而且，使用的时候，很容易忘记加上new，就会变成执行函数，然后莫名其妙多出几个全局变量。所以，建议不要这样创建对象，而采用一种变通方法：
```javascript
Object.beget = function (o) {
var F = function (o) {};
F.prototype = o ;
return new F;
};
//创建对象时就利用这个函数，对原型对象进行操作：
var Cat = {
　　name:'',
　　saying:'meow'
};
var myCat = Object.beget(Cat);
//对象生成后，可以自行对相关属性进行赋值：
myCat.name = 'mimi';
```
12.	在大多数语言中，void都是一种类型，表示没有值。但是在Javascript中，void是一个运算符，接受一个运算数，并返回undefined。但这个命令没多大用处，而且令人困惑，应尽量不用。




## JavaScript 的10个设计缺陷 ##
http://www.ruanyifeng.com/blog/2011/06/10_design_defects_in_javascript.html 


1.	不适合开发大型程序
Javascript没有名称空间（namespace），很难模块化；没有如何将代码分布在多个文件的规范；允许同名函数的重复定义，后面的定义可以覆盖前面的定义，很不利于模块化加载。
2.	标准库缺乏
Javascript提供的标准函数库非常小，只能完成一些基本操作，很多功能都不具备。
3.	null和undefined
null属于对象（object）的一种，意思是该对象为空；undefined则是一种数据类型，表示未定义。
```javascript
typeof null; // object
typeof undefined; // undefined
```
两者非常容易混淆，但是含义完全不同。在编程实践中，null几乎没用，根本不应该设计它。
4.	全局变量难以控制
Javascript的全局变量，在所有模块中都是可见的；任何一个函数内部都可以生成全局变量，这大大加剧了程序的复杂性。
5.	自动插入行尾分号
Javascript的所有语句，都必须以分号结尾。但是，如果你忘记加分号，解释器并不报错，而是为你自动加上分号。有时候，这会导致一些难以发现的错误。
6.	加号运算符
加号作为运算符，有两个含义，可以表示数字与数字的和，也可以表示字符与字符的连接。这样的设计，不必要地加剧了运算的复杂性，完全可以另行设置一个字符连接的运算符。
7.	NaN
NaN是一种数字，表示超出了解释器的极限。但与其设计NaN，不如解释器直接报错，反而有利于简化程序。
8.	对象和数组的区分
由于Javascript的数组也属于对象（object），所以要区分一个对象到底是不是数组，相当麻烦。
区分代码：
```javascript
　　if ( arr && 
　　　　typeof arr === 'object' &&
　　　　typeof arr.length === 'number' &&
　　　　!arr.propertyIsEnumerable('length')){
　　　　alert("arr is an array");
　　}
```
9.	== 和 ===
==用来判断两个值是否相等。当两个值类型不同时，会发生自动转换，得到的结果非常不符合直觉。因此，推荐任何时候都使用"==="（精确判断）比较符。
10.	基本类型的包装对象
Javascript有三种基本数据类型：字符串、数字和布尔值。它们都有相应的建构函数，可以生成字符串对象、数字对象和布尔值对象。与基本数据类型对应的对象类型，作用很小，造成的混淆却很大，尽量不用。

