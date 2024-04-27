 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

**注意：**

- 本文大部分内容来自博主：[【小林coding】](https://blog.csdn.net/qq_34827674?type=sub&subType=receivdComment)，如有需求请大家关注。本文会在此基础上结合王道课本进行梳理
- **三次握手、四次挥手属于计算机网络核心中的核心中的核心**，甚至可以说是计算机网络的代名词之一，考试和面试八股文（尤其面试）必出。王道书中这一部分介绍的实在太少了
- 【实践-理解TIMIE\_WAIT状态】部分会借助网络编程相关知识讲解，需要Linux及C++相关知识

### 文章目录

- [一：TCP三次握手过程和状态变迁](#TCP_21)
- - [（1）三次握手过程和状态变迁过程详解](#1_22)
  - [（2）为什么必须是三次握手？](#2_49)
  - - [A：原因之一：只有三次握手才可以阻止重复历史连接的初始化（主要原因）](#A_52)
    - [B：同步双方初始序列号](#B_67)
    - [C：避免资源浪费](#C_81)
- [二：TCP四次挥手过程和状态变迁](#TCP_91)
- - [（1）四次挥手过程和状态变迁详解](#1_94)
  - [（2）为什么需要四次挥手？](#2_109)
  - [（3）什么是TIME\_WAIT以及它为什么是2MSL](#3TIME_WAIT2MSL_118)
  - - [A：TIME\_WAIT](#ATIME_WAIT_119)
    - [B：为什么需要TIME\_WAIT](#BTIME_WAIT_135)
- [三：实践-理解TIMIE\_WAIT状态](#TIMIE_WAIT_163)

# 一：TCP三次握手过程和状态变迁

## （1）三次握手过程和状态变迁过程详解

**①：开始的时候，客户端和服务端都处于`CLOSED`状态。服务器主动监听某个端口，进入`LISTEN`状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F771de5c3126544339499420e09388630.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

**②：客户端随机初始化序号（client\_isn），并将此序号置于TCP首部的【序号】字段中，同时将SYN标志位置为1，表示SYN报文。接着把第一个SYN报文发送给服务端，表示向服务器发起连接（该报文不包含应用层数据），之后客户端就处于了向服务器发起连接`SYN-SENT`状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa719085cf40e49d08674cb220ddb6ac1.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

**③：服务端收到客户端的SYN报文后，服务端也会随机初始化自己的序号\(server\_isn\)，将此序号填入TCP首部的【序号】字段中，然后在TCP首部【确认应答号】填入client\_isn+1，接着把SYN和ACK标志位置为1，最后把报文发给客户端，该报文也不包含应用程序数据。之后服务端处于`SYN-RCVD`状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F862e49bd324f4ebb9993f5492c6befdf.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

**④：客户端收到服务端报文后，还要向服务器回应最后一个应答报文。于是将该应答报文TCP首部ACK标志位置为1，其【确认应答号】字段填入server\_isn+1，最后把报文发送给服务端。此次报文可以携带客户服务器的数据，之后客户端处于`ESTABLISHED`状态。服务器在收到客户端应答报文后，也进入`ESTABALISHED`状态**

**⑤：至此。连接建立完成，客户端和服务端都处于了`ESTABALISHED`状态，接着就可以相互发送数据了**

---

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fed3a651480ed4cc38825e95fd0ed4206.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

## （2）为什么必须是三次握手？

### A：原因之一：只有三次握手才可以阻止重复历史连接的初始化（主要原因）

RFC793指出的TCP连接使用三次握手的主要原因：**为了防止旧的重复连接初始化造成混乱**

> Thep principle reason for the three-way handshake is to prevent old duplicate connection initiations from causing confusion.

网络环境是错综复杂的，并不会遵循先发先到的原则，有可能新数据会比旧数据更早到达主机。因此在网络拥堵的情况下，假如一个旧的SYN报文比新的SYN早到了服务端，那么此时服务端会返回一个SYN+ACK报文给客户端，客户端收到后可以根据自身上下文，判断这是一个**历史连接**，那么客户端会**发送RST给服务端，中止此次连接**

- 如果是两次握手，就无法判断是否是历史连接
- 也即**三次握手则可以在客户端准备发送第三次报文时，能够拥有足够的上下文判断当前是否为历史连接**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fad58f7a55e3b47a18c9bf80e215bd989.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

### B：同步双方初始序列号

TCP协议通信的双方，都必须维护一个【序列号】，序列号是可靠传输的关键因素，具体作用

- **接收方可以去除重复数据**
- **接收方可以根据数据包的序列号按序接受**
- **可以标识发送出去的数据，哪些是已经被对方接收的**

所以当客户端发送携带【初始序列号】的SYN报文的时候，需要服务端返回一个ACK应答报文，表示客户端的SYN报文服务端已经成功接收；而当服务端发送【初始序列号】给客户端时，依然也要得到客户端的应答回应。**这样一来一回，才能保证双方的序列号可以被可靠同步**

当然，四次握手也能够可靠的同步初始化序列号，不过我们会把把第二步和第三步优化为一步以此**减少通信次数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3969c5eb4a004df28a36dbcfb86fe942.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

### C：避免资源浪费

如果仅有两次握手，当客户端的SYN请求在网络中堵塞时，客户端没有接受到ACK，就会触发重传，此时**服务端不清楚客户端是否已经收到了自己发送的建立连接的ACK确认，所以每收到一个SYN就先去建立一个连接**

**这样做的后果很麻烦。如果客户端的SYN堵塞了，重复发送了多次SYN报文，那么服务器在收到SYN后就会建立多个冗余的无效连接，造成资源浪费**

- 利用这一点，服务器很容易受到**SYN FIood（SYN洪水攻击）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F89d61bdd947848639ba4da5bf1ff04a8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

# 二：TCP四次挥手过程和状态变迁

## （1）四次挥手过程和状态变迁详解

**具体过程如下（注意主动关闭连接的才会有`TIME_WAIT`状态）**

- 客户端打算关闭连接，此时会发送一个TCP首部FIN标志位置为1的报文，也即FIN报文，之后客户端会进行`FIN_WAIT_1`状态（应用层close，用户不可能发送数据）
- 服务端受到该报文后，就像客户端发送ACK应答报文，接着服务端进入`CLOSE_WAIT`状态
- 客户端收到服务端的ACK应答报文后，进行`FIN_WAIT_2`状态
- 等待服务端处理完数据后，就会向客户端发送FIN报文，之后服务端进行`LAST_ACK`状态
- 客户端再收到服务端的FIN报文后，回应一个ACK应答报文，之后进入`TIME_WAIT`状态
- 服务端收到了ACK后，进入`CLOSED`状态，至此服务端完成连接关闭
- 客户端在经过2MSL后，自动进入`CLOSED`状态，至此客户端完成连接的关闭

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fce9e1bdb497d4123807c203c6783f657.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

## （2）为什么需要四次挥手？

**多的那一次一般就是服务端需要等待完成数据的发送和处理，其中的ACK和FIN分开发送了**

- 首先关闭连接时，客户端向服务端发送FIN，仅仅表示关闭的是客户端对服务端的单向信道；
- 服务端收到客户端的FIN报文时，先回应一个ACK，表示“你先等等，可能还有数据未处理完”
- 等服务端不再发送数据时，才发送FIN报文给客户端表示同意现在关闭连接

## （3）什么是TIME\_WAIT以及它为什么是2MSL

### A：TIME\_WAIT

- 前面说过，IP头部中有一个**TTL字段**，是IP数据报文可以经过的**最大路由数目**，每经过一个处理他的路由器此值就会减1，当此值为0时数据报将会被丢弃，同时发送ICMP报文通知源主机
- **MSL是 M a x i m u m Maximum Maximum S e g m e n t Segment Segment L i f e t i m e Lifetime Lifetime的缩写，意为报文最大生存时间**，它是任何报文在网络上存在的最长时间，超过此时间报文将会被丢弃

这里，TIME\_WAIT被设置为了2MSL是因为**网络中可能存在来自发送方的数据包，当这些发送方的数据报文被接收方处理之后又会向对方发送响应，所以一来一回需要2倍时间**

**2MSL的时间是从客户端接收到FIN后发送ACK开始计时的。如果在TIME-WAIT时间内，因为客户端的ACK没有传输到服务器，客户端又接收到了服务器重发的FIN报文，那么2MSL会重新计时**

**在Linux系统中，一个MSL是30s、所以Linux系统停在TIME\_WAIT的时间为60s**  
其定义在Linux内核代码里的名称为TCP\_TIME`WAIT_LEN`

```cpp
#define TCP_TIMEWAIT_LEN (60*HZ) /* how long to wait to destroy TIME-WAIT state, about 60 seconds */
```

### B：为什么需要TIME\_WAIT

**①：防止旧连接的数据包**

假设TIME\_WAIT没有等待时间或者等待时间过多，会发生什么呢？如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2ff2fde5813648e7bf140605748f3e75.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)  
上图中，由于关闭连接前SEQ=301的报文被延迟了，如果此时有相同端口的TCP连接被复用后，被延长的SEQ=301抵达了客户端，**那么客户端有可能会接受这个过期的报文**，造成一些严重的问题

**所以TCP设计了2MSL的时间，足以让这两个方向上的数据包都自动丢弃，使原来连接的数据包在网络中自然消失，再出现的数据包一定是新建立的。**

**②：保证连接正确关闭**

**TIME\_WAIT的另一个作用就是等待足够的时间以确保最后的ACK让被动关闭方接收，从而帮助其正确关闭**

还是假设TIME\_WAIT没有等待时间或过短，此时又会造成什么问题呢？如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F155c0c10b88040c385d6a332cac8d682.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)  
可以看出，如果最后一个ACK报文传输丢失了，因为没有TIME\_WAIT或时间过短，**此时客户端就会直接进入`CLOSE`状态，服务端则会一直保持`LASE_ACK`状态**。那么此时当客户端发起建立连接的SYN请求后，服务端就会发送RST给客户端，**连接会被中止**

# 三：实践-理解TIMIE\_WAIT状态

- 请参考此文章代码写出该http服务器：[Linux网络编程之Http服务器](https://zhangxing-tech.blog.csdn.net/article/details/118698638)

如下有一个简单的http服务器，启动服务器后绑定8080端口，在正常情况下是可以绑定成功的，服务器开始运行

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F86a170a426024f11a6a2ff31c7afd632.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)  
然后使用浏览器访问该服务器，一般情况下是客户端主动断开连接。但是现在我们用`Ctrl+C`结束服务端程序，然后再次连接时，会发现无法再绑定这个端口。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1ddf95a87d5b4375b85e8e851031bde4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

这是因为，虽然服务端的应用程序终止了，但是TCP协议层的连接并没有完全断开，因此不能再次监听同样的端口。**主动关闭连接的一方，进入TIME\_WAIT状态**  
可以使用netstat命令查看  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb14a33c10ba74c3b85627a65c2250f92.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)

其实在服务端的TCP连接没有完全断开之前不允许重新监听，这样的设计在有些情况下是不合理的。  
服务器一般需要处理巨量的客户端连接（每个连接的生存时间可能很短，但是每秒都有很大数量的客户端请求），这个时候如果由服务器主动关闭连接，机会产生大量的TIME\_WAIT状态，由于请求量很大，就可能导致TIME\_WAIT的连接数很多，每个连接都会占用一个通信五元组（源ip，源端口，目的ip，目的端口，协议）其中服务器的ip和端口和协议是固定的。如果新到来的客户端连接的ip和端口和TIME\_WAIT占用连接重复了，就会出现很多问题

其实使用setsockopt\(\)，其中SO\_REUSEADDR为1，可以设置允许创建端口号相同但ip地址不同的多个socket描述符

```cpp
int opt=1;
setsockopt(listenfd,SOL_SOCKET,SO_reuseaddr,&opt,sizeof(opt));

```

在服务器中加入以下代码  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4d9e9e7e66674792815fdd01d6b03cda.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa90cbaffb8ed4fe7864f2b3bfb2bdd46.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125560517)