- [Webpack](#webpack)
  - [1. 概述](#1-%E6%A6%82%E8%BF%B0)
    - [1.1. 定义](#11-%E5%AE%9A%E4%B9%89)
    - [1.2. webpack 与 gulp、Browserify 的关系](#12-webpack-%E4%B8%8E-gulp%E3%80%81browserify-%E7%9A%84%E5%85%B3%E7%B3%BB)
  - [2. entry](#2-entry)
  - [3. output](#3-output)
  - [4. loader](#4-loader)
    - [4.1. 使用](#41-%E4%BD%BF%E7%94%A8)
  - [5. plugin](#5-plugin)
  - [6. 实践：构建一个 gulp 与 webpack 相配合的前端工作流](#6-%E5%AE%9E%E8%B7%B5%EF%BC%9A%E6%9E%84%E5%BB%BA%E4%B8%80%E4%B8%AA-gulp-%E4%B8%8E-webpack-%E7%9B%B8%E9%85%8D%E5%90%88%E7%9A%84%E5%89%8D%E7%AB%AF%E5%B7%A5%E4%BD%9C%E6%B5%81)
  - [7. Refer Links](#7-refer-links)

# Webpack

## 1. 概述

### 1.1. 定义

> 本质上，webpack 是一个现代 JavaScript 应用程序的模块打包器 (module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图 (dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

Webpack 的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack 将从这个文件开始找到你的项目的所有依赖文件，使用 loaders 处理它们，最后打包为一个（或多个）浏览器可识别的 JavaScript 文件。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/9/3cb68f2ed60b66aa68fae54f2381b648.jpg)

### 1.2. webpack 与 gulp、Browserify 的关系

参见 [Webpack、Browserify 和 Gulp 三者之间到底是怎样的关系？](https://www.zhihu.com/question/37020798)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/5/cbb566cf50f5513b6433fa28d6f70935.jpg)

- Gulp / Grunt 是一种工具，能够优化前端工作流程。比如自动刷新页面、combo、压缩 css、js、编译 less 等等。简单来说，就是使用 Gulp/Grunt，然后配置你需要的插件，就可以把以前需要手工做的事情让它帮你做了。

- seajs / requirejs /browserify / webpack 都是 JS 模块化的方案：
  - seajs / require : 是一种在线"编译" 模块的方案，相当于在页面上加载一个 CMD/AMD 解释器。这样浏览器就认识了 define、exports、module 这些东西。也就实现了模块化。
  - browserify / webpack : 是一个预编译模块的方案，相比于上面 ，这个方案更加智能。没用过 browserify，这里以 webpack 为例。首先，它是预编译的，不需要在浏览器中加载解释器。另外，你在本地直接写 JS，不管是 AMD / CMD / ES6 风格的模块化，它都能认识，并且编译成浏览器认识的 JS。

## 2. entry

入口起点 (entry point) 指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

## 3. output

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。你可以通过在配置中指定一个 output 字段，来配置这些处理过程。

## 4. loader

Loaders 是 webpack 提供的最激动人心的功能之一了。通过使用不同的 loader，webpack 有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如转换 scss 为 css，或者把下一代的 JS 文件（ES6，ES7) 转换为现代浏览器兼容的 JS 文件，对 React 的开发而言，合适的 Loaders 可以把 React 的中用到的 JSX 文件转换为 JS 文件。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图可以直接引用的 JavaScript 模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

在更高层面，在 webpack 的配置中 loader 有两个目标：
- 识别出应该被对应的 loader 进行转换的那些文件。（使用 test 属性)
- 转换这些文件，从而使其能够被添加到依赖图中（并且最终添加到 bundle 中）(use 属性)

### 4.1. 使用

Loaders 需要单独安装并且需要在 webpack.config.js 中的 modules 关键字下进行配置，Loaders 的配置包括以下几方面：
- test：一个用以匹配 loaders 所处理文件的拓展名的正则表达式（必须）
- loader：loader 的名称（必须）
- include/exclude: 手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- query：为 loaders 提供额外的设置选项（可选）

## 5. plugin

loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

Loaders 和 Plugins 常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders 是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

Webpack 有很多内置插件，同时也有很多第三方插件，可以让我们完成更加丰富的功能。

## 6. 实践：构建一个 gulp 与 webpack 相配合的前端工作流

https://segmentfault.com/a/1190000004249679

要构建这样一个工作流，首先要理清几个问题：

- 什么工作应该交给 gulp, 什么工作应该交给 webpack

  Webpack 概念很多，但搞清楚 entry，output 和 loader 三个关键点，基本上就可以解决简单的问题了，稍微复杂的场景主要包括对资源的合并处理、分拆处理、多次打包等，部分这样的问题可以使用插件辅助解决，但是 Webpack 的强大并不在文件处理，而是依赖分析，所以在流程操作特别复杂的情况，webpack 并不能胜任工作，往往会被作为 gulp 的一个 task，整体工作流交给 gulp 主导。

  webpack 只是一个模块打包器，所以，交予 webpack 处理的应该已是经过各种 lint 检查，各种编译处理的代码，而各种检查，各种预处理就应该交给 gulp 之流了，最后压缩代码应该要交给 webpack 最后打包时再去执行。

  围绕 gulp 打造前端工程化方案，同时引入 webpack 来管理模块化代码，大致分工如下：
  - gulp：处理 html 压缩 / 预处理 / 条件编译，图片压缩，精灵图自动合并等任务
  - webpack：管理模块化，构建 js/css。

- webpack 貌似支持增量更新，gulp 支持增量更新吗？（这个是我之前一直很纠结的问题)

  使用 [gulp-changed](https://github.com/sindresorhus/gulp-changed) 插件。

- 如何实现 livereload?

  之前开发时 live reload 都是交给 gulp 的，而现在 gulp 的构建任务并不是在任务链的最后端，由 gulp 来实现显然不再合适。

## 7. Refer Links

[webpack 中文文档](https://doc.webpack-china.org/concepts/)

[入门 Webpack，看这篇就够了](https://segmentfault.com/a/1190000006178770)

[gulp + webpack 构建多页面前端项目](https://github.com/fwon/blog/issues/17)

[gulp & webpack 整合，鱼与熊掌我都要](http://www.jianshu.com/p/9724c47b406c)