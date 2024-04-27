 

### 文章目录

- [一：再探ls命令](#ls_1)
- - [（1）基本使用](#1_2)
  - [（2）选项参数](#2_9)
- [二：file确定文件类型](#file_21)
- [三：less命令查看文件内容](#less_26)
- [四：探索Linux文件系统](#Linux_47)

# 一：再探ls命令

## （1）基本使用

前文说过直接键入ls命令，会列出当前工作目录下的文件和目录  
除了这样操作外，也可以使用ls配合目录名，来列出该目录的信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304210234450.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)  
同时不止可以列出单个目录，目录名之间使用空格空开，则开始列出多个目录下的信息

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304210412221.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)

## （2）选项参数

很多命令可以配合相应参数去使用，依次发挥功能类似但不尽相同的需求。基本格式为`command \-options arguments`  
比如说，ls有个选项-`l`会以长格式列出文件

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304210700993.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)  
相应的诸多参数如下，这里就不做一一介绍了，其中最长使用的使用矩形方框已经标识  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030421083329.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)

- 注意以上命令可以结合在一起使用，比如`ls -al`

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030421105698.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)

# 二：file确定文件类型

关于这个文件类型我们要特别说明一下，由于受到Windows的影响，我们总认为文件后面应该要加上后缀名。**但是在Linux中并不用文件的后缀名区分文件，而是根据文件的头部信息区分文件**

使用file命令可以查看文件类型，如果下尽管该C语言文件并没有后缀名“`.c`”，但是使用该命令仍然可以得知其为C语言文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304212510818.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)

# 三：less命令查看文件内容

在了解这个命令，我觉得有必要在这里说一下**什么是文本**\?

我们知道计算机是只知道0和1的，所有文件最终一定会被转化为0和1。为了能使计算机表示出字符（比如a，b，c等等），我们使字符与数字进行一一对应，也就是ASCII文本  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304213243760.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)  
在Windows中大家经常会使用到记事本，其实它就是一个处理ASCII文本的文件编辑器。

那么文本文件这么重要呢，因为Linux遵从**一切皆文件**，所以它的很多信息都是由文本文件构成（多数），比如后面的会说到的脚本也是以文本文件的格式存储的

使用less命令非常简单，只需`less+文件名`即可。由于less命令再查看行数非常多的文件时才有效果，所以读者可以用下面的脚本生成一个1000行的文件，然后输入表格中的命令进行相关测试

```bash
count=0; while [ $count -le 1000 ]; do echo "hello $count"; let count++; done > file.txt

```

less命令非常好用，是因为当键完命令后，我们可以通过输入相应的命令来控制一些行为，比如说翻页等等

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304213912658.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304213922586.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)

# 四：探索Linux文件系统

之前咋们在说文件系统时，只是泛泛而谈。但是现在学习这些命令后，我们可以肆无忌惮的在Linux系统中“游荡”一番了。下面的表格中Linux系统下的一些文件及文件夹，请读者们依据表格自行尝试

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304215746329.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304215759515.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114377798)