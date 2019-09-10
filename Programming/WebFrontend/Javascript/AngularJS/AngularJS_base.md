- [AngularJS Base Note](#angularjs-base-note)
	- [1. 作用域与 $scope](#1-%E4%BD%9C%E7%94%A8%E5%9F%9F%E4%B8%8E-scope)
		- [1.1. 概述](#11-%E6%A6%82%E8%BF%B0)
		- [1.2. $scope 生命周期](#12-scope-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
		- [1.3. 属性和工具方法](#13-%E5%B1%9E%E6%80%A7%E5%92%8C%E5%B7%A5%E5%85%B7%E6%96%B9%E6%B3%95)
			- [1.3.1. 属性](#131-%E5%B1%9E%E6%80%A7)
			- [1.3.2. 工具方法](#132-%E5%B7%A5%E5%85%B7%E6%96%B9%E6%B3%95)
	- [2. 双向数据绑定](#2-%E5%8F%8C%E5%90%91%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A)
		- [2.1. ng-moddel](#21-ng-moddel)
		- [2.2. ng-bind](#22-ng-bind)
	- [3. MVC](#3-mvc)
		- [3.1. Controller](#31-controller)
		- [3.2. Model](#32-model)
		- [3.3. View](#33-view)
			- [3.3.1. 表达式](#331-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
	- [4. 模块](#4-%E6%A8%A1%E5%9D%97)
		- [4.1. 概述](#41-%E6%A6%82%E8%BF%B0)
		- [4.2. 模块定义](#42-%E6%A8%A1%E5%9D%97%E5%AE%9A%E4%B9%89)
		- [4.3. 模块加载](#43-%E6%A8%A1%E5%9D%97%E5%8A%A0%E8%BD%BD)
			- [4.3.1. config](#431-config)
			- [4.3.2. run](#432-run)
		- [4.4. 项目模块化](#44-%E9%A1%B9%E7%9B%AE%E6%A8%A1%E5%9D%97%E5%8C%96)
	- [5. 服务](#5-%E6%9C%8D%E5%8A%A1)
		- [5.1. 概述](#51-%E6%A6%82%E8%BF%B0)
		- [5.2. 内置服务](#52-%E5%86%85%E7%BD%AE%E6%9C%8D%E5%8A%A1)
			- [5.2.1. $location](#521-location)
				- [5.2.1.1. path()](#5211-path)
				- [5.2.1.2. replace()](#5212-replace)
				- [5.2.1.3. absUrl()](#5213-absurl)
			- [5.2.2. $http](#522-http)
				- [5.2.2.1. General usage](#5221-general-usage)
				- [5.2.2.2. Shortcut](#5222-shortcut)
				- [5.2.2.3. 文件上传](#5223-%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0)
			- [5.2.3. $cookies](#523-cookies)
			- [5.2.4. $log](#524-log)
			- [5.2.5. $sce](#525-sce)
		- [5.3. 服务定义](#53-%E6%9C%8D%E5%8A%A1%E5%AE%9A%E4%B9%89)
	- [6. 依赖注入](#6-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
		- [6.1. 隐式依赖注入](#61-%E9%9A%90%E5%BC%8F%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
		- [6.2. 显示依赖注入](#62-%E6%98%BE%E7%A4%BA%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
		- [6.3. 行内依赖注入](#63-%E8%A1%8C%E5%86%85%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
		- [6.4. 应用](#64-%E5%BA%94%E7%94%A8)
			- [6.4.1. 模块依赖注入](#641-%E6%A8%A1%E5%9D%97%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
			- [6.4.2. 控制器依赖注入](#642-%E6%8E%A7%E5%88%B6%E5%99%A8%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
	- [7. 路由](#7-%E8%B7%AF%E7%94%B1)
		- [7.1. ngRoute](#71-ngroute)
		- [7.2. uiRoute](#72-uiroute)
			- [7.2.1. 使用 $stateProvider 和 $urlRouterProvider 配置路由](#721-%E4%BD%BF%E7%94%A8-stateprovider-%E5%92%8C-urlrouterprovider-%E9%85%8D%E7%BD%AE%E8%B7%AF%E7%94%B1)
			- [7.2.2. 页面跳转](#722-%E9%A1%B5%E9%9D%A2%E8%B7%B3%E8%BD%AC)
				- [7.2.2.1. 使用 ui-sref 指令](#7221-%E4%BD%BF%E7%94%A8-ui-sref-%E6%8C%87%E4%BB%A4)
				- [7.2.2.2. 使用 $state 服务](#7222-%E4%BD%BF%E7%94%A8-state-%E6%9C%8D%E5%8A%A1)
			- [7.2.3. 嵌套视图](#723-%E5%B5%8C%E5%A5%97%E8%A7%86%E5%9B%BE)
			- [7.2.4. $state 多视图](#724-state-%E5%A4%9A%E8%A7%86%E5%9B%BE)
		- [7.3. 预载入 Resolve](#73-%E9%A2%84%E8%BD%BD%E5%85%A5-resolve)
		- [7.4. 使用 $transition 监控路由变化](#74-%E4%BD%BF%E7%94%A8-transition-%E7%9B%91%E6%8E%A7%E8%B7%AF%E7%94%B1%E5%8F%98%E5%8C%96)
		- [7.5. onEnter](#75-onenter)
		- [7.6. onExit](#76-onexit)
	- [8. 指令系统](#8-%E6%8C%87%E4%BB%A4%E7%B3%BB%E7%BB%9F)
		- [8.1. 生命周期](#81-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
		- [8.2. 指定定义](#82-%E6%8C%87%E5%AE%9A%E5%AE%9A%E4%B9%89)
			- [8.2.1. restrict 匹配模式](#821-restrict-%E5%8C%B9%E9%85%8D%E6%A8%A1%E5%BC%8F)
			- [8.2.2. replace](#822-replace)
			- [8.2.3. templateUrl](#823-templateurl)
			- [8.2.4. template](#824-template)
			- [8.2.5. scope 作用域](#825-scope-%E4%BD%9C%E7%94%A8%E5%9F%9F)
			- [8.2.6. controller、compile、link](#826-controller%E3%80%81compile%E3%80%81link)
		- [8.3. 内置指令](#83-%E5%86%85%E7%BD%AE%E6%8C%87%E4%BB%A4)
			- [8.3.1. 布尔类型指令](#831-%E5%B8%83%E5%B0%94%E7%B1%BB%E5%9E%8B%E6%8C%87%E4%BB%A4)
			- [8.3.2. 在指令中使用子作用域](#832-%E5%9C%A8%E6%8C%87%E4%BB%A4%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%AD%90%E4%BD%9C%E7%94%A8%E5%9F%9F)
	- [9. 过滤器](#9-%E8%BF%87%E6%BB%A4%E5%99%A8)
		- [9.1. 使用过滤器](#91-%E4%BD%BF%E7%94%A8%E8%BF%87%E6%BB%A4%E5%99%A8)
		- [9.2. 内置过滤器](#92-%E5%86%85%E7%BD%AE%E8%BF%87%E6%BB%A4%E5%99%A8)
			- [9.2.1. currency](#921-currency)
			- [9.2.2. number](#922-number)
			- [9.2.3. date](#923-date)
			- [9.2.4. lowercase/uppercase](#924-lowercaseuppercase)
			- [9.2.5. limitTo](#925-limitto)
			- [9.2.6. orderBy](#926-orderby)
		- [9.3. 自定义过滤器](#93-%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%BB%A4%E5%99%A8)
	- [10. Refer Links](#10-refer-links)

# AngularJS Base Note

## 1. 作用域与 $scope
### 1.1. 概述
- 作用域是视图和控制器之间的胶水。在应用将视图渲染并呈现给用户之前，视图中的模板会和作用域进行连接，然后应用会对 DOM 进行设置以便将属性变化通知给 Angular。 我们可以依赖视图在修改数据时立刻更新 $scope，也可以依赖 $scope 在其发生变化时立刻重新渲染视图。我们可以将应用的业务逻辑都放在控制器中，而将相关的数据都放在控制器的作用域中。
- $scope 是一个指向应用 model 的 object，也是表达式的执行上下文（作用域）；
- $scope 被放置于一个类似应用的 DOM 结构的树形层次结构中，子 $scope 对象会继承父 $scope 对象的属性和方法；
- 每个 Angular 应用（ng-app）都有且只有一个根 $scope，即 $rootScope，$rootScope 是所有 $scope 对象的最上层；
- 除非是在指令内部创建的作用域（即孤立作用域），所有的作用域都是通过原型继承而来的，这也是它们可以访问父级作用域的原因；
- ng-controller 指令为本 HTML 页面的 DOM 创建一个新的 $scope 对象，并将它嵌套在 $rootScope 中；
- 通过 $scope 可以传播事件，即可向父 scope 传播也可以向子 scope 传播；
- 默认情况下，AngularJS 在当前作用域中无法找到某个属性时，便会在父级作用域中查找，如果找不到，则会顺着父级作用域一直向上寻找，直到抵达 $rootScope 为止，如果这时也找不到，则继续运行，但是视图无法更新；
- 可以通过 angular.element($0).scope() 获取和调试 $scope 对象；

### 1.2. $scope 生命周期

1)	creation

用户请求应用起始页，angular 被加载，查找 ng-app，Angular 遍历模板，查找其它指令；
在创建控制器或指令时，AngularJS 会用 $injector 创建一个新的作用域，并在这个新建的控制器或指令运行时将作用域传递进去；

2)	link

当 AngluarJS 开始运行时，所有的 $scope 对象都会附加或者链接到视图中。所有创建 $scope 对象的函数也会将自身附加到视图中。这些作用域将会注册当 Angular 应用上下文中发生变化时需要运行的 $watch 函数，Angular 通过这些函数获知何时启动事件循环；

3)	update

当事件循环运行时，它通常执行在顶层 $scope 对象上（也就是 $rootScope）, 每个子作用域都执行自己的脏值检测。每个监控函数都会检测变化，如果检测到任意变化，$scope 对象就会触发指定的回调函数；

4)	scope destruction

当 child scope 不再是必须的时候，child scope 的产⽣生者有责任通过 scope.$destroy() API 销毁它们（child scope）。这将会停止 $digest 的调用传播传播到 child scope 中，让被 child scope model 使用的内存可以被 gc 回收；

### 1.3. 属性和工具方法
https://angularjs.shujuwajue.com/apply.html
https://angularjs.shujuwajue.com/watchfang_fa.html
http://angularjs.cn/A0a6

$scope 实际上是一个 POJO，即普通 JavaScript 对象，提供了一些属性和工具方法（如 $apply、$watch 等），是 MVC 和双向数据绑定实现的基础；

#### 1.3.1. 属性
- $id：唯一标识一个 $scope 对象的值，一般是一个数字；
- $parent：当控制器发生嵌套或使用指令时，会使得作用域发生嵌套，$parent 指向父作用域；
- $root：指向根作用域 $rootScope 对象；

#### 1.3.2. 工具方法

- $watch：监控 $scope 中的属性，值发生变化时调用相应的回调函数；
	例：
	```javascript
	$scope.$watch(‘test’, function (newVal, oldVal, scope) {
	});
	```
	其中，test 为 $scope 对象中的某个属性的属性名，function 为该属性值发生改变时的回调函数，参数分别为改变后的值，改变前的值，$scope 对象；
	注意：过多的 $watch 函数会影响性能，因此应少用 $watch 函数；

- $on、$broadcast、$emit：定义自定义事件，通过 $broadcast 向下进行广播，传给子作用域，通过 $emit 向上进行广播，传给父级作用域，$on 用于接收事件；
- $digest：当双向数据绑定失效时，可调用此函数手动同步视图与 controller 的数据，一般用于在指令中使用原生方法（document.getXXX 等方法）操作 DOM 时；若在正常逻辑中调用会报错；

## 2. 双向数据绑定

### 2.1. ng-moddel

### 2.2. ng-bind

- ng-model 和 ng-bind 的区别：
	- ng-bind 是从 $scope -> view 的单向绑定，也就是说 ng-bind 是相当于{{object.xxx}}，是用于展示数据的，当页面 angularjs 未加载完毕时，显示为空；
	- ng-model 是 $scope <-> view 的双向绑定，当页面 angularjs 未加载完毕时，显示“{{xxxxxx}}”；

## 3. MVC
MVC 工作模式：

![image](http://img.cdn.firejq.com/jpg/2017/9/25/efa776ab6cdbc18a96c8c90995ca1160.jpg)

### 3.1. Controller

- Angular 中控制器的作用是增强视图，Angular 中的控制器是一个函数，用来向视图的作用域中添加额外的功能，我们用它来给作用域对象设置初始状态，并添加自定义行为；
- AngularJS 不再让用户操作 DOM 和事件，用于只需要关注 JS 与 HTML 的关系，并且实现 HTML 与 JS 的隔离，通过 angularJS 来实现粘合，我们可以把 angularJS 看出 html 与 JS 之间的胶水；
- 当我们在模块中创建控制器时，AngularJS 会生成并传递一个新的 $scope 给这个控制器；
- 设计良好的应用会将复杂的逻辑放到指令和服务中。通过使用指令和服务，我们可以将控制器重构成一个轻量且更易维护的形式。

注意：
- controller 中应该是业务逻辑；
- 不要复用 controller，因为每个 controller 只负责渲染一块视图；
- 不要在 controller 中操作 DOM，应通过指令的 link 函数实现；
- 不要在 controller 中进行数据格式化，应通过表单控件实现；
- 不要在 controller 中进行数据过滤，应通过过滤器 $filter 实现；
- controller 之间不应相互调用，其通信应通过事件广播 /$rootScope/ 服务进行；
多个控制器间通信的方式：https://segmentfault.com/a/1190000009167725

### 3.2. Model

![image](http://img.cdn.firejq.com/jpg/2017/9/25/177ec936519d50809027b31fbbd79cf2.jpg)

加载 angular 的 js 文件后，angularJS 启动并找到 ng-app 所在的标签，接管该标签内部包含 angular 元素的 JavaScript 解析；

创建数据模型 greeting.text，并将 input 标签的值，与该模型进行双向绑定；

### 3.3. View

![image](http://img.cdn.firejq.com/jpg/2017/9/25/50eed8d8a0f65fd7d06c90d534b2d2c0.jpg)

#### 3.3.1. 表达式

{{ }}符号将一个变量绑定到 $scope 上。当用 $watch 进行监听时，AngularJS 会对表达式或函数进行运算；

表达式和 eval 非常类似，但是由 AngularJS 来处理，它们有以下不同的特性：
- 所有的表达式都在其所属的作用域内部执行，并有访问本地 $scope 的权限；
- 如果表达式发送了 TypeError 和 ReferenceErroe 并不会抛出异常；
- 不允许使用任何流程控制比如 if；
- 可以接受过滤器和过滤器链；

## 4. 模块

### 4.1. 概述

大部分应用都有⼀一个主方法 (main) 用来实例化、组织、启动应用，AngularJS 应用没有主方法，而是使用模块来声明应用应该如何启动；

模块是组织业务的一个封装，在一个模块当中定义多个服务，当引入了一个模块的时候，就可以使用这个模块提供的一种或多种服务了，另外，服务只是模块提供的多种机制中的一种，其它的还有指令（directive ），过滤器（ filter ），及其它配置信息；

模块允许通过声明的方式来描述应用中的依赖关系，以及如何进行组装和启动；

AngularJS 本身的一个默认模块叫做 ng ，它提供了 $http ， $scope 等等服务；

优点：
- 在单元测试是不需要加载全部模块的，因此这种方式有助于写单元测试；
- 可以在特定情况的测试中增加额外的模块，这些模块能更改配置，能帮助进行端对端的测试；
- 第三方代码可以作为可复用的 module 打包到 angular 中；
- 模块可以以任何先后或者并行的顺序加载（因为模块的执行本身是延迟的）；

### 4.2. 模块定义

```javascript
angular.module(name[, requires], configFn);
```
这个方法能够接受两个参数，第一个是模块的名称，第二个是依赖列表，也就是可以被注入到模块中的对象列表；

![image](http://img.cdn.firejq.com/jpg/2017/9/25/eb9c278d96bf6a6e3221ce88aa175fdc.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/9/25/3c743f7a0ad3a6205eb479dbb3c5c527.jpg)

使用 angular.moudle() 方法创建一个名为 HelloAngular 的模块；
使用模块的方法 controller() 创建一个名为 helloNgCtrl 的控制器；

### 4.3. 模块加载

#### 4.3.1. config

配置块。在模板的加载阶段，AngularJS 会在提供者注册和配置的过程中对模块进行配置。在整个 AngularJS 工作流中，这个阶段是**唯一能够在应用启动前进行修改的部分**。

```javascript
myModule.config(['$locationProvider', function($locationProvider) {
  // Configure existing providers
  $locationProvider.hashPrefix('!');
}]);

```

如果在定义模块时没有写 config 函数，AngularJS 会在编译时执行；
例：
```javascript
angular.module("myApp",[])
  .factory('myFactory',function(){
    var service = {};
    return service;
})
  .directive('myDirective',function(){
    return {
      template:'<button>click me</buttom>'
    }
})
```
AngularJS 会在编译时执行这些辅助函数，实际上效果等同于下面的代码：
```javascript
angular.module("myApp",[])
  .config(function($provide,$compileProvider){
    $provide.factory("myFactory",function(){
      var service = {};
      return service;
    });
    $compileProvider.directive("myDirective",function(){
      return {
        template:"<button>Click me</button>"
      };
    });
});
```
AngularJS 会以这些函数书写和注册的顺序来执行它们，也就是说我们无法注入一个尚未注册的 provider；

#### 4.3.2. run

运行块。和配置块不同，运行块在注入器创建之后被执行，它是所有 AngularJS 应用中第一个被执行的方法。

运行块通常用来注册全局的事件监听器。例如，我们会在。run() 块中设置路由事件的监听器以及过滤未经授权的请求

实例：
在每次路由发送变化时，都执行一个函数来验证用户的权限，放置这个功能唯一合理的地方就是 run 方法：
```javascript
angular.module("myApp",[])
  .run(function($rootScope,AuthService){
    $rootScope.$on("$routeChangeStart",function(evt,next,current){
      // 如果用户未登录
      if(!AuthService,userLoggedIn()){
        if(next.templateUrl==="login.html"){
          // 已经转向登陆路由因此无需重定向
        } else {
          $location.path("/login");
        }
      }
    });
});
```

### 4.4. 项目模块化

![image](http://img.cdn.firejq.com/jpg/2017/9/25/b093b727fe23df04a2fba230b23f3e21.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/9/25/4873703d6e8a9b99bd346853ff777221.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/9/25/67e523d9d8f5999892424200cafe4040.jpg)

## 5. 服务

### 5.1. 概述

- 控制器只会在需要时被实例化，不再需要就会被销毁。这意味着每次切换路由或重新加载视图时，当前的控制器会被 AngularJS 清除掉。而服务提供了一种能在应用的整个生命周期内保持数据的办法，它能够在控制器之间进行通信，并且能保证数据的一致性。 **服务是一个单例对象，在每个应用中只会被实例化一次（被 $injector 实例化)，并且是延迟加载的，服务提供了把与特定功能相关联的方法集中在一起的接口**；
- 服务本身是一个任意的对象；
- Angular 是用 $provider 对象来实现自动依赖注入机制，注入机制通过调用一个 provider 的 $get() 方法，把得到的对象作为参数进行相关调用，$provider 提供很多很定义服务的方法，这些简便的方法还直接被 module 所引用；

### 5.2. 内置服务

#### 5.2.1. $location
Angular 中使用内置的 $location 服务来监听、操作 URL，功能包括了获取、监听、改变地址栏的 URL；
与 URL 实现双向数据绑定（地址栏变动、前进后退或者点击页面的链接均会触发）；
将 URL 对象封装成了一套方法（protocol、host、port、path、search 和 hash），对 window.location 对象的 API 进行优雅封装，支持 jQuery 风格的链式写法，集成了 HTML5 的 history API；

工具方法：
##### 5.2.1.1. path()

用来获取页面当前的路径。
```javascript
$location.path();// 返回当前路径
```
修改当前路径并跳转到应用中的另一个 URL：
```javascript
$location.path('/');// 把路径修改为'/'路由
```
注意：$location 无法使整个页面重新加载。如果改变 URL 后希望重新加载页面，请使用 `$window.location.href`。

##### 5.2.1.2. replace()

跳转后不能点击后退按钮。
```javascript
$location.path('/home');
$location.replace();
// 或者
$location.path('/home').replace();
```

##### 5.2.1.3. absUrl()

获取编码后的完整 URL。

#### 5.2.2. $http
https://code.angularjs.org/1.6.4/docs/api/ng/service/$http

$http 是 angular 中的一个核心服务，利用浏览器的 xmlhttprequest 对象或者 JSONP 与远程 HTTP 服务器进行交互；

在 controller 中可通过与 $scope 相同的方式注入获取 $http 对象：
```javascript
function controller($scope,$http){
}；
```

##### 5.2.2.1. General usage
例：
```javascript
$http({
	method: 'GET',
	url: '/someUrl'
}).then(function successCallback(response) {
		// 请求成功执行代码
	}, function errorCallback(response) {
		// 请求失败执行代码
});
```
注：v1.5 之后 $http 的 success 和 error 方法已废弃，使用 then 方法替代。

- $http 服务的参数 config 作为一个 json 对象，接受的配置项有：
	- method 方法；
	- url 路径；
	- params：GET 请求的参数，如 data: {name:"12346",pwd:"123"}；
	- data：post 请求的参数，如 data: {name:"12346",pwd:"123"}；
	- headers: 头信息对象，有缺省定义，如果自定义则会覆盖默认定义，如 headers: {'Content-Type': undefined}；
	- transformRequest 请求预处理函数，有缺省定义，如果自定义则会覆盖默认定义；
	- transformResponse 响应预处理函数，有缺省定义，如果自定义则会覆盖默认定义；
	- cache 缓存；
	- timeout 超时毫秒，超时的请求会被取消；
	- withCredentials 跨域安全策略的属性；

- response 对象包含的属性：
	- data – {string|Object} – The response body transformed with the transform functions.
	- status – {number} – HTTP status code of the response.
	- headers – {function([headerName])} – Header getter function.
	- config – {Object} – The configuration object that was used to generate the request.
	- statusText – {string} – HTTP status text of the response.

##### 5.2.2.2. Shortcut

对于几个标准的 HTTP 方法，有对应的简写方法：
```javascript
$http.delete(url, config)
$http.get(url, config)
$http.head(url, config)
$http.jsonp(url, config)
$http.post(url, data, config)
$http.put(url, data, config)
```
其中的 JSONP 方法，在实现上会在页面中添加一个 script 标签，然后放出一个 GET 请求。你自己定义的，匿名回调函数，会被 ng 自已给一个全局变量。在定义请求，作为 GET 参数，你可以使用 JSON_CALLBACK 这个字符串来暂时代替回调函数名，之后 ng 会为你替换成真正的函数名

例：
```javascript
$http.get('/someUrl', config).then(successCallback, errorCallback);
$http.post('/someUrl', data, config).then(successCallback, errorCallback);
```
参数说明：

1)	config：一个 JSON 对象，其中主要包含该请求的 url、data、method 等，如{url:"login.do",method:"post",data:{name:"12346",pwd:"123"}}；

2)	url {String} 请求的 URL 地址；

3)	params {key,value} 请求参数，将在 URL 上被拼接成？key=value；

4)	data {key,value} 数据，将被放入请求内发送至服务器；

##### 5.2.2.3. 文件上传

http://blog.csdn.net/u013360850/article/details/57421535

HTML
```html
<form>
	<input id="fileUpload" type="file"/>
	<input ng-model="username" />
	<input ng-model="password" />
	<button ng-click="ok()">上传</button>
</form>
```
JavaScript
```javascript
$scope.ok = function () {
		var form = new FormData();

		form.append('userfile', document.getElementById("fileUpload").files[0]);
		form.append('username', $scope.username);
		form.append('password', $scope.password);

		$http({
				method: 'POST',
				url: '/addUser',
				data: form,
				headers: {
					'Content-Type': undefined
				},
				transformRequest: angular.identity
		}).then(function (resp) {
				console.log('operation success');

		}, function (resp) {
				console.log('operation fail');

		});
};
```
注：
- AngularJS 默认的'Content-Type'是 application/json ，通过设置'Content-Type': undefined，这样浏览器不仅帮我们把 Content-Type 设置为 multipart/form-data，还填充上当前的 boundary，如果手动设置为：'Content-Type': multipart/form-data，后台会抛出异常：the request was rejected because no multipart boundary was found，而 boundary 是随机生成的字符串，用来分隔文本的开始和结束
- 通过设置 transformRequest: angular.identity ，anjularjs transformRequest function 将序列化我们的 formdata object，也可以不添加

#### 5.2.3. $cookies

https://docs.angularjs.org/api/ngCookies/service/$cookies

$cookies 服务能够很好的发挥 HTML5 cookies，当 HTML5 API 可用时浏览器会选择使用 HTML5 提供的 API，如果不可用则默认选择 document.cookies。无论那种方式，你都可以选择使用相同的 API 来进行工作。

![image](http://img.cdn.firejq.com/jpg/2017/9/25/dab85c8deb94c707590f628e085c77d8.jpg)

方法：
- get(key)：Returns the raw cookie value of given cookie key；
- getObject(key)：Returns the deserialized cookie value of given cookie key；
- getAll()：Returns a key value object with all the cookies；
- put(key, value, [options])：Sets a value for given cookie key；
- putObject(key, value, [options])：Serializes and sets a value for given cookie key；
- remove(key, [options])：Remove given cookie；

例：
```javascript
angular.module('cookiesExample', ['ngCookies'])
.controller('ExampleController', ['$cookies', function($cookies) {
  // Retrieving a cookie
  var favoriteCookie = $cookies.get('myFavorite');
  // Setting a cookie
  $cookies.put('myFavorite', 'oatmeal');
}]);
```
例：用户登录，记住密码的 cookie 有效期 7 天
```javascript
  var cookieInfo= {};
  cookieInfo.username = $scope.username;
  cookieInfo.password = $scope.password;
  var expireDate = new Date();
  expireDate.setDate(expireDate.getDate() + 7);// 设置 cookie 保存 7 天
  $cookies.putObject("user", cookieDate, {'expires': expireDate});
```
#### 5.2.4. $log

ng 提供 $log 这个服务用于向终端输出相关信息：
```
error()
info()
log()
warn()
```

#### 5.2.5. $sce

> $sce is a service that provides Strict Contextual Escaping services to AngularJS. SCE assists in writing code in way that (a) is secure by default and (b) makes auditing for security vulnerabilities such as XSS, clickjacking, etc. a lot easier.

用例：在 iframe 中使用动态 src，直接使用 url 会报错：Can't Interpolate Error With Iframe
```javascript
$scope.url = $sce.trustAsResourceUrl($scope.global.url + 'web?Id=' + $state.params.id);
```
HTML
```html
<iframe class="pt-50" ng-src="{{url}}" frameborder="0">
</iframe>
```

参考：https://solidfoundationwebdev.com/blog/posts/can-t-interpolate-error-with-iframe-and-ng-src-in-angularjs-1-x

### 5.3. 服务定义

- $provider.factory
factory 方法直接把一个函数当成是一个对象的 $get() 方法；
返回的内容可以是任何类型；
例：
```javascript
var myApp = angular.module('myApp',[],function($provide){
$provide.factory('CustomFactory',function(){
        return [1,2,3,4,5,6,7];
});
```

- $provider.service
和 factory 类似，但返回的内容必须是对象；
例：
```javascript
var myApp = angular.module('myApp',[],function($provide){
    $provide.service('CustomService2',function(){
        return {
                message : 'CustomService Message2'
            }
});
```

- $provider.provider
例：
```javascript
var myApp = angular.module('myApp',[],function($provide){
    // 自定义服务
    $provide.provider('CustomService',function(){

        this.$get = function(){
            return {
                message : 'CustomService Message'
            }
        }
});
```
**注：factory、service 实际上都是 provider；**

## 6. 依赖注入
通常一个对象有三种方法可以获得对其依赖的控制权：
- 在内部创建依赖：这种方法的弊端就是不方便以后的维护，使隔离变的异常困难。
- 通过全局变量进行引用：这种方法的弊端就是污染了全局作用域，当代码量达到了一定程度后，容易出现问题。
- 在需要的地方进行参数传递（依赖注入）：这种方法不仅对测试很有用，而且不会污染全局变量，是很好的设计模式。AngularJS 用的就是这种方法。

每一个 AngularJS 应用都有一个注入器 (injector) 用来处理依赖的创建。注入器是一个负责查找和创建依赖的服务定位器。

内部机制：https://benweizhu.github.io/blog/2015/01/04/yes-angularjs-6/

### 6.1. 隐式依赖注入

隐式依赖注入，即通过函数的参数进行推断式注入声明；

例：
```javascript
var myApp = angular.module('myApp', [], ['$filterProvider', '$provide', '$controllerProvider', function (a, b, c) {
    console.log(a, b, c);
}])

.factory('CustomService', ['$window', function (a) {
    console.log(a);
}])

// 隐式依赖注入
.controller('firstController', function ($scope, CustomService) {
    console.log(CustomService);
})
```

如果没有明确的声明，AngularJS 会假定参数名称就是依赖的名称。因此，它会在内部调用函数对象的 toString() 方法，分析并提取出函数的参数列表，然后通过 $injector 将这些参数注入进对象实例；

**这个方法只适合未经过压缩和混淆的代码，因为 AngularJS 需要原始未经压缩的参数列表来进行解析。**

### 6.2. 显示依赖注入

AngularJS 提供了显式地方法来明确定义一个函数在被调用时需要用到的依赖关系。

例：
```javascript
var aControllerFactory = function aController($scope,greeter){
  // 控制器
};
aControllerFactory.$inject = ['$scope','greeter'];

angular.module('myApp',[])
  .controller('MyController',aControllerFactory)
  .factory('greeter',greeterService);
// 获取注入器并创建一个新的作用域
var injector = angular.injector(['ng','myApp']),
   controller = angular.get('$controller'),
   rootScope = angular.get('$rootScope'),
   newScope = rootScope.$new();
// 调用控制器
controller('MyController',{$scope:newScope});
```
通过这种方法声明依赖，即使在源代码被压缩，参数名称发生改变的情况下依然可以正常工作；

对于这种声明方式来讲，参数的顺序是十分重要的，因为 $inject 数组元素的顺序必须和注入的参数的顺序一一对应。

### 6.3. 行内依赖注入

AngularJS 提供的行内注入方法实际上是一种语法糖；

例：
```javascript
angular.module("MyApp", [])
.controller("MyController", ["$scope", "$timeout", function (s, t) {
    var updateTime = function () {
        s.clock = {
            time: new Date()
        };
        t(function () {
            s.clock.time = new Date();
            updateTime();
        }, 1000);
    }
    updateTime();
}]);
```

行内依赖注入与前面的提到的通过 $inject 属性进行声明的原理是一样的，但是允许我们在函数定义的时候从行内将参数传入，这种方法方便，简单，而且避免了在定义的过程中使用临时变量；

需要注意的是，行内声明的方式允许我们直接传入一个参数数组，而不是一个函数。数组的元素是字符串，它们代表的是可以被注入到对象中的依赖名字，最后一个参数就是依赖注入的目标函数对象本身。

### 6.4. 应用

#### 6.4.1. 模块依赖注入

```javascript
// 采用行内依赖注入构造 module
var myApp = angular.module('myApp', [
	‘ngRoute’, ‘ngAnimate’, ‘bookStoreCtrl’, ‘bookStoreFilter’, ‘bookStoreService’, ‘bookStoreDirective’
]);
// 其中 ng 开头的是 angularJS 默认提供的模块，其它是自定义的第三方模块
```
**注：这里所依赖的模块必须在 html 文件中导入，若没有导入 html 文件却依赖了模块，会导致整个 angularjs 的 module 初始化加载失败（当页面的 ng 指令都失效时很有可能是这个原因导致的）；**

#### 6.4.2. 控制器依赖注入

```javascript
// 采用行内依赖注入构造 controller
angular.module('myApp',[])
.controller('DemoController',['$rootScope','$scope','$http',function($rootScope,$scope,$http){
	...
}])
```

## 7. 路由
<!-- TODO 排版错乱 -->
除了使用 ng-include 指令在视图中引用多个模板外，更好的做法是将视图分解成布局和模板视图，并且根据用户当前访问的 URL 来展示对应的视图。 我们可以将模板分解到视图中，并在布局模板内进行组装。我们可以在 $route 服务的提供者 $routeProvider 中通过声明路由来实现这个功能。

- 为什么需要前端路由的重要原因？
	- AJAX 请求不会在浏览器留下历史记录，用户体验差；
	- 用户无法直接通过 URL 进入应用中的某个指定页面（保存为书签 / 连接分享）；
	- Ajax 的使用极不利于 SEO；

- 前端路由实现的基本方法：
	- 通过锚点 # 实现；
	- 通过 HTML5 的 history API 实现；

### 7.1. ngRoute

在 HTML 中引入 angular-route.js 文件
```html
<script src="https://cdn.bootcss.com/angular.js/1.6.4/angular-route.min.js"></script>
```
并把 ngRoute 模块在我们的应用中当做依赖加载进来：
```javascript
angular.module("app",["ngRoute"]);
```
- 布局模板 ng-view
```html
<header>
  <h1>Header</h1>
</header>
<div class="content">
  	<div ng-view></div>
</div>
<footer>
  <h5>Footer</h5>
</footer>
```
ng-view 是由 ngRoute 模块提供的一个特殊指令，它的独特作用是将 HTML 中给 $route 对应的视图内容占位。它会创建自己的作用域并将模板嵌套在内部。通过将 ng-view 指令和路由组合到一起，我们可以精确地指定当前路由所对应的模板在 DOM 中的渲染位置。

- 使用 $routeProvider 配置路由
可以使用服务 $routeProvider 的 when() 和 otherwise() 两个方法在模块的 config 函数中定义应用的路由：
例：
  ```javascript
  angular.module("myApp",[])
    .config(["$routeProvider", function($routeProvider){
      // 在这里定义路由
      $routeProvider
        .when('/',{
          templateUrl:'views/home.html',
          controller:'HomeController'// 指定 controller 后不再需要使用 ng-controller 进行 HTML 与 controller 的绑定
        })
        .when('/',{
          templateUrl:'views/login.html',
          controller:'LoginController'
        })
        .when('/',{
          templateUrl:'views/dashboard.html',
          controller:'DashboardController',
          resolve:{
            user:function(SessionService){
              return SessionService.getCurrentUser();
            }
          }
        })
        .otherwise({
          redirectTo:'/'
        });

    $locationProvider.html5Mode(true);
    }]);
  ```

	- when
	when 方法接受两个参数：
		- 路由路径，这个路径会和 $location.path 进行匹配，$location.path 也就是当前 URL 的路径；
		- config 配置对象，决定了当第一个参数中的路由能够匹配时具体做什么：
		以下是配置对象的可用属性：
	 		- controller：如果配置对象中设置了 controller 属性，那么这个指定的控制器会与路由所创建的新作用域关联在一起。如果参数值是字符型，会在模块中所有注册过的控制器中查找对应的内容，然后与路由关联在一起。如果参数值是函数型，这个函数会作为模板中 DOM 元素的控制器并与模板进行关联；
      		例：
				```javascript
				controller:'MyController'
				// 或者
				controller:function($scope){}
				```
		 	- template：AngularJS 会将配置对象中的 HTML 模板渲染到对应的具有 ng-view 指令的 DOM 元素中；
		 	- templateUrl：应用会根据该属性指向的路径读取视图，然后渲染到具有 ng-view 指令元素中；
		 	- resolve：如果设置了该属性，AngularJS 会将列表中的元素都注入到控制器中的。如果这些依赖是 promise 对象，它们在控制器加载以及 $routeChangeSuccess 被触发之前，会被 resolve 并设置成一个值；
			例：resolve 会发送一个 $http 请求，并将 data 的值替换为返回结果的值。列表中的键 data 会被注入到控制器中，所以在控制器中可以使用它
				```javascript
				resolve:{
				'data':["$http",function($http){
					return $http.get('/api').then(
					function success(resp) {return response.data;},
					function error(reason) {return false;}
					);
				}];
				}
				```

		 	- redirectTo：路径替换；
		 	- reloadOnSearch：如果设置为 true（默认），当 $location.search() 发生变化时会重新加载路由。如果设置为 false，那么当 URL 中的查询串部分发生变化时就不会重新加载路由。这个小窍门对路由嵌套和原地分页等需求非常有用；

	- html5Mode
	https://www.oschina.net/translate/pretty-urls-in-angularjs-removing-the-hashtag
	Angular 默认通过操作锚点来进行客户端路由，即服务器端路由和客户端路由的 URL 以 # 分隔，因此通过路由的 URL 中会出现一个 # 号，例如 /foo/bar#/users/alice；
	若要将 # 号从 URL 中去除，可以设置 Angular 通过 HTML5 的 history API 进行客户端路由：使用 $locationProvider 模块，并将 html5Mode 设置为 true，并在 html 中，设置 base，比如： `<base rel="nofollow" href="/Xxxxwebapp/">`，这样会使 URL 变成 /foo/bar/users/alice（需要浏览器支持 HTML5，因为此时 Angular 通过 pushState 来进行路由）；此时服务端对所有的客户端路由的 URL 都需要返回首页（/foo/bar）视图，再交给 Angular 路由到 /foo/bar/users/alice 对应的视图。

	- $locationProvider
	http://blog.csdn.net/u014291497/article/details/54427103
	1.6.x 中关于前缀 /#!/ 的改动

- 路由参数
	```javascript
	app.controller('UserCtrl', function($scope, $routeParams) {
		$scope.params = $routeParams;
	});
	```
	user.html
	```html
	<h1>User Page</h1>
	<span ng-bind="params.userName"></span>
	```
	我们点击首页的 /users/alice 时，将会载入 user.html，span 的值为 alice。$routeParams 提供了当前的路由参数；
	例：
	```javascript
	// Given:
	// URL: http://server.com/index.html#/Chapter/1/Section/2?search=moby
	// Route: /Chapter/:chapterId/Section/:sectionId
	// Then
	$routeParams ==> {chapterId:'1', sectionId:'2', search:'moby'}
	```

- 路由事件
$route 服务在路由过程中的每个阶段都会触发不同的事件，可以为这些不同的路由事件设置监听器并作出响应。我们需要给路由设置事件监听器，用 $rootScope 来监听这些事件；
AngularJS 在路由变化之前会广播 $routeChangeStart 事件。路由服务会开始加载路由变化所需要的所有依赖，并且模板和 resolve 键中的 promise 也会被 resolve；

### 7.2. uiRoute

UI-Router 是 Angular-UI 提供的客户端路由框架，它解决了原生的 ng-route 的很多不足：
- 视图不能嵌套。这意味着 $scope 会发生不必要的重新载入。这也是我们在 Onboard 中引入 ui-route 的原因；
- 同一 URL 下不支持多个视图。这一需求也是常见的：我们希望导航栏用一个视图（和相应的控制器）、内容部分用另一个视图（和相应的控制器）；

#### 7.2.1. 使用 $stateProvider 和 $urlRouterProvider 配置路由

UI-Router 提出了 $state 的概念。一个 $state 是一个当前导航和 UI 的状态，每个 $state 需要绑定一个 URL Pattern。 在控制器和模板中，通过改变 $state 来进行 URL 的跳转和路由；

- 示例：uiRouter 基本使用
	router.js
	```javascript
	angular.module('app').config(['$stateProvider', '$urlRouterProvider',
		function ($stateProvider, $urlRouterProvider) {
		<!-- 指定路由 id 为 main -->
		$stateProvider.state('main', {
			url: '/main',
			templateUrl: 'view/main.html',
			controller: 'mainCtrl'
		});
		$urlRouterProvider.otherwise('main');

	}]);
	```
	在 html 中使用`<ui-view>`或`<div ui-view>`作为占位标签，index.html：
	```html
	<ui-view></ui-view>

	<!--AngularJS 1.6.6-->
	<script src="https://cdn.bootcss.com/angular.js/1.6.6/angular.min.js"></script>
	<script src="https://cdn.bootcss.com/angular-ui-router/1.0.3/angular-ui-router.min.js"></script>

	<!--customize script-->
	<script src="scripts/app.js"></script>
	<script src="scripts/config/router.js"></script>
	```

- stateProvider 中 URL 的写法
	- '/home'：只匹配'/home'；
	- '/user/:id'、'/user/{id}'：匹配'/user/1234'或'/user/'，不匹配'/user'；
	- '/message?before&after'：使用 URL Query 方式传参；
	- '/inbox/{inboxId:[0-9a-fA-F]{6}}'：使用正则表达式来匹配，限定 id 为 6 位 16 进制数字

#### 7.2.2. 页面跳转

##### 7.2.2.1. 使用 ui-sref 指令

点击该指令配置的标签后，会跳转到对应的路由页面，且在跳转时支持参数传递

例：
```javascript
<a ui-sref="main({id:1234})">
```

注意：可使用 ui-router 提供的指令`ui-sref-active`，指定标签被点击后的样式；
例：
```html
<ul>
	<li ui-sref-active="select" ui-sref="main">
		<i class="icon-document"></i>
		<span class="va-t">职位</span>
	</li>
	<li ui-sref-active="select" ui-sref="search">
		<i class="con-search"></i>
		<span class="va-t">搜索</span>
	</li>
	<li ui-sref-active="select" ui-sref="me">
		<i class="icon-my"></i>
		<span class="va-t">我</span>
	</li>
</ul>
```
定义 select 样式：
```less
.foot {
  li {
    &.select {
      background-color: @highlightBgColor;
      color: @highlightColor;
    }
  }
```

##### 7.2.2.2. 使用 $state 服务

在 ui-router 时，可以使用 $state 来完成页面跳转，而不是直接操作 URL。

例：
页面跳转
```javascript
$state.go('contacts');  // 指定 state 名，相当于跳转到 /contacts
$state.go('contacts.detail', {id: 42}, {location: 'replace'});  // 跳转时传递参数，相当于跳转到 /contacts/42，{location: 'replace'}表示跳转时消除当前路径，进行会跳时就不会跳回当前页面
```
获取参数
```javascript
$state.params.id
// 或
$stateParams.id
```

#### 7.2.3. 嵌套视图
https://www.cnblogs.com/xf110/p/6838343.html

http://www.jruif.com/2015-05/【Angular 系列】ui-router- 多层 state 与多层 view 的解构 /

嵌套视图指的是一个 ui-view 的一个状态下对应了一个 html，这个 html 里面又有一个 ui-view，视图嵌套通常对应着 $state 的嵌套；

注意：
- 定义嵌套状态的时候，不用计较状态的先后循序，甚至可以在不同的模块中，在程序执行的时候，ui-route 会在父状态定义了之后定义子状态；

- ui-view 可以配合 $state 进行任意层级的嵌套；

- 如果你只定义了一个单一的状态，就像 contacts.list，那么你必须定义一个叫做 contacts 的状态，否则你定义的 contacts.list 就不会执行；
- 当一个子 state 处于 active 状态时，其父级 state 也会隐含的变为 active，因为子状态将会导入自己的模板到他父级模板的 ui-view 中；

- 子 state 不定义 url 时，将使用与父级 state 相同的 url，若定义了 url，实际 url 为父级 url+ 子 url；

- 子 state 不定义 controller 时，将使用与父级相同的 controller；

例：
路由配置
```javascript
.state('myOrderDetail', {
	url: '/myOrderDetail/:orderId',
	templateUrl: 'view/myOrderDetail.html',
	controller: 'myOrderDetailCtrl'
})
.state('myOrderDetail.state', {
	url: '/state',
	templateUrl: 'view/myOrderDetail.state.html'
})
.state('myOrderDetail.detail', {
	url: '/detail',
	templateUrl: 'view/myOrderDetail.detail.html'
})
```
index.html
```html
<ui-view></ui-view>
```
myOrderDetail.html
```html
<h1>myOrderDetail</h1>
<div ui-sref="myOrderDetail.state">state</div>
<div ui-sref="myOrderDetail.detail">detail</div>
<ui-view></ui-view>
```
myOrderDetail.state.html
```html
<h2>myOrderDetail.state</h2>
```
myOrderDetail.detail.html
```html
<h2>myOrderDetail.detail</h2>
```
当访问 index.html 时，myOrderDetail.html 的内容会被渲染到 index.html 中的 ui-view 处（URL 为 /myOrderDetail/:orderId），点击跳转 div 后，myOrderDetail.state.html 和 myOrderDetail.detail.html 会被分别渲染到 myOrderDetail.html 中的 ui-view 处（URL 为 /myOrderDetail/:orderId/state 和 /myOrderDetail/:orderId/detail）；

<!-- TODO -->
嵌套视图采用特殊的语法标记：
![image](http://img.cdn.firejq.com/jpg/2017/9/25/9425e223da215424d219ab98a8a61070.jpg)

#### 7.2.4. $state 多视图
http://bubkoo.com/2014/01/01/angular/ui-router/guide/multiple-named-views/

多视图指的是一个 state 下包含了多个 ui-view，其中每个 ui-view 都有各自的模板和控制器；这一点也是 ng-route 所没有的， 给了前端路由极大的灵活性；

例：
index.html
```html
<body>
  <div ui-view="filters"></div>
  <div ui-view="tabledata"></div>
  <div ui-view="graph"></div>
</body>
```
这一个模板包含了三个命名的 ui-view，可以给它们分别设置模板和控制器：
```javascript
$stateProvider
  .state('report',{
    views: {
      'filters': {
        templateUrl: 'report-filters.html',
        controller: function($scope){ ... controller stuff just for filters view ... }
      },
      'tabledata': {
        templateUrl: 'report-table.html',
        controller: function($scope){ ... controller stuff just for tabledata view ... }
      },
      'graph': {
        templateUrl: 'report-graph.html',
        controller: function($scope){ ... controller stuff just for graph view ... }
      }
}
```

<!-- TODO -->
### 7.3. 预载入 Resolve
使用预载入功能，开发者可以预先载入一系列依赖或者数据，然后注入到控制器中。在 ngRoute 中 resolve 选项可以允许开发者在路由到达前载入数据保证（promises）。在使用这个选项时比使用 angular-route 有更大的自由度。

预载入选项需要一个对象，这个对象的 key 即要注入到控制器的依赖，这个对象的 value 为需要被载入的 factory 服务。

如果传入的时字符串，angular-route 会试图匹配已经注册的服务。如果传入的是函数，该函数将会被注入，并且该函数返回的值便是控制器的依赖之一。如果该函数返回一个数据保证（promise），这个数据保证将在控制器被实例化前被预先载入并且数据会被注入到控制器中。

```javascript
$stateProvider.state('home', {
	resolve: {
		// 这个函数的值会被直接返回，因为它不是数据保证
		person: function() {
			return {
				name: "Ari",
				email: "ari@fullstack.io"
			}
		},
		// 这个函数为数据保证，因此它将在控制器被实例化之前载入。
		currentDetails: function($http) {
			return $http({
				method: 'JSONP',
				url: '/current_details'
			});
		},
		// 前一个数据保证也可作为依赖注入到其他数据保证中！（这个非常实用）
		facebookId: function($http, currentDetails) {
			$http({
				method: 'GET',
				url: 'http://facebook.com/api/current_user',
				params: {
					email: currentDetails.data.emails[0]
				}
			})
		}
	},
	// 定义控制器
	controller: function($scope, person,
								currentDetails, facebookId) {
			$scope.person = person;
	}
})
```

### 7.4. 使用 $transition 监控路由变化

https://ui-router.github.io/ng1/docs/latest/classes/transition.transition-1.html

https://ui-router.github.io/ng1/docs/latest/interfaces/transition.hookmatchcriteria.html

### 7.5. onEnter

> onEnter(criteria: HookMatchCriteria, callback: TransitionStateHookFn, options?: HookRegOptions)

The HookMatchCriteria is used to determine which Transitions the hook should be invoked for. onEnter hooks generally specify { entering: 'somestate' }. To match all Transitions, use an empty criteria object {}.

例：
```javascript
angular.module('app').run(['$transitions', function ($transitions) {
	$transitions.onEnter({entering: 'me'}, function (transition, state) {
		var AuditService = trans.injector().get('AuditService');
  	AuditService.log("Entered " + state.name + " module while transitioning to " + transition.to().name);
	});
}]);
```

除此之外，还可以使用返回布尔值的 function 判断是否拦截路由：

https://stackoverflow.com/questions/22537311/angular-ui-router-login-authentication

```javascript
angular.module('app').run(['$transitions', '$cookies', '$state', function ($transitions, $cookies, $state) {
		$transitions.onEnter({
			to: function (state) {
				return state.name === 'me' || state.name === 'order';// 当 state 返回 true 时，invoke 回调下边的函数
			}
		}, function (transition, state) {
			if ($cookies.get('Token') !== '' && $cookies.get('Mobileno') !== '') {
				return $state.target('login');
			}
		})
```
注意：若要在回调函数中进行 state 重定向，直接执行`$state.go('xxx')`会报错 Transition Rejection：
![image](http://img.cdn.firejq.com/jpg/2017/10/28/1049ea009eefc654d564c266cd450d9e.jpg)

要解决此问题，要使用上例中的方法，即`return $state.target('xxx');`，使用 [TargetState](https://ui-router.github.io/ng1/docs/latest/classes/state.stateservice.html#target)。

![image](http://img.cdn.firejq.com/jpg/2017/10/28/5515e4ebeb63f37163de7f92d8783d67.jpg)

### 7.6. onExit

> onExit(criteria: HookMatchCriteria, callback: TransitionStateHookFn, options?: HookRegOptions)

The HookMatchCriteria is used to determine which Transitions the hook should be invoked for. onExit hooks generally specify { exiting: 'somestate' }. To match all Transitions, use an empty criteria object {}.

注：在 onExit 中若要使用返回布尔值的 function 判断是否拦截路由，应使用 from 而不是 to。

## 8. 指令系统

指令是 AngularJS 的灵魂、最重要的元素；

### 8.1. 生命周期
![image](http://img.cdn.firejq.com/jpg/2017/9/25/d31baf17e0e61ff3117bc42224bc42b8.jpg)

### 8.2. 指定定义

- 通过 AngularJS 的 directive() 方法，我们可以传入一个字符串和一个函数来注册一个新指令；
	- 字符串是这个指令的名字，指令名是**驼峰命名**风格的，如：指令名 appHead 对应指令 app-head；
	- 函数中应该返回一个对象，该对象为指令的具体配置；

例：

#### 8.2.1. restrict 匹配模式

restrict 匹配模式设置告诉 AngularJS 在编译 HTML 时用哪种声明格式来匹配指令定义：
![image](http://img.cdn.firejq.com/jpg/2017/9/25/dee59c3124f6cf458b4786f070522bed.jpg)

注意：**推荐使用元素和属性模式，即将 restrict 属性设置为“AE“**；

例：
```javascript
<my-directive></my-directive>
<script>
angular.module("app",[])
  .directive('myDirective',function(){
    return {
      restrict:'E',
      template:'<a href="http://www.baidu.com">点击我去百度</a>'
    };
  });
</script>
```

#### 8.2.2. replace

通过浏览器工具可以看到浏览器渲染视图后使用的是我们自定义的标签，这样不利于 SEO：
![image](http://img.cdn.firejq.com/jpg/2017/9/25/744762086084973c495384985d9b6180.jpg)

我们可以通过设置 replace 属性为 true，使得指令加载模板后，将自定义标签从 html 中剔除以保证 SEO；

```javascript
angular.module("app",[])
  .directive('myDirective',function(){
    return {
      restrict:'E',
      replace:true,
      template:'<a href="http://www.baidu.com">点击我去百度</a>'
    };
});
```
![image](http://img.cdn.firejq.com/jpg/2017/9/25/d31b65b0aac0f5fc022f5941077f0938.jpg)

#### 8.2.3. templateUrl

templateUrl 为指令要加载的模板位置；

注意：要被指令加载的模板只能有一个根元素，不能有任何兄弟节点；

#### 8.2.4. template

temple 为指令要加载的模板 HTML 代码；

#### 8.2.5. scope 作用域

每当一个指令被创建的时候，都会有这样一个选择，是继承自己的父作用域（一般是外部的 Controller 提供的作用域或者根作用域（$rootScope）），还是创建一个新的自己的作用域，AngularJS 为我们指令的 scope 参数提供了三种选择，分别是：false,true,{}；默认情况下是 false。

- scope = false
	当我们将 scope 设置为 false 的时候，我们创建的指令和父作用域（其实是同一个作用域）共享同一个 model 模型，所以在指令中修改模型数据，它会反映到父作用域的模型中。在这种情况下，在指令模板中可以直接使用父作用域中的变量，函数。

- scope = true
	当我们将 scope 设置为 true 的时候，我们就新创建了一个作用域，只不过这个作用域是继承了我们的父作用域；可以这样理解：我们新创建的作用域是一个新的作用域，只不过在初始化的时候，用了父作用域的属性和方法去填充我们这个新的作用域。它和父作用域不是同一个作用域。

- scope = {}
	当我们将 scope 设置为{}时，意味着我们创建的一个新的与父作用域隔离的新的作用域，这使我们在不知道外部环境的情况下，就可以正常工作，不依赖外部环境，大大降低指令与控制器的耦合。
	我们使用了隔离的作用域，不代表我们不可以使用父作用域的属性和方法，我们可以通过向 scope 的{}中传入特殊的前缀标识符（prefix）“@,&,=“，引用应用指令的元素的属性，来进行数据的绑定。
	- 前缀标识符：
		- @
		这是一个单项绑定的前缀标识符
		使用方法：在元素中使用属性，如 `<div my-directive my-name="{{name}}"></div>`，注意，属性的名字要用 - 将两个单词连接，因为是数据的单项绑定所以要通过使用 `{{}}`来绑定数据。
		- =
		这是一个双向数据绑定前缀标识符
		使用方法：在元素中使用属性，如 `<div my-directive age="age"></div>`, 注意，数据的双向绑定要通过 = 前缀标识符实现，所以不可以使用 `{{}}`。
		- &
		这是一个绑定函数方法的前缀标识符
		使用方法：在元素中使用属性，如 `<div my-directive change-my-age="changeAge()"></div>`，注意，属性的名字要用 - 将多个单词连接。
	- 注意：在新创建指令的作用域对象中，使用属性的名字进行绑定时，要使用**驼峰命名**标准，比如下面的代码：
		例：
		```javascript
		angular.module("MyApp", [])
			.controller("MyController", function ($scope) {
				$scope.name = "dreamapple";
				$scope.age = 20;
				$scope.changeAge = function(){
						$scope.age = 0;
				}
			})
			.directive("myDirective", function () {
				var obj = {
					restrict: "AE",
					scope: {
							name: '@myName',
							age: '=',
							changeAge: '&changeMyAge'
					},
					replace: true,
					template: "<div class='my-directive'>" +
							"<h3>下面部分是我们创建的指令生成的</h3>" +
							"我的名字是：<span ng-bind='name'></span><br/>" +
							"我的年龄是：<span ng-bind='age'></span><br/>" +
							"在这里修改名字：<input type='text' ng-model='name'><br/>" +
							"<button ng-click='changeAge()'>修改年龄</button>" +
							" </div>"
				}
			return obj;
		});
		```
		html 代码
		```html
		<div ng-app="MyApp">
				<div class="container" ng-controller="MyController">
						<div class="my-info">我的名字是：<span ng-bind="name"></span>
								<br/>我的年龄是：<span ng-bind="age"></span>
								<br />
						</div>
						<div class="my-directive" my-directive my-name="{{name}}" age="age"  change-my-age="changeAge()"></div>
				</div>
		</div>
		```

#### 8.2.6. controller、compile、link
http://hudeyong926.iteye.com/blog/2073488

https://checkcheckzz.gitbooks.io/angularjs-learning-notes/content/chapter18/18-8.html

https://xgfe.github.io/2016/08/12/y8n/angular-directive-attributes/
<!-- TODO -->

### 8.3. 内置指令

AngularJS 提供了一系列的内置指令，其中一些指令重载了元素的 HTML 元素，比如 form 和 a 标签。另一些则是 ng 为前缀；

这里有一个最佳实践，就是 ng 开头的都是 AngularJS 提供的内置指令，因此不要把自己开发的指令也以这个前缀命名；

#### 8.3.1. 布尔类型指令

AngularJS 提供了一组带有 ng- 前缀版本的布尔属性，通过运算表达式的值来决定在目标元素上是插入还是移除对应的属性。

- ng-disabled

- ng-readonly

- ng-checked
	例：
	```html
	<!-- 如果 checkbox 选中，则 checkProperty 为 true，初始为选中状态 -->
			<label>checkProperty = {{ checkProperty }}</label>
			<input type="checkbox" ng-checked="checkProperty" ng-init="checkProperty = true" ng-model="checkProperty">
	```
- ng-selected
	对是否出现 option 标签的 selected 属性进行绑定
	例：
	```html
	<!-- 当 checkbox 选中后，select 会选择第二个 -->
			<input type="checkbox" ng-model="isTwoFish">
			<select>
				<option>One Fish</option>
				<option ng-selected="isTwoFish">Two Fish</option>
			</select>
	```
- ng-href
	ng-href 是用来取代 a 标签原有的 href 的。当使用当前作用域中的属性动态创建 URL 时，如果插值尚未生效，href 会指向 404，而 ng-href 会等到插值生效后再执行点击链接的行为。

- ng-src
	AngularJS 会告诉浏览器在 ng-src 对应的表达式生效之前不要加载图片。和 ng-href 类似；
	注意：ng-src 和 ng-href 是 AngularJS 内置指令中唯二只能用“{{}}”设置属性值的指令，其它指令基本上都支持表达式；
	- src 与 ng-src 的区别：
		src 是 HTML 的属性，{{}} 是 ng 的表达式，表达式可用于很多地方，包含属性，所以直接 src="{{vm.url}}" 其实就是使用 ng 的表达式给属性赋值，这种做法的缺点是当第一次加载模板的时候浏览器会去请求 “{{vm.url}}” 的地址，当 ng 编译模板后把 {{vm.url}} 替换成对应的 URL 后会再次请求真实的地址，所以为了避免第一次无效的请求

#### 8.3.2. 在指令中使用子作用域

- ng-app

- ng-controller

- ng-include
	使用 ng-include 可以加载、编译并包含外部 HTML 片段到当前的应用中。模板的 URL 被限制在与应用文档相同的域和协议下，可以通过白名单或包装成被信任的值来突破限制。
	使用 ng-include 时 AngularJS 会自动创建一个子作用域。如果我们想使用某个特定的作用域，必须在同一个 DOM 元素上添加 ng-controller 指令，这样当模板加载完成后，不会像往常一样从外部作用域继承并创建一个新的子作用域：
	例：
	```html
	<div ng-include="/myTempalteName.html" ng-c ng-init="name = 'world'">
		hello {{ name }}
	</div>
	```

- ng-switch
	```html
	<!-- 当输入 Liu 时会显示 Liu，其他时候显示 And The Winner is -->
	<input type="text" ng-model="person.name"/>
	<div ng-switch
		<!-- 默认值 -->
		<p ng-switch-default>And The Winner is</p>
		<h1 ng-switch-when="Liu">{{ person.name }}</h1>
	</div>
	<script>
		var app = angular.module("app",[]);
	</script>
	```
	在 switch 被调用之前我们用 ng-switch-default 来输出默认值。

- ng-view
	ng-view 指令用来设置将被路由管理和放置在 HTML 中的视图的位置。

- ng-if
	使用 ng-if 指令可以完全根据表达式的值在 DOM 中生成或移除一个元素。如果赋值给 ng-if 的表达式的值是 false，那对应的元素将会从 DOM 中移除，否则对应元素的一个的克隆将被重新插入 DOM 中。
	注意 ng-if 同 ng-show 或 ng-hide 指令最本质的区别是，它不是通过 css 显示或隐藏 DOM 节点，而是真正生成或移除节点。当一个元素被 ng-if 从 DOM 中移除，同它关联的作用域也会被销毁，当它重新加入 DOM 中时，会通过原型继承从它的父作用域生成一个新的作用域；
	注意：
	并不是只有 Controller 可以创建作用域，ng-if 、ng-switch、ng-include 等指令也会动态（隐式地）产生新作用域；
	辨析：
	ng-if 控制元素是否存在，ng-show 控制元素是否显示；

- ng-repeat
	ng-repeat 用来遍历一个集合或为集合中的每个元素生成一个模板实例。集合中的每个元素都会被赋予自己的模板和作用域。同时每个模板实例的作用域中都会暴露一些特殊的属性。

- ng-init
	ng-init 指令用来在指令被调用时设置内部作用域的初始状态。

- ng-bind
	我们可以通过 ng-bind 指令实现和{{}}一样的功能。HTML 加载含有{{}}语法的元素后并不会立即渲染它们，导致未渲染内容闪烁。我们可以通过 ng-bind 将内容绑定来避免闪烁。
	```html
	<span ng-bind="name"></span>
	```

- ng-bind-template
	同 ng-bind 类似，ng-bind-template 用来在视图中绑定多个表达式。
	```html
	<pre ng-bind-template="{{salutation}} {{name}}!"></pre>
	```

- ng-cloak
	除了使用 ng-bind 来避免未渲染元素闪烁，还可以在含有{{}}的元素上使用 ng-cloak 指令。ng-cloak 指令会将内部元素隐藏，直到路由调用相应的页面时才显示出来。
	```html
	<div id="template1" ng-cloak>{{ 'hello' }}</div>
	```

- ng-model
	ng-model 指令用来将 input、select、textarea 或自定义表单控件同包含它们的作用域中的属性进行绑定。
	它将当前作用域中运算表达式的值同给定的元素进行绑定。如果属性不存在，它会隐式创建并将其添加到当前作用域中。

- ng-show/ng-hide
	ng-show 和 ng-hide 根据所给表达式的值来显示或隐藏 HTML 元素。

- ng-change
	这个指令会在表单输入发生变化时计算给定表达式的值，需要和 ng-model 配合来使用

- ng-form
	ng-form 用来在一个表单内部嵌套另一个表单，普通的 HTML 的 form 标签是不允许嵌套了。

- ng-click
	ng-click 用来给指定元素绑定点击时调用的方法或者表达式；
	ng-click 可以通过点击调用一个方法或 scope 中的表达式（直接用 onclick 无法调用 controller 里定义的函数）；

- ng-submit
	ng-submit 用来将表达式同 onsubmit 事件进行绑定。这个指令同时会阻止浏览器默认行为，除非表单不含有 action 属性。

- ng-class
	使用 ng-class 动态设置元素的类，方法是绑定一个代表所有需要添加的类的表达式。

- ng-options
	使用 ng-options 结合 select 标签可以创建动态的选择框，教程：https://www.cnblogs.com/wolf-sun/p/4614532.html

## 9. 过滤器

AngularJS 中过滤器一般用于格式化和过滤后展示数据，开发者可以自定义过滤规则，来创建过滤器。

### 9.1. 使用过滤器

在 HTML 中的模板{{}}内通过 `|` 符号来调用过滤器，在过滤器中使用 `:` 连接过滤器参数；
例：字符转大写
```html
{{name | uppercase}}
```
在 JavaScript 代码中可以通过 $filter('filter_name')(data) 来调用过滤器；
例：字符串小写：
```javascript
 app.controller('DemoController',['$scope','$filter'],function($scope,$filter){
      $scope.name = $filter(lowercase')('LIUFANG');
    });
```
filter 过滤器可以从一个给定数组中选择一个子集，并将其生成一个新数组返回，和原生的数组方法 filter 类似。这个过滤器通常用来过滤需要进行展示的元素；

例：html 调用方式：
```html
{{ filter_expression | filter : expression :comparator}}
```
javascript 调用方式：
```javascript
$filter('filter')(array, expression, comparator)
```

### 9.2. 内置过滤器

过滤器可以链式调用，理论上没有数量限制；

#### 9.2.1. currency
currency 过滤器可以将一个数值格式化为特定的货币格式：
```html
<h2>{{ 123 | currency}}</h2><!-- $123.00 -->
```
参数：自定义货币符号（会自动添加到数字前）：保留几位小数（默认是保留两位小数）
```html
<h2>{{ 123 | currency:"￥":3}}</h2><!-- ￥123.000 -->
```

#### 9.2.2. number
格式化数值：
```html
<h2>{{1234.56789 | number}}</h2>
<!-- 默认保留 3 位小数：1234.568 -->
<h2>{{1234.56789 | number:1}}</h2>
<!-- 保留 1 位小数：1234.6 -->
```

#### 9.2.3. date
date 过滤器可以将日期格式化成需要的格式：
```html
<h1>date 过滤器</h1>
	<h5>{{today }}</h5><!-- "2016-01-23T08:44:59.271Z" -->
	<h5>{{today | date:'medium' }}</h5><!-- Jan 23, 2016 4:44:59 PM -->
	<h5>{{today | date:'short' }}</h5><!-- 1/23/16 4:44 PM -->
	<h5>{{today | date:'fullDate' }}</h5><!-- Saturday, January 23, 2016 -->
	<h5>{{today | date:'longDate' }}</h5><!-- January 23, 2016 -->
	<h5>{{today | date:'mediumDate' }}</h5><!-- Jan 23, 2016 -->
	<h5>{{today | date:'shortDate' }}</h5><!-- 1/23/16 -->
	<h5>{{today | date:'mediumTime' }}</h5><!-- 4:44:59 PM -->
	<h5>{{today | date:'shortTime' }}</h5><!-- 4:44 PM -->
	<h5>{{today | date:'yyyy-MM-dd HH:mm:ss' }}</h5><!-- 2016-01-23 08:44:59 -->
	<h5>{{1506997794 | date:'yyyy-MM-dd HH:mm:ss' }}</h5><!-- 直接将 unix 时间戳转换为指定格式 2017/10/3 10:29:46 -->

```

#### 9.2.4. lowercase/uppercase
lowercase/uppercase 用于大小写的转换：
```html
{{'Abc | lowercase'}}
<!-- abc -->
{{'Abc | uppercase'}}
<!-- ABC -->
```

#### 9.2.5. limitTo
limitTo 用于截取数组前 n 位元素进行显示：
```html
{{[1,2,3,4,5,6,7,8,9] | limitTo:4}}
<!-- 1,2,3,4 -->
{{abcdefghijk | limitTo:4}}
<!-- abcd -->
```

#### 9.2.6. orderBy
orderBy 用于在 ng-repeat 中指定排序的列：
```html
<table>
	<tr>
		<th>Name</th>
		<th>Phone Number</th>
		<th>Age</th>
	</tr>
	<tr ng-repeat="friend in friends | orderBy: 'age'"><!--age：按 age 升序排序，-age：按 age 降序排序 -->
		<td>{{friend.name}}<td>
		<td>{{friend.phone}}<td>
		<td>{{friend.age}}<td>
	</tr>
</table>
```

### 9.3. 自定义过滤器

例：时间格式自定义过滤器：
```javascript
angular.module('app').filter('recommendSMSToDate',function() {
	return function(_res){ // 第一个参数：要过滤 / 格式化的对象；第 2、3、4 个参数：使用过滤器时冒号后传入的参数
		var _date = new Date(_res);
		var year = _date.getFullYear();
		var month = _date.getMonth() + 1;
		var date = _date.getDate();
		var hour = _date.getHours();
		var minute = _date.getMinutes();
		var result = year + "-" + month + "-" + date + " " + hour + ":" + minute;
		return result;
  }
});
// 将时间戳转化为格式：2016-5-3 10:54
```

注意：若在不是 HTML 中的地方引用过滤器，引入依赖时需要在过滤器名后加上“Filter”后缀：
```javascript
angular.module('app').controller('mainCtrl', ['dateFilter', '$scope',  function (dateFilter, $scope) {
	$scope.test = dateFilter(123456);
}]);
```

## 10. Refer Links
官方 API 文档：https://code.angularjs.org/1.6.4/docs/api

Angularjs 权威指南读书笔记：https://github.com/brizer/Study-Notes/tree/master/%E5%BF%83%E5%BE%97%E4%BD%93%E4%BC%9A/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/AngularJS%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97

Angularjs 笔记：https://angularjs.shujuwajue.com/