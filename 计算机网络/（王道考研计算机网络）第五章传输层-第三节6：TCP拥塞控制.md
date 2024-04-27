 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

本文大部分内容来自博主：[【小林coding】](https://blog.csdn.net/qq_34827674?type=sub&subType=receivdComment)，如有需求请大家关注。本文会在此基础上结合王道课本进行梳理

### 文章目录

- [一：什么是拥塞控制](#_13)
- - [（1）拥塞控制](#1_15)
  - [（2）拥塞控制和流量控制的区别](#2_19)
- [二：拥塞窗口](#_27)
- [三：拥塞控制四大算法](#_41)
- - [（1）慢启动](#1_43)
  - [（2）拥塞避免](#2_66)
  - [（3）拥塞发生](#3_85)
  - - [A：发生超时重传时的拥塞发生算法](#A_93)
    - [B：发生快速重传时的拥塞发生算法](#B_107)
  - [（4）快速恢复](#4_118)
- [四：拥塞控制算法完整过程图](#_139)

# 一：什么是拥塞控制

## （1）拥塞控制

**拥塞控制：是指防止过多的数据注入网络，保证网络中的路由器或链路不致过载。出现拥塞时，端点并不了解到拥塞发生的细节，对通信连接的端点来说，拥塞往往表现为通信时延的增加**

## （2）拥塞控制和流量控制的区别

- **拥塞控制**：是**让网络能够承受现有的网络负荷**，是一个**全局性**的过程，涉及**所有的主机、所有的路由器**，以及与降低网络传输性能有关的所有因素
- **流量控制**：是指**点对点的通信量的控制**，即接收端控制发送端，它所要做的是**抑制发送端发送数据的速率**，以便使接收端来得及接收

# 二：拥塞窗口

拥塞控制cwnd是发送方维护的一个状态变量，**它会根据网络的拥塞程度动态变化**  
前面讲到的发送窗口`swnd`和接受窗口`rwnd`是约等于的关系，这里由于加入了拥塞窗口，因此`swnd=min(cwnd,rwnd)`

拥塞窗口变化规则

- **网络中没有出现拥塞：cwnd就会增大**
- **网络中出现了拥塞：cwnd就会减少**

**每次发送数据包的时候，将拥塞窗口和接收端主机反馈的窗口大小作比较，取较小值作为实际发送的窗口**

# 三：拥塞控制四大算法

## （1）慢启动

**慢启动：TCP在刚建立完连接后，会一点一点的提高发送数据包的数量，随着时间推移数据量会越来越大。具体规则为：当发送方每收到一个ACK，拥塞窗口cwnd大小+1**

如下图，假定拥塞窗口`cwnd`和发送窗口`swnd`相等

- 连接建立完成后，一开始初始化cwnd为1，表示可以传一个MSS大小的数据
- 当收到一个ACK确认应答后，cwnd增加1，于是一次可以发送2个
- 当收到2个ACK时，cwnd曾家2，于是这次可以发送4个
- 当4个都到来时，每个cwnd增加一个，那么这次就可以发送8个

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9bce99bdf7e046b4bcf358100b2ff0e8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)

可以看出慢启动算法，发送数据包的数量是呈现**指数级的增长的**，当超过某个阈（慢启动阈值）值时，按照**线性方式增长**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1c87e325601744ba964a1a649a6f5155.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)  
其中这个阈值就是`ssthresh`\(`slow start threshold`\)

- 当`cwnd>ssthresh`时，使用**慢启动**算法
- 当`cwnd>=ssthresh`时，使用**拥塞避免**算法

## （2）拥塞避免

**拥塞避免：当拥塞窗口`cwnd`超过慢启动门限`ssthresh`时就会进入拥塞避免算法（一般来说，`ssthresh`\=65535字节）。具体规则为：每收到一个ACK时，`cwnd`增加`1/cwnd`**

接上面的例子，如下图

- 当8个ACK到来时，每个确认增加1/8，8个ACK于是cwnd一共增加1，于是这一次可以发送9个MSS大小的数据，变为了线性增长

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe12255954a7840ce80833f6d457485a2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)

因此拥塞避免算法将原本慢启动算法的指数级增长变为了线性增长，因此增长速度变得缓慢了很多。

虽然速度变慢了，但是毕竟一直在增长，所以网络就会慢慢进入拥塞的状态，也就会产生丢包，这时就需要对丢失的数据包进行重传

**当触发重传时，进入拥塞发生算法**

## （3）拥塞发生

**当网络出现拥塞，发生数据包重传，重传方式主要有两种，其对应的拥塞发生算法是不同的**

- 超时重传
- 快速重传

### A：发生超时重传时的拥塞发生算法

这个时候，`ssthresh`和`cwnd`的值会发生变化

- `ssthresh`设为cwnd/2
- `cwnd`重置为1

如下图，发生超时重传就意味着“一夜回到解放前”，之后就重新开始了慢启动。这种方式很明显太激进了，几乎是戛然而止，会造成**网络卡顿**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7823b81ed34b42cfb55b895c18dde875.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)

### B：发生快速重传时的拥塞发生算法

快速重传算法是**当接收方发现丢了一个中间包时，发送三次前一个包的ACK，于是发送单就会快速重传**，不必等待超时再重传

TCP认为这种情况不严重，因为大部分没有丢失，于是`ssthresh`和`cwnd`变化如下

- `cwnd`\=`cwnd/2`
- `ssthresh`\=`cwnd`
- 进入快速恢复算法

## （4）快速恢复

**快速恢复：快速重传和快速恢复算法一般同时使用，快速恢复算法认为，你还能收到3个重复ACK说明网络也不那么糟糕嘛，所以没有必要像超时重传那样强烈**

**因此首先**

- `cwnd`\=`cwnd/2`
- `ssthresh`\=`cwnd`

**然后进入快速恢复**

- 拥塞窗口`cwnd`\=`ssthresh`+3（意思是确认有3个数据包收到了）
- 重传丢失的数据包
- 如果再收到重复的ACK，那么`cwnd+1`
- 如果收到新数据的ACK后，就把`cwnd`设置为第一个中的`ssthresh`的值，原因是该ACK确认了新的数据，说明从duplicated ACK的数据都已经收到，该恢复过程已经结束，可以回到恢复之前的状态了，也就是再次进入拥塞避免状态。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F011b5f3caf9e41bcae7179fe19a20e57.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)

# 四：拥塞控制算法完整过程图

如下图是一个比较经典的拥塞算法的过程图  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F94a68d0d607142d2b5bbfb2ba6518352.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125627384)