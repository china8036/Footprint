- [Maven Note](#maven-note)
  - [概念](#%E6%A6%82%E5%BF%B5)
  - [maven标准目录结构](#maven%E6%A0%87%E5%87%86%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)
  - [依赖坐标](#%E4%BE%9D%E8%B5%96%E5%9D%90%E6%A0%87)
  - [仓库](#%E4%BB%93%E5%BA%93)
  - [命令](#%E5%91%BD%E4%BB%A4)
  - [生命周期](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [pom.xml](#pomxml)
  - [依赖范围](#%E4%BE%9D%E8%B5%96%E8%8C%83%E5%9B%B4)
  - [依赖传递](#%E4%BE%9D%E8%B5%96%E4%BC%A0%E9%80%92)
  - [聚合和继承](#%E8%81%9A%E5%90%88%E5%92%8C%E7%BB%A7%E6%89%BF)

# Maven Note

## 概念

Apache Maven，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。

**依赖查询**：https://mvnrepository.com 



## maven标准目录结构

https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html 

https://www.cnblogs.com/now-fighting/p/4858982.html 

```
----src                               	// 包含构建项目的所有源材料
|   |----main                          	// 主构建工件
|   |   |----java                      // 应用/库的源文件
|   |   |----resources                 // 应用/库的资源
|   |   |----resources-filtered        // 应用/库中被过滤的资源
|   |   |----filters                   	// 资源过滤器文件
|   |   |----webapp                    // Web 应用源文件
|   |----test                          // 单元测试代码和资源
|   |   |----java                      	// 测试用源文件
|   |   |----resources                 // 测试用资源
|   |   |----resources-filtered        // 测验用被过滤的资源
|   |   |----filters                  	// 测试用过滤器文件
|   |----it                            // 集成测试（主要用于插件）
|   |----assembly                      // 装配描述符
|   |----site                          // 站点文件
|   |----...
|----target                            // 存放项目构建过程中的所有输出
|----pom.xml                           // Maven 核心描述文件
|----LICENSE.txt                       // 项目许可说明
|----NOTICE.txt                        // 项目所依赖的库所要求的条款和授予
|----README.txt                        // 项目的自述文件
|----...

```


## 依赖坐标

1)	GroupID：项目组织唯一的标识符，实际对应JAVA的包的结构，即main目录里java的目录结构，即公司网址反写+项目名；

2)	ArtifactID：项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称，即项目名+“-”+模块名；（java包名应与pom.xml中定义的groupID与artifactID相吻合）

3)	version：构建版本；

例：      
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/7ae73481bc4eb8339d76dfdcb4e9cdfc.jpg)




## 仓库

- 本地仓库

- 远程仓库（maven中央仓库）
  在本地仓库中找不到时就会去远程中央仓库中查找依赖；

  在maven根目录下的lib文件夹中，用解压软件打开maven-model-builder-xxx.jar，可以找到pom-4.0.0.xml，这是maven的超级pom文件，所有pom文件都会继承于该文件，在该文件中可以看到maven的远程中央仓库地址如下：    
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/2d5a99f0f5d9b36f3d05c0c24a948062.jpg)

- 镜像仓库
  maven的中央仓库在国内访问速度不理想，因此可以配置maven使用国内的镜像仓库，加快maven项目的构建速度；
  在{maven根目录}/conf/settings.xml文件中，在<mirrors>标签体中添加镜像仓库地址；
  例：      
  使用阿里云的maven镜像仓库：
  ```
  <mirrors>
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>//*表示所有依赖都从镜像仓库中获取，不再使用中央仓库
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
  </mirrors>
  ```

  - 更改本地仓库地址
    maven从远程仓库中下载的jar包默认是安放在用户目录中（例如：C:\Users\wjq\.m2\repository）（包括使用mvn install命令安装的jar包也在该目录下），若要更改本地仓库地址，在settings.xml中，找到<localRepository>/path/to/local/repo</localRepository>，去掉注释，并将其中的路径改为新设的本地仓库路径即可（如：D:/repo，注意不能使用“\”）；


## 命令
```
mvn -v			//查看maven版本号
	compile		//编译maven项目
	clean		  //删除target文件夹
	install		//安装jar包到本地仓库中
	test		  //测试
	package		//打包
```
命令行创建目录的两种方式：

1)	mvn archetype:generate  //按照提示输入，构建项目目录

2)	mvn archetype:generate -DgroupID={组织名/公司网址的反写 + 项目名} -DartifactID={项目名 + “-” + 模块名} -Dversion={版本号} -Dpackage={代码所在的包名} 	//一次性输入，直接构建项目目录



## 生命周期

clean、compile、test、package、install；

完整的项目构建过程包括：      
清理、编译、测试、打包、集成测试、验证、部署；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/5abecf81202a765a5e298708d224a0ed.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/4940f2371233e7143de95c866239779e.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/7e1523ebcf89866f8f92a18454fe35bc.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/28/4310c965efe75a9edcc116d5084224ca.jpg)

## pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">//指定了当前pom的版本
<modelVersion>4.0.0</modelVersion>
<groupId>com.firejq.test</groupId>//公司网址+项目名
<artifactId>test_project</artifactId>//项目名+模块名
<packaging>war</packaging>//项目打包类型，默认是war（还可以是zip/pom）
<version>0.0.1-SNAPSHOT</version>
<name>test_project Maven Webapp</name>//项目描述名
<url>http://maven.apache.org</url>//项目地址
	<description></description>//项目描述信息
	<developers></developers>//开发者
	<licenses></licenses>//许可证信息（一般开源项目都会有）
	<organzation></organization>//组织者信息
	

  	<dependencies>//依赖列表，标签体可包含多个依赖项
    		<dependency>//确定依赖所在坐标
      		<groupId>junit</groupId>
      		<artifactId>junit</artifactId>
      		<version>3.8.1</version>
      		<scope>test</scope>
/*
scope可为compile,provided,runtime,test,system,import；
compile是默认值，表示该依赖在依赖的生命周期（编译、测试、运行）内都有效；
provided：在编译和测试时有效；
runtime：在测试和运行时有效；
test：只在test下有效；
system：与本机系统相关联，可移植性差；
import：只可使用在dependencyManagement中，表示从其它的pom中导入dependency的配置；
			
*/
<type></type>
			<optional></optional>//设置依赖是否可选，默认为false
			<exclusions>//排除依赖传递列表
			<exclusion></exclusion>
</exclusions>
    		</dependency>
  	</dependencies>
	<dependencyManagement>//项目依赖管理
		<dependencies>
	<dependency></dependency>
</dependencies>
</dependencyManagement>
  	<build>//为构建行为提供支持
    		<finalName>test_project</finalName>
		<plugins>//插件列表
	<plugin>
	      		<groupId></groupId>
      			<artifactId></artifactId>
      			<version></version>

</plugin>
</plugins>
  	</build>

	<moudules></moudules>
</project>

```

## 依赖范围



## 依赖传递



## 聚合和继承