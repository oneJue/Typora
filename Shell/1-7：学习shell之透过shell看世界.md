 

### 文章目录

- [一：扩展](#_2)
- - [（1）路径名扩展](#1_9)
  - [（2）波浪线扩展](#2_15)
  - [（3）算数扩展](#3_18)
  - [（4）花括号扩展](#4_25)
  - [（5）参数扩展](#5_35)
- [二：引用](#_40)
- - [（1）双引号](#1_45)
  - [（2）单引号](#2_54)
  - [（3）转义字符](#3_57)

# 一：扩展

其实每次你按下回车键时，bahs都会在执行命令前对文本进行多重的处理。举个简单的例子，`*`作为通配符的意思匹配任意多个字符，那么当命令行中有通配符`*`时，在执行命令之前，bash会把`*`进行扩展

echo命令用于显示文本，也就是将文本参数内容打印到标准输出，如下`echo ls`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308230527619.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
那么`echo *.txt`，就会把当前目录下所有以`.txt`结尾的文件名显示出来

## （1）路径名扩展

像刚才这种echo \*.txt，使用通配符实现扩展的机制称为路径名扩展，这种功能非常灵活，可以配合通配符实现很多需求  
比如找出当前目录下所有以大写字母开头的文件或文件夹  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308230902152.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
查看usr目录下，众多目录里含有share目录的路径  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308231101616.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

## （2）波浪线扩展

其实这个大家也都知道，`cd ~`是回到用户目录，而`echo ~`是显示用户目录  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308231356169.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

## （3）算数扩展

shell可以当作计算器使用。使用算数扩展要尊崇这样的格式：`$((expression))`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308231604470.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
算数扩展支持的算数运算符如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308231646594.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
这里算数扩展仅了解，更多要在后续介绍

## （4）花括号扩展

用过花括号扩展的人不得不说它的强大  
使用花括号扩展时，花括号内的以逗号分隔的字符会进行分别处理  
比如说我要一次性创建5个文件夹分别为`test_1_test`，`test_3_test`，`test_5_test`，`test_7_test`，`test_9_test`，一个一个创建太过麻烦，所以可以使用花括号扩展  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309200734114.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
还有更神奇的用法，如果花括号中左右两边中间是`..`，那么他就会按照左右两边的规律进行处理  
比如说创建26个文件夹，文件夹名字分别是26个大写字母  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309200920683.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
还比如生成1-10000的一万个数，则可写作`echo {1..10000} > test.txt`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309201124797.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

## （5）参数扩展

其实参数扩展，我在另外一篇文章中就讲到过了，就是如何显示环境变量，使用到的就是`ech $PATH`

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309201438425.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

# 二：引用

第一部分我们讲了，\$命令作为扩展的关键字，对命令进行扩展，但是有时会产生一些问题，比如说下图我的目的是在屏幕上显示 `I have $5.00`，但是\$遇见了5，于是开始了参数扩展，但是没有这样类型的环境变量，所以它被替换为了空字符串，于是输出了`I have .00`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309202505695.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
所以为了解决这样的问题，shell提供了引用

## （1）双引号

**双引号是第一种引用，主要用来处理空格问题，只要文本在双引号之中，Linux中除了\$,/外，其余特殊符号将失去意义，也就是说除了参数扩展，算数扩展外其他扩展将失效。**

空格在命令行中具有很特殊的意义，空格空开的两个字符很可能被当做为两个参数。如果必须要创建一个名字叫做`Hello World.txt`的文件夹，要是按照之前的方式创建，就会创建出两个文件夹  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309203050274.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
如果把文件夹名字放在双引号之内，将会忽略掉空格  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309203228680.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)  
但是注意双引号对参数扩展和算数扩展无效  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309203410727.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

## （2）单引号

如果要使得所有扩展都不生效，那么就是用单引号  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309203955669.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)

## （3）转义字符

我相信在这里，转义字符就不需要我多说了吧，能看这种类型文章人，还会不知道转义字符的作用吗  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210309204314336.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114555368)