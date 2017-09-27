

<!-- toc -->

* [Build Tool: Gulp](#build-tool-gulp)
  * [概述](#概述)
  * [基本使用](#基本使用)
  * [核心 API](#核心-api)
  * [使用插件](#使用插件)
  * [Gulp 搭配 Yeoman Generator：webapp](#gulp-搭配-yeoman-generatorwebapp)
  * [其它](#其它)

<!-- toc stop -->


# Build Tool: Gulp #
官方文档：https://github.com/gulpjs/gulp/blob/master/docs/API.md   
中文文档：http://www.gulpjs.com.cn/docs/    
gulp 入门教程：http://www.tangshuang.net/3126.html   
gulp 资料合集：https://github.com/Platform-CUF/use-gulp   
慕课网 gulp in action：http://www.imooc.com/video/5692   

## 概述 ##

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



## 基本使用 ##

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




## 核心 API ##
gulp 的核心 API 主要使用于 gulpfile.js 中；


- gulp.task 
gulp.task 用于注册一个 gulp 任务；




- gulp.src 
gulp.src 获取指定源文件路径的数据流；




- gulp.dest 
gulp.dest 用于将信息输出到某个文件；




- gulp.watch
gulp.watch 用于监听文件变化，以运行相应的 task；




## 使用插件 ##
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


## Gulp 搭配 Yeoman Generator：webapp ##
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




## 其它 ##


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