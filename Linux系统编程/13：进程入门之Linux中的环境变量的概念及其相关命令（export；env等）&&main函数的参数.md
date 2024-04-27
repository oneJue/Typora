 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）为什么你的程序不能直接执行？](#1_5)
  - [（2）环境变量](#2_14)
  - [（3）查看/设置环境变量](#3_28)
  - [（4）和环境变量相关的命令总结](#4_39)
  - [（5）通过代码获取环境变量](#5_47)
  - - [A：main函数的前两个参数](#Amain_48)
    - [B：main函数的第三个参数](#Bmain_102)
  - [（6）环境变量的全局属性](#6_162)

## （1）为什么你的程序不能直接执行？

我们知道在Linux编写C/C程序，编译完成之后，如果需要运行这个程序，需要加上`./`，表示当前路径下。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307201959806.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
在Linux下，一切皆文件，因此像我们熟知的ls，mkdir这些命令都存在与`/usr/bin`目录下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030720210356.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

但是问题就在于这：为什么都是程序，都是命令，像ls命令就不需要类似`/usr/bin/ls`这样的方式去执行，能够直接执行，而我的命令则必须输入`./`，否则就报错？  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307202313106.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
**这一切的一切都和环境变量有关**

## （2）环境变量

说人话：**每当遇到一个命令，环境变量中保存了一些目录，可以告诉操作系统可以去我所规定的目录里寻找，操作系统如果能找见就去执行，如果找不见则返回错误**

为什么要有环境变量：**通俗解释就是每次敲命令，都要带上一长串路径，实在太麻烦了。**

**常见的环境变量如下**：

- PATH：指定命令的搜索路径
- HOME：指定用户的主工作目录（这也就是为什么我们每次登陆系统时默认目录会是用户目录）
- SHELL：当前shell，它的值通常是`/bin/bash`

## （3）查看/设置环境变量

方法是`echo $环境变量名字`，比如`echo $PATH`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307203045273.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

- 这些环境变量以冒号为分割，每一个表示一个目录

**因此通过上述描述，如果想要让系统也能直接执行我的程序，那么我只需将我的可执行程序的目录添加到环境变量即可**  
添加方法是：`export PATH=$PATH:你的程序所在目录`，**千万不要忘记\$，不然环境变量会被覆盖！！！**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307203557688.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

- 如上，当添加到PATH后，不强制`./`的方式运行我的程序了

## （4）和环境变量相关的命令总结

- echo：显示环境变量值
- export：设置一个新的环境变量
- env：显示所有环境变量
- unset：清除环境变量
- set：显示本地定义的shell变量和环境变量

## （5）通过代码获取环境变量

### A：main函数的前两个参数

在Windows写C/C++久了，我们经常会忽略一点，认为main函数是没有参数的

```c
#include <stdio.h>
int main()
{
            
            
	printf("Hello World\n");
	return 0;
}
```

但是虽然没有实际写过main函数的参数，大部分人还是知道main函数是有参数的，确切点将有两个参数。但是为什么会有参数，这一点在Windows中是无法讲清的。

首先请大家思考一点：在Linux中我所编译的程序和系统的程序是否本质是一样的？答案是肯定的，**但是问题就在于为什么系统的程序可以带有命令行参数，而我的程序却不能带参数，或者说根本不知道有什么参数**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308093836357.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
答案是main函数有两个参数：`int argc`和`char* argv[]`，他们称为命令行参数，就是用来接受命令行传回的参数，然后根据参数的不同进行分流操作，具体作用如下

- `char* argv[]`：这是一个字符指针数组，该数组中的每个指针指向一个命令行参数
- `int argc[]`：这是字符指针数组的大小，或者说命令行参数的个数

**所以可以这样说，main函数接收到命令行参数，然后根据接收的参数与字符指针数组中，指针指向的字符进行比较，从而达到不同的参数实现不同的功能的操作**

为了测试这一点，编写如下C++文件，用于判断命令行参数，使用argument接收第一个命令行参数，并进行判断输出对应的命令行参数

```cpp
include <iostream>
#include <string>


using namespace std;
int main(int argc,char* argv[])
{
            
            
	 string argunment=argv[1];
	 for(int i=0;i<argc;i++)
	 {
            
            
		cout<<"argv["<<i<<"] : " << argv[i]<<endl;
	 }
	 if(argunment=="-a")
	 {
            
            
	   	cout<<"hello -a"<<endl;
	 }
	 else if(argunment=="-b")
	 {
            
            
	    cout<<"hello -b"<<endl;
	 }
	 else 
	 {
            
            
	    cout<<"hello -c"<<endl;
	 }
}
```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308095641212.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

### B：main函数的第三个参数

大部分人对main函数的了解也止步于main函数有两个参数，**但其实main函数是有第三个参数的，这个第三个参数和环境变量有关，称为环境变量参数。**  
也就是完整形式应该是这样的

```cpp
#include <stdio.h>
int main(int argc,char* argv[],char* env[])
{
            
            
	printf("hello world\n");
	return 0;
}
```

**环境变量的组织方式如下**  
每个程序都会收到一张环境表，环境表是一个字符指针数组，每个指针指向一个以`‘\0’`结尾的字符串  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307205236252.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
你可能会有疑问？第三个参数究竟是干什么的，关于第三个参数其实解释的没有前两个参数那么清楚，但是有一个问题相信大家死活都百思不得其解：**为什么安装软件的时候，程序一定都会默认安装到`C:\Program Files`目录下，这其实和第三个参数有关**

为了解答这个问题，我们先写一个小程序，根据上面的那张图，可以说明main函数的第三个参数一定也是这样一个数组，我们的目的就是将其打印出来

```c
#include <stdio.h>

int main(int argc,char* argv[],char* env[])
{
            
            
  int i=0;
  while(env[i]!=NULL)
  {
            
            
    printf("%d %s",i,env[i]);
    printf("\n");
    ++i;
  }
  return 0;

}
```

效果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308102816689.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

- 如果使用env查看，发现程序其实输出的就是环境变量

回到那个问题：为什么默认安装位置是`C:\Program Files`？其实安装程序会自动调用环境变量，找到系统的默认安装位置  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308103613176.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

 -    当然C/C++也提供了，查看环境变量的函数`getenv()`比如你要查看PATH，那么直接调用函数`getenv("PATH")`即可

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
            
            
  printf("%s\n",getenv("PATH"));
  printf("%s\n",getenv("PWD"));
  return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308135527691.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)

## （6）环境变量的全局属性

**环境变量通常具有全局属性，可以被子进程继承下去**

且看下面的这一段程序

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
            
            
	printf("%s\n",getenv("TEST"));
}
```

程序的运行结果也如你所料，什么都不会输出，因为根本就没有“TEST”这样的环境变量  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308140203296.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
和前面添加PATH环境变量一样，我在这里使用export导入一个名叫TEST的环境变量，并赋值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308140316271.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
再次运行这个程序，结果仍然如你所料，结果就是我赋的值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210308140427959.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189151)  
这里，我的程序是由bash执行的，前面说过bash是程序命令的父进程，，而刚才的设置环境变量正是利用bash设置的，那么我的程序所谓bash的子进程居然也能输出由bash创建的环境变量，**说明环境变量具有全局属性，可以被子进程继承**