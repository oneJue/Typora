 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）为什么子进程需要被等待](#1_9)
  - [（2）等待进程的方法](#2_12)
  - - [A：wait方法](#Await_13)
    - [B：waitpid方法](#Bwaitpid_146)
    - [C：进程非阻塞式等待](#C_449)

  
前文说过，子进程被创建之后，父子进程究竟谁先运行是由调度器说了算。  
但是，谁先结束呢？一般来说肯定是要让子进程先结束，想一想我们的bash，bash是所有命令的父进程，你总不能让bash先挂吧  
**其实之所以让子进程先退出，是因为父进程容易对子进程进行管理，而且子进程被创建的原因就是因为父进程想要让子进程帮助自己完成一些业务，因此它要拿到结果。因此父进程一般需要等待子进程**

## （1）为什么子进程需要被等待

**既然子进程要先退出，那么子进程退出后就变成了僵尸状态，一旦变成僵尸状态，这个子进程就如同僵尸一样，杀也杀不死（因为它已经死了），所以它必须需要让父进程读取到它的状态，回收子进程信息。只有这样，子进程才能得到“救赎”，“魂魄”才能归天**

## （2）等待进程的方法

### A：wait方法

[这篇文章](https://blog.csdn.net/qq_39183034/article/details/114284873)中讲过一个例子，通过那个例子我们看到了子进程死去之后变为了僵尸状态

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

使用一个脚本动态查看

```bash
while :; do ps axj | head -1 && ps axj | grep a.out | grep -v grep;sleep 1;echo "###########";done

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315112029145.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
修改代码如下，子进程在10s后退出，父进程在10s后继续运行，运行至第15s，跳出循环，加入语句`wailt(NULL)`以回收子进程

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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315124044286.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
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
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315125226389.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)

### B：waitpid方法

**1：基本使用**

这是waitpid的函数原型：`pid_ t waitpid(pid_t pid, int *status, int options);` **其中形参`pid_t pid`就是需要等待的子进程的pid（如果设置为-1，表示等待任意一个子进程，其实就等同于了wait），当等待成功时，函数的返回值就是子进程的PID**

如下，修改上面的代码，使用`waitpid`等待子进程（`waitpid`后面的两个参数先用NULL和0），然后判断`waitpid`的返回值和字进程`PID`是否相等，如果相等，则输出“等待成功”，否则输出“等待失败”。

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
		pid_t rec=waitpid(ret,NULL,0);//等待子进程，阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			exit(0);
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
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

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315143400476.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
**2：重点理解第二个参数`int* status`**

首先大家可以发现这个参数比较特殊，需要注意的是这个参数`wait`也有，**它是一个指针，是一个输出型参数。在外面定义一个变量，然后把这个变量的地址传给`waitpid`（如果传入`NULL`表示不关心子进程的退出状态信息），然后操作系统会根据这个变量的值，将子进程的退出信息反馈给父进程**

这个status在理解的时候稍微有点难度。对于一个进程来说，如果不要管它结果对不对，**那么退出时只有两种状态：一种是正常退出（操作系统没有发送信号）另外一种就是异常退出（操作系统发送了终止信号）**  
**而这个status参数的目的就是为了判断你这个进程究竟是异常退出还是正常退出，如果是正常退出再看一下你的退出码是否是0，如果不是0，那么根据我业务的相关信息进行判断，报出相应的错误**

在32位环境中，int有4个字节，32个比特位，为了判断状态，只研究status的最后两个字节，也就是低16位。**在低16位中，前高8位保存子进程的退出状态，低八位记录接受到的信号值**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315153742570.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)

- 大家还记得`kill` 的参数列表吗，它的范围是1-64，你可以理解为不管是否正常退出操作系统都在发送信号，只不过正常时发送的是0，异常时发送的是非0  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315153849112.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)

如下程序，子进程设置其退出码为`exit(3)`

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
    pid_t ret=fork();
    sleep(1);
    if(ret>0)
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st;//st传地址进去
		pid_t rec=waitpid(ret,&st,0);//阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			printf("st是%d\n",st);//查看st结果
			exit(0);
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }
      exit(3);//让子进程运行10s
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

结果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315155515418.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
其结果为768，输出的并不是3。我们可以这样想象， **st用来判断：进程是否异常退出，如果正常退出结果又是否正确？** 所以其传入时低16位默认就将其设为`00000000 00000000`，也就是默认正常。上**面的例子中，并没有出现逻辑上的大问题，只是我的退出码是3，我想要说明这个进程是正常退出的只是结果有误**。前面说过，高8位是子进程的退出状态，而3的二进制是`11`，将其“贴合”在高八位上，就变成了`00000011 00000000`，而其结果正好就是768。

那么怎么输出真实的退出码呢，这其实就是C语言的知识了，只需把结果右移8，为了保证结果正确，再和0xff（11111111）进行与运算，也即是`st>>8&0xff`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315161016262.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
现在我们故意再次犯一个“浮点异常”的错误，代码如下

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
    pid_t ret=fork();
    sleep(1);
    if(ret>0)
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st;//st传地址进去
		pid_t rec=waitpid(ret,&st,0);//阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			printf("st是%d\n",st);//查看st结果
			exit(0);
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }
      int x=1/0;//浮点异常
      exit(0);
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}

```

结果如下，其状态返回码是8，而之前咋们说过`kill \-8`发送的就是浮点异常错误信息，而8的二进制是1000，也就是一旦出现异常，操作系统发送信号，将这个信号的二进制“贴合”到低8位上，也就是`00000000 00001000`，于因此退出码是8  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315161823121.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
**所以**：**如果进程不是异常退出的，那么操作系统就不可能给进程发送1-64的信号，其低8位就是0，然后子进程的退出码让其二进制形式“贴合”到高8位，那么要正常输出必须右移8位（虽然进程没有异常退出，但是我们需要获得退出码来验证这个进程的执行结果是否是正确的，依照不同的退出码返回不同的错误信息）；如果进程是异常退出的，那么操作系统给进程发送kill信号，让其信号值的二进制“贴合”到低八位，已经异常退出了，所以高8位一定是0，所以输出退出码，不需要右移**

所以我们现在可以去判断进程是否是异常退出的，如果是正常退出，其退出码又是多少。**如果是正常退出，那么其低8位一定是0，如果是异常退出其低8位一定不是0，所以我们让st和`00000000 11111110(0x7f)`进行与运算，如果其结果是0那么一定是正常退出的，反之不是0，那么一定是异常退出**

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            ;
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st=0;
		pid_t rec=waitpid(ret,&st,0);//阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			if((st&0x7f)==0)
			{
            
            
				printf("正常退出且其退出码为:%d\n",(st>>8)&0xff);
			}
			else
			{
            
            
				printf("异常退出，退出信号是:%d\n",st&0x7f);
			}
			
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }
	  int x=1/0;
      exit(0);
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

- 下面进行测试，**从上到下分别是正常退出，正常退出但结果不对和异常退出**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315164019352.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315164255711.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315164415844.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)

**可以看出上面分析过程与代码十分复杂，但是咋们在实际开发时并不这样写，这个逻辑的函数已经封装好了,它是宏函数**

- `WIFEXITED(status)`：判断进程是否异常退出，如果正常退出返回真
- `WEXITSTATUS(status)`：如果正常退出，看其结果是否正确

所以代码可以改写如下

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st=0;
		pid_t rec=waitpid(ret,&st,0);//阻塞
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			if(WIFEXITED(st))//如果为真，正常退出
			{
            
            
				printf("正常退出且退出码为%d\n",WEXITSTATUS(st));
			}
			else
			{
            
            
				printf("异常退出，信号值为%d\n",st&0x7f);
			}
			
			
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }
      exit(3);
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315170827598.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)

### C：进程非阻塞式等待

前面讲的都是进程的阻塞式等待，也就是父进程遇到`wai/waitpid`时，父进程就卡在那里，然后静静得看着子进程，子进程不死那么父进程就不向后执行

进程的非阻塞式等待和`waitpid`的第三个参数`options`有关，如果`options`设置为`WNOHANG`，表示：**如果指定的子进程还没有结束那么就不等待了，返回0，如果结束了就返回该子进程的ID**

下面这段程序中的`do while`循环表示每次用rec接受`waitpid`的返回值，由于waitpid设置了`WNOHANG`，因此如果要等待的子进程还没有结束它就会返回0。一直让这个循环走下去，直到子进程结束，其返回值是子进程的`PID`，也就是大于0的数，然后循环终止

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
    pid_t ret=fork();//其返回值类型是pid_t型的
    sleep(1);
    if(ret>0)//父进程返回的是子进程ID
    {
            
            
		printf("父进程正在等待子进程死亡\n");
		int st=0;
		pid_t rec=0;
		
		do
		{
            
            
			rec=waitpid(ret,&st,WNOHANG);//如果子进程没有结束返回0
			if(rec==0)//判断是否结束，
			{
            
            
				printf("子进程还在运行当中，请稍后再来\n");
			}
			else
			{
            
            
				break;//如果返回的是子进程的PID，那么就跳出循环
			}
			sleep(1);
			
		}while(1);//直到waitpid返回不是0，也就是子进程结束了
		
		
		if(rec==ret)//如果返回值是子进程id，等待成功
		{
            
            
			printf("等待成功\n");
			if(WIFEXITED(st))//如果为真，正常退出
			{
            
            
				printf("正常退出且退出码为%d\n",WEXITSTATUS(st));
			}
			else
			{
            
            
				printf("异常退出，信号值为%d\n",st&0x7f);
			}
			
			
		}
		else
		{
            
            
			printf("等待失败\n");
			exit(0);
		}
			
		
	}
    else if(ret==0)//子进程fork返回是0
    {
            
            
      int count=1;
      while(count<=10)
      {
            
            
        printf("子进程[%d]已经运行了%d秒\n",getpid(),count);
		count++;
        sleep(1);
      }

      exit(3);
    }
    else
      printf("进程创建失败\n");

    sleep(1);
    return 0;
}


```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210315185618879.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189479)  
**阻塞和非阻塞区别：想象你在电脑上下载游戏，阻塞就是从游戏下载开始我就一直盯着屏幕，它不下载完我就不干别的事情，而非阻塞就是游戏一直下载，我时不时的打开下载器看看进度，没有下载好的话我下次在看，但是在空余时间我可以看看视频，打打字**