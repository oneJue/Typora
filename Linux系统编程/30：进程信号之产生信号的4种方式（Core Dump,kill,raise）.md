 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）通过按键产生信号-Core Dump](#1Core_Dump_36)
  - [（2）调用系统函数向进程发送信号](#2_67)
  - - [A：kill](#Akill_68)
    - [B：raise](#Braise_76)
    - [C：abort](#Cabort_105)
  - [（3）由软件条件产生信号](#3_140)
  - [（4）硬件异常产生信号](#4_177)
  - [总结：](#_208)

  
为了方便后面的讲解，我们先要了解一下如何捕捉信号， **使用`signal`函数可以捕捉到发送给这个进程的信号**，该函数第一个形参设定要捕捉的信号，第二个形参实则是一个函数指针，指向了一个函数，表明当捕捉到要捕捉的信号时，所要进行的操作

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

void handler(int sig)
{
            
            
  printf("catch a sin : %d\n",sig);

}

int main()
{
            
            
  signal(2,handler);//一旦捕捉到2号信号，将会执行handler函数内的操作
  while(1)
  {
            
            
    printf("I Am runnng now...\n");
    sleep(1);
  }
  return 0;

}

```

效果如下，大家可以看到，一旦信号被捕捉后，将会执行所指定函数内的内容，所以这里的2号信号被捕捉后也无法结束进程了。**其中9号进程无法被捕捉，因为9号进程一旦被捕捉，操作系统就无法终止一些恶意进程了**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410170754983.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)

## （1）通过按键产生信号-Core Dump

通过按键产生信号，这里就不多强调了。**本节重点介绍一下Core Dump**

Core Dump是什么？在VS中如果我们写上这样的代码，那肯定会报出相应的错误的

```cpp
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

但是在Linux中不是图形化界面，如果要给出这样的报错信息的话，就需要到Core Dump。一旦程序异常终止后，就会在磁盘上转存一份以core开头的文件，后面跟上的数字是此进程的pid。  
如下，这个进程运行时出现了发生了段错误  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411164241640.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
但是这里没有相应的文件，是因为`core file size`被设置为了0（使用`ulimit \-a`查看）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021041116442687.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
可以使用ulimit \-c设置core的大小  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411164613440.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
再次运行后，出现了相应的文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411164653240.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
这个文件就记录了出错的信息，有了这样的文件，使用gdb调试的时候，键入`core-file` 【那个文件名】后，就可以列出十分详细的错误信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411165029180.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)

## （2）调用系统函数向进程发送信号

### A：kill

我们使用到的最多也就是kill 命令了，kill命令是调用kill函数实现的，k**ill函数可以给一个指定的进程发送指定的信号**

```c
#include <signal.h>
int kill(pid_t pid,int signo);
```

### B：raise

```c
#include <signal.h>
int raise(int signo);
```

raise函数可以给当前进程发送指定的信号

```c
int main(int argc,char* argv[])
{
            
            
  if(argc==2)//保证传入参数正确
  {
            
            
    raise(atoi(argv[1]));//将信号值传入
  }
  while(1)
  {
            
            
    printf("I Am runnng now...\n");
    sleep(1);
  }
  return 0;

}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410172200498.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)

### C：abort

abort函数使进程接收到信号而异常终止，和exit函数一样，abort函数总能被调用成功

```c
#include <stdlib.h>
void abort(void);
```

abort对应的信号就是SIGABRT，编号是6

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>
void handler(int sig)
{
            
            
  printf("catch a sin : %d\n",sig);

}

int main(int argc,char* argv[])
{
            
            
  signal(6,handler);//捕捉6号信号
  abort();//异常终止
  while(1)
  {
            
            
    printf("I Am runnng now...\n");
    sleep(1);
  }
  return 0;

}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410172921696.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)

## （3）由软件条件产生信号

前面咋们在讲管道的特性四时说到过：如果读端关闭文件描述符，那么写端就会被操作系统终结掉，因为操作系统发现此时的写端是一个无用，浪费资源的进程，并且发送的型号是SIGPIPE，这其实就是一种由软件条件产生的信号。这种类型信号不同于之前的硬件，系统调用接口那种方式，而是一种满足了一定条件就发出信号，就像水满了就溢出的感觉

今天主要介绍`alarm`函数和`SIGALRM`信号，它的作用是设定一个时间进行倒计时，当倒计时完成之后结束进程

比如下面的死循环中，首先设定alarm\(1\)，这表示进程进行1s就终止，然后再while循环中不断打印

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>

int cout=0;
void handler(int sig)
{
            
            
  printf("catch a sin : %d\n",sig);
  printf("cout:%d\n",cout);
  exit(1);

}

int main()
{
            
            
  alarm(1);//1s后结束
  while(1)
  {
            
            
    cout++;
    printf("cout:%d\n",cout);
  }
  return 0;

}

```

可以发现1s后，被SIGALRM信号给终止  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410180943419.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)

## （4）硬件异常产生信号

除0，空指针，野指针这些操作是不被允许的，所以操作系统一旦监测这样的进程，就会发送响应的信号终止  
比如说空指针

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>

void handler(int sig)
{
            
            
  printf("catch a sin : %d\n",sig);
  exit(1);

}

int main()
{
            
            
  signal(11,handler);
  sleep(1);
  int* p=NULL;
  *p=10;//错误

  return 0;
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410184608220.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
其实在C++中使用到的捕获异常，在系统层面，就是在处理信号

## 总结：

1：从上面的描述中大家可以看到，不管是哪一种产生信号的方式，背后总有一个角色在默默支撑着，**信号的发送依靠的是操作系统，因为操作系统是进程的管理者**

2：以core dump为例，大家可能也看到了，即便发生了段错误，但是后面的语句也被执行了，**这表明进程在接受到信号时并不一定会立即处理，中间可能会存在一定的空档期**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411171812563.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277581)  
3：操作系统给进程发送信号，其中发送二字显得尤为专业，总让感觉这一定是一个很复杂的过程。实则不然，与其说发送，倒不如用写入二字更为贴切。**每个进程都有其`task_struct`，所以可以在它里面搞一个位图，32位（就像文件系统中的`inode`和`bitmap`），一旦操作系统发送某个信号，就把某一位置为1，比如发送8号信号，就把它的第八位的0置1**