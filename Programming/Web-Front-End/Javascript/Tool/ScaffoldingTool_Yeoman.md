- [Scaffolding Tool: Yeoman](#scaffolding-tool-yeoman)
  - [1. 安装](#1-%E5%AE%89%E8%A3%85)
  - [2. 使用](#2-%E4%BD%BF%E7%94%A8)
    - [2.1. 安装 generators](#21-%E5%AE%89%E8%A3%85-generators)
    - [2.2. 使用 generators](#22-%E4%BD%BF%E7%94%A8-generators)
    - [2.3. 其它命令](#23-%E5%85%B6%E5%AE%83%E5%91%BD%E4%BB%A4)
  - [3. 自定义 Yeoman generators](#3-%E8%87%AA%E5%AE%9A%E4%B9%89-yeoman-generators)


# Scaffolding Tool: Yeoman #
http://yeoman.io/learning/index.html

Yeoman 是一款现代 webapp 的 SCAFFOLDING TOOL （脚手架工具），在 web 项目的立项阶段，使用 yeoman 来生成项目文件和代码结构，Yeoman 自动整合了最佳实践和工具，大大加速了后续的开发；

Yeoman is language agnostic. It can generate projects in any language (Web, Java, Python, C#, etc.)

## 1. 安装 ##
YeoMan 是一个 node.js 项目，首先需要使用 npm 进行安装：
```
npm install -g yo
```
验证安装成功：
```
yo --version
```

## 2. 使用 ##

Yeoman 的使用依赖于模板项目(generators)，每一个 generators 都是一个 node.js 项目，但是需要符合一定的规范，比如命名上必须得是 generator-XYZ 这种形式（其中 XYZ 即为项目名）；


### 2.1. 安装 generators ###

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

### 2.2. 使用 generators ###

- 使用 generators 快速构建项目结构；
```
yo <generator_name> [项目名]
```
例：使用 generator-webapp 构建一个项目结构（若省略项目名则以当前目录名作为项目名）：
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


### 2.3. 其它命令 ###
- `yo --help`：获取帮助信息；
- `yo --version`：获取版本信息；
- `yo --generators`：列出所有已安装的 generators；
- `yo doctor`：诊断问题并提供解决方案；


## 3. 自定义 Yeoman generators ##
http://imweb.io/topic/586766b5b3ce6d8e3f9f99b1

https://ivweb.io/topic/58d92e3fdb35a9135d42f840