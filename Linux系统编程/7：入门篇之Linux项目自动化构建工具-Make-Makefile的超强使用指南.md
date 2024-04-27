 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）前言](#1_4)
  - [（2）依赖关系和依赖方法](#2_9)
  - [（3）单文件Makefile](#3Makefile_19)
  - [（4）多文件Makefile](#4Makefile_31)
  - [（5）总结](#5_41)

## （1）前言

对于一个大型项目，可能会涉及到很多文件，例如头文件，源文件等等。在VS中，我们只需在其设定的目录下，新建对应文件然后进行编写即可，然后按下运行键，只要你的代码没有问题，那么就可以运行出结果。这一切一切的感觉看起来很轻松，但是实则不然，例如首先编译哪个文件，如何链接？这些都是需要考虑的，但是VS作为一个集成开发环境，自然而然帮你做好了这一切，但是在Linux中，这些都是需要自己做的。而正因为做这些很麻烦，且不好理解，所以make，makefile应用而生

**make/makefilie：用于在Linux中维护项目文件之间的关系，**其中make是一个目录，makefile是一个文件，通常该文件存放于当前工作目录****

## （2）依赖关系和依赖方法

**通俗的来讲依赖关系是指：要生成A就必须先生成B，而依赖方法是指：怎样通过B生成A。**

比如前述在gcc编译过程时，这几个文件就存在以下依赖关系和依赖方法（假设由`hello.c`编译生成`hello.exe`）

- `hello.exe`依赖`hello.o`,依赖方法是`gcc hello.o -o hello.exe`
- `hello.o`依赖`hello.s`,依赖方法是`gcc hello.s -o hello.o`
- `hello.s`依赖`hello.i`，依赖方法是`gcc hello.i -o hello.s`
- `hello.i`依赖`hello.c`，依赖方法是`gcc hello.c -o hello.i`

## （3）单文件Makefile

简单其工作过程就是，在同一个目录下，创建一个文件叫做`Makefile`，然后在该文件内，进行编写，编写的代码反映了上述的依赖关系和依赖方法，然后返回终端通过输入目命令，来执行这个过程。很像Windows中的批处理。

准确来说：`make`是执行依赖关系和依赖方法的命令，`Makefile`是维护该机制的文件，`Makefile`里面保存了项目的依赖结构

- 如下，创建一个`hello.c`的文件，并编写简单代码，然后再在相同目录下创建文件`Makefile`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129201016968.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)
- 接着在`Makefile`编写如下代码，其中各项作用解释于图中。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129202955259.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)
- `Makefile`编写好之后，在命令行直接使用`make`，就可以调用这个文件执行操作。下面的两个操作相当于VS中的**生成解决方案和清理解决方案**。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129203143922.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)

## （4）多文件Makefile

编写一个简单加法程序，两个源文件：`main .c`，`compute .c`，一个头文件`compute. h`，使用Makefile进行编译

- 首先，编写程序如下  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012921245618.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)
- 编写`Makefile`如下  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129212738169.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)
- 执行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129212919682.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)

## （5）总结

综上所述，当我们编写好`Makefile`，然后在终端输入`make`，`make`就会在当前目录下寻找名字叫做`Makefile`的文件，如果找到，它会以该文件中的第一个文件（比如上面的`exe`文件）作为最终需要生成的文件，如果该文件不存在或者说它依赖的`.o`文件要比这个`.exe`文件新，那么它就会执行依赖方法，来生成这个`.exe`文件，同样的，如果`.o`文件不存在，那么`make`就会找这个文件的依赖性，再根据上面的规则走下去。但最终，`.c`文件和`.h`文件一定会存在的，于是一层套一层，最终直到生成目标文件。

 -    终极`makefile`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210131150659115.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149436)
 -    一般来说，make只会生成makfefile中的定义的一个目标对象，如果需要生成多个对象时，可以这样书写
 -    makefile文件保存了编译器和链接器的参数选项
 -    makefile主要包含了：显示规则，隐晦规则，变量定义，文件指示和注释
 -    默认情况下，make命令会在当前目录下按照顺序查找文件名字为`“GNUmakefile”`，`“makefile”`，`“Makefile”`的文件

```c
.PHONY:all
all: mytest mymain

mytest:test.c
	gcc -o $@ $^
mymain:main.c
	gcc -o $@ $^
```