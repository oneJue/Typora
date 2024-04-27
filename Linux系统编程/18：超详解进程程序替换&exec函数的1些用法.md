 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）进程程序替换是什么](#1_4)
  - [（2）exec-替换函数](#2exec_9)
  - [（3）实例展示-了解exec函数的替换原理](#3exec_28)
  - - [A：execl和execv](#Aexeclexecv_31)
    - [B：execlp和execvp](#Bexeclpexecvp_71)
    - [C：替换自己的程序和execle](#Cexecle_104)
    - [D：execve](#Dexecve_182)

## （1）进程程序替换是什么

之前我们说过，子进程与父进程共享代码，也就是说代码是共有的，**但是如果想要让子进程去执行另外的程序，而不是父进程原有的程序，该怎么办？**

**所以要完成这样的操作，子进程就要调用名字叫作`exec`的函数（共有6个，都以`exec`开头）去执行另外一个程序，** **进程调用`exec`函数时，该进程的用户空间代码和数据会完全被新程序代替，但是需要注意这并不意味着创建了一个新的进程，因为调用`exec`后进程PID并没有改变**

## （2）exec-替换函数

`exec`函数共有6个，均以`exec`开头，如下

```c
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ...,char *const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);

int execve(const char *path, char *const argv[], char *const envp[]);
```

这6组看起来的确有点混乱，但是只要抓住关键点就很容易理解，除了exec外，其余涉及到的字符有`‘l’`,`‘v’`,`'p'`,`'e'`，它们各自含义如下

- `l(list)`：表示参数要用列表的形式挨个输入
- `v(vector)`：表示参数全部装到数组里，然后把数组传过去
- `p(path)`：如果有`'p'`。表示会自动搜索环境变量`PATH`，没有`p`就得自己输入程序的完整路径名了
- `e(env)`：表示自己维护环境变量

## （3）实例展示-了解exec函数的替换原理

在Linux中，所有命令的都是程序，所以以下的例子中为了清晰展现替换函数的原理，就不进行分流操作了，直接使用main函数，然后替换的程序就使用最简单的`ls`命令

### A：execl和execv

**`execl`函数没有带`p`，所以对于`ls`，必须输入其完整路径名，然后传入两个参数`-a，-l`，模拟`ls \-a \-l`。这里需要注意的是第二个参数要先写上`ls`，后面的参数再跟其他的，因为第一个参数只是路径名，而且需要注意的是要用NULL作为结尾，表示参数部分我已经写完了**

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  printf("替换函数前\n");
  execl("/usr/bin/ls","ls","-a","-l",NULL);
  printf("替换函数后\n");

}


```

执行效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316163932150.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)  
结果和我们预期想的一样，我的这个程序最终展现出了`ls \-a \-l`的效果。但是细心的读者可能会发现一个非常奇怪的现象，**就是原本程序代码的“替换函数后”这样的文字怎么没有被打印出来？**这其实也是替换函数的核心，**main函数启动后就成为了一个进程，执行到execl后直接把ls程序的代码整体全部搬了过来，也就是它直接不要main函数的代码了，直接跑去ls的代码了，所以文字没有输出**

**所以exec函数是没有返回值的，一旦执行失败，它就不去进行程序替换了，所以下面我故意把上面程序的路径名写错，后面的文字也就输出了**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316164402573.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)

前面说过execl和execv的区别就是传入参数的方式不同，所以稍加改动也能实现和execl相同的效果

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  printf("替换函数前\n");
  char* argv[]={
            
            "ls","-a","-l",NULL};
  execv("/usr/bin/ls",argv);
  printf("替换函数后\n");

}
```

### B：execlp和execvp

根据前面的描述，如果有p，它就会自动查找环境变量，所以不需要我们输入全路径  
因此以`execvp`为例

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  printf("替换函数前\n");
  char* argv[]={
            
            "ls","-a","-l",NULL};
  execvp("ls",argv);
  printf("替换函数后\n");

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316165301941.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)  
`execlp`这样写，很多人会感觉到这样的写法非常奇怪，其实每个`ls`的含义是不同的

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  printf("替换函数前\n");
  execlp("ls","ls","-a","-l",NULL);
  printf("替换函数后\n");

}
```

### C：替换自己的程序和execle

上面替换的程序都是系统的程序，这里我写一个我的程序叫做`myprocess.c`，编译好的名字叫做`myprocess.exe`，主要功能是每隔1s在屏幕上打印一段文字，共打印5s

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
            
            
  int count=0;
  while(count<5)
  {
            
            
    printf("Hello World\n");
    sleep(1);
    count++;
  }
 return 0;
}

```

然后在我们测试文件中去替换程序

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
            
            
  printf("替换函数前\n");
  execl("./myprocess.exe","myprocess.exe",NULL);
  printf("替换函数后\n");

}

```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316191200972.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)

`execle`它的最后一个参数是主程序传给被调进程的，比如下面在`test.c`中自己组装了一个环境变量叫做`myenv`，然后在`execle`中传入字符数组，然后在`myprocess.c`中获得这个环境变量  
代码如下

```c
//myprocess.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
            
            
  int count=0;
  while(count<5)
  {
            
            
    printf("Hello World\n");
    sleep(1);
    count++;
  }
  printf("%s\n",getenv("myenv"));
 return 0;
}

//test.c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
int main()
{
            
            
  char* env[]={
            
            "myenv=you_can_see_this_env",NULL};
  printf("替换函数前\n");
  execle("./myprocess.exe","myprocess.exe",NULL,env);
  printf("替换函数后\n");

}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316193713829.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316193747373.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)  
之前咋们main函数的第三个参数其实就是环境变量，所以也可以把它传过去  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316194025689.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316194124605.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)

### D：execve

为什么要把`execve`放到最后呢，因为它才是终极大BOOS。**只有`execve`才是系统调用，其他都是根据用户的需求而封装的不同的形式，最终还会调用`execve`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210316194901605.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189565)