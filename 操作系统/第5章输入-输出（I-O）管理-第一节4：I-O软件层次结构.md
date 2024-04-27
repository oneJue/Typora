 

[专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：用户层软件](#_20)
- [二：设备独立性（无关性）软件](#_32)
- [三：设备驱动程序](#_45)
- - [（1）为什么需要驱动](#1_46)
  - [（2）功能](#2_58)
- [四：中断处理程序](#_65)
- [五：硬件设备](#_71)

I/O软件是操作系统中很特别的存在

- **它向下与硬件有着密切的联系，向上又与用户交互**
- **它与进程管理、存储器管理、文件管理都有着一定的联系**

为了使复杂的I/O软件具有清晰的结构，**在I/O软件中普遍采用层次式结构，将系统输入/输出功能组织成一系列的细节，每层都利用其下次提供的服务，完成输入/输出功能中的某些子功能，并屏蔽这些功能的细节，然后向高层服务**

一般会将层次划分如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1b93804220024bf587981ab1477a9e2e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

# 一：用户层软件

**向上**：用户层软件实现了**与用户交互的接口**，用户可以直接使用该层提供的、与I/O操作相关的库函数对设备进行操作

- 例如，使用C语言开发软件，可以使用`printf`函数向屏幕输出信息

**向下：** 接着，用户层软件就会负责将用户的请求翻译为等价的**系统调用**

- 此时，库函数`printf`就会在其内部调用`write()`系统调用
- 需要注意`printf`内部实现其实较为复杂，并不是简简单单的调用一个`write()`就完事了  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd03a4537a0804976878f1eeb28833b23.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

# 二：设备独立性（无关性）软件

**向上**：向用户层提供统一的**系统调用接口**

**向下**：主要实现

- **设备保护**：设备会被看作为一种特殊的文件，通过赋予文件权限也能间接保护设备
- **差错处理**
- **设备的分配与回收**
- **数据缓冲区管理**
- **建立逻辑设备名到物理设备名的映射关系；根据设备类型调用相应的驱动程序**：设备独立性软件需要通过 **逻辑设备表（LUT）** 来确定逻辑设备对应的物理设备，并找到该设备对应的设备驱动程序  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F290848b417f94344821a9ae00e06baee.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

# 三：设备驱动程序

## （1）为什么需要驱动

虽然I/O控制器屏蔽了设备的众多细节，但是每种设备的控制器的寄存器、缓冲区等使用模式都是不同的

- 例如惠普的打印机和佳能的打印机虽然都是打印机，但是这两个厂商生产的打印机其参数、使用方式都有很大的不同。**只有在其对应的驱动下，将不同打印机不同的指令翻译过来才有可能实现统一控制**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F938b47eed08841409453a170600549ba.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

**I/O控制器不属于操作系统范畴，而属于硬件；设备驱动程序属于操作系统，操作系统的内核代码可以像本地调用一样使用设备驱动程序的接口，而设备驱动程序是面向设备控制器的代码，它发出操控设备控制器的指令后，才可以操作设备控制器**

**不同的设备控制器虽然功能不同，但是设备驱动程序会提供统一接口给操作系统，这样不同设备驱动程序，就可以以相同方式接入操作系统**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9b1df46840b54ff7830805e4150184e8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

## （2）功能

**向上**：向上层提供一组标准接口，设备具体的差别**被设备驱动程序封装**，用于接受上层发来的抽象I/O请求，如`read()`和`write()`

**向下**：转换为具体要求后，发送给I/O控制器，控制设备工作

# 四：中断处理程序

**当I/O完成之后，I/O控制器会发出一个中断信号，系统会根据中断信号类型转到中断处理程序并执行，流程如下**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F51af3aab58b9443c99ed33339c884bd3.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122404649)

# 五：硬件设备

I/O设备通常包括一个**机械部件**和一个**电子部件**。为了达到设计的模块性和通用性，一般将其分开