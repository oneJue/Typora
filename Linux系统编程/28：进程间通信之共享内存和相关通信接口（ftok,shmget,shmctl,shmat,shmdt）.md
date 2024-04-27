 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）管道和共享内存的区别](#1_6)
  - [（2）先组织，再描述](#2_22)
  - [（3）进程间通信相关接口](#3_29)
  - - [A：ftok（获取唯一标识码）](#Aftok_37)
    - [B：shmget（创建共享内存）](#Bshmget_83)
    - [C：shmctl（控制共享内存）](#Cshmctl_159)
    - [D：shmat（将共享内存段与当前进程挂接）](#Dshmat_229)
    - [E：shmdt（将共享内存段与当前进程脱离）](#Eshmdt_253)
  - [（4）演示-共享内存实现客户端和服务端的通信](#4_352)
  - [（5）补充](#5_437)

  
前面讲到了管道，不管是匿名管道也好，还是命名管道也罢，它都是文件，也就是两个进程之间通信时还要借助文件系统。但是，文件系统也是帮助我们最终回到相同的内存空间， **所以如果能跳过文件系统，直接让进程看到相同的内存空间，是否通信的效率就会提高呢**？ **答案是的，这一种方式就是System V，它是一种标准，这里重点介绍的是System V共享内存**

## （1）管道和共享内存的区别

前面在进程地址空间的时候，我们知道了每个进程看到的都是虚拟内存，页表则负责将虚拟内存映射到真实的物理内存处  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405185924272.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

既然页表是负责映射的，那么是否可以在物理内存上开辟一片空间，然后通过页表让他们都映射到一片内存空间，这样就符合“看到同一片内存空间”的规则呢？  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405190300481.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
答案是可以的。这样的话，两个进程在进行读写时实际操纵的是同一片内存，进程1的读写操作就可以让进程2看到了。

**大家可以回忆刚才管道的操作，在写端写入时，首先从标准输入中输入，拷贝至buffer中，然后再从buffer拷贝到管道文件中，这实则经过了两次拷贝，同时读取时从管道中读取，这算拷贝一次，接着再输出到标准输出中，还算一次拷贝，总共经过了4次拷贝。而通过咋们刚才的页表映射时，直接写入，只需要拷贝一次，对于另一个进程，它是可以直接看到，感知到这块内存的，所以总共只需要进行一次拷贝。所以共享内存要实现进程通信要比管道快很多。**

所以，共享区除了放咋们前面说过的动态库外，还可以放共享内存，也是专门处理通信的区域  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210405191141113.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

## （2）先组织，再描述

不管是管道，还是共享内存，进程间通信的本质就是让他们看见相同的内存资源。大家需要明白的一点是，**进程间通信不是嘴上说说那么简单，想要实现两个进程看见同一份内存资源，以及看见资源后如何写入，读取，同时对于这份内存如何把不同进程关联上去，如何保证关联的稳定等等 ，这都是需要去管理的，况且操作系统会存在大量的进程通信，所以对于操作系统，想要管理好进程间通信，一定是先组织，再描述，也就是底层会存在大量与此相关的数据结构**

正如描述进程时的`task_strct`,描述文件时的`file struct`，描述进程间通信的结构体则是`shmid_ds`

这里仅做展示，具体其中参数都是什么作用将会在后面一并介绍  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406130416659.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

## （3）进程间通信相关接口

既然我们需要共享内存来帮助进程完成通信，那么必然要对共享内存进行相应的操作，操作系统也提供了相应的接口给我们，以便完成一些基础操作，涉及的操作主要会有

1.  创建共享内存
2.  删除共享内存
3.  关联共享内存
4.  取消共享内存的关联

### A：ftok（获取唯一标识码）

**1：头文件**

```c
#include <sys/type.h>
#include <sys/ipc.h>
```

**2：函数原型**

```c
key _t ftok(const char* pathname,int proj_id);
```

**3：参数说明**

**pathname**：可以是目录或者是文件的绝对路径，可以随便设置  
**proj\_id**：设置权限

**4：作用及返回值**

共享内存有很多，你既然申请了一块内存，那么操作系统总要能区分出来，所以它的作用就类似于身份证号一样，**生成一个唯一的数字，来标识你申请的那块内存**

**5：源代码**

`shmid_ds`结构体中有一个叫做`ipc perm`，系统为每一个共享内存保存一个`ipc_perm`结构体，该结构说明了共享内存的权限和所有者  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406133658442.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
注解如下

```c
struct ipc_perm
{
            
            
	key_t	key;          //调用shmget()时给出的关键字
	uid_t	uid;          /*共享内存所有者的有效用户ID */
	gid_t   gid;          /* 共享内存所有者所属组的有效组ID*/
	uid_t   cuid;         /* 共享内存创建 者的有效用户ID*/
	gid_t   cgid;         /* 共享内存创建者所属组的有效组ID*/
	unsigned short	mode; /* Permissions + SHM_DEST和SHM_LOCKED标志*/
	unsignedshort	seq;  /* 序列号*/
};

```

所以ftok的结果就是上面的那个key

### B：shmget（创建共享内存）

**1：头文件**

```c
#include <sys/ipc.h>
#include <sys/shm.h>
```

**2：函数原型**

```c
int shmget(key_t key, size_t size,int shmflg);
```

**3：参数说明**

**key**：ftok的返回值  
**size**：共享内存创建的大小，**最好是页的整数倍**  
**shmflg**：一些标志选项

- `IPC_CREAT`：如果内核中不存在键值与传入的key值相等的共享内存，则会新建一个共享内存；如果内核中存在键值与传入的key值相等的共享内存，则返回此共享内存的标识符。如果按照从文件的角度理解就是：**打开一个目标名字为key的文件，没有的话就创建，有的话就返回key**
- `IPC_CREAT|IPC_EXCL`：承接上面，如果内核中存在键值与传入的key值相等的共享内存，则报错。如果从文件的角度理解就是：**打开一个目标名字为key的文件，没有的话就创建，有的话直接报错，说key已经存在了**

**4：返回值**

**成功**：返回共享内存的标识符  
**错误**：返回-1

这里还是要强调一下该函数的返回值和传入的参数key值的区别。**key值是内核用于唯一标识共享内存的方法，因为将来可能有很多进程挂接到这块共享内存上，以及进行一些其他操作，如果能唯一标识就能保证不出错，所以只要pathname和id是一样的那么生成的key就是一样的（当然你要保证原来的文件pathname对应的文件不能删除，不然会导致生成一个新的key值，访问到位置的区域），而shmget的返回值是交给用户的，就像文件描述符的，让用户便于去操作**

**5：演示**

结合fotk，我们就可以创建共享内存了

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 1
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%d\n",k);

  int shmid=shmget(k,SIZE,~~IPC_CREAT | IPC_EX~~ CL | 0666);
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  return 0;
}

```

运行之后可以发现输出了key值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406143054754.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
如果再次运行，它就会报错，这是因为咋们设置了`IPC_CREAT|IPC_EXCL`选项  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406143151797.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
`ipcs`命令是关于共享内存的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406143318570.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

其中ipcs \-m表示查看共享内存，可以发现其中id=30的便是我创建的共享内存  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406143933233.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

- 其实上图标题的那些类别分别就对应咋们前面源代码中说到的`ipc_perm`结构体的里的成员，这里不再详述  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406144054366.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

**管道的声明周期是随进程的，但是前面的演示中，大家可以发现虽然可执行文件也是一个进程，但是只要执行一遍后，它就会存在，除非系统重启或手动删除，说明共享内存的生命周期是跟随内核的**

删除共享内存的命令是`ipcrm`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040614432791.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
我们删除之前创建的共享内存  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406144418685.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

### C：shmctl（控制共享内存）

（这个函数主要用来删除，当然设置其他选项还可以做其他事情）

**1：头文件**

```c
#include <sys/ipc.h>
#include <sys/shm.h>
```

**2：函数原型**

```c
int shmctl(int shmid,int cmd,struct shmid_ds* buf);
```

**3：参数说明**

**shmid**：共享内存的id  
**cmd**：将要采取的动作，有三个选项

- `IPC_RMID`：删除共享内存（主要使用）
- `IPC_STAT`：将shmid\_ds结构体中的数据设置为共享内存的当前关联值
- `IPC_SET`：在进程有足够权限的前提下，把共享内存的当前关联值设置为shmid\_ds结构中给出的值

**buf**：这是一个结构指针，指向的就是`shmd_ds`结构体，不使用就传入NULL

**4：返回值**

**成功**：0  
**失败**：-1

**5：实验**

如下，向刚才的代码中加入删除共享内存的功能，在创建3s后，删除共享内存。利用如下脚本监控

```bash
while :; do ipcs-m | grep 4096;sleep 1;echo "########";done
```

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,IPC_CREAT | IPC_EXCL|0666);
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  sleep(5);
  shmctl(shmid,IPC_RMID,NULL);
  sleep(3);
  return 0;
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406150634776.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

### D：shmat（将共享内存段与当前进程挂接）

**1：头文件**

```c
#include <sys/ipc.h>
#include <sys/shm.h>
```

**2：函数原型**

```c
void* shmat=(int shmid,const void* shmaddr,int shmflag);
```

**3：参数说明**

**shmid**：共享内存标识符  
**shmaddr**：指定的连接地址，设置为NULL即可  
**shmflag**：设置为0（默认值）即可

**4：返回值**

它会返回一个指针，**该指针指向那块共享内存的起始位置**，如果失败则返回-1

### E：shmdt（将共享内存段与当前进程脱离）

**1：头文件**

```c
#include <sys/ipc.h>
#include <sys/shm.h>
```

**2：函数原型**

```c
int shmdt(const void* shmaddr);
```

**3：参数说明**

**shmaddr**：就是shmat的返回值

**4：返回值**

**成功**：0  
**失败**：-1

**5：实验**

现在结合`shmat,shmdt`以及前面的代码，就可以将本进程与共享内存进行挂接，使用刚才的脚本进行监控

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,IPC_CREAT | IPC_EXCL|0666);
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  sleep(5);//5s后与共享内存进行挂接
  void* shmaddr=shmat(shmid,NULL,0);
  sleep(3);//挂接3s后将本进程与共享内存脱离
  shmdt(shmaddr);
  sleep(3);//脱离后3s释放共享内存
  shmctl(shmid,IPC_RMID,NULL);
  sleep(3);//释放后结束进程
 
  return 0;
}

```

效果如下，可以看见其连接数的变化情况  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406153328519.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
上面只是挂接了本进程（`server.c`），现在将另外一个进程也挂接上（`client.c`）。需要注意，下一个进程在挂接时，对于`shmget`最后一个参数可以传入0，表示接受到主进程创建的那个共享内存，并且释放共享内存不需要它进行

`client.c`的代码如下

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,0);//服务端已经申请了，写成0直接获取
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  sleep(5);//5s后与共享内存进行挂接
  void* shmaddr=shmat(shmid,NULL,0);
  sleep(3);//挂接3s后将本进程与共享内存脱离
  shmdt(shmaddr);
  sleep(3);//脱离后3s释放共享内存
  return 0;
}
```

效果如下，可以看见它们依次挂接，然后依次脱离，最后释放  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406154804292.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)

## （4）演示-共享内存实现客户端和服务端的通信

介绍完如上接口后，现在我们使用共享内存完成客户端和服务端之间的通信，为了说明一些情况，**我们让客户端每隔5s向内存中依次序输入小写字母，而服务端每隔1s读取一次**。**其中注意，shmat的返回值是一个void类型的，所以要打印字符，就要强转为char**\*

`server.c`代码

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,IPC_CREAT | IPC_EXCL|0666);
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  char* shmaddr=shmat(shmid,NULL,0);//挂接,注意强转
  while(1)//每1s打印一次
  {
            
            
      sleep(1);
      printf("%s\n",shmaddr);
  }

  shmdt(shmaddr);//脱离
  shmctl(shmid,IPC_RMID,NULL);//释放
 
  return 0；
}

```

`client.c`代码

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <unistd.h>
#include <sys/shm.h>

#define PATHNAME "tmp"
#define PROJ_ID 88
#define  SIZE 4096

int main()
{
            
            
  key_t k=ftok(PATHNAME,PROJ_ID);
  printf("key值：%#X\n",k);

  int shmid=shmget(k,SIZE,0);//服务端已经申请了，写成0直接获取
  if(shmid<0)
  {
            
            
    perror("creat failed");
    return 1;
  }
  char* shmaddr=shmat(shmid,NULL,0);//挂接，注意强转
  int i=0;
  while(i<26)
  {
            
            
    shmaddr[i]=97+i;每隔5s依次输入a,b,c...........................
    i++;
    sleep(5);

  }
  shmdt(shmaddr);//脱离
  return 0;
}

```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210406161852195.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115354952)  
从上面的图中可以看出，服务端是每隔1s的读取的，而客户端是每隔5s输入一次。也就是写端慢，读端快，按照管道中的逻辑，读端将会发生阻塞，**但是在共享内存这里并没有出现读端堵塞的情况，这是因为共享内存底层不提供任何同步和互斥的机制。** 这说明两个进程根本就不知道对方的存在

**所以服务端一旦写入数据，客户端立马就可以看到，而不存在管道中的那种缓冲区导致发生发生多次拷贝的现象，所以共享内存也是最快的进程间通信的方式**

## （5）补充

1.  共享内存被删除后，则其他进程无法通信，这句话是有逻辑错误的，因为共享内存只有连接数全部为0时，才会真正删除