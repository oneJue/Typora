 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）POSIX线程库](#1POSIX_5)
  - [（2）pthread\_create——创建线程](#2pthread_create_10)
  - - [A：关于Linux线程的再理解](#ALinux_78)
    - [B：线程ID及地址空间布局](#BID_90)
  - [（3）pthread\_exit——线程终止](#3pthread_exit_131)
  - [（4）pthread\_join——线程等待](#4pthread_join_246)
  - [（5）pthread\_detach——线程分离](#5pthread_detach_348)

## （1）POSIX线程库

前面说过，在Linux中是用进程模拟线程的，所以就不会用形如`fork()`这类的系统调用提供给我们用来专门控制线程。**所以要实现多线程，就要使用到库函数，这里面比较底层的是POSIX线程库，所以它就是产生的就是用户级别的线程，其绝大多数函数名字都是以`pthread_`开头，并且注意引入头文件`<pthread.h>`，而且链接时注意加入`-lpthread`选项**

## （2）pthread\_create——创建线程

复习：创建进程做了哪些事

> 父进程调用系统调用fork之后，就多了一个子进程。于是创建与该进程相关的一批数据结构，如PCB，地址空间还有files struct等；开辟地址空间之后，将虚拟地址与物理地址通过页表进行映射；将我们的代码和数据加载进物理内存；将进程投递到运行队列中供CPU调度

创建线程做了哪些事

> 创建线程不用额外分配资源，所以首先创建该线程的PCB，接着与创建该线程的进程共享地址空间；然后分配该进程的资源（代码和数据）；在库层面，形成一些与该线程相关的数据结构，为线程形成私有栈，最后返回线程ID（也就是一个地址）

**1：头文件**

```c
#include <pthread.h>
```

**2：函数原型**

```c
int pthread_create(pthread_t* thread,const pthread_attr_t* attr,void* (*start_routine)(void*),void* arg);
```

**3：参数说明**

**thread**：输出型参数，返回线程的ID，类型是`ptread_t`  
**attr**：设置线程的属性，一般使用默认属性，设置为NULL  
**start\_routine**：是一个函数指针，指向线程启动后要执行的函数  
**arg**：传给线程启动函数的参数

**4：返回值**

**成功**：返回0  
**失败**：返回错误码

**5：举例**

如下代码中，在`main`函数里，使用`pthread_create`创建一个线程（新线程），线程创建完毕之后让其执行`thread_run`这个执行流，`man`函数所在的执行流是主线程。两个线程各自有一个死循环，并且循环打印该线程所在的进程ID，

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>

void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("%s,我所在的进程id是:%d\n",(char*)arg,getpid());//获取此线程所在进程的ID
    sleep(1);
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我所在的进程的id是:%d\n",getpid());//获取主线程所在进程的ID
    sleep(1);
  }
}

```

效果如下，他们打印进程ID是一致的，这表明他们是同一个进程的两个不同的线程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210414101040746.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)

### A：关于Linux线程的再理解

上面我们创建线程后，使用`ps-aL`，查看进程信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210414142657218.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
其中LWP就是我们所说到过的轻量级进程。

**线程这个概念很早就出现了，但是那时操作系统它调度的基本单位是LWP，如果想要实现线程，有一种方法就是直接修改内核，让内核实现多线程（类似于Windows），但是这样做的风险很大。所以就有了类似于pthread这样的库，利用函数来实现多线程，这样的线程称为用户级线程，它属于用户空间，那么对于操作系统，它根本就不知道这样的函数库，操作系统管理时还是按照LWP的方式去管理。同时咋们的函数库在库函数里也会实现一些类似的数据结构去管理线程，然后再调用相应的结构（比如`pthread_create`,`pthread_exit`）实现对底层的LWP的管理**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210414152226476.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)

- 可以看出从内核角度的视角上来看，不管你是fork出了一个进程，还是thread出了一个线程，本质都是多了一个LWP，但是从用户角度来看，由于库函数的封装所以我们认为他是同一个进程，不同的线程

### B：线程ID及地址空间布局

`pthread_create()`函数会产生一个线程ID，存放在第一个参数指向的地址中，**可以使用`pthread_self()`函数获取本线程的ID**

如下，修改代码如下，使用格式控制符`%p`打印，线程ID

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(1);
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    sleep(1);

  }


}

```

效果如下，可以看出这个线程ID实则就是一个地址  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021041415402173.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
pthread提供的线程管理是一个用户级别线程，但是不论怎样管理，管理时总要先描述再组织，因此再者库函数中一定也会有类似`struct_pthread`的结构体进行描述，**而且我们编译时是动态链接的，因此程序在运行时动态库会被加载到共享区**，这里的线程ID实则指向的就是这片内存，库函数里的其他操作就是根据这个ID实现的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210414154409419.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)

## （3）pthread\_exit——线程终止

线程的终止可以使用`return`，但是主线程例外，因为主线程如果`return`了，相当于调用了`exit`退出进程，如果你不关心的退出状况，那么就传入`NULL`

```cpp
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(1);
    return(NULL);//线程退出
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    sleep(1);
  }
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418155528783.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
但我们经常使用的是`pthread_exit`，其实函数原型为

```c
void pthread_exit(void* value_ptr);
```

其中`value_prt`是一个指针，因为线程退出时可以把状态码（用int强转）放在一块内存里（必须是全局的或是malloc分配的），然后外部进行获取（这一点在线程等待具体细说）

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(1);
    pthread_exit(NULL);
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    sleep(1);
  }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418160037670.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
最后一种就是`pthread_cancel`函数，一个线程可以借助该函数结束另一个线程

```c
int pthread_cancel(pthread_t thread);
```

如下，主线程可以借助该函数结束新线程

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(1);
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  int count=0;
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    sleep(1);
    count++;
    if(count==3)
    {
            
            
      pthread_cancel(tid);//3秒后主线程结束新线程
      printf("新线程已经结束**********************************************************\n");
    }

  }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418160609886.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)

## （4）pthread\_join——线程等待

在进程那一章，我们知道，进程必须要被等待，如果一个子进程，父进程不等待那么就会使这个子进程变成僵尸状态。**对于线程也是一样的，对于主线程，如果它先于其他线程退出，那么这就会导致整个进程结束，而其他线程工作还没做完。已经退出的线程，其空间没有被释放，因此还在占用进程的地址空间，所以就会造成一定的内存泄漏**  
**因此调用ptread\_join函数的线程（比如主线程）将会挂起等待，知道id为thread的线程终止**

**1：头文件**

```c
#include <pthread.h>
```

**2：函数原型**

```c
int pthread_join(pthread_t thread,void** value_ptr);
```

**3：参数说明**

**thread**：线程的ID  
**value\_ptr**：**这是一个二级指针，前面说过线程退出时，可以使用pthread\_exit\(\)，将退出码放在一片内存空间里。所以在主线程里，我们可以定义一个一级指针，通过传入该一级指针的地址，这样就把那片内存空间里的内容，也就是状态码放在了这个指针里，也就是获得了线程退出时的状态码,如果主线程对新线程的结束状态不感兴趣，那么可以设置位NULL**

**4：返回值**

**成功**：返回0  
**失败**：返回错误码

如下，新线程退出时调用`pthread_exit((void)*10)`，表明将返回码10放在一片空间里，然后再主线程里使用ret获取这片空间的地址，最后使用整形读出。

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(5);
    
    pthread_exit((void*)10);//线程退出码
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    void* ret;//退出码
    pthread_join(tid,&ret);//阻塞等待
    printf("主线程阻塞等待新线程，退出码码为%d\n",(int)ret);//输出退出码
    break;
  }


}

```

可以发现主线程并没有死循环打印，因为它在阻塞等待  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418162751679.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
**最后需要注意的是，不像进程等待一样，我们不关心线程是否异常退出，因为线程一旦异常就等同于了进程异常。**

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            
  while(1)
  {
            
            
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(5);
    int a=1/0; //浮点异常
  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    void* ret;//获取退出码
    pthread_join(tid,&ret);
    printf("主线程阻塞等待新线程，退出码码为%d\n",(int)ret);
    break;
  }


}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418163431760.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)

## （5）pthread\_detach——线程分离

可以发现主线程在阻塞等待时是不能干自己的事情的，尤其是主线程在不关心新线程返回状态时这一点就显得更为繁琐，**所以如果主线程不关心新线程的状况，我们可以让新线程在结束后自己释放资源，需要使用到`pthread_detach`函数**

```c
int pthread_detach(pthread_t thread);
```

如下，在新线程中加入`pthread_detach(pthread_self())`，当新线程结束后会自动释放资源

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>


void* new_thread(void* arg)
{
            
            

  pthread_detach(pthread_self());//设置为分离，结束后自动释放资源
  int cout=0;
  while(1)
  {
            
            
    cout++;
    if(cout==5)
    {
            
            
      break;//5s后新线程退出
    }
    printf("我是新线程，我的线程id是%p\n",pthread_self());
    sleep(1);

  }
}

int main()
{
            
            
  pthread_t tid;//线程的ID
  pthread_create(&tid,NULL,new_thread,(void*)"我是新线程");
  while(1)
  {
            
             
    printf("-----------------------------------------------------------\n");
    printf("我是主线程，我的线程id是:%p，新线程的id是%p\n",pthread_self(),tid);
    sleep(1);
  }


}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210418171442782.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116310050)  
**需要注意的是即便新线程和主线程分离，但是一旦新线程出现了错误，那么也一定会波及到主线程，也就会使进程退出**