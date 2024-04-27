 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_7)
- [一：单块存储芯片与CPU的连接](#CPU_22)
- [二：多块存储芯片与CPU的连接](#CPU_43)
- - [（1）位扩展](#1_61)
  - - [①：单个连接](#_63)
    - [②：多个连接](#_86)
  - [（2）字扩展](#2_114)
  - - [A：线选法](#A_115)
    - [B：译码片选法](#B_154)
  - [（3）字位同时扩展](#3_201)
- [补充知识点：译码器](#_212)

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc420106fbd78433382584c89fad6d115.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

**现代计算机中，MAR和MDR通常集成在CPU的内部，而存储芯片内的仅是一个普通的寄存器。主存储器与CPU的连接示意图如下**

- 主存储器通过**数据总线、地址总线和控制总线**与CPU连接
- 数据总线的位数与工作频率的乘积正比于**数据传输率**
- **地址总线的位数**决定了可寻址的最大内存空间
- **控制总线**（读或写）指出总线周期的类型和本次输入输出操作完成的时刻

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4ee8e52c644e4ff6878fc07b5728060a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

# 一：单块存储芯片与CPU的连接

下图是前文中讲到过的一**单个存储芯片的内部构造**（[第二节：基本的半导体原件和存储器芯片的原理](https://blog.csdn.net/qq_39183034/article/details/119733615)），它和CPU通过以下总线连接（**具体过程会在“二：多块存储芯片与CPU的连接-（1）位扩展-①：单个连接”中进行描述**）

- **数据总线（绿色线）**：用于传送数据
- **地址总线（红色线）**：用于传送地址
- **控制总线（橙色线）**：用于发出控制信号

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F435dd0a2dc25497bab2719915c5e3c0b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

# 二：多块存储芯片与CPU的连接

**上图展示的只是一个8×8位的存储芯片，仅能存储8B的数据，所以会存在以下问题**

- 问题一：存储字长为8位，也即CPU一次只能读或写8位，而现代计算机数据总线宽度至少是64位，严重不匹配。因此问题在于如何**增加存储字长，使CPU一次能读或写多位数据** （对应**位扩展**）
- 问题二：只有8个地址，地址数目太少。因此问题在于如何**扩展地址空间，使地址数目变多**（对应**字扩展**）

**这里，为了后续描述方便，为一块存储芯片的输入输出信号进行命名**

- **地址线**：有可能输入多位的地址，因此地址用 A n A\_\{n\} An​表示， n n n从0开始，表示从地址低位到地址高位
- **数据线**：用 D n D\_\{n\} Dn​表示， n n n从0开始，表示从数据低位到数据高位
- **片选线**：片选信号通常用 C ‾ S ‾ \\overline C\\overline S CS或 C ‾ E ‾ \\overline C\\overline E CE表示，其中的横线表示低电平有效，高电平无效
- **读写控制线**：该信号用 W ‾ E ‾ \\overline W\\overline E WE或 W ‾ R ‾ \\overline W\\overline R WR表示，其中的横线表示低电平写，高电平读。（注意有些地方也可能将读写分开，分别为 O ‾ E ‾ \\overline O\\overline E OE或 W ‾ E ‾ \\overline W\\overline E WE，低电平表示有效，高电平无效）

## （1）位扩展

### ①：单个连接

下图是买来的一块8K×1位的存储芯片

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F52214df348b541988b656c077aee23f0.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

该存储芯片有8K个存储单元，由于 2 13 = 8192 2\^\{13\}=8192 213\=8192，这意味着至少需要1**3根地址线**才能表示这么多地址，**因此该存储芯片要向外暴露出13个地址引脚**，然后CPU会把它想要访问的地址通过**地址总线**送过来  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F448cb27a13494087b20af9346aa7a9dc.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

左下角的“ W E WE WE”表示`Write Enable`，上方没有横线，那么就表示**低电平读、高电平写**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Faa8426d2c45647b1a617f1a33079b9ce.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
上图显示CPU一次是可以读写8位数据的，**但是由于存储芯片字长的限制，所以一次最多只能进行一位**，这导致数据总线没有被充分利用

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8511481c15d64b8abc5cd7abb6001d9e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

还有一个片选线 C S CS CS，表示高电平有效，这里暂时先给一个高电平，具体作用后面会说

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc3679670cdc240419132bc426a576499.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
至此，单个芯片的连接已经完成了。**但是由于存储字长为1，一次只能读写一位数据，所以数据总线利用率很差**，因此在这种情况下可以进行**位扩展**

### ②：多个连接

接着又买到了一个和上面相同规格的存储芯片  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F70c43b363ca84dcaba804bb0ebfc6197.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

对于地址线，**我们从刚才连接的每一个地址线分别分流出一根线连接到该存储芯片的引脚上，这意味着一个地址可以同时选中两个存储单元**

对于读写控制线也是这样连接，这意味着它们是**同时读或者同时写**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2b78dd5fb904496a9b5e2721c888c69d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

对于数据线，**该存储芯片的引脚可以连接在CPU的 D 1 D\_\{1\} D1​位置**

最后，也给片选 C S CS CS线给一高电平

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd1c675d74dd343989e5f2cb09421534f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

至此，两块芯片连接完成。现在，**从整体上看存储字长被扩展为了2位，也即可以同时读或写两位的信息了**

最后，再买来6块芯片，连接好即可

- 每块芯片都有8个存储单元，CPU发出的 A 0 A\_\{0\} A0​\- A 12 A\_\{12\} A12​的这13位的地址信息会同时送给8片存储芯片  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F844cff4b091d47448527535a3934c5c9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

## （2）字扩展

### A：线选法

如下是买来的一块8K×8位的存储芯片  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdc2a9310ed14426ab58c37f1727cef14.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

单个芯片的连接过程同“二：多块存储芯片与CPU的连接-（1）位扩展-①：单个连接”，可自行研究

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0237dd8984884854a64b1f4c3ee8ad85.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

这个芯片明显不需要进行位扩展。但其问题在于：**此CPU的寻址能力很大，可以达到 2 16 2\^\{16\} 216，但却只利用了其中一部分，有3位没有被利用，所以在这情况下就要采用字扩展的方式来解决问题**

再买来一块相同规格的存储芯片，先采用之前的位扩展的规则进行连接，如下图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F67e96f46161a4d9b87dac2a87f5b236f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
可以发现采用这种方式连接将会产生很大的问题：**两块芯片会被同时选中，比如读数据时，两块存储芯片的8位数据会同时传给CPU，因此存在数据冲突**

**解决方法就在于片选信号 C S CS CS。现在，将 A 13 A\_\{13\} A13​连接到左边存储芯片的片选信号 C S CS CS上，将 A 14 A\_\{14\} A14​连接到右边存储芯片的片选信号 C S CS CS上。由于是高电平有效，因此当地址位为1时表示该存储芯片工作**

**A 13 A\_\{13\} A13​和 A 14 A\_\{14\} A14​只有两位，故取值只会有四种情况：01、10、11、00**

- **如果是01（注意 A 14 A\_\{14\} A14​是0 A 13 A\_\{13\} A13​是1， A 14 A\_\{14\} A14​是地址高位）**：此时左边芯片工作，右边片不工作。因此现在**CPU提供的这13位的地址只会读取左边存储芯片对应的8位的数据**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F83be46dcd2a744b2b1bd64b1e6c4a96c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

- **如果是10**：此时右边芯片工作，左边芯片不工作；因此现在**CPU提供的这13位的地址只会读取右边存储芯片对应的8位的数据**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4c07a169ad9a4b7085f5111b5bb6576b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

- **如果是11或00**：这种情况又会出现刚才的矛盾，因此**不能出现**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd90320f9c88a4502954cf3f46f6d1d6c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

**这样操作后，芯片所表示的地址范围会发生改变**

- **左边芯片**：010 0000 0000 0000\~011 1111 1111 1111
- **右边芯片**：100 0000 0000 0000\~101 1111 1111 1111\*\*

**这种连接方法称之为“线选法”，其缺点在于以00和11开头的地址是不能用的**

- 注意：不是仅有两个地址，是以00和11开头的所有地址均不可用，其数量是相当多的

### B：译码片选法

- **译码片选法会在线选法的基础上做一定改进，只需要加入一个非门**

**以 A 13 A\_\{13\} A13​为例，让它分别连接左边芯片和右边芯片的片选信号 C S CS CS上，但是在第二个线路中加入一个非门，这样当 A 13 A\_\{13\} A13​为1时，左边会被选中，右边由于非门的取反作用会变为不工作**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff2634128cb3d41e99b9b64139e353e7b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

这样的话，**左边芯片的地址范围就为：10 0000 0000 0000\~11 1111 1111 1111；右边的则为：00 0000 0000 0000 \~01 1111 1111 1111，整个主存地址空间是连续的**

上面用到的非门叫做“**1-2译码器**”，这种方法叫做**译码片选法**，**如果CPU有n条多余的片选线，那么他可以对应 2 n 2\^\{n\} 2n个片选信号**

- 译码器编号：1-2译码器是输入1个对应2个，2-4译码器是输入两个对应4个，以此类推

---

讲完上面的操作，现在可以使用真正使用字扩展了。**这里采用一个2,4译码器，也就是输入两个信号，输出4个信号，接着加入4个8×8位的存储芯片，每块存储芯片都会接受CPU发过来的低13位的地址信息**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc42fae8ee51540cab798efe76706449b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

- 注意上方图片中，每个存储芯片所有的地址信息都是直接来自于CPU的，不是从左边相邻的芯片传递过来的，这里是这样画只是为了整洁
- 在电路图中，当需要表示低电平有效时，通常会在上面画一个“小圆”

当 A 13 A\_\{13\} A13​， A 14 A\_\{14\} A14​为均为0时，就表示第一根线为1，剩余为0，但是经过译码器后，由于是取反，所以第一根为0，其余为1，而正好0表示存储芯片工作，所以这种情况第一个存储芯片工作，其余不工作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F903c4e3f4eaa428e8c261bd9984fae20.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
类似的，当 A 13 A\_\{13\} A13​为1， A 14 A\_\{14\} A14​为0时，就表示第二根线为1，剩余为0，但是经过译码器后，由于是取反，所以第二根为0，其余为1，而正好0表示存储芯片工作，所以这种情况就表示第二个存储芯片工作，其余不工作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F49f41132cfe844a981361d25f256ca17.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
因此：

**要访问第一块存储芯片，就要保证 A 13 A\_\{13\} A13​为0， A 14 A\_\{14\} A14​为0，此时地址范围为：000 0000 0000 0000到001 1111 1111 1111  
要访问第二块存储芯片，就要保证 A 13 A\_\{13\} A13​为1， A 14 A\_\{14\} A14​为0，此时地址范围为：010 0000 0000 0000到011 1111 1111 1111  
要访问第三块存储芯片，就要保证 A 13 A\_\{13\} A13​为0， A 14 A\_\{14\} A14​为1，此时地址范围为：100 0000 0000 0000到101 1111 1111 1111  
要访问第四块存储芯片，就要保证 A 13 A\_\{13\} A13​为1， A 14 A\_\{14\} A14​为1，此时地址范围为：110 0000 0000 0000到111 1111 1111 1111**

**所以这样的操作就能保证主存地址范围从全0到全1，而且是连续的**

另外还需要注意的是，考试时可能不会是连续的 A 13 A\_\{13\} A13​和 A 14 A\_\{14\} A14​，有可能是 A 13 A\_\{13\} A13​和 A 15 A\_\{15\} A15​，**但是无论怎么样，只要不选中，就不影响选片操作，是0是1不用管，只看选中的那几位**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd2c5e9017ef04e5b952c7739560f812b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

最后，这里的 A 15 A\_\{15\} A15​没有被启用，如果要使用的话那么就需要一个3-8译码器，然后再增加4个相同规格的存储芯片即可

## （3）字位同时扩展

位扩展可以使得存储芯片的字长变得更长，从而更好的发挥**数据总线的传输能力**；字扩展可以增加存储器的存储字数，从而更好利用**CPU的寻址能力**。既然二者都有的优点，那么就可以将它们综合起来，这种方法就是**字位同时扩展**

如下图有8块芯片，共有4组，每组两块，每组芯片实现了位扩展

- 前面的可以连接 D 0 − D 3 D\_\{0\}-D\_\{3\} D0​−D3​,后面的可以连接前面的可以连接 D 4 − D 7 D\_\{4\}-D\_\{7\} D4​−D7​

这是一个16K的存储芯片，因此将 A 0 − A 13 A\_\{0\}-A\_\{13\} A0​−A13​作为片内地址， A 14 − A 15 A\_\{14\}-A\_\{15\} A14​−A15​介入2,4译码器（因为有4组）。**一个芯片是16K×4位，一组就是16K×8位，整体就是64×8位**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa1069adb5b274575b73b2b7eaf18ff2b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

# 补充知识点：译码器

1：需要注意的是片选信号和译码器要配合使用，一定要注意是高电平有效还是低电平有效  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feaba99af9237435cb0708448e4984e36.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
2：译码器往往还有一个\(还有可能是多个\)和 C S CS CS类似功能的控制端，叫做使能端，即 E N EN EN  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc51cf68b724046a2aa3a260af15c9559.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

- 下面两个必须是低电平，上面必须是高电平才能工作。如果是其他非法状态，译码器右侧输出将会是全1

3：在实际场景中，CPU上还会有一个MREQ（访问存储器的控制信号），CPU会通过它来控制访问存储器。如下，只有当MREQ发出高电平时，经过非门，变为低电平后，译码器使能端变为低电平，此时译码器工作，地址才会被映射

- 之所以CPU需要控制，是因为这些地址信息都是电信号，开始时电信号是不稳定的，因此需要等稳定后才能“打开”译码器

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F188032d8597e4a97bd6ba139836290e6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd8f404a02c2f491d9fdc73a7c6820f52.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119852179)  
4：注意74ls138