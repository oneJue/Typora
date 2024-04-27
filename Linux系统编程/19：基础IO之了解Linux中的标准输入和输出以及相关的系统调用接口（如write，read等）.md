 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- [一：标准输入，标准输出和标准错误](#_6)
- - [（1）回忆C语言写文件](#1C_7)
  - [（2）stdin，stdout和stderr](#2stdinstdoutstderr_32)
- [二：读写文件新的系统调用接口](#_45)
- - [注意：库函数和系统调用接口的关系](#_53)
  - [（1）：open基本情况介绍](#1open_61)
  - [（2）：close基本情况介绍](#2close_99)
  - [（3）：read基本情况介绍](#3read_115)
  - [（4）：write基本情况介绍](#4write_139)
  - [（5）：演示](#5_161)

# 一：标准输入，标准输出和标准错误

## （1）回忆C语言写文件

学习C语言时，将数据写入到一个文件中可能会用到下面这种代码

```c
#include <stdio.h>
#include <string.h>

int main()
{
            
            
  FILE* fp=fopen("myfile","w");
  if(fp==NULL)
  {
            
            
    printf("open error\n");
  }
  const char* text="Hello World\n";
  fwrite(text,strlen(text),1,fp);

  fclose(fp);
  return 0;


}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322085424181.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232764)

## （2）stdin，stdout和stderr

可以看到上图中`fwrite(text,strlen(text),1,fp)`，其输出流选择的是文件指针指向的文件`myfile`，所以`myfile`的内容被写入进去了

**仔细思考：为什么使用printf会将内容打印到屏幕上，为什么scanf接受的是键盘上的输入的内容？其实C默认会打开三个输入输出流，分别是标准输入`（stdin）`，标准输出`（stdout）`和标准错误`（stderr）`，在Linux中一切皆文件，所以如果将fwrite的输出流换成stdout，也就是`fwrite(text,strlen(text),1stdout)`，那么程序运行后，内容将被写到标准输出也就是屏幕上**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322090036458.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232764)  
**所以stdin，stdout和stderr也是文件，因为形参的位置就是一个文件指针嘛**

我在[学习shell之重定向\(点击跳转\)](https://blog.csdn.net/qq_39183034/article/details/114476945)这篇文章中仔细探讨过标准输入，标准输出和标准错误，现在搬运过来

> 一个命令或程序，按下回车键后，要么会显示程序运行的结果，要么会显示状态和错误信息。以ls为例，当按下ls命令后，它会把其运行结果发送到一个称为标准输出\(stdout\) 的特殊文件中，其状态信息则会发送到一个称为 标准错误（stderr） 的文件中。标准输出和标准错误都将会被链接到屏幕上，然后输出，它不会保存在磁盘中我们都知道命令是通过键盘输入给电脑的，这个键盘叫做的标准输入（stdin）在默认情况下，标准输入和标准输出都是按照这样的逻辑进行的，而I/O重定向功能可以改变输出内容的发送目的（也就是不让你发送到屏幕上），也可以改变输入内容的来源地（也就是说甚至可以来自于文件）

# 二：读写文件新的系统调用接口

**读写文件使用到的系统调用接口如下**

- `open`：打开文件
- `read`：在打开的文件中读取数据
- `write`：在打开的文件中写入数据
- `close`：关闭已经打开的文件

## 注意：库函数和系统调用接口的关系

说起fopen，fclose等相关库函数大家可能再熟悉不过了吗，因为在C语言中读写文件中经常会使用到他们，而上面说到的这些`open`,`close`大家可能很少接触。其实**fopen和open之间就是库函数和系统调用接口的关系**，关于他们的关系，我在[Linux进程（点击跳转）](https://blog.csdn.net/qq_39183034/article/details/114284873)这篇文章中做过详细介绍

> 从开发角度上看，操作系统会对外表现为一个整体，但是会暴露自己的部分接口，以供上层开发者使用——系统调用。系统调用在使用上功能比较繁琐，对用户水平要求也比较高。所以，一些开发者会将部分系统调用进行适度封装，从而形成库，有了库，就便于上层用户使用或者开发者进行二次开发。  
> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210322091638992.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232764)

**也就是说fopen是建立在open之上的，fopen在底层仍然会调用open。open只有一个，但是它的变种形式可能无穷无尽，比如zopen，copen，都是为了适应不同的开发，使用需求而适度封装的产物。**

## （1）：open基本情况介绍

**1：头文件**

```c
#include <sys/type.h>
```

**2：函数原型**

```c
int open(const char* pathname,int flags);
int open(const char* pathname,int flags,mode_t mode);
```

**3：参数说明**

**pathname**：要打开或创建的目标文件名  
**flags**：打开文件时可以传入多个参数

- `O_RDONLY`：只读打开
- `O_WRONLY`：只写打开
- `O_ROWR`：读写打开
- _\(上面这三个是常量，必须指定且只能指定一个\)_
- `O_CREAT`：**若文件不存在，则创建。需要使用mode选项，指明权限**
- `O_APPEND`：追加

**4：返回值**

**成功**：返回新打开的文件描述符  
**失败**：返回-1

**5：举例**

以只读方式打开，若不存在则创建，指定权限为0644

```c
int fd=open("myfile",O_RONLY|O_CREAT,0644)
```

## （2）：close基本情况介绍

**1：头文件**

```c
#include <unistd.h>
```

**2：函数原型**

```c
int close(int fd)
```

**3：返回值**

**成功**：返回0  
**失败**：返回-1

## （3）：read基本情况介绍

**1：头文件**

```c
#include <unistd.h>
```

**2：函数原型**

```c
ssize_t read(int fd,void* buffer,size_t count);
```

**3：参数说明**

**fd**：文件描述符  
**buffer**：读取的内容会送入这个缓冲区  
**count**：读取文件的大小

**4：返回值**

**正常情况**：返回读取到的字节数  
**读到EOF**：返回0  
**异常情况**：返回-1

## （4）：write基本情况介绍

**1：头文件**

```c
#include <unistd.h>
```

**2：函数原型**

```c
ssize_t write(int fd,const void* buffer,size_t count);
```

**3：参数说明**

**buffer**：数据来源  
**count**：你期望写入的字节数

**4：返回值**

**成功**：返回成功写入的字节数  
**失败**：返回-1

## （5）：演示

演示一：以只写方式打开log.txt吸入字符串（在C语言中我们的字符串是以`'\0'`结尾的，但是在用strlen求写入的字符串长度时不要+1，因为你写入进去的是文件，和字符串没有关系）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328150731221.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232764)  
演示二：在上面的情况下，添加追加选项，以此不断运行程序字符串会被追加在文件后面  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328151356983.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232764)