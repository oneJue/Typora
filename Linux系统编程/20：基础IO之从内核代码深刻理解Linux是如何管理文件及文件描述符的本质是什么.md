 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）文件描述符到底是什么](#1_4)
  - - [A：输出描述符](#A_5)
    - [B：文件描述符](#B_34)
  - [（2）系统如何管理文件](#2_48)
  - [（3）一切皆文件](#3_54)
  - [（4）用源代码验证](#4_60)
  - [（5）FILE](#5FILE_72)

## （1）文件描述符到底是什么

### A：输出描述符

编写如下C语言文件，

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  int fd1=open("log1.txt",O_WRONLY);//打开错误
  int fd2=open("log2.txt",O_WRONLY|O_CREAT);//打开成功
  int fd3=open("log3.txt",O_WRONLY|O_CREAT);//打开成功
  int fd4=open("log4.txt",O_WRONLY|O_CREAT);//打开成功
  int fd5=open("log5.txt",O_WRONLY);//打开错误


  printf("%d\n",fd1);
  printf("%d\n",fd2);
  printf("%d\n",fd3);
  printf("%d\n",fd4);
  printf("%d\n",fd5);
}

```

运行后，结果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322150051362.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
第一个文件以只读的方式打开，但是磁盘中并没有这个文件，所以打开失败（C语言中的fopen，如果没有这个文件会自动创建，这一点也可以看出库函数是对系统调用的封装），**第2-4个文件打开成功，所对应的文件描述符就是整形数值，对应为3-5**，最后一个也是打开失败

### B：文件描述符

**文件描述符（File descriptor）：从系统层面讲就是一个个的整数，整数的范围是0-N ，你也许好奇为什么上述案例中第一个正常打开的文件的描述符是从3开始的,0,1,2呢？其实0,1,2就是标准输入，标准和输出和标准错误。**

可以使用前面的代码去演示，如下write时第一个参数写成1，表示输出到标准输出  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328151902440.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328151920439.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
**那么如果一上来就关闭1号文件呢，也就是一开始就把标准输出关闭。这样在创建文件时，新产生的文件就会被分配较低的文件描述符，也就是1号。而1号本该是输出到屏幕的，但是却输出到了文件当中去**——这其实就是重定向的本质  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328152618434.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328152640978.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)

## （2）系统如何管理文件

**文件描述符的数值其实有一个特点，就是起始是从0开始的，这和数组下标有相似之处。在进程管理中，我们讲到操作系统遵循的管理原则就是“先描述，再组织”，组织的形式就是PCB。那么操作系统管理文件时依旧遵循这样的原则**

**每当打开一个文件，操作系统就位这个文件建立一个结构体struct file，该结构体存储了文件的属性及其他相关内容，有100个文件就有100个这样的结构体。为了方便管理，这些结构体全部都会有一个指针指向它，所有指针放在一个数组中，因此构成了一个结构体指针数组struct file\* fd\_array\[\]。一个进程有可能会开辟多个文件，当用户把文件的描述符传递给进程时，进程就能根据文件描述符查找数组索引，依次来找到文件，进行相关操作**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322153033926.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)

## （3）一切皆文件

Linux下一切皆文件，这句话自学习Linux时就被不断提及，但是很多人对于这个概念还是模棱两可。**最主要的一点就是像硬盘，屏幕这类硬件为什么可以当做文件去对待？**

**Linux是由C语言编写完成的，C语言是一门面向过程的编程语言，不支持面向对象。尤其体现在其结构体中不能有成员函数，不支持面向对象不代表不能编写面向对象的代码。C语言中有一个非常强大的神器——函数指针。对于屏幕来说，屏幕之上有让其正确执行功能驱动代码，将显示器封装为结构体，使用函数指针指向驱动代码，这样不管是硬盘，还是键盘又或者是屏幕，从用户的角度上讲，他们都一样，都是文件，这也是一切皆文件的含义**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322154810412.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)

## （4）用源代码验证

上面的叙述都停留在理论角度，下面的是Linux内核源代码，也证实了这些

首先`task_struc`t里有`struct files_struct* files`，这样的一个结构体指针，指向一个结构体  
![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322162438718.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
接着`files_struct`中保存了一个指针数组，每个指针指向一个文件结构  
![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322162438809.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
每个文件结构体里保存了文件的一些基本信息

![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322162438926.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
值得注意的是有一个const struct file\_operations\* f\_op，这个结构体里全是函数指针——一切皆文件  
![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322162438895.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)

## （5）FILE

在学习C语言的时候，我们接触过`printf`函数，它的结果会输出到屏幕上，还有一个`fprintf`，他可以指定输出流输出，如果指定为了`stdout`，则也表示输出到屏幕上

所以如下的代码中，没有关闭1号文件，使用`write`（write输出到1号文件）,`printf`,`fprintf`进行输出  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328164033104.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
可以观察到屏幕上输出了对应的结果  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328164054836.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
然后接着关闭1号文件,此时结果就会重定向到log.txt这个文件中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328164427549.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
但是输出后缺乏只有write成功了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328164443937.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
这是因为使用`fprintf`和`printf`我们要刷新一下缓冲区，所以在最后加入`fflush(stdout)`  
然后再次执行，可以发现输出了结果  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328164818249.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)

通过上面的描述大家可能也发现了，`fprintf`和`prinf`在这里也说到了关闭1号文件的那个影响

**printf是一个库函数，会向stdout中输出，stdout本质也是一个文件指针**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328165313946.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
**其指向的结构体就是C语言中FILE**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328165459328.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232824)  
**这个结构体中的\_fileno其实就是文件描述符，而stdout指向的FILE结构体里的这个\_fileno默认就是1，所以这才会出现关闭1号文件后，printf函数也会将其内容输入到log.txt中的现象**