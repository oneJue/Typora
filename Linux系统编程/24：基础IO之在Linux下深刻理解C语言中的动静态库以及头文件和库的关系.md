 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- [七：动态库和静态库](#_6)
- - [（1）什么是库](#1_7)
  - [（2）静态库和动态库初步认识](#2_15)
  - - [A：静态库](#A_27)
    - [B：动态库](#B_41)
    - [C：头文件和库文件的关系](#C_51)

# 七：动态库和静态库

## （1）什么是库

**库就是现有的，已经写好的可复用的代码。每个程序都要依赖很多基础的底层库，不可能每个人编写代码时都要从0写起（比如printf，scanf）**

本质上库是一种可执行代码的二进制形式，可以被操作系统载入内存。库主要分为**静态库\(`.a .lib`\)和动态库\(.`so .dll`\)**

静态和动态指的就是链接。我们知道编译一个C程序需要经过**预处理，编译，汇编和链接**这4个步骤。在链接这个步骤，会将obj文件与系统库进行链接生成可执行文件。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330150139873.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)

## （2）静态库和动态库初步认识

使用一段简单的C语言程序做测试

```c
#include <stdio.h>
int main()
{
            
            
	printf("Hello World\n");
	return 0;
}
```

### A：静态库

**静态库在链接阶段，会将.o文件与用到库一起链接打包到可执行文件中**

编译时加上`-static`选项表示使用静态链接  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330152146904.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
然后使用file命令可以查看一些文件信息，其中重要信息如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330152255235.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
这段程序使用的静态库的名字是`libc.a`，其真正名字的是c，也就是c库  
**静态库体积较大，你可以将静态库看成一组目标文件的集合，也就是很多目标文件经过压缩打包后形成的一个文件**

静态库在链接时在预处理阶段就被添加到了代码中，所以会使得形成的可执行文件过大，但是程序在运行过程中就再也无需依赖库了，**所以其移植性很强，当然会很浪费资源和空间**

前面说过进程地址空间，每个进程都有自己对应的进程地址空间，由相应的页表负责映射，如果十个进程用到的静态库是一样的，那么相同的静态库就会被重复九编，由于它是直接拷贝进了代码，所以会造成进程地址空间，代码段部分过大  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330153837170.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)

### B：动态库

静态库最大的问题就是空间浪费，而且库与库之间也是有依懒性的，所以有时只需要一个库的代码，但是由于依赖性导致可能会链接到多个库，那么空间的浪费程度是很恐怖的。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330154117599.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
在Linux下面默认编译时使用的就是动态库  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330154217581.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
使用ldd命令可以查看该程序依赖的动态库  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330154258595.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
**大家会发现进程地址空间中在堆和栈之间有一部分共享区域，这是因为动态库可以在多个程序之间共享，所以动态链接就会使得可执行程序很小**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330154503941.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)

### C：头文件和库文件的关系

我们都知道使用`printf，scanf`需要引入头文件`#include <stdio.h>`。`printf，scanf`就是库函数，**一个标准的库包含两个东西：头文件和对应的`.a` 或`. so`文件**

比如C语言提供的头文件就在`/use/include`目录下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330161709679.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
C语言提供的库则在`/use/lib`目录下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210330161842861.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116233049)  
如果我要提供一个库供他人使用，那么我编写好众多`.c`文件和对应`.h`文件后，将`.c`文件编译为`.o`文件，然后将众多`.o`文件打包为上面的`.a`或`.so`文件，然后将`.a`或`.so`文件再次打包后，就能提供给别人使用了。

**所以头文件是告诉编译器有哪些方法，以及对应的参数是什么，`.a`文件`.so`文件则封装了它们的实现方法**