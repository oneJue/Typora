 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）用户态和内核态](#1_4)
  - [（2）用户态和内核态的切换](#2_36)
  - [（3）内核是如何实现信号的捕捉](#3_44)
  - [（4）sigaction](#4sigaction_79)

## （1）用户态和内核态

我们说过，每个Linux进程有4GB的地址空间  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412211037138.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277719)  
其中0-3G是用户空间，由用户页表负责映射到物理内存，剩余的1G存放的是内核及其维护的数据，由内核页表负责映射。

一个非常简单的C语言程序如下

```c
#include <stdio.h>
int main()
{
            
            
	int a=10;
	printf("%d\n",a);
	return 0;
}
```

这段程序中有一个printf函数作用是向屏幕打印内容，根据对操作系统理解，我们知道printf底层肯定是调用了系统调用接口write完成了对应的功能  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412211602822.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277719)  
也就是说这段程序中像`int a=10`这种语句是属于进程的，也就是属于用户的，而执行printf的时候却需要深入到内核完成。

**内核态**

当一个进程执行系统调用（比如上面的printf，本质是系统调用）而**陷入**内核代码中运行时，我们就称为该进程处于内核运行状态。

**用户态**

当进程执行用户自己的代码时（比如上面的`int a=10`），则称其处于用户状态

**用户态核内核态下工作的程序有很多的差别，但是最主要的差别就在于其权限的不同，运行在内核态下的程序可以直接方位用户态的代码和数据（但是禁止这样做）**

所以这样的一个简单的程序反映的却是进程在用户态和内核态中的不断切换的过程

## （2）用户态和内核态的切换

当在系统中执行一个程序时，大部分时间都是运行在用户态下的，在需要操作系统帮助完成一些用户态没有能力完成的操作时就会切换到内核态（比如printf函数）  
用户态切换到内核态主要有三种方式

- **系统调用**：除了前面的printf外，我们之前说过的fork\(\)函数也是一个典型的例子
- **异常**：当CPU在执行运行在用户态下的程序时，，发生了一些没有预知的异常，这时会触发由当前运行进程切换到处理此异常的内核相关进程中
- **外围设备的终端**：这个其实就是咋们上面说过的信号

## （3）内核是如何实现信号的捕捉

**信号是什么时候处理，以及什么时候被捕捉的呢？是在从内核态切换到用户态的时候**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412214628959.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277719)

1.  首先在用户态的程序由于中断，系统调用或者是异常将会进入内核态
2.  进入内核态，处理完需求，准备返回用户态
3.  **在返回用户态之前检测信号。如果没有信号继续返回；如果有信号，但是被阻塞了，由于无法处理，所以也返回；最后就是有信号，但是没有被阻塞，那么信号就应该递达。当递达后如果是默认动作，那么一般就是终止进程，而此时在内核态它是有权利终止进程的，如果是忽略，那么不要忘记将pending对应位置置为0，最后一种就是自定义的函数了**
4.  如果是自定义函数，将会再次返回用户态，处理完成之后，借助相关系统调用再次进入内核
5.  最后再通过返回到上次中断的地方继续向下执行

所以上述过程可以用下面的这一张图记忆  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412220906984.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277719)  
**如果信号的处理动作是用户自定义函数，所以内核决定返回用户态时就会去指定自定义函数，而这个自定义函数和原先的main函数使用的是不同的堆栈空间，所以他们之间不存在调用和被调用的关系，是两个独立的控制流程，而一个进程可以有多个控制流程——线程**

至此我们可以解释上面[Core Dump](#1)中为什么下标已经越界了，但是后面的代码还是可以输出

```c
int main()
{
            
            
	int arr[10]={
            
            0};
	for(int i=0;i<=100;i++)
	{
            
            
		arr[i]=i;
	}
	printf("运行到了这里\n");
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413084723678.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277719)  
**这是因为信号是在从内核态切换到用户态的时候处理的，所以上面的for循环的代码始终运行在用户态，当涉及到printf的时候，将会陷入内核态完成调用，然后返回时检测信号，此时检测到了11号信号，所以进行处理**

## （4）sigaction

`sigaction`这个函数和`signal`作用基本一致，但是前者功能要比后者丰富一点

```c
#include <signal.h>
int sigaction(int signo,const struct sigaction* act,struct sigaction* oact);
```

其中`act`和`oact`分贝是`struct sigaction`的结构体指针，`act`表示捕捉后你做的操作，`oact`是备份一下捕捉前的操作  
其中`struct sigaction`结构体如下

```c
struct sigaction
{
            
            
	void (*sa_handler)(int);//捕捉后你要做的操作
	void (*sa_sigaction)(int,siginfo*,void*);//处理实时信号
	sigset_t sa_mask;//设置为0
	int sa_flags;//设置为0
	void (*sa_restorer)(void)//处理实时信号

}

```

- sa\_mask的作用：**大家可以想象这样一个场景，上节展示的捕捉过程中，如果处理完2号信号的自定义捕捉动作时，再来了一个2号信号怎么办，这样就会导致无法从内核态返回至用户态，会造成很严重的后果，于是sa\_mask也是一个信号集，表示当你在处理某个信号时想要把哪些信号加入屏蔽。在默认状态下，当前处理的信号是默认加入进去的，也就是操作系统可以处理很多信号的自定义捕捉动作，但是每一个信号的自定义捕捉动作一次只能处理一个**