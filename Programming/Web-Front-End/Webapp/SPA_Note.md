<!-- toc -->

# 单页面应用 #
单页应用的数据流方案探索 https://github.com/xufei/blog/issues/47 

## 概念 ##
单页应用，指的是在一个页面上集成多种功能，甚至整个系统就只有一个页面，所有的业务功能都是它的子模块，通过特定的方式挂接到主界面上。它是AJAX技术的进一步升华，把AJAX的无刷新机制发挥到极致，因此能造就与桌面程序媲美的流畅用户体验。

- 优势：
	- 避免了页面跳转，操作体验流畅，媲美本地应用的感觉，切换过程中不会频繁有被“打断”的感觉；
	- 因为界面框架都在本地，与服务端的通讯基本只有数据，所以便于迁移，可以用比较小的代价，迁移成桌面产品，或者各种移动端Hybrid产品；

- 缺点：
	- 对搜索引擎不友好；
	- 开发难度相对较高；
	如何尽可能增强单页应用的操作体验？
	https://github.com/xufei/blog/issues/35 
	- 路由的规划
	- 推送的使用
	- 断线重连机制
	- 操作补偿机制
	- 本地缓存
	- 热更新
	- 良好的内存管理
	- 服务端预渲染



## 使用 AngularJS 配置 ##





## 存在问题 ##

### 加载 ###



### SEO ###

- 单页面App上的SEO
	- 理想情况下，此技术可能会被用在有用户登录后的应用程序中。你当然不会真的想要特定用户私人性质的页面被搜索引擎索引. 例如，你不会想要你的读者账户，Facebook登录的页面或者博客CMS页面被索引到.
	- 如果你确实像针对你的应用进行SEO，那么如何让SEO在使用js构建页面的应用/站点上起效呢? 搜索引擎难于处理这些应用程序因为内容是由浏览器动态构建的，而且对爬虫是不可见的.

- 让你的应用对SEO友好
使得js单页面应用对SEO友好的技术需要定期做维护. 根据官方的 [Google 建议](https://developers.google.com/webmasters/ajax-crawling/) , 你需要创建HTML快照. 其如何运作的概述如下:
	- 爬虫会发现一个友好的URL(http://scotch.io/seofriendly#key=value)
	- 然后爬虫会想服务器请求对应这个URL的内容(用一种特殊的修改过的方式)
	- Web服务器会使用一个HTML快照返回内容
	- HTML快照会被爬虫处理
	- 然后搜索结果会显示原来的URL
更多关于这个过程的信息，可以去看看Google的 [AJAX爬虫](https://developers.google.com/webmasters/ajax-crawling/docs/getting-started)  和他们有关 [创建HTML快照的指南](https://developers.google.com/webmasters/ajax-crawling/docs/html-snapshot).






