 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：UDP的主要特点](#UDP_15)
- [二：UDP首部格式](#UDP_26)
- [三：UDP校验](#UDP_42)

UDP协议在IP之上只增加了两个最基本的服务：**复用分用和差错检测**，剩下只做一件事情：**尽全力交付，能给多少就给多少**。当开发者选择UDP协议时，程序**几乎直接与IP打交道**

# 一：UDP的主要特点

虽然UDP提供的是不可靠的服务，但是它具有很多的优点或者特点让其在某些应用场景中仍然熠熠生辉

- **UDP无需建立连接、不会引入建立连接时的时延**：
- **无连接状态**：TCP需要在端系统中维护连接状态。此连接状态包括接收和发送缓存、拥塞控制参数和序号与确认号的参数。而UDP不维护连接状态，也不跟踪这些参数。因此，某些专用应用服务器使用UDP时，一般都能支持更多的活动客户机
- **分组首部开销小**：TCP有20B的首部开销，而UDP仅有8B的开销
- **无拥塞控制**：因此网络中的拥塞不会影响主机的发送效率。某些实时应用要求以稳定的速度发送，能容忍一些数据的丢失，但不允许有较大的时延，而UDP正好满足这些应用的需求

# 二：UDP首部格式

**UDP数据报包含两部分: UDP 首部和用户数据，整个UDP数据报作为IP数据报的数据部分封装在IP数据报中，如下图所示。UDP首部有8B，由4个字段组成，每个字段的长度都是2B**

- **源端口**：源端口号。在需要对方回信时选用，**不需要时可用全0**
- **目的端口**：目的端口号。这在终**点交付**报文时必须使用到
- **长度**：UDP数据报的长度\(包括首部和数据\)，其最小值是8 \(仅有首部\)
- **校验和**：检测UDP数据报在**传输中是否有错**。有错就丢弃。该字段是可选的，当源主机不想计算校验和时，则直接令该字段为全0

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1328fb8ee2c0402485d1fabe64bbf5a2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125522568)

当传输层从IP层收到UDP数据报时，就根据首部中的**目的端口**，把UDP数据报通过相应的端口上交给应用进程

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa89acb97ce2b4bb9b278993b14ac6831.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125522568)

# 三：UDP校验

在计算校验和时，要在UDP数据报之前增加**12B的伪首部**，伪首部并不是UDP的真正首部，只是在计算校验和时，**临时添加在UDP数据报的前面**，得到一个临时的UDP数据报。**校验和就是按照这个临时的UDP数据报计算的**。伪首部既不向下传送也不向上递交，而仅为了计算校验和。这样的校验和，既检查了UDP数据报，又对IP数据报的源IP地址和目的IP地址进行了检验

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff299d4df21b848808fb77ebe6c90847b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125522568)

UDP校验和的计算方法和IP数据报首部校验和的计算方法相似，**都使用二进制反码运算求和再取反**。但不同的是，**IP数据报的校验和只检验IP数据报的首部，但UDP的校验和则检查首部和数据部分**

- 发送方首先把**全零放入校验和字段并添加伪首部**，然后把UDP数据报视为许多16位的字连接起来
- 若UDP数据报的数据部分**不是偶数个字节**，则要在数据部分**末尾增加一个全零字节\(但此字节不发送\)**
- 接下来按**二进制反码计算出这些16位字的和**，并将此和的二进制反码写入校验和字段
- 接收方把收到的UDP数据报加上伪首部\(如果不为偶数个字节，那么还需要**补上全零字节\)后**，按二进制反码计算出这些16位字的和
- 当无差错时其结果应全为1,否则表明有差错出现，接收方就**应该丢弃这个UDP数据报**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F54d9fc28c5874722810101acb0603012.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125522568)