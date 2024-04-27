 

### 文章目录

- [一：什么是命令](#_1)
- [二：如何识别命令](#_10)
- - [（1）显示命令的类型-type](#1type_11)
  - [（2）显示可执行程序的位置-which](#2which_13)
- [三：创建自己的命令-alias](#alias_16)

# 一：什么是命令

一条命令说来说去也就是其中下面四种中的一种

- **可执行程序**
- **shell内置命令**
- **shell函数**
- **alias命令**

# 二：如何识别命令

## （1）显示命令的类型-type

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210306230233266.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114461945)

## （2）显示可执行程序的位置-which

使用which命令可以显示可执行程序的位置，**需要注意的是which仅仅适用于可执行程序**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210306230859510.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114461945)

# 三：创建自己的命令-alias

使用alias可以创建自己的命令  
alias的基本格式是`alias name='string'`  
**如下我的用户目录下有个3\_6文件夹，3\_6文件夹下有一个test，我需要使用less来查看这个test文件，命名一个命令叫做MyCommand来执行这个流程**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210306232542151.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114461945)  
如果要删除它，可以使用`unalias`