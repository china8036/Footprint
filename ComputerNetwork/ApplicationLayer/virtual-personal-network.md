- [虚拟专用网技术 (VPN)](#%E8%99%9A%E6%8B%9F%E4%B8%93%E7%94%A8%E7%BD%91%E6%8A%80%E6%9C%AF-vpn)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [需求背景](#%E9%9C%80%E6%B1%82%E8%83%8C%E6%99%AF)
    - [定义](#%E5%AE%9A%E4%B9%89)
    - [基本功能](#%E5%9F%BA%E6%9C%AC%E5%8A%9F%E8%83%BD)
    - [优点](#%E4%BC%98%E7%82%B9)
    - [类型](#%E7%B1%BB%E5%9E%8B)
  - [关键技术：隧道](#%E5%85%B3%E9%94%AE%E6%8A%80%E6%9C%AF%EF%BC%9A%E9%9A%A7%E9%81%93)
    - [隧道协议](#%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)
      - [概述](#%E6%A6%82%E8%BF%B0)
        - [定义](#%E5%AE%9A%E4%B9%89)
        - [组成](#%E7%BB%84%E6%88%90)
        - [常见隧道协议](#%E5%B8%B8%E8%A7%81%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)
      - [第二层隧道协议](#%E7%AC%AC%E4%BA%8C%E5%B1%82%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)
        - [PPTP(Point-to-Point Tunnel Protocol)](#pptppoint-to-point-tunnel-protocol)
        - [L2TP(Layer Two Tunneling Protocol)](#l2tplayer-two-tunneling-protocol)
      - [第三层隧道协议](#%E7%AC%AC%E4%B8%89%E5%B1%82%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)
        - [GRE](#gre)
        - [IPSec](#ipsec)
      - [第四层隧道协议：SSL VPN](#%E7%AC%AC%E5%9B%9B%E5%B1%82%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE%EF%BC%9Assl-vpn)

# 虚拟专用网技术 (VPN)

## 概述

### 需求背景

- 出差人员需要随时随地访问单位的 Intranet 获得信息。 

- 分布在各地的下属分支机构需要与总部的 Intranet 连接，互通信息。 

- 合作伙伴、产品供应商等需要与企业的 Intranet 连接，互通信息。 

通过租用专线、建立拨号服务等方式可以解决这种需求，但费用昂贵，而且扩展性不好，不能很好地满足机构规模扩大等的需要。

### 定义

虚拟专用网 (Virtual Private Network, VPN) 指通过在公用网络（如 Internet 等）中建立一条安全、专用的虚拟通道，连接异地的两个网络，构成逻辑上的虚拟子网。 

- VPN 依靠 ISP（Internet 服务提供商）或其他 NSP（网络服务提供商），在公用网络中建立专用的数据通信网络的技术。 

- 通过 VPN 从异地连接到机构的 Intranet，就像在本地 Intranet 上一样。

- 为了保障信息的安全，VPN 技术采用了鉴别、访问控制、保密性、完整性等措施，以防止信息被泄露、篡改和复制。

### 基本功能

- 加密数据：以保证通过公网传输的信息即使被他人截获也不会泄露。

- 信息验证和身份识别：保证信息的完整性，并能鉴别用户的身份。 

- 提供访问控制：不同的用户有不同的访问权限。 

- 地址管理：VPN 方案必须能够为用户分配专用网络上的地址并确保地址的安全性。 

- 密钥管理：VPN 方案必须能够生成并更新客户端和服务器的加密密钥。 

- 多协议支持：VPN 方案必须支持公共因特网络上普遍使用的基本协议，包括 IP、IPX 等。

### 优点

- 安全可靠 
  
  VPN 对通信数据进行了加密、认证，有效保证了数据通过公用网络传输时的安全性，保证数据不会被未授权的人员窃听和篡改。 

- 易于部署 

  - VPN 只需要在节点部署 VPN 设备，然后通过公用网络建立起犹如置身于内部网络的安全连接。
  - 如果要与新的网络建立 VPN 通信，只需增加 VPN 设备，改变相关配置即可。
  - 与专线连接相比较，特别是在需要安全连接的网络越来越多时，VPN 的实施就要简单很多。

- 成本低廉

  VPN 通过公共网络建立安全连接，只需一次性投入 VPN 设备，价格也比较便宜，节约了通信成本。

### 类型

1. 按应用范围划分 
    
    - 远程访问 VPN（Access VPN） 
    
      适用于企业内部人员流动频繁或远程办公的情况：出差员工或者在家办公的员工利用当地 ISP，通过拨号、ISDN、ADSL 等连接互联网，就可以和企业的 VPN 网关建立私有的隧道连接。

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/26/c2eaa91c31b84cf83c608e926126cdf7.jpg)

    - 企业内部 VPN（Intranet VPN） 
    
      是一种网关对网关 VPN：应用于企业内部两个或多个异地网络的互联，实施一样的安全策略。Intranet 加密的 VPN 隧道，两个异地网络通过 VPN 安全隧道进行通信，在一个局域网中访问异地的另一个局域网时，如同在本地网络一样。

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/26/0de2a138d8c0bade203b34ba262c2c20.jpg)

    - 企业外部 VPN（Extranet VPN）

      应用于企业网与合作者、客户等网络的互连，也是一种网关对网关的 VPN。与 Intranet VPN 不同的是，它需要在不同企业的内部网络之间组建，需要应用不同的协议，对不同的网络要有不同的安全策略。

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/26/2eae95e2e3e391957c05d0aec58a70e3.jpg)

2. 按 VPN 网络结构划分 

    - 基于 VPN 的远程访问：即单机连接到网络，又称点到站点、桌面到网络。用于提供远程移动用户对公司内部网的安全访问。 
    
    - 基于 VPN 的网络互联：即网络连接到网络，又称站点到站点、网关（路由器）到网关（路由器）或网络到网络。用于企业总部网络和分支机构网络的内部主机之间的安全通信。 
    
    - 基于 VPN 点对点通信：即单机到单机，又称端对端。用于企业内部网的两台主机之间的安全通信。

3. 按接入方式划分 

    - 专线 VPN：通过固定的线路连接到 ISP，如 DDN、帧中继等都是专线连接。 
    
    - 拨号接入 VPN(VPDN)：使用拨号连接（如 ISDN 和 ADSL 等）连接到 ISP，是典型的按需连接方式。这是一种非固定线路的 VPN。

4. 按隧道协议划分 
 
    - 第二层隧道协议 VPN：PPTP VPN 、L2TP VPN 
    
    - 第三层隧道协议 VPN：IPSec VPN 
    
    - 第四层隧道协议 VPN：SSL VPN

5. 按隧道建立方式划分

    - 自愿隧道

      以客户端计算机或路由器为端点的隧道，使用隧道客户软件发送 VPN 请求来创建。每个客户都会创建一条单独的隧道。 、

      为建立自愿隧道，需要以下两个条件：
      - 用户端计算机或路由器上必须安装适当的隧道协议软件 
      - 需要有一条 IP 连接（通过局域网或拨号网络）

    - 强制隧道

      用户端的计算机不作为隧道端点，而是由远程接入服务器 NAS（Network Access Server）作为隧道的一个端点。 
      
      NAS 和隧道服务器之间建立的隧道可以被多个客户共享，不必为每个客户建立一条新的隧道。

      用户端计算机中不需要安装隧道软件，仅在 NAS 中安装适当的隧道协议软件。

## 关键技术：隧道

隧道技术：通过对数据进行封装，在公共网络上建立一条数据通道（隧道），让数据包通过这条隧道传输，即将一种协议数据封装在另一种协议中传输，以保证被封装协议数据的安全传输。

使用隧道的原因是在不兼容的网路上传输数据，或在不安全网路上提供一个安全路径。

传输过程：
1. 在源节点，按隧道协议对数据进行封装。
2. 新的包头提供了路由信息，从而使封装的负载数据能够通过公共网络（如 Internet）传递。
3. 被封装的数据包一旦达到网络终点，数据将被解包并送达主机。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/9eb553802874997101779fe2cc0943a9.jpg)

### 隧道协议

#### 概述

##### 定义

隧道协议：封装数据、管理隧道的通信标准。
- 第二层隧道协议：在数据链路层进行隧道处理（数据封装），主要包括有 L2F、PPTP、L2TP 等，其中 L2TP 是目前的 IETF 标准。 
- 第三层隧道协议：在网络层进行隧道处理（数据封装），主要包括 GRE 和 IPSEC。 
第二层隧道和第三层隧道的本质区别在于：在隧道里传输的用户数据包是哪一层的数据包。

##### 组成

无论何种隧道协议，其数据包格式都是由乘客协议、封装协议和传输协议 3 部分组成的。 
- 乘客协议

  指用户要传输的数据，也就是被封装的数据，它们可以是 IP、PPP、SLIP 等。这是用户真正要传输的数据，如果是 IP 协议，其中包含的地址有可能是保留 IP 地址。

- 封装协议

  用于建立、保持和拆卸隧道，如 L2F、PPTP、L2TP 属于封装协议。 

- 传输协议

  乘客协议被封装之后应用传输协议，以使数据包顺利通过包交换网络（如 Internet），抵达目的地，如：用 TCP 协议对 PPTP 协议数据包进行传输，用 UDP 协议对 L2TP 协议数据包进行传输。

##### 常见隧道协议

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/9aee19cf6918510d7e9e62ad744582ca.jpg)

（来自 [wikipedia](https://zh.wikipedia.org/wiki/%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE#.E5.B8.B8.E8.A6.8B.E7.A9.BF.E9.9A.A7.E5.8D.94.E8.AD.B0))

#### 第二层隧道协议

工作原理：
- 将需要传输的协议包封装到 PPP 中。
- 再把新生成的 PPP 包封装到隧道协议包中。
- 然后通过第二层协议进行传输。

第二层隧道协议有 L2F、PPTP、L2TP 等，其中，PPTP 和 L2TP 集成在 windows 中，所以最常用，L2TP 是目前的 IETF 标准。主要应用于构建 Access VPN。 

##### PPTP(Point-to-Point Tunnel Protocol)

- 背景
  
  利用 PPP 协议，在企业网络内部需要架设一个拨号服务器作为 RAS（Remote Access Server），用户通过拨号到该 RAS 来访问企业内部网。 但通过这种方式，远程用户需要支付昂贵的长途拨号费用。因此希望能通过拨入当地的 ISP，进入 Internet，再连接企业的 VPN 网关，避免长途电话的费用，PPTP 正是为解决这一问题而引入的。 

- [定义](https://zh.wikipedia.org/wiki/%E9%BB%9E%E5%B0%8D%E9%BB%9E%E9%9A%A7%E9%81%93%E5%8D%94%E8%AD%B0)

  PPTP **使用传输控制协议（TCP）** 建立控制通道来传送控制命令，以及利用通用路由封装（GRE）通道来封装点对点协议（PPP）封包，在 IP 网络中传输资料。
  - PPTP 包封装在 IP 数据包内，通过 IP 网络进行传输，只要网络层是连通的，就可运行 PPTP 协议 
  - 使用一个 TCP 连接对隧道进行维护，提供了一种在 Internet 上建立多协议 VPN 的通信方式。 
  - 这个协议最早由微软等厂商主导开发，但因为它的**加密方式容易被破解，微软已经不再建议使用这个协议**。

- 工作原理

  PPTP 通信时，客户机和服务器间有 2 个通道：
  - 一个通道是 TCP 1723 端口的控制连接
  - 另一个通道用于传输数据，是传输 PPP 数据包的 IP 隧道

- 报文
  - 控制报文：用于 PPTP 隧道的建立、维护和断开，采用 TCP 控制。 

    PPTP 客户端“拨号”到 PPTP 服务器，创建 PPTP 隧道，即连接 PPTP 服务器的 TCP1723 端口建立控制连接。PPTP 控制连接数据包包括一个 IP 报头、一个 TCP 报头和 PPTP 控制信息：
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/6a6f5740ba85223b6e0580e64c963a4b.jpg)

  - 数据报文：先封装在 PPP 协议中，然后再用 GRE 协议（通用路由封装协议）封装成标准 IP 包，通过 IP 网络进行发送。

    在隧道建好之后，真正的用户数据经过加密和 / 或压缩之后，再依次经过 PPP、GRE、IP 的封装最终得到一个 IP 包，IP 包通过 IP 网络发送。其中，用户的数据可以是多种协议，比如 IP 数据包、IPX 数据包或者 NetBEUI 数据包。

    PPTP 服务器收到该 IP 后层层解包，得到真正的用户数据，并将用户数据转发到内部网络上。 

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/9e880343d10fcf51c72a67c14db952d1.jpg)

##### L2TP(Layer Two Tunneling Protocol)

- 组成
  
  L2TP 主要由 LAC（L2TP Access Concentrator，L2TP 接入集中器）和 LNS（L2TP Network Server，L2TP 网络服务器）构成：
  - LAC 用于发起呼叫，接收呼叫和建立隧道。
  - LNS 是所有隧道的终点 

- [定义](https://zh.wikipedia.org/wiki/%E7%AC%AC%E4%BA%8C%E5%B1%82%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)

  L2TP 协议自身不提供加密与可靠性验证的功能，可以和安全协议搭配使用，从而实现数据的加密传输。**通常会与加密协议 IPsec 搭配使用，合称 L2TP/IPsec**。

  L2TP 支持包括 IP、ATM、帧中继、X.25 在内的多种网络。在 IP 网络中，L2TP 协议使用注册端口 UDP 1701。因此，在某种意义上，**尽管 L2TP 协议的确是一个数据链路层协议，但在 IP 网络中，它又的确是一个会话层协议**。

- 报文

  **采用 UDP 协议**封装和传送 PPP 帧（PPP 帧的有效载荷即用户传输数据，可以经过加密和 / 或压缩）。 

  - 控制报文

    L2TP 控制报文用于隧道的建立与维护。

    由于 UDP 提供的是无连接的数据包服务，因此 L2TP 采用将报文序列化的方式来保证 L2TP 报文的按序递交。在 L2TP 控制报文中，Next Received 字段（类似于 TCP 中的确认字段）和 Next Sent 字段（类似于 TCP 中的序列号字段）用于维持控制报文的序列化，无序数据包将被丢弃。Next Received 字段和 Next Sent 字段同样用于用户传输数据的按序递交和流控制。

  - 数据报文

    L2TP 隧道数据报文和控制报文具有相同的包格式 ，L2TP 用户传输数据的隧道化过程采用多层封装方法。 

- 工作原理
  - 数据发送端处理过程：
  
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/4f465d7b2f95fb0cb8e501a7e6469b0b.jpg)

    - L2TP 封装。初始 PPP 有效载荷如 IP 数据报、IPX 数据报或 NetBEUI 帧等首先经过 PPP 报头和 L2TP 报头的封装。 
    
    - UDP 封装。L2TP 帧进一步添加 UDP 报头进行 UDP 封装，在 UDP 报头中，源端和目的端端口号均设置为 1701。 
    
    - IPSec 封装。基于 IPSec 安全策略，UDP 报文通过添加 IPSec ESP 报头、报尾和 IPSec 认证报尾 ，进行 IPSec 加密封装。 
    
    - IP 封装。在 IPSec 数据报外再添加 IP 报头进行 IP 封装，IP 报头中包含 VPN 客户机和服务器的源和目的端 IP 地址。 
    
    - 数据链路层封装。数据链路层封装是 L2TP 帧多层封装的最后一层，依据不同的物理网络再添加相应的数据链路层报头和报尾。

  - 数据接收端的处理过程：
    
    - 处理并去除数据链路层报头和报尾。 
    
    - 处理并去除 IP 报头。 
    
    - 用 IPSec ESP 认证报尾对 IP 有效载荷和 IPSec ESP 报头进行认证。 
    
    - 用 IPSec ESP 报头对数据报的加密部分进行解密 
    
    - 处理 UDP 报头并将数据报提交给 L2TP 协议。 
    
    - L2TP 协议依据 L2TP 报头信息分解出某条特定的 L2TP 隧道。 
    
    - 依据 PPP 报头分解出 PPP 有效载荷，并将它转发 至相关的协议驱动程序做进一步处理。

- 对比 PPTP
  - PPTP 通过 TCP 协议进行隧道的维护，L2TP 则是采用 UDP 协议。
  - PPTP 的控制报文没有经过加密，而 L2TP 的控制报文应用 IPSec ESP 进行了加密：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/98a136b12af46e953b63a3aee53ed0fd.jpg)

  

#### 第三层隧道协议

把各种网络协议直接装入隧道协议中，形成的数据包依靠第三层协议进行传输。 

##### GRE

在 GRE 中，乘客协议就是上面这些被封装的协议，封装协议就是 GRE，传输协议就是 IP。

- 工作原理
  - 路由器接收到一个需要封装和路由的原始数据包（比如 IP 数据包），首先在这个原始数据包的外面增加一个 GRE 头部构成 GRE 报文。 
  - 再为 GRE 报文增加一个新 IP 头，从而构成最终的 IP 包，这个新生成的 IP 包完全由 IP 层负责转发，中间的路由器只负责转发，而根本不关心是何种乘客协议。

- 利用 GRE 实现 VPN

  企业私有网络的 IP 地址通常是自行规划的保留 IP 地址，只是在企业网络出口有一个公网 IP 地址。原始 IP 包的 IP 地址通常是企业私有网络的保留 IP 地址，而外层的 IP 地址是企业网络出口的 IP 地址。在接收端，将收到的包的 IP 头部和 GRE 头部解开后，将原始的 IP 数据包发送到自己的私有网络上，此时在私有网络上传输的 IP 包的地址是保留 IP 地址，从而可以访问到远程企业的私有网络。

- 优点：
  - 通过 GRE，用户可利用公共 IP 网络连接非 IP 网络，如 IPX 网络、AppleTalk 网络等。多协议的本地网可以通过单一协议的骨干网实现传输，比如两端的私有网络既有 IP 网又有 IPX 等其他网络，通过 GRE，可以使所有协议的私有网络连接起来。
  - GRE 只提供封装，不提供加密，对路由器的性能影响较小，设备档次要求相对较低。 
  - 通过 GRE，还可以使用保留地址进行网络互联，或者对公网隐藏企业网的 IP 地址。

- 缺点
  - GRE 只提供了数据包的封装，而没有加密功能来防止网络监听和攻击，所以在实际环境中经常与 IPSec 一起使用。

##### IPSec

- 定义
  
  IPSec(IP Security) 是基于 TCP/IP 的标准协议，是一种开放式网络安全标准，由 IETF 定义，它在 TCP/IP 协议栈中间位置的 IP 层实现，已集成到 IPv6。IPSec 协议提供了强大的安全、加密、认证和密钥管理功能。适合大规模 VPN 使用，需要认证中心（CA）来进行身份认证和分发用户的公共密钥。
 

- 报文

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/27/c13b7c733bdcccc4d18d1f3ec8a8de85.jpg)

- 三个协议
  - 鉴别报头 Authentication Header, AH 协议： 只涉及到认证，不涉及到加密。 
  - 封装安全载荷 Encapsulating Security Payload, ESP 协议：主要用来处理对 IP 数据包的加密，与具体的加密算法独立，几乎支持各种对称密钥加密算法，默认为 3DES 和 DES。
  - 密钥管理与交换协议 Internet Key Exchange, IKE 协议：对使用的协议、加密算法和密钥进行协商。

- 工作原理
  - 使用安全方式封装加密整个 IP 包，然后对加密的负载再次封装在明文 IP 包内，通过网络发送到隧道服务器端。 
  - 隧道服务器对收到的数据包进行处理，去掉明文 IP 包头，对内容进行解密之后，获得最初的负载 IP 包，负载 IP 包在经过正常处理之后被路由到位于目标网络的目的地。

- 工作模式
  - 传输模式：
    - 只对 IP 数据包的有效负载进行加密或认证。
    - 继续使用以前的 IP 头，只对 IP 头部的部分域进行修改，IPSec 协议头插入到 IP 头和传输层头之间
  - 隧道模式：
    -  对整个 IP 数据包进行加密或认证。
    -  需要新产生一个 IP 头部，IPSec 头部被放在新产生的 IP 头部和以前的 IP 数据包之间，从而组成一个新的 IP 头部。

#### 第四层隧道协议：SSL VPN 

安全套接字层（Secure Socket Layer，SSL） 属于高层安全机制，广泛应用于 Web 浏览程序和 Web 服务器程序。在 SSL 中，身份认证是基于证书的。服务器方向客户方的认证是必须的，而 SSL 版本 3 中客户方向服务方的认证只是可选项。

- 握手协议：负责配置用于客户端和服务器之间会 话的加密参数。 
- 记录协议：用于交换应用数据。
  - 应用程序消息被分割成可管理的数据快，还可以压缩，并产生一个 MAC（消息认证码），然后结果被加密并传输。 
  - 接收方接收数据并对它解密，校验 MAC，解压并重新组合，把结果提供给应用程序协议。