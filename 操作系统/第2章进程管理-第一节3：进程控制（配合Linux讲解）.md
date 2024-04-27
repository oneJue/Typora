 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/125668174)

### 文章目录

- [一：什么是进程控制](#_10)
- [二：如何实现进程控制——原语](#_16)
- - [（1）复习：原语](#1_18)
  - [（2）使用原语实现进程控制的原因](#2_25)
  - [（3）原语的“原子性”是如何实现的](#3_40)
- [二：进程控制原语](#_64)
- - [（1）进程创建](#1_69)
  - - [A：概述](#A_71)
    - [B：补充-Linux中的创建进程操作](#BLinux_94)
    - - [①：fork\(\)](#fork_101)
      - [②：fork\(\)相关问题](#fork_212)
  - [（2）进程终止](#2_300)
  - - [A：概述](#A_302)
    - [B：补充-僵尸进程与孤儿进程](#B_318)
    - - [①：僵尸进程](#_323)
      - [②：孤儿进程](#_386)
  - [（3）进程阻塞\(Block\)/等待\(Wait\)](#3BlockWait_445)
  - - [A：概述](#A_446)
    - [B：补充-Linux中的进程等待](#BLinux_459)
    - - [①：为什么进程需要被等待/阻塞](#_463)
      - [②：进程阻塞式等待](#_467)
      - [③：进程非阻塞式等待](#_554)
  - [（4）进程唤醒（Wake）](#4Wake_559)
  - [（5）进程切换](#5_575)

# 一：什么是进程控制

**进程控制：是指对系统中所有进程实施有效的管理，它具有创建新进程、撤销已有进程、实现进程状态转换等功能。可以简单理解为实现进程状态转换**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8d838f2687914dec8fa98641ce4610c2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

# 二：如何实现进程控制——原语

## （1）复习：原语

**原语：按层次结构设计的操作系统，底层必然是一些可被调用的公用小程序，他们各自完成一个规定的操作，通常把具有如下特点的程序称为原语\(Atomic Operation\)。定义原语的直接方法是关闭中断，使所有动作不可分割地完成后再打开中断**

- 处于操作系统的最底层，**最接近硬件**
- 这些程序的运行具有**原子性**，其操作只能 **“一气呵成”**（为了系统安全性和便于管理考虑）
- 这些程序的**运行时间都较短**，且**调用频繁**

## （2）使用原语实现进程控制的原因

**进程控制（进程状态转换）是一个复杂的过程，整个过程需要一气呵成，也即不能中断，所以需要依靠原语来实现。想要实现进程的状态转换，操作系统至少需要2步**

1.  修改PCB中的用于标识进程目前状态的变量`state`
2.  将进程投递到对应的队列

**以上的步骤中，如果在完成第1步后被中断了，那么就会产生矛盾：进程状态被改变了但却没有在对应的队列中**

- 就像网上购物已经确认收货了，但实际没有收到

## （3）原语的“原子性”是如何实现的

- 此部分内容涉及《计算机组成原理》相关知识

**原语原子性是依靠关中断和开中断这两条特权指令来实现的**

**正常情况下，CPU每执行完一条指令都会例行检查是否有中断信号需要处理，如果有，则暂停运行当前这段程序，转而执行相应的中断处理程序**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F093d05582ee34a3c9ee0635deec4bf78.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

**当CPU执行了关中断指令之后，就不再例行检查中断信号，直到执行开中断指令之后才会恢复检查。这样，关中断、开中断之间的这些指令序列就是不可被中断的，这就实现了“原子性”**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa1a3c796bad24f71943c8759640358ee.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

# 二：进程控制原语

## （1）进程创建

### A：概述

**进程创建：一个进程创建另一个进程，此时创建者称为父进程，被创建的进程称之为子进程**

- 子进程可以**继承父进程所拥有的资源**
- 子进程撤销时，应将其从父进程哪里获得的资源**还给父进程**

**创建新进程的过程如下**

1.  **申请空白PCB**：为新进程分配一个唯一的进程标识号，并申请一个空白的PCB，若PCB申请失败，则进程创建失败
2.  **为新进程分配所需资源**：为新进程的程序和数据及用户栈分配必要的内存空间，**注意若资源不足，进入阻塞态等待资源**
3.  **初始化PCB**：主要包括初始化标志信息、初始化处理机状态信息和初始化处理机控制信息，以及设置进程的优先级等等
4.  **将PCB插入就绪队列**：若进程就绪队列能够接纳新进程，则将新进程插入就绪队列，等待被调度运行

**可以引起进程创建的事件有**

- **用户登录**：分时系统中，用户登录成功，系统会为其建立一个新的进程。比如Linux中的`bash`
- **作业调度**：多道批处理系统中，有新的作业放入内存时，会为其建立一个新的进程
- **提供服务**：用户向操作系统提出某些请求时，会建立一个进程处理该请求
- **应用请求**：由用户进程主动请求创建一个子进程。比如Linux中的系统调用`fork`

### B：补充-Linux中的创建进程操作

- 上面所叙述的均是概述，并没有针对特定的操作系统，因此这里以Linux为例，展示一下在Linux中创建进程等操作，加深同学们的理解。如果想要了解更多请移步：[Linux系统编程10：进程入门之系统编程中最重要的概念之进程\&\&进程的相关操作\&\&使用fork创建进程](https://blog.csdn.net/qq_39183034/article/details/116188883)

**fork\(\)函数：是用来创建进程的，在fork函数执行后，如果成功创建新进程就会出现两个进程，一个是子进程，一个是父进程，fork函数有两个返回值**

#### ①：fork\(\)

**演示一**

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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030110312350.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
根据上面程序的运行效果，似乎可以发现下面比较值得注意的几点

- 它们的逻辑关系有些特点  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301103737803.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)
- 从上面的动图可以发现，fork\(\)函数调用完成之后，**它们似乎是同时输出的，这是否告诉我们这两个进程是同时进行的？** 或许它可以被画成这样？  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301104540762.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

**演示二**

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

效果如下![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301150834191.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
**演示三：**

为了方便演示，修改上述代码如下，为每个if语句块内加入死循环，使其能不断输出

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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301151157467.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
同时再使用之前的命令查看这个进程，发现也是两个进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301151219424.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
但是这里有一个很大的问题：**我们知道，不论是哪种编程语言，`if-else`执行时每次只能执行一路，怎么可能同时执行多路，同时每个if语句块内都有死循环，一个循环未结束，又怎么可能去执行其他语句呢**

**其实在Linux中，进程创建会形成链表，父进程创建子进程，那么父进程的进程指针会指向子进程ID。所以这两个进程是同时运行的**

#### ②：fork\(\)相关问题

**何理解进程创建**

前面说过，操作系统在进行管理时，必然遵循**“先描述，再组织”**的原则，所以在进行进程管理时。首先会创建相应的`task_struct`，写入有关信息，然后和你编写好的代码共同组成进程

**为什么fork有两个返回值**

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

**执行完fork之后，父进程pid不等于0，子进程pid等于0。这两个进程都是独立的，存在于不同地址中，不是公用的。fork把进程当前的情况进行拷贝，执行fork时，fork只拷贝下一个要执行的代码到新的进程**

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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210301155447518.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

**为什么两个返回值不一样**

其实很好理解，**创建进程时相当于形成了链表（Linux）中，父进程指向子进程，所以返回的是子进程的ID，而子进程没有它的子进程，所以返回0。**

再者从现实生活中理解，一个孩子肯定知道它只有一个爹，而一个爹可能有多个孩子，所以子进程在标识父进程时就不要做那么多的区分，但是父进程可能有多个子进程，它与它在区分不同的子进程时必须要使用PID

**为什么代码是共享的，数据是私有的**

首先代码是不可修改的（还记得代码段，数据段吗？），还有下面的常量字符串其实反映的也是这个道理

```c
const char* str1="Hello World";
const char* str2="Hello World";
//str1和str2地址相同
```

对于数据来说，如果数据不私有，造成的后果就是同一份数据在父子进程之间改来改去引起混乱

## （2）进程终止

### A：概述

**操作系统终止进程的过程如下：**

1.  根据被终止进程的标识符，检索PCB，**从中读出该进程的状态**
2.  若被终止进程处于执行状态，**立即终止该进程的执行**，将处理机资源分配给其他进程
3.  若该进程还有子孙进程，则应将其所有**子孙进程终止**
4.  将该进程所有用的全部资源**归还给操作系统或其父进程**
5.  将其PCB从所在队列中**删除**

**引起进程终止的事件主要有：**

- **正常结束**：表示进程的任务已经完成并准备退出运行
- **异常结束**：表示进程在运行时，发生了某种异常事件，使程序无法继续运行。比如存储区越界、保护错误、非法指令、特权指令错误、运行超时、浮点异常等等
- **外界干预**：指进程相应外界的请求而终止运行。比如操作员或操作系统干预、父进程请求或父进程终止

### B：补充-僵尸进程与孤儿进程

- 上面所叙述的均是概述，并没有针对特定的操作系统，因此这里以Linux为例，展示一下在Linux中由于进程终止而产生的一些现象，加深同学们的理解。如果想要了解更多请移步：[Linux系统编程10：进程入门之系统编程中最重要的概念之进程\&\&进程的相关操作\&\&使用fork创建进程](https://blog.csdn.net/qq_39183034/article/details/116188883)

#### ①：僵尸进程

简单点来说：**僵尸进程就是子进程已经退出了，父进程还在运行当中，父进程没有读取到子进程的状态，子进程就会进入僵尸状态**

使用下面的C语言程序模拟一个僵尸程序，子进程在10秒后利用exit退出，但是父进程一直在运行

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
   // printf("还没执行fork函数时的本进程为：%d\n",getpid());
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
      while(1)
      {
            
            
        printf("----------------------------------------------------\n");
        printf("父进程一直在运行\n");
        sleep(1);
      }
    }
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=0;
      while(count<=10)
      {
            
            
        printf("子进程已经运行了%d秒\n",count+=1);
        sleep(1);
      }
      exit(0);//让子进程运行10s
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}

```

效果如下，可以发现，在10s后，子进程已经退出，父进程还在运行  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302103348181.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

根据上面的定义，当子进程先退出，父进程还在运行，由于读取不到子进程的退出状态，所以子进程会变为僵尸状态。为了方便演示，使用下面的脚本，来每1s监控进程

```bash
while :; do ps axj | head -1 && ps axj | grep a.out | grep -v grep;sleep 1;echo "###########";done
```

效果如下，**可以发现当子进程结束后，父进程还是在运行，此时进程太变为`Z`，也就是僵尸状态**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302104156604.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
**那么为什么有僵尸进程呢？**

其实道理也很简单，子进程是由父进程创建的，父进程之所以要创建子进程，其目的就是要给子进程分配任务，那么在这个过程中，子进程平白无故的没了，而父进程却不知道子进程到底把自己交给它的任务完成的怎么样，成功了还好，失败的话就能再交代一个进程去操作。  
**所以进程结束时一定要给父进程返回一个状态，父进程一直不读取这个状态的话，那么子进程就会一直卡在僵尸状态，其中像代码这些资源已经被释放，但是这个进程却没有真正退出，因为PCB还在维护它，直到父进程读取到它的状态，才能进入死亡状态**

- 进程控制块中，一个进程退出后，还有一个退出码返回给父进程，如下是Linux内核中关于这部分的定义  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302135757793.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)
- 在Linux中一行命令就是一个进程，那么这个命令的父进程是`bash`，那么命令在结束的一瞬间也会给`bash`返回一个状态码，`bash`作为父进程，就是依靠这个返回码来判断命令是否正常结束，如果状态码为某一个值即可判定为没有这样的命令。在Linux中可以用`echo $?`来查看上一个输入命令的状态返回码，命令正确返回0，否则返回非0  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302140144386.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

#### ②：孤儿进程

**孤儿进程就是父进程没了，子进程还在**。那么根据上面的僵尸进程，子进程在退出后由于没有父进程来读取它的状态，所以会一直卡在僵尸状态，那么这样就会存在一个问题，它的内存资源谁来回收，通俗点将就会造成 **内存泄漏**

修改上面的代码，让父进程先挂，如下

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
   // printf("还没执行fork函数时的本进程为：%d\n",getpid());
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
      int cout=0;
      while(cout<10)
      {
            
            
        printf("----------------------------------------------------\n");
        printf("父进程运行了%d秒\n",cout+=1);
        sleep(1);
      }
      exit(0);//让父进程挂了
    }
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=0;
      while(1)
      {
            
            
        printf("子进程已经运行了%d秒\n",count+=1);
        sleep(1);
      }
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}

```

效果如下，这里还有一个现象就是，当父进程挂了之后，子进程一直在运行，并且`ctrl+C`，无法结束进程。这是因为`ctrl+C`此时结束的是父进程，但是父进程早已结束，子进程像孤儿一样四处游荡  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302143013530.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
除非使用killl才能将其杀掉

那么问题来了，这个进程难道一直要占用资源吗，其实操作系统在设计的时候就考虑到了这一步。**所以一旦父进程先挂了，那么这个子进程就会被1号进程领养**

依然使用下面脚本进行观察

```bash
while :; do ps axj | head -1 && ps axj | grep a.out | grep -v grep;sleep 1;echo "###########";done
```

效果如下，可以发现，当父进程挂了，这个进程的ppid，也就是父进程更换为了1号进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302143628983.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
1号进程是什么呢，其实就是systemd  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210302143718839.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

## （3）进程阻塞\(Block\)/等待\(Wait\)

### A：概述

**进程阻塞执行过程如下**

1.  找到将要被阻塞进程的**对应的PCB**
2.  若该进程为运行态，则**需要保护其现场**，然后将其状态转换为阻塞态，停止运行
3.  把PCB**插入相应事件的等待队列**，将处理机资源调度给**其他就绪进程**

**引起进程阻塞的事件有**

- 需要等待系统**分配某种资源**
- 需要等待**相互合作的其他进程完成工作**

### B：补充-Linux中的进程等待

- 上面所叙述的均是概述，并没有针对特定的操作系统，因此这里以Linux为例，展示一下Linux中的进程等待现象，加深同学们的理解。如果想要了解更多请移步：[Linux系统编程17：进程控制之进程等待\&\&为什么进程需要被等待\&wait方法和waitpid方法\&\&阻塞和非阻塞等待](https://blog.csdn.net/qq_39183034/article/details/116189479)

#### ①：为什么进程需要被等待/阻塞

**前面的例子中说过，子进程退出后就变成了僵尸状态，一旦变成僵尸状态，这个子进程就如同僵尸一样，杀也杀不死（因为它已经死了），所以它必须需要让父进程读取到它的状态，回收子进程信息。只有这样，子进程才能得到“救赎”，“魂魄”才能归天，这属于进程需要被等待的一个典型例子**

#### ②：进程阻塞式等待

在上面的僵尸进程的例子中，修改代码子进程在10s后退出，父进程在10s后继续运行，运行至第15s，跳出循环，加入语句`wailt(NULL)`以回收子进程

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
   // printf("还没执行fork函数时的本进程为：%d\n",getpid());
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
	  int count=1;
      while(count<=15)
      {
            
            
        printf("----------------------------------------------------\n");
        printf("父进程运行了%d秒\n",count);
		count++;
        sleep(1);
      }
	  wait(NULL);//回收子进程
    }
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程已经运行了%d秒\n",count);
		count++;
        sleep(1);
      }
      exit(0);//让子进程运行10s
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

如下可以发现，当父进程将子进程回收后，僵尸进程也消失了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315124044286.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)  
如果父进程里只写上`wait(NULL)`，那么就表示父进程阻塞在这里，等着子进程死亡，回收子进程

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
   // printf("还没执行fork函数时的本进程为：%d\n",getpid());
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		wait(NULL);//进程阻塞
		printf("子进程已经死亡，父进程退出\n");
		exit(0);
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程已经运行了%d秒\n",count);
		count++;
        sleep(1);
      }
      exit(0);//让子进程运行10s
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315125226389.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120950373)

#### ③：进程非阻塞式等待

- 这一部分需要用到大量Linux基础知识，如有兴趣，可移步进行系统学：[Linux系统编程17：进程控制之进程等待\&\&为什么进程需要被等待\&wait方法和waitpid方法\&\&阻塞和非阻塞等待](https://blog.csdn.net/qq_39183034/article/details/116189479)

## （4）进程唤醒（Wake）

**唤醒进程的执行过程如下**

1.  在该事件的等待队列中找到**相应进程的PCB**
2.  将其从**等待队列中移除**，并置其状态为就绪态
3.  把该PCB插入就绪队列，**等待调度程序调度**

**注意：Block和Wakeup作用刚好相反，必须成对使用**

- **Block**：是由被阻塞进程自我调用实现的
- **Wakeup**：是由一个与被唤醒进程合作或其他相关进程调用实现的

## （5）进程切换

**进程切换的过程如下**

1.  保存处理机上下文，包括程序计数器和其他寄存器
2.  更新PCB信息
3.  把进程的PCB移入相应的队列，如就绪、在某事件阻塞等队列
4.  选择另一个程序进程执行，并更新其PCB
5.  更新内存管理的数据结构
6.  恢复处理机上下文

**引起进程切换的事件有：**

- 当前进程**时间片已到**
- 有**更高优先级**的进程到达
- 当前进程**主动阻塞**
- 当前进程**终止**