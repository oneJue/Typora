 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_7)
- [一：存储器元件不同导致的特性差异](#_24)
- - [（1）栅极电容](#1_33)
  - [（2）双稳态触发器](#2_47)
  - [（3）SRAM和DRAM对比](#3SRAMDRAM_70)
- [二：DRAM的刷新](#DRAM_102)
- - [（1）译码器需要使用行列地址](#1_106)
  - [（2）分散刷新、集中刷新和异步刷新](#2_124)
  - - [A：DRAM刷新](#ADRAM_126)
    - [B：分散刷新、集中刷新和异步刷新特点](#B_136)
    - [C：分散刷新、集中刷新和异步刷新优缺点](#C_156)
- [三：DRAM的地址线复用技术](#DRAM_177)

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F53a6186e259340c09fa8d6a690ca1da1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

**本节会在上节的基础上，介绍两种重要的存储器芯片：DRAM和SRAM**

- **DRAM\(Dynamic-Random- Access -Memory\)：动态DRAM，主要用于主存**
- **SRAM\(Static- Random -Access -Memory\)：静态RAM，主要用于Cache**

**注意**：DRAM芯片已经过时了，现在主存通常采用SDRAM（如DDR3和DDR4）

# 一：存储器元件不同导致的特性差异

上一节介绍的芯片实则就是**DRAM芯片**，主要被用于制作主存。**其实DRAM芯片和SRAM芯片的核心区别点就在于他们的存储元制作材料不一样**

- **DRAM**：使用**栅极电容**
- **SRAM**：使用**双稳态触发器**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb149aeebcb1f491e8ab8312989fe6c7a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

## （1）栅极电容

**栅极电容：当给字选择线一高电平时，MOS管会接通，然后给数据线一高电平，由于电容一端接地，因此电容板之间产生电压差，于是电荷被“写入”电容**

- **1**：表示电容内**存储**了电荷
- **0**：表示电容内**未存储**电荷

**在读出时，如果电容里面有电荷，那么当MOS管接通后，一定会在数据线位置检测到电流信号，反之则不会，分别对应数据1和数据0**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4f5105f61a2b40df9695b6c01f032326.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

## （2）双稳态触发器

- **关于双稳态触发器具体原理请点击链接跳转**：[双稳态触发器](http://www.360doc.com/content/19/0827/18/2289804_857407763.shtml)

**双稳态触发器：是一种具有记忆功能的逻辑单元电路，它能储存一位二进制码。它有两个稳定的工作状态，在外加信号触发下电路可从一种稳定的工作状态转换到另一种稳定的工作状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7f43fae6add34d6cbf20ff3c2703d877.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

**相比栅极电容，双稳态触发器有两根数据线。当给字选择线一高电平信号后**

- **如果里面存储的是1**，那么将会在BLX这条线上输出低电平信号（左边没有）
- **如果存储的是0**，那么将会在BL这条线上输出低电平信号（右边没有）

**同时在写入时**

- **如果要写入数据0**：那么只需要给BL一**低电平信号**，同时给BLX一**高电平信号**
- **如果要写入数据1**，那么只需要给BL一**高电平信号**，同时给BLX一**低电平信号**

## （3）SRAM和DRAM对比

**关于读写速度**

- **DRAM使用栅极电容的充放电来完成读写操作**：电容的物理特性就决定了其充放电是一种**破坏性读出**，读出后应该**有重写操**作，也就是需要重新充电，也称之为**再生**，读写**速度较慢**
- **SRAM使用双稳态触发器**：在读写数据时，触发器的状态是**保持稳定的**，因此属于**非破坏性读出**，无需进行**重写操作**，读写速度**也就更快**

**2：关于成本和功耗**

- **DRAM**：单个存储元制造成本**低**，集成度**高**，功耗**低**
- **SRAM**：单个存储元制造成本更**高**，集成度**低**，功耗**大**

**3：其它区别**

| 类型/特点 | SRAM（静态RAM） | DRAM（动态RAM） |
| --- | --- | --- |
| **存储原理** | 触发器 | 电容 |
| **是否是破坏性读出** | 否 | 是 |
| **是否需要重写** | 否 | 是 |
| **运行速度** | 快 | 慢 |
| **集成度** | 低 | 高 |
| **发热量** | 大 | 小 |
| **成本** | 高 | 低 |
| **是否是易失性存储器** | 是 | 是 |
| **是否需要“刷新”** | 否 | 是 |
| **送行列地址** | 同时送 | 分两次 |
| **用途** | Cache | 主存 |

- 刷新：**电容里面的电荷不能永久存在**，一般只能维持2ms，因此即使不断电，2ms后信息也会丢失，因此对于DRAM需要“刷新”，也就是再充电；而SRAM只要不断电，触发器的状态就不会改变

# 二：DRAM的刷新

## （1）译码器需要使用行列地址

上一节说到了译码器的作用：**把某一位的地址，转化为相应的选通线的高电平信号， n n n位地址就对应 2 n 2\^\{n\} 2n个选通线**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc9a21d2ed73d4a9c98495ba041489dfb.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)  
**当地址数变多时，选通线数量级将会非常大**，例如仅20个地址就需要 2 20 = 1048576 2\^\{20\}=1 048 576 220\=1048576根选通线，这已接近百万了

**解决方法就是：将原来的单纯的一维的地址，改变为行列地址，也就是一个矩阵，分别交给行地址译码器和列地址译码器管理，这样的话每个译码器只需处理一半的地址信息，也就是1024根选通线**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdaa4ab5301994aa1a0b4dac6e1eb8711.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

例如地址`0000` `0000`，如果采用之前的方案，那么经过译码器译码后，第0根选通线会被选中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F89988eece82b420da9e5bce77555e256.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)  
而如果采用行列译码器的方案，**地址`0000` `0000`的低四位将会交给列地址译码器，高四位将会交给行地址译码，每个存储单元只有列选通线和行选通线同时被选中时才能被选中**，因此`(1,1)`位置的存储单元此时会被选中

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F21bea4895a82455abc957db9a176a4af.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)  
**对于八位地址，原本需要使用 2 8 = 256 2\^\{8\}=256 28\=256根选通线，而现在只需要 2 4 + 2 4 = 16 + 16 = 32 2\^\{4\}+2\^\{4\}=16+16=32 24+24\=16+16\=32根选通线，所以使用行列地址的本质就是要减少选通线数量**

## （2）分散刷新、集中刷新和异步刷新

### A：DRAM刷新

**关于DRAM的刷新，这里有4个问题需要回答**

- **多久刷新一次**：一般为2ms
- **每次刷新多少存储单元**：以行为单位，每次刷新一行存储单元
- **如何刷新**：有硬件支持，读出一行的信息后重新写入，占用1个读写周期
- **什么时候刷新**：分散刷新、集中刷新和异步刷新（接下来介绍）

### B：分散刷新、集中刷新和异步刷新特点

假设DRAM内部结构排列形式为128×128，存储周期为0.5 u s us us，电容最多坚持2ms，因此对应2ms/0.5 u s us us\=4000个周期，有128行，刷新每一行都需要0.5 u s us us，同时注意以下内容

- 刷新对CPU是透明的，也即**刷新不依赖于外部的访问**
- DRAM刷新单位是行，因此**刷新操作时仅需要行地址**
- 刷新操作类似于读操作，但又有所不同：**刷新操作仅给栅极电容补充电荷，不需要信息输出，另外刷新时不需要进行选片，即整个存储器中的所有芯片同时被刷新**

一共有**分散刷新、集中刷新和异步刷新**这三种方式

- **分散刷新**：把对每行的刷新分散到各个工作周期当中，这样，一个存储器的系统工作周期就分为了两个部分，**前半部分用于正常读写或保持；后半部分用于刷新某一行**。这种刷新方式增加了系统的存取周期，增加为1 u s us us  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0841211908b34fa883a2a9c5b6028825.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

- **集中刷新**：**是指利用一段固定的时间，依次对存储器的所有行进行逐一再生，存储周期不变**，在刷新期间内会停止对存储器的访问，因此称之为“死时间”，又称访存“死区”  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F54bb28fa56294755a262e01b80d97f2d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

- **异步刷新**：它是前两种刷新方式的结合。**具体做法是用刷新周期除以行数，得到两次刷新操作之间的时间间隔t（2ms/128=15.6 u s us us）**，接着利用逻辑电路每隔该时间间隔t\(15.6 u s us us\)产生一次刷新请求，因此每15.6 u s us us内会有0.5 u s us us的死时间。所以死时间会分散在整个过程中，而且可以在译码阶段刷新  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F26b0139c787c4fe6832c7ca98cd8289e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)

### C：分散刷新、集中刷新和异步刷新优缺点

**集中刷新**

- **优点**：读写操作不受刷新工作的影响，因此系统的**存取速度较高**
- **缺点**：是在集中刷新期间（死区）不能访问存储器

**分散刷新**

- **优点**：是**没有死区**
- **缺点**：是加长了系统的存取周期**降低了整体速度**

**异步刷新**：综合最优

- 可以避免使CPU连续等待过长的时间，而且减少了刷新次数，从根本上**提高了整机的工作效率**；
- 同时如果将刷新安排在不需要访问存储器的译码阶段，则**既不会加长存取周期，又不会产生“死时间”**，这是分散刷新的方式的发展，也称之为“**透明刷新**”

# 三：DRAM的地址线复用技术

- 前面说过SRAM需要同时送行列地址，也即**行列地址信息会同时丢给行译码器和列译码器**

而DRAM由于用于主存，所以容量可能较大，因此地址线可能也会更多，**所以为了使地址线电路变得更简单，会采用一种地址线的复用技术，也就是分两次送**

**这种技术可以使行列地址分两次前后进行传送，传送时只需要一半地址，先传送至缓冲区，再传送给译码器即可，这样会使地址线更少**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1672aa8e558e402e82efc536eef6553e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119779144)