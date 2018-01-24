- [Tomcat 类加载机制](#tomcat-%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6)
  - [当 tomcat 启动时，会创建几种类加载器](#%E5%BD%93-tomcat-%E5%90%AF%E5%8A%A8%E6%97%B6%EF%BC%8C%E4%BC%9A%E5%88%9B%E5%BB%BA%E5%87%A0%E7%A7%8D%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8)
  - [当 web 应用需要用到某个类时，会按照以下顺序进行类加载](#%E5%BD%93-web-%E5%BA%94%E7%94%A8%E9%9C%80%E8%A6%81%E7%94%A8%E5%88%B0%E6%9F%90%E4%B8%AA%E7%B1%BB%E6%97%B6%EF%BC%8C%E4%BC%9A%E6%8C%89%E7%85%A7%E4%BB%A5%E4%B8%8B%E9%A1%BA%E5%BA%8F%E8%BF%9B%E8%A1%8C%E7%B1%BB%E5%8A%A0%E8%BD%BD)
  - [Refer Links](#refer-links)

# Tomcat 类加载机制

## 当 tomcat 启动时，会创建几种类加载器

- Bootstrap 引导类加载器：
  
  加载 JVM 启动所需的类；
  
  以及标准扩展类（位于 jre/lib/ext 下）；

- System 系统类加载器：
  
  加载 tomcat 启动的类，比如 bootstrap.jar，通常在 catalina.bat 或者 catalina.sh 中指定；
  
  位于 CATALINA_HOME/bin 下；

- Common 通用类加载器 
  
  加载 tomcat 使用以及应用通用的一些类；
  
  位于 CATALINA_HOME/lib 下，比如 servlet-api.jar；

- webapp 应用类加载器
  
  每个应用在部署后，都会创建一个唯一的类加载器；
  
  该类加载器会加载位于 WEB-INF/lib 下 jar 包中的 class 和 WEB-INF/classes 下的 class；

## 当 web 应用需要用到某个类时，会按照以下顺序进行类加载

1. 使用 bootstrap 引导类加载器加载；
1. 使用 system 系统类加载器加载；
1. 使用应用类加载器在 WEB-INF/classes 中加载；
1. 使用应用类加载器在 WEB-INF/lib 中加载；
1. 使用 common 类加载器在 CATALINA_HOME/lib 中加载；

当成功加载到所需的类后，不再往下寻找；若到最后一层仍找不到所需要的类，则抛出异常；

P.S. 这也就是为什么 WEB-INF/classes 中的类比 WEB-INF/lib 中的类有更高的优先级的原因。

## Refer Links

https://www.cnblogs.com/xing901022/p/4574961.html
