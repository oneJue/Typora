 

### 文章目录

- [前言](#_1)
- [一：进程如何工作](#_17)
- - [（1）使用ps命令查看进程信息](#1ps_23)
  - [（2）使用top命令查看资源管理器](#2top_41)
- [二：控制进程](#_54)
- - [（1）中断进程](#1_57)
  - [（2）使进程在后台运行](#2_64)
  - [（3）fg-使进程回到前台运行](#3fg_73)
  - [（4）暂停进程](#4_76)
- [三：信号](#_80)
- - - [A：使用kill命令发送信号到进程](#Akill_85)
    - [B：使用killall命令发送信号给多个进程](#Bkillall_93)

# 前言

进程可以说是Linux中非常重要的概念了，关于进程一些核心概念大家可以移步，在这篇文章中详细查看

> [Linux进程](https://blog.csdn.net/qq_39183034/article/details/114284873)

**本章需要用到的命令如下**

- `ps`：显示当前所有进程的运行情况
- `top`：相当于资源管理器
- `jobs`：列出所有活动作业的状态信息
- `bg`：设置在后台中运行作业
- `fg`：设置在前台中运行作业
- `kill`：发送信号给某个进程
- `killall`：杀死指定名字的进程
- `shutdown`：关机或重启系统

# 一：进程如何工作

> 系统启动时，内核先把它的一些程序初始化为进程，然后运行一个称为init的程序。init程序依次运行一系列称为脚本初始化（init script）的shell脚本（存放于etc目录下），这些脚本将会启动所有系统服务。其中的很多服务都是通过守护程序（daemon program）来实现的。而后台程序只是呆在后台做他们自己的事情，并且没有用户界面。所以即使用户没有登录，系统也在忙于执行一些例行程序

内核会保存每个进程的信息以便确保任务有序进行，**比如每个进程都将分配一个称为进程ID（PID）的号码**。进程ID是按照递增的顺序分配的，init进程始终是1。

## （1）使用ps命令查看进程信息

如果直接输入ps命令，将会输出和当前终端会话相关的进程信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312211539222.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

- `TTY`代表了进程的控制终端
- `TIME`表示了进程消耗CPU的时间总和

如果在`ps`后面跟上选项`x`，也就是`ps x`，表**示告知ps命令显示所有的进程无需关注他们是由哪个终端控制的**（因为有可能有多个用户在使用电脑，所以TTY显示为问号）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312212110293.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

- 上图中，多列出一个选项`STAT`，它是state的缩写，表示了进程状态。关于进程状态我在前言的那篇文章中也做了深入的探究。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312212357884.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
  还有一个常用的选项是`aux`，也就是`ps aux`，该选项会输出每个用户的进程信息，并且输出的信息更加丰富  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031221254916.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)
- 上图中，列表标题的含义如下  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312212757893.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

## （2）使用top命令查看资源管理器

top命令相较于ps命令而言，top可以动态显示进程的信息，默认每3s更新一次  
**它主要查看的是最高进程的运行状况**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312213000338.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

- 整张图分为两个部分，上半部分显示的是系统总体状态信息，下半部分显示的是一张按照CPU活动排序的进程情况表

其中系统总体状态信息显示的内容非常有用，主体注解与参考如下（标号对应）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312213747418.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312213845704.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
top命令类似于Windows中的资源管理器，但是它是由于资源管理器的，大家可能有这样的体会，一打开资源管理器CPU的占用率就会直线上升。

# 二：控制进程

为了方便演示，我们在终端中输入`xlogo`，xlogo是由X窗口系统提供的一个实例程序，它简单地显示了一个包含X标识的可缩放窗口  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313091817542.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

## （1）中断进程

X窗口打开的情况下，可以发现终端的shell提示符并没有返回，那是因为这个进程正在运行当中。**如果在终端中输入`Ctrl+C`，那么这个进程将会被终端，并且shell提示符返回**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313091956539.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

- 需要注意后台进程是无法用这种方式终端的，这一点后面会讲到

## （2）使进程在后台运行

可以发现在xlogo运行期间，我们是无法对终端进行其他操作的，所以如果想要让进程不要在前台运行，**可以在命令后面加入`&`，也即是`xlogo &`，这样的话进程将会转到后台运行**，效果就是我们仍然能在终端中输入其他命令  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313092305500.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
大家还可以发现另外有趣的一点：当把输入`xlogo &`后，终端显示了`【1】4467`这样的字样，**这种表现其实专业术语叫做shell的作业控制**，转到后台后，shell会告诉你已经启动作业编号【1】，对应PID为4467

如果输入ps命令，可以发现除了前面讲过的那两条基本进程外，此时还多了一个咋们刚才转到后台运行的xlogo进程，而且其PID恰好就是4467  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313092630210.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
**如果想要查看由该终端启动的所有作业（也就是后台进程），可以输入`jobs`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313092844243.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

## （3）fg-使进程回到前台运行

后台运行的程序是无法使用`Ctrl+C`中断的。**如果要使得后台进程转到前台运行，可以使用fg命令，在fg后面加上百分号和作业编号**，也就是`fg %1`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313093250977.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

## （4）暂停进程

前台启动xlogo后，如果此时按下`Ctrl+Z`，那么进程将被暂停，同时终端提示已停止，该进程会被转到后台  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313095405140.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

# 三：信号

**使用`kill`命令可以杀死一个进程**，尤其是杀死那些不正常的拒绝终止的程序  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313095942569.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
但是kill命令的用法并不只是这么简单，准确点来说kill的含义是发送信号给进程，使用`kill \-l`可以发现kill可以发送的信号有这么多  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313100057987.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

### A：使用kill命令发送信号到进程

kill的基本用法就是`kill \-信号代码 PID`  
众多信号中，最为常用的是以下几种  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313100242167.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313100252521.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

- 需要注意`kill -9`，**这种终止进程的方式属于“无奈之举”，当进程以这种方式被终止时，它将没有机会对自己进行清理或保存工作**。下面的这张漫画很形象的说明了它  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210313100658157.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114707681)

### B：使用killall命令发送信号给多个进程

使用killall命令可以给指定程序或者指定用户名的多个进程发送信号，格式为`killall \-user \-signal name`