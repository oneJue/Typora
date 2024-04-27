 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/125668174)

### 文章目录

- [一：什么是进程间通信](#_9)
- [二：进程间通信方式1——共享存储](#1_33)
- - [（1）课本基础内容](#1_34)
  - - [A：概述](#A_36)
    - [B：分类](#B_49)
  - [（2）补充-Linux中的进程通信](#2Linux_63)
- [四：进程间通信方式2——消息传递](#2_75)
- - [（1）直接通信方式](#1_92)
  - [（2）间接通信方式](#2_105)
- [五：进程间通信方式3——管道](#3_118)
- - [（1）管道是什么](#1_119)
  - [（2）匿名管道](#2_125)
  - - [A：读端和写端](#A_126)
    - [B：建立匿名管道的函数](#B_136)
    - [C：最简单的进程间通信-演示](#C_149)
    - [D：管道四大特性](#D_203)
    - [E：管道的特点](#E_382)
    - [F：从内核角度理解管道](#F_391)
    - [G：管道总结](#G_404)
  - [（3）命名管道](#3_410)
  - - [A：命名管道和匿名管道的区别](#A_411)
    - [B：如何创建命名管道](#B_416)
    - [C：演示-管道实现服务端和客户端的通信](#C_431)

# 一：什么是进程间通信

**进程间通信（IPC）：进程是操作系统分配资源的基本单位，每个进程拥有各自独立的地址空间，在正常情况下，它们是互不干扰的，体现了进程的独立性**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1d5395bc584a4f5bb845fbd83c4a34c2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

**所谓独立性本质就是进程间数据的互不干扰，而这里进程间通信的目的是为了让数据产生交互**

- **这两点看似矛盾但实则不然**：我们所说的独立并不是完全独立，进程与进程之间也会产生协作关系，这种协作是需要通过通信来完成的

**进程间通信的目的无外乎以下几种**

- **数据传输**：一个进程需要将它的数据发送给另外一个进程
- **资源共享**：多个进程之间共享资源
- **通知事件**：一个进程需要向另一个或另一组进程发送消息，通知发生了什么事件
- **进程控制**：有些进程希望控制另一些进程，比如说调试功能

**操作系统实现进程间通信的方式有以下三种**

- **共享存储**
- **消息传递**
- **管道通信**

# 二：进程间通信方式1——共享存储

## （1）课本基础内容

### A：概述

**共享存储：每个进程都有自己的地址空间（虚拟的），页表负责将虚拟内存映射到真实的物理内存处**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405185924272.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

**既然页表是负责映射的，那么自然就可以在物理内存上开辟一片空间，然后让他们都映射到同一片内存空间，这样的话就可以进程间通信了。其中被映射的区域称其为共享存储区**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405190300481.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

**需要注意的是，为避免出错，各进程对共享空间的访问应该是互斥的**

### B：分类

**共享存储分类：分为以下两类**

- **基于数据结构的共享**：这种共享方式**速度慢、限制多**，是一种**低级通信**方式

- **基于存储区的共享**：是指操作系统在内存中会划分出一块共享存储区，**数据的形式、存放位置都由通信进程控制，而不是操作系统**，是一种**高级通信**方式

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff47a5bfab9b94985a9bf9db801b1d6f5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

## （2）补充-Linux中的进程通信

**进程间通信远远没有上面所叙述的那么简单，想要使两个进程看见同一份内存资源，以及看见资源后如何写入，读取，同时对于共享区域的安全及其他问题这些都是需要去考虑的，况且操作系统内会存在大量的进程通信。所以想要管理好进程间通信，一定是先组织，再描述，也就是说底层会存在大量与此相关的数据结构**

在Linux中，描述进程时会使用`task_strct`，描述文件时会使用`file struct`，**描述进程间通信则会使用`shmid_ds`**

- 相关篇幅较长，如有兴趣可以移步[Linux系统编程28：进程间通信之共享内存和相关通信接口（ftok,shmget,shmctl,shmat,shmdt）](https://blog.csdn.net/qq_39183034/article/details/115354952)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406130416659.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

# 四：进程间通信方式2——消息传递

**消息传递：进程间的数据交换以格式化的消息\(Message\)为单位。进程通过操作系统提供的“发送消息/接收消息”两个原语进行数据交换**

**消息由以下两部分构成**

- **消息头**：包括**发送进程DI、接受进程ID、消息长度等格式化信息**
- **消息体**：消息的主体内容

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F27ab551e30fa4f64ac678dfdcb54282c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

**消息传递有以下两种方式**

- **直接通信方式**：消息的发送进程**直接指明**接受进程的ID
- **间接通信方式**：通过“**信箱**”间接通信，所以又称为**信息通信方式**

## （1）直接通信方式

**直接通信方式：在每个进程的PCB中，会有一个数据结构叫做消息队列，它保存了所有由其他进程发送给自己的消息。在直接通信方式下，进程 P P P向进程 Q Q Q通信的过程如下**

- 进程 P P P组装好待发送的消息
- 进程 P P P使用**发送原语`send(Q, msg)`**，将消息挂靠到进程 Q Q Q的消息队列队尾
- 进程 Q Q Q使用**接收原语`receive(P, &msg)`**，拿到消息队列中由进程 P P P发送过来的消息

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Faa16fa601859401a9681cd184d72e5bc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

## （2）间接通信方式

**间接通信方式：在内存中会划分出若干区域，称其为“信箱”，作用是暂存某个进程发送过来的消息。多个进程可以向同一个信箱发送消息，也可以从同一个信箱中接受消息。在间接通信方式下，进程 P P P向进程 Q Q Q通信的过程如下**

- 进程 P P P组装好待发送的消息
- 进程 P P P使用**发送原语`send(A, msg)`**，将消息发送到指定的信箱 A A A中
- 进程 Q Q Q使用**接收原语`receive(A &msg)`**，从信箱 A A A拿到消息

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F57605b9b16f943aeaebf2576afc2632e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

# 五：进程间通信方式3——管道

## （1）管道是什么

管道是UNIX中一种古老的通信方式，管道本质其实是一个文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404165545870.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
如上，命令行who的标准输出原本是屏幕，但是却输出到了管道文件中，发生了重定向，然后wc命令再从以管道文件作为标准输入，然后输出到屏幕中  
其中`who | wc \-l`这种属于匿名管道

## （2）匿名管道

### A：读端和写端

从`who | wc \-l`可以看出，who作为一个进程是把内容写入管道文件，使用的是管道的**写端**，wc从管道中读入数据，使用的是管道的**读端**。  
**所以两个进程利用管道通信时，一个进程要使用管道的写端写入数据，另一个进程则要使用管道的读端读入数据**，所以管道文件就要用两个文件描述符进行控制，一个控制读端，一个控制写端

所以下面是父进程创建了管道  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404170843482.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
接着父进程创建子进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404171114698.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
可以发现此时父子进程可以同时对管道进行写入的和读取，**但是管道只能一端写入一端读入**，所以要进行调整  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040417121921.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

### B：建立匿名管道的函数

`pipe`函数用于建立匿名管道，其头文件是`unistd.h`  
其函数原型为`int pipe(int fd[2])`,其中fd是一个有两个元素`fd[0]`,`fd[1`\]的数组，传入函数pipe后在其内部分别以读写的方式打开管道文件，默认情况下，`fd[0`\]和`fd[1]`会分别获得文件描述符，其中`fd[0`\]表示读端，`fd[1]`表示写端

模拟实现一下pipe函数可能就是下面这样的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404195752821.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
有很多同学在这里会感到疑惑，因为用于进程间通信的管道文件就只有一个，为什么会有两个文件描述符呢？（默认是3和4）

**其实这一点在之前的基础IO中我没有表示特别清楚，以读方式的打开一个文件，会分配一个描述符（假设是3），然后再以写方式打开刚才的你文件也会分配一个描述符（假设是4），这里的3和4操作的是一个文件，只不过一个负责读，一个负责写**  
比如下面这个例子就可以说明这个情况  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404200216177.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404200253410.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

### C：最简单的进程间通信-演示

这里我们可以根据上面读端和写端的那个流程，首先调用pipe函数，接着创建子进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404200730711.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
现在的场景就是这样  
**接着，我们让子进程写入数据到管道，父进程从管道读取数据。于是关闭子进程的读端也即是`fd[0]`，关闭父进程的写端也就是`fd[1]`**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404200817950.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

还有一点十分重要：文件描述符数组（`pipefd`）是创建子进程之前就有的，而且调用`pipe`函数之后，数组内容就没有改变过，所以不发生写时拷贝，所以数组是父子进程共有的。但是`files struct`是父子进程各自拥有的,对子进程来说close\(fd\[0\]\)，就相当于抹杀了子进程对该文件的读权限  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404201732924.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
为了观察方便，对父子进程都是用死循环。子进程每隔一秒读入一段信息`this is the data that the child process wrote`用来证明子进程写入了数据；对于父进程则取读取数据，一旦读完数据，就输出`the father process got the information`，用来证明父进程读取到了数据

```c
#include  <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main()
{
            
            
  int  pipefd[2]={
            
            0};
   pipe(pipefd);
  pid_t id=fork();
  if(id==0)//child
  {
            
            
    close(pipefd[0]);
    const char* msg="This is the data that the child process wrote";
    while(1)
    {
            
            
        write(pipefd[1],msg,strlen(msg));
        sleep(1);
    }     
  }
  else//father
  {
            
            
    close(pipefd[1]);
    char buffer[64];
    while(1)
    {
            
            
        ssize_t ret=read(pipefd[0],buffer,sizeof(buffer)-1);
        if(ret>0)//判断是否读到
        {
            
            
            buffer[ret]='\0';//加上结束标志，便于输出
            printf("The father process got the information:%s\n",buffer);
        }
    }
  }
  return 0;
}
```

效果如下，这样就完成了一个最简单的进程间通信  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404203115981.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

### D：管道四大特性

**特性一：如果写端（这里是子进程）不关闭文件描述符，且不写入（简称为读端条件不就绪），那么读端可能会长时间阻塞（当管道有历史数据时会先读完，管道为空，且写端不写入会长时间堵塞），也就是读端快，写端慢**

比如，将上面例子中，子进程的睡眠由1秒提升至5秒，就会发现虽然父进程在死循环且没有睡眠的情况下，**也会和子进程同步**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404204111384.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
**特性二：当写端在写入时，写端条件不就绪（比如管道已经满了），写端就要被阻塞，也就是写端快，读端慢**

比如，修改上面的例子如下，在子进程中使用cout查看子进程写入管道的次数，然后父进程每隔1s读取一次

```c
#include  <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main()
{
            
            
  int  pipefd[2]={
            
            0};
  pipe(pipefd);

  pid_t id=fork();
  if(id==0)//child
  {
            
            
    close(pipefd[0]);
    const char* msg="This is the data that the child process wrote";
    int cout=0;//统计次数
    while(1)
    {
            
            
        write(pipefd[1],msg,strlen(msg));
        printf("The number of times the child process writes:%d\n",cout++);
    }
      
  }
  else//father
  {
            
            
    close(pipefd[1]);

    char buffer[64];
    while(1)
    {
            
            
        ssize_t ret=read(pipefd[0],buffer,sizeof(buffer)-1);
        if(ret>0)
        {
            
            
            buffer[ret]='\0';
            printf("The father process got the information:%s\n",buffer);
            sleep(1);
        }
    }
  }
  return 0;
}
```

效果如下，写端瞬间将管道充满，然后读端慢慢的从管道中读数据  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404210001794.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

**特性三：如果写端关闭文件描述符，那么读端当读完管道内容后，或读到文件结尾（此时read的返回值是0）**

如下，当子进程读入上次后，关闭子进程的写端，跳出循环退出子进程，父进程仍旧每秒从管道中读取数据一次，并且输出read接口的返回值

```c
#include  <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main()
{
            
            
  int  pipefd[2]={
            
            0};
  pipe(pipefd);

  pid_t id=fork();
  if(id==0)//child
  {
            
            
    close(pipefd[0]);
    const char* msg="This is the data that the child process wrote";
    int cout=0;
    while(1)
    {
            
            
        write(pipefd[1],msg,strlen(msg));
        printf("The number of times the child process writes:%d\n",cout++);
        if(cout==10)
        {
            
            
          close(pipefd[1]);//读10次后关闭写端
          break;
        }
    }
    exit(2);
      
  }
  else//father
  {
            
            
    close(pipefd[1]);

    char buffer[64];
    while(1)
    {
            
            
        ssize_t ret=read(pipefd[0],buffer,sizeof(buffer)-1);
        if(ret>0)
        {
            
            
            buffer[ret]='\0';
            printf("The father process got the information:%s\n",buffer);
            sleep(1);
        }
        printf("the father process read the end of file which the return value of 'read' is %ld\n",ret);//read接口的返回值
    }
  }
  return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404212656851.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
刷屏太快，将其重定向到文件中。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404212934313.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
**对比特性一，特性一中是写端不关闭文件描述符还写的特别慢，因此读端也被牵制住，造成读端堵塞。而当写端文件描述符关闭之后，这个管道文件唯一的输入来源就切断了，因此如果不给其结束标记，那么就会造成读端永久阻塞**

**特性四：如果读端关闭文件描述符，那么写端有可能被操作系统结束掉**

如下，让子进程不断写入数据，让父进程读取5次数据后，就关闭读端，使用如下脚本观察进程

```bash
while :; do ps axj | grep test.exe | grep -v grep; echo "#######################";sleep 1;done
```

```c
#include  <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main()
{
            
            
  int  pipefd[2]={
            
            0};
  pipe(pipefd);

  pid_t id=fork();
  if(id==0)//child
  {
            
            
    close(pipefd[0]);
    const char* msg="This is the data that the child process wrote";
    int cout=0;
    while(1)
    {
            
            
        write(pipefd[1],msg,strlen(msg));
        printf("The number of times the child process writes:%d\n",cout++);
    }  
      
  }
  else//father
  {
            
            
    close(pipefd[1]);

    char buffer[64];
    int cout=0;
    while(1)
    {
            
            
        ssize_t ret=read(pipefd[0],buffer,sizeof(buffer)-1);
        if(ret>0)
        {
            
            
            buffer[ret]='\0';
            printf("The father process got the information:%s\n",buffer);
            sleep(1);
            cout++;
        }
        if(cout==5)
        {
            
            
            close(pipefd[0]);//读5次后就关闭读端
        }
    }
  }
  return 0;
}
```

可以很明显当父进程读5次后，子进程退出，变为了僵尸状态  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404215910955.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
当读端关闭之后，就没有进程读取数据了，那么写入的操作就变成了一种无用操作，所以操作系统发现了这种浪费资源的行为后，就发送了13号信号，结束了子进程  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404220724978.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
根据前面的进程等待的知识，我们也可以获取退出信号，正是`SIGPIPE`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210404221041185.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

### E：管道的特点

1.  **只能用于具有共同祖先的进程（具有亲缘关系的进程）之间进行通信**。通常，一个管道由一个进程创建，然后该进程调用fork，此后父子进程之间就可使用该管道
2.  **管道提供流式服务**。所谓流式服务就是读端在读取时可以任意读取，想读多少就读多少，就像水龙头一样，你想开多大完全取决于你
3.  一般而言，进程退出，管道释放，**所以管道的生命周期跟随进程**
4.  由特性1，2可知，**管道之间具有同步和互斥的机制**
5.  **管道是半双工的**，数据只能向一个方向流动

### F：从内核角度理解管道

Linux下一切皆文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405091455786.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
如下便是进程打开的文件的file结构体，其中有一个结构体path  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405091740145.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
跳转过去，其中的dentry表示该file所在的目录结构体  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040509192612.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
跳转过去，当找到其所在的目录后，其结构体内就存储了目录的inode  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405092049173.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
根据目录的inode可以找到目录的数据块，而之前说过目录中存储的就是文件名和inodei·映射关系，于是就可找到该文件file的inode，如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405092253968.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
而inode中有一个union，它是迎来标识文件类型的，可以发现第一个便是管道文件  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405092336553.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

### G：管道总结

至此我们便可以从更深的层次中理解管道的本质。`sleep 1000 | sleep 2000`，分别是两个进程，它们的父进程均是bash，所以bash创建了管道，然后关闭了它对管道的通信，这两个sleep命令则利用管道进行通信  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405102143907.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
**`who | wc \-l`，bash创建了管道，`who`和`wc`利用管道通信，`who`发生输出了重定向，将输出重定向的管道文件中，`wc`发生了输入重定向，将输入来源从键盘更改为管道文件，Linux一切皆文件，这就管道的本质**

## （3）命名管道

### A：命名管道和匿名管道的区别

前面说过，匿名管道的限制就是只能在具有共同祖先（具有亲缘关系）的进程间通信，而不适合与毫无相干的两个进程

**如果我们想在两个不相干的进程之间进行通信，可以使用FIFO文件完成，也被称为命名管道，命名管道实际是一种类型为“p”的文件**

### B：如何创建命名管道

匿名管道由`pipe`函数创建并打开，命名管道则有`mkfifo`函数创建  
命名管道可以从命令行上创建

```bash
mkfifo filename
```

也可以从程序中创建，其函数原型为

```c
int mkfifo(const char* filename,modet_t mode);
//filename是创建管道文件的路径+文件名
//mode是权限值
```

### C：演示-管道实现服务端和客户端的通信

如下我将虚拟机中的centos系统作为服务端，在其上创建一个文件叫做`server.c`，服务端用来读取数据。  
在Windows中，以Windows作为客户端，客户端用来写入数据，利用xshell远程登录主机，然后创建一个文件`client.c`。这就像xshell是QQ窗口，我像Linux主机，也就是腾讯服务器发送消息，然后服务端回传回来。虽然不是特别准确，但是足以说明命名管道在·的作用

Makefile如下

```bash
.PHONY:all
all:client.exe server.exe 

client.exe:client.c
	gcc -o $@ $^
server.exe:server.c
	gcc -o $@ $^

.PHONY:clean
clean:
	rm client.exe server.exe fifo

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405115151462.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
首先编写服务端代码，服务端中利用mkfifo创建管道文件，然后打开这个管道文件，不断读取数据

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main()
{
            
            
  umask(0);//屏蔽命令行umask干扰
  if(mkfifo("./fifo",0666)==-1)//如果mkfifo返回值是-1，创建失败
  {
            
            
    perror("打开失败");
    return 1;
  }
  int fd=open("fifo",O_RDONLY);//服务端以只读的方式打开管道文件
  if(fd>=0)
  {
            
            
    char buffer[64];
    while(1)
    {
            
            
      printf("客户端正在接受消息\n");
      printf("############################\n");
      ssize_t ret=read(fd,buffer,sizeof(buffer)-1);
      if(ret>0)
      {
            
            
          buffer[ret]='\0';
          printf("服务端接受到客户端消息：%s\n",buffer);
      }
      else if(ret==0)//如果客户端退出，将会读到文件结束，所以服务端也要退出
      {
            
            
          printf("客户端已经下线，服务端下线\n");
          break;
      }
      else 
      {
            
            
        perror("读取失败\n");
        break;
      }
    }
  }
}

```

在客户端则直接打开管道文件，然后写入数据

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>


int main()
{
            
            
  int fd=open("fifo",O_WRONLY);//直接打开管道文件
  if(fd>=0)
  {
            
             
      char buffer[64];//从键盘读入数据到这个缓冲区
      while(1)
      {
            
             
          printf("客户端-请输入消息：");
          ssize_t ret=read(0,buffer,sizeof(buffer)-1);//从键盘读入数据
          if(ret>0)
          {
            
             
              buffer[ret]='\0';
              write(fd,buffer,ret);//读入ret个数据就向管道中写入ret个数据
          }
      }
  }
}

```

现在，在虚拟机中运行服务端，然后在xshell中运行客户端，然后客户端输入数据，服务端就会接受到，客户端下线，服务端也会下线  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405123128188.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)

这里还有一个非常有趣的点：那个fifo文件是0个字节，自始至终它都是一个字节  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040514360760.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
**这表明它们之间通信时，并没有直接在这个文件上进行IO操作**，因为如果进行IO操作其实代价就太大了。**这里的fifo其实仅仅起到了一种标志的作用**，它的底层其实和匿名管道是差不多的

上面演示的是调用系统调用接口mkfifo进行操作，而mkfifo其实也是一个命令，也就是直接可以创建管道完成通信  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405144016395.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)  
然后在客户端输入一段脚本，将一段文字不断输入到管道中，接着在服务端不断读取

```bash
while :; do echo "this is client";sleep 1;done > pipe
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040514450033.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120982541)