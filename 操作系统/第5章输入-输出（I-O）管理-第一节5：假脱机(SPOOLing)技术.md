 

[专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：脱机技术](#_4)
- [二：假脱机技术\(SPOOLing技术\)](#SPOOLing_17)
- - [（1）输入井和输出井](#1_22)
  - [（2）输入缓冲区和输出缓冲区](#2_29)
  - [（3）输入进程和输出进程](#3_36)
- [三：SPOOLing技术实例——共享打印机](#SPOOLing_41)

# 一：脱机技术

在[\(王道408考研操作系统\)第一章计算机系统概述-第一节2：操作系统的发展史](https://blog.csdn.net/qq_39183034/article/details/120810537?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164181821616780274115646%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=164181821616780274115646&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-2-120810537.nonecase&utm_term=%E8%84%B1%E6%9C%BA&spm=1018.2226.3001.4450)这一节提到了脱机技术

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F898a28c1b2144365b58e4fff22093fad.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122418880)  
**引入脱机技术后**

- 缓解了CPU与慢速I/O设备的速度矛盾
- 即使CPU在忙碌，也可以提前将数据输入到磁带
- 即使慢速的输出设备在忙碌，也可以提前将数据输出到磁带

**所谓脱机指的就是脱离主机的控制进行输入/输出操作**

# 二：假脱机技术\(SPOOLing技术\)

**SPOOLing的意思是外部设备同时联机操作，是一种用软件模拟的脱机技术，其组成如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe8c41333b727473d88ac150ec023246f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122418880)

## （1）输入井和输出井

输入井和输出井是指**在磁盘上开辟出的两个存储区域**

- **输入井模拟脱机输入时的磁盘，用于收容I/O设备输入的数据**
- **输出井模拟脱机输出时的磁盘，用于收容用户程序的输出数据**

## （2）输入缓冲区和输出缓冲区

输入缓冲区和输出缓冲区是**在内存中开辟的两个缓冲区**

- **输入缓冲区用于暂存由输入设备送来的数据，之后再传送到输入井**
- **输出缓冲区用于暂存从输出井送来的数据，之后再传送到输出设备**

## （3）输入进程和输出进程

- **输入进程模拟脱机输入时的外围控制机，将用户要求的数据从输入机通过输入缓冲区再送到输入井，当CPU需要输入数据时，直接将数据从输入井读入内存**
- **输出进程模拟脱机输出时的外围控制机，将用户要求输出的数据先从内存送到输出井，待输出设备空闲时，再将输出井中的数据经过输出缓冲区送到输出设备**

# 三：SPOOLing技术实例——共享打印机

打印机是一种典型的**独占式设备**，在一段时间内只能供一个用户使用。而通过SPOOLing技术就可以将打印机改造为“**共享设备**”

---

当多个用户进程提出打印请求时，系统**先会答应他们的请求，但并不是真正把打印机分配给他们，而是由假脱机管理进程为每个进程做两件事情**

- 在磁盘输出井中**为每个进程申请一个空闲缓冲区**，然后将打印的数据送入其中
- 为用户进程申请一张空白的**打印申请表**，并将用户的一些打印规格信息填入表中，再将该表挂到**假脱机文件队列上**

当打印机空闲时，输出进程会从文件队列的队头取出一张打印请求表，并根据表中要求将需要打印的数据从输出井送至输出缓冲区，再输出到打印机进行打印

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fca1ade0e88834c2ea8f2b234efa4b311.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122418880)