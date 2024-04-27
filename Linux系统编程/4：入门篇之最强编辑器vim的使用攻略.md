 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）创建文件](#1_11)
  - [（2）三种基本模式](#2_15)
  - [（3）vim命令模式指令集](#3vim_35)
  - - [A：复制与粘贴](#A_37)
    - [B：删除](#B_48)
    - [C：光标移动和定位](#C_70)
    - [D：撤销](#D_103)
  - [（4）vim末行模式命令集](#4vim_110)
  - - [A：查找](#A_112)
    - [B：设置行号](#B_117)
    - [C：跨文件操作](#C_121)
  - [（5）补充](#5_131)
  - [（6）vim配置](#6vim_147)
  - [（7）最后](#7_158)

  
**前言**  
之前编写C语言程序，经常会使用到VS，VS是属于 **IDE**的代表，也就是 **集成开发环境**，它将上述很多功能集成于一体，让开发者只去关心代码的编写操作，而无需过多顾及其他方面，让使用者感到很方便。

正如为什么不常用Python去实现数据结构呢，因为Python封装的太厉害了，不便于我们更深层次的去掌握这些细节。所以，的确IDE很方便，但是这种方便，却不利于我们深入理解计算机，理解程序的编译等等。比如大家都知道main函数有两个参数，但是为什么有两个参数，很多人却答不上来。但是这些问题在却能Linux编程中会得到一一解答。

在Linux中编写C语言或其他语言，可以去安装类似于VS这种集成开发环境，但是我们刻意让这些功能独立。**编写代码使用vim编辑器，编译代码使用gcc/g++编译器，调试代码时使用gdb调试器等等**。

## （1）创建文件

在安装好vim之后，终端中输入`vim`即可打开vim编辑器  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210127214717630.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)  
直接使用`vim [文件名]`，即可创建文件，并进行编写。但建议不要这样做，在编写代码写事先创建好目录，文件，然后在打开已经存在的文件进行编写

## （2）三种基本模式

vim是一款多模式的编辑器，共有12种模式，其中主要用到的有以下三种模式

- **命令模式：**刚进入后，会发现输入任何字符都不会进行回显，这是因为此时处在命令模式下
- **插入模式：**只有在插入模式下，才可以进行文字输入，按下`i`进行入插入模式，按下`ESC`则返回命令模式
- **末行模式：**末行模式可以进行文件的保存，退出等等操作。命令模式下输入`：`即可进入该模式

**1：进入插入模式的三种方式**  
使用vim打开文件后，可以三种方式进入插入模式，略有区别

| 方式 | 描述 |
| --- | --- |
| i | 光标原位，不会移动 |
| a | 光标向后移动 |
| o | 光标移至下一行 |
| **2：如何返回命令模式** |  |
| 按下`ESC`即可返回命令模式 |  |
| **3：如何进入末行模式** |  |
| 切换为命令模式后，输入`：`，即可进入末行模式。在末行模式中可以进行保存，退出等操作。需要保存时输入`w！`，需要退出时输入`q`！，保存并推出则输入`wq！`。如下： |  |
| ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210127220754851.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553) |  |
| 如果需要另存为，则进入末行模式下，输入`w 文件名` |  |

## （3）vim命令模式指令集

注意以下命令，必须在**命令模式**下操作

### A：复制与粘贴

**1：复制粘贴单行**

- 按下`yy`将**光标所在行**复制到缓冲区
- 按下`p`将缓冲区字符粘贴至**光标所在行的下一行**

**2：复制粘贴多行**

- 输入`nyy`，复制n行
- 输入`np`，粘贴n行

### B：删除

**1：删除某行**

- 输入`dd`表示删除光标所在行

**2：剪切**

- 输入`dd`删除后，然后再在某行输入`p`，可以实现剪切功能

**3：删除多行**

- 输入`ndd`，可以删除n行（同样可以进行多行剪切）

**4：删除字符**

- 输入`x`，删除光标所在的字符，输入一次删除一个
- 输入`nx`，删除光标所在位置及以后的n个字符
- 输入`X`，删除光标所在位置的前一个字符
- 输入`3X`，删除光标所在位置及以前的n个字符

### C：光标移动和定位

**1：光标的上下定位**

- 输入`gg`即可将光标定位至开始
- 输入`G`即可将光标定位至末尾
- 输入`nG`，即可将光标定位至第n行

**2：光标的左右定位**

- 输入`$`，将光标定位至所在行行尾
- 输入`^`，将光标定位至所在行行首
- 输入`w`，将光标定位至所在行下个单词的首字母
- 输入`b`，将光标定位至所在行上个单词的首字母

**3：hjkl**

- 输入`h`，将光标向前移动到上一个字符
- 输入`j`，将光标向下移动到下一行
- 输入`k`，将光标向上移动到上一行
- 输入`l`，将光标向后移动到下一个字符

注意这些命令的前面可以加入数字，比如5j表示下移5行

**4：打开时定位**

- 输入`vim test.c +n`，表示打开文件后定位到test.c的第n行，该操作经常用于定位错误日志所在行

大致总结如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313215326961.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313215345352.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)

### D：撤销

- 输入`u`表示撤销上次操作
- 输入`ctrl+r`表示重组

## （4）vim末行模式命令集

注意以下命令全部要在**末行模式**下进行

### A：查找

- 输入`/[查询关键字]`，可以查找所有匹配的关键字，然后高亮显示，按住`n`可以向下在关键字中切换

### B：设置行号

- 输入`set nu`调出行号

### C：跨文件操作

**1：分屏**

- 输入`vs[文件名]`（没有将自动创建，即可分屏）  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012723432546.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)

**2：切换窗口**

- 在**命令模式下**，输入`ctrl+w+w`（连按两下w），即可在打开的窗口中来回切换

## （5）补充

**1：大小写转换**

- 输入`shift+~`即可将光标所在的字母在大小和小写间来回切换

**2：打开上次编辑的文件**

- 在终端输入`！vim`可以打开上次编辑的文件

**3：快速退出**

- 快速输入两次大写`Z`，也即`ZZ`，即可快速保存并退出

**4：全部替换**

- 在末行模式下，要将所有的linux替换为Windows：`%s/linux/Windows/g`  
  **5：批量化注释**
- 使用批量化注释，需要进入vim的**块模式状态**下。比如注释C语言文件，首先光标移动到需要开始注释的地方，然后进入命令模式，按下`ctrl+v`，进入块模式，接着使用`h j k l`进行区域选择，然后输入大写的`I`，接着输入你要输入的符号，这里比如要输入//，接着快速按两下`esc`，完成注释。![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021022819141196.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)

## （6）vim配置

如果不对vim进行一定的配置，那么纯纯的vim使用起来一定是很不爽的，因为作为一款编辑器，没有代码不全，语法高亮等基本功能是万万不可以的

**一键配置，复制即可**  
注意配置文件在**普通用户目录**下，也就是每个人的vim是不一样的，每个人配置的是自己的vim。****

**```xml
curl -sLf https://gitee.com/HGtz2222/VimForCpp/raw/master/install.sh -o ./install.sh && bash ./install.sh
```** 

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128163240127.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144553)

## （7）最后

> [vim手册](https://github.com/wsdjeg/vim-galore-zh_cn)