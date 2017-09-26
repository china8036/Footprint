<!-- toc -->
# 前端开发工具与环境搭建 #


一个完整的前端项目目录结构实例：   
<img src="http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/c5870a89bb08a6a2900d39911aa67f50.jpg" style="height:400px"/>


- .git：由 git 工具自动创建
- app：存放项目源代码
	- 其中 bower_components 目录存放项目依赖代码（如 jQuery 等），其它目录存放用户代码；
- node_modules：由 nodejs 自动创建，存放 nodejs 安装的各种工具、插件；
- test：
	- e2e：集成测试代码
	- unit：单元测试代码

## browser-sync ##

在项目根目录下：
```
browser-sync start --server --files "*.*"
```
注意：不可在系统根目录下运行，会导致监控的文件过多而造成系统卡慢；

## http-server ##
http-server 可以将本地的任何目录映射到 127.0.0.1 上；

- 安装
```
npm install -g http-server
```

- 使用
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/5ea6b2f888f496a96ef34eade7132dfd.jpg)



## Scaffolding Tool: Yeoman ##
http://yeoman.io/learning/index.html

Yeoman 是一款现代 webapp 的 SCAFFOLDING TOOL （脚手架工具），在 web 项目的立项阶段，使用 yeoman 来生成项目文件和代码结构，Yeoman 自动整合了最佳实践和工具，大大加速了后续的开发；

Yeoman is language agnostic. It can generate projects in any language (Web, Java, Python, C#, etc.)

### 安装 ###
YeoMan 是一个 node.js 项目，首先需要使用 npm 进行安装：
```
npm install -g yo
```
验证安装成功：
```
yo --version
```

### 使用 ###

Yeoman 的使用依赖于模板项目(generators)，每一个 generators 都是一个 node.js 项目，但是需要符合一定的规范，比如命名上必须得是 generator-XYZ 这种形式（其中 XYZ 即为项目名）；


#### 安装 generators ####

1. 搜索 generators
搜索 generators 有两种方式：
	- 前往 http://yeoman.io/generators/ 搜索；
	“胡子” 标识表示该 generators 为官方出品：   
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/2e2ae23d9bc6d4f2c711261539113da1.jpg)
	- 使用 yo 交互式界面搜索；
	使用 `yo` 可进入交互式界面：   
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/26/c810568b795fdf1a1709db221347786d.jpg)
2. 安装 generators
```
npm install -g generator-xxx
```
注意：必须是全局安装；

#### 使用 generators ####

- 使用 generators 快速构建项目结构；
例：使用 generator-webapp 构建一个项目结构：
```
yo webapp
```
即进入交互式命令行，按照提示输入信息后即创建了一个基本的项目结构；

- 使用 generators 子项目
某些复杂的 generators 项目会提供子项目（安装完后自动安装了所有子项目），此时可通过以下方法使用：   
例：使用 generator-angular 的子项目 controller 快速创建一个控制器
```
yo angular:controller MyNewController
```


- 通过以下命令访问项目主页
```
npm home generator-xxx
```


#### 其它命令 ####
- `yo --help`：获取帮助信息；
- `yo --version`：获取版本信息；
- `yo --generators`：列出所有已安装的 generators；
- `yo doctor`：诊断问题并提供解决方案；


### 自定义 Yeoman generators ###
http://imweb.io/topic/586766b5b3ce6d8e3f9f99b1

https://ivweb.io/topic/58d92e3fdb35a9135d42f840






## 包管理工具：npm ##
http://javascript.ruanyifeng.com/nodejs/npm.html    
http://www.runoob.com/nodejs/nodejs-npm.html    

### 概述 ###
NPM（node package manager），通常称为node包管理器，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。NPM依赖于couchdb的一个数据库，其中详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个很重要的作用就是：将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。


### 安装 ###
由于最新版的 nodejs 已经集成了 npm，所以只要安装完 nodeJS，npm也就安装完毕了。

查看npm版本，出现版本提示说明安装成功：
```
npm -v
```
升级npm：
```
npm install npm -g
```

- 淘宝 NPM 镜像 
http://npm.taobao.org/    
由于国内网络因素限制，npm 的官方镜像是非常慢，因此可以使用淘宝的 NPM 镜像，淘宝 NPM 镜像是一个完整 npmjs.org 镜像，可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。    
可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```
$ cnpm install [name]
```


