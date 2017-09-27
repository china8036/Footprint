<!-- toc -->

* [AngularJS Base Note](#angularjs-base-note)
  * [作用域与 $scope](#作用域与-scope)
    * [概述](#概述)
    * [$scope 生命周期](#scope-生命周期)
    * [工具方法](#工具方法)
  * [双向数据绑定](#双向数据绑定)
    * [ng-moddel](#ng-moddel)
    * [ng-bind](#ng-bind)
  * [MVC](#mvc)
    * [Controller](#controller)
    * [Model](#model)
    * [View](#view)
  * [模块](#模块)
    * [概述](#概述)
    * [模块定义](#模块定义)
    * [模块加载](#模块加载)
    * [项目模块化](#项目模块化)
  * [服务](#服务)
    * [概述](#概述)
    * [内置服务](#内置服务)
    * [服务定义](#服务定义)
  * [依赖注入](#依赖注入)
    * [隐式依赖注入](#隐式依赖注入)
    * [显示依赖注入](#显示依赖注入)
    * [行内依赖注入](#行内依赖注入)
    * [应用](#应用)
  * [路由](#路由)
    * [ngRoute](#ngroute)
    * [uiRoute](#uiroute)
  * [指令系统](#指令系统)
    * [生命周期](#生命周期)
    * [指定定义](#指定定义)
    * [内置指令](#内置指令)
  * [过滤器](#过滤器)
    * [使用过滤器](#使用过滤器)
    * [内置过滤器](#内置过滤器)
    * [自定义过滤器](#自定义过滤器)
  * [Refer Links](#refer-links)

<!-- toc stop -->

# AngularJS Base Note #

## 作用域与 $scope ##
### 概述 ###
- 作用域是视图和控制器之间的胶水。在应用将视图渲染并呈现给用户之前，视图中的模板会和作用域进行连接，然后应用会对DOM进行设置以便将属性变化通知给Angular。 我们可以依赖视图在修改数据时立刻更新$scope，也可以依赖$scope在其发生变化时立刻重新渲染视图。我们可以将应用的业务逻辑都放在控制器中，而将相关的数据都放在控制器的作用域中。
- $scope是一个指向应用model的object，也是表达式的执行上下文（作用域）；
- $scope被放置于一个类似应用的DOM结构的树形层次结构中，子$scope对象会继承父$scope对象的属性和方法；
- 每个Angular应用（ng-app）都有且只有一个根$scope，即$rootScope，$rootScope是所有$scope对象的最上层；
- 除非是在指令内部创建的作用域（即孤立作用域），所有的作用域都是通过原型继承而来的，这也是它们可以访问父级作用域的原因；
- ng-controller指令为本HTML页面的DOM创建一个新的$scope对象，并将它嵌套在$rootScope中；
- 通过$scope可以传播事件，即可向父scope传播也可以向子scope传播；
- 默认情况下，AngularJS在当前作用域中无法找到某个属性时，便会在父级作用域中查找，如果找不到，则会顺着父级作用域一直向上寻找，直到抵达$rootScope为止，如果这时也找不到，则继续运行，但是视图无法更新；
- 可以通过angular.element($0).scope()获取和调试$scope对象；





### $scope 生命周期 ###

1)	creation

用户请求应用起始页，angular 被加载，查找ng-app，Angular 遍历模板，查找其它指令；
在创建控制器或指令时，AngularJS会用$injector创建一个新的作用域，并在这个新建的控制器或指令运行时将作用域传递进去；

2)	link

当AngluarJS开始运行时，所有的$scope对象都会附加或者链接到视图中。所有创建 $scope对象的函数也会将自身附加到视图中。这些作用域将会注册当Angular应用上下文中发生变化时需要运行的$watch函数,Angular通过这些函数获知何时启动事件循环；

3)	update

当事件循环运行时，它通常执行在顶层$scope对象上（也就是$rootScope）,每个子作用域都执行自己的脏值检测。每个监控函数都会检测变化，如果检测到任意变化，$scope对象就会触发指定的回调函数；

4)	scope destruction

当child scope不再是必须的时候，child scope的产⽣生者有责任通过scope.$destroy() API销毁它们（child scope）。这将会停止$digest的调用传播传播到child scope中，让被child scope model使用的内存可以被gc回收；



### 工具方法 ###
https://angularjs.shujuwajue.com/apply.html      
https://angularjs.shujuwajue.com/watchfang_fa.html   
http://angularjs.cn/A0a6    

$scope实际上是一个POJO，即普通JavaScript对象，提供了一些工具方法（如$apply、$watch等），是MVC和双向数据绑定实现的基础；

#### $watch ####


#### $digest ####



#### $apply ####



## 双向数据绑定 ##


### ng-moddel ###



### ng-bind ###


- ng-model和ng-bind的区别：   
	- ng-bind是从$scope -> view的单向绑定，也就是说ng-bind是相当于{{object.xxx}}，是用于展示数据的，当页面angularjs未加载完毕时，显示为空；
	- ng-model是$scope <-> view的双向绑定，当页面angularjs未加载完毕时，显示“{{xxxxxx}}”；


## MVC ##
MVC工作模式：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/efa776ab6cdbc18a96c8c90995ca1160.jpg)

### Controller ###

- Angular中控制器的作用是增强视图，Angular中的控制器是一个函数，用来向视图的作用域中添加额外的功能，我们用它来给作用域对象设置初始状态，并添加自定义行为；
- AngularJS不再让用户操作DOM和事件，用于只需要关注JS与HTML的关系，并且实现HTML与JS的隔离，通过angularJS来实现粘合，我们可以把angularJS看出html与JS之间的胶水；
- 当我们在模块中创建控制器时，AngularJS会生成并传递一个新的$scope给这个控制器；
- 设计良好的应用会将复杂的逻辑放到指令和服务中。通过使用指令和服务，我们可以将控制器重构成一个轻量且更易维护的形式。


注意：
- controller中应该是业务逻辑；
- 不要复用controller，因为每个controller只负责渲染一块视图；
- 不要在controller中操作DOM，应通过指令的link函数实现；
- 不要在controller中进行数据格式化，应通过表单控件实现；
- 不要在controller中进行数据过滤，应通过过滤器$filter实现；
- controller之间不应相互调用，其通信应通过事件广播/$rootScope/服务进行；
多个控制器间通信的方式：https://segmentfault.com/a/1190000009167725 




### Model ###

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/177ec936519d50809027b31fbbd79cf2.jpg)

加载angular的js文件后，angularJS启动并找到ng-app所在的标签，接管该标签内部包含angular元素的JavaScript解析；

创建数据模型greeting.text，并将input标签的值，与该模型进行双向绑定；



### View ###

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/50eed8d8a0f65fd7d06c90d534b2d2c0.jpg)



#### 表达式 ####

{{ }}符号将一个变量绑定到$scope上。当用$watch进行监听时，AngularJS会对表达式或函数进行运算；

表达式和eval非常类似，但是由AngularJS来处理，它们有以下不同的特性： 
- 所有的表达式都在其所属的作用域内部执行，并有访问本地$scope的权限；
- 如果表达式发送了TypeError和ReferenceErroe并不会抛出异常；
- 不允许使用任何流程控制比如if；
- 可以接受过滤器和过滤器链；


## 模块 ##


### 概述 ###
大部分应用都有⼀一个主方法(main)用来实例化、组织、启动应用，AngularJS应用没有主方法，而是使用模块来声明应用应该如何启动；

模块是组织业务的一个封装，在一个模块当中定义多个服务，当引入了一个模块的时候，就可以使用这个模块提供的一种或多种服务了，另外，服务只是模块提供的多种机制中的一种，其它的还有指令（directive ），过滤器（ filter ），及其它配置信息；

模块允许通过声明的方式来描述应用中的依赖关系，以及如何进行组装和启动；

AngularJS 本身的一个默认模块叫做 ng ，它提供了 $http ， $scope等等服务；

优点：
- 在单元测试是不需要加载全部模块的，因此这种方式有助于写单元测试；
- 可以在特定情况的测试中增加额外的模块，这些模块能更改配置，能帮助进行端对端的测试；
- 第三方代码可以作为可复用的module打包到angular中；
- 模块可以以任何先后或者并行的顺序加载（因为模块的执行本身是延迟的）；

### 模块定义 ###

```javascript
angular.module(name[, requires], configFn);
```
这个方法能够接受两个参数，第一个是模块的名称，第二个是依赖列表，也就是可以被注入到模块中的对象列表；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/eb9c278d96bf6a6e3221ce88aa175fdc.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/3c743f7a0ad3a6205eb479dbb3c5c527.jpg)

使用 angular.moudle() 方法创建一个名为 HelloAngular 的模块；   
使用模块的方法 controller() 创建一个名为 helloNgCtrl 的控制器；   


### 模块加载 ###

#### config ####

配置块。在模板的加载阶段，AngularJS会在提供者注册和配置的过程中对模块进行配置。在整个AngularJS工作流中，这个阶段是**唯一能够在应用启动前进行修改的部分**。

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
AngularJS会在编译时执行这些辅助函数，实际上效果等同于下面的代码：
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
AngularJS会以这些函数书写和注册的顺序来执行它们，也就是说我们无法注入一个尚未注册的 provider；


#### run ####

运行块。和配置块不同，运行块在注入器创建之后被执行，它是所有AngularJS应用中第一个被执行的方法。

运行块通常用来注册全局的事件监听器。例如，我们会在.run()块中设置路由事件的监听器以及过滤未经授权的请求

实例：
在每次路由发送变化时，都执行一个函数来验证用户的权限，放置这个功能唯一合理的地方就是run方法：
```javascript
angular.module("myApp",[])
  .run(function($rootScope,AuthService){
    $rootScope.$on("$routeChangeStart",function(evt,next,current){
      //如果用户未登录
      if(!AuthService,userLoggedIn()){
        if(next.templateUrl==="login.html"){
          //已经转向登陆路由因此无需重定向
        } else {
          $location.path("/login");
        }
      }
    });
});
```

### 项目模块化 ###

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/b093b727fe23df04a2fba230b23f3e21.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/4873703d6e8a9b99bd346853ff777221.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/67e523d9d8f5999892424200cafe4040.jpg)

## 服务 ##

### 概述 ###

- 控制器只会在需要时被实例化，不再需要就会被销毁。这意味着每次切换路由或重新加载视图时，当前的控制器会被AngularJS清除掉。而服务提供了一种能在应用的整个生命周期内保持数据的办法，它能够在控制器之间进行通信，并且能保证数据的一致性。 **服务是一个单例对象，在每个应用中只会被实例化一次(被$injector实例化)，并且是延迟加载的，服务提供了把与特定功能相关联的方法集中在一起的接口**；
- 服务本身是一个任意的对象；
- Angular 是用 $provider 对象来实现自动依赖注入机制，注入机制通过调用一个 provider 的 $get() 方法，把得到的对象作为参数进行相关调用，$provider提供很多很定义服务的方法,这些简便的方法还直接被module所引用；



### 内置服务 ###

#### $location ####
Angular中使用内置的 $location 服务来监听、操作URL，功能包括了获取、监听、改变地址栏的URL；  
与URL实现双向数据绑定（地址栏变动、前进后退或者点击页面的链接均会触发）；   
将URL对象封装成了一套方法（protocol、host、port、path、search和hash），对window.location对象的API进行优雅封装，支持jQuery风格的链式写法，集成了 HTML5 的 history API；

工具方法：
##### path() #####

用来获取页面当前的路径。
```javascript
$location.path();//返回当前路径 
```
修改当前路径并跳转到应用中的另一个URL：
```javascript
$location.path('/');//把路径修改为'/'路由 
```
注意：$location 无法使整个页面重新加载。如果改变URL后希望重新加载页面，请使用 `$window.location.href`。


##### replace() #####

跳转后不能点击后退按钮。
```javascript
$location.path('/home');
$location.replace();
//或者
$location.path('/home').replace(); 
```



##### absUrl() #####

获取编码后的完整URL。

#### $http ####
https://code.angularjs.org/1.6.4/docs/api/ng/service/$http 

$http 是 angular 中的一个核心服务，利用浏览器的 xmlhttprequest 对象或者 JSONP 与远程 HTTP 服务器进行交互；

在 controller 中可通过与 $scope 相同的方式注入获取 $http 对象：
```javascript
function controller($scope,$http){
}；
```

##### General usage #####
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
注：v1.5之后 $http 的 success 和 error 方法已废弃，使用 then 方法替代。

- $http 服务的参数config作为一个json对象，接受的配置项有：
	- method 方法；
	- url 路径；
	- params：GET请求的参数，如data: {name:"12346",pwd:"123"}；
	- data：post请求的参数，如data: {name:"12346",pwd:"123"}；
	- headers 头信息，有缺省定义，如果自定义则会覆盖默认定义，如headers: {'Content-Type': undefined}；
	- transformRequest 请求预处理函数，有缺省定义，如果自定义则会覆盖默认定义；
	- transformResponse 响应预处理函数，有缺省定义，如果自定义则会覆盖默认定义；
	- cache 缓存；
	- timeout 超时毫秒，超时的请求会被取消；
	- withCredentials 跨域安全策略的属性；

- response对象包含的属性：
	- data – {string|Object} – The response body transformed with the transform functions.
	- status – {number} – HTTP status code of the response.
	- headers – {function([headerName])} – Header getter function.
	- config – {Object} – The configuration object that was used to generate the request.
	- statusText – {string} – HTTP status text of the response.



##### Shortcut #####

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

1)	config：一个JSON对象，其中主要包含该请求的url、data、method等，如{url:"login.do",method:"post",data:{name:"12346",pwd:"123"}}；

2)	url {String} 请求的URL地址；

3)	params {key,value} 请求参数，将在URL上被拼接成？key=value；

4)	data {key,value} 数据，将被放入请求内发送至服务器；


#### $cookies ####
https://docs.angularjs.org/api/ngCookies/service/$cookies 

$cookies 服务能够很好的发挥 HTML5 cookies，当 HTML5 API 可用时浏览器会选择使用 HTML5 提供的 API，如果不可用则默认选择 document.cookies。无论那种方式，你都可以选择使用相同的 API 来进行工作。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/dab85c8deb94c707590f628e085c77d8.jpg)

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
例：用户登录，记住密码的cookie有效期7天
```javascript
  var cookieInfo= {};
  cookieInfo.username = $scope.username;
  cookieInfo.password = $scope.password;
  var expireDate = new Date();
  expireDate.setDate(expireDate.getDate() + 7);//设置cookie保存7天
  $cookies.putObject("user", cookieDate, {'expires': expireDate});
```




### 服务定义 ###

- $provider.factory
factory 方法直接把一个函数当成是一个对象的 $get() 方法；
返回的内容可以是任何类型；
例：
```javascript
var myApp = angular.module('myApp',[],function($provide){
$provide.factory('CustomFactory',function(){
        return [1,2,3,4,5,6,7];
});
});
```

- $provider.service
和factory类似，但返回的内容必须是对象；
例：
```javascript
var myApp = angular.module('myApp',[],function($provide){
    $provide.service('CustomService2',function(){
        return {
                message : 'CustomService Message2'
            }
});
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
});
```
**注：factory、service 实际上都是 provider；**



## 依赖注入 ##
通常一个对象有三种方法可以获得对其依赖的控制权：
- 在内部创建依赖：这种方法的弊端就是不方便以后的维护，使隔离变的异常困难。
- 通过全局变量进行引用：这种方法的弊端就是污染了全局作用域，当代码量达到了一定程度后，容易出现问题。
- 在需要的地方进行参数传递（依赖注入）：这种方法不仅对测试很有用，而且不会污染全局变量，是很好的设计模式。AngularJS用的就是这种方法。

每一个AngularJS应用都有一个注入器(injector)用来处理依赖的创建。注入器是一个负责查找和创建依赖的服务定位器。

内部机制：https://benweizhu.github.io/blog/2015/01/04/yes-angularjs-6/ 

### 隐式依赖注入 ###

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

如果没有明确的声明，AngularJS会假定参数名称就是依赖的名称。因此，它会在内部调用函数对象的toString()方法，分析并提取出函数的参数列表，然后通过 $injector将这些参数注入进对象实例；

**这个方法只适合未经过压缩和混淆的代码，因为AngularJS需要原始未经压缩的参数列表来进行解析。**

### 显示依赖注入 ###

AngularJS提供了显式地方法来明确定义一个函数在被调用时需要用到的依赖关系。

例：
```javascript
var aControllerFactory = function aController($scope,greeter){
  //控制器
};
aControllerFactory.$inject = ['$scope','greeter'];

angular.module('myApp',[])
  .controller('MyController',aControllerFactory)
  .factory('greeter',greeterService);
//获取注入器并创建一个新的作用域
var injector = angular.injector(['ng','myApp']),
   controller = angular.get('$controller'),
   rootScope = angular.get('$rootScope'),
   newScope = rootScope.$new();
//调用控制器
controller('MyController',{$scope:newScope});
```
通过这种方法声明依赖，即使在源代码被压缩，参数名称发生改变的情况下依然可以正常工作；

对于这种声明方式来讲，参数的顺序是十分重要的，因为$inject数组元素的顺序必须和注入的参数的顺序一一对应。

### 行内依赖注入 ###

AngularJS提供的行内注入方法实际上是一种语法糖；

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

行内依赖注入与前面的提到的通过$inject属性进行声明的原理是一样的，但是允许我们在函数定义的时候从行内将参数传入，这种方法方便，简单，而且避免了在定义的过程中使用临时变量；

需要注意的是，行内声明的方式允许我们直接传入一个参数数组，而不是一个函数。数组的元素是字符串，它们代表的是可以被注入到对象中的依赖名字，最后一个参数就是依赖注入的目标函数对象本身。

### 应用 ###

#### 模块依赖注入 ####

```javascript
//采用行内依赖注入构造 module
var myApp = angular.module('myApp', [
	‘ngRoute’, ‘ngAnimate’, ‘bookStoreCtrl’, ‘bookStoreFilter’, ‘bookStoreService’, ‘bookStoreDirective’
]);
// 其中ng开头的是angularJS默认提供的模块，其它是自定义的第三方模块
```
**注：这里所依赖的模块必须在html文件中导入，若没有导入html文件却依赖了模块，会导致整个angularjs的module初始化加载失败（当页面的ng指令都失效时很有可能是这个原因导致的）；**


#### 控制器依赖注入 ####

```javascript
//采用行内依赖注入构造 controller
angular.module('myApp',[])
.controller('DemoController',['$rootScope','$scope','$http',function($rootScope,$scope,$http){
	...
}])
```

## 路由 ##

除了使用ng-include指令在视图中引用多个模板外，更好的做法是将视图分解成布局和模板视图，并且根据用户当前访问的URL来展示对应的视图。 我们可以将模板分解到视图中，并在布局模板内进行组装。我们可以在$route服务的提供者$routeProvider中通过声明路由来实现这个功能。

- 为什么需要前端路由的重要原因？
	- AJAX请求不会在浏览器留下历史记录，用户体验差；
	- 用户无法直接通过URL进入应用中的某个指定页面（保存为书签/连接分享）；
	- Ajax的使用极不利于SEO；

- 前端路由实现的基本方法：
	- 通过锚点 # 实现；
	- 通过HTML5的history API实现；


### ngRoute ###

在HTML中引入angular-route.js文件
```html
<script src="https://cdn.bootcss.com/angular.js/1.6.4/angular-route.min.js"></script>
```
并把ngRoute模块在我们的应用中当做依赖加载进来：
```javascript
angular.module("app",["ngRoute"]);
```
- 布局模板ng-view
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
ng-view是由ngRoute模块提供的一个特殊指令，它的独特作用是将HTML中给$route对应的视图内容占位。它会创建自己的作用域并将模板嵌套在内部。通过将ng-view指令和路由组合到一起，我们可以精确地指定当前路由所对应的模板在DOM中的渲染位置。

- 使用$routeProvider配置路由
可以使用服务$routeProvider的when()和otherwise()两个方法在模块的config函数中定义应用的路由：   
例：
```javascript
	angular.module("myApp",[])
	  .config(["$routeProvider", function($routeProvider){
	    //在这里定义路由
	    $routeProvider
	      .when('/',{
	        templateUrl:'views/home.html',
	        controller:'HomeController'
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
	when方法接受两个参数：
		- 路由路径，这个路径会和$location.path进行匹配，$location.path也就是当前URL的路径；
		- config配置对象，决定了当第一个参数中的路由能够匹配时具体做什么：
		以下是配置对象的可用属性：
	 		- controller：如果配置对象中设置了controller属性，那么这个指定的控制器会与路由所创建的新作用域关联在一起。如果参数值是字符型，会在模块中所有注册过的控制器中查找对应的内容，然后与路由关联在一起。如果参数值是函数型，这个函数会作为模板中DOM元素的控制器并与模板进行关联；
			例：
			```javascript	
			controller:'MyController'
			//或者
			controller:function($scope){} 
			```
		 	- template：AngularJS会将配置对象中的HTML模板渲染到对应的具有ng-view指令的DOM元素中；
		 	- templateUrl：应用会根据该属性指向的路径读取视图，然后渲染到具有ng-view指令元素中；
		 	- resolve：如果设置了该属性，AngularJS会将列表中的元素都注入到控制器中的。如果这些依赖是promise对象，它们在控制器加载以及$routeChangeSuccess被触发之前，会被resolve并设置成一个值；
			例：resolve会发送一个$http请求，并将data的值替换为返回结果的值。列表中的键data会被注入到控制器中，所以在控制器中可以使用它
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
		 	- reloadOnSearch：如果设置为true（默认），当$location.search()发生变化时会重新加载路由。如果设置为false，那么当URL中的查询串部分发生变化时就不会重新加载路由。这个小窍门对路由嵌套和原地分页等需求非常有用；
	- html5Mode
	https://www.oschina.net/translate/pretty-urls-in-angularjs-removing-the-hashtag   
	Angular默认通过操作锚点来进行客户端路由，即服务器端路由和客户端路由的URL以 # 分隔，因此通过路由的URL中会出现一个 # 号，例如/foo/bar#/users/alice；    
	若要将 # 号从URL中去除，可以设置Angular通过HTML5的history API进行客户端路由：使用 $locationProvider 模块，并将html5Mode设置为true，并在html 中，设置base，比如： `<base rel="nofollow" href="/Xxxxwebapp/">`，这样会使URL变成/foo/bar/users/alice（需要浏览器支持HTML5，因为此时Angular通过pushState来进行路由）；此时服务端对所有的客户端路由的URL都需要返回首页（/foo/bar）视图，再交给Angular路由到/foo/bar/users/alice对应的视图。

	- $locationProvider
	http://blog.csdn.net/u014291497/article/details/54427103 
	1.6.x中关于前缀/#!/的改动

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
我们点击首页的/users/alice时，将会载入user.html，span的值为alice。$routeParams提供了当前的路由参数；   
例：
```javascript	
// Given:
// URL: http://server.com/index.html#/Chapter/1/Section/2?search=moby
// Route: /Chapter/:chapterId/Section/:sectionId
// Then
$routeParams ==> {chapterId:'1', sectionId:'2', search:'moby'}
```

- 路由事件
$route服务在路由过程中的每个阶段都会触发不同的事件，可以为这些不同的路由事件设置监听器并作出响应。我们需要给路由设置事件监听器，用$rootScope来监听这些事件；   
AngularJS在路由变化之前会广播$routeChangeStart事件。路由服务会开始加载路由变化所需要的所有依赖，并且模板和resolve键中的promise也会被resolve；



### uiRoute ###
UI-Router是Angular-UI提供的客户端路由框架，它解决了原生的ng-route的很多不足：
- 视图不能嵌套。这意味着$scope会发生不必要的重新载入。这也是我们在Onboard中引入ui-route的原因；
- 同一URL下不支持多个视图。这一需求也是常见的：我们希望导航栏用一个视图（和相应的控制器）、内容部分用另一个视图（和相应的控制器）；

#### 使用 $stateProvider 配置路由 ####

UI-Router提出了$state的概念。一个$state是一个当前导航和UI的状态，每个$state需要绑定一个URL Pattern。 在控制器和模板中，通过改变$state来进行URL的跳转和路由；

例：使用$stateProvider路由配置
```javascript
adminApp.config(function($stateProvider, $urlRouterProvider){  
         $stateProvider
           .state('aggreLive',{
            url:'/aggreLive/',
            templateUrl:'aggreLive.htm',
            controller:'aggreLive'
           })
           .state('aggreVideo',{
            url:'/aggreVideo/',
            templateUrl:'aggreVideo.htm',
            controller:'aggreVideo'
           }) 
           .state('aggreTeacher',{
            url:'/aggreTeacher/',
            templateUrl:'aggreTeacher.htm',
            controller:'aggreTeacher'
           })
         $urlRouterProvider.otherwise('/aggreLive/');      
})
//当访问/aggreLive/时，aggre这个$state 被激活，载入对应的控制器和视图。
```



#### 页面跳转 ####

在ui-router时，通常使用$state来完成页面跳转，而不是直接操作URL。

例：
```javascript
$state.go('contacts');  // 指定state名，相当于跳转到 /contacts
$state.go('contacts.detail', {contactId: 42});  // 相当于跳转到 /contacts/42
```


#### 深层次嵌套视图 ####

不同于Angular原生的ng-route，ui-router的视图可以嵌套，视图嵌套通常对应着$state的嵌套。 contacts.detail是contacts的子$state，contacts.detail.html也将作为contacts.html的子页面：

例：

contacts.html 
```html
<h1>My Contacts</h1>
<div ui-view></div>
<!-- contacts.detail.html -->
<span ng-bind='contactId'></span>
```
app-states.js
```javascript
$stateProvider
    .state('contacts', {
        url: '/contacts',
        template: 'contacts.html',
        controller: 'ContactCtrl'
    })
    .state('contacts.detail', {
        url: "/contacts/:contactId",
        templateUrl: 'contacts.detail.html',
        controller: function ($stateParams) {
            // If we got here from a url of /contacts/42
            $stateParams.contactId === "42";
        }
    });
```
上述ui-view的用法和ng-view看起来很相似，但不同的是ui-view可以配合$state进行任意层级的嵌套， 即contacts.detail.html中仍然可以包含一个ui-view，它的$state可能是contacts.detail.hobbies。


<!-- TODO -->
嵌套视图采用特殊的语法标记：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/9425e223da215424d219ab98a8a61070.jpg)


#### $state 匹配多个视图 ####

在ui-router中，一个$state下可以有多个视图，它们有各自的模板和控制器。这一点也是ng-route所没有的， 给了前端路由极大的灵活性；

例：
index.html
```html
<body>
  <div ui-view="filters"></div>
  <div ui-view="tabledata"></div>
  <div ui-view="graph"></div>
</body>
```
这一个模板包含了三个命名的ui-view，可以给它们分别设置模板和控制器：
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




## 指令系统 ##

### 生命周期 ###
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/d31baf17e0e61ff3117bc42224bc42b8.jpg)


### 指定定义 ###

通过AngularJS的.directive()方法，我们可以传入一个字符串和一个函数来注册一个新指令。其中字符串是这个指令的名字，指令名应该是驼峰命名风格的，函数应该返回一个对象。

#### restrict 匹配模式 ####
restrict匹配模式设置告诉AngularJS在编译HTML时用哪种声明格式来匹配指令定义：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/dee59c3124f6cf458b4786f070522bed.jpg)

注意：推荐使用元素和属性模式，即将restrict属性设置为“AE“；

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


#### replace ####
通过浏览器工具可以看到浏览器渲染视图后使用的是我们自定义的标签，这样不利于SEO：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/744762086084973c495384985d9b6180.jpg)

我们可以通过设置replace属性为true，将自定义标签从html中剔除以保证SEO；

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
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/d31b65b0aac0f5fc022f5941077f0938.jpg)



#### scope 作用域 ####

每当一个指令被创建的时候，都会有这样一个选择，是继承自己的父作用域（一般是外部的Controller提供的作用域或者根作用域（$rootScope）），还是创建一个新的自己的作用域，当然AngularJS为我们指令的scope参数提供了三种选择，分别是：false,true,{}；默认情况下是false。

- scope = false
当我们将scope设置为false的时候,我们创建的指令和父作用域（其实是同一个作用域）共享同一个model模型，所以在指令中修改模型数据，它会反映到父作用域的模型中。在这种情况下，在指令模板中可以直接使用父作用域中的变量，函数。

- scope = true
当我们将scope设置为true的时候，我们就新创建了一个作用域，只不过这个作用域是继承了我们的父作用域；我觉得可以这样理解，我们新创建的作用域是一个新的作用域，只不过在初始化的时候，用了父作用域的属性和方法去填充我们这个新的作用域。它和父作用域不是同一个作用域。

- scope = {}
当我们将scope设置为{}时，意味着我们创建的一个新的与父作用域隔离的新的作用域，这使我们在不知道外部环境的情况下，就可以正常工作，不依赖外部环境。
我们使用了隔离的作用域，不代表我们不可以使用父作用域的属性和方法，我们可以通过向scope的{}中传入特殊的前缀标识符（即prefix）“@,&,=“，引用应用指令的元素的属性，来进行数据的绑定。
	- 前缀标识符：
		- @
		这是一个单项绑定的前缀标识符   
		使用方法：在元素中使用属性，好比这样 `<div my-directive my-name="{{name}}"></div>`，注意，属性的名字要用-将两个单词连接，因为是数据的单项绑定所以要通过使用 `{{}}`来绑定数据。
		- =
		这是一个双向数据绑定前缀标识符   
		使用方法：在元素中使用属性，好比这样 `<div my-directive age="age"></div>`,注意，数据的双向绑定要通过=前缀标识符实现，所以不可以使用 `{{}}`。
		- &
		这是一个绑定函数方法的前缀标识符   
		使用方法：在元素中使用属性，好比这样 `<div my-directive change-my-age="changeAge()"></div>`，注意，属性的名字要用-将多个个单词连接。
	- 注意：在新创建指令的作用域对象中，使用属性的名字进行绑定时，要使用驼峰命名标准，比如下面的代码：
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
	html代码
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








### 内置指令 ###
AngularJS提供了一系列的内置指令，其中一些指令重载了元素的HTML元素，比如form和a标签。另一些则是ng为前缀；

这里有一个最佳实践，就是ng开头的都是AngularJS提供的内置指令，因此不要把自己开发的指令也以这个前缀命名；


#### 布尔类型指令 ####
AngularJS提供了一组带有ng-前缀版本的布尔属性，通过运算表达式的值来决定在目标元素上是插入还是移除对应的属性。

- ng-disabled 

- ng-readonly

- ng-checked
例：
```html
<!-- 如果checkbox选中，则checkProperty为true，初始为选中状态 -->
    <label>checkProperty = {{ checkProperty }}</label>
    <input type="checkbox" ng-checked="checkProperty" ng-init="checkProperty = true" ng-model="checkProperty">
```
- ng-selected
对是否出现option标签的selected属性进行绑定
例：
```html
 <!-- 当checkbox选中后，select会选择第二个 -->
    <input type="checkbox" ng-model="isTwoFish">
    <select>
      <option>One Fish</option>
      <option ng-selected="isTwoFish">Two Fish</option>
    </select>
```
- ng-href
ng-href是用来取代a标签原有的href的。当使用当前作用域中的属性动态创建URL时，如果插值尚未生效，href会指向404，而ng-href会等到插值生效后再执行点击链接的行为。
- ng-src 
AngularJS会告诉浏览器在ng-src对应的表达式生效之前不要加载图片。和ng-href类似；



#### 在指令中使用子作用域 ####

- ng-app

- ng-controller

- ng-include
使用ng-include可以加载、编译并包含外部HTML片段到当前的应用中。模板的URL被限制在与应用文档相同的域和协议下，可以通过白名单或包装成被信任的值来突破限制。
使用ng-include时AngularJS会自动创建一个子作用域。如果我们想使用某个特定的作用域，必须在同一个DOM元素上添加ng-controller指令，这样当模板加载完成后，不会像往常一样从外部作用域继承并创建一个新的子作用域：
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
在switch被调用之前我们用ng-switch-default来输出默认值。

- ng-view
ng-view指令用来设置将被路由管理和放置在HTML中的视图的位置。

- ng-if
使用ng-if指令可以完全根据表达式的值在DOM中生成或移除一个元素。如果赋值给ng-if的表达式的值是false，那对应的元素将会从DOM中移除，否则对应元素的一个的克隆将被重新插入DOM中。   
注意ng-if同ng-show或ng-hide指令最本质的区别是，它不是通过css显示或隐藏DOM节点，而是真正生成或移除节点。当一个元素被ng-if从DOM中移除，同它关联的作用域也会被销毁，当它重新加入DOM中时，会通过原型继承从它的父作用域生成一个新的作用域；    
注意：   
并不是只有 Controller 可以创建作用域，ng-if 、ng-switch、ng-include等指令也会动态（隐式地）产生新作用域；   

- ng-repeat
ng-repeat用来遍历一个集合或为集合中的每个元素生成一个模板实例。集合中的每个元素都会被赋予自己的模板和作用域。同时每个模板实例的作用域中都会暴露一些特殊的属性。

- ng-init
ng-init指令用来在指令被调用时设置内部作用域的初始状态。

- ng-bind
我们可以通过ng-bind指令实现和{{}}一样的功能。HTML加载含有{{}}语法的元素后并不会立即渲染它们，导致未渲染内容闪烁。我们可以通过ng-bind将内容绑定来避免闪烁。   
```html
<span ng-bind="name"></span> 
```

- ng-bind-template
同ng-bind类似，ng-bind-template用来在视图中绑定多个表达式。
```html
<pre ng-bind-template="{{salutation}} {{name}}!"></pre>
```

- ng-cloak
除了使用ng-bind来避免未渲染元素闪烁，还可以在含有{{}}的元素上使用ng-cloak指令。ng-cloak指令会将内部元素隐藏，直到路由调用相应的页面时才显示出来。
```html
<div id="template1" ng-cloak>{{ 'hello' }}</div> 
```

- ng-model
ng-model指令用来将input、select、textarea或自定义表单控件同包含它们的作用域中的属性进行绑定。   
它将当前作用域中运算表达式的值同给定的元素进行绑定。如果属性不存在，它会隐式创建并将其添加到当前作用域中。   

- ng-show/ng-hide
ng-show和ng-hide根据所给表达式的值来显示或隐藏HTML元素。

- ng-change
这个指令会在表单输入发生变化时计算给定表达式的值，需要和ng-model配合来使用

- ng-form
ng-form用来在一个表单内部嵌套另一个表单，普通的HTML的form标签是不允许嵌套了。

- ng-click
ng-click用来给指定元素绑定点击时调用的方法或者表达式；   
ng-click 可以通过点击调用一个方法或scope中的表达式（直接用onclick无法调用controller里定义的函数）；

- ng-submit
ng-submit用来将表达式同onsubmit事件进行绑定。这个指令同时会阻止浏览器默认行为，除非表单不含有action属性。

- ng-class
使用ng-class动态设置元素的类，方法是绑定一个代表所有需要添加的类的表达式。




## 过滤器 ##

AngularJS中过滤器的作用就是格式化展示数据，开发者可以自定义过滤规则，来创建过滤器。

### 使用过滤器 ###

在HTML中的模板{{}}内通过|符号来调用过滤器。 
例：字符转大写
```html
{{name | uppercase}}
```
在JavaScript代码中可以通过$filter('filter_name')(data)来调用过滤器；
例：字符串小写：
```javascript
 app.controller('DemoController',['$scope','$filter'],function($scope,$filter){
      $scope.name = $filter(lowercase')('LIUFANG');
    });
```
filter 过滤器可以从一个给定数组中选择一个子集，并将其生成一个新数组返回，和原生的数组方法filter类似。这个过滤器通常用来过滤需要进行展示的元素；

例：html调用方式：
```html
{{ filter_expression | filter : expression :comparator}} 
```
javascript调用方式：
```javascript
$filter('filter')(array, expression, comparator)
```

### 内置过滤器 ###


- currency
currency过滤器可以将一个数值格式化为货币格式。
```html
<h2>{{ 123 | currency}}</h2><!-- $123.00 --> 
```
也可以自定义货币符号：
```html
 <h2>{{ 123 | currency:"￥"}}</h2><!-- ￥123.00 --> 
```

- date
date过滤器可以将日期格式化成需要的格式：
```html
 <h1>date过滤器</h1>
      <h5>{{today }}</h5><!-- "2016-01-23T08:44:59.271Z" -->
      <h5>{{today | date:'medium' }}</h5><!-- Jan 23, 2016 4:44:59 PM -->
      <h5>{{today | date:'short' }}</h5><!-- 1/23/16 4:44 PM -->
      <h5>{{today | date:'fullDate' }}</h5><!-- Saturday, January 23, 2016 -->
      <h5>{{today | date:'longDate' }}</h5><!-- January 23, 2016 -->
      <h5>{{today | date:'mediumDate' }}</h5><!-- Jan 23, 2016 -->
      <h5>{{today | date:'shortDate' }}</h5><!-- 1/23/16 -->
      <h5>{{today | date:'mediumTime' }}</h5><!-- 4:44:59 PM -->
      <h5>{{today | date:'shortTime' }}</h5><!-- 4:44 PM -->
```


### 自定义过滤器 ###

例：时间格式自定义过滤器：
```javascript
adminApp.filter('recommendSMSToDate',function(){ 
return function(_res){ 
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
//将时间戳转化为格式：2016-5-3 10:54
```













## Refer Links ##
官方 API 文档：https://code.angularjs.org/1.6.4/docs/api 

Angularjs 权威指南读书笔记：https://github.com/brizer/Study-Notes/tree/master/%E5%BF%83%E5%BE%97%E4%BD%93%E4%BC%9A/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/AngularJS%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97 

Angularjs 笔记：https://angularjs.shujuwajue.com/