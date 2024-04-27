 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）gcc/g++完成编译的过程](#1gccg_5)
  - - [A：预处理](#A_6)
    - [B：编译](#B_14)
    - [C：汇编](#C_23)
    - [D：链接](#D_31)
  - [（2）gcc/g++选项](#2gccg_36)
  - [（3）重要概念：函数库](#3_52)
  - - [A：gcc/g++在哪实现了函数](#Agccg_53)
    - [B：静态库与动态库](#B_61)

## （1）gcc/g++完成编译的过程

### A：预处理

- 预处理主要包括**宏定义，文件包含，条件编译，去注释**
- 输入`gcc -E hello.c -o hello.i`，其中选项E作用是让gcc在预处理后停止编译  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195123511.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200856214.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)

### B：编译

- 此阶段，gcc检查代码的**规范性，是否具有语法错误**
- 输入`gcc -S hello.i -o hello.s`，即可将预处理里的结果继续继续编译  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195406753.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200956550.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)

### C：汇编

- 编译阶段无误后，进入汇编，**将“.s”文件转化为“.o”二进制文件**
- 输入`gcc -c hello.s -o hello.o`，即可将编译停止在此阶段

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195837234.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)  
（打开二进制文件使用`od`命令）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128201057806.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)

### D：链接

- 此阶段，将目标文件与系统库进行链接生成可执行文件。
- 输入`gcc hello.o -o hello`，则完成编译

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200250407.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)

## （2）gcc/g++选项

| 选项 | 描述 |
| --- | --- |
| \-E | 进行预处理，不进行编译，汇编和链接 |
| \=S | 进行编译，不进行汇编和链接 |
| \-c | 进行汇编，不进行链接 |
| \-o | 链接 |
| static | 采用静态链接 |
| \-g | 生成调试信息 |
| \-shared | 使用动态库 |
| \-O0 | 无优化 |
| \-O1 | 默认优化级别 |
| \-O3 | 优化最高 |
| \-w | 不生成警告信息 |
| \-Wall | 生成所有警告信息 |

## （3）重要概念：函数库

### A：gcc/g++在哪实现了函数

在学C语言是，我们知道想要向屏幕正常打印字符，则必须在头部引入`#inlcude <stdio.h>`这样的头文件，因为`printf`函数的是在其中实现的  
在刚才的例子中，查看`hello.i`，也就是预编译生成的文件，可以发现`#include <stdio.h>`，在如下路径中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128202700704.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)  
进入该路径，可以发现这里存放的便是头文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128202853653.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116148074)  
可是gcc为什么知道头文件会在这个路径下的呢？实际上，在没有特别指定时，gcc会默认搜索路径`/usr/lib`，并进行查找。这也就是引用头文件时两种方式的区别所在：`#include <stdio.h>`会在设定目录下寻找，而`#include "Myhead.h"`，会在当前目录下寻找。

### B：静态库与动态库

- **静态库**是指编译链接时，把库文件的代码全部加入到可执行文件中，因此生成的文件比较大，但在运行时也就不再需要库文件了。其后缀名为`.a`。输入`gcc hello.c -o helloc -static`，，采用静态链接
- **动态库**在编译链接时并没有把库文件的代码加入到可执行文件中，而是**在程序运行时由运行时链接文件加载库**，这样做可以节省系统开销。动态库后缀名一般为`.so`，**gcc在编译时默认使用动态库**，完成链接之后，就生成了可执行文件。

所以动态链接形成的程序体积较小，比较节省资源，但是一旦库丢失，程序就不可以运行了；而静态形成的程序的体积很大，但具有独立性，即便库丢失，也不影响程序运行。