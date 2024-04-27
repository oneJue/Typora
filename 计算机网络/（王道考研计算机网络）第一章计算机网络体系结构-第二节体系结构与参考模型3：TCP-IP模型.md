 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：OSI参考模型与TCP/IP参考模型比较](#OSITCPIP_21)
- - [（1）相同点](#1_23)
  - [（2）不同点](#2_34)
- [二：5层参考模型](#5_48)

OSI参考模型是一个法定的标准，但是它没有指明应该如何应用，只是一个理论上的概念；**TCP/IP模型则诞生于协议栈，之后对协议栈进行分层，继而产生了TCP/IP模型**

- OSI制定的初衷是希望网络标准化，人们对OSI寄予厚望。可是OSI却迟迟未能推出产品，妨碍了第三方厂家开发相应的软件，硬件，进而影响了OSI的市场占有和未来的发展；而TCP/IP在OSI推出之前就已经占有一定的市场，代表着市场的主流，OSI在出台后很长时间不具有操作系。这样，经过十几年的发展，得到广泛应用的不是法律上的国际标准OSI，而是非国际标准TCP/IP，所以TCP/IP被称为事实上的国际标准

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F777a059357ed4c48a22e9a7aab066c32.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124332043)

# 一：OSI参考模型与TCP/IP参考模型比较

## （1）相同点

主要有以下几点

- 二者都采取**分层的体系结构**，将庞大且复杂的问题划分为若干较容易处理的、范围较小的问题，而且分层的功能也大体相似。
- 二者都是基于**独立的协议栈**的概念
- 二者都可以**解决异构网络的互联**，实现世界上不同厂家生产的计算机之间的通信

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2b27ef6372f74bffabc62b37f4b3846a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124332043)

## （2）不同点

①：OSI参考模型的最大贡献就是精确地定义了三个主要概念：**服务、协议和接口**，这与现代的面向对象程序设计思想非常吻合。而TCP/IP 模型在这三个概念上却没有明确区分，不符合软件工程的思想

②：第二，OSI参考模型产生在协议发明之前，**没有偏向于任何特定的协议**，通用性良好。但设计者在协议方面没有太多经验，不知道把哪些功能放到哪一层更好；TCP/IP模型正好相反，首先出现的是协议，**模型实际上是对已有协议的描述**，因此不会出现协议不能匹配模型的情况，但该模型不适合于任何其他非TCP/IP的协议栈

③：**TCP/IP模型在设计之初就考虑到了多种异构网的互联问题**，并将网际协议 **\(IP\) 作为一个单独的重要层次**。OSI参考模型最初只考虑到用一种标准的公用数据网将各种不同的系统互联。OSI参考模型认识到网际协议IP的重要性后，只好在网络层中划分出一个子层来完成类似于TCP/IP模型中的IP的功能

④：重点区别

- OSI参考模型**在网络层支持无连接和面向连接的通信，但在传输层仅有面向连接的通信**
- TCP/IP模型**在网际层仅有一种无连接的通信模式，但传输层支持无连接和面向连接两种模式**

# 二：5层参考模型

OSI参考模型详细说明了每层应该具有的功能，TCP/IP参考模型层次简单、独立性强。因此**结合两种模型的优点，计算机网络考研中研究的模型是5层模型，课本中章节也是按照这个逻辑划分的**

| 层次 | 作用 | 传输单位 |
| --- | --- | --- |
| 物理层 | 实现比特流的透明传输 | 比特 |
| 数据链路层 | 负责把网络层传下来的数据报组装成帧 | 帧 |
| 网络层 | 分组从源端传到目的端，为分组交换网上的不同主机提供通信服务 | 数据报 |
| 传输层 | 负责主机中两个进程的通信，也即端到端的通信 | 数据包 |
| 应用层 | 为特定类型的网络应用提供访问OSI环境的手段 | 报文 |

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd0f90147e32a41ef89346bf1fe4d0281.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124332043)