### 使用 ###

#### 安装包 ####

- 安装最新版的package
```
npm install package
```
- 安装指定版本的package
```
npm install package@版本号
```
- 使用package.json进行安装
如果我们的项目依赖了很多package，一个一个地安装那将是个体力活。我们可以将项目依赖的包都在package.json这个文件里声明，然后在package.json所在目录使用命令一次性安装全部package：
```
npm install
```
若在当前目录下找不到 package.json，运行此命令会报错；

- 包安装模式
	- 本地安装
	会在当前目录创建一个新的文件夹 node_components，并将目标 package 下载到该文件夹中，且该 package 也只能在当前目录下使用：
	```
	npm install packagnpm install -g grunt-clie
	```
	例：
	在当前目录下安装 grunt-cli （grunt命令行工具）：
	```
	npm install grunt-cli
	```
	安装结束后，当前目录下回多出一个 node_modules 目录，grunt-cli就安装在里面。同时注意控制台输出的信息：
	```
	grunt-cli@0.1.9 node_modules/grunt-cli
	├── resolve@0.3.1
	├── nopt@1.0.10 (abbrev@1.0.4)
	└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)
	```
	简单说明：   
	grunt-cli@0.1.9：当前安装的package为grunt-cli，版本为0.19   
	node_modules/grunt-cli：安装目录   
	resolve@0.3.1：依赖的包有resolve、nopt、findup-sync，它们各自的版本、依赖在后面的括号里列出来   

	- 全局安装
	package会被下载到到特定的系统目录下，安装的 package 能够在所有目录下使用：
	```
	npm install -g package
	```
	例：
	上边虽然安装了grunt-cli，但在其它目录下，使用grunt仍会提示grunt命令不存在，因为只进行了本地安装（只能在当前安装目录下使用）。因此，对于这种类型的package，更适合进行全局安装，使得在所有目录下都可以使用grunt命令：
	```
	npm install -g grunt-cli
	```
	可以在控制台输出中看到安装所在目录：
	```
	grunt-cli@0.1.9 /usr/local/lib/node_modules/grunt-cli
	├── resolve@0.3.1
	├── nopt@1.0.10 (abbrev@1.0.4)
	└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)
	```


#### 卸载包 ####
```
npm uninstall package
```
卸载指定版本：
```
npm uninstall package@<version>
```

#### 更新包 ####

```
npm update package
```

#### 搜索包 ####
```
npm search package
```


## 其它命令 ##

### 查看帮助 ###
```
npm help
```

### 查看包信息 ###

查看当前目录安装的package：
```
npm ls
```
查看全局安装的package：
```
npm ls -g
```
查看指定package的信息：
```
npm ls package
```
查看指定package的详细信息：
```
npm info package
```

### 配置 ###
npm的配置工作主要是通过 `npm config` 命令。

#### 编辑配置文件 ####
```
npm config edit
```

#### 配置proxy ####

设置代理
```
npm config set proxy http://proxy.example.com:8080
```
查看代理
```
npm config get proxy
```
删除代理
```
npm delete proxy
```



#### package.json ####
package.json 位于模块的目录下，用于定义包的属性。

- 属性说明
	- name 包的名称
	- version 包的版本
	- description 包描述

	- main 入口文件，当require这个包名的时候，这个文件会被require进去。
	- repository 仓库信息
	- keywords 描述信息
	- author 作者信息
	- license 授权信息
	- bugs 如果其他人发现了bugs，应该怎么办
	- homepage 包的主页地址

	- scripts 基于npm的脚本，例如上面这个，可以在命令行里，在这个包里面运行npm run test，就会显示一行字
	- dependencies 包依赖
	- ependencies 包的开发依赖

例：
```
{
  "name": "steerjs",
  "version": "1.0.0",
  "description": "a tool package to build your component more quickly",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/tangshuang/steerjs.git"
  },
  "keywords": [
    "javascript",
    "component"
  ],
  "author": "frustigor",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/tangshuang/steerjs/issues"
  },
  "homepage": "https://github.com/tangshuang/steerjs#readme",
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-preset-latest": "^6.16.0",
    "babel-register": "^6.18.0",
    "gulp": "^3.9.1"
  }
}
```


