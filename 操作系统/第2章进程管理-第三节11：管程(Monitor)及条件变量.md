 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/125668174)

### 文章目录

- [一：管程的定义和基本特征](#_14)
- [二：条件变量](#_58)
- [三：补充-Linux中实现线程同步的条件变量函数](#Linux_98)
- [四：Java中的管程机制](#Java_201)

通过前面**经典同步问题**的讲解，大家也可能体会到了这一点，**信号量机制的确为我们提供了一种安全可靠的实现进程同步的办法，但是其对应的PV操作却在编写程序上带来了一定困难，哪怕顺序写错都有可能造成死锁等严重的问题**，如果程序员的功力不深，很难驾驭这种操作

于是正是基于此，产生了一种高级同步工具——**管程**，它**保证了进程互斥、无需程序员自己实现互斥，从而降低了死锁发生的可能性，同时它也提供了条件变量，可以让程序员灵活实现进程同步**

管程属于**语言级别**的互斥解决方案，最早由 B r i n c h Brinch Brinch H a n s o n Hanson Hanson和 H o a r e Hoare Hoare于上世纪70年代提出，已在 P a s c a l Pascal Pascal、 J a v a Java Java， P y t h o n Python Python等语言中得到了实现。需要注意的是管程的因为全称为 M o n i t o r Monitor Monitor P r o c e d u r e s Procedures Procedures，意为管理一个或多个执行程序

# 一：管程的定义和基本特征

**管程\(Monitor\)：它是一种特殊的软件模块，由以下部分组成**

- **局部于管程的共享数据结构说明**——可以理解为类的成员
- **对该数据结构进行操作的一组过程\(函数\)**——可以理解为类的方法
- **对局部于管程的数据设置初始值的语句**——可以理解为类的构造函数
- **管程有一个名字**——可以理解为类名

所以，**管程是一个代表共享资源的数据结构，进程对共享资源的申请、释放等操作是通过过程来实现的（过程就是对这一数据结构的操作），这组过程还可以根据资源情况，或接受或阻塞进程的访问，确保每次仅有一个进程使用共享资源，这样就可以统一管理对共享资源的所有访问，实现进程互斥，即：**

```c
monitor Test//定义了一个名称为"Test"的管程
{
            
            
	Data Structure DS;//定义共享数据结构，对应系统中的某种共享资源
	Init_Code()//对共享数据结构初始化语言
	{
            
            
		DS=5;//初始资源数目为5
	}
	
	Take_Away()//过程1：申请一个资源
	{
            
            
		对共享数据结构的一系列操作
		DS--;//可用资源数目减一
		...
	}
	Give_Back()//过程2：归还一个资源
	{
            
            
		对共享数据结构的一系列操作
		DS++;//可用资源数目加一
		...
	}
}
```

**管程基本特征：**

- **局部于管程的数据只能被局部的过程所访问**——可以理解为类成员变量只能被类方法访问
- **一个进程只有通过调用管程内的过程才能进入管程访问共享数据**——可以理解为访问要合法进行
- **每次仅允许一个进程在管程内执行某个内部过程**

# 二：条件变量

当一个进程进入管程后被阻塞，直到阻塞的原因解除时，在此期间，如果该进程不释放管程，**那么其他进程无法进入管程**，为此，将**阻塞原因定义为条件变量`condition`**

如果一个进程被阻塞的原因有多个，那么就设置多个条件变量，而**每一个条件变量保存了一个等待队列，用于记录因该条件变量而阻塞的所有进程**，对于条件变量的操作只有两种：**`wait`和`signal`**

- `x.wait`：当`x`对应的条件不满足时，正在调用管程的进程调用`x.wait`将自己插入`x`条件的等待队列，并释放管程。此时其他进程可以使用该管程
- `x.signal`：当`x`对应的条件发生了变化，则调用`x.signal`，唤醒一个因`x`条件而阻塞的进程

逻辑描述如下

```c
monitor Test()
{
            
            
	Data Structure DS;//定义共享数据结构，对应系统中的某种共享资源
	condition x;//定义了一个条件变量x
	Init_Code(){
            
            ...}
	
	Take_Away()
	{
            
            
		if(DS<=0)
		{
            
            
			x.wait();//资源不够，在条件变量x上阻塞的大概
		}
		资源足够，分配资源，做一系列相应处理
	}
	
	Give_Back()
	{
            
            
		归还资源，做一系列相应处理
		if(进程在等待)
			x.sginal();//唤醒一个阻塞进程
	}
}
```

可以看出：**相较于信号量，条件变量或者管程把具体的同步互斥关系实现封装了起来，只暴露两个特别简单的接口以供程序员调用，而这两个接口其反应的本质问题就是资源是否存在，能否互斥访问的问题，程序员不用关心复杂的同步互斥关系，只关心在相应的程序逻辑下资源数目的问题**

# 三：补充-Linux中实现线程同步的条件变量函数

Linux下可以使用pthread\(POSIX\)线程库实现线程同步，下面的这些函数其实就是咋们上面说到过的过程

**1：像创建锁一样，在全局域中创建一个`pthread_cond_t`类型的变量`cond`**

```c
pthread_cond_t cond;
```

**2：在创建线程之前，使用`pthread_cond_init()`函数初始化条件**

```c
int pthread_cond_init(pthread_cond_t* restrict cond,const pthread_condattr_t* restrict attr);
//cond是那个条件
//attr设置为NULL
```

**3：条变量件也是要销毁的，使用`pthread_cond_destroy()`销毁**

```cpp
int pthread_cond_destory(pthread_cond_t* cond)
```

**4：某个线程需要等待消息（或者说等待条件满足）才能被唤醒执行，所以需要等待条件的那个线程使用`pthread_cond_wait()`函数**，关于该函数第二个参数为什么要携带一把锁，[解释请点击本文字跳转](#1)

```c
int pthread_cond_wait(pthread_cond_t* restrict cond,pthread_mutex_t* restrict mutex)
//cond：该线程需要等待这个条件
//mutex：
```

**5：某个线程需要唤醒一个线程，那么就使用`pthread_cond_signal()`函数**

```c
int pthread_cond_signal(pthread_cond_t* cond);
```

如下，创建线程1,2,3,4,5，让线程1唤醒其他线程。

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;//锁
pthread_cond_t cond;//条件

void* thread_a(void* arg)//其他线程被唤醒
{
            
            
  const char* name=(char*)arg;//线程名字
  while(1)
  {
            
            
    pthread_cond_wait(&cond,&lock);//一直在等待条件成熟
    printf("%s被唤醒了\n=================================================\n",name);

  }

}

void* thread_1(void* arg)//让线程1唤醒其他线程
{
            
            
  const char* name=(char*)arg;
  while(1)
  {
            
            
    sleep(rand()%5+1);//随机1-5秒唤醒
    pthread_cond_signal(&cond);
    printf("%s现在正在发送信号\n",name);
  }

}



int main()
{
            
            
  pthread_mutex_init(&lock,NULL);//初始化锁
  pthread_cond_init(&cond,NULL);//初始化条件

  pthread_t t1,t2,t3,t4,t5;//创建两个线程
  pthread_create(&t1,NULL,thread_1,(void*)"线程1"); 

  pthread_create(&t2,NULL,thread_a,(void*)"线程2"); 
  pthread_create(&t3,NULL,thread_a,(void*)"线程3"); 
  pthread_create(&t4,NULL,thread_a,(void*)"线程4"); 
  pthread_create(&t5,NULL,thread_a,(void*)"线程5"); 

  pthread_join(t1,NULL);
  pthread_join(t2,NULL);
  pthread_join(t3,NULL);
  pthread_join(t4,NULL);
  pthread_join(t5,NULL);

  pthread_mutex_destroy(&lock);
  pthread_cond_destroy(&cond);//销毁条件

}


```

效果如下，可以看到当线程1发送信号后想，线程2,3,4,5被唤醒，**并且唤醒是有顺序的**，这表明cond肯定是有这样的队列存在的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210421094918137.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121432664)

# 四：Java中的管程机制

本人主要方向为C/C++，对Java不太熟悉，如果需要可查看：

> [Java中的管程](https://blog.csdn.net/death05/article/details/94177146?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163733729216780274123058%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163733729216780274123058&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-94177146.first_rank_v2_pc_rank_v29&utm_term=java+%E7%AE%A1%E7%A8%8B&spm=1018.2226.3001.4187)  
> 作者：[death05](https://blog.csdn.net/death05)