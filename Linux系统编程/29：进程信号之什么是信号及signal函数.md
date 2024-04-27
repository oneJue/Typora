 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）CTRL+C](#1CTRLC_5)
  - [（2）注意](#2_9)
  - [（3）信号列表](#3_16)
  - [（4）处理信号-signal函数](#4signal_21)

## （1）CTRL+C

**我们都知道，按下`ctrl+c`后将会结束当前运行的一个前台进程**，当我们按下ctrl+c后，会被操作系统获取，而这个动作或者这个快捷键组合已经被赋予了结束进程的含义（就像你从小就被告知看见红灯就要停下来），**它被解释为信号，发送给目标前台进程，而进程由于收到了信号所以退出**  
其中2号信号KILLINT发挥的作用就是`ctrl+c`，如下可以一个进程不断地向屏幕打印文字，然后使用2号信号结束  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410160223380.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277561)

## （2）注意

1.  **`ctrl+c`产生的信号只能发送给前台进程，一个命令后面加入`&`可以将进程放在后台运行**，这样shell就不必等待进程结束就可以接受新的命令从而启动新的进程
2.  **shell可以同时运行一个前台进程和多个后台进程，但是只有前台进程才能接收到类似于`ctrl+c`这样的控制键产生的信号**
3.  前台进程在运行过程中用户随时可能按下`ctrl+c`而产生一个信号，也就是说该进程的用户空间代码执行到任何地方都有可能受到`SIGINT`信号而终止，**所以信号相对于进程的控制流是异步的**

## （3）信号列表

使用kill \-l命令可以查看系统定义的信号列表  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410161200361.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277561)  
**每一个信号都有一个编号和一个宏定义名称，编号34及以上的属于实时信号，我们在这里只讨论的是1-31的非实时信号**

## （4）处理信号-signal函数

进程获得一个信号后，有如下三种处理方式

1.  **忽略此信号（`SIGIGN`）**
2.  **执行该信号的默认动作\(`SIG_DFL`\)**
3.  **提供一个信号处理函数，要求内核在处理该信号时切换到用户态执行这个处理函数，这种方式称为捕捉一个信号**

signal函数是用来让进程处理信号的，它的函数原型是（大家先不要管它的返回值）

```c
sighandler_t signal(int signum,sighandler_t handler)
```

其中第一个参数就是我们要处理的信号（可以输入编号或者名称）；第二个参数就是我们处理的方式，处理的方式分别就对应上面的三条

**举例1**：`ctrl+c`对应的是2号信号，如果使用signal函数，第一个参数设置为2，表示处理2号信号，第二个参数设置为`SIG_IGN`，然后在`signal`函数后面循环打印文字

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

int main()
{
            
            
  signal(2,SIG_IGN);//忽略此信号 
  while(1)
  {
            
            
    printf("I'm running now!\n");
    sleep(1);
  }
  return 0;

}
```

由于我们动作是忽略2号信号，所以大家可以发现即便我疯狂的按下`ctrl+c`，也无法终端进程（按下`ctrl+\`结束进程）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412134917518.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277561)

**举例2：** 如果将第二个参数设置为`SIG_DFL`呢，那么它就表示默认，这个就相当于不写这个函数一样，所以按下`Ctrl+c`就能正常终止，这里就不演示了

**举例3：** 除了前两种处理方式外，我们说过第三种方式就是用户自定义一个函数，称这种方式为捕捉到一个信号，当捕捉到信号后，进程就会执行你所定义的函数的内容（所以第二个参数是一个函数指针）

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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210410170754983.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277561)

在这里我们就可以具体说一说这个函数的形参，返回值了，因为它的真正的原型是这样子的，是不是感觉很懵？

```c
typedef void(*sighandler_t)(int);
sighandler_t signal(int signum,sighandler_t handler);
```

我们首先要明白的就是这里`typedef`的作用，typedef的作用实则就是定义一个新类型，比如说`typedef int Myint`，这样的话我定义整形就可以用`Myint a=10`了，那么在这里先不要看typedef，先看后面的，void\(\*sighandler\)\(int\)这个明显是一个函数指针，sighandler指向一个一个形参为int返回值为void的函数，当加上typedef后，sighandler\_t就是一个新的类型，就可以像int一样用它，只不过int声明的是一个整形变量，而sighandler声明的是一个函数指针，而且这个函数指针指向的函数接收一个整型参数并返回一个void。

你可能会问，这样的写法有什么作用呢？其实如果不这样写，那么你看见的signal函数将会是下面的这样子

```c
void (*signal(int signum,void (*handler)(int)))(int);
```

是不是感觉更加不懂了。所以现在我们去剖析一下这个函数，想要弄清楚，你必须明白下面的两个简单的例子

- `void (*p)(int)`：这个肯定很简单，自然是一个函数指针p，p指向的函数是一个形参为int，且返回值为void的函数
- `void (*fun())(int)`：这里，fun是一个函数，所以可以看成是fun这个函数执行完毕之后，它的返回值是一个函数指针，指向了一个形参为int，返回值为void的函数

了解完毕后，就可以结合上面的例子解释一下它的运行过程了

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210412143313204.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116277561)