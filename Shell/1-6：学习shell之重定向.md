 

### 文章目录

- [一：标准输入，标准输出和标准错误](#_7)
- - [（1）：标准输出重定向](#1_17)
  - - [A：\`>\`重定向](#A_20)
    - [B：\`>>\`重定向](#B_35)
  - [（2）：标准错误重定向](#2_48)
  - [（3）：将标准输出和标准错误重定向到同一个文件中](#3_57)
  - [（4）：标准输入重定向](#4_61)
- [二：管道](#_83)
- - [（1）less命令](#1less_88)
  - [（2）过滤器](#2_95)
  - [（3）uniq-去除重复行](#3uniq_100)
  - [（4）wc-打印行数，字数和字节数](#4wc_109)
  - [（5）grep](#5grep_113)
  - [（6）head/tail-只看开头或结尾](#6headtail_121)
  - [（7）tee-从stdin读取数据，同时输出到stdout（没有文件参数）或文件](#7teestdinstdout_130)

  
要说命令行中最酷的内容是什么，我觉得是 **重定向**与 **管道**

**重定向可以把命令行的输入重定向为从文件中获取内容，也可以把命令行的输出结果重定向到文件中**

**管道可以将多个命令行关联起来**

# 一：标准输入，标准输出和标准错误

一个命令或程序，按下回车键后，要么会显示程序运行的结果，要么会显示状态和错误信息。

以ls为例，当按下ls命令后，它会把其运行结果发送到一个称为**标准输出\(stdout\)** 的特殊文件中，其状态信息则会发送到一个称为 **标准错误（stderr）** 的文件中。**标准输出和标准错误都将会被链接到屏幕上，然后输出，它不会保存在磁盘中**  
我们都知道命令是通过键盘输入给电脑的，这个键盘叫做的**标准输入（stdin）**

**在默认情况下，标准输入和标准输出都是按照这样的逻辑进行的，而I/O重定向功能可以改变输出内容的发送目的（也就是不让你发送到屏幕上），也可以改变输入内容的来源地（也就是说甚至可以来自于文件）**

**总之，通常来说输出内容在屏幕上，而输入内容来自于键盘，但是重定向可以改变这种逻辑**

## （1）：标准输出重定向

**标准输出重定向符号是`>`’或`>>`，它表示把左面的内容重定向到右面。**

### A：`>`重定向

如下，使用`ls`命令，把ls命令输出的内容重定向到`a.txt`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307100358957.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
刚才的指令是正确的的，因为ls命令的列出的目录是存在的，但是现在我们改变一下。我们将一个根本不存在的目录，进行重定向  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307100926789.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
结果在意料之内，它的确是不存在的，但是还有一个很奇怪的问题，既然这个目录是不存在的，**那么为什么最终这个a.txt还是生成了？**

接着我们利用长列表显示这个文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307101135447.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
更奇怪的事情发生了，这个文件的大小竟然是0。**使用重定向“>”，进行重定向时，目的文件会从文件开头部分进行重写，但是上面咋们ls了一个根本不存在的目录，所以当重定向重写这个文件时，在出现错误的情况下停止了操作，因此文件内容被删除，而文件没有删除**

- 因此，这给我们一个启发，需要创建新的空文件或者删除文件内容时可以使用这种方法  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307101626979.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

### B：`>>`重定向

上面的`>`重定向只能从文件头部开始重写，有时会导致文件内容被删除，**而使用>>重定向可以实现从文件的尾部开始添加输出内容**

为了验证这一点，我们先用刚才的`>`重定向，将正确的内容重定向三次  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307102058549.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
可以发现即便正确重定向了三次，最终文件的大小也只能是54个字节

**但是同样方式利用`>>`完成，依然正确重定向三次，可以发现大小变为原来的三倍，也就是162个字节**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307102240130.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

- **从某种方面理解，你可以将>重定向理解为覆盖重定向，而把>>重定理解为追加重定向**

## （2）：标准错误重定向

前面，我们在故意错误重定向时，还发现了一个奇怪的地方  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307102501934.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
这个错误信息为什么被输出到了屏幕上，难道它不应该作为一种日志类的信息添加到文件中吗？

**其实这个问题在前面也能的到解答，ls命令不会把它的错误信息发送到标准输出文件中，而重定向到了标准错误文件中，这里我们只干了一件事情那就是重定向了标准输出，所以自然而然它就输出到了屏幕上**

冲向标准错误时和前面的有所不同，简单点来说：重定向时要加对应索引，0表示标准输入，1表示标准输出，2表示标准错误，所以要重定向标准错误时可以这样做  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030710421426.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （3）：将标准输出和标准错误重定向到同一个文件中

一般情况下，我们重定向时要同时重定向标准错误和标准输出（毕竟是日志信息嘛）  
**只需借助`&>`就可以同时重定向标准错误和标准输出**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307104731401.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （4）：标准输入重定向

**这里先介绍一下cat命令，后序会有更好的标准输入命令，因为cat命令其实很模糊，有的时候使用并不详细，但是有一个作用一定要记得，对于函数不是太长的文件，可以使用它查看**

cai命令命令准确点将是用来合并的文件。举个例子，在互联网上下载电影，并不是把这个电影一次性全部搞下来，而是分段下载，这些文件可能较`movie.avi1`,`movie.av2`,`movie.av3`·········，如果使用cat命令，则利用通配符可以一次性把这些文件全部合并

```bash
cat movie.avi* >movie.avi
```

上面的cat带有参数，如果这里直接只输入一个cat命令，会发现光标闪烁，正在等待我的输入  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307223550177.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
此时随便输入一些文字  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307223702566.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
然后按下`ctrl+D`，告知cat已经到达了文件尾了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307223810866.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
**由于缺少文件名，因此cat会把标准输入内容复制到标准输出文件（此时的标准输出文件就是屏幕），因此你会看到重复。**

现在我们加上文件名，再利用输出重定向，于是我们就做出了世界上最简单的文字处理器  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307224058473.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
再次使用cat查看文件（这里可就可以解释为什么cat具有查看文件内容的作用了，它会把文件复制到标准输出中）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307224201372.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
到现在我们知道了，cat默认的标准输入来源键盘，因此如果我在这里使用`<`，右面跟上文件名，那么标准输入源就成为了该文件。

# 二：管道

从第一部分的叙述我们可以得知：**命令从标准输入获取数据，然后把数据再发送到标准输出，这个过程本质其实是两个过程**，但是为什么感觉执行的时候感觉是一瞬间的呢？这其实利用了管道。

**使用管道操作符“|”可以把一个命令的标准输出发送到另一个命令的标准输入中** `Command 1 | Command2`

## （1）less命令

less命令可以接受标准输入，使用less命令可以分页显示任意命令的输入，该命令可以分页显示任意命令的输入，并将其结果发送到标准输出（屏幕）

如下输入`ls \-l /usr/bin | less`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307230142534.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
你可以把上述理解为这样：`ls \-l /usr/bin > test.txt`，然后`less test.txt`

## （2）过滤器

**管道可以完成复杂的操作，管道左侧的内容发送到管道，然后右侧进行操作，右侧操作完成之后，再传递给更右侧，有点像过滤的感觉，所以称为过滤器**

如下`ls /usr/bin | sort | less`，表示将/usr/bin的内容发送到管道，然后sort处理管道内容，再交给less，接着less把内容发送到屏幕，所以你看到的将是一个排序好的文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307230940243.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （3）uniq-去除重复行

uniq命令可以去除一些重复的行，比如下面的文件中我故意设置了一些很多行  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307231237196.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
首先使用cat命令，将其发送到屏幕，此时内容将作为标准输入发送到管道，接着uniq对管道内容进行处理，然后交给less查看。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307231419490.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

- 注意`uniq -d`表示只查看重复行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307231509456.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （4）wc-打印行数，字数和字节数

wc在没有任何文件参数时，默认以键盘作为标准输入源。  
下面是wc和管道的配合使用  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307232145381.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （5）grep

**grep功能非常强大，你可把它简单的理解为抓取某些字符**，grep不止可以匹配简单的字符，配合正则表达式，将会达到你意想不到的结果，但是本节只是展现一下其基本的用法  
如下，配合管道，我可以将文件中具有包含zip行的文本列出来  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308083808817.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
如果在输入加上选项`-n`，就可以打印出文本所在行行号  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308083910649.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
还有其他常用选项，读者可以进行尝试  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030808394068.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)

## （6）head/tail-只看开头或结尾

有些文件，你可只需要查看的前几行或者后几行，这里head和tail命令可以帮助你完成，**head和tail默认会输出文件的前10行和后10行**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308084452364.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
如果需要改变行数，在后面只需要加上`-n`即可，n代表行数

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308084547777.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)  
**其中tail有一个`-f`选项十分有用，可以查看正在被写入的日志文件的进展状态**。  
比如，`/var/log`下的message文件包含安全信息，它会时常更新，所以可以用`tail \-f`进行监视（可能要提高用户等级才能操作），如`sudo tail \-f /var/log/messages`

## （7）tee-从stdin读取数据，同时输出到stdout（没有文件参数）或文件

前面我们用管道时，管道后面的命令直接可以操作管道里的内容，但是现在我需要把管道里的东西保存到某个文件中（如果tee后面不加任何参数，那么默认就到标准输出文件，也就是屏幕）该怎么办呢？可以使用`tee`命令完成  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308085602864.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114476945)