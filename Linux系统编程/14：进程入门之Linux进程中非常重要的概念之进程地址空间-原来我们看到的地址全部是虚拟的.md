 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）旧知回顾](#1_4)
  - [（2）程序地址空间？](#2_39)
  - - [A：同一个地址有两个数据\?](#A_41)
    - [B：物理地址和虚拟地址](#B_83)
    - [C：进程地址空间及作用](#C_92)
    - [D：进程地址空间如何工作](#D_110)

## （1）旧知回顾

学习C/C++总免不了这张图  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308143051424.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)  
这张图帮助我们解决了不少C/C++问题，但是我们对它的认识还是不够深刻。现在我我们用一端C程序，深刻的取感受一下这个过程的内存地址的变化

```cpp
#include <stdio.h>
#include <stdlib.h>

int gobal_val=100;//全局变量已经初始化
int gobal_unval;//全局变量未初始化
int main(int argc,char* argv[],char* env[])
{
            
            
	printf("main函数处于代码段，地址为：%p,十进制为：%d\n",main,main);
	printf("\n");
	printf("全局变量gobal_val，地址为：%p,十进制为:%d\n",&gobal_val,&gobal_val);
	printf("\n");
	printf("全局变量未初始化gobal_unval，地址为：%p,十进制为:%d\n",&gobal_unval,&gobal_unval);
	printf("\n");
	
	char* mem=(char*)malloc(10);
	
	printf("mem开辟的堆空间，mem是堆的起始地址，是%p,十进制为：%d\n",mem,mem);
	printf("\n");	
	printf("mem是指针变量，指针变量在栈上开采，其地址为%p,十进制为：%d\n",&mem,&mem);
	printf("\n");	
	printf("命令行参数起始地址：%p,十进制为：%d\n",argv[0],argv[0]);
	printf("\n");
	printf("命令行参数结束地址：%p,十进制为：%d\n",argv[argc-1],argv[argc-1]);
	printf("\n");
	printf("第一个环境变量的地址：%p,十进制为：%d\n",env[0],env[0]);
	printf("\n");
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308151339316.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)

## （2）程序地址空间？

上面的图在学习C/C++时我们称之为程序的地址空间分布图，说白了就是一段程序在其运行过程中的数据的内存分布情况，但是接下来的叙述可能和你所认为的有所出入

### A：同一个地址有两个数据\?

如下C程序，有个全局变量val，初始值为0，使用fork创建一个子进程，然后让父进程先睡眠三秒，先让子进程运行，子进程运行时把val改为100，三秒后父子进程同时运行

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int val=0;
int main()
{
            
            
	pid_t id=fork();
	if(id==0)
	{
            
            
		val=100;
		while(1)
		{
            
            
			printf("现在是子进程：val=%d，其地址为%p\n",val,&val);
			sleep(1);
		}
	}
	else if(id>0)
	{
            
            
		sleep(3);
		while(1)
		{
            
            
			printf("############################################\n")
			printf("现在是父进程：val=%d，其地址为%p\n",val,&val);
			sleep(1);
		}
	}
	else
	{
            
            
		exit(-1);
	}
	sleep(1);
	return 0;

}
```

效果如下，**问题就在于同一个变量怎么会同时具有两个不同的值？**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308163301294.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)

### B：物理地址和虚拟地址

我们能够很明确一点：**同一片空间不可同时具有两个值**，就像现实生活中，你不可能同时再学校又同时在家。那么只有一种解释：你所看到并不是真实的，也就是说你所看到的地址并不是真实的物理地址，而是表象上的相同，他们对应的真实的物理地址肯定是不相同的,我们称这种地址为虚拟地址

实际上：我们使用C/C++看到的地址，全部都是虚拟地址，真实的地址用户看不到，而操作系统就负责将虚拟地址转换为物理地址

可以这样描述：**父进程启动（它的内存空间也是虚拟的不是真实的），然后遇见fork就创建了一个子进程，fork就把父进程的内存状态拷贝了给自己一份，如果val的值没有改变，那么操作系统本着不浪费一点空间原则，直接使用val这个虚拟地址所对应的物理地址就可以了。但是好景不长，子进程把val修改掉了，所以操作系统就把子进程val的虚拟地址所对应的物理地址进行了修改。从表面看起来好似地址没有变，但是真实情况早都变化了。**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030817103520.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)

### C：进程地址空间及作用

**1：早期直接访问物理内存的缺陷所在**

我们知道进程=进程控制块PCB+代码+数据，早期计算机，也就是没有进程地址空间的时候进程一旦启动，这些东西就会被全部装入内存，那么此时进程访问的就是真实的物理内存，如果画一张图，应该就是下面这样  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308200650693.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)  
但是这样防止有很大坏处：比如说**野指针**，放的这么近，一旦指针访问的别的进程的数据，这就出了大乱子了。还有进程在运行过程中，是会产生数据的，产生的数据一旦不能放在本进程后面，就要另外找内存去存放，**这样就会导致不连续的现象，也增加了异常访问的情况**

**2：进程地址空间的发明**

所以计算机设计者意识到了这种模式缺陷，想到了一种方法：增加一个中间层，利用中间层映射物理内存。程序访问内存时不直接访问物理内存，先访问中间层，如果中间层访问没有问题，那么操作系统就会将中间层映射到物理层，完成正常执行

**一个进程创建之后，操作系统会为这个进程分配一个专属于它的大小为4GB的虚拟进程地址空间（4GB是因为32位系统中，指针是4个字节），与它相对的是一片真实的物理地址空间，操作系统在映射虚拟内存时只会映射到那一片物理空间，而且需要特别注意这个虚拟空间并不是真的有4GB，它只是虚拟的** 。由于每一个进程都有自己的虚拟的进程地址空间，所以它只能访问自己的进程的数据，这样做实现了隔离，也就是进程之间的相互独立。并且把虚拟地址空间划分为这样，那样的区，这样的话也能解决数据的连续存放  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308202740437.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)  
**3：页表**  
不管怎样，程序运行一定要在物理内存上，所以如何映射成为了关键，，这种映射称为页表  
**映射时，让虚拟地址和物理地址一一对应，进程在虚拟地址上的地址就对应了物理内存上的某个地址。这样的话就不存在非法访问了，因为每个进程都有自己独立的进程地址空间，如果非法访问，页表上根本就找不到这样的地址，所以操作系统拒绝映射。“代码，字符串是只读的，数据是可读可写的”就是基于这个原因，虽然咋页表上能找到，但是对代码，字符串做了权限限制，一旦监测到这些数据类型要做一些写入操作，那么同样操作系统拒绝映射**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308205012474.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)

### D：进程地址空间如何工作

还是那个观点“先描述，再组织”，也就是说我们看见的那个经典的栈，堆分布图，其实本质也是一个结构体，这个结构体隶属于`task_struct`，叫做`task mm_struct`\(文件在mm\_types.h\)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308212505124.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)  
这张图可以说明task\_stuct和task mm\_struct的关系  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308212730933.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114284873)  
**申请空间的本质就是想内存索要空间，得到物理地址，然后在特定区域申请没有使用的虚拟地址，建立映射关系，再返回虚拟地址**