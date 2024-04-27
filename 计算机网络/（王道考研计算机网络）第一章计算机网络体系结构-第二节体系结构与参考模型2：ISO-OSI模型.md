 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：ISO/OSI参考模型简介](#ISOOSI_11)
- [二：ISO/OSI参考模型通信流程](#ISOOSI_22)
- [三：ISO/OSI参考模型各层功能及涉及协议（重点）](#ISOOSI_67)
- - [（1）应用层（Application Layer）](#1Application_Layer_76)
  - [（2）表示层（Presentation Layer）](#2Presentation_Layer_90)
  - [（3）会话层（Session Layer）](#3Session_Layer_99)
  - [（4）传输层（Transport Layer）](#4Transport_Layer_108)
  - [（5）网络层（Network Layer）](#5Network_Layer_124)
  - [（6）数据链路层（Data Link Layer）](#6Data_Link_Layer_146)
  - [（7）物理层（Physical Layer）](#7Physical_Layer_162)

# 一：ISO/OSI参考模型简介

**OSI（开放式系统互联通信参考模型）：是国际化标准组织（ISO）于1984年提出的网络体系结构模型。自下而上依次为 **物理层、数据链路层、网络层、传输层、会话层、表示层、应用层；其中低三层统称为**通信子网，完成数据传输功能，高三层统称为资源子网，完成数据处理功能，传输层承上启下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4d936137f98a48b087ccf60ff142af6d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

**OSI参考模型，在理论上获得了成功，但是在市场应用上却是失败的。而之后的TCP/IP模型则更适合于市场，因此获得了成功，OSI参考模型只能成为一种法定标准**

# 二：ISO/OSI参考模型通信流程

**分层后整个通信流程类似于收发快递。寄快递时，你需要将信息填写好，然后用包裹封装起来，接着交给快递公司寄送；收到快递时，你需要将包裹一层一层剥开，拿到最终想要的物品（信息）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff517f4b15f0f4c9fac8fd34f82e49520.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

**信息在通信双方的主机上需要经过七层考验，而在经过中间的一些设备时最多只能到达网络层**

- 如果两个主机属于同一网段，那就不需要经过网络层，否则需要通过路由器等设备进行转发

**对等实体间会有相应的协议，用来规定如何封装数据和解析数据**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6a1700409f8a43eba2f1e02a6096958c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

这七层模型中：

- **上面四层是中间系统（例如路由器、网关等）用不到的，实现了一种逻辑上的连通（就好像信息“直接”从主机 A A A传到了主机 B B B），因此实现了端到端通信**
- **下面三层所负责的就是“信息下一步应该要传到哪里、是否需要进行转发”等问题，传输的过程中可能需要很多点、发送很多次才能完成，因此实现了点到点通信**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1088f65bf601409baa950745fb985adc.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

---

**通信流程：总的来说，网络中信息在传输时需要经历封包和解包两个过程**

- **封包**：每一层都将上一层传过来的数据看作源信息，并附加本层的**首部（或尾部）**，然后传送给下一层
- **解包**：每一层在取走本层的包后，然后将数据传送给下一层进行解包

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fba322011ce7a43a1b1fb4dc8383a9794.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

# 三：ISO/OSI参考模型各层功能及涉及协议（重点）

下图比较重要

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6cc1391e084d44f9a54bc609d5aba006.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124284720)

---

## （1）应用层（Application Layer）

**应用层：是OSI模型的最高层，是用户与网络的界面；应用层为特定类型的网络应用提供访问OSI环境的手段；用户的实际应用多种多样，所以这也就要求应用层采用不同的协议来解决不同类型的应用要求，比如QQ、浏览器等等；典型的协议有**

- FTP（文件传送）
- SMTP（电子邮件）
- HTTP（万维网）
- …

## （2）表示层（Presentation Layer）

**表示层：用于处理在两个通信系统中交换信息的表示方式（语法和语义）；主要有以下功能**

- **数据格式变换**
- **数据加密解密**
- **数据压缩和恢复**

## （3）会话层（Session Layer）

**会话层：向表示层实体/用户进程提供建立连接并在连接上有序传输数据，也即建立同步（SYN）；主要有以下功能**

- **建立、管理、终止会话**
- **使用校验点可以使会话在通信失效时从校验点或同步点继续恢复通信，实现数据同步**

## （4）传输层（Transport Layer）

**传输层：负责主机中两个进程的通信，也即端到端的通信（因为主机中的进程是靠端口号区分的），传输单位是报文段或用户数据包；主要有以下功能**

- **可靠传输和不可靠传输**：可靠传输负责信息及时、完整到达；不可靠传输只负责发送信息
- **差错控制**：当发送的报文段失序、丢失时负责纠正
- **流量控制**：控制发送方发送数据的速度或发送量，使接收方能够正确完整地接收数据
- **复用和分用**：复用是指多个应用层进程可以同时使用下面传输层的服务；分用是指传输层把收到的信息分别交付给上面应用层中相应的进程

**主要协议有**

- TCP
- UDP

## （5）网络层（Network Layer）

**网络层：负责把分组从源端传到目的端，为分组交换网上的不同主机提供通信服务，传输单位是数据报（数据报由大小相等分组构成）；主要有以下功能**

- **路由选择**：根据路由选择算法选择合适的路径发送
- **流量控制**：协调发送端和接收端速度
- **差错控制**：双方根据一定的校验规则，验证传输数据是否正确
- **拥塞控制**：洞察整个网络情况，控制全局，避免网络堵塞

**主要协议有**

- IP
- ICMP
- ARP
- OSPF

## （6）数据链路层（Data Link Layer）

**数据链路层：负责把网络层传下来的数据报组装成帧，传输单位是帧；主要有以下功能**

- **成帧**：定义帧的开始和结束，否则无法区分信息
- **差错控制**：包括帧错误或位错误，提供检错和纠错手段
- **流量控制**：
- **访问控制**：控制对信道的访问

**主要协议有**

- SDLC
- HDLC
- PPP

## （7）物理层（Physical Layer）

**物理层：负责在物理介质上实现比特流的透明传输，传输单位是比特；主要有以下功能**

- **定义接口特性**
- **定义传输模式**
- **定义传输速率**
- **比特同步**
- **比特编码**