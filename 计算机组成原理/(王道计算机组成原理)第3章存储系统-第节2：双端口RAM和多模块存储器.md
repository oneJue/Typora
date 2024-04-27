 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_9)
- [一：双端口RAM](#RAM_22)
- [二：多模块存储器-多体并行](#_38)
- - [（1）高位交叉编址](#1_48)
  - [（2）低位交叉编址](#2_64)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F779bef07c86a4423b1dcb00a5fff98f4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

# 本节思维导图

之前在[第一节：存储器分类、多级存储系统和存储器性能指标](https://blog.csdn.net/qq_39183034/article/details/119715863#t11) 这篇文章中讲到了存取周期的概念：：**存取周期又称读写周期或访问周期，它是指存储器进行一次完整的读写操作所需的全部时间，即连续两次独立访问存储操作（读或写操作）之间所需的最小时间间隔**。对于DRAM芯片，它的恢复时间是比较长的，有时有可能会到达存取周期的几倍，而现代计算机CPU通常都是多核的，**那么这么多CPU核心究竟应该怎样访问主存才能解决恢复时间过长带来的问题呢**？主要有两种思路

- 双端口RAM
- 多模块存储器

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd89825b477684c70a145b37d6c780720.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

# 一：双端口RAM

**双端口RAM：是指同一个存储器有左右两个独立的端口，分别具有两组相互独立的地址线、数据线和读写控制线，允许两个独立的控制器同时异步地访问存储单元。该项技术可以优化多核CPU访问一根内存条的速度**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb6c7a7f392324d50bf6d1f27a2acda58.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

**两个端口对同一主存操作时无外乎有以下四种情况**

- **两个端口不同时对同一地址单元读出数据**：没有错误
- **两个端口同时对同一地址单元读出数据**：没有错误
- **两个端口同时对同一地址单元写入数据**：发生写入错误
- **两个端口同时对同一地址单元操作，一个写入，一个读出**：发生读出错误

其解决方法为：**置“忙”信号 B ‾ U ‾ S ‾ Y ‾ \\overline B\\overline U\\overline S\\overline Y BUSY为0**，由判断逻辑决定暂时关闭一个端口（延时）。**未被关闭的端口正常访问，被关闭的延长一个很短的时间段后再访问**

# 二：多模块存储器-多体并行

**多模块存储器：多体并行存储器由多体模块组成。每个模块都有相同的容量和存取速度，各模块都有独立的读写控制电路、地址寄存器和数据寄存器。它们既能并行工作，又能交叉工作。有两种编址方式**

- **高位交叉编址**
- **低位交叉编址**

## （1）高位交叉编址

**高位交叉编址：高位地址表示体号，低位地址为体内地址。在这种编址方式下，总是把低的体内地址送到由高位体号所确定的模块内进行译码。访问一个连续的主存块时，总是先在一个模块内访问，等到该模块访问完才转到下一个模块访问，CPU总是按顺序访问存储模块，存储模块不能并行访问，因此不能提高存储器的吞吐率**

如下图，存储器共有4个模块 M 0 − M 3 M\_\{0\}-M\_\{3\} M0​−M3​（可以将其理解为“4根内存条”），按照这种方式编址后，**地址前两位（高位）表示的是某根内存条，后面部分（低位）表示的是该内存条中的具体地址**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdf799c687b2149de90b9378e16df5e1d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

假设每个存储体的存取周期为 T T T，存取时间为 r r r，且T=4r。如果多体存储器采用高位交叉编址，那么CPU真正花在读数据上的时间只有 r r r，但却要再花费3r的时间用来等待，效率不高。**也就是说连续读取 n n n个存储字，就要耗时 n T nT nT**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd52e5e7a49dd4bf28c4ae09f2c1f58ee.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

## （2）低位交叉编址

**低位交叉编址：低位地址表示体号，高位地址为体内地址。在这种编址方式下，总是把高位的体内地址送到由低位体号所确定的模块内进行译码。程序连续存放在相邻的模块中，将采用此编址方式的存储器称为交叉存储器。采用低位交叉编址后，可在不改变每个模块存取周期的前提下，采用流水线的方式并行存取，提高存储器带宽**

如下图，存储器共有4个模块 M 0 − M 3 M\_\{0\}-M\_\{3\} M0​−M3​（可以将其理解为“4根内存条”），**每个模块的模块号=单元地址\%4**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F53da619c722e432e81abdc91d11c5e0e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

**采用低位交叉编址的存储器连续读取 n n n个存储字，耗时为 T + \( n − 1 \) r T+\(n-1\)r T+\(n−1\)r**。CPU每经过时间 r r r后会启动下一模块，**因此交叉存储器要求其模块数必须大于等于 T r \\frac\{T\}\{r\} rT​**，以保证某模块后经过 T T T时间后再次启动该模块时，其上次的存取周期已到（也就是已经恢复）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4a38284516204b34a54fc9efc7b2bbf4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd32475ffd0814c949ff214ee2a02bd65.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119870186)