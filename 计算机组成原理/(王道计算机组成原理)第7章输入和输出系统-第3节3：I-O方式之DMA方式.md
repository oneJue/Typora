 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [思维导图](#_6)
- [一：什么是DMA方式](#DMA_11)
- [二：DMA控制器组成](#DMA_26)
- [三：DMA传送过程](#DMA_45)
- - [（1）预处理](#1_46)
  - [（2）数据传送](#2_57)
  - [（3）后处理](#3_68)
- [四：DMA方式的特点](#DMA_80)
- [五：DMA传送方式](#DMA_95)
- - [（1）停止CPU访问主存](#1CPU_99)
  - [（2）DMA和CPU交替访问主存](#2DMACPU_104)
  - [（3）周期挪用（周期窃取）](#3_109)
- [六：DMA和中断对比](#DMA_118)

# 思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7171c4f636a44812a3c32cc803110b9a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

# 一：什么是DMA方式

**DMA方式：DMA方式是一种完全由硬件进行成组信息传送的控制方式，它具有程序中断方式的优点，即在数据准备阶段，CPU与外设并行工作。DMA方式在外设与内存之间开辟一条“直接数据通道”，信息传送不再经过CPU，降低了CPU在传送数据时的开销，因此称为直接存储器存取方式。由于数据传送不需要经过CPU，也就不需要保护，恢复CPU现场等繁琐操作。此种方式适用于磁盘机、磁带机等高速设备大批量数据的传送，它的硬件开销较大**

**如下图，CPU向DMA控制器指明要输入还是输出、要传送多少个数据、以及数据在主存、外设中的地址**

- **传送前**：接受外设发出的DMA请求（外设传送一个字的请求），并向CPU发出总线请求
- **传送前**：CPU响应总线请求，发出总线响应信号，接管总线控制权，进入DMA操作周期
- **传送时**：确定传送数据的主存单元地址及长度，并能自动修改主存地址计数和传送长度计数
- **传送时**：规定数据在主存和外设间的传送方向，发出读写等控制信号，执行数据传送操作
- **传送后**：向CPU报告DMA操作的结束

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F906765933ea04900b794c2ca107850de.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

# 二：DMA控制器组成

**DMA控制器组成：**

- **主存地址寄存器**：简称AR，存放**要交换数据的主存地址**
- **传送长度计数器**：简称WC，记录传送**数据的长度**，计数溢出时，数据即传送完毕，自动发中断请求信号
- **数据缓冲寄存器**：暂存**每次传送的数据**
- **DMA请求触发器**：每当I/O设备准备好数据，给出一个控制信号，**使DMA请求触发器置位**
- **控制/状态逻辑**：由控制和时序电路及状态标志组成，用于**指定传送方向，修改传送参数**，并对DMA请求信号和CPU相应信号进行协调和同步
- **中断机构**：当一个数据块传送完毕之后触发中断机构，向CPU提出中断请求

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff7e3294e997d4923a4f7d5e0682ed861.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

**注意**：在DMA传送过程中，DMA控制器将接管CPU的地址总线、数据总线和控制总线，CPU的主存控制信号被禁止使用，而当DMA传送结束之后，将恢复CPU的一切权利并开始执行其操作，由此可见，DMA控制器必须具有控制系统总线的能力

# 三：DMA传送过程

## （1）预处理

**DMA控制器组成：由CPU完成一些必要的准备工作。首先,CPU执行几条I/O指令，用以测试I/O设备状态，再向DMA控制器的有关寄存器置初值、设置传送方向，启动该设备等等。然后CPU继续执行原来的程序，直到I/O设备准备好发送的数据（输入情况）或接受的数据（输出情况）时，I/O设备向DMA控制器发送DMA请求，再由DMA控制器向CPU发送总线请求，用以传输数据**

- 主存起始地址->AR
- I/O设备地址->DAR
- 传送数据个数->WC
- 启动I/O设备

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F670647b5ce264d97a88c8756b7cfac6c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

## （2）数据传送

**数据传送：DMA的数据传输可以单字节（或字）为基本单位，也可以数据块为基本单位。对于以数据块为单位的传送（如硬盘），DMA占用总线后的数据输入和输出操作都是通过循环来实现的。需要指出的是，这一循环也是由DMA控制器而非通过CPU执行程序实现的，即数据传送阶段完全由DMA（硬件）控制**

- 设备将输入写入DR，发出DMA请求
- 控制逻辑检测到一个数据后，向CPU发出总线请求，CPU给予反馈信号
- DMA控制器接管总线，进行数据传送
- 主存地址计数器+1，长度计数器+1
- 传输完多个字后，长度计数器溢出，溢出信号传送给中断机构，向CPU发出中断请求

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F054252a528f94735ad6f7d7431805099.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

## （3）后处理

**后处理：DMA控制器向CPU发出中断请求：CPU执行中断服务程序做DMA结束处理，包括校验送入主存的数据是否正确，测试传送过程中是否出错（如果出错则转入诊断程序）以及决定是否继续使用DMA传送其他数据块等等**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4478501367c24b769ff7c63ddbd36ea1.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

**接着，CPU继续执行主程序**

# 四：DMA方式的特点

主存和DMA接口之间有一条“直接数据通路”，由于DMA方式传送数据不需要经过CPU，因此不必中断线性程序，**I/O主机并行工作，程序和传送并行工作**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F321842c3c08d4502a5c49c566abbf648.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

**DMA方式特点：**

- 它使主存与CPU的固定联系脱钩，**主存既可以被CPU访问，又可以被外设访问**
- 在数据块传送时，主存地址的确定、传送数据的计数都有**硬件电路直接实现**
- 主存开辟专门的**数据缓冲区**，及时供给和接受外设的数据
- DMA传送速度快，CPU和外设并行工作，**提高了系统效率**
- **DMA在传送开始前要通过程序进行预处理，结束之后要通过中断方式进行后处理**

# 五：DMA传送方式

**DMA传送方式：主存和DMA控制器之间有一条数据通路，因此主存和I/O设备之间交换信息时，不通过CPU。但是当I/O设备和CPU同时访问主存时，可能发生冲突，为了有效地使用主存，DMA控制器和CPU通常采用以下三种方法使用主存**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe9dd65f682ad49dbb2807dc9f7ff50ae.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

## （1）停止CPU访问主存

**停止CPU访问主存**：这种方式是当外设需要传送成组数据时，**由DMA接口向CPU发送一个信号，要求CPU放弃地址线、数据线和有关控制线的使用权**，DMA接口获得总线控制权后，开始进行数据传送。数据传送结束后，DMA接口通知CPU可以使用主存,并把总线控制权交还给CPU。**在这种传送过程中，CPU基本处于不工作状态或保持原始状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd9ba46c45f08493bb6c4c404e1d49156.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

## （2）DMA和CPU交替访问主存

**DMA和CPU交替访问主存**：DMA与CPU交替访存，这种方式适用于**CPU的工作周期比主存存取周期长**的情况。例如，若CPU的工作周期是1.2μs，主存的存取周期小于0.6μs，则可将一个CPU周期分为C1和C2两个周期,其中C1专供DMA访存，C2 专供CPU访存。这种方式不需要总线使用权的申请、建立和归还过程，总线使用权是通过C1和C2**分时控制的**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbdda0f8ee83649aa924010f2d09a011f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

## （3）周期挪用（周期窃取）

**周期挪用（周期窃取）**：这种方式是前面两种的折中操作。**当I/O设备没有DMA请求时，CPU按程序的要求访问主存，一旦I/O设备有了DMA请求，就会有以下三种情况**

- \*\***CPU不再访存**：因此I/O的访存请求与CPU未发生冲突
- CPU正在访存\*\*：此时必须等待存取周期结束后，CPU再将总线占有权让出
- **I/O和CPU同时请求访存，出现访存冲突**：此时CPU要暂时让出总线占有权，由I/O设备挪用一个或多个存取周期

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb83a056746aa4c56956ae0d5b4fe1f8a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)

# 六：DMA和中断对比

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffc8d3e23f2154c37b58cf53f455b2ed8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120721538)