- [Serverless 架构](#Serverless-架构)
  - [1. 发展历史](#1-发展历史)
- [云服务模型](#云服务模型)
  - [2. IaaS (Infrastructure-as-a-service，基础设施服务)](#2-IaaS-Infrastructure-as-a-service基础设施服务)
  - [3. PaaS (Platform-as-a-service，平台服务)](#3-PaaS-Platform-as-a-service平台服务)
  - [4. SaaS (Software-as-a-service，软件服务)](#4-SaaS-Software-as-a-service软件服务)
  - [5. Content as a service](#5-Content-as-a-service)
  - [6. Software as a service](#6-Software-as-a-service)
  - [7. Platform as a service](#7-Platform-as-a-service)
  - [8. Function as a service](#8-Function-as-a-service)
  - [9. Infrastructure as a service](#9-Infrastructure-as-a-service)
  - [10. Desktop as a service](#10-Desktop-as-a-service)
  - [11. Data as a service](#11-Data-as-a-service)
  - [12. Mobile backend as a service](#12-Mobile-backend-as-a-service)
  - [13. Network as a service](#13-Network-as-a-service)
  - [14. Security as a service](#14-Security-as-a-service)
  - [15. Refer Links](#15-Refer-Links)

# Serverless 架构

大多数公司在开发应用程序并将其部署在服务器上的时候，无论是选择公有云还是私有的数据中心，都需要提前了解究竟需要多少台服务器、多大容量的存储和数据库的功能等。并需要部署运行应用程序和依赖的软件到基础设施之上。假设我们不想在这些细节上花费精力，是否有一种简单的架构模型能够满足我们这种想法？这个答案已经存在，这就是今天软件架构世界中新鲜但是很热门的一个话题——Serverless（无服务器）架构。

Serveless 架构试图帮助开发者摆脱运行后端应用程序所需的服务器设备的设置和管理工作。这项技术的目标并不是为了实现真正意义上的“无服务器”，而是指由第三方云计算供应商负责后端基础结构的维护，以服务的方式为开发者提供所需功能，例如数据库、消息，以及身份验证等。简单地说，这个架构的就是要让开发人员关注代码的运行而不需要管理任何的基础设施。程序代码被部署在诸如 AWS Lambda 这样的平台之上，通过事件驱动的方法去触发对函数的调用。很明显，这是一种完全针对程序员的架构技术。其技术特点包括了事件驱动的调用方式，以及有一定限制的程序运行方式，例如 AWS Lambda 的函数的运行时间默认为 3 秒到 5 分钟。从这种架构技术出现的两年多时间来看，这个技术已经有了非常广泛的应用，例如移动应用的后端和物联网应用等。简而言之，无服务器架构的出现不是为了取代传统的应用。然而，从具有高度灵活性的使用模式及事件驱动的特点出发，开发人员／架构师应该重视这个新的计算范例，它可以帮助我们达到减少部署、提高扩展性并减少代码后面的基础设施的维护负担。

Serverless 这个概念并不容易理解。乍见之下，很容易让人混淆硬件服务器及软件上的服务与其所谓的“服务器”差别。在这里强调的**所谓“无服务器”指的是我们的代码不会明确地部署在某些特定的软件或者硬件的服务器上**，运行代码托管的环境是由例如 AWS 这样的云计算厂商所提供的。

在 Serverless 的世界里面，服务供应商扮演了一个非常重要的角色。常见厂商有 AWS、Google Cloud Functions、Microsoft Azure Functions、IBM OpenWhisk、Iron.io 和 Webtask 等，这些开源平台都提供了类似的服务。

## 1. 发展历史

1. Serverless 这个词第一次被使用大约是 2012 年由 Ken Form 所写的一篇名为《Why The Future of Software and Apps is Serverless》的文章。这篇文章谈到的内容是关于持续集成及源代码控制等内容，并不是我们今天所特指的这一种架构模式。
1. Amazon 在 2014 年发布的 AWS Lambda 让“Serverless”这一范式提高到一个全新的层面，为云中运行的应用程序提供了一种全新的系统体系结构。至此再也不需要在服务器上持续运行进程以等待 HTTP 请求或 API 调用，而是可以通过某种事件机制触发代码的执行，通常这只需要在 AWS 的某台服务器上配置一个简单的功能。
1. Ant Stanley 在 2015 年 7 月的名为《Server are Dead…》的文章中更是围绕着 AWS Lambda 及刚刚发布的 AWS API Gateway 这两个服务解释了他心目中的 Serverless，“Server are dead…they just don’t know it yet”。到了 2015 年 10 月份，在那一年的 AWS re:Invent 大会上，Serverless 的这个概念更是反复出现在了很多场合。印象中就包括了“（ARC308）The Serverless Company Using AWS Lambda”及“（DVO209）JAWS: The Monstrously Scalable Serverless Framework”这些演讲当中。随着这个概念的进一步发酵，2016 年 10 月在伦敦举办了第一届的 Serverlessvconf。在两天时间里面，来自全世界 40 多位演讲嘉宾为开发者分享了关于这个领域进展。

# 云服务模型

云服务只是一个统称，可以分成三大类。

![image](http://img.cdn.firejq.com/jpg/2019/6/21/b7d82f0d562b450de15013a5cdd65363.jpg)

![image](http://img.cdn.firejq.com/jpg/2019/6/21/b88f3f6c870b06523e8bc5c33fb3cb4c.jpg)

## 2. IaaS (Infrastructure-as-a-service，基础设施服务)

> Infrastructure as a service (IaaS) are online services that provide high-level APIs used to dereference various low-level details of underlying network infrastructure like physical computing resources, location, data partitioning, scaling, security, backup etc. A hypervisor, such as Xen, Oracle VirtualBox, Oracle VM, KVM, VMware ESX/ESXi, or Hyper-V, LXD, runs the virtual machines as guests. Pools of hypervisors within the cloud operational system can support large numbers of virtual machines and the ability to scale services up and down according to customers' varying requirements.

IaaS 是云服务的最底层，主要提供一些基础资源。它与 PaaS 的区别是，用户需要自己控制底层，实现基础设施的使用逻辑。例如：Amazon EC2、Digital Ocean、RackSpace Cloud等。

## 3. PaaS (Platform-as-a-service，平台服务)

> Platform as a Service (PaaS) or Application Platform as a Service (aPaaS) or platform-based service is a category of cloud computing services that provides a platform allowing customers to develop, run, and manage applications without the complexity of building and maintaining the infrastructure typically associated with developing and launching an app.

PaaS 提供软件部署平台（runtime），抽象掉了硬件和操作系统细节，可以无缝地扩展（scaling）。开发者只需要关注自己的业务逻辑，不需要关注底层。例如：Heroku、Google App Engine、OpenShift等。

TODO:

PaaS 就是把计算能力放在线上，你只管写代码就行了，目的也是为了减少后端维护的成本，让开发者更关注到开发本身。国内有 Sina App Engine，国外有 Heroku、Google App Engine、Amazon Web Services，但是这类服务被真正用来做产品的并不多，大多是当作开发的试验田跑一下，而且跑起来的成本比独立部署个服务器也差不多，你要理解很多服务的相关性，应用运行时还有提供各种服务的桥接，就造成你需要去理解一大堆东西才能把他们五花大绑到一起，所以这类服务并没有成为真正的主流，更多的是还是用原生的计算能力，比如 Amazone EC2、AWS 这类 IaaS 平台，国内的阿里云、UCloud 等。




## 4. SaaS (Software-as-a-service，软件服务)

> Software as a service (SaaS) is a software licensing and delivery model in which software is licensed on a subscription basis and is centrally hosted. It is sometimes referred to as "on-demand software", and was formerly referred to as "software plus services" by Microsoft. SaaS is typically accessed by users using a thin client, e.g. via a web browser. SaaS has become a common delivery model for many business applications, including office software, messaging software, payroll processing software, DBMS software, management software, CAD software, development software, gamification, virtualization, accounting, collaboration, customer relationship management (CRM), Management Information Systems (MIS), enterprise resource planning (ERP), invoicing, human resource management (HRM), talent acquisition, learning management systems, content management (CM), Geographic Information Systems (GIS), and service desk management. SaaS has been incorporated into the strategy of nearly all leading enterprise software companies.

SaaS 是软件的开发、管理、部署都交给第三方，不需要关心技术问题，可以拿来即用。普通用户接触到的互联网服务，几乎都是 SaaS，例如：客户管理服务 Salesforce、团队协同服务 Google Apps、储存服务 Box、储存服务 Dropbox、社交服务 Facebook / Twitter / Instagram等。


## 5. Content as a service

Content as a service (CaaS) or managed content as a service (MCaaS) is a service-oriented model, where the service provider delivers the content on demand to the service consumer via web services that are licensed under subscription. The content is hosted by the service provider centrally in the cloud and offered to a number of consumers that need the content delivered into any applications or system, hence content can be demanded by the consumers as and when required.

Content as a Service is a way to provide raw content (in other words, without the need for a specific human compatible representation, such as HTML) in a way that other systems can make use of it. Content as a Service is not meant for direct human consumption, but rather for other platforms to consume and make use of the content according to their particular needs. This happens usually on the cloud, with a centralized platform which can be globally accessible and provides a standard format for your content. With Content as a Service, you centralize your content into a single repository, where you can manage it, categorize it, make it available to others, search for it, or do whatever you wish with it.

## 6. Software as a service

## 7. Platform as a service

## 8. Function as a service

Function as a service (FaaS) is a category of cloud computing services that provides a platform allowing customers to develop, run, and manage application functionalities without the complexity of building and maintaining the infrastructure typically associated with developing and launching an app. Building an application following this model is one way of achieving a "serverless" architecture, and is typically used when building microservices applications.

FaaS was initially offered by various start-ups circa 2010, such as PiCloud.

AWS Lambda, was the first FaaS offering by a large public cloud vendor, followed by Google Cloud Functions, Microsoft Azure Functions, IBM/Apache's OpenWhisk (open source) in 2016 and Oracle Cloud Fn (open source) in 2017.

CF:

A similar concept to FaaS are platform as a service (PaaS) application hosting services, in that such application hosting services also hide "servers" from developers. However, such hosting services typically always have at least one server process running that receives external requests. Scaling is achieved by booting up more server processes, which the developer is typically charged directly for. Consequently, scalability remains visible to the developer.[5]

FaaS by contrast does not require any server process constantly being run. While an initial request may take longer to be handled than an application hosting platform (up to several seconds[6]), caching may enable subsequent requests to be handled within milliseconds. As developers only pay for function execution time (and no process idle time), lower costs at higher scalability can be achieved (at the cost of latency).

## 9. Infrastructure as a service

## 10. Desktop as a service

Remote desktop virtualization can also be provided via cloud computing similar to that provided using a software as a service model. This approach is usually referred to as Cloud Hosted Virtual Desktops. Cloud Hosted Virtual Desktops are divided into two technologies: (1) Managed VDI, which is based on VDI technology provided as an outsourced managed service, and (2) Desktop-as-a-Service (DaaS), which provides a higher level of automation and real multi-tenancy, reducing the cost of the technology. The DaaS provider typically takes full responsibility for hosting and maintaining the computer, storage, and access infrastructure, as well as applications and application software licenses needed to provide the desktop service in return for a fixed monthly fee. For example, VMware's Horizon DaaS, based on VMware's acquisition of Desktone, is a monthly fixed rate DaaS service,[8] as is Amazon's WorkSpaces on Amazon EC2. Another example is Help Desk virtualization service, provided by the ProProfs[10].

Cloud-hosted virtual desktops can be implemented using both VDI and Remote Desktop Services-based systems and can be provided through the public cloud, private cloud infrastructure, and hybrid cloud platforms. Private cloud implementations are commonly referred to as "Managed VDI". Public Cloud offerings tend to be based on Desktop-as-a-Service technology.

The U.S. Company Desktone, which was acquired by VMware in October 2013, has the trademarks on the expressions "desktops as a service" and "DaaS" from the U.S. Patent and Trademark Office.

## 11. Data as a service

In computing, data as a service (or DaaS) is a cousin of software as a service (SaaS).[1] Like all members of the "as a service" (aaS) family, DaaS builds on the concept that the product (data in this case) can be provided on demand[2] to the user regardless of geographic or organizational separation of provider and consumer. Additionally, the emergence[when?] of service-oriented architecture (SOA) and the widespread use of application programming interface (API) has also rendered the actual platform on which the data resides irrelevant.[3] This development has enabled the emergence of the relatively new concept of DaaS.[citation needed]

Data provided as a service began primarily in Web mashups, but as of 2015 is being increasingly employed both commercially and also less commonly within organisations such as the UN.[4]

Traditionally, most organisations have used data stored in a self-contained repository, for which software was specifically developed to access and present the data in a human-readable form. One result of this paradigm is the bundling of both the data and the software needed to interpret it into a single package, sold as a consumer product. As the number of bundled software/data packages proliferated and required interaction among one another, another layer of interface was required. These interfaces, collectively known as enterprise application integration (EAI), often tended to encourage vendor lock-in, as it is generally easy to integrate applications that are built upon the same foundation technology.[5]

The result of the combined software/data consumer package and required EAI middleware has been an increased amount of software for organizations to manage and maintain, simply for the use of particular data. In addition to routine maintenance costs, a cascading amount of software updates are required as the format of the data changes. The existence of this situation contributes to the attractiveness of DaaS to data consumers, because it allows for the separation of data cost and of data usage from the cost of a specific software environment or platform. Sensing as a Service[6][7] (S2aaS) also shares a similar vision. S2aaS is presented as a business model that aims to use Internet of Things data to create data trading marketplaces.

More recently, Data as a Service solutions offered by leading vendors (Oracle, Microsoft) help organizations more rapidly ingest large volumes of data, integrate that data, analyze the data and publish the data to business users in real-time using Web service APIs that adhere to the REST architectural constraints (also known as RESTful API).

## 12. Mobile backend as a service

Mobile backend as a service (MBaaS), also known as "backend as a service" (BaaS),[1][2][3] is a model for providing web app and mobile app developers with a way to link their applications to backend cloud storage and APIs exposed by back end applications while also providing features such as user management, push notifications, and integration with social networking services.[4] These services are provided via the use of custom software development kits (SDKs) and application programming interfaces (APIs). BaaS is a relatively recent development in cloud computing,[5] with most BaaS startups dating from 2011 or later.[6][7][8] Although a fairly nascent industry, trends indicate that these services are gaining mainstream traction with enterprise consumers.

## 13. Network as a service

Network as a service (NaaS) describes services for network transport connectivity.[1] NaaS involves the optimization of resource allocations by considering network and computing resources as a unified whole.

## 14. Security as a service

Security as a service (SECaaS) is a business model in which a service provider integrates their security services into a corporate infrastructure on a subscription basis more cost effectively than most individuals or corporations can provide on their own, when total cost of ownership is considered.[1] SECaaS is inspired by the "software as a service" model as applied to information security type services and does not require on-premises hardware, avoiding substantial capital outlays [2][3]. These security services often include authentication, anti-virus, anti-malware/spyware, intrusion detection, Penetration testing[4] and security event management, among others.[5]

Outsourced security licensing and delivery is boasting a multibillion-dollar market.[6] SECaaS provides users with Internet security services providing protection from online threats and attacks such as DDoS that are constantly searching for access points to compromise websites.[7] As the demand and use of cloud computing skyrockets, users are more vulnerable to attacks due to accessing the Internet from new access points. SECaaS serves as a buffer against the most persistent online threats.


## 15. Refer Links

[从 IaaS 到 FaaS—— Serverless 架构的前世今生](https://amazonaws-china.com/cn/blogs/china/iaas-faas-serverless/)

[阮一峰：IaaS，PaaS，SaaS 的区别](https://www.ruanyifeng.com/blog/2017/07/iaas-paas-saas.html)

[知乎：怎么理解 IaaS、SaaS 和 PaaS 的区别？](https://www.zhihu.com/question/20387284)

[FaaS，未来的后端服务开发之道](https://www.jianshu.com/p/6e86c42f85bd)