- 文件生成
```
npm init
```
npm init命令可以用来初始化生成一个新的package.json文件。它会向用户提问一系列问题，如果你觉得不用修改默认配置，一路回车就可以了。   
如果使用了-f（代表force）、-y（代表yes），则跳过提问阶段，直接生成一个新的package.json文件。
```
$ npm init -y
```

- 设置默认信息
npm set用来设置环境变量。
```
$ npm set init-author-name 'Your name'
$ npm set init-author-email 'Your email'
$ npm set init-author-url 'http://yourdomain.com'
$ npm set init-license 'MIT'
```
上面命令等于为npm init设置了默认值，以后执行npm init的时候，package.json的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的 ~/.npmrc文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行npm config。
```
$ npm set save-exact true
```
上面命令设置加入模块时，package.json将记录模块的确切版本，而不是一个可选的版本范围。


## 包管理工具：bower ##
https://segmentfault.com/a/1190000002971135   
http://javascript.ruanyifeng.com/tool/bower.html   


### 安装 bower ###
```
npm install -g bower
```
验证安装成功：
```
bower --version
```

### 使用 bower ###
安装的包会被放置在bower_compontents目录下（该目录会在bower运行的文件夹下自动被创建），也可以通过更改.bowerrc文件中的配置选项来改变改目录创建的路径；

#### 安装包 ####

- 基本安装方式
	安装包的最新版本(会自动在 bower 库中找到对应的 GitHub 地址，并下载最新 release 版本)：
	```shell
	bower install <package>
	```
	安装包的指定版本：
	```shell
	bower install <package>#<version>
	```
	安装包的同时将新的依赖添加到 bower.json 中（--save会把安装的新包加入到dependencies，--save-dev则会加入devDependencies）：
	```shell
	bower install <package> --save
	```
	注：若使用了 --save 参数，则 uninstall 时也需要带上 --save；
	除了使用包的名称，还可以使用其他的参数来安装一个包：
	1)	Git端点，如： git://github.com/components/jquery.git；   
	2)	本地Git仓库的路径；   
	3)	端点的缩写，如components/jquery。Bower会默认你使用的是Github，在这种情形下，端点名称是git仓库URL中github.com后面的部分；   
	4)	指向zip或者tar文件的URL，文件内容将会自动被解压缩；   
	例：
	```shell
	# installs the project dependencies listed in bower.json
	$ bower install
	# registered package
	$ bower install jquery
	# GitHub shorthand
	$ bower install desandro/masonry
	# Git endpoint
	$ bower install git://github.com/user/package.git
	# URL
	$ bower install http://example.com/script.js
	```
- 使用 bower.json 安装
	如果你要在你的项目中安装多个包那么最好使用bower.json文件来安装包。这样你可以使用一行命令安装或者更新多个包。   
	bower.json文件属性：   
	1)	name – 你的应用/包的名字；   
	2)	version – 你的应用/包的版本号；   
	3)	dependencies – 你的应用需要的包。你应该像上面的例子一样指明每一个包的版本号。如果指定latest，Bower会安装最新发布额版本；   
	4)	private – 将该属性设置为true意味着你想要这个包保持私有并且并不想在将来将它添加到registry中；   
	一旦你编写完bower.json文件之后，可以简单地执行bower install命令来安装你指定的包。    
	例：
	```
	bower.json
	{
	  "name": "app-name",
	  "version": "0.0.1",
	  "dependencies": {
	    "sass-bootstrap": "~3.0.0",
	    "modernizr": "~2.6.2",
	    "jquery": "~1.10.2"
	  },
	  "private": true
	}
	```
	以上定义了一些关于你的项目的信息以及依赖的包的列表。bower.json文件实际上是用来定义一个Bower包的，因此事实上你是使用一些依赖包创建你自己的包；
	
	Bower包含了一个非常有用的功能来帮助你创建一个bower.json文件。在你的项目的根目录下执行：
	```
	bower init
	```
	bower会启动一个互动的程序帮助你创建这个文件。同时，你依然可以自己添加内容到这个文件中。



#### 使用包 ####
- 基本使用
例：
在HTML文件中：
```html
<script src="bower_components/jquery/dist/jquery.min.js"></script>
```
- 搭配 grunt
<!--TODO-->

#### 搜索包 ####

