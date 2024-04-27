 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [思维导图](#_8)
- [一：CPU的功能](#CPU_12)
- - [（1）CPU的具体功能](#1CPU_13)
  - [（2）每个部件的功能](#2_23)
- [二：运算器基本结构](#_32)
- - [（1）运算器概述](#1_34)
  - [（2）两种数据通路设计方式](#2_53)
  - - [A：专用数据通路](#A_62)
    - [B：CPU内部单总线（主要使用）](#BCPU_83)
- [三：控制器基本结构](#_106)
- - [（1）控制器概述](#1_108)
  - [（2）控制器控制过程概述](#2_122)
- [四：CPU的本质——寄存器的集合体](#CPU_143)

# 思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb559a1117b444577aa44dcd42cd86278.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

# 一：CPU的功能

## （1）CPU的具体功能

**CPU具体功能包括**

- **指令控制**：完成**取指令、分析指令和执行指令**的操作，也即程序的**顺序控制**
- **操作控制**：**一条指令的功能是通过若干操作信号组合来实现的**。CPU管理并产生由内存取出的每条指令的操作信号，**把各种操作信号送往相应部件，从而控制这些部件按指令的要求进行动作**
- **时间控制**：对各种操作加以时间上的控制。时间控制要为每条指令按**时间顺序**提供应有的控制信号
- **数据加工**：对数据进行**算数和逻辑运算**
- **中断处理**：对计算机运行过程中出现的**异常情况和特殊请求**进行处理

## （2）每个部件的功能

**CPU由运算器和控制器构成，其中运算器主要作用就是对数据进行加工**；**控制器主要作用就是协调和控制计算机各部件执行程序的指令序列，具体来说**

- **取指令**：自动形成**指令地址**，发出取指令的命令
- **分析指令**：操作码**译码**（分析本条指令要完成什么操作）；产生操作数的**有效地址**
- **执行指令**：由“操作命令”和“操作数地址”，形成**操作信号控制序列**，控制运算器、存储器以及I/O设备完成相应的操作
- **中断处理**：管理**总线**及输入输出；处理**异常情况**（比如掉电、浮点异常等）和**特殊情况**的请求（打印机请求打印一行字符等）

# 二：运算器基本结构

## （1）运算器概述

运算器核心是**ALU算数逻辑单元**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F21f8769d67874b418bdf76a3220cd695.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)  
ALU需要**两个操作数**，经过处理后，就会输出运算结果

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc21f58e07197454b8aa847334103efb0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

**运算器是计算机中加工数据的中心**，除了ALU外，它还有很多寄存器，这里先给出它们的大致功能

- **暂存寄存器**：用于**暂存从主存读过来的数据**，该数据不能存放在通用寄存器中，否则会破坏其原有内容。暂存寄存器对**应用程序员**是透明的
- **累加寄存器**：它是一个通用寄存器，用于**暂时存放ALU的运算结果**，可以作为加法运算的一个输入端
- **通用寄存器组**：如`AX`、`BX`、`CX`、`DX`、`SP`等，用于**存放操作数（包括源操作数，目的操作数及中间结果）和各种地址信息**。注意`SP`是堆栈指针，用于指示栈顶地址
- **程序状态字寄存器**：保留**由算数逻辑运算指令或测试指令的结果而建立的各种状态信息**，如溢出标志（`OF`）、符号标志（`SF`）、零标志（`ZF`），进行标志（`CF`）等。PSW中的这些位参与并决定**微操作**的形成
- **移位器**：对操作数或运算结果进行**移位运算**
- **计数器**：控制**乘除运算**的操作步数

## （2）两种数据通路设计方式

- 接下来介绍两种数据通路设计方式，来详细探讨这些寄存器的作用

- 注意下面的叙述仅仅是了解，不知道没有关系，后面会具体学习的

**数据通路：是指执行部件之间传送信息的路径，由控制信号控制**

### A：专用数据通路

**专用数据通路：根据指令执行过程中的数据和地址的流动方向安排连接线路。例如下图中每个寄存器与ALU都有专门的数据连线**

- 任何一个通用寄存器中保存的数据都有可能作为ALU的输入，因此需要**提供两组连线分别将通用寄存器两端连接至ALU两端**（注意连线并不是只有一根，而是要视具体的数据传输情况而定）  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2cb855a820604b618e9593fec8bf3301.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

**专用数据通路方式下，有可能多个寄存器会同时向ALU传输数据，这显然是不合理的，主要有以下两种解决方案**

- **多路选择器（MUX\)**：根据控制信号选择一路输出，**每个多路选择器都可以决定要把哪一个信号输出**。比如下图左侧的多路选择器信号为00，就表示让 R 0 R\_\{0\} R0​通过，右侧的多路选择器信号为01，就表示让 R 1 R\_\{1\} R1​通过  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9f77ae51389b45878c5af9dae5daf5be.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_17%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

- **三态门**：控制**每一路是否可以输出**。比如下图， R 0 o u t R0out R0out为1时表示 R 0 R\_\{0\} R0​的数据可以输出到A端， R 0 o u t R0out R0out为0时表示 R 0 R\_\{0\} R0​的数据无法输出到B端

**专用数据通路方式优缺点如下**

- **优点**：基本不存在数据冲突的现象
- **缺点**：结构复杂，流量大，不易实现，只在特殊场合、需求中使用

### B：CPU内部单总线（主要使用）

**CPU内部单总线：此种方式会将所有寄存器的输入和输出端都连接到一条公共的通路上**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fffbb26a6ce7b4ab4a0aeea331420a2b6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

- R x o u t R\_\{x\}out Rx​out表示寄存器的输出控制信号， R x i n R\_\{x\}in Rx​in表示寄存器的输入控制信号

ALU接受数据时也是通过总线接受，但这种方式会导致**ALU无法分清这是哪一个操作数**，所以我们可以在其中设置一个**暂存寄存器**。例如，**下图中 R 0 R\_\{0\} R0​的数据会被先送到暂存寄存器上，然后使 R 0 o u t R\_\{0\}out R0​out失效，再导通 R 1 o u t R\_\{1\}out R1​out，最后将 R 1 R\_\{1\} R1​数据输出到B端，这样的话就可以保证操作数的次序正确**

同时，**暂存寄存器也可以避免破坏寄存器原有的内容**。例如，某次运算两个操作数分别来自主存和 R 0 R\_\{0\} R0​，那么来自主存的操作数就可以直接放入暂存寄存器，而不用先放入A，**这样就避免了因A原本有内容而由于主存操作数的读入破坏了其内容的情况发生**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F80d412474acc4ac299603799e34bba16.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)  
ALU在计算完成之后仍然会将计算结果放回内部总线，不过这样做容易产生一个问题，**一旦输入端发送的信号还没有稳定前，ALU就产生了计算结果，并通过内部总线送回了寄存器，这样会导致运算错误**。所以我们可以在ALU的输出端再加一个暂存寄存器，同时在暂存寄存器上方加一个三态门，等ALU输出结果稳定之后，让三态门导通，然后给寄存器加上电信号让输出结果送回寄存器即可。

# 三：控制器基本结构

## （1）控制器概述

**控制器主要作用是取指令，分析指令和执行指令，主要涉及以下寄存器**

- **程序计数器PC**：用于指出**下一条指令在主存中的存放地址**
- **指令寄存器IR**：用于保存**当前正在执行的那条指令**
- **指令译码器**：仅对操作码字段进行**译码**，向控制器提供特定的操作信号
- **存储器地址寄存器**：用于存放要**访问的主存单元的地址**
- **存储器数据寄存器**：用于存放向**主存写入的信息或从主存读出的信息**
- **时序系统**：用于产生各种**时序信号**，它们都是由统一时钟（CLOCK）分频得到
- **微操作信号发生器**：根据IR的内（指令）、PSW的内容及时序信号，**产生控制计算机系统的所需要的各种控制信号**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F46a9fa782073427c92c53bede1fa061a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

## （2）控制器控制过程概述

**控制器控制过程概述：大致逻辑过程描述如下**

- **程序计数器PC**会指明下一条指令的地址，当取出该指令后会将其放到**指令寄存器**IR当中。指令的地址码指明了操作数的地址信息，所以**地址码的信息需要输出到内部总线上，而操作码部分会送给控制单元CU**。

- **具体来说，操作码会送给指令译码器** ，译码器的对应端会被选通，了解当前的指令类型后，就明白了下次执行的微操作是什么，所以**译码器的输出信号会作为微操作信号发生器**的输入信号，用于产生该指令的**微操作序列**

- 微操作序列需要受到时序系统的控制。**时序系统**产生时序信号，微操作发生器每接受到一次信号，就会产生一个微操作（注意此时会受到PSW标志位的影响，有可能改变微操作的类型）

- 最后还需要**MAR和MDR**，MAR连接地址总线，MDR连接数据总线，用于和存储器进行交互

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8ed4b271b08a4590b2991bfb9834ad4d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)

# 四：CPU的本质——寄存器的集合体

**CPU的本质就是寄存器的集合体，所以这也是CPU很贵的原因**

- **用户可见的寄存器**：通用寄存器组、程序状态字寄存器PSW，程序计数器PC
- **用户不可见的寄存器**：MAR、MDR、IR和暂存寄存器

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F398fa7f79cce4f08a8417b3ba8428c6d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120223141)