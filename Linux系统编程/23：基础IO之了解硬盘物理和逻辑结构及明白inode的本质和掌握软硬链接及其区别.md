 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）硬盘的逻辑结构与物理结构](#1_4)
  - - [A：物理结构](#A_6)
    - [B：逻辑结构](#B_27)
  - [（2）inode](#2inode_41)
  - - [A：inode是什么](#Ainode_42)
    - [B：块组](#B_55)
    - [C：块中有什么](#C_63)
    - [D：创建，删除文件的本质](#D_77)
    - [E：目录的本质](#E_85)
  - [（3）软硬链接](#3_95)
  - - [A：复习](#A_96)
    - [B：软硬链接对比](#B_99)
    - [C：链接数目的问题](#C_120)

## （1）硬盘的逻辑结构与物理结构

文件，自然而然是存在于硬盘中的，所以想要说清楚文件系统，必然先要理解硬盘的逻辑结构与物理结构。由于这部分所涉及的内容非常复杂，所以叙述时以简洁明了为主，一些细节问题将忽略

### A：物理结构

**物理结构一般由磁头、盘片、电动机、主控芯片、排线、接口等部件组成**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328220655618.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

**1：磁头**

> 磁头是硬盘中对盘片进行读写工作的工具,是硬盘中最精密最关键的部位之一。最初的磁头是读写二合一的,后来逐渐分离出读磁头和写磁头两个部分

**2：盘片**

> 盘片是硬盘中承载数据存储的介质。 硬盘中一般会有多个盘片,每个盘片包含两个面,每个盘面都对应地有一个读/写磁头

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328221008905.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
盘面又被划分为若干个同心圆的磁道,其中最外圈的就是"0"磁道.每个磁道又会被划分多个段，称为扇区，每个扇区容量为512个字节  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328221412496.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
**3：动态演示**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032822243492.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

### B：逻辑结构

硬盘结构复杂，操作系统对这些扇区进行寻址时将会变得很麻烦，**所以一般是用文件系统把硬盘的若干扇区组合成簇，然后文件和树形目录，方便查找数据**

简单来说，我们可以将盘片展开，展开成一个长条状，然后划分边界范围，根据边界范围就能很容易确定硬盘中的某些扇区处于的位置  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329085339211.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

- 0-1000一个分区，1000-2000一个分区

**正如中国国土面积巨大，所以管理时难度很大，因此被划分了各个行政单位。在硬盘中也是如此，硬盘容量动辄1T，2T，也不是很容易管理，所以硬盘分区被划定为一个个的block，一个block的大小是由格式化的时候确定的，并且不可以更改。但是不像真实生活中那样，管理北京市和上海市还是有区别的，因为还会涉及到其他一些因素，但是管理块就没有那么大的麻烦了，只要把一个块管理好，剩余的全部进行“复制粘贴”就可以了**

而如何划分，以及每个block里怎么办，有什么数据都是文件系统决定的

> 文件系统是操作系统用于明确存储设备（常见的是磁盘，也有基于NAND Flash的固态硬盘）或分区上的文件的方法和数据结构；即在存储设备上组织文件的方法。操作系统中负责管理和存储文件信息的软件机构称为文件管理系统，简称文件系统。文件系统由三部分组成：文件系统的接口，对对象操纵和管理的软件集合，对象及属性。从系统角度来看，文件系统是对文件存储设备的空间进行组织和分配，负责文件存储并对存入的文件进行保护和检索的系统。具体地说，它负责为用户建立文件，存入、读出、修改、转储文件，控制文件的存取，当用户不再使用时撤销文件等

## （2）inode

### A：inode是什么

我们知道**文件=属性+内容**，即便你创建了一个大小为0个字节文件，但是其真实大小并不是0个字节，描述该文件的属性也要占据一定的空间。

**所以我们把文件的属性的集合称之为inode，把文件的内容的集合称之为block，这些文件可以删除，可以复制，可以粘贴，所以我们把实现它的一些方法以及前面的inode和block等其他信息组装在文件系统里，以此方便对文件进行管理**

大家思考一个问题，ls-l为什么可以皮准确的列出文件及其信息呢  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329092024234.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
其实，当我们输入ls \-l后这条命令就成为了进程，这条进程就会根据一定的方法去拿到对应的文件信息，然后将其加载进内存，显示在我们的面前  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329092156758.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
而它是怎样找到对应的文件的呢，其实就和inode有关，使用stat命令查看文件的一些信息时，会发现其中就有一个叫做inode的属性  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329092345406.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

### B：块组

如下可以看做是一个分区（比如上图中的0-1000），一个分区搞好了，其余分区也是这样，**需要注意的是这里我们讲的是硬盘上的结构，当进入内存后，内存里也有其相应的映像**

**每个分区中又会被划分，也就是咋们上面说过的块（block group），同样这个分区中只要管好了一个块组 其余也都是是复制粘贴了**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329090032388.jpg&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

这里大家可能也注意到了，每个分区前有一个`boot block`，它是引导块，它包含了一个硬盘的一些基本信息，比如放在D盘中的软件，知道操作系统在哪里，就是这个引导块告诉它的，当然这一部分损坏的话，这个分区也就完了。

### C：块中有什么

这里以一个块组为例，展示一下块组中的分配，其余块组都是一样的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329090711897.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

- `Super Block`：**超级块**，存放的是文件系统本身的结构信息。记录的信息有block和inode（后面会说）的总量，未使用的block和inode的数量，一个block和inode的大小，最近一次挂载的时间，最近一次写入的时间等等。一旦破坏了Super Block那么整个文件系统的结构就被破坏了
- `Group Descriptor Table`：**块组描述符**，描述了块组属性信息
- `inode Table` 和 `Data block`：i**node Table中存储的就是Inode，每个inode会映射到对应data blocks**。一个文件就对应一个inode，inode table有多大就有多少个inode也就可以最多存放多少个文件。有时候会出现一种情况，就是硬盘空间还有，却无法创建文件，这种原因就是文件数太多，但是每个文件又太小，将inode Table给占满了
- `inode Bitmap`：上面两个很好理解，但是有一个问题：如何保证inode不重复？那么这就要靠inode Bitmap，**本质是位图**，如果inode Table里能存十万个inode，那么inode bitmap就是十万个0，也就是0000…000000（这10万个0占据的空间其实很小），创建文件时，就去遍历这个位图，只要位置处是0，那么就可以申请一个对应的inode
- `Block Bitmap`：既然inode有位图，那么Block也有位图，其原理和上面的是相似的，只要位图某个位不为0，就能在块中申请空间，然后将对应位图的位传递给inode Table 和 Data block就能建立对应关系。比如第三个位是1，那么Date blocks=inode Table\(3\)

于是上面的文字信息可以用下面这张图解释  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329135919324.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

### D：创建，删除文件的本质

综上，在一个分区中创建文件的过程可以描述成这样：**要创建文件，首先就要给这个文件分配inode，所以在inode位图中找到不为0的位置，置为1，然后根据这个分配inode在inode Table中写入文件的创建信息；接着写入内容，在block位图中也去遍历，找到不为0的位置，置为1，然后在Data blocks中分配空间，同时让inode和Data Blocks建立映射关系，这样inode就对应了一个或多个数据块**

同时，如果一个文件的inode是1234，那么查找这个文件的过程：**首先肯定要找到对应的分区和块组，然后遍历inode Table找到对应的值，拿到属性之后，根据映射关系，就可以找到数据块内容**

相应的，如果要删除一个文件：**删除数据时，无需将inode Table和Data block中的内容抹去，直接在inode位图中将对应的1置为0即可。因为硬盘本质就是存放的是0和1，在这里没必要去做一些无用操作，这也就是为什么删除文件会很快的原因 。同时既然删除文件时，没有抹去inode，所以恢复文件时知道当时的inode就可以遍历inode table实现恢复，所以一些数据恢复软件的本质也是基于此的。**

### E：目录的本质

前面的文件大家应该能够理解了，那么这里目录究竟干了什么。首先能够确定的一点，Linux下一切皆文件，所以目录是一种文件。

当按下ls \-l，所有文件信息被展示了出来  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032914031058.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

然后在该目录下，使用ls命令，可以列出某个文件的具体信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329140348820.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
这些文件的信息没有保存在目录里面，操作系统也没有必要这样做。**目录只维护了文件名和其对应的inode的映射关系** `ls \-l test.txt`，它首先找到目录的数据块，这些文件名和映射关系就存储在这里，然后根据映射关系再去列出文件的具体信息

## （3）软硬链接

### A：复习

我在《Linux命令行大全》之[1-4：学习shell之操作文件与目录](https://blog.csdn.net/qq_39183034/article/details/114412404)这篇文章中讲到过如何创建硬链接和符号链接（软链接），这里我就不再重复了，同时本文也是解答了那篇文章的疑惑

### B：软硬链接对比

有test.txt这样的一个文件，其inode是1189005  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142140536.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
像这个文件中写入随意内容  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142225878.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
然后首先创建一个硬链接，叫做`hard_test.txt`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142334576.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
可以发现，他们的inode同样的值，说**明他们对应的是一个相同的文件**，所以在`hard_test.txt`上进行修改后，test.txt也一定会变化  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142518963.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
前面我们说过，**目录保存的是文件名和映射关系**，所以在这里如果删除了原来的文件，它删除的只是文件名和映射关系，而不是文件本身，相当于这里进行重命名  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142703303.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

**\<分割线>**

重新回到开头，为`test.txt`创建软链接  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142918619.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

查看其详细信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329142957483.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
发现并不是相同的inode，**这表明软连接是一个独立的文件**，这个文件的内容里保存的是路径等其他信息

### C：链接数目的问题

如下，创建几个文件和目录  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329143811504.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
可以发现文件的默认的链接数是1，这一点也很好理解，因为自己就是那个对应的关系，如果为test 1创建一个链接，可以发现其链接数就变成了2  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329144008833.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)  
那么为什么目录的链接数默认是2呢？其中一个肯定是自身，还有一个（这里我们进入dir3）就是隐藏文件\`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329144346361.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)

那么2\_29这个目录链接数为什么是5呢：自己算一个， `.` 算一个，然后dir1，dir2，dir3中各自有一个隐藏文件`..`，指向了3\_29，因此总共5个  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210329144609885.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232991)