在Bower只寻找包的方法有两种，一种是查看在线的组件目录 https://bower.io/search/，另一种是直接使用命令行工具：
```
bower search <query_string>   
```
例：
搜索包含关键字’jquery’的包：
```
bower search query
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/3305cfcedf4178564591b0339873f647.jpg)   
每一条结果都显示了包的名称以及一个对应的Git端点，可以使用名字或者Git端点来安装一个包；



#### 更新包 ####

- 基本方式
```
bower update <package>
```
- 使用 bower.json 更新
```
bower update
```


#### 卸载包 ####


```
bowser uninstall <package1> <package2> <package3> . . .
```


#### 其它 ####

- list
```
bower list
```
列出所有已安装的包


- .bowerrrc 文件
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/d159e6523defa3cc1edebd2dd7097142.jpg)



### Build Tool: grunt ###

- 为什么需要构建工具？
减少如压缩、编译、单元测试、代码校验等重复且与业务逻辑无关的工作，让开发更加自动化；


安装
```
npm install -g grunt-cli
```

Grunt 有三个核心概念：Task、Target、Options；




### Build Tool: Gulp ###
官方文档：https://github.com/gulpjs/gulp/blob/master/docs/API.md   
中文文档：http://www.gulpjs.com.cn/docs/    
gulp 入门教程：http://www.tangshuang.net/3126.html   
gulp 资料合集：https://github.com/Platform-CUF/use-gulp   
慕课网 gulp in action：http://www.imooc.com/video/5692   

#### 概述 ####

gulp 是一个基于流的构建工具，个人感觉比 grunt 更简单清晰，也解决了 grunt 插件职责不明的问题。

- Gulp 解决了 Grunt 的不足
	- 插件很难遵守单一责任原则；
	- 常常用插件做一些本来不需要插件做的事情；
	- 试图用配置文件完成所有的事情，导致混乱不堪；
	- 落后的流程控制产生了让人头疼的临时文件（夹子），导致性能滞后；

- 基于流 -- pipe
什么是『基于流』呢？可以看下一个简单示例：
```
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```
可以看到这样一个链式调用的结构，除了开头和结尾，每一个命令的输出都是下一个命令的输入。先把任务分解成一个一个的小模块，然后再各取所需，组装起来。



#### 基本使用 ####

- 安装 gulp 命令行工具
```
npm install -g gulp-cli 
```
验证安装成功
```
gulp --version
```

- 在 npm 项目中，将 gulp 作为开发依赖（devDependencies）进行安装
创建一个 npm 项目
```
mkdir my_project
cd my_project
npm init
```
安装开发依赖 gulp（在项目 package.json 文件的 dependencesDev 字段中添加 gulp 信息） 
```
npm install --save-dev gulp
```

- 创建 gulpfile.js
```
	var gulp = require('gulp');//引用安装在项目中的 gulp 依赖
	
	gulp.task('my_task', function() {
		// 将你的自定义任务代码放在这
		setInterval(function(){
			console.log('I am out!');
		},5000);
	});
```


- 执行 gulp 任务
```
gulp one_task two_task
```
若不指定 task 名称，会默认执行名为 default 的 task；




#### 核心 API ####
gulp 的核心 API 主要使用于 gulpfile.js 中；


- gulp.task 
gulp.task 用于注册一个 gulp 任务；




- gulp.src 
gulp.src 获取指定源文件路径的数据流；




- gulp.dest 
gulp.dest 用于将信息输出到某个文件；




- gulp.watch
gulp.watch 用于监听文件变化，以运行相应的 task；




#### 使用插件 ####
https://github.com/twtrubiks/Gulp-Beginners-Guide   
https://www.cnblogs.com/libin-1/p/6439550.html   


在 https://gulpjs.com/plugins/ 上可以搜索 gulp 的各种 plugin；

- 使用 gulp-uglify
搜索 gulp-uglify，查看该 plugin 的主页 https://www.npmjs.com/package/gulp-uglify/，获取安装的帮助信息；   
```
npm install --save-dev gulp-uglify
```
修改 gulpfile.js
```
	var gulp = require('gulp');
	var uglify = require('gulp-uglify');
	var pump = require('pump');
	 
	gulp.task('compress', function (cb) {
	  pump([
	        gulp.src('lib/*.js'),
	        uglify(),
	        gulp.dest('dist')
	    ],
	    cb
	  );
	});
