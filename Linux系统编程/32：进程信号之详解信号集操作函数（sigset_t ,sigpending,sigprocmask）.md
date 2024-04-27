 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）sigset\_t](#1sigset_t_6)
  - [（2）信号集操作函数](#2_11)

## （1）sigset\_t

前面说过，未决和阻塞分别用位图来表示，于是我们把保存位图这样的数据类型称为`sigset_t`，`sigset_t`称为信号集，于是他们分别称为**阻塞信号集和未决信号集**

`sigset_t`这种类型可以表示每个信号的有效和无效的状态（**阻塞信号集的有效和无效的含义是该信号是否被阻塞，未决信号集则是该信号是否处于未决状态**），其中阻塞信号集也叫做当前进程的信号屏蔽字（`SignaL Mask`）

## （2）信号集操作函数

sigset既然是一个保存位图的数据类型，那么是否直接修改它对应数据的比特位就能达到屏蔽信号，产生信号的目的呢？答案是可以的，但是由于这个类型内部如何存储这些位图要依赖于系统实现，简单来说不同平台的存储方式是不一样的，所以我们不能直接操作比特位，**我们只能调用一下函数来操作`sigset_t`变量**

（**注意以下函数仅在操作变量，它并没有深入到内核中改变对应的位图，就像ftok函数生成key的作用一样**）

```c
#include <signal.h>
int sigemptyset(sigset_t* set);
int sigfillset(sigset_t* set);
//注意，使用sigset_t类型的变量前，一定使用他们其中的一个做初始化，
//让信号集处于一种确定的状态
int sigaddset(sigset_t* set,int signo)
int sigdelset(sigset_t* set,int signo)
int sigismember(const sigset_t* set,int signo)

int sigpending(sigset_t* set);
```

- `sigemptyset`是初始化set所指向的信号集，所有比特位清零
- `sigfillset`是初始化set所指向的信号集，所有比特位置为1
- `sigaddset`是对set所指信号集添加信号signo
- `sigdelset`是从set所指信号集中删除信号signo
- `sigismember`用于判断一个信号集的有效信号是否包含signo这个信号
- `sigpending`用于读取当前进程的未决信号集

所以结合上述例子，我们就可以写下面这样一段小程序，查看一下未决信号集，

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>


void print_pending(sigset_t* pending)
{
            
            
  int i=1;
  for(i=1;i<=31;i++)
  {
            
            
    if(sigismember(pending,i))
    {
            
            
      printf("1");//只要i信号存在，就打印1
    }
    else 
    {
            
            
      printf("0");//不存在这个信号就打印0
    }
  }
  printf("\n");
}

int main()
{
            
            
  sigset_t pending;//定义信号集变量
  while(1)
  {
            
            
    sigemptyset(&pending);//初始化信号集
    sigpending(&pending);//读取未决信号集，传入pending
    print_pending(&pending);//定义一个函数，打印未决信号集
    sleep(1);
  }
}

```

运行效果如下，由于没有传入信号，所以未决信号集全部为0  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412161719503.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277684)  
那么下一个问题就是用户如何去控制阻塞信号集（信号屏蔽字），为什么不控制未决信号集呢？因为产生信号无非就是咋们上面说过的4种方式之一嘛。  
**前面说过你不能直接操作比特位，所以我们要使用到一个函数：`sigprocmask`**

```c
#include <signal.h>
int sigprocmask(int how,const sigset_t* set,sigset_t* oset);
```

参数how指示了如何更改，当选定参数how后，set的意思如下

| how | set |
| --- | --- |
| SIG\_BLOCK | set包含了我们希望添加到当前信号屏蔽字的信号 |
| SIG\_UNBLOCK | set包含了我们希望添加到当前信号屏蔽字中解除阻塞的信号 |
| **SIG\_SETMASK（常用）** | 设置当前信号屏蔽字为set所指的值 |

参数oset如果不设置为NULL，由于此函数会更改当前的信号屏蔽字，所以会在更改之前将此时的信号屏蔽字备份一份到oset所指的变量中

所以现在在上面的例子的基础上，添加`block`和`oblock`变量，再使用`sigaddset`函数添加2号信号11号信号，此时如果发送2号信号和11号信号，就会将信号添加到阻塞信号集也就是block中，然后使用`sigprocmask`将屏蔽关键字设置为`block`的值，并将原先的屏蔽关键字备份到`oblock`中。

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>


void print_pending(sigset_t* pending)
{
            
            
  int i=1;
  for(i=1;i<=31;i++)
  {
            
            
    if(sigismember(pending,i))
    {
            
            
      printf("1");//只要i信号存在，就打印1
    }
    else 
    {
            
            
      printf("0");//不存在这个信号就打印0
    }
  }
  printf("\n");
}

int main()
{
            
            
  sigset_t pending;//定义信号集变量

  sigset_t block,oblock;//定义阻塞信号集变量
  sigemptyset(&block);
  sigemptyset(&oblock);//初始化阻塞信号集

  sigaddset(&block,2);//将2号信号添加的信号集
  sigaddset(&block,11);

  sigprocmask(SIG_SETMASK,&block,&oblock);//设置屏蔽关键字

  while(1)
  {
            
            
    sigemptyset(&pending);//初始化信号集
    sigpending(&pending);//读取未决信号集，传入pending
    print_pending(&pending);//定义一个函数，打印未决信号集
    sleep(1);
  }
}

```

效果如下，大家可以发现当我发送2号信号时（之前发送2号信号进程立即终止，是因为没有阻塞它）而现在发送2号信号大家可以看到未决信号集对应的第2号位置变为了1，进程也没有终止，因为此时在阻塞状态，没有递达  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412164653522.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277684)

可以发现由于信号一直被阻塞，没有递达，所以本该结束进程的命令也将“失效”。要使其生效，就必须接触阻塞状态，所以再次修改上面的案例，由于之前用oblock备份了屏蔽关键字（都是0），所以在10s之后，用该函数再次设置屏蔽关键字为oblock，以此接触阻塞，由于接触阻塞后，进程会终止，所以这里用`signal`捕捉信号，以免结束进程，便于观察

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

void handler(int sig)
{
            
            
  printf("获得信号:%d\n",sig);

}

void print_pending(sigset_t* pending)
{
            
            
  int i=1;
  for(i=1;i<=31;i++)
  {
            
            
    if(sigismember(pending,i))
    {
            
            
      printf("1");//只要i信号存在，就打印1
    }
    else 
    {
            
            
      printf("0");//不存在这个信号就打印0
    }
  }
  printf("\n");
}

int main()
{
            
            
  signal(2,handler);//捕捉

  sigset_t pending;//定义信号集变量

  sigset_t block,oblock;//定义阻塞信号集变量
  sigemptyset(&block);
  sigemptyset(&oblock);//初始化阻塞信号集

  sigaddset(&block,2);//将2号信号添加的信号集

  sigprocmask(SIG_SETMASK,&block,&oblock);//设置屏蔽关键字

  int cout=0; 
  while(1)
  {
            
            
    sigemptyset(&pending);//初始化信号集
    sigpending(&pending);//读取未决信号集，传入pending
    print_pending(&pending);//定义一个函数，打印未决信号集
    sleep(1);
    cout++;
    if(cout==10)//10s后解除阻塞
    {
            
            
      printf("解除阻塞\n");
      sigprocmask(SIG_SETMASK,&oblock,NULL);
    }

  }
}

```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412165940581.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277684)