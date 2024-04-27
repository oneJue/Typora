 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：广域网的基本概念](#_10)
- - [（1）广域网](#1_12)
  - [（2）广域网和局域网对比](#2_22)
- [二：PPP协议](#PPP_27)
- - [（1）PPP协议应该和不需要满足的要求](#1PPP_32)
  - [（2）PPP协议的三个组成部分](#2PPP_56)
  - [（3）PPP协议状态图](#3PPP_66)
  - [（4）PPP协议帧格式](#4PPP_79)
- [三：HDLC协议](#HDLC_91)
- - [（1）HDLC站及数据操作方式](#1HDLC_98)
  - [（2）HDLC帧格式](#2HDLC_116)
- [四：PPP协议和HDLC协议对比](#PPPHDLC_128)

# 一：广域网的基本概念

## （1）广域网

**广域网\(WAN, Wide Area Network\)：通常跨接很大的物理范围，所覆盖的范围从几十公里到几千公里，能连接多个城市或国家，或横跨几个洲并能提供远距离通信，形成国际性的远程网络。广域网的通信子网主要使用分组交换技术。广域网的通信子网可以利用公用分组交换网、卫星通信网和无线分组交换网，它将分布在不同地区的局域网或计算机系统互连起来，达到资源共享的目的。如因特网\(Internet\)是世界范围内最大的广域网**

- **注意**：广域网不等于互联网，互联网可以连接不同类型的网络\(既可以连接局域网，又可以连接广域网\),通常使用路由器来连接

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8461f70eaec64746a6257182ea242be0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

## （2）广域网和局域网对比

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1ffd827be2ab404581c8275933fd40e6.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

# 二：PPP协议

**PPP（Point-to-Point Protocol）协议：是使用串行线路通信的面向字节的协议，该协议应用在直接连接两个结点的链路上。其设计目的主要是用来通过拨号或专线方式建立点对点连接发送数据。PPP协议只支持全双工链路**

## （1）PPP协议应该和不需要满足的要求

**需要满足**

- **简单**：对于链路层的帧， 无需纠错，无需序号，无需流量控制
- **封装成帧**：帧定界符
- **透明传输**：与帧定 界符一-样比特组合的数据应该如何处理:异步线路用字节填充，同步线路用比特填充
- **多种网络层协议**：封装的IP数据报可以采用多种协议
- **多种类型链路**：串行/并行，同步/异步，电/光…
- **差错检测**：错就丢弃
- **检测连接状态**：链路是否正常工作
- **最大传送单元**：数据部分最大长度MTU
- **网络层地址协商**：知道通信双方的网络层地址
- **数据压缩协商**

**不需要满足（上层负责）**

- **纠错**
- **流量控制**
- **序号**
- **不支持多点线路**

## （2）PPP协议的三个组成部分

PPP协议有以下三个部分组成

1.  **一个将IP数据报封装到串行链路\( 同步串行/异步串行\)的方法**
2.  **链路控制协议LCP**:建立并维护数据链路连接
3.  **网络控制协议NCP**: PPP可 支持多种网络层协议，每个不同的网络层协议都要一个相应的NCP来配置，为网络层协议建立和配置逻辑连接

## （3）PPP协议状态图

- 当线路处于静止状态时，不存在物理层连接
- 当线路检测到**载波信号**时，建立物理连接，线路变为**建立状态**。此时，LCP开始**选项商定**，商定成功后就进**入身份验证状态**。双发身份验证通过后，进入网络状态
- 这时，采用NCP配置网络层，配置成功后，进入**打开状态**，然后就可进行数据传输。当数据传输完成后，线路转为**终止状态**。载波停止后则回到**静止状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdc9e6c43c7ed4d92bb293c9359d40021.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

## （4）PPP协议帧格式

- **F**：第一个字节和最后一个字节（7E）是**帧定界符**（为了实现**透明传输**还需要在信息部分插入转义字符7D）
- **A（地址字段）**：FF
- **C（控制字段）**：03
- **协议部分**：用于标识信息部分究竟是什么
- **FCS（帧检验序列）**：差错检测

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F407c9d622f444d3c81cabc815f114c54.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

# 三：HDLC协议

**HDLC（High-level Data Link Control, HDLC）协议：是一个在同步网上传输数据、面向比特的数据链路层协议，它是由国际标准化组织\(ISO\)根据IBM公司的SDLC\(SynchronousData Link Control\)协议扩展开发而成的。数据报文可透明传输，用于实现透明传输的“0比特插入法”易于硬件实现。采用全双工通信所有帧采用CRC检验，对信息帧进行顺序编号，可防止漏收或重份，传输可靠性高**

## （1）HDLC站及数据操作方式

**HDLC有三种类型的站**

- **主站**：主要功能是**发送命令\(包括数据信息\)帧、接收响应帧，并负责对整个链路的控制系统的初启、流程的控制、差错检测或恢复**等
- **从站**：主要功能是**接收由主站发来的命令帧，向主站发送响应帧，并且配合主站参与差错恢复**等链路控制
- **复合站**：主要功能是**既能发送，又能接收命令帧和响应帧**，并且负责整个链路的控制

**相应的，HDLC有三种数据操作方式**

- **正常响应方式**：这是一种非平衡结构操作方式，即**主站向从站传输数据**，从站响应传输，但**从站只有在收到主站的许可后**，才可进行响应
- **异步平衡方式**：这是一种平衡结构操作方式。在这种方式中，**每个复合站都可以进行对另一站的数据传输**
- **异步响应方式**：这是一种非平衡结构操作方式。在这种方式中，从站**即使未受到主站的允许**，也可进行传输

## （2）HDLC帧格式

- **标志字段 F F F**：01111110
- **地址字段 A A A**：共8位；在使用非平衡方式传送数据时，站地址字段总是写入从站的地址；在使用平衡方式传送数据时，站地址字段填入的是应答站的地址
- **控制字段 C C C**：共8位，可划分为三类  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa8e039fed09c48f79d369e1613c59345.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc8cbee4644cc41bfa99da65ea6b84be9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)

# 四：PPP协议和HDLC协议对比

HDLC、PPP只支持**全双工链路**，都可以实现**透明传输、差错检测**，但**不纠正差错**。区别如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7b5a01f81944466d85acee22da52d84c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125171574)