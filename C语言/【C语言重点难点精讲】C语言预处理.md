 

### 文章目录

- [一：C/C++程序程序编译过程](#CC_2)
- - [（1）预处理](#1_4)
  - [（2）编译](#2_12)
  - [（3）汇编](#3_21)
  - [（4）链接](#4_29)
- [二：宏定义](#_51)
- - [（1）数值宏常量](#1_52)
  - [（2）字符串宏常量](#2_55)
  - [（3）使用宏充当注释](#3_58)
  - [（4）使用宏充当表达式](#4_62)
- [三：宏其他](#_105)
- [四：条件编译](#_168)
- - [（1）#ifdef和#ifndef](#1ifdefifndef_169)
  - [（2）#if](#2if_206)
  - [（3）文件包含](#3_269)

# 一：C/C++程序程序编译过程

## （1）预处理

- 预处理主要包括**宏定义，文件包含，条件编译，去注释**
- 输入`gcc -E hello.c -o hello.i`，其中选项E作用是让gcc在预处理后停止编译  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195123511.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200856214.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

## （2）编译

- 此阶段，gcc检查代码的**规范性，是否具有语法错误**
- 输入`gcc -S hello.i -o hello.s`，即可将预处理里的结果继续继续编译  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195406753.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200956550.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

## （3）汇编

- 编译阶段无误后，进入汇编，**将“.s”文件转化为“.o”二进制文件**
- 输入`gcc -c hello.s -o hello.o`，即可将编译停止在此阶段

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128195837234.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)  
（打开二进制文件使用`od`命令）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128201057806.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

## （4）链接

- 此阶段，将目标文件与系统库进行链接生成可执行文件。
- 输入`gcc hello.o -o hello`，则完成编译

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128200250407.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

| 选项 | 描述 |
| --- | --- |
| \-E | 进行预处理，不进行编译，汇编和链接 |
| \-S | 进行编译，不进行汇编和链接 |
| \-c | 进行汇编，不进行链接 |
| \-o | 链接 |
| static | 采用静态链接 |
| \-g | 生成调试信息 |
| \-shared | 使用动态库 |
| \-O0 | 无优化 |
| \-O1 | 默认优化级别 |
| \-O3 | 优化最高 |
| \-w | 不生成警告信息 |
| \-Wall | 生成所有警告信息 |

# 二：宏定义

## （1）数值宏常量

略

## （2）字符串宏常量

略

## （3）使用宏充当注释

先去掉注释，再进行宏替换

## （4）使用宏充当表达式

```c
#define SUM(X) (X)+(X)

int main()
{
            
            
	printf("%d\n", SUM(10));
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3cf848ab0cd14d4c8607130462cfb05f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

需要注意使用宏充当多条语句时有可能出现一些潜在的问题，比如下面这个宏有两条语句，但是在`if`后面如果直接跟上宏而没有带花括号就会出现问题

```c
#define INT_VAL(a,b) a=0;b=0
```

如果需要宏替换大块语句，可以使用`do-while(0)`结构

```cpp
#define GIVE_VALUE(a,b) do{
              
              a=0;b=0;}while(0)

int main()
{
            
            
	int x = 10;
	int y = 20;
	int flag = 0;
	scanf("%d", &flag);
	printf("before:%d,%d\n", x,y);
	if (flag)
		GIVE_VALUE(x, y);
	else
		x = 10, y = 20;
	printf("after:%d,%d\n", x, y);

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F65a5065a50374c458d4a08ccbe8dcf7b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

# 三：宏其他

**第一：** 宏定义在文件的任何位置都可以，只要是在你使用这个宏之前定义就可以

```c
void show()
{
            
            
#define M 10
	printf("show:%d\n", M);
}
int main()
{
            
            
	show();
	printf("main:%d\n", M);
	return 0;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F906a695866e44aaabe9f75abbb9a7973.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

**第二** 可以使用`#undef`来结束宏的作用

```c
int main()
{
            
            
#define M 10//从这里开始
	printf("main:%d\n", M);
	printf("main:%d\n", M);
	printf("main:%d\n", M);
#undef M//到这里结束
	printf("main:%d\n", M);//错误

	return 0;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffa6b00eb8d1c4d2aa4a1aeed1a77dfb7.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

**第三：有如下宏定义**

```c
#define foo(4+foo)
```

按照一般理解 + foo\)会展开成\(4 + \(4 + foo\)\)，然后一直展开下去，直至内存耗尽。但是，**预处理器采取的策略是只展开一次** 。也就是说，foo只会展开成\(4 + foo\)，而展开之后foo的含义就要根据上下文来确定了。

对于以下的交叉引用，宏体也只会展开一次

```c
#define x(4+y)
#define y(2+x)
```

其中x展开成\(4+y\)->\(4+\(2\*x\)\)，y展开成\(2 \* x\) \-> \(2 \* \(4 + y\)\)。

```c
#include <stdio.h>
#define N 2
#define M N + 1
#define NUM (M + 1) * M / 2
main() {
            
             printf("%d\n", NUM); }
```

宏只是字符串替换，并没有计算能力，所以最后的结果是\(2+1+1\)\*2+1/2

# 四：条件编译

## （1）#ifdef和#ifndef

- `ifdef`：判定宏是否被定义，宏为真假没有关系，倾向于**要定义**，
- `ifndef`：判定宏是否没有定义，宏为真假没有关系，倾向于**不要定义**

下面的例子中没有定义`PRINT`，**所以代码只会保留`#else`这一部分,代码在预处理时直接被裁掉**

```cpp
int main()
{
            
            
#ifdef PRINT//实际未定义
	printf("hello\n");
#else
	printf("no print\n");
#endif
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd40b8c41f3bf47d7be676099bb4849f7.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)  
如果`PRINT`被定义了，然后换成`ifndef`，效果也是一样的

```c
#define PRINT//宏被定义
int main()
{
            
            
#ifndef PRINT//如果没有定义
	printf("hello\n");
#else
	printf("no print\n");
#endif
	printf("continue\n");
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F344e2bd14a5c4c5a81b32501b40e6150.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

## （2）#if

- `#if`：判定是否存在，以及是否为真，倾向于**存在且为真**

如下宏`PRINT`，被定义了但是为0，因此会输出`#else`里面内容

```c
# define PRINT 0//定义了但是为假

int main()
{
            
            
#if PRINT
	printf("hello\n");
#else PRINT
	printf("other");
#endif
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3e68c177a1d24eb2ab00f805171ec9ea.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

`#if`可以进行多分支控制

```c
#define HELLO 2

//注意这里使用的是==，像!=，<，>等可以使用
int main()
{
            
            
#if HELLO==1
	printf("hello1\n");
#elif HELLO==2
	printf("hello2\n");
#elif HELLO==3
	printf("hello3\n");
#else
	printf("other\n");
#endif
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8336ffa08cc54f8bb6be9c94e558136d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)  
如果要判断多个宏，可以这样进行

```c
#define Condition1 1
#define Condition2 0

int main()
{
            
            
#if Condition1 && Condition2
	printf("all\n");
#else
	printf("other\n");
#endif
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F405eedbd299b4d489746b75d0210207a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120779589)

## （3）文件包含

有一个头文件`test.h`，为了防止其被重复包含，我们通常的做法就是，在该文件内写上这样的语句。现在看来效果很明显，在第一次包含时#define会被立即被定义，第n次包含时由于\_TEST\_H被定义了，所以就不会执行

```c
#ifndef _TEST_H
#define _TEST_H


#endif // !_TEST_H
```

- 其实头文件在展开时，本质就是将其内容经过一些处理后拷贝到了目标源文件