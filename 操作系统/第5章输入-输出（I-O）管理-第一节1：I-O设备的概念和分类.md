 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

**注意**：

- 本章内容和《计算机组成原理》中的“**输入输出系统**”联系较为紧密，如有需要请移步：[【专栏必读】王道考研408计算机组成原理万字笔记（有了它不需要你再做笔记了）：各章节内容概述导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162)
- 本章涉及内容知识点较为分散，重点内容是：**I/O设备的基本特性、I/O系统的特性、三种I/O控制方式、高速缓存与缓冲区、SPOOLing技术**
- 本章内容在考试中所占比例并不大，考察的可能性整体不大，如果考察则其难度一般很低，所以主要还是对于内容的多次复习

### 文章目录

- [一：什么是I/O设备](#IO_18)
- [二：I/O设备的分类](#IO_27)
- - [（1）按使用特性分类](#1_28)
  - [（2）按传输速率分类](#2_46)
  - [（3）按信息交换单位分类](#3_58)

---

操作系统需要实现**进程管理、内存管理、文件管理和设备管理**，其中前三个管理在前面都有详细介绍。对于设备管理来说，它却显得有点特殊，因为这些设备通常不会在集成在计算机内部，而是通过接口的方式与计算机连接

# 一：什么是I/O设备

现代计算机结构大致分为**主机**和**I/O设备（外设）**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0e99924747e34e41bd2762cd50feaf1f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384025)  
I/O\(Input/Output\)，意为输入和输出，**I/O设备就是可以将数据输入到计算机，或者可以接受计算机输出数据的外部设备**

常见的I/O设备如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa204ad41eb5d45fbaecd16facf828e04.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384025)

# 二：I/O设备的分类

## （1）按使用特性分类

**人机交互类外部设备：用于与计算机用户之间进行数据交互，如打印机、显示器、鼠标和键盘等**

- **这类设备的数据交换速度相对较慢，通常是以字节为单位进行的**

**存储设备：用于存储程序和数据，如磁盘、磁带、光盘等。**

- **这类设备的数据交换速度相对较快，通常是以字节组成的块为单位进行的**

**网络通信设备：用于与远程设备通信，如各种网络接口、调制解调器等**

- **这类设备速度介于前两类设备之间**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb68a1f6ba6054f3d983274e07ce0b1ba.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384025)

## （2）按传输速率分类

**低速设备：传输速率仅为每秒几字节到数百字节，如磁盘、鼠标等**

**中速设备：传输速率为每秒数千字节到数万字节，如行式打印机、激光打印机等**

**高速设备：传输速率在数百千字节至千兆字节，如磁带机、磁盘机、光盘机等**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5bc1330605d54783a72e5f1488b77f49.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384025)

## （3）按信息交换单位分类

**块设备：数据传输的单位是块；该类设备传输速率较高，可寻址（也即可以任意读/写任一块）；如磁盘等**

**字符设备：数据传输的单位是字符；该类设备传输速率较低，不可寻址，并且在输入/输出时常采用中断驱动的方式；如交互式终端机、打印机等**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd342726ab3a14d56a5f1c640d7b3240d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122384025)