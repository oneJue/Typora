 

### 文章目录

- - [（1）预备工作](#1_1)
  - [（2）具体实现](#2_19)
  - - [A：写一个自己的提示符](#A_20)
    - [B：接受命令](#B_25)
    - [C：解析命令](#C_32)
    - [D：创建子进程](#D_47)
    - - [①：如果进程创建失败](#_52)
      - [②：子进程该做什么](#_55)
      - [③：父进程该做什么](#_59)
    - [E：测试](#E_63)
    - [F：增加重定向功能](#F_66)
  - [（3）代码](#3_125)

## （1）预备工作

**1：更换默认shell提示符及颜色**

由于咋们自己设计的shell其实是“偷梁换柱”，也就说是一个程序，为了运行的时候和默认的shell做以区别，这里修改一下提示符和颜色。相关修改方法我已经在下面的文章中提到过了，有兴趣的读者可以自行查阅

> [定制shel提示符](https://blog.csdn.net/qq_39183034/article/details/114776612)

修改效果如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321182505394.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)  
**2：实现逻辑**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321182616785.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

3：复习strtok函数

**为了去解析命令行输入的命令，需要用到`strtok`函数、`strtok`作用就是以给定的标记分割字符**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321182804931.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321182823100.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

## （2）具体实现

### A：写一个自己的提示符

一个shell首先必备的就是提示符，这里咋们顶一个字符串，每次输入命令时都让其出现  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321183510368.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321183616137.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

### B：接受命令

这一步就需要从命令行上接受命令，由于咋们是用键盘输入的，所以使用`fgets`，来源是标准输入流  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321184934565.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321184149604.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

### C：解析命令

我们把命令输入到了字符数组`commnd`中，那么进程替换函数就可以选择`execv`或者`execvp`了，但是不能直接传入`commnd`，因为这写命令目前是一个整体。**而且`execv`要传入的是一个字符指针数组，所以建立一个字符指针数组，数组的每个指针指向`commnd`这个字符数组的每一条命令，那么怎么分割这些命令呢，就要使用到`strtok`函数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321190047372.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321190010243.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

上图基本正确的输出了所有的参数，但是有一个非常大的问题，**就是最后输出了一个换行符，这是因为`fgets`会把换行符读入**，这一点问题比较大，会导致后面的参数错误，**所以`commnd`数组中最后一个字符是`'\n'`，必须将其换成`'\0'`**  
输入

```cpp
commnd[strlen(commnd)-1]='\0'
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321190600176.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

### D：创建子进程

截止目前为止，所有的代码都在一个进程中。如果不创建子进程，直接在这个进程中去进行进程替换，如果替换成功，那么执行完也就结束了，**这就相当于你的shell给挂了**

**所以我们要让子进程去执行进程替换，让父进程（就是你的shell）去等待执行结果，不管成功与否，还要接受下一次输入。**

#### ①：如果进程创建失败

加入创建失败，那么利用`perror`函数，将其输入到标准错误中，然后利用`continue`，继续回到while等待下一次输入  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321192841895.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

#### ②：子进程该做什么

子进程就应该去进程替换，需要注意的是如果进程替换成功，也即是正确的输入命令，那么最后的返回码是0，一旦输入命令错误，进程替换失败，将会继续执行子进程的剩余代码，**所以子进程的返回码设置为1，表示进程替换失败了**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321193031723.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

#### ③：父进程该做什么

**父进程阻塞等待子进程，当子进程进程替换成功，或者由于命令输入错误而执行到`exit`，此时父进程会读取子进程，利用变量`st`，读取子进程返回状态，利用其退出码判断是否进程替换成功，如果其退出码是1，告诉用户命令错误，让其再次输入，否则成功执行**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210321193307944.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

### E：测试

测试效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032119360024.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114799947)

### F：增加重定向功能

需要注意的是这部分要学习完[Linux之基础IO](https://blog.csdn.net/qq_39183034/article/details/115061389?spm=1001.2014.3001.5501)才能实现

实现时我们只需建立一个函数，去找寻是否有重定向的符号，然后进行分割即可

```cpp
void redirect(char* str)
{
            
            
	//   ls -l > test.txt
	//处理开头空格问题：
	while(*str==' ')
	{
            
            
		str++;
	}
	
	char* file;//指向文件的字符串
	int flag=0;//标记是哪一种重定向
	int fd=-1;
	
	
	char* ret1=strstr(str,">");//用strstr去找寻根据逻辑关系判断
	char* ret2=strstr(str,">>");
	char* ret3=strstr(str,">>>");
	if(ret1==NULL)//没有重定向
	{
            
            
		flag=0;
		return;
		
	}
	else if(ret1!=NULL && ret2==NULL)//覆盖重定向
	{
            
            
		flag=1;
		*(ret1-1)='\0';
		file=ret1+2;
		
	}
	else if(ret2!=NULL && ret3==NULL)//追加重定向
	{
            
            
		flag=2;
		*(ret1-1)='\0';
		file=ret1+3;
		
	}
	
	if(flag==1)
	{
            
            
		fd=open(file,O_CREAT | O_TRUNC | O_WRONLY,0644);
		
	}
	if(flag==2)
	{
            
            
		fd=open(file,O_CREAT | O_APPEND | O_WRONLY,0644);
	}
	
	dup2(fd,1);
	close(fd);
}
```

## （3）代码

代码如下

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main()
{
            
            
  char commnd[256];//用于接受命令
  const char* Myshell="[Myshell@zhangxing]$:";//我的命令提示符
  while(1)
  {
            
            
    commnd[0]=0;//重置命令数组
    printf("%s",Myshell);//显示命令提示符
    fgets(commnd,256,stdin);//由键盘输入命令
    commnd[strlen(commnd)-1]='\0';//最后一个字符本来是换行符，注意换成结束标志，否则参数是错误的

    char* argv[16];//用来传入进程替换函数的数组
    argv[0]=strtok(commnd," ");//下面是strtok的基本用法
    int i=1;
    do 
    {
            
            
      argv[i]=strtok(NULL," ");
      if(argv[i]==NULL)//execv最后一个参数是以NULL结尾的 
        break;
      ++i;
    }while(1);
  
    
    //创建子进程
    pid_t id=fork();
    if(id<0)
    {
            
            
      perror("子进程创建失败\n");
      continue;//进程一旦创建失败，返回while继续执行
    }
    if(id==0)
    {
            
            
      execvp(argv[0],argv);
      exit(1);//程序替换失败，一定会执行到这里

    }
    int st;
    pid_t ret=waitpid(id,&st,0);//父进程等待子进程，阻塞
    if(ret==id)//等待成功
    {
            
            
      if(((st>>8)&0xff)==1)
      {
            
            
        printf("调用失败，有可能是命令输入错误，请重新输入，返回码是%d\n",(st>>8)&0xff);
      }
      else 
      {
            
            
        printf("调用成功，返回码是%d\n",(st>>8)&0xff);
      }
    }
  }
  return 0;
}

```