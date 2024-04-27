 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：序号](#_21)
- [二：确认](#_32)
- [三：重传](#_73)
- - [（1）超时重传](#1_81)
  - [（2）冗余ACK（快速重传）](#2ACK_95)

IP层是不可靠的，所以TCP的任务就是在IP层上提供一种可靠的数据传输服务。**TCP提供的可靠数据传输服务保证接收方进程从缓存区读出的字节流与发送方发出的字节流完全一样**，使用以下四种机制来达到此目的

- 校验（同UDP校验机制，见[（王道考研计算机网络）第五章传输层-第二节：UDP协议](https://blog.csdn.net/qq_39183034/article/details/125522568?spm=1001.2014.3001.5501)）
- 序号
- 确认
- 重传

# 一：序号

**序号：用于保证数据能有序提交给应用层。TCP会将数据视为一个无结构但有序的字节流，序号建立在传送的字节流之上，而不建立在报文段之上。TCP连接传送的数据流中的每个字节都编上一个序号。序号字段的值是指本报文段所发送的数据的第一个字节的序号**

如下图，假设 A A A和 B B B之间建立了一条TCP连接， A A A的发送缓存区中共有10B，序号从0开始标号，第一个报文包含第0\~2个字节，则该TCP报文段的序号是0，第二个报文段的序号是3

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1489f730312c4a2c86dd014cec231295.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

# 二：确认

**确认：TCP首部的确认号是期望收到对方的下一个报文段的数据的第一个字节的序号**

如下图，发送方先发送【123】给接收方

- 为防止丢失，在确认【123】到达之前接收方要一直保存

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8d7f5477537e4b22a386ac7b848e1408.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

接收方在收到【123】后返回确认给发送方，由于收到序号为3，下一个要收到的应该是4，所以该确认报文确认号字段为4

- 有时它会把该报文结合在一起一并发送给发送方（捎带确认）

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd96916dc16b947b5ad7fbd8ba3f69671.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

发送方在收到确认报文4后，获悉接收方已经正确接收，所以在其缓存中删除【123】

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcb14199c66af4151bb8219ee394678cd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

发送继续发送【456】和【78】，【78】正确接收，但【456】报文由于种种原因未能及时送达

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F965f0303e7594dc9a4b77c98249e96e7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

此时TCP会采用**累计确认**机制，虽然【78】已经接收，但这里确认号字段仍然为4

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd5a5a659b0574c6f8ef8706d18a8ae0f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

接着发送方就会继续重传【456】，【456】正确到达后，下一个确认号字段将会填9

# 三：重传

**重传：有两种事件会触发TCP重传机制**

- **超时**
- **冗余ACK**

## （1）超时重传

**超时重传：TCP每发送一个报文段，就对这个报文段设置一次计时器。计时器设置的重传时间到期但还未收到确认时，就重传这一报文段**

- **RTTs**：为计算超时计时器的重传时间，TCP采用一种**自适应算法**，它**记录一个报文段发出的时间，以及收到相应确认的时间**，这两个时间之差称为**报文段的往返时间\(RTT\)**。 TCP保留了RTT的一个加权平均往返时间RTTs，它会随新测量RTT样本值的变化而变化。显然，**超时计时器设置的超时重传时间\( RTO\) 应略大于RTTs**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F01eee4da492b431bb489aff6ec4f58c0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

TCP会采用以下公式计算RTO

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F85c6b68fd2d245c299f23562e41ca741.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)

## （2）冗余ACK（快速重传）

- 超时触发重传存在的一个问题是**超时周期往往太长**。所幸的是，发送方通常可在超时事件发生之前通过注意所谓的**冗余ACK**来较好地检测丢包情况

**冗余ACK：冗余ACK就是再次确认某个报文段的ACK，而发送方先前已经收到过该报文段的确认**

如下图所示

- 第一份`Seq1`先送到了，于是就`Ack`返回2
- 结果`Seq2`因为某些原因没收到，`Seq3` 到达了，于是还是`Ack`返回2
- 后面的`Seq4`和`Seq5`都到了，但还是`Ack`返回2，因为`Seq2`还是没有收到
- 发送端收到了三个`Ack = 2`的确认，知道了`Seq2`还没有收到，就会在定时器过期之前，重传丢失的`Seq2`
- 最后，收到了`Seq2`，此时因为`Seq3`、`Seq4`、`Seq5` 都收到了，于是`Ack`返回6

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F08213ffbac8c444e852648aa2c3844f7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125599599)