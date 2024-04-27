 

### 文章目录

- [前言：什么是shell](#shell_2)
- [一：终端仿真器](#_12)
- [二：尝试输入](#_22)
- - [（1）shell提示符](#1shell_23)
  - [（2）第一次输入](#2_30)
  - [（3）试一下简单的命令](#3_41)
- [三：退出](#_51)

# 前言：什么是shell

首先需要明确的一点是，我们经常口中提到的命令行，其实指的就是`shell`，那么`shell`是什么呢，这就不得不用下面这么一张图了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304125034140.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)

- 注意，关于这个问题其实我在下面的这篇文章中说的已经很清楚了，如果需要理解请移步：[操作系统原理](https://blog.csdn.net/qq_39183034/article/details/114284873?spm=1001.2014.3001.5501)

大家可以看到shell处于用于操作接口，所以 **shell是一个把键盘输入的命令传递给操作系统的程序**

大家使用的可能是各种各样的Linux发行版，但是无一例外的都会提供这样一个shell程序，**这个程序来自于称之为`bash`的`GUN`项目**

# 一：终端仿真器

我们安装Linux时会有一个选项咨询你是否提供它的图形界面，**如果你安装了图形界面，那么就需要终端仿真器与shell进行交互。**

我相信在座各位有很多人使用的Linux是ubuntn，如果仔细查看你的桌面菜单，应该是可以找到一个终端仿真器的\(terminal emulator\)

比如我使用的是CentOS 7.0  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304132534826.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)

# 二：尝试输入

## （1）shell提示符

好的，现在让我们尝试打开终端，画面如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304132624428.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)  
**矩形方框中的文字我们称之为shell提示符，他表示此时shell准备接受外部的输入**。我的shell提示符是`zhangxing@MiWiFi-R4CW-srv ~ $`，你的可能和我的有所不同，但是基本都是`username@machinename`组合而成

**其中shell提示符最后有一个 `$`，这表示当前我是以普通用户登录的，如果后面是`#`则表示是以超级用户\(root\)登录的**。两者的区别就是，超级用户的权利非常大，几乎等同于操作系统

## （2）第一次输入

好的，现在在终端中随便输入，任意发挥，并按回车键

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304133021817.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)  
shell这样提示的原因是，没有这样的命令，让我们重新输入

如果你按上方向键可以发现，刚才那个胡乱输入命令又回到了终端，**这是shell的命令历史记录**。

- **注意不要使用ctrl+C和ctrl+V在终端进行复制粘贴操作**，因为这两个快捷键在Windows出来之前就有了他们自己的含义

## （3）试一下简单的命令

好的，为了小试牛刀，在这里我们尝试一些简单的命令  
比如输入`date`，显示日期  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304133452180.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)  
输入`cal`，显示日历  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030413351566.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)  
查看一下内存使用情况，使用`free`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304133547647.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114365756)  
Linux的命令实在太多了，这里就不一一介绍了。只要记住，命令行一定要输入正确的命令

# 三：退出

按下`exit`，退出终端