 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）进程的概念](#1_24)
  - [（2）如何管理进程](#2_32)
  - - [A：描述](#A_33)
    - [B：PCB](#BPCB_37)
    - [C：task\_struct](#Ctask_struct_40)
  - [（3）进程相关操作](#3_59)
  - - [A：查看进程](#A_60)
    - [B：进程与父进程](#B_70)
  - [（4）创建进程-fork](#4fork_100)
  - - [A：fork的作用：演示](#Afork_101)
    - [B：fork相关问题](#Bfork_213)

  
为了方便，引入需要用到一些命令（这些命令后续会说）

首先编写一个C语言文件，其作用是每隔一秒在屏幕上输出Hello

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
	while(1)
	{
            
            
		printf("Hello\n");
		sleep(1);
	}
}
```

运行这个程序，使用下面的命令获取这个进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227164706758.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
这个程序之前是在硬盘上，当我们使用`./test.exe`，就会把程序加载到内存中，所以开始运行后使用相关命令会查找到这个进程。

## （1）进程的概念

**所以像上述程序的一个执行实例，或者说正在执行的程序都叫做进程**

查看进程时，可以通过`/proc`系统文件夹查看，而且大多数进程信息可以使用`top`和`ps`这些工具获取  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227165655866.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)

玩游戏的时候可以同时听音乐，也可以看视频，所以**操作系统允许多个程序同时运行**，那么下面的问题就是面对如此多的进程，操作系统是如何管理的？

## （2）如何管理进程

### A：描述

前文说过，操作系统主要有四大功能：内**存管理，进程管理，文件管理和驱动管理**。对于操作系统，只要是管理就遵从先描述，再组织的原则

**对于Linux，现在它想要管理诸多进程，只需这样做：把每个进程的相关信息封装在一个struct中（因为Linux是由C/C++编写的），接着把这些结构体用我们学习的数据结构（比如链表，二叉树等）组织起来，进行管理时只需遍历他们，然后修改相应的信息**。

### B：PCB

**PCB——对于所有操作系统而言，所有进程信息被放在一个叫做进程控制块的数据结构中，你可以理解为进程属性的集合**。比如 前面咋们写过的C语言文件，如果它被加载进内存中开始运行，那么相应的进程信息就有比如对应的二进制文件，这个程序的运行时间，进程是由谁创建的等等。**所以如果那个C语言文件大小为1M，那么对应PCB实际占用的大小将会超过1M**（额外信息）

### C：task\_struct

就像“操作系统有Linux，Windows，而Linux又属于操作系统”这样的关系，task\_struct的关系也是这样。

**task\_struct——是在Linux中描述进程的结构体，它是Linux内核的数据结构，会被装在到内存里面。**

**task\_struct里面的信息大致包含如下**

| 种类 | 包含 |
| --- | --- |
| 标识符 | 描述本进程的唯一标识符，类似于身份证 |
| 状态 | 任务状态，退出代码，退出信号 |
| 优先级 | 相对于其他进程的优先级 |
| 程序计数器 | 程序中即将被执行的下一条指令地址 |
| 内存指针 | 包括程序代码和进程相关数据的指针等 |
| 上下文数据 | 进程执行时处理器的寄存器中的数据 |
| I/O状态信息 | 包括显示的输入输出请求等 |
| … |  |

## （3）进程相关操作

### A：查看进程

- Linux系统中所有的进程会被镜像在`/proc`中，所以可以列出此文件夹，查看相关内容。
- 还可以使用ps命令，ps命令最万能的用法就是`ps aux | grep 文件名字/进程标识符`
- 使用`ps axj`查看父子进程关系
- 使用`ps aux`查看所有进程

还是使用最开始的那个例子，运行程序。![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227191034984.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
列出PID为17963文件夹下，也就是这个进程相对应的文件夹，里面保存的就是进程的属性信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227191231973.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)

### B：进程与父进程

- 使用getpid\(\)获取进程标识符
- 使用getppid\(\)获取父进程标识符

修改之前C语言文件如下，注意导入头文件`<sys/types.h>`，使该文件在运行时可以输出其进程与父进程的标识符

```c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
            
            
	printf("本进程为：%d ; 父进程为：%d",getpid(),getppid());
	printf("\n");
	return 0;
}
```

结果如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F202102271935213.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
我们知道，程序运行时进程就被加载进去了，程序结束，进程结束。所以对应本进程来说，下一次再运行程序时的PID与之前运行时的一般会不一样，但是这里父进程却丝毫不改变  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227193753885.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
使用ps命令，抓取这个进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210227193844832.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
**这里的`-bash`叫做命令行解释器，所有命令都是由它创建的，那么自然而然它的PID就不会变化**。前面的程序的执行也是依靠它的，命令行解释器要是挂了，系统也就完了。  
**其实这里的命令行解释器也不是其本体，因为命令行解释器特别重要，所以命令行解释器只需创建它的子进程，让这个子进程代替自己完成任务，即便子进程挂了，也不会影响其自身。当然不是所有的操作都能有子进程完成，有些特殊操作还需要本体亲自出马**

## （4）创建进程-fork

### A：fork的作用：演示

**fork\(\)函数是用来创建进程的，在fork函数执行后，如果成功创建新进程就会出现两个进程，一个是子进程，一个是父进程，fork函数有两个返回值**

**1：演示一：创建子进程**

编写如下C语言文件，进入主函数后，执行fork函数，创建进程

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            

	printf("执行到fork函数之前其进程为：%d，其父进程为：%d\n",getpid(),getppid()); 
	sleep(1);
	fork();
	sleep(1);

	prinf("这个进程id为:%d,它的父进程id为%d\n",getpid(),getppid());
	sleep(1);
	return 0;
}
```

运行效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030110312350.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
根据上面程序的运行效果，似乎可以发现下面比较值得注意的几点

1.  它们的逻辑关系有些特点  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301103737803.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)
2.  从上面的动图可以发现，fork\(\)函数调用完成之后，\*\*它们似乎是同时输出的，这是否告诉我们这两个进程是同时进行的？\*\*或许它可以被画成这样？  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301104540762.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)

**2：演示二：创建子进程**  
前面说过，fork有两个返回值。官方手册中是这样解释到的

- **在父进程中，fork返回新创建子进程的ID**
- **在子进程中，fork返回0**
- **未能创建，fork返回负值**

**在子进程中，fork函数返回0，在父进程中，fork返回新创建子进程的ID。我们可以通过fork返回值判断当前进程是什么进程。**

根据以上描述，编写C语言代码，使用fork函数的返回值来进行分流

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
	prinf("还没有执行fork函数的本进程为：%d\n",getpid());
	pid_t=fork();//其返回值是pid类型的
	sleep(1);
	
	if(ret>0)//父进程返回的是子进程ID
		printf("我是父进程，我的id是：%d,我的孩子id是%d\n",getpid(),ret);
	else if(ret==0)//子进程fork返回值是0
		printf("我是子进程，我的id是%d，我的父亲id是%d\n",getpid(),getppid());
	else
		printf("进程创建失败\n");
		
	sleep(1);
	return 0;
}
```

效果如下![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301150834191.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
**3：演示三：一个大的问题**  
为了方便演示，修改上述代码如下，为每个if语句块内加入死循环，使其不能不断输出

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
	prinf("还没有执行fork函数的本进程为：%d\n",getpid());
	pid_t=fork();//其返回值是pid类型的
	sleep(1);
	
	if(ret>0)//父进程返回的是子进程ID
	{
            
            
		while(1)
		{
            
            
			printf("----------------------------------------------------------------\n");
			printf("我是父进程，我的id是：%d,我的孩子id是%d\n",getpid(),ret);
			sleep(1);
		}
	}
	else if(ret==0)//子进程fork返回值是0
	{
            
            
		while(1)
		{
            
            
			printf("我是子进程，我的id是%d，我的父亲id是%d\n",getpid(),getppid());
			sleep(1);
		}
	}
	else
		printf("进程创建失败\n");
		
	sleep(1);
	return 0;
}
```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301151157467.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
同时再使用之前的命令查看这个进程，发现也是两个进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301151219424.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
但是这里有一个很大的问题：**不过是什么语言，`if-else`执行时每次只能执行一路，怎么可能同时执行多路，同时每个if语句块内都有死循环，一个循环未结束，又怎么可能去执行其他语句呢**

**在Linux中，进程创建会形成链表，父进程创建子进程，那么父进程的进程指针会指向子进程ID。**

**所以这两个进程是同时运行的**

### B：fork相关问题

**1.如何理解进程创建**  
前面说过，操作系统在进行管理时，必然遵循**“先描述，再组织”**的原则，所以在进行进程管理时。首先会创建相应的`task_struct`，写入有关信息，然后和你编写好的代码共同组成进程

**2.为什么fork有两个返回值**  
根据上面的描述，可以大致描述fork函数的执行逻辑如下

```c
pid_t fork()
{
            
            
	//先描述，再组织，所以首先为子进程创建结构体
	struct task_struct* p=malloc(struct task_struct);
	//以下逻辑就是写入属性信息
	p->XX=father->XX;
	....
	p->status=run;
	p->id=1888;
	
	//到这里之前，子进程创建完毕
	return p->id;

}
```

其实，**进程数据=代码+数据，代码是共享的，数据是私有的**，上述逻辑中return之前的语句是父进程执行，结果就是生成了子进程，等执行return语句时，子进程已经生成，于是父子进程同时执行这一条语句，又因为数据是私有的，所以各自返回不同的值

**执行完fork之后，父进程pid不等于0，子进程pid等于0。这两个进程都是独立的，存在于不同地址中，不是公用的。  
fork把进程当前的情况进行拷贝，执行fork时，fork只拷贝下一个要执行的代码到新的进程**

为了说明变量不共用，可以编写一个C语言代码如下，同一个变量分别在父进程和子进程中修改

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
  int cout=0;
  printf("还未执行fork函数的cout=%d\n",cout);
  pid_t ret=fork();
  if(ret>0)
  {
            
            
    cout+=1;//父进程cout=1；
    printf("父进程：cout=%d\n",cout);
  }
  else if(ret==0)
  {
            
            
    cout+=10;//子进程cout=10；
    printf("子进程：cout=%d\n",cout);
  }
  else 
    printf("失败\n");

  sleep(1);
  return 0;
  

}

```

结果如下，可以发现它们不公用变量  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301155447518.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116188883)  
**3.为什么两个返回值不一样**

其实很好理解，**创建进程时相当于形成了链表（Linux）中，父进程指向子进程，所以返回的是子进程的ID，而子进程没有它的子进程，所以返回0。**

再者从现实生活中理解，一个孩子肯定知道它只有一个爹，而一个爹可能有多个孩子，所以子进程在标识父进程时就不要做那么多的区分，但是父进程可能有多个子进程，它与它在区分不同的子进程时必须要使用PID。

**4.为什么代码是共享的，数据是私有的**

首先代码是不可修改的（还记得代码段，数据段吗？），还有下面的常量字符串其实反映的也是这个道理

```c
const char* str1="Hello World";
const char* str2="Hello World";
//str1和str2地址相同
```

对于数据来说，如果数据不私有，造成的后果就是同一份数据在父子进程之间改来改去引起混乱