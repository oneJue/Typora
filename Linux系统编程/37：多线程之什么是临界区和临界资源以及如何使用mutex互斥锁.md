 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）临界区，临界资源和原子性问题](#1_5)
  - [（2）互斥量（锁）](#2_99)
  - - [A：互斥锁](#A_107)
    - [B：锁的作用](#B_205)
    - [C：互斥锁实现的原理](#C_211)
  - [（3）可重入函数和线程安全](#3_224)
  - - [A：可重入函数和线程安全](#A_225)
    - [B：常见的线程安全和不安全情况](#B_230)
    - [C：常见可重入和不可重入的情况](#C_246)
  - [（4）死锁](#4_264)
  - - [A：死锁的概念](#A_265)
    - [B：死锁的四个必要条件](#B_268)
    - [C：解决死锁的方法](#C_276)

## （1）临界区，临界资源和原子性问题

- **临界资源**：多线程执行流共享的资源叫做临界资源（一般是被访问的共享资源）
- **临界区**：每个线程内部，访问临界资源的代码，叫做临界区
- **原子性**：一件事情要么完成，要么不完成，不要出现模棱两可的情况

如下有一个全局变量`a=10`，主线程创建两个线程，1号2号，分别执行`new_thread`函数，这个函数被两个执行流进入，因此称它被重入了。由于线程之间共享内存资源，因此全局变量可以被三个线程访问到，**这里变量a就叫做临界资源，每个线程内的访问临界资源的代码叫做临界区**

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>

int a=10;//全局变量
void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程%s,a=%d\n",(char*)arg,a);
    sleep(1);
  }
}

int main()
{
            
            
  pthread_t tid1;//线程1号
  pthread_t tid2;//线程2号
  pthread_create(&tid1,NULL,new_thread,(void*)"1号");
  pthread_create(&tid2,NULL,new_thread,(void*)"2号");

  while(1)
  {
            
             
    printf("我是主线程，a=%d\n",a);//主线程
    sleep(1);
  }
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210419092005806.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)  
如果不能妥善处理临界资源将会引发一定的问题。  
如下是一个简单的抢票系统的逻辑，在节假日期间抢票会达到高峰，这是会有多个线程进入抢票函数

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


int tickets=100;

void scarmble_tickets(void* arg)
{
            
            
  long int ID=(long int)arg;//线程ID
  while(1)//多个线程循环抢票
  {
            
            
    if(tickets>0)
    {
            
            
      usleep(1000);
      printf("线程%ld号抢到了一张票，现在还有%d张票\n",ID,tickets);
      tickets--;
    }
    else 
    {
            
            
      break;
    }
  }

}


int main()
{
            
            
  int i=0;
  pthread_t tid[4];//4个线程ID
  for(i=0;i<4;i++)
  {
            
            
    pthread_create(tid+1,NULL,scarmble_tickets,(void*)i);//创建4个线程
  }

  for(i=0;i<4;i++)
  {
            
            
    pthread_join(tid+1,NULL);//线程等待
  }
  return 0;
}

```

**在操作系统，要修改一个变量，首先要将其加载进CPU，运算之后在放回内存中，假如某个线程在做票数减一的操作数，刚刚把数据加载进了内存，还没来得及运算时又有一个线程进入了函数，此时它读取的票数仍然是没有修改过的数据，于是它进行运算，这样就导致票数只减少了1张，但是却卖出了2张票。**  
这就是一个原子性的问题，其他线程进入时，对于之前的线程它并不能保证使一项操作处于一个确定的状态，也就是要么票数没有减少，要么票数减少了，因此会引发一些严重的业务问题  
比如上面的程序运行之后，最终的票数竟然成为了负数  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210420081915726.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)

## （2）互斥量（锁）

可见，上面的案例中，票数并没有正确返回，究其原因主要有以下三点

- `if`语句判断条件为真后，代码可以并发的切换到其他线程
- `usleep`等待比较漫长（因为要等待一个线程把票数修改，然后进行调用输出到屏幕，这都是需要时间的），所以这个过程中可能会有多个线程进入该代码段
- `ticket--`本身就不是一个原子操作

### A：互斥锁

要解决上面的问题，本质就是需要一把锁，这把锁能保证以下行为

- 当代码进入临界区执行时，不允许其他线程进入该临界区
- 如果多个线程同时要求执行临界区代码，而且临界区没有线程在执行，那么只能允许一个线程进入该临界区
- 如果线程不在临界区中执行，那么该线程不能阻止其他线程进入该临界区

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210420082658884.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)  
使用互斥锁步骤如下

**1：在全局中创建一把锁，这把锁你可以认为是一种数据类型，对应的数据类型为`pthread_mutex_t`**

```c
pthread_mutex_t lock//创建一把名字叫做lock的锁
```

**2：在线程创建之前使用`pthread_mutex_init`初始化这把锁**

```c
int pthread_mutex_init(pthread_mutex_t* restrict mutex,const pthread_mutexattr_t* retstrict attr);
//mutex就是要初始化的那把锁
//第二个参数设置为NULL
```

**3：线程等待成功后，使用`pthread_mutex_destory`销毁锁**

```c
int pthread_mutex_destory(pthread_mutex_t* mutex);
//注意不要销毁一个已经加锁的锁，已经销毁的锁要确保后面不会有线程再尝试加锁
```

**4：使用`pthread_mutex_lock`和`pthread_mutex_unlock`进行在线程中进行加锁和解锁，注意加锁的粒度要细**

```c
int pthread_mutex_lock(pthread_mutex_t* mutex);
int pthread_mutex_unlock(pthread_mutex_t* mutex);
```

- 所以调用pthread\_mutex\_lock时，一个线程把资源锁定了，那么其他线程申请锁定时将被阻塞，直到那个线程把资源解封，其他线程才能申请加锁

所以根据以上描述，修改抢票代码

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


int tickets=1000;

pthread_mutex_t lock;//申请一把锁

void scarmble_tickets(void* arg)
{
            
            
  long int ID=(long int)arg;//线程ID
  while(1)//多个线程循环抢票
  {
            
            
    pthread_mutex_lock(&lock);//那个线程先到，谁就先锁定资源
    if(tickets>0)
    {
            
            
      usleep(1000);
      printf("线程%ld号抢到了一张票，现在还有%d张票\n",ID,tickets);
      tickets--;
      pthread_mutex_unlock(&lock);//抢到票就解放资源
    }
    else 
    {
            
            
      pthread_mutex_unlock(&lock);//如果没有抢到也要释放资源，否则线程直接退出，其他线程无法加锁
      break;
    }
  }

}


int main()
{
            
            
  int i=0;
  pthread_t tid[4];//4个线程ID
  pthread_mutex_init(&lock,NULL);//初始化锁
  for(i=0;i<4;i++)
  {
            
            
    pthread_create(tid+1,NULL,scarmble_tickets,(void*)i);//创建4个线程
  }

  for(i=0;i<4;i++)
  {
            
            
    pthread_join(tid+1,NULL);//线程等待
  }
  pthread_mutex_destroy(&lock);//销毁锁资源
  return 0;
}

```

效果如下，多个进程并发抢票，最后票数没有减少到负数（最后结果是1是因为先输出再减一的）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210420084548365.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)

### B：锁的作用

**1：因此这把锁的作用就是对临界区进行保护，所有的线程都必须执行这样的规则，每个线程在访问临界区时都必须要申请加锁，然后使用完毕之后再次解锁。锁本质也是一种临界资源，所以所有线程必须看见一把锁，锁要想保护临界资源的安全，首先要保证自己的安全，加锁和解锁都是原子性的操作。因此像这种，一次保证一个线程访问临界资源的现象就叫做互斥**

2：如果一个线程加锁之后在正在访问临界区，但是由于某种原因（比如时间片到了，或者碰到了更高优先级的线程）而被剥夺下来。虽然它被剥夺了，但是其他线程依然进入不了这个临界区，**对于其他线程来说，锁的就带了原子性，一个线程只要碰到临界区正在锁定那么表示有个线程正在操作，但是只要解锁一定能保证所做操作结果是唯一的，是原子性的**

### C：互斥锁实现的原理

首先前面的`++ticket`为什么不是原子性的操作呢，实际上这一条语句如果转换为汇编，实际上是需要三行的

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021042016460795.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)  
因此CPU每次运算时需要将内存中的值加载进来，计算完毕之后再将值放在内存中，这样**加载-运算-放回**的操作存在中间过程，因此会导致非原子性

其实为了实现互斥锁的操作，大多体系结构都提供了swap或exchange的指令\*\*，该指令的作用是把寄存器的和内存单元的数据进行交换，由于只有一条指令，所以保证了原子性\*\*  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021042016491729.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310058)

> 如上图，举个例子：我们知道线程在切换时，如果回到CPU需要依靠TSS恢复现场。假如有两个线程（分别为线程1和线程2），锁本质是一个变量，假设它在内存中表示的是1，谁拿到这个1就代表哪个线程得到了锁。因此假设线程1先执行`movb $0 %al`表示将0放在了寄存区al中，此时线程1还没有执行第二条语句的时候，线程2跑了过来，因此线程2也执行`movb $0 %al`，这样线程1和2都有了0.此时线程2下CPU，线程1回来，依靠TSS恢复数据，接着执行`xchab $al mutex`，将其寄存器中的数据与内存直接进行交换，那么线程1就拿到了所，接着线程1下CPU，线程2上CPU，依靠TSS恢复数据，其寄存器的值仍然为0，也执行`xchab $al mutex`，交换数据，但是此时内存中的1早就被线程1拿到了，因此线程没有拿到所，接着线程1回来，进行if判断返回，表示此时它拿到了锁，而线程2回来时，其实值为0，所以判断不成功，被挂起等待

**所以总之就是锁它只依靠了一条汇编就完成了操作，所以是原子性的**

## （3）可重入函数和线程安全

### A：可重入函数和线程安全

- **可重入函数**：前面就说过，一个函数如果被多个执行流同时进入，那么就称这个函数被重入了。如果重入之后不会引发任何问题那么就称这个函数为可重入函数否则称其为不可重入函数
- **线程安全**：线程安全和可重入函数说的其实是一个问题，线程安全是指多个线程同时执行一段代码时是否会出现问题，尤其常见于全局变量或静态变量

### B：常见的线程安全和不安全情况

**1：不安全的情况**

- 不保护共享变量的函数
- 函数状态随着被调用，状态发生变化的函数
- 返回指向静态变量指针的函数
- 调用线程不安全函数的函数（不可重入函数）

**2：安全的情况**

- 每个线程对全局变量或者静态变量只有读取的权限，而没有写入的权限，一般来说这些线程是安全的
- 类或者接口对于线程都是原子操作
- 多个线程的切换不会导致该接口的执行结果存在二义性

### C：常见可重入和不可重入的情况

**1：不可重入**

- 调用了malloc或free函数（malloc使用全局链表管理堆的）
- 调用了标准I/O库函数，标准I/O库的很多函数都是不可重入的
- 可重入函数体内使用了静态的数据结构

**2：可重入**

- 不使用全局变量或静态变量
- 不使用malloc或者new开辟出的空间
- 不调用不可重入函数
- 不返回静态或全局数据，所有数据都有函数的调用者提供
- 使用本地数据，或者通过制作全局数据的本地拷贝来保护全局数据

## （4）死锁

### A：死锁的概念

**当两个线程同时拥有一定的资源，但是都缺少对方手上的资源才能进行下一步动作，而去竞争对方的资源，从而都陷入等待的一种场景，这种场景被称为死锁。例如：A、B线程在运行开始时分别持有a、b对象，A拥有a，对象a被A上锁了，B拥有b，对象b被B上锁了，此时，线程A在要往后运行需要对象b，而线程B要往后运行需要对象a，此时A、B线程都希望获得对方的资源，但是手上的资源都不愿拿出来，这个时候就形成了“僵局”，进入了死锁**

### B：死锁的四个必要条件

产生死锁必定要满足以下条件

- **互斥条件**：一个资源每次只能被一个执行流执行
- **请求与保持条件**：一个执行流因请求资源时而被阻塞，对已经获得的资源不释放（也就是看着锅里，但是不放弃碗里的）
- **不剥夺条件**：一个执行流已经获得的资源，在没有使用完之前，不能被强行剥夺（也就是虽然我想要，但是我不能硬抢）
- **循环等待条件**：简单来说A想要B的，B想要的C的，C又想要A的，就发生了死锁

### C：解决死锁的方法

- 破坏死锁的的四个必要田间
- 加锁顺序一致
- 避免资源未释放的场景
- 资源一次性分配