 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：I/O控制器](#IO_6)
- [二：I/O控制器功能](#IO_16)
- [三：I/O控制器组成](#IO_28)
- [四：I/O端口编址](#IO_45)
- - [（1）统一编址](#1_47)
  - [（2）独立编址](#2_55)

# 一：I/O控制器

我们的电脑可以接入非常多的I/O设备，这些设备功能各异，控制方式更是大不相同，那么操作系统是如何实现这么多设备的统一管理呢？

为了屏蔽设备之间的差异，**每个设备都有一个叫做I/O控制器（设备控制器）的组件**， 比如硬盘就有硬盘控制器，显示器就有视频控制器等

- 操作系统控制I/O控制器，I/O控制器控制设备机械部件

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd1e3635621b9458c8ed713b1876301d6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384448)

# 二：I/O控制器功能

**I/O控制器内部含有三种寄存器，分别用于实现三种功能**

**控制寄存器：用于接受和识别CPU发出的命令；如CPU的发出的`read/write`命令**

**状态寄存器：用于向CPU报告设备的状态；如1表示空闲，0表示忙碌**

**数据寄存器：用于暂存CPU或设备发来的数据（也即数据交换功能）；如打印的内容是“Hello”，CPU就要先发送一个H字符给对应的设备**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb02db3aed8e04110bd7320ba1a8030e9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384448)  
**另外，为了区分I/O控制器中的各个寄存器，也需要给各个寄存器设置一个特定的地址。I/O控制器通过CPU提供的地址来判断需要读/写的是哪个寄存器，也即I/O控制器还需要实现地址识别的功能**

# 三：I/O控制器组成

I/O控制器的核心部件便是那三个寄存器，但仅仅有这三个寄存器是无法实现操作系统与外设的交互的

I/O控制器组成如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb026bf745c1d44d890d9d41148c4796e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384448)  
其中：

- **CPU与控制器的接口**：用于实现CPU与控制器之间的通信。**CPU通过控制器发出命令;通过地址线指明要操作的设备；通过数据线来取出或放入数据**
- **控制器与设备的接口**：用于实现控制器与设备之间的通信；**控制器要向设备发出控制信息；设备要反馈自己的状态**
- **I/O逻辑**：负责接收和**识别CPU的各种命令**，并负责**对设备发出命令**

需要注意以下两点

- **一个I/O控制器可能会对应多个设备**
- **数据/控制/状态寄存器可能有多个，且这些寄存器都要有相应的地址，才便于CPU操作**。对其编址的方式见下个标题

# 四：I/O端口编址

## （1）统一编址

又称**存储器映射方式**，是把**I/O端口当作存储器的单元进行地址分配**，这种方式CPU不需要设置专门的I/O指令，用统一的访存指令就可以访问I/O端口  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9f78b1cf58ad4cf5adeac494d6d1efee.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_18%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384448)

- **优点**：不需要专门的输入输出指令，可使CPU访问I/O操作更加灵活、方便，还可以使端口有较大的编址空间
- **缺点**：端口占用存储器地址，使内存容量变小，而且利用存储器编址的I/O设备进行数据输入输出操作，执行速度较慢

## （2）独立编址

又称为**I/O映射方式**。I/O端口的地址空间与主存地址空间是两个独立的地址空间，因而无法从地址码的形式上区分，需要设置专门的I/O指令来访问I/O端口  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5e39693729d94541b8235cac7551b38a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_18%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384448)

- **优点**：输入输出指令与存储器指令有明显区别，程序编制清晰，便于理解
- **缺点**：输入输出指令少，一般只能对端口进行传送操作，尤其需要CPU提供存储器读写，I/O设置读写两组控制信号，增加了控制的复杂性