```

- 其他插件
```
npm init     //- 会生成一个package.json文件
npm install gulp --save-dev     //- gulp插件的核心
npm install gulp-minify-css --save-dev     //- 压缩CSS文件
npm install gulp-rev --save-dev         //- 对css、js文件名加MD5后缀
npm install gulp-rev-collector --save-dev   //- 路径替换
npm install gulp-clean --save-dev            //- 用于删除文件
npm install gulp-uglify --save-dev            //- 压缩js代码
npm install gulp-imagemin --save-dev      //- 压缩图片
npm install gulp-base64 --save-dev        //- 把小图片转成base64字符串
```


#### Gulp 搭配 Yeoman Generator：webapp ####
https://github.com/yeoman/generator-webapp#readme
```
Install: npm install --global yo gulp-cli bower generator-webapp
Run yo webapp to scaffold your webapp
Run gulp serve to preview and watch for changes
Run bower install --save <package> to install frontend dependencies
Run gulp serve:test to run the tests in the browser
Run gulp to build your webapp for production
Run gulp serve:dist to preview the production build
```




- 搭建 webapp 基本框架（自动安装了 gulp-uglify 等 plugin）
```
yo webapp
```

- 打开项目 index.html 并在开发时使用 browser-sync 实时监控文件变化自动刷新浏览器
```
gulp serve
```

- 打开项目 index.html 并在开发时使用 browser-sync 实时监控文件变化自动刷新浏览器且实时“编译”输出到 dist 目录
```
gulp serve:dist
```

- “编译”项目文件
```
gulp build
```

- clean 且 “编译”项目文件
```
gulp
```




#### 其它 ####


- 使用ES6
我们可以用 gulpfile.babel.js代替gulpfile.js（直接重命名即可），这样可以借助 babel 使用 ES6 新特性，不过我们也需要在项目中安装 babel：
```
	$ npm install --save-dev babel-core
	$ npm install --save-dev babel-register
	$ npm install --save-dev babel-preset-latest
```
在项目根目录下创建.babelrc文件，内容如下：
```
	{
	  "presets": ["latest"]
	}
```
这样，就可以在gulpfile中使用ES6代码。


- 部署项目任务
当项目任务比较复杂时，不可能通过一个 gulpfile.js 实现全部我们想要的任务功能，否则会让这个文件超级大；   
这种情况下，我们可能需要将不同的任务分到不同的文件模块中去。我的做法是创建一个gulp目录，把所有的任务分割成一个一个的，每一个文件一个任务：
```
	$ mkdir gulp/task
	$ vi gulp/task/add.js
```
在add.js中撰写任务流程：
```
	var args = require('../../tools/process.args'); // 引入 process.args
	
	module.exports = function() {
	    if(!args.name) {}
	    ...
	};
```
然后再把add.js require到gulpfile中进行任务注册：
```
	var gulp = require('gulp');
	gulp.task('add',require('./gulp/task/add'));
```
这样就可以将gulp任务进行分解，当你的gulp任务特别多的时候，可以有效的进行管理。

- 可使用 `--gulpfile gulpfile_path` 参数手动指定一个 gulpfile 的路径，这在你有很多个 gulpfile 的时候很有用。这也会将 CWD 设置到该 gulpfile 所在目录；   
例：执行了两次gulp task任务，但是因为第一次和第二次对应的gulpfile不同，在两个gulpfile里面，可能task任务的具体内容不同，所以两次任务的执行结果也可能不一样
```
$ gulp --gulpfile gulpfile.es6.js
$ gulp task
$ gulp --gulpfile gulpfile.js
$ gulp task
```


## webpack ##



## 测试工具：karma 和 jasmine ##

### 单元测试容器 karma ###


- 安装
```
npm install karma
```




### 单元测试工具 jasmine ###

jasmine 意为 “茉莉花”，类似 Java 中的 JUnit，jasmine 提供了一套语法，用来简洁地编写测试用例；

jasmine 有四个核心概念：**分组、用例、期望、匹配**，分别对应 jasmine 的四个函数：
- describe(string, function)：表示分组，即一组测试用例；
- it(string, function)：表示测试用例；
- expect(expression)：表示期望指定表示式具有某个值或者某种行为；
- to***(arg)：表示匹配；


- 安装
```
npm install jasmine
```


















































