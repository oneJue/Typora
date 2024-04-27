 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：BGP协议](#BGP_10)
- [二：BGP协议报文格式](#BGP_32)
- [三：BGP协议特点](#BGP_39)
- [四：BGP-4的四种报文](#BGP4_47)
- [三种协议对比](#_58)

# 一：BGP协议

**边界网关协议（BGP）：是不同自治系统的路由器之间交换路由信息的协议，是一种外部网关协议，常用于互联网的网关之间。是应用层协议。其特点如下：**

- **和谁交换**：与其他自治系统的**邻站BGP的发言人**交换信息
- **交换什么**：交换的**网络可达性**信息，即要达到某个网络所要经过的一系列AS
- **多久交换**：发生变化时更新**有变化**的部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4320062e5a6848d18c6c2abe3f2fef74.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125484529)

**BGP所交换的网络可达性的信息就是要到达某个网络所要经过的一系列AS。当BGP发言人互相交换了网络可达性的信息后，各BGP发言人就根据所采用的策略从收到的路由信息中找出到达各AS的较好路由**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F87c90d8acbb0487fbe432849d7a576d3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125484529)

# 二：BGP协议报文格式

一个BGP发言人与其他自治系统中的BGP发言人要交换路由信息，就要先**建立TCP连接**，即通过TCP传送，然后在此连接上交换BGP报文以建立BGP会话\(session\)，利用BGP会话交换路由信息

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdbc658bfeefb47388e2cf51b281b0be1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125484529)

# 三：BGP协议特点

- **BGP交换路由信息的结点数量级是自治系统的数量级，要比这些自治系统中的网络数少很多**
- **每个自治系统中BGP发言人\(或边界路由器\)的数目是很少的。这样就使得自治系统之间的路由选择不致过分复杂**
- **BGP支持CIDR，因此BGP的路由表也就应当包括目的网络前缀、下一跳路由器，以及到达该目的网络所要经过的各个自治系统序列**
- **在BGP刚刚运行时，BGP的邻站是交换整个的BGP路由表。但以后只需要在发生变化时更新有变化的部分。这样做对节省网络带宽和减少路由器的处理开销都有好处**

# 四：BGP-4的四种报文

**BGP-4共使用4种报文**

- **OPEN \(打开\)报文：用来与相邻的另-一个BGP发言人建立关系，并认证发送方**
- **UPDATE \(更新\)报文：通告新路径或撤销原路径**
- **KEEPALIVE \(保活\)报文：在无UPDATE时，周期性证实邻站的连通性;也作为OPEN的确认**
- **NOTIFICATION \(通知\)报文：报告先前报文的差错;也被用于关闭连接**

# 三种协议对比

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F77180c155adc4b5ba0c0bc5f1ad391b3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125484529)