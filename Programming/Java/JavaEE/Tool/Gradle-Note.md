- [Gradle Note](#gradle-note)
    - [1. 介绍](#1-%E4%BB%8B%E7%BB%8D)
    - [2. 安装](#2-%E5%AE%89%E8%A3%85)
    - [3. 使用](#3-%E4%BD%BF%E7%94%A8)
        - [3.1. 基础](#31-%E5%9F%BA%E7%A1%80)
        - [3.2. 配置](#32-%E9%85%8D%E7%BD%AE)
        - [3.3. build.gradle](#33-buildgradle)
            - [3.3.1. 依赖管理](#331-%E4%BE%9D%E8%B5%96%E7%AE%A1%E7%90%86)
            - [3.3.2. 配置依赖仓库](#332-%E9%85%8D%E7%BD%AE%E4%BE%9D%E8%B5%96%E4%BB%93%E5%BA%93)
        - [3.4. 使用 Gradle Wrapper 构建项目](#34-%E4%BD%BF%E7%94%A8-gradle-wrapper-%E6%9E%84%E5%BB%BA%E9%A1%B9%E7%9B%AE)
        - [3.5. 使用镜像 maven 库](#35-%E4%BD%BF%E7%94%A8%E9%95%9C%E5%83%8F-maven-%E5%BA%93)
        - [3.6. 修改本地仓库位置](#36-%E4%BF%AE%E6%94%B9%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E4%BD%8D%E7%BD%AE)
        - [3.7. IDEA 中使用 Gradle](#37-idea-%E4%B8%AD%E4%BD%BF%E7%94%A8-gradle)
            - [3.7.1. IDEA 中三种 gradle 模式的区别](#371-idea-%E4%B8%AD%E4%B8%89%E7%A7%8D-gradle-%E6%A8%A1%E5%BC%8F%E7%9A%84%E5%8C%BA%E5%88%AB)
            - [3.7.2. Unindexed remote maven repositories](#372-unindexed-remote-maven-repositories)
        - [3.8. 优化 Gradle](#38-%E4%BC%98%E5%8C%96-gradle)
        - [3.9. 将 pom.xml 转化为 build.gradle](#39-%E5%B0%86-pomxml-%E8%BD%AC%E5%8C%96%E4%B8%BA-buildgradle)
    - [4. Refer Links](#4-refer-links)

# Gradle Note

## 1. 介绍

Gradle 是一种构建工具，它抛弃了基于 XML 的构建脚本，取而代之的是采用一种基于 Groovy 的 DSL（Domain Specific Language），即内部领域特定语言；

Gradle 构建工具是任务驱动型的构建工具，并且可以通过各种 Plugin 扩展功能以适应各种构建任务；

Gradle 采用约定优于配置的原则，最简单方式是使用一个默认的目录结构，默认目录结构与 maven 基本上完全相同，当然目录结构是可以自己修改的；

Gradle 基于 Groovy 语言开发，在安装包中集成了 Groovy 库；

## 2. 安装

最新版下载地址 https://gradle.org/releases/ 

安装 Gradle 前必须先安装 JDK；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/603f5dc8f1bd46545cde18dec092a523.jpg)

下载后解压，将 bin/ 目录添加至环境变量；

验证安装成功：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/6fa72c1fd3503910c60684780886a502.jpg)


将 `GRADLE_HOME` 和 `GRADLE_USER_HOME` 添加到环境变量中：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/c5dde8f563f1082aea0ed0442997dd64.jpg)

将 /bin 目录添加到环境变量的路径中：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/4f9148298ddc210a3d8e10202a30e107.jpg)


## 3. 使用

### 3.1. 基础

1)	Project

    构建产物（比如 Jar 包）或实施产物（将应用程序部署到生产环境），Project 由一些组件组成，如一个 Project 可以代表一个 JAR 库或者一个 WEB 应用程序，也可能包含其他项目生成的 JAR 包；

    每个 gradle build 由一到多个 Project 组成；

2)	Task

    不可分的最小工作单元，执行构建工作（比如编译项目或执行测试），Task 可以是编译一些 Java 类，或者创建一个 JAR 包，或者是生成 JavaDoc，或者是发布文档到仓库，Task 作为原子工作存在；

    每个 Project 由一到多个 Task 组成；

    在 Gradle 项目目录下，查看可用 tasks 列表：
    ```
    gradle task
    ```

3)	Plugin

    在 Gradle 中，所有有用的特性都是由 Plugin 来提供的。添加 Plugin 到 Gradle 中其实就是添加了一些新的 task，域对象（如 SourceSet)，约定（如 Java source 默认放在 src/main/java 下)，同时也会扩展一些已经存在的类型。 Plugin 分两种：脚本插件 apply from: 'other.gradle'和二进制插件 apply plugin: 'java'；

### 3.2. 配置

可以通过配置文件对 Gradle 构建进行配置

1)	Gradle 构建脚本（build.gradle）：指定了一个项目和它的任务。

2)	Gradle 属性文件（gradle.properties）：用来配置构建属性。

3)	Gradle 设置文件（gradle.settings）：对于只有一个项目的构建而言是可选的，如果我们的构建中包含多于一个项目，那么它就是必须的，因为它描述了哪一个项目参与构建。每一个多项目的构建都必须在项目结构的根目录中加入一个设置文件。

### 3.3. build.gradle

Gradle 有一个类似 Maven 中 pom.xml 的配置文件：build.gradle。功能也基本一样，负责当前 project 的构建定义。

#### 3.3.1. 依赖管理

Gradle 和 Maven 在依赖管理上几乎差不多，核心的概念是一样的，只不过 Gradle 语法更精简，并且多了一些更灵活的自定义配置。我们先看一个例子，Maven 的 pom.xml：
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
    </dependency>
</dependencies>
```
更换成 Gradle 脚本，结果是这样：
```
dependencies {
    compile('org.springframework:spring-core:3.2.4.RELEASE')
    compile('org.springframework:spring-beans:3.2.4.RELEASE')
    compile('org.springframework:spring-context:3.2.4.RELEASE')
    testCompile('junit:junit:4.7')
}
```

引用一个外部依赖需要指定使用的 group, name 和 version 属性，三者缺一不可。那从哪里得知 JAR 包的这三个属性呢？我们可以从 mvnrepository (https://mvnrepository.com) 中搜索到；

使用本地依赖
Gradle 也可以从本地目录中引入 JAR 包依赖，可以单一引入指定的某一 JAR 包，也可以引入某目录下所有的 JAR 包
```
dependencies {
	compile files('dir/file.jar')
	compile fileTree(dir: 'libs', include: '*.jar')
}
```

#### 3.3.2. 配置依赖仓库

```
repositories {
    mavenCentral()          // 定义仓库为 maven 中心仓库
}
repositories {
    jcenter()               // 定义仓库为 jcenter 仓库
}
repositories {
    maven {
        url "http://repo.mycompany.com/maven2"      // 定义依赖包协议是 maven，地址是公司的仓库地址
    }
}
repositories {                              // 定义本地仓库目录
    flatDir {
        dirs 'lib'
    }
}
repositories {                              // 定义 ivy 协议类型的仓库
    ivy {
        url "http://repo.mycompany.com/repo"
    }
}

```

NOTE：**由于网络问题，经过测试只有jcenter仓库速度较为稳定**。

### 3.4. 使用 Gradle Wrapper 构建项目

Gradle Wrapper 是开始一个 Gradle 构建的首选方式。它包含了 windows 批处理以及 OS X 和 Linux 的 Shell 脚本。这些脚本允许我们在没有安装 Gradle 的系统上执行 Gradle 构建。

要实现这个功能，我们需要在我们的 build.gradle 文件中增加以下代码：
```
task wrapper(type: Wrapper) {
	gradleVersion = '4.3.1'
}
```

### 3.5. 使用镜像 maven 库

https://yrom.net/blog/2015/02/07/change-gradle-maven-repo-url/ 

切换到国内的 Maven 镜像仓库，如：

- 开源中国的 Maven 库 http://maven.oschina.net/content/groups/public/ ；

- 阿里云的 maven 库 http://maven.aliyun.com/nexus/content/groups/public/ ；

- 自建的 Maven 私服；



针对单个项目：

修改项目根目录下的 build.gradle，将 jcenter() 或者 mavenCentral() 替换掉即可：
```
buildscript {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
    }
}

allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
    }
}
```

在 `GRADLE_USER_HOME/.gradle/` 中创建 init.gralde：
```
allprojects{
    repositories {
        def REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public/'
        all { ArtifactRepository repo ->
            if(repo instanceof MavenArtifactRepository){
                def url = repo.url.toString()
                if (url.startsWith('https://repo1.maven.org/maven2') || url.startsWith('https://jcenter.bintray.com/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $REPOSITORY_URL."
                    remove repo
                }
            }
        }
        maven {
            url REPOSITORY_URL
        }
    }
}
```
注：init.gradle 文件其实是 Gradle 的初始化脚本 (Initialization Scripts)，也是运行时的全局配置；

### 3.6. 修改本地仓库位置

http://blog.csdn.net/kl28978113/article/details/53018225 

将 C:Users/owner/.gradle 的默认目录复制到 xxxx/gradle_repo/.gradle，删除 C:Users/ower/.gradle，然后设置系统环境变量： 

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/aae83817483868a4571b774d9eeccdd9.jpg)

重启计算机后生效；

### 3.7. IDEA 中使用 Gradle

#### 3.7.1. IDEA 中三种 gradle 模式的区别

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/7d7e7dcb30d109bd397a6b8ae03ce597.jpg)

- [Using the default gradle wrapper（官方推荐）](https://github.com/RichardNi/mynote/blob/master/note/java/gradle/%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%BF%E7%94%A8idea%E5%AF%BC%E5%85%A5gradle%E5%B7%A5%E7%A8%8B%E5%BE%88%E6%85%A2.md)       

  由 Gradle 控制自身版本，使用的版本号可在 gradle/wrapper/gradle-wrapper.properties 中查看；

  会将 Gradle 及其版本文件存储在项目自身中（即项目目录下的 gradle 文件夹中），便于项目的迁移；

  - 修改默认 Gradle 版本：          
    在 build.gradle 文件最后加入：
    ```
    task wrapper(type: Wrapper) {
        gradleVersion = '4.1'
    }
    ```
    然后在 IDEA 中 ALT + F12 打开 terminal 执行
    ```
    gradlew wrapper
    ```
    gradle 就会下载指定版本的 gradle，并且项目的 gradle wrapper 也同步修改了；

  - 首次使用此选项创建项目时非常慢，这是因为 IDEA 需要下载默认版本的 Gradle，由于网络问题因此会卡很久，使用网络代理下载即可；

- Using the customizable gradle wrapper          
  由 IDEA 控制 Gradle 版本

- Use local gradle distribution          
  使用本地 Gradle；         
  不利于项目的迁移，若在别的环境中打开项目，很可能会发生 Gradle 的版本冲突等问题；

#### 3.7.2. Unindexed remote maven repositories

创建项目后出现下边的错误：        
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/644c6ee249567850e69050df3d090547.jpg)

解决方式一：

将 build.gradle 中的`mavenLocal()`都替换为`jcenter()`；

解决方式二：

将 build.gradle 中的 mavenLocal() 都替换为
```
maven {
    url("https://plugins.gradle.org/m2/")
}
```

### 3.8. 优化 Gradle

Gradle 有个 Daemon 配置，开启这个配置能有效的提高编译速度；

优点：

1)	不需要每次启动 gradle 进程（JVM 实例），减少了初始化相关的工作；

2)	daemon 可以缓存项目结构，文件，task 等，尽可能复用之前的编译成果，缩短编译过程；

操作：

在`.gradle` 目录下创建一个 gradle.properties 文件，再打开该文件在其中添加如下语句保存即可：
```
org.gradle.daemon=true
```

### 3.9. 将 pom.xml 转化为 build.gradle

复制 maven 项目到另外一个文件夹，执行
```
gradle init --type pom
```
即可生成 gradle 项目所需的一系列文件

参考：https://www.cnblogs.com/yjmyzz/p/gradle-to-maven.html 

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/29d67799e3787372ec34d6411447bc3a.jpg)

## 4. Refer Links

官网文档 https://gradle.org/docs/ 

官网教程 https://gradle.org/guides/#getting-started 

其它教程
https://waynell.github.io/2015/04/03/gradle-use-01/ 

https://tech.meituan.com/gradle-practice.html 

http://www.importnew.com/15881.html 
