- [Build Tool: Gulp](#build-tool-gulp)
	- [1. 概述](#1-%E6%A6%82%E8%BF%B0)
	- [2. 基本使用](#2-%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
	- [3. 核心 API](#3-%E6%A0%B8%E5%BF%83-api)
	- [4. 使用插件](#4-%E4%BD%BF%E7%94%A8%E6%8F%92%E4%BB%B6)
	- [5. Gulp 搭配 Yeoman Generator：webapp](#5-gulp-%E6%90%AD%E9%85%8D-yeoman-generator%EF%BC%9Awebapp)
	- [6. 其它](#6-%E5%85%B6%E5%AE%83)
		- [6.1. 部署项目任务](#61-%E9%83%A8%E7%BD%B2%E9%A1%B9%E7%9B%AE%E4%BB%BB%E5%8A%A1)
		- [6.2. 指定一个 gulpfile 的路径](#62-%E6%8C%87%E5%AE%9A%E4%B8%80%E4%B8%AA-gulpfile-%E7%9A%84%E8%B7%AF%E5%BE%84)
	- [7. 常用插件](#7-%E5%B8%B8%E7%94%A8%E6%8F%92%E4%BB%B6)
		- [7.1. gulp-load-plugins](#71-gulp-load-plugins)
		- [7.2. 文件操作](#72-%E6%96%87%E4%BB%B6%E6%93%8D%E4%BD%9C)
			- [7.2.1. 清理文件夹：del](#721-%E6%B8%85%E7%90%86%E6%96%87%E4%BB%B6%E5%A4%B9%EF%BC%9Adel)
			- [7.2.2. 合并文件：gulp-concat](#722-%E5%90%88%E5%B9%B6%E6%96%87%E4%BB%B6%EF%BC%9Agulp-concat)
			- [7.2.3. 重命名：gulp-rename](#723-%E9%87%8D%E5%91%BD%E5%90%8D%EF%BC%9Agulp-rename)
			- [7.2.4. 以 Hash 命名：gulp-rev、gulp-rev-replace](#724-%E4%BB%A5-hash-%E5%91%BD%E5%90%8D%EF%BC%9Agulp-rev%E3%80%81gulp-rev-replace)
		- [7.3. 图片处理](#73-%E5%9B%BE%E7%89%87%E5%A4%84%E7%90%86)
			- [7.3.1. 压缩：gulp-imagemin](#731-%E5%8E%8B%E7%BC%A9%EF%BC%9Agulp-imagemin)
		- [7.4. CSS 处理](#74-css-%E5%A4%84%E7%90%86)
			- [7.4.1. 风格检查：CSSLint、sass-lint](#741-%E9%A3%8E%E6%A0%BC%E6%A3%80%E6%9F%A5%EF%BC%9Acsslint%E3%80%81sass-lint)
			- [7.4.2. 预编译：gulp-less、gulp-sass](#742-%E9%A2%84%E7%BC%96%E8%AF%91%EF%BC%9Agulp-less%E3%80%81gulp-sass)
			- [7.4.3. 兼容前缀：gulp-autoprefixer](#743-%E5%85%BC%E5%AE%B9%E5%89%8D%E7%BC%80%EF%BC%9Agulp-autoprefixer)
			- [7.4.4. 移除未使用的 CSS 选择器：gulp-uncss](#744-%E7%A7%BB%E9%99%A4%E6%9C%AA%E4%BD%BF%E7%94%A8%E7%9A%84-css-%E9%80%89%E6%8B%A9%E5%99%A8%EF%BC%9Agulp-uncss)
			- [7.4.5. 压缩：gulp-cssmin](#745-%E5%8E%8B%E7%BC%A9%EF%BC%9Agulp-cssmin)
			- [7.4.6. 图片 Base：gulp-base](#746-%E5%9B%BE%E7%89%87-base%EF%BC%9Agulp-base)
		- [7.5. JavaScript 处理](#75-javascript-%E5%A4%84%E7%90%86)
			- [7.5.1. 风格检查：gulp-eslint](#751-%E9%A3%8E%E6%A0%BC%E6%A3%80%E6%9F%A5%EF%BC%9Agulp-eslint)
			- [7.5.2. 使用 ES：gulp-babel](#752-%E4%BD%BF%E7%94%A8-es%EF%BC%9Agulp-babel)
			- [7.5.3. 压缩混淆：gulp-uglify](#753-%E5%8E%8B%E7%BC%A9%E6%B7%B7%E6%B7%86%EF%BC%9Agulp-uglify)
		- [7.6. HTML 处理](#76-html-%E5%A4%84%E7%90%86)
			- [7.6.1. 风格检查：gulp-htmlhint](#761-%E9%A3%8E%E6%A0%BC%E6%A3%80%E6%9F%A5%EF%BC%9Agulp-htmlhint)
			- [7.6.2. 压缩：gulp-html-minify](#762-%E5%8E%8B%E7%BC%A9%EF%BC%9Agulp-html-minify)
		- [7.7. 其它](#77-%E5%85%B6%E5%AE%83)
			- [7.7.1. open](#771-open)
			- [7.7.2. gulp-connect](#772-gulp-connect)
			- [7.7.3. gulp-debug](#773-gulp-debug)
			- [7.7.4. 显示你的项目的大小：gulp-size](#774-%E6%98%BE%E7%A4%BA%E4%BD%A0%E7%9A%84%E9%A1%B9%E7%9B%AE%E7%9A%84%E5%A4%A7%E5%B0%8F%EF%BC%9Agulp-size)
			- [7.7.5. 仅让发生改变的文件通过：gulp-changed](#775-%E4%BB%85%E8%AE%A9%E5%8F%91%E7%94%9F%E6%94%B9%E5%8F%98%E7%9A%84%E6%96%87%E4%BB%B6%E9%80%9A%E8%BF%87%EF%BC%9Agulp-changed)
	- [8. Refer Links](#8-refer-links)

# Build Tool: Gulp 

## 1. 概述

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

## 2. 基本使用

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
	```javascript
	var gulp = require('gulp');// 引用安装在项目中的 gulp 依赖
	
	gulp.task('my_task', function() {
		// 将你的自定义任务代码放在这
		setInterval(function(){
			console.log('I am out!');
		},5000);
	});
	```

- 执行 gulp 任务
	```javascript
	gulp one_task two_task
	```
	若不指定 task 名称，会默认执行名为 default 的 task；

## 3. 核心 API
gulp 的核心 API 主要使用于 gulpfile.js 中；

- gulp.task 
	
	gulp.task 用于注册一个 gulp 任务；

- gulp.src 
	
	gulp.src 获取指定源文件路径的数据流；

- gulp.dest 
	
	gulp.dest 用于将信息输出到某个文件；

- gulp.watch
	
	gulp.watch 用于监听文件变化，以运行相应的 task；

## 4. 使用插件
https://github.com/twtrubiks/Gulp-Beginners-Guide   

https://www.cnblogs.com/libin-1/p/6439550.html   

在 https://gulpjs.com/plugins/ 上可以搜索 gulp 的各种 plugin；

- 使用 gulp-uglify
	
	搜索 gulp-uglify，查看该 plugin 的主页 https://www.npmjs.com/package/gulp-uglify/，获取安装的帮助信息；   
	```
	npm install --save-dev gulp-uglify
	```
	修改 gulpfile.js
	```javascript
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
	npm init     //- 会生成一个 package.json 文件
	npm install gulp --save-dev     //- gulp 插件的核心
	npm install gulp-minify-css --save-dev     //- 压缩 CSS 文件
	npm install gulp-rev --save-dev         //- 对 css、js 文件名加 MD5 后缀
	npm install gulp-rev-collector --save-dev   //- 路径替换
	npm install gulp-clean --save-dev            //- 用于删除文件
	npm install gulp-uglify --save-dev            //- 压缩 js 代码
	npm install gulp-imagemin --save-dev      //- 压缩图片
	npm install gulp-base64 --save-dev        //- 把小图片转成 base64 字符串
	```

## 5. Gulp 搭配 Yeoman Generator：webapp

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

- preview the production build
	```
	gulp serve:dist
	```

- “编译”项目文件
	```
	gulp build
	```

- clean and build your webapp for production
	```
	gulp
	```

<!-- TODO: 怎么实时监控源文件变化并编译到 dist？ -->

## 6. 其它

### 6.1. 部署项目任务

当项目任务比较复杂时，不可能通过一个 gulpfile.js 实现全部我们想要的任务功能，否则会让这个文件超级大；   

这种情况下，我们可能需要将不同的任务分到不同的文件模块中去。我的做法是创建一个 gulp 目录，把所有的任务分割成一个一个的，每一个文件一个任务：

```shell
mkdir gulp/task
vi gulp/task/add.js
```
在 add.js 中撰写任务流程：
```javascript
var args = require('../../tools/process.args'); // 引入 process.args

module.exports = function() {
		if(!args.name) {}
		...
};
```
然后再把 add.js require 到 gulpfile 中进行任务注册：
```javascript
var gulp = require('gulp');
gulp.task('add',require('./gulp/task/add'));
```
这样就可以将 gulp 任务进行分解，当你的 gulp 任务特别多的时候，可以有效的进行管理。

### 6.2. 指定一个 gulpfile 的路径

可使用 `--gulpfile gulpfile_path` 参数手动指定一个 gulpfile 的路径，这在你有很多个 gulpfile 的时候很有用。这也会将 CWD 设置到该 gulpfile 所在目录；   

例：执行了两次 gulp task 任务，但是因为第一次和第二次对应的 gulpfile 不同，在两个 gulpfile 里面，可能 task 任务的具体内容不同，所以两次任务的执行结果也可能不一样
```shell
$ gulp --gulpfile gulpfile.es6.js
$ gulp task
$ gulp --gulpfile gulpfile.js
$ gulp task
```

## 7. 常用插件

### 7.1. gulp-load-plugins

https://www.npmjs.com/package/gulp-load-plugins

```javascript
// package.json 

"devDependencies": {
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.1",
    "gulp-rename": "^1.2.2",
    "gulp-uglify": "^2.0.1"
}

// gulpfile.js

var $ = require('gulp-load-plugins')();     // $ 是一个对象，加载了依赖里的插件

gulp.src('./**/*.js')
    .pipe($.concat('all.js'))               // 使用插件就可以用 $.PluginsName()
    .pipe($.uglify())
    .pipe($.rename('all.min.js'))
    .pipe(gulp.dest('./dist'))
```

### 7.2. 文件操作

#### 7.2.1. 清理文件夹：del

del （替代 gulp-clean)
```javascript
var del = require('del');
del('./dist');                      // 删除整个 dist 文件夹
```
#### 7.2.2. 合并文件：gulp-concat

#### 7.2.3. 重命名：gulp-rename

#### 7.2.4. 以 Hash 命名：gulp-rev、gulp-rev-replace

### 7.3. 图片处理

#### 7.3.1. 压缩：gulp-imagemin

### 7.4. CSS 处理

#### 7.4.1. 风格检查：CSSLint、sass-lint

https://github.com/sasstools/sass-lint

#### 7.4.2. 预编译：gulp-less、gulp-sass

gulp-sass
```javascript
var sass = require('gulp-sass');

gulp.src('./sass/**/*.scss')
    .pipe(sass({
        outputStyle: 'compressed'           // 配置输出方式，默认为 nested
    }))
    .pipe(gulp.dest('./dist/css'));
    
gulp.watch('./sass/**/*.scss', ['sass']);   // 实时监听 sass 文件变动，执行 sass 任务
```

#### 7.4.3. 兼容前缀：gulp-autoprefixer

#### 7.4.4. 移除未使用的 CSS 选择器：gulp-uncss

#### 7.4.5. 压缩：gulp-cssmin

#### 7.4.6. 图片 Base：gulp-base

将 css 文件里引用的图片转为 base64。

```javascript
var base64 = require('gulp-base64');

gulp.src('./css/*.css')
    .pipe(base64({
        maxImageSize: 8*1024,               // 只转 8kb 以下的图片为 base64
    }))
    .pipe(gulp.dest('./dist'))
```

### 7.5. JavaScript 处理

#### 7.5.1. 风格检查：gulp-eslint

规则文档：https://eslint.org/docs/rules/

AlloyTeam ESLint 配置指南：http://www.alloyteam.com/2017/08/13065/

#### 7.5.2. 使用 ES：gulp-babel

https://www.npmjs.com/package/gulp-babel

我们可以用 gulpfile.babel.js 代替 gulpfile.js（直接重命名即可），这样可以借助 babel 使用 ES6 新特性，不过我们也需要在项目中安装 babel：
```
	$ npm install --save-dev babel-core
	$ npm install --save-dev babel-register
	$ npm install --save-dev babel-preset-latest
```
在项目根目录下创建。babelrc 文件，内容如下：
```
	{
		"presets": ["latest"]
	}
```
这样，就可以在 gulpfile 中使用 ES6 代码。

#### 7.5.3. 压缩混淆：gulp-uglify

### 7.6. HTML 处理

#### 7.6.1. 风格检查：gulp-htmlhint

#### 7.6.2. 压缩：gulp-html-minify

### 7.7. 其它

#### 7.7.1. open

#### 7.7.2. gulp-connect

#### 7.7.3. gulp-debug

https://www.npmjs.com/package/gulp-debug

```javascript
const gulp = require('gulp');
const debug = require('gulp-debug');
 
gulp.task('default', () =>
    gulp.src('foo.js')
        .pipe(debug({title: 'unicorn:'}))
        .pipe(gulp.dest('dist'))
);
```

#### 7.7.4. 显示你的项目的大小：gulp-size

#### 7.7.5. 仅让发生改变的文件通过：gulp-changed

## 8. Refer Links

官方文档：https://github.com/gulpjs/gulp/blob/master/docs/API.md   

中文文档：http://www.gulpjs.com.cn/docs/    

gulp 入门教程：http://www.tangshuang.net/3126.html   

gulp 资料合集：https://github.com/Platform-CUF/use-gulp   

慕课网 gulp in action：http://www.imooc.com/video/5692   