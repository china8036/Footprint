- [前端开发工具](#%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7)
  - [目录结构](#%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)
  - [browser-sync](#browser-sync)
  - [http-server](#http-server)
  - [webpack](#webpack)

# 前端开发工具 #

## 目录结构 ##

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



## webpack ##