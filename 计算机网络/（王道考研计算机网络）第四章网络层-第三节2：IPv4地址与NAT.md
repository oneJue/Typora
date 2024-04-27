 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：IP地址格式（点分十进制）](#IP_18)
- [二：分类的IP地址](#IP_36)
- - [（1）A、B、C类地址](#1ABC_49)
  - [（2）D、E类地址](#2DE_63)
  - [（3）特殊的IP地址](#3IP_73)
- [三：分类IP的优缺点](#IP_96)
- [四：网络地址转换NAT](#NAT_112)
- - [（1）公有IP地址与私有IP地址](#1IPIP_115)
  - [（2）网络地址转换NAT](#2NAT_134)

连接到因特网上的每台主机\(或路由器\)都分配一个**32比特的全球唯一标识符**， 即IP地址，IP编址经历了如下几个阶段

- 分类的IP地址
- 子网的划分
- 构成超网\(无分类编址方法\)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0c133dc2e7a04985bc16c678d5fa40dd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

# 一：IP地址格式（点分十进制）

IP地址\(IPv4 地址\)由32 位正整数来表示，IP 地址在计算机是以二进制的方式处理的。人类为了方便记忆，采用了 **点分十进制的标记方式，即：将32位IP地址以每8位为一组，共分为4组，每组以`.`隔开，再将每组转化为十进制**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6eb9a47df36c49e387addc85747d6dfa.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

需要注意的是，除了主机外，像服务器、路由器这些设备**至少具有2个以上的IP地址**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F53c2bf7530fc4fff9bae83aaa03c88b4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

# 二：分类的IP地址

**分类的IP地址：传统的IP地址就是分类的地址，分为 A A A、 B B B、 C C C、 D D D、 E E E 五类。无论哪类IP地址，都由网络号和主机号两部分组成，也即IP地址::=\{\<网络号>,\<主机号>\}**

- **网络号**：标志主机或路由器**所连接到的网络**；一个网络号在整个因特网范围内必须是**唯一的**
- **主机号**：标志该**主机或路由器**；在网络号指定的网络范围内主机号必须是**唯一的**

**所以，一个IP地址在整个因特网范围内是唯一的。分类如下**

- 下图中黄色部分是分类号，用于区分IP地址类别  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe9ec32b9cd974406b6d044852316fc5b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

## （1）A、B、C类地址

**下表显示了A、B、C类地址对应的地址范围、最大主机个数**

- A A A类地址最大可用网络数要减去全0和全1这两种情况
- B B B、 C C C类地址最大可用网络数要减去全1这种情况

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F42776ea93cd9403f8d3048d569391e6e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

每个网络中最大主机数要减去2  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb3978ff5621446809136a5910ae744b1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

## （2）D、E类地址

D、E类地址是**没有主机号的**，所以不可用于主机IP， D D D类常被用于多播， E E E类是预留的分类，暂时未使用

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F21754a8c7bbc42cab3fb12cfd4e47ff7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

## （3）特殊的IP地址

上述IP地址中，有些IP地址**具有特殊用途**，不用作主机IP地址

| 网络号 | 主机号 | 作为IP分组源地址 | 作为IP分组目的地址 | 用途 | 举例 |
| --- | --- | --- | --- | --- | --- |
| 全0 | 全0 | 可以 | 不可以 | 本网范围内表示主机、路由表中用于表示默认路由 | 0.0.0.0 |
| 全0 | 特定值 | 可以 | 不可以 | 本网内某个特定主机 |  |
| 全1 | 全1 | 不可以 | 可以 | 本网广播地址 | 255.255.255.255 |
| 特定值 | 全0 | 不可以 | 不可以 | 网络地址，表示一个网络 | 202.98.174.0 |
| 特定值 | 全1 | 不可以 | 可以 | 直接广播地址 | 202.98.174.255 |
| 127 | 任何数（非全0或全1） | 可以 | 可以 | 本地环回（环路自检） | 127.0.0.0 |

# 三：分类IP的优缺点

**优点**

- 简单明了
- 选路简单

**缺点**

- **同一网络下面没有地址层次**：IP分类是没有地址层次划分的功能的，所以降低了地址的灵活性
- **不能很好的与现实网络匹配**：例如C类地址包含的最大主机数量实在太少了，一个网吧都可能不够用；B类地址包含的最大主机数量又太多了，造成闲置浪费

# 四：网络地址转换NAT

## （1）公有IP地址与私有IP地址

**在 A A A、 B B B、 C C C类地址中，实际上还分为公有IP地址与私有IP地址**

- 平时在家里、学校用的IP地址，一般都**是私有IP地址**。因为这些地址允许组织内部的IT人员自己管理、自己分配，而且可以重复。所以你会发现，有时你学校的某个私有IP地址和我们学校的可以是一样的

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc223fa227f04411595153f2d514e6d2f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

**而对于公有IP地址是不能重复的，需要一个组织统一分配**

- 比如你要开一个博客网站，那么你就需要去申请购买一个公有IP，这样全世界的人才能访问。并且公有IP地址基本上要在整个互联网范围内保持唯一

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F10a8208c0a144a2bb26d8bbe30bffe90.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)

## （2）网络地址转换NAT

**网络地址转换NAT：在专用网连接到因特网的路由器上安装NAT软件（此时此路由器叫做NAT路由器），它至少有一个有效的外部全球IP地址。这样做有很大的好处**

- 使得整个专用网**只需要一个全球IP地址**就可以与因特网连通
- 由于专用网本地IP地址是可重用的，所以**NAT大大节省了IP地址的消耗**
- 隐藏了**内部网络结构**，降低了内部网络受到攻击的风险

下图显示NAT过程， A A A和 B B B通信

- A A A发送数据报（源：192.168.0.3；目的：213.18.2.4；端口号3000）
- 数据报文到达NAT路由器，根据NAT转换表，替换源地址和端口（源：172.38.1.5；目的：213.18.2.4；端口号：40001）
- …

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F081ebb68a2cf4138ad3e9630ae242a4f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125298770)