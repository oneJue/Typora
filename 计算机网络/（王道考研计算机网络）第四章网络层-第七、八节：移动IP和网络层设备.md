 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：移动IP](#IP_10)
- - [（1）移动IP相关概念](#1IP_12)
  - [（2）移动IP通信过程](#2IP_24)
- [二：网络层设备-路由器](#_68)
- [三：三层设备的区别](#_96)

# 一：移动IP

## （1）移动IP相关概念

**移动IP技术：移动IP技术是移动结点\(计算机/服务器等\)以固定的网络IP地址，实现跨越不同网段的漫游功能，并保证了基于网络IP的网络权限在漫游过程中不发生任何改变**

- **移动IP技术：** 具有**永久IP地址**的移动设备
- **归属代理（本地代理）：** 一个移动结点拥有的就“居所”称为**归属网络**，在归属网络中代表移动节点执行移动管理功能的实体叫做**归属代理**
- **外部代理（外地代理）：** 在**外部网络**中帮助移动节点完成移动管理功能的实体称为外部代理
- **永久地址（归属地址/主地址）：** 在**外部网络**中帮助移动节点完成移动管理功能的实体称为外部代理
- **转交地址（辅地址）：** 移动站点在外部网络使用的**临时地址**

## （2）移动IP通信过程

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8be401112a404b2c8da99a634ae138a9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125502859)  
**A A A刚进入外部网络：**

- 在外部代理登记获得一个**转交地址**，离开时注销
- 外地代理向本地代理**登记转交地址**

**B B B给 A A A发送数据报：**

- 本地代理**截获数据报**
- 本地代理再封装数据报，新的数据报目的地址是转交地址，发给**外部代理\(隧道\)**
- 外部代理**拆封数据报**并发给 A A A

**A A A给 B B B发送数据报：**

- A A A用自己的主地址作为数据报源地址，用 B B B的IP地址作为数据报的目的地址

**A A A移动到了下一个网络：**

- 在**新外部代理**登记注册一个转交地址
- 新外部代理给本地代理发送新的转交地址\(**覆盖旧的**\)
- 通信

**A A A回到了归属网络：**

- A A A向本地代理**注销转交地址**
- 按原始方式通信

# 二：网络层设备-路由器

**路由器：路由器是一种具有多个输入/输出端口的专用计算机，其任务是连接不同的网络\(连接异构网络\)并完成路由转发。在多个逻辑网络\(即多个广播域\)互联时必须使用路由器**

- **路由选择**：根据所选定的路由选择协议**构造出路由表**，同时经常或定期地和相邻路由器交换路由信息而不断地更新和维护路由表
- **分组转发**：根据**转发表**\(路由表得来\)对分组进行发。

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2701d1aad4b44dfc8ac46e50323aa068.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125502859)

**输入端口对线路上收到的分组的处理**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbb44fa66f8d34a4cab6ee7125fae4dae.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125502859)

**输出端口将交换结构传送来的分组发送到线路**

- 若路由器处理分组的速率赶不上分组进入队列的速率，则队列的存储空间最终必定减少到零，这就使后面再进入队列的分组由于没有存储空间而只能被丢弃。**路由器中的输入或输出队列产生溢出是造成分组丢失的重要原因**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F83935e6fb0954a08a87cfaeb87bf52ce.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125502859)

# 三：三层设备的区别

- **路由器：可以互联两个不同网络层协议的网段**
- **网桥：可以互联两个物理层和链路层不同的网段**
- **集线器：不能互联两个物理层不同的网段**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F496794c429164028907b7bf8c7670f59.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125502859)