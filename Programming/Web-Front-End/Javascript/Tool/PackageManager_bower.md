- [包管理工具：bower](#%E5%8C%85%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7%EF%BC%9Abower)
	- [1. 安装 bower](#1-%E5%AE%89%E8%A3%85-bower)
	- [2. 使用 bower](#2-%E4%BD%BF%E7%94%A8-bower)
		- [2.1. 安装包](#21-%E5%AE%89%E8%A3%85%E5%8C%85)
		- [2.2. 使用包](#22-%E4%BD%BF%E7%94%A8%E5%8C%85)
		- [2.3. 搜索包](#23-%E6%90%9C%E7%B4%A2%E5%8C%85)
		- [2.4. 更新包](#24-%E6%9B%B4%E6%96%B0%E5%8C%85)
		- [2.5. 卸载包](#25-%E5%8D%B8%E8%BD%BD%E5%8C%85)
		- [2.6. 其它](#26-%E5%85%B6%E5%AE%83)


# 包管理工具：bower #
https://segmentfault.com/a/1190000002971135   
http://javascript.ruanyifeng.com/tool/bower.html   


## 1. 安装 bower ##
```
npm install -g bower
```
验证安装成功：
```
bower --version
```

## 2. 使用 bower ##
安装的包会被放置在bower_compontents目录下（该目录会在bower运行的文件夹下自动被创建），也可以通过更改.bowerrc文件中的配置选项来改变改目录创建的路径；

### 2.1. 安装包 ###

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



### 2.2. 使用包 ###
- 基本使用
例：
在HTML文件中：
```html
<script src="bower_components/jquery/dist/jquery.min.js"></script>
```
- 搭配 grunt
<!--TODO-->

### 2.3. 搜索包 ###

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



### 2.4. 更新包 ###

- 基本方式
```
bower update <package>
```
- 使用 bower.json 更新
```
bower update
```


### 2.5. 卸载包 ###


```
bowser uninstall <package1> <package2> <package3> . . .
```


### 2.6. 其它 ###

- list
```
bower list
```
列出所有已安装的包


- .bowerrrc 文件
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/25/d159e6523defa3cc1edebd2dd7097142.jpg)