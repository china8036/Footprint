

<!-- toc -->

* [包管理工具：npm](#包管理工具npm)
  * [概述](#概述)
  * [安装](#安装)
  * [使用](#使用)
    * [安装包](#安装包)
    * [卸载包](#卸载包)
    * [更新包](#更新包)
    * [搜索包](#搜索包)
  * [其它命令](#其它命令)
    * [查看帮助](#查看帮助)
    * [查看包信息](#查看包信息)
  * [配置](#配置)
    * [编辑配置文件](#编辑配置文件)
    * [配置proxy](#配置proxy)
    * [package.json](#packagejson)

<!-- toc stop -->



# 包管理工具：npm #
http://javascript.ruanyifeng.com/nodejs/npm.html    
http://www.runoob.com/nodejs/nodejs-npm.html    

## 概述 ##
NPM（node package manager），通常称为node包管理器，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。NPM依赖于couchdb的一个数据库，其中详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个很重要的作用就是：将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。


## 安装 ##
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

- 配置 npm 使用淘宝镜像    
	运行 
	```
	npm config set registry https://registry.npm.taobao.org
	```
	然后 编辑 ~/.npmrc 加入下面内容
	```
	registry=https://registry.npm.taobao.org
	sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
	phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
	ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
	```

## 使用 ##

### 安装包 ###

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


### 卸载包 ###
```
npm uninstall package
```
卸载指定版本：
```
npm uninstall package@<version>
```

### 更新包 ###

```
npm update package
```

### 搜索包 ###
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

## 配置 ##
npm的配置工作主要是通过 `npm config` 命令。

### 编辑配置文件 ###
```
npm config edit
```

### 配置proxy ###

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


### package.json ###
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