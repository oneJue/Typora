 

### 文章目录

- [一：什么是shell脚本](#shell_2)
- [二：如何编写shell脚本](#shell_5)
- - [（1）脚本文件的格式](#1_12)
  - [（2）可执行权限](#2_23)
  - [（3）执行脚本](#3_26)
- [三：第一个shell脚本](#shell_29)
- - [（1）基本结构](#1_32)
  - [（2）变量和常量](#2_75)
  - - [A：创建变量和常量](#A_97)
    - [B：变量和常量赋值的问题](#B_129)

# 一：什么是shell脚本

你可以这样理解：**shell脚本是一个包含一系列的文件，shell会读取这个文件，最后执行相应命令。就好像这些命令是直接输入到命令行中一样**

# 二：如何编写shell脚本

编写shell脚本必须明确三件事情

1.  **一个文本编辑器**：shell脚本是普通文件，所以需要文本编辑器进行编辑，这个编辑最好有代码高亮，语法提示等功能
2.  **使脚本可执行**：系统会执行你的脚本文件，所以必须要将脚本文件的权限设置为允许执行
3.  **将脚本放置在shell可以发现的位置**

## （1）脚本文件的格式

好的现在让我们编写第一个shell脚本，新建一个文件hello，该脚本文件的目的就是向屏幕输出Hello World  
于是输入

```bash
#! /bin/bash
echo 'Hello World' #this is a script

```

**就像`#include`一样，`#! /bin/bash`是用来告知操作系统用命令行解释器的名字，每一个shell脚本都应该以它作为第一行**

## （2）可执行权限

为了让该文件可以被执行，需要赋予执行权限。一般有两种，755表示每个人都可以执行，700表示只有脚本创建者可以执行  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401202215960.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

## （3）执行脚本

好的现在让我们执行文件，还记得是如何执行C语言编译好的文件吗？  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401202312446.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

# 三：第一个shell脚本

好的，经过前面的叙述，想必大家对shell脚本应该有了一个简单的认识了吧。**现在我来做一个案例，我们要编写一个报告生成器，用来显示系统的各种统计数据和状态，并以HTML的方式保存。这样每次运行这个脚本，它就能获取最新的系统状况了。**

## （1）基本结构

我认为HTML的基本格式大家都应该知道吧

```html
<DOCTYPE html>
  5 <html>
  6 <head>
  7     <meta charset="utf-8">
  8     <title>TEST</title>
  9 </head>
 10 <body>
 11     can you see me?                                                                                  
 12 </body>
 13 </html>

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401204201212.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

首先我们需要将这个html文件输出到标准输出，编写脚本如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401204227846.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

然后运行这段程序，并将其重定向到文件`info.html`中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401203700604.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)  
接着使用火狐浏览器打开这个文件，就可以看见相应的内容了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040120433124.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)  
好的继续完善如下

```html
<DOCTYPE html>
  5 <html>
  6 <head>
  7     <meta charset="utf-8">
  8     <title>System information report</title>
  9 </head>
 10 <body>
 11    <h1>System information report</h1>                                                                           
 12 </body>
 13 </html>

```

## （2）变量和常量

可以发现上面我们的脚本出现了两次`System information report`，类似于C语言中的宏，对于这种多次出现的如果需要修改时工作量会很大，所以我们这样做\*\*，类似于C语言中的字符串，定义一个变量存储它，修改时只需修改变量的内容\*\*

```html
  1 #! /bin/bash
  2 
  3 
  4 mytitle="System information report" //变量                                                                                  
  5 echo "
  6 <DOCTYPE html>
  7 <html>
  8 <head>
  9     <meta charset="utf-8">
 10     <title>$title</title>
 11 </head>
 12 <body>
 13     <h1>$title</h1>
 14 </body>
 15 </html>
 16 "

```

### A：创建变量和常量

不同于某些编程语言，shell遇见一个新的变量时，会自动创建这个变量。  
比如在命令行中,为变量赋值为yes，然后使用\$扩展进行查看  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401205509637.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)  
如果直接使用参数扩展查看一个没有命名的变量，那么这个变量将会被自动创建，并且被赋值为空值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401205928145.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

这样会导致一些问题，比如下面`test`和`test1`两个文件，还有`test`和`test1`两个变量，他么分别赋值为`test.txt`和`test1.txt`，使用`cp`命令将`test`变量的值复制给`test1`，如果输入正确，将达到我们的目的。但是如果故意输入错误，那么根据前面的描述，cp的第二个参数就会成为了空值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401210713410.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)  
所以变量就意味着会发生变化，类似于用const修饰的变量，我们需要常量，使其值不能发生变化，发生变化就会报错  
**在shell中我们用大写字母表示常量，用小写字母表示变量**  
我们把之前的例子，修改如下，使用扩展来输出我们的主机名

```html
  1 #! /bin/bash
  2 
  3 
  4 MYTTILE="System information report for $HOSTNAME" //变量                                                                                  
  5 echo "
  6 <DOCTYPE html>
  7 <html>
  8 <head>
  9     <meta charset="utf-8">
 10     <title>$TITLE</title>
 11 </head>
 12 <body>
 13     <h1>$TITLE</h1>
 14 </body>
 15 </html>
 16 "

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401211106445.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

### B：变量和常量赋值的问题

shell并不关心的你的变量是什么类型的，因为它都会被当做字符串，但是要注意以下问题  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401211937416.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)  
在扩展期间，变量名称可以用花括号括起来，当变量名因为周遭环境而变得不明确时，就会非常有帮助  
比如要使用变量的方式，将一个文件的名字由file改为file1  
如果直接操作，会发现由于\$name1会被当做新的变量名，而产生空值，所以使用花括号会解决这种歧义的问题  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401212309390.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)

好的回归正题，我们加入一些其他信息

```html
  1 #! /bin/bash
  2 
  3 
  4 MYTTILE="System information report for $HOSTNAME" 
  5 TIME=$(date)                                                                            
  5 echo "
  6 <DOCTYPE html>
  7 <html>
  8 <head>
  9     <meta charset="utf-8">
 10     <title>$TITLE</title>
 11 </head>
 12 <body>
 13     <h1>$TITLE</h1>
 14 	<h2>time is :$TIME</h2>
 14 </body>
 15 </html>
 16 "

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401213123557.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115383125)