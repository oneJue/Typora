 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）可重入函数](#1_6)
  - [（2）volatile关键字](#2volatile_15)
  - - [A：背景知识](#A_16)
    - [B：产生的问题](#B_19)
    - [C：volatile关键字](#Cvolatile_56)
  - [（3）SIGHLD信号](#3SIGHLD_84)
  - - [A：复习僵尸进程](#A_85)
    - [B：清理僵尸状态的新方法-SIGCHLD](#BSIGCHLD_130)

## （1）可重入函数

如下是一个不带头结点的单链表的头插操作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413134248518.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)

- 上述过程是这样的：main函数调用insert函数向一个链表head中插入节点node1，插入操作分为两步，刚做完第一步的时候，因为硬件终端使进程切换到内核态，再次返回用户态时做信号检测，由于是自定义用户操作，所以切换到sighandler函数，而sinhanderl函数也调用了insert函数向同一个链表中插入了结点node2，插入操作的两步都做完之后从sighandler返回至内核态，接着再次返回用户态就从main函数调用的insert函数中继续向下执行，完成了之前由于中断而未完成的第二步。结果就是最终只有一个有效的结点插入链表中了，剩余的一个结点由于没有指针指向，造成了内存泄漏

**像上面这样，iinsert函数被不同的控制流程调用，有可能在第一次调用还没有返回的时候就再次进入了该函数，我们称之为重入**。很明显，insert函数是不可以被重入的，因为会造成混乱，所以像insert这样的函数称之为不可重入函数。

## （2）volatile关键字

### A：背景知识

由于内存的访问速度远不及CPU的处理速度，所以为了提高机器的整体性能，在硬件上引入了高速缓冲Cache，加速对内存的访问。**除了硬件级别的优化外，编译器也通常会进行优化，比如说将内存变量缓冲到寄存器或调整指令顺序充分利用CPU指令流水线**

### B：产生的问题

其实这样的优化会产生一定的问题，因为访问寄存器要比访问内存单元快的多，所以编译器一般都会作减少存取内存的优化，也就是读取数据时更加倾向于读取寄存器中的数据，这就有可能读取到**脏数据**

结合前面信号所讲过的相关知识可以做一个案例：

在下面的这段代码中，首先定义一个全局变量`flag`并初始化为0，接着在main函数中捕捉2号信号，一旦发送2号信号就执行自定义的捕捉动作——将`flag`设置为1，`signal`函数下面是一个`while`循环，在默认状态下，`while`将会不断检测`flag`的值，如果此时编译器不做任何优化，那么`flag`默认处于内存中，那么`while`将会不断读取内存中`flag`的值做判断。

```c
#include <stdio.h>
#include <signal.h>

int flag=0;

void handler(int sig)
{
            
            
  flag=1;
  printf("flag被设置为了1\n");

}
int main()
{
            
            
  signal(2,handler);

  while(!flag);
  printf("程序运行到了这里\n");


}

```

效果如下，由于发送了2号信号，于是`flag`被设置为1，因此`while`循环没有成为死循环，最后的一条语句也被成功打印了，这的确是我们预料的结果  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413144122673.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)  
**上述编译过程中编译器其实没有做优化，而gcc其实可以携带一定的优化选项，也就是`-O`选项，分别是`-O0`，`-O1`，`-O2`，`-O3`和`-Os`。所以这一次我们让编译器进行优化，因为前面我们说过编译器一旦进行优化，将会把变量放在寄存器里面**

于是效果如下，大家可以发现这里似乎产生了一定的矛盾：`flag`的值已经被设置为了1，按理说`while`循环一旦对`flag`取反，肯定是不会产生死循环的，但实际情况是它还在死循环当中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413144723879.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)

### C：volatile关键字

其实上面矛盾的现象反映也正是volatile的关键字，**volatile将保持内存的关键字，一个变量一旦被volatile修饰，那么系统总是会从内存中读取数据，而不是从寄存器** 。**因此刚才那个例子中，由于编译器做了一定的优化，将`flag`放置到了寄存器中，同时因为这是两个不同的执行流程，而`handler`函数修改时修改的仍然是内存中的那个数据，这样就导致内存中的`flag`已经为1了，但是`while`循环读取的确实寄存器中的数据，而寄存器里的`flag`值仍然是0，因此会造成死循环**

所以既然编译器已经做了优化，而我们又要达到正常的效果，就可以使用volatile修饰`flag`，这样`while`读取`flag`时将会被强制从内存中读取

```c
#include <stdio.h>
#include <signal.h>

volatile int flag=0;

void handler(int sig)
{
            
            
  flag=1;
  printf("flag被设置为了1\n");

}
int main()
{
            
            
  signal(2,handler);

  while(!flag);
  printf("程序运行到了这里\n");


}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413145600445.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)

## （3）SIGHLD信号

### A：复习僵尸进程

在[linux进程](https://blog.csdn.net/qq_39183034/article/details/114284873)那一节我们提及了僵尸进程这个概念。僵尸进程就是子进程已经退出了，父进程还在运行当中，由于父进程没有读取到子进程的状态，所以子进程就会进入僵尸状态

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

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413151720234.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)

### B：清理僵尸状态的新方法-SIGCHLD

> 在[Linux进程控制](https://blog.csdn.net/qq_39183034/article/details/114799947)那一节我们讲了可以使用wait和waitpid清理僵尸进程。而且父进程可以以阻塞或非阻塞的方式等待子进程结束，但是这两种方式都有一个很大的缺点就是：父进程除了忙于自己的工作外，还要时不时的关心子进程怎么样了，尤其在子进程的返回状态不是那么重要的情况下，这样的操作就显得有点多余了

**其实，子进程在终止时会给父进程发送SIGCHLD信号，该信号的默认动作是忽略，当然父进程也可以自定义SIGCHLD信号的处理函数，这样父进程就只需要关心自己的工作，当子进程终止时，会向父进程发送信号，而父进程则利用信号捕捉自动处理子进程**

于是上面的案例中在父进程中加入

```cpp
signal(SIGCHLD,SIG_IGN);//忽略
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210413153226855.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115578577)