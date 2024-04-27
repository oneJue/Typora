 

- [王道考研复习指导下载（密码7281）](https://url18.ctfile.com/f/22722418-803125355-edf378)

其他科目导航

- [【专栏必读】王道考研408计算机组成原理万字笔记（从学生角度辅助大家理解）：各章节导航及思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162)

- [【专栏必读】王道考研408数据结构万字笔记（有了它不需要你再做笔记了）：各章节内容概述导航和思维导图](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)

- [【专栏必读】王道考研408计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)

- [C++学习](https://blog.csdn.net/qq_39183034/category_10813323.html?spm=1001.2014.3001.5482)

- [【免费分享】软件工程核心知识点](https://blog.csdn.net/qq_39183034/category_11563929.html?spm=1001.2014.3001.5482)

- [【免费分享】数据库系统概论（王珊 第五版）知识点](https://blog.csdn.net/qq_39183034/category_11568281.html?spm=1001.2014.3001.5482)

---

**视频介绍**

408（计组+操作系统+数据结构+计网）王道计算机考研专栏万字笔记-祝您考研上岸

**首先感谢王道大大（手动比心）**，很用心在做了，笔记会按照如下方式、特点记录，大家可以看看，介绍在后面

- [\(王道408考研操作系统\)第二章进程管理-第一节3：进程控制（配合Linux讲解）](https://blog.csdn.net/qq_39183034/article/details/120950373)
- [\(王道408考研操作系统\)第二章进程管理-第二节3：调度算法详解2\(RR、HPF和MFQ\)](https://blog.csdn.net/qq_39183034/article/details/121131983)

### 文章目录

- [一：必读](#_42)
- [二：关于本专栏及学习建议](#_91)
- [三：王道考研408操作系统导航](#408_115)
- - [第一章：计算机系统概述、](#_118)
  - - [第一节：操作系统的基本概念](#_121)
    - [第二节：操作系统的发展历程](#_125)
    - [第三节： 操作系统运行环境](#__129)
    - [第四节：操作系统结构](#_142)
    - [第五、六节：操作系统引导和虚拟机](#_150)
  - [第二章：进程管理](#_166)
  - - [第一节：进程与线程](#_167)
    - [第二节：进程调度、分类及其调度算法等](#_182)
    - [第三节：进程同步和互斥、信号量及经典进程同步问题等](#_195)
    - [第四节：死锁和避免死锁等](#_220)
  - [第三章：内存管理](#_231)
  - - [第一节：内存基本知识、分配管理方式等](#_232)
    - [第二节：虚拟内存、请求分页及页面置换算法等](#_256)
  - [第四章：文件管理](#_269)
  - - [第一节：文件基本概念、目录、物理结构、逻辑结构、共享和安全等](#_271)
    - [第二节：磁盘结构、磁盘调度算法等](#_290)
  - [第五章：输入/输出（I/O）管理](#IO_301)
- [四：Linux系统编程导航](#Linux_319)
- - [（1）第一部分：博客文章导航](#1_329)
  - [（2）第二部分：涉及的系统调用命令总结](#2_490)
  - - [A：进程部分](#A_492)
    - [B：基础IO部分](#BIO_494)
    - [C：进程间通信部分](#C_497)
    - [D：进程信号部分](#D_499)
    - [E：多线程部分](#E_502)
  - [（3）第三部分：重要代码](#3_508)
- [五：Linux命令行大全导航](#Linux_1152)

---

# 一：必读

①：**《计算机科学专业基础综合》（代码408）** 想必每位计算机考研人都有所了解，虽然可能所考院校是自命题，但总会涉及408中的一种或多种。408会涉及如下4门课，它们各有特点

- **《计算机组成原理》（占30\%）**：涉及硬件等底层知识、部分知识晦涩难懂
- **《操作系统》（占23.3\%）**：计算机中的“哲学”，内容特别抽象，感觉“学了等于没学”
- **《数据结构》（占30\%）**：最重要的一门课，逻辑性强，较为抽象，常和算法有关
- **《计算机网络》（占16.7\%）**：关联知识较多（例如通信），所以知识点“又臭又长”

所以408难度确实不小，在180分钟内做完一套试卷犹如进行了一场战斗。而且最为关键的是**考计算机一定会考数学**，数学的复习几乎会占据你考研复习时间的一半（甚至更多），因为“得数学者得天下”

②：对于408的复习，市面上的授课机构主要是王道和天勤，它们两家真的都非常非常好，我都细心看过

- **王道**：知识点涵盖全面、讲解仔细
- **天勤**：讲解角度独特，动画制作精美，关键问题容易理解

而王道也把人家的课程全部上传至了B站，所以专栏笔记主体会基于王道进行，**所以这里真的特别特别感谢王道**

- [《王道-计算机组成原理》](https://www.bilibili.com/video/BV1BE411D7ii?spm_id_from=333.999.0.0)
- [《王道-操作系统》](https://www.bilibili.com/video/BV1YE411D7nH?spm_id_from=333.999.0.0)
- [《王道-数据结构》](https://www.bilibili.com/video/BV1b7411N798?spm_id_from=333.999.0.0)
- [《王道-计算机网络》](https://www.bilibili.com/video/BV19E411D78Q?spm_id_from=333.999.0.0)

③：只有考研人才能懂考研人，所以我深知复习408的痛苦。**面对海量的知识点你会感觉力不从心**，尤其在前期，总是学了这一章忘了上一章，而且很多时候不同科目的知识经常搅到一起。因此，我花费了很长时间写了这些专栏笔记以帮助大家考研复习，有以下特点

- 专栏笔记会按照**视频课和课本的逻辑**进行记录，会把老师课上所讲内容和课本进行结合，同时辅助一些自己在工作、学习中的想法

- 说个实话，视频课中的内容是有点“**乱**”的，因为老师在讲课时是需要按照他的思路来进行的，所以我的目的就是要让其**系统性**（也即你会知道每一节究竟在干什么），便于同学查阅；同时有些知识点晦涩难懂，我会**加入自己的理解便于大家学习**

- 所有笔记**纯手打（课本+老师说的话+自己的理解）**，并不是视频课截图（当然有些图片肯定还是会采用截图）

- 所有笔记会严格控制格式（主要就是公式和配色），力争做到**清晰、简洁、整齐**

- 部分科目会配有题型讲解，大家可以在我主页处找到，这一部分还在更新

④：专栏会一直更新，主要是纠错和补充知识点

⑤：希望大家能够意识到**学习这四门课并不是简简单单为了考研，只要你真心想要走计算机这条道路，它就是你的基本功**

- 就拿离你们最近的校招来说，其实70\%的内容都是这些

⑥：“道阻且长、行则将至”，大家加油吧！

# 二：关于本专栏及学习建议

- [王道操作系统思维导图链接](https://pan.baidu.com/s/1idIW0bH5UnZTwiC2_B895g)：sxky

- 标题是按照视频课走的，可能有些部分存在差异，因为部分视频课内容太少，不值得单独做一篇文章

- **所用教材为 《2023年操作系统考研复习指导》**

- 笔记主要以**王道视频为主**，少量会补充 **《操作系统精髓与设计原理\(第八版\)》** 中的内容。建议大家有时间可以读一读，十分经典  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff3bfb5ba34b947b5bd88c3a8c9d887de.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

- 笔记是学好的必要条件，但不是充分条件

- 记笔记的目的不是单纯的为了“记”，是为了以后复习时不需要太大的时间成本

- 学习一门课就像开发一个程序一样，**先搭框架，后解决细节问题**。前期需要迅速建立一门学科的整体框架与逻辑，不会的可以直接跳，学完之后需要**反复梳理**

# 三：王道考研408操作系统导航

## 第一章：计算机系统概述、

### 第一节：操作系统的基本概念

[\(王道408考研操作系统\)第一章计算机系统概述-第一节1、2：操作系统概念、概念和特征](https://blog.csdn.net/qq_39183034/article/details/120759021)

### 第二节：操作系统的发展历程

[\(王道408考研操作系统\)第一章计算机系统概述-第二节：操作系统的发展历程](https://blog.csdn.net/qq_39183034/article/details/120810537)

### 第三节： 操作系统运行环境

[\(王道408考研操作系统\)第一章计算机系统概述-第三节1：操作系统的运行机制与体系结构](https://blog.csdn.net/qq_39183034/article/details/120825466)

[\(王道408考研操作系统\)第一章计算机系统概述-第三节2：中断和异常](https://blog.csdn.net/qq_39183034/article/details/120845171)

[\(王道408考研操作系统\)第一章计算机系统概述-第三节3：系统调用](https://blog.csdn.net/qq_39183034/article/details/120904490)

### 第四节：操作系统结构

[\(王道408考研操作系统\)第一章计算机系统概述-第四节：操作系统体系结构](https://blog.csdn.net/qq_39183034/article/details/126288600)

### 第五、六节：操作系统引导和虚拟机

[\(王道408考研操作系统\)第一章计算机系统概述-第五节：操作系统引导](https://blog.csdn.net/qq_39183034/article/details/126317317)

## 第二章：进程管理

### 第一节：进程与线程

[\(\(王道408考研操作系统\)第二章进程管理-第一节1：进程的概念、组成和特征](https://zhangxing-tech.blog.csdn.net/article/details/120920040)

[\(王道408考研操作系统\)第二章进程管理-第一节2：进程状态与转换](https://blog.csdn.net/qq_39183034/article/details/120934926)

[\(王道408考研操作系统\)第二章进程管理-第一节3：进程控制（配合Linux讲解）](https://blog.csdn.net/qq_39183034/article/details/120950373)

[\(王道408考研操作系统\)第二章进程管理-第一节4：进程通信（配合Linux）](https://blog.csdn.net/qq_39183034/article/details/120982541)

[\(王道408考研操作系统\)第二章进程管理-第一节5：线程的概念](https://blog.csdn.net/qq_39183034/article/details/120983181)

[\(王道408考研操作系统\)第二章进程管理-第一节6、7：线程的实现方式、多线程模型、线程的状态与转换、线程的组织与控制](https://blog.csdn.net/qq_39183034/article/details/126380261)

### 第二节：进程调度、分类及其调度算法等

[\(王道408考研操作系统\)第二章进程管理-第二节1：调度的基本概念和层次](https://blog.csdn.net/qq_39183034/article/details/121043786)

[\(王道408考研操作系统\)第二章进程管理-第二节2、3：进程调度的时机、切换与过程、方式、调度器和闲逛进程](https://blog.csdn.net/qq_39183034/article/details/126398736)

[\(王道408考研操作系统\)第二章进程管理-第二节4：调度算法评价指标](https://blog.csdn.net/qq_39183034/article/details/121058201)

[\(王道408考研操作系统\)第二章进程管理-第二节5：调度算法详解1（FCFS、SJF和HRRN）](https://blog.csdn.net/qq_39183034/article/details/121071441)

[\(王道408考研操作系统\)第二章进程管理-第二节6、7：调度算法详解2\(RR、HPF和MFQ\)](https://blog.csdn.net/qq_39183034/article/details/121131983)

### 第三节：进程同步和互斥、信号量及经典进程同步问题等

[\(王道408考研操作系统\)第二章进程管理-第三节1：进程同步与互斥的基本概念](https://blog.csdn.net/qq_39183034/article/details/121172119)

[\(王道408考研操作系统\)第二章进程管理-第三节2：实现进程互斥的软件方法](https://blog.csdn.net/qq_39183034/article/details/121172492)

[\(王道408考研操作系统\)第二章进程管理-第三节3：实现进程互斥的硬件方法](https://blog.csdn.net/qq_39183034/article/details/121238968)

[\(王道408考研操作系统\)第二章进程管理-第三节4：信号量机制（整型、记录型信号量和P、V操作）](https://blog.csdn.net/qq_39183034/article/details/121258695)

[\(王道408考研操作系统\)第二章进程管理-第三节5：用信号量实现进程互斥、同步和前驱关系](https://blog.csdn.net/qq_39183034/article/details/121278167)

[\(王道408考研操作系统\)第二章进程管理-第三节6：经典同步问题之生产者与消费者问题](https://blog.csdn.net/qq_39183034/article/details/121278992)

[\(王道408考研操作系统\)第二章进程管理-第三节7：经典同步问题之多生产者与多消费者问题](https://blog.csdn.net/qq_39183034/article/details/121312323)

[\(王道408考研操作系统\)第二章进程管理-第三节8：经典同步问题之吸烟者问题](https://blog.csdn.net/qq_39183034/article/details/121346179)

[\(王道408考研操作系统\)第二章进程管理-第三节9：经典同步问题之读者写者问题](https://blog.csdn.net/qq_39183034/article/details/121367847)

[\(王道408考研操作系统\)第二章进程管理-第三节10：经典同步问题之哲学家进餐问题](https://blog.csdn.net/qq_39183034/article/details/121390617)

[\(王道408考研操作系统\)第二章进程管理-第三节11：管程\(Monitor\)及条件变量](https://blog.csdn.net/qq_39183034/article/details/121432664)

### 第四节：死锁和避免死锁等

[\(王道408考研操作系统\)第二章进程管理-第四节1：死锁相关概念](https://blog.csdn.net/qq_39183034/article/details/121447663)

[\(王道408考研操作系统\)第二章进程管理-第四节2：死锁处理策略之预防死锁](https://blog.csdn.net/qq_39183034/article/details/121462546)

[\(王道408考研操作系统\)第二章进程管理-第四节2：死锁处理策略之避免死锁（银行家算法）](https://blog.csdn.net/qq_39183034/article/details/121483447)

[\(王道408考研操作系统\)第二章进程管理-第四节3：死锁处理策略之检测和解除](https://blog.csdn.net/qq_39183034/article/details/121526731)

## 第三章：内存管理

### 第一节：内存基本知识、分配管理方式等

[\(王道408考研操作系统\)第三章内存管理-第一节1：内存基础知识、程序编译运行原理](https://blog.csdn.net/qq_39183034/article/details/121550004)

[\(王道408考研操作系统\)第三章内存管理-第一节2：内存管理的基本概念](https://blog.csdn.net/qq_39183034/article/details/121585496)

[\(王道408考研操作系统\)第三章内存管理-第一节3：覆盖与交换](https://blog.csdn.net/qq_39183034/article/details/121585799)

[\(王道408考研操作系统\)第三章内存管理-第一节4：连续分配管理方式\(单一连续、固定分区和动态分区分配\)](https://blog.csdn.net/qq_39183034/article/details/121600501)

[\(王道408考研操作系统\)第三章内存管理-第一节5：动态分区分配算法（首次适应、和邻近适应）](https://blog.csdn.net/qq_39183034/article/details/121645735)

[\(王道408考研操作系统\)第三章内存管理-第一节6-1：非连续分配管理方式之基本分页存储管理](https://blog.csdn.net/qq_39183034/article/details/121668055)

[\(王道408考研操作系统\)第三章内存管理-第一节6-2：非连续分配管理方式之基本分页存储管理之基本地址变换机构](https://blog.csdn.net/qq_39183034/article/details/121733607)

[\(王道408考研操作系统\)第三章内存管理-第一节6-3：非连续分配管理方式之基本分页存储管理之具有快表的地址变换机构](https://blog.csdn.net/qq_39183034/article/details/121753002)

[\(王道408考研操作系统\)第三章内存管理-第一节6-4：非连续分配管理方式之基本分页存储管理之两级页表](https://blog.csdn.net/qq_39183034/article/details/121753797)

[\(王道408考研操作系统\)第三章内存管理-第一节7：非连续分配管理方式之基本分段管理方式](https://blog.csdn.net/qq_39183034/article/details/121798452)

[\(王道408考研操作系统\)第三章内存管理-第一节8：非连续分配管理方式之段页式管理方式](https://blog.csdn.net/qq_39183034/article/details/121842272)

### 第二节：虚拟内存、请求分页及页面置换算法等

[\(王道408考研操作系统\)第三章内存管理-第二节1：虚拟内存管理基本概念](https://blog.csdn.net/qq_39183034/article/details/121863515)

[\(王道408考研操作系统\)第三章内存管理-第二节2：请求分页管理方式](https://blog.csdn.net/qq_39183034/article/details/121877642)

[\(王道408考研操作系统\)第三章内存管理-第二节3：页面置换算法1](https://blog.csdn.net/qq_39183034/article/details/121891511)

[\(王道408考研操作系统\)第三章内存管理-第二节3：页面置换算法2](https://blog.csdn.net/qq_39183034/article/details/121933790)

[\(王道408考研操作系统\)第三章内存管理-第二节4：页面分配策略](https://blog.csdn.net/qq_39183034/article/details/121958622)

## 第四章：文件管理

### 第一节：文件基本概念、目录、物理结构、逻辑结构、共享和安全等

[\(王道408考研操作系统\)第四章文件管理-第一节1：文件管理初识](https://blog.csdn.net/qq_39183034/article/details/122216672)

[\(王道408考研操作系统\)第四章文件管理-第一节2：文件的逻辑结构](https://blog.csdn.net/qq_39183034/article/details/122242794)

[\(王道408考研操作系统\)第四章文件管理-第一节3：文件目录](https://blog.csdn.net/qq_39183034/article/details/122251368)

[\(王道408考研操作系统\)第四章文件管理-第一节4：文件物理结构（文件分配方式）](https://blog.csdn.net/qq_39183034/article/details/122255612)

[\(王道408考研操作系统\)第四章文件管理-第一节5：文件存储空间管理](https://blog.csdn.net/qq_39183034/article/details/122279127)

[\(王道408考研操作系统\)第四章文件管理-第一节6：文件基本操作](https://blog.csdn.net/qq_39183034/article/details/122291097)

[\(王道408考研操作系统\)第四章文件管理-第一节7：文件共享](https://blog.csdn.net/qq_39183034/article/details/122291689)

[\(王道408考研操作系统\)第四章文件管理-第一节8：文件保护](https://blog.csdn.net/qq_39183034/article/details/122305390)

[\(王道408考研操作系统\)第四章文件管理-第一节9：文件系统的层次结构](https://blog.csdn.net/qq_39183034/article/details/122308623)

### 第二节：磁盘结构、磁盘调度算法等

[\(王道408考研操作系统\)第四章文件管理-第二节1：磁盘的结构](https://blog.csdn.net/qq_39183034/article/details/122309521)

[\(王道408考研操作系统\)第四章文件管理-第二节2：磁盘调度算法](https://blog.csdn.net/qq_39183034/article/details/122350433)

[\(王道408考研操作系统\)第四章文件管理-第二节3：减少延迟时间的方法](https://blog.csdn.net/qq_39183034/article/details/122370299)

[\(王道408考研操作系统\)第四章文件管理-第二节4：磁盘的管理](https://blog.csdn.net/qq_39183034/article/details/122371090)

## 第五章：输入/输出（I/O）管理

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节1：I/O设备的概念和分类](https://blog.csdn.net/qq_39183034/article/details/122384025)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节2：I/O控制器](https://blog.csdn.net/qq_39183034/article/details/122384448)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节3：I/O控制方式](https://blog.csdn.net/qq_39183034/article/details/122385649)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节4：I/O软件层次结构](https://blog.csdn.net/qq_39183034/article/details/122404649)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节5：假脱机\(SPOOLing\)技术](https://blog.csdn.net/qq_39183034/article/details/122418880)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节6：设备的分配和回收](https://blog.csdn.net/qq_39183034/article/details/122420101)

[\(王道408考研操作系统\)第五章输入/输出（I/O）管理-第一节7：缓冲区管理](https://blog.csdn.net/qq_39183034/article/details/122441445)

# 四：Linux系统编程导航

**用“哲学”来比喻操作系统我觉得再合适不过了**。**操作系统就是这样，你感觉你懂了其实你没有懂，你没有懂但是你又明白一点。所以想要落实唯一的一个方法就是实践、实践、实践。我们所学的操作系统并没有针对某个特定的操作系统，这里我选择的是Linux进行演示，因为它真的很直观**

- **所选用Linux系统为Centos 8.0**

## （1）第一部分：博客文章导航

**[Linux系统编程1：Linux中使用率最高的一些命令](https://blog.csdn.net/qq_39183034/article/details/112894245)**

**[Linux系统编程2：详解Linux中的权限问题](https://blog.csdn.net/qq_39183034/article/details/116143912)**

**[Linux系统编程3：基础篇之详解Linux软件包管理器yum](https://blog.csdn.net/qq_39183034/article/details/116144487)**

**[Linux系统编程4：入门篇之最强编辑器vim的使用攻略](https://blog.csdn.net/qq_39183034/article/details/116144553)**

**[Linux系统编程5：Linux系统编程5：入门篇之在Linux下观察C/C++程序编译过程 \&\& gcc/g++使用详解](https://blog.csdn.net/qq_39183034/article/details/116148074)**

**[Linux系统编程7：入门篇之Linux项目自动化构建工具-Make/Makefile的超强使用指南](https://blog.csdn.net/qq_39183034/article/details/116149436)**

**[Linux系统编程9：进程入门之操作系统为什么这么重要以及它是如何实现管理的](https://blog.csdn.net/qq_39183034/article/details/116150559)**

**[Linux系统编程10：进程入门之系统编程中最重要的概念之进程\&\&进程的相关操作\&\&使用fork创建进程](https://blog.csdn.net/qq_39183034/article/details/116188883)**

**[Linux系统编程11：进程入门之详细阐述进程的一些状态\&\&区分僵尸状态和孤儿状态\&\&动图演示](https://blog.csdn.net/qq_39183034/article/details/116189035)**

**[Linux系统编程12：进程入门之进程的优先级及PR和NI\&\&如何修改进程优先级](https://blog.csdn.net/qq_39183034/article/details/116189083)**

**[Linux系统编程13：进程入门之Linux中的环境变量的概念及其相关命令（export；env等）\&\&main函数的参数](https://blog.csdn.net/qq_39183034/article/details/116189151)**

**[Linux系统编程14：进程入门之Linux进程中非常重要的概念之进程地址空间-原来我们看到的地址全部是虚拟的](https://blog.csdn.net/qq_39183034/article/details/114284873)**

**[Linux系统编程15：进程控制之如何创建进程和写时拷贝技术](https://blog.csdn.net/qq_39183034/article/details/116189328)**

**[Linux系统编程16：进程控制之进程终止以及终止进程的三种情况](https://blog.csdn.net/qq_39183034/article/details/116189390)**

**[Linux系统编程17：进程控制之进程等待\&\&为什么进程需要被等待\&wait方法和waitpid方法\&\&阻塞和非阻塞等待](https://blog.csdn.net/qq_39183034/article/details/116189479)**

**[Linux系统编程18：超详解进程程序替换\&exec函数的一些用法](https://blog.csdn.net/qq_39183034/article/details/116189565)**

**[Linux系统编程19：基础IO之了解Linux中的标准输入和输出以及相关的系统调用接口（如write，read等）](https://blog.csdn.net/qq_39183034/article/details/116232764)**

**[Linux系统编程20：基础IO之从内核代码深刻理解Linux是如何管理文件及文件描述符的本质是什么](https://blog.csdn.net/qq_39183034/article/details/116232824)**

**[Linux系统编程21：基础IO之全缓冲和行缓冲的区别及深刻理解缓冲区及其作用](https://blog.csdn.net/qq_39183034/article/details/116232861)**

**[Linux系统编程22：基础IO之掌握重定向的本质和使用dup2完成重定向](https://blog.csdn.net/qq_39183034/article/details/116232918)**

**[Linux系统编程23：基础IO之了解硬盘物理和逻辑结构及明白inode的本质和掌握软硬链接及其区别](https://blog.csdn.net/qq_39183034/article/details/116232991)**

**[Linux系统编程24：基础IO之在Linux下深刻理解C语言中的动静态库以及头文件和库的关系](https://blog.csdn.net/qq_39183034/article/details/116233049)**

**[Linux系统编程25：基础IO之亲自实现一个动静态库](https://zhangxing-tech.blog.csdn.net/article/details/115061389)**

**[Linux系统编程26：进程间通信之进程间通信的基本概念](https://zhangxing-tech.blog.csdn.net/article/details/116233564)**

**[Linux系统编程27：进程间通信之管道的基本概念和匿名管道与命名管道及管道特性](https://zhangxing-tech.blog.csdn.net/article/details/116269239)**

**[Linux系统编程28：进程间通信之共享内存和相关通信接口（ftok,shmget,shmctl,shmat,shmdt）](https://blog.csdn.net/qq_39183034/article/details/115354952)**

**[Linux系统编程29：进程信号之什么是信号及signal函数](https://blog.csdn.net/qq_39183034/article/details/116277561)**

**[Linux系统编程30：进程信号之产生信号的四种方式（Core Dump,kill,raise）](https://blog.csdn.net/qq_39183034/article/details/116277581)**

**[Linux系统编程31：进程信号之什么是信号的阻塞及相关术语（递达，未决，pending位图，handler位图）](https://blog.csdn.net/qq_39183034/article/details/116277610)**

**[Linux系统编程32：进程信号之详解信号集操作函数（sigset\_t ,sigpending,sigprocmask）](https://blog.csdn.net/qq_39183034/article/details/116277684)**

**[Linux系统编程33：进程信号之详解信号的捕捉过程，用户态和内核态及其切换，sigaction和signal](https://blog.csdn.net/qq_39183034/article/details/116277719)**

**[Linux系统编程34：进程信号之可重入函数，volatile关键字的作用和SIGHLD](https://blog.csdn.net/qq_39183034/article/details/115578577)**

**[Linux系统编程35：多线程之如何理解Linux中的线程以及轻量级进程LWP](https://blog.csdn.net/qq_39183034/article/details/116310025)**

**[Linux系统编程36：多线程之线程控制之pthread线程库\(线程创建，终止，等待和分离\)](https://blog.csdn.net/qq_39183034/article/details/116310050)**

**[Linux系统编程37：多线程之什么是临界区和临界资源以及如何使用mutex互斥锁](https://blog.csdn.net/qq_39183034/article/details/116310058)**

**[Linux系统编程38：多线程之什么是线程同步以及条件变量函数](https://blog.csdn.net/qq_39183034/article/details/116310080)**

**[Linux系统编程39：多线程之基于阻塞队列生产者与消费者模型](https://zhangxing-tech.blog.csdn.net/article/details/116310088)**

**[Linux系统编程40：多线程之基于环形队列的生产者与消费者模型](https://zhangxing-tech.blog.csdn.net/article/details/116310101)**

**[Linux系统编程41：多线程之线程池的概念及实现](https://blog.csdn.net/qq_39183034/article/details/116310114)**

## （2）第二部分：涉及的系统调用命令总结

### A：进程部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210501112437770.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

### B：基础IO部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210502111503826.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

### C：进程间通信部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210502173134235.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

### D：进程信号部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210504105616778.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

### E：多线程部分

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210504160347428.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121004242)

## （3）第三部分：重要代码

**1：理解fork的作用**

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
	prinf("还没有执行fork函数的本进程为：%d\n",getpid());
	pid_t=fork();//其返回值是pid类型的
	sleep(1);
	
	if(ret>0)//父进程返回的是子进程ID
	{
            
            
		while(1)
		{
            
            
			printf("----------------------------------------------------------------\n");
			printf("我是父进程，我的id是：%d,我的孩子id是%d\n",getpid(),ret);
			sleep(1);
		}
	}
	else if(ret==0)//子进程fork返回值是0
	{
            
            
		while(1)
		{
            
            
			printf("我是子进程，我的id是%d，我的父亲id是%d\n",getpid(),getppid());
			sleep(1);
		}
	}
	else
		printf("进程创建失败\n");
		
	sleep(1);
	return 0;
}

```

**2：理解僵尸状态**

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
   // printf("还没执行fork函数时的本进程为：%d\n",getpid());
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
      while(1)
      {
            
            
        printf("----------------------------------------------------\n");
        printf("父进程一直在运行\n");
        sleep(1);
      }
    }
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=0;
      while(count<=10)
      {
            
            
        printf("子进程已经运行了%d秒\n",count+=1);
        sleep(1);
      }
      exit(0);//让子进程运行10s
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

**3：理解进程等待及waitpid第二个参数**

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st=0;
		pid_t rec=waitpid(ret,&st,0);//阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			if(WIFEXITED(st))//如果为真，正常退出
			{
            
            
				printf("正常退出且退出码为%d\n",WEXITSTATUS(st));
			}
			else
			{
            
            
				printf("异常退出，信号值为%d\n",st&0x7f);
			}
			
			
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }
      exit(3);
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}



```

**4：理解进程程序替换**

```c
//myprocess.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
  int count=0;
  while(count<5)
  {
            
            
    printf("Hello World\n");
    sleep(1);
    count++;
  }
  printf("%s\n",getenv("myenv"));
 return 0;
}

//test.c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
int main()
{
            
            
  char* env[]={
            
            "myenv=you_can_see_this_env",NULL};
  printf("替换函数前\n");
  execle("./myprocess.exe","myprocess.exe",NULL,env);
  printf("替换函数后\n");

}


```

**5：理解文件描述符**

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  int fd1=open("log1.txt",O_WRONLY);//打开错误
  int fd2=open("log2.txt",O_WRONLY|O_CREAT);//打开成功
  int fd3=open("log3.txt",O_WRONLY|O_CREAT);//打开成功
  int fd4=open("log4.txt",O_WRONLY|O_CREAT);//打开成功
  int fd5=open("log5.txt",O_WRONLY);//打开错误


  printf("%d\n",fd1);
  printf("%d\n",fd2);
  printf("%d\n",fd3);
  printf("%d\n",fd4);
  printf("%d\n",fd5);
}


```

**6：理解匿名管道**

```c
#include  <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main()
{
            
            
  int  pipefd[2]={
            
            0};
   pipe(pipefd);
  pid_t id=fork();
  if(id==0)//child
  {
            
            
    close(pipefd[0]);
    const char* msg="This is the data that the child process wrote";
    while(1)
    {
            
            
        write(pipefd[1],msg,strlen(msg));
        sleep(1);
    }     
  }
  else//father
  {
            
            
    close(pipefd[1]);
    char buffer[64];
    while(1)
    {
            
            
        ssize_t ret=read(pipefd[0],buffer,sizeof(buffer)-1);
        if(ret>0)//判断是否读到
        {
            
            
            buffer[ret]='\0';//加上结束标志，便于输出
            printf("The father process got the information:%s\n",buffer);
        }
    }
  }
  return 0;
}

```

**7：理解命名管道**  
server.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main()
{
            
            
  umask(0);//屏蔽命令行umask干扰
  if(mkfifo("./fifo",0666)==-1)//如果mkfifo返回值是-1，创建失败
  {
            
            
    perror("打开失败");
    return 1;
  }
  int fd=open("fifo",O_RDONLY);//服务端以只读的方式打开管道文件
  if(fd>=0)
  {
            
            
    char buffer[64];
    while(1)
    {
            
            
      printf("客户端正在接受消息\n");
      printf("############################\n");
      ssize_t ret=read(fd,buffer,sizeof(buffer)-1);
      if(ret>0)
      {
            
            
          buffer[ret]='\0';
          printf("服务端接受到客户端消息：%s\n",buffer);
      }
      else if(ret==0)//如果客户端退出，将会读到文件结束，所以服务端也要退出
      {
            
            
          printf("客户端已经下线，服务端下线\n");
          break;
      }
      else 
      {
            
            
        perror("读取失败\n");
        break;
      }
    }
  }
}


```

client.c

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>


int main()
{
            
            
  int fd=open("fifo",O_WRONLY);//直接打开管道文件
  if(fd>=0)
  {
            
             
      char buffer[64];//从键盘读入数据到这个缓冲区
      while(1)
      {
            
             
          printf("客户端-请输入消息：");
          ssize_t ret=read(0,buffer,sizeof(buffer)-1);//从键盘读入数据
          if(ret>0)
          {
            
             
              buffer[ret]='\0';
              write(fd,buffer,ret);//读入ret个数据就向管道中写入ret个数据
          }
      }
  }
}


```

**8:理解共享内存**  
server.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,IPC_CREAT | IPC_EXCL|0666);
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  char* shmaddr=shmat(shmid,NULL,0);//挂接,注意强转
  while(1)//每1s打印一次
  {
            
            
      sleep(1);
      printf("%s\n",shmaddr);
  }

  shmdt(shmaddr);//脱离
  shmctl(shmid,IPC_RMID,NULL);//释放
 
  return 0；
}


```

client.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,0);//服务端已经申请了，写成0直接获取
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  char* shmaddr=shmat(shmid,NULL,0);//挂接，注意强转
  int i=0;
  while(i<26)
  {
            
            
    shmaddr[i]=97+i;每隔5s依次输入a,b,c...........................
    i++;
    sleep(5);

  }
  shmdt(shmaddr);//脱离
  return 0;
}


```

**9：理解signal函数**

```cpp
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

void handler(int sig)
{
            
            
  printf("catch a sin : %d\n",sig);

}

int main()
{
            
            
  signal(2,handler);//一旦捕捉到2号信号，将会执行handler函数内的操作
  while(1)
  {
            
            
    printf("I Am runnng now...\n");
    sleep(1);
  }
  return 0;

}


```

**10：理解信号集操作函数**

```cpp
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

void handler(int sig)
{
            
            
  printf("获得信号:%d\n",sig);

}

void print_pending(sigset_t* pending)
{
            
            
  int i=1;
  for(i=1;i<=31;i++)
  {
            
            
    if(sigismember(pending,i))
    {
            
            
      printf("1");//只要i信号存在，就打印1
    }
    else 
    {
            
            
      printf("0");//不存在这个信号就打印0
    }
  }
  printf("\n");
}

int main()
{
            
            
  signal(2,handler);//捕捉

  sigset_t pending;//定义信号集变量

  sigset_t block,oblock;//定义阻塞信号集变量
  sigemptyset(&block);
  sigemptyset(&oblock);//初始化阻塞信号集

  sigaddset(&block,2);//将2号信号添加的信号集

  sigprocmask(SIG_SETMASK,&block,&oblock);//设置屏蔽关键字

  int cout=0; 
  while(1)
  {
            
            
    sigemptyset(&pending);//初始化信号集
    sigpending(&pending);//读取未决信号集，传入pending
    print_pending(&pending);//定义一个函数，打印未决信号集
    sleep(1);
    cout++;
    if(cout==10)//10s后解除阻塞
    {
            
            
      printf("解除阻塞\n");
      sigprocmask(SIG_SETMASK,&oblock,NULL);
    }

  }
}


```

**11：理解线程的创建，等待等**

```cpp
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(5);
    int a=1/0; //浮点异常
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    void* ret;//获取退出码
    pthread_join(tid,&ret);
    printf("主线程阻塞等待新线程，退出码码为%d\n",(int)ret);
    break;
  }


}

```

**12：理解互斥锁**

```cpp
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


int tickets=1000;

pthread_mutex_t lock;//申请一把锁

void scarmble_tickets(void* arg)
{
            
            
  long int ID=(long int)arg;//线程ID
  while(1)//多个线程循环抢票
  {
            
            
    pthread_mutex_lock(&lock);//那个线程先到，谁就先锁定资源
    if(tickets>0)
    {
            
            
      usleep(1000);
      printf("线程%ld号抢到了一张票，现在还有%d张票\n",ID,tickets);
      tickets--;
      pthread_mutex_unlock(&lock);//抢到票就解放资源
    }
    else 
    {
            
            
      pthread_mutex_unlock(&lock);//如果没有抢到也要释放资源，否则线程直接退出，其他线程无法加锁
      break;
    }
  }

}


int main()
{
            
            
  int i=0;
  pthread_t tid[4];//4个线程ID
  pthread_mutex_init(&lock,NULL);//初始化锁
  for(i=0;i<4;i++)
  {
            
            
    pthread_create(tid+1,NULL,scarmble_tickets,(void*)i);//创建4个线程
  }

  for(i=0;i<4;i++)
  {
            
            
    pthread_join(tid+1,NULL);//线程等待
  }
  pthread_mutex_destroy(&lock);//销毁锁资源
  return 0;
}


```

**13：理解条件变量**

```cpp
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;//锁
pthread_cond_t cond;//条件

void* thread_a(void* arg)//其他线程被唤醒
{
            
            
  const char* name=(char*)arg;//线程名字
  while(1)
  {
            
            
    pthread_cond_wait(&cond,&lock);//一直在等待条件成熟
    printf("%s被唤醒了\n=================================================\n",name);

  }

}

void* thread_1(void* arg)//让线程1唤醒其他线程
{
            
            
  const char* name=(char*)arg;
  while(1)
  {
            
            
    sleep(rand()%5+1);//随机1-5秒唤醒
    pthread_cond_signal(&cond);
    printf("%s现在正在发送信号\n",name);
  }

}



int main()
{
            
            
  pthread_mutex_init(&lock,NULL);//初始化锁
  pthread_cond_init(&cond,NULL);//初始化条件

  pthread_t t1,t2,t3,t4,t5;//创建两个线程
  pthread_create(&t1,NULL,thread_1,(void*)"线程1"); 

  pthread_create(&t2,NULL,thread_a,(void*)"线程2"); 
  pthread_create(&t3,NULL,thread_a,(void*)"线程3"); 
  pthread_create(&t4,NULL,thread_a,(void*)"线程4"); 
  pthread_create(&t5,NULL,thread_a,(void*)"线程5"); 

  pthread_join(t1,NULL);
  pthread_join(t2,NULL);
  pthread_join(t3,NULL);
  pthread_join(t4,NULL);
  pthread_join(t5,NULL);

  pthread_mutex_destroy(&lock);
  pthread_cond_destroy(&cond);//销毁条件

}



```

# 五：Linux命令行大全导航

多数情况下，我们使用Linux系统时都会用**命令行**而不使用图形界面，因为Linux多作远程服务器开发，所以图形界面不方便。因此这里为了让大家能够快速掌握Linux中的一些常用命令，特向大家推荐 **《Linux命令行大全》** 这本书，并且已做笔记导航如下

[第1章：shell是什么](https://blog.csdn.net/qq_39183034/article/details/114365756?spm=1001.2014.3001.5502)

[第2章：导航](https://blog.csdn.net/qq_39183034/article/details/114366502?spm=1001.2014.3001.5502)

[第3章：Linux系统](https://blog.csdn.net/qq_39183034/article/details/114377798)

[第4章：操作文件与目录](https://blog.csdn.net/qq_39183034/article/details/114412404)

[第5章：命令的使用](https://blog.csdn.net/qq_39183034/article/details/114461945)

[第6章：重定向](https://blog.csdn.net/qq_39183034/article/details/114476945)

[第7章：透过shell看世界](https://blog.csdn.net/qq_39183034/article/details/114555368)

[第8章：高级键盘技巧](https://blog.csdn.net/qq_39183034/article/details/114632955)

[第9章：权限](https://blog.csdn.net/qq_39183034/article/details/114666535)

[第10章：进程](https://blog.csdn.net/qq_39183034/article/details/114707681)

[第11章：环境](https://blog.csdn.net/qq_39183034/article/details/114753327)

[第12章：Vim编辑器](https://blog.csdn.net/qq_39183034/article/details/114765124)

[第13章：定制shell提示符](https://blog.csdn.net/qq_39183034/article/details/114776612)

[第14章：软件包管理](https://blog.csdn.net/qq_39183034/article/details/114871775)

[第15章：存储介质](https://blog.csdn.net/qq_39183034/article/details/114877493)

[第16章：网络](https://blog.csdn.net/qq_39183034/article/details/114878210)

[第17章：文件搜索](https://blog.csdn.net/qq_39183034/article/details/114930001)

[第18章：归档和备份](https://blog.csdn.net/qq_39183034/article/details/114973757)

[第19章：正则表达式](https://blog.csdn.net/qq_39183034/article/details/115013650)

[第19章：文本处理](https://blog.csdn.net/qq_39183034/article/details/115189069)

[第20章：格式化输出](https://blog.csdn.net/qq_39183034/article/details/115276897)

[第21章：shell脚本](https://blog.csdn.net/qq_39183034/article/details/115383125)