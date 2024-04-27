 

**必读**：

- C语言关键字是一个非常重要的话题，因为它能在相当的程度上将C语言的核心内容串联起来，起到一种提纲挈领的效果
- 下面的内容重点提及的是相应关键字特别值得注意的地方，这些地方是我们经常忽略的，而且考试也会经常涉及到
- 讲解这些关键字时默认大家都有C语言的基础，因此不会从0开始谈起

### 文章目录

- [一：auto关键字](#auto_45)
- [二：register关键字](#register_51)
- - [（1）存储器分级](#1_54)
  - [（2）register修饰变量](#2register_57)
- [三：static关键字](#static_71)
- - [（1）修饰全局变量和函数](#1_72)
  - [（2）修饰局部变量](#2_98)
- [四：sizeof关键字](#sizeof_141)
- [五：signed、unsigned关键字](#signedunsigned_173)
- [六：if、else](#ifelse_223)
- - [（1）关于C语言中bool类型](#1Cbool_229)
  - [（2）float与“零值”的比较](#2float_281)
  - [（3）if和else的匹配问题](#3ifelse_362)
- [七：switch-case组合](#switchcase_385)
- [八：do 、while 、for关键字](#do_while_for_466)
- [九：goto关键字](#goto_575)
- [十：void关键字](#void_596)
- [十一：return关键字](#return_644)
- [十二：const关键字](#const_687)
- [十三：volatile关键字](#volatile_794)
- [十四：extern关键字](#extern_809)
- [十五：struct关键字](#struct_813)
- [十六：Union关键字](#Union_848)
- [十七：enum关键字](#enum_890)
- [十八：typedef关键字](#typedef_902)
- [总结](#_987)
- - [（1）关键字分类](#1_988)

一般来讲，C语言一共有32个关键字（`C90`标准），当然`C99`后又新增了5个关键字，不过我们还是重点讨论这32个关键字

| 关键字 | 说明 |
| --- | --- |
| `auto` | 声明自动变量 |
| `short` | 声明短整型变量或函数 |
| `int` | 声明整形变量或函数 |
| `long` | 声明长整形变量或函数 |
| `float` | 声明浮点型变量或函数 |
| `double` | 声明双精度变量或函数 |
| `char` | 声明字符型变量或函数 |
| `struct` | 声明结构体变量或函数 |
| `union` | 声明共用数据类型 |
| `enum` | 声明枚举类型 |
| `typedef` | 用以给数据类型取别名 |
| `const` | 声明只读变量 |
| `unsigned` | 声明无符号类型变量或函数 |
| `signed` | 声明有符号类型变量或函数 |
| `extern` | 声明变量是在其它文件中正声明 |
| `register` | 声明寄存器变量 |
| `static` | 声明静态变量 |
| `volatile` | 说明变量在程序执行过程中可以被隐含地改变 |
| `void` | 声明函数无返回值或无参数，声明无类型指针 |
| `if` | 条件语句 |
| `else` | 条件语句否定分支（与if连用） |
| `switch` | 用于开关语句 |
| `case` | 开关语句分支 |
| `for` | 一种循环语句 |
| `do` | 循环语句的循环体 |
| `while` | 循环语句的循环条件 |
| `goto` | 无条件跳转语句 |
| `continue` | 结束当前循环，开始下一轮循环 |
| `break` | 跳出当前循环 |
| `default` | 开关语句中的“其它”分支 |
| `sizeof` | 计算数据类型长度 |
| `return` | 子程序返回语句，循环条件 |

# 一：auto关键字

一般来说，在代码块中定义的变量（也即局部变量），默认都是auto修饰的，不过会省略。但是一定要注意：**不是说默认的所有变量都是auto的，它只是一般用来修饰局部变量**

当然在C语言中，我们已经不再使用auto了，或者称其为过时了，但是在C++中却赋予了auto新的功能，它变得更加强大了。有兴趣请点击[2-6：C++快速入门之内联函数，auto关键字，C++11基于范围的for循环和nullptr](https://blog.csdn.net/qq_39183034/article/details/113837275?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163296614416780366558060%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=163296614416780366558060&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-113837275.pc_v2_rank_blog_default&utm_term=auto%E5%85%B3%E9%94%AE%E5%AD%97&spm=1018.2226.3001.4450)

# 二：register关键字

register意味寄存器

## （1）存储器分级

这个概念我们在计算机组成原理中讲得已经非常详细了，请点击：[\(计算机组成原理\)第三章存储系统-第一节：存储器分类、多级存储系统和存储器性能指标](https://blog.csdn.net/qq_39183034/article/details/119715863)

## （2）register修饰变量

可以看出，如果将变量放到寄存器中，那么效率就会提高。可以用register修饰的变量有以下几种

- **局部的**（全局变量会导致CPU寄存器长时间被占用）
- **不会被写入的**（写入的话就需要被写回内存，要是这样的话register就没有意义的）
- **高频需要被读取的**

如果要使用，不要大量使用，因为寄存器的数量有限。

另外还需要注意的一点是：**被register修饰的变量，是不能取地址的，因为它已经放在了寄存器中，地址会涉及到内存，但是可以被写入**  
当然这个register关键字现在也基本不会用了，因为如今的编译器优化已经很智能了，不需要你自己手动优化

# 三：static关键字

## （1）修饰全局变量和函数

我们知道全局变量（加入关键字`extern`声明）和函数都可以跨文件使用的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F809e0ab488df47e99a52d505d8f1b32a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe79beccf26fd46a0a1597d438668bd55.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

但是有一些应用场景中，我们不想让全局变量或函数跨文件访问应该怎么办呢？那么就可以使用`static`关键字

```cpp
static int g_value=100;//修饰staic后全局变量将不能跨文件使用
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F07bf592523c4401babdf617224832a99.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
**可以看出被`static`修饰的全局变量是不能被外部其他文件直接访问的，而只能在本文件内使用**

- 需要注意这里说的是直接访问，那意味着可以间接访问，比如通过函数的方式实现  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F36ef3039b6cc46629df300062b532705.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**同样，被`static`修饰的函数只能在本文件内访问，而不能在外部其它文件中直接访问**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F24c97c1643744bc18ce862f9ffb78dad.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

- 还是需要注意，这里是不能直接访问，并不是不能访问，比如可以通过函数嵌套的方式  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F08e1dac84f2547f09873fb5ed172ea20.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

static这种功能本质为了**封装**，因为我们可以把一些不需要或者不想要暴露的细节保护起来了，只提供一个功能函数个，该函数在内部调用它们即可，这样的话代码安全性也比较高

## （2）修饰局部变量

我们知道全局变量仅在当前代码块内有效，代码块结束之后局部变量会自动释放空间，因此下面代码的结果就会是这样  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb422f7d2e6bc4e8c9648f5376a473f3f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
**如果使用`static`修饰局部变量，会更改其生命周期，但其作用域不变**，如下当用static修饰后，变量i地址不变，且结果累加  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2120a65887c048048acc5f5de7ae447a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

`static`为什么可以更改局部变量的生命周期呢？**因为被static修饰的变量会将其从栈区移动到数据段**，当然这就涉及到了C/C++地址空间的问题了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff99970dfcb4b4d27a85e7e7a0e5e3127.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

查看实际地址

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

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F884cce47701a4bf79306e5d22592ccb7.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 四：sizeof关键字

**`sizeof`用于确定一种类型对应在开辟空间的时候的大小，注意它是关键字而不是函数**

它的基本用法就是下面这样，这我就不再多说了（注意Windows32位平台）

```cpp
int main()
{
            
            
	cout <<"char:" <<sizeof(char) << endl;
	cout << "short:" << sizeof(short) << endl;
	cout << "int:" << sizeof(int) << endl;
	cout << "long:" << sizeof(long) << endl;
	cout << "long long:" << sizeof(long long) << endl;
	cout << "float:" << sizeof(float) << endl;
	cout << "double:" << sizeof(double) << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F403c0f17264348dbb46147f795944822.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
特别注意，`sizeof`求一种类型大小的写法共有三种，特别**第三种**很多人认为是错误的，而考试就爱给你整这些犄角旮旯的东西

```cpp
int main()
{
            
            
	int a = 10;
	第一种：cout << sizeof(a) << endl;
	第二种：cout << sizeof(int) << endl;
	第三种：cout << sizeof a << endl;//这种写法其实也证明了sizeof不是函数
	cout << sizeof int << endl;//注意这种写法是错误的
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2abd83e473ab4639941d687af56331f1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 五：signed、unsigned关键字

这一部分需要涉及数据存储及原码反码等基础概念，请参照以下章节

- [\(计算机组成原理\)第二章数据的表示和运算-第二节1：定点数的表示（原码、反码、补码和移码）](https://blog.csdn.net/qq_39183034/article/details/119031289)
- [\(计算机组成原理\)第二章数据的表示和运算-第二节2：原码、反码、补码和移码的作用](https://blog.csdn.net/qq_39183034/article/details/119145959)

**第一点：** 需要深刻理解`signed`和`unsigned`只是对数据的一种解读方式，其中`signed`会把首位数据解读为符号位，符号位用于标识其正负，`unsigned`的首位也算作数据位,**也就是说类型决定了其读写的时候的解释方式**  
因此像下面的这样一句代码，看似不合适，但是它是没有问题的，因为存储时对于变量a它只关心我所开辟的空间上的二进制数据放进了没有，并不关心你之前是怎么样的

```cpp
unsigned int b=-10;
-10的原码：1000 0000 0000 0000 0000 0000 0000 1010
-10的反码：1111 1111 1111 1111 1111 1111 1111 0101
-10的补码：1111 1111 1111 1111 1111 1111 1111 0110
```

也就是说b里面的存储的内容会按照不同的解释方式而变化  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2b197bfcdbf94a128e43c257d6e14168.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二点：** `signed`和`unsigned`也是相关C语言考试的重点，下面代码可以帮助你很好的理解

```c
#include <windows.h>
#include <stdio.h>
#include <stdlib.h>


int main()
{
            
            
	unsigned int i;
	for (i = 9; i >= 0; i--)
	{
            
            
		printf("%u\n", i);
		Sleep(100);
	}

}
```

由于变量`i`是无符号整形，因此在与0比较的时候，不会小于0，所以会死循环，并且打印时从9开始减小到0，然后接着是42亿多，然后依次减小，最后再到0  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb381e5abfcbb489a83daf11f1d4e27c8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

第三点：使用`unsigned`时初始化变量时，建议带上`u`，也即

```c
unsigned int b=10u;
```

# 六：if、else

`if`和`else`如果简单点学其实也很简单，主要就是以下内容

1.  0为表示假，非0表示真
2.  if语句执行时，必然是先执行`“（）”`里面的表达式或者是函数，得到真假后，然后进行判定，再进行分支功能

## （1）关于C语言中bool类型

在C99之前C语言是没有`bool`类型的，在C00之后引入了`_Bool`类型，它处于头文件`stdbool`.h中

```c
#include <windows.h>
#include <stdio.h>
#include <stdbool.h>


int main()
{
            
            
	bool ret = false;
	ret = true;
	printf("%d\n", sizeof(ret));//在vs中为1
	return 0;
}
```

源码中显示就是一个宏定义

```c
//
// stdbool.h
//
//      Copyright (c) Microsoft Corporation. All rights reserved.
//
// The C Standard Library <stdbool.h> header.
//
#ifndef _STDBOOL
#define _STDBOOL

#define __bool_true_false_are_defined	1

#ifndef __cplusplus

#define bool	_Bool
#define false	0
#define true	1

#endif /* __cplusplus */

#endif /* _STDBOOL */

/*
 * Copyright (c) 1992-2010 by P.J. Plauger.  ALL RIGHTS RESERVED.
 * Consult your license regarding permissions and restrictions.
V5.30:0009 */


```

## （2）float与“零值”的比较

使用`if`进行浮点数比较时，下面的代码正确吗？按照道理`1.0-0.9=0.1`，应该是正确的

```c
int main()
{
            
            
	double x = 1.0;
	double y = 0.1;
	if ((x - 0.9) == y)
	{
            
            
		printf("correct\n");
	}
	else
	{
            
            
		printf("wrong\n");
	}
	return 0;
}
```

但实际结果却是：  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbf68526b76474b62a3d9e47c3e155733.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
为什么会这样呢，其实这涉及到的的浮点数如何在计算机中存储的问题，详细细节请移步：

- [\(计算机组成原理\)第二章数据的表示和运算-第三节1：浮点数的表示](https://blog.csdn.net/qq_39183034/article/details/119455329)

其实如果你打印出来后，你会发现两者根本不相等，精度丢失

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6cb12a67d5314d0480821575f081363a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
既然浮点数比较时不能直接使用`“==”`，那么应该怎么办呢？

**比较时，有点像高等数学中的取极限， δ \\delta δ可以被视为一个误差范围，这个 δ \\delta δ需要你自己定义，当两者的绝对值之差小于该范围时，C语言就认定他们相等，否则不相等**

```c
int main()
{
            
            
	double x = 1.0;
	double y = 0.1;
	if (fabs((1.0 - 0.9)-0.1) < CMP)
	{
            
            
		printf("correct\n");
	}
	else
	{
            
            
		printf("wrong\n");
	}

	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F424f771843de469cb48a4ebf7992b02d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

这里的 δ \\delta δ其实C语言已经帮我们定义好了，处在`float`.h头文件之下

```c
#define DBL_EPSILON 2.2204460492503131e-016 /* smallest such that 1.0+DBL_EPSILON !=1.0 */
#define FLT_EPSILON 1.192092896e-07F /* smallest such that 1.0+FLT_EPSILON !=1.0 */
```

**回归到主题，如果0被定义为了浮点数，我们要判断某个数是否是0的话可以这样写**

```c
int main()
{
            
            
	double x = 0;
	if (fabs(x) < DBL_EPSILON)//注意不要写成<=
	{
            
            
		printf("x是0\n");
	}
	else
	{
            
            
		printf("x不是0\n");

	}
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fce014eff76844260b56b680f3aff88bb.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

## （3）if和else的匹配问题

这是一个老生常谈的话题。下面代码看似会输出“2”，但实际什么都不会输出

```c
int main()
{
            
            
	int x = 0;
	int y = 1;
	if (10 == x)
		if (11 == y)
			printf("1\n");
		else
			printf("2\n");

	return 0;

}
```

这属于代码风格问题，`else`匹配采用的是就近原则

# 七：switch-case组合

**第一：** `switch case`的基本语法结构

```c
switch(整型变量/常量/整型表达式)//注意只能这三种
{
            
            
	case var1://判断在这里
		break;
	case var2:
		break;
	case var3:
		break;
	default:
		break;
}
```

其中`case`完成的判断功能，`break`完成的是分支功能，所以如果忘记写`break`，就会导致**击穿**现象  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F360ac7ebbb5b4fab8f9630cf8f74c00a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二：** 注意一个语法细节，就是`case`里面如果要定义变量的话，必须加花括号

```c
int main()
{
            
            
	int num = 0;
	scanf("%d", &num);
	switch (num)
	{
            
            
	case 1:
	{
            
            
		int a = 1;//注意花括号
		printf("first\n");
		break;
	}
	case 2:
		printf("second\n");
		break;
	case 3:
		printf("third\n");
		break;
	default:
		printf("other\n");
		break;
	}

}
```

**第三：** 多条件匹配时可以这样写

```c
int main()
{
            
            
	int num = 0;
	scanf("%d", &num);
	switch (num)
	{
            
            
	case 1:
	case 2:
	case 3:
		printf("first\n");
		break;
	case 4:
	case 5:
		printf("second\n");
		break;
	default:
		printf("other\n");
		break;
	}
}
```

**第四：** 注意`default`可以放在任意位置

**第五：** `switch`中可以使用`return`语句，但不建议使用

# 八：do 、while 、for关键字

**第一：** 这三种循环基本语法如下

```c
//while
条件初始化
while(条件判定){
            
            
	//业务更新
	条件更新
}

//for
for(条件初始化; 条件判定; 条件更新){
            
            
	//业务代码
}

//do while
条件初始化
do{
            
            
	条件更新
  }while(条件判定)
```

**第二：** 三种循环对应的死循环写法如下

```c
while(1){
            
            
}

for(;;){
            
            
}

do{
            
            
}while(1);
```

**第三：** `break`是跳出该循环，`continue`是结束一次循环

```c
int main()
{
            
            
	while (1)
	{
            
            
		int c = getchar();
		if (c == '#')
		{
            
            
			break;//表示接受到“#”就结束
		}
		putchar(c);
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F76cbcd753b7c49f1b545f94a97aab9f5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

```c
int main()
{
            
            
	while (1)
	{
            
            
		int c = getchar();
		if (c == '#')
		{
            
            
			continue;//表示接受到“#”略过
		}
		putchar(c);
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1dc608875fdd44db993feeca5316396f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
这里需要注意`for`循环的`continue`，经常爱考察。`for`循环在`continue`时是跳到循环更新处

```c
int main()
{
            
            
	int i = 0;
	for (; i < 10; i++)
	{
            
            
		printf("continue before:%d\n", i);
		if (i == 5) {
            
            
			printf("continue语句之前\n");
			continue;
			printf("continue语句之后\n");

		}
		printf("continue after:%d\n", i);
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6e721eb58f294f07b9dae112a0396717.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
**第四：** `for`循环区间建议是前闭后开

```c
for(int i=0;i<10;i++)
{
            
            
	//循环10次
}
for(int i=6;i<10;i++)
{
            
            
	//循环10-6=4次
}


```

# 九：goto关键字

**第一：** goto基本控制逻辑或者基本语法如下

```c
int main()
{
            
            
	int i = 0;
START:
	printf("[%d]goto running ... \n", i);
	Sleep(1000);
	++i;
	if (i < 10){
            
            
		goto START;
	}
	printf("goto end ... \n");
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F31175f7e6a7c4056914a8e42fb0bb7f0.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 十：void关键字

**第一：** `void`是不能用来定义变量的。因为定义变量的本质就是开辟内存空间，而`void`作为空类型是不应该开辟空间的，即使开辟了空间，**也仅仅作为一个占位符来看待**，所以这种行为直接就会被编译器禁止

**第二：** 首先说明一点，在C语言中函数是可以不带返回值的，返回类型为整型  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F339dda3253e04e5492a0ad195de5ce03.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
的确在有些场景中我们是不需要函数的返回值的，但如果采用上面的那种方式书写，很容易产生阅读上的歧义，**因此如果函数不想让其返回，可以用`void`，这里一定要将其理解为一种占位符，它是告知用户和编译器的**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb76142d1b6f5475d9a33f5149bc3dbef.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二：** 在如下情形中，编译器是不会报错的，因此会有很大的安全隐患  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdc22ac9370fc4e4b99e6f036d7d2471d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
而如果限制`void`后，编译器将会报警。**因此`void`可以充当函数的形参列表，用于告知编译器和用户该函数不需要传入参数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6573bb1083394dd89a5ac0c1ca30f3d4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第四：** `void`的确不可以定义变量，但是`void*`可以，因为指针变量的大小是明确的\(Windows32位下为4个字节大小\)

```c
void* p=nullptr;
```

**第五：** `void*`可以被任何类型的指针接受，**`void*`也可以接受任意指针类型**

```c
int main()
{
            
            
	void* p = NULL;
	int* x = NULL;
	double* y = NULL;
	p = x;//void*接受int*
	p = y;//void*接受double*
}
```

- 尤其注意 **`void*`也可以接受任意指针类型**，这一点通常用作一些通用接口的设计

**第六：** 我们知道，普通类型的指针可以进行位运算

```c
int* p=NULL;
p++;
p--;
```

而对于`void*`呢？要视平台而定，一般VS下不可以，Linux下可以（Linux认为void是1）

# 十一：return关键字

**第一**：**return不可以返回指向“栈内存”的指针，因为在函数体结束时会被自动销毁**

因此下面的语句会出现乱码

```c
char* show()
{
            
            
	char str[] = "hello world";
	return str;
}
int main()
{
            
            
	char* s = show();
	printf("%s\n", s);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7725b4dd7e0f46669326b2076543bd89.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二：** 函数的返回值，通过寄存器的方式，返回给函数的调用方（注意区别上面，上面不能那样做，因为那是指向栈的指针）

```c
int GetData()
{
            
            
	int x = 0x11223344;
	printf("running\n");
	return x;
}

int main()
{
            
            
	int y = GetData();
	printf("return value:%x\n", y);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd34832987b434283862c245418d7a2a2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

`return x`对应的汇编代码为：  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdd57d7489ba5452da4f198d0b7c11a19.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 十二：const关键字

**第一：** const修饰的变量不可以**直接**被修改

```c
const int a=10;
a=20;//错误
```

但间接可以修改

```c
int main()
{
            
            
	const int a = 10;
	int* p = &a;
	printf("change before:%d\n", a);
	*p = 20;
	printf("change after:%d\n", a);
	return 0;
}
```

那么既然这样其意义何在呢？其实`const`修饰变量主要有下面两个目的

1.  让编译器进行直接修改式检查
2.  告诉其他人这个变量不要改动，属于“自描述”含义

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4f0cee5649d54a679b81c0f0e67c751f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

真正意义上的不可修改如C语言中的常量字符串

```c
int main()
{
            
            
	char* str = "hello world\n";//常量字符串
	*str ='E';
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6ddcf9c05e4c4250b2073bd3acb785b6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二：** `const int i` 和`int const i`是等价的

**第三：** `const`修饰的变量同样不能作为数组定义的一部分（标准C不可以，但是在Linux可以）

```c
int main()
{
            
            
	const int n = 100;
	int arr[n];//错误
	return 0;
}
```

**第四：** `const`在定义时必须初始化

**第五：** 建立只读数组可以这样写

```c
int const a[5]={
            
            1,2,3,4,5};
或
const int a[5]={
            
            1,2,3,4,5};
```

**第六** ：`const`放在谁后面就修饰谁，因此它与指针的关系如下

①：`const int* i` 与`int const* i`等价  
其中i是指针，const修饰了int，表示指针可以变化，但是指针指向内容不能被修改

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F04eeeb0ca15b4b4999ed3c4abedb16e4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

②：`int* const i`

`const`修饰的是指针，所以指针不可变，但是指向的内容可变  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe0b054b349984c23b488ab35975c99ae.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

③：`const int* const i=&a`

表示指针不可以变，指向的内容也不可以变  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F45f401c3bac84fdb9899b618485d6bc6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第七：** `const` 也可以用来修饰函数参数，表明不可更改

```c
void show(const int* _p)//防止指针指向内容被修改
{
            
            
	printf("value:%d\n", *_p);
	*_p = 20;//非法操作
}

int main()
{
            
            
	int a = 10;
	int* p = &a;
	show(p);
}
```

# 十三：volatile关键字

有关volatile关键字的作用在下面这篇文章中有详细介绍，请移步

- [Linux系统编程34：进程信号之可重入函数，volatile关键字的作用和SIGHLD](https://blog.csdn.net/qq_39183034/article/details/115578577?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163375803716780261986763%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=163375803716780261986763&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-115578577.pc_v2_rank_blog_default&utm_term=volatile&spm=1018.2226.3001.4450#2volatile_10)

`volatile`关键字的作用：**volatile将保持内存的可见性，一个变量一旦被volatile修饰，那么系统总是会从内存中读取数据，而不是从寄存器**

需要注意`const`和`volatile`的区别，两者并不矛盾

- `const`要求你不要进行写入
- `volatile`意思是你读的时候每次要从内存读

# 十四：extern关键字

`extern`关键字这里就多说了，非常简单

# 十五：struct关键字

**第一：** `struct`基本介绍

定义  
![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5dae9c3506b840d7b9db2d1b4021ae84.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

初始化（不能初始化后整体赋值）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fca6ad08c0e914d079e2d2cef006372ac.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F79c206d205ce4274a4bd6fd9221e113e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
成员访问  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc6f796eedd0542d18ffea363207e9f82.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
结构体传参  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd281f29fa74c475eaf68a7aebb1bdd67.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
**第二：** 在Linux中空结构体的大小为0

**第三：** 柔性数组

我们知道C语言中是不能有这样的操作的，就是用变量对数组进行初始化

```cpp
int main()
{
            
            
	int i=0;
	scanf("%d",&i);
	int arr[i];
}
```

在C语言中如果要完成动态数组，可以借助柔性数组。使用柔性数组时，我们采用结构体的方式，将一个数组作为结构体成员放置于其中，但注意该数组不初始化，什么都不写  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F276de935c6834a42ab251d4ca6bfe79b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
在上述结构体中，有两个结构体变量，数组似乎不占空间，但其实不然。实则，该结构体将其所占空间划分为两部分，一部分就是那个整形，一部分用于动态开辟，以此满足数组的动态变化![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F01ba40e04f5d4a6ea224eb8efb7d0192.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
既然是柔性，那就可以修改，使用realloc修改  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F71169fa706bc40f49e6a2286e87952ee.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 十六：Union关键字

**第一：** Union是什么  
联合也是一种特殊的自定义类型 这种类型定义的变量也包含一系列的成员，**特征是这些成员公用同一块空间**,联合体内所有成员的起始地址都是一样的，每个成员都认为它是联合体的第一个成员

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F87118d800cee4e1aae9ef3b6fea2988d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第二：** 根据内存地址分布，如下，`b`永远定义在相对于`a`的低地址处

```c
union Un
{
            
            
	int a;
	int b;
};
```

根据这一性质我们可以利用联合体来判断机器是大端机还是小端机，如下

```c
union Un
{
            
            
	int a;
	char b;
};
int main()
{
            
            
	union Un u;
	u.a = 1;
	if (u.b == 1){
            
            
		printf("小端机\n");
	}
	else {
            
            
		printf("大端机\n");
	}
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3faa02afd5e24d80adb310bf6d97d33e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

这是因为  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc902d81caf974a818a03c898a5f6185f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 十七：enum关键字

`enum`用于枚举一堆常量，就像Excel中的数据有效性，它规定了数据的取值类型，比如说男性它只有男或女

定义  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4095124f22514a0da90b34ce4c421d57.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
enum color是枚举类型，括号中的内容是枚举类型的可能取值，也叫做枚举常量。这些可能取值实际上是有值的，默认是从0开始的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb33d5f7354cd418092df22bef5f58ad2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

当然是可以修改的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F658a365d36174550a82fc2cad3b8a1fd.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
枚举的这样的写法其实和宏的写法在代码的逻辑上是相似的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe222068488214c439a2a2b89d37df411.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

# 十八：typedef关键字

**第一：** `typedef`的作用就是为类型重新命名

```c
typedef unsigned int u_int;

int main()
{
            
            
	u_int a = 10;
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7867bbc661bf4f1ea8b8f3ee5d73027f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)  
`typedef` 经常会在结构体重命名里

```c
typedef struct stu
{
            
            
	int a;
	int b;
}Student;

int main()
{
            
            
	Student student1;
}
```

**第二：** 大家一定要对`typedef`理解到位，如下

```c
int main()
{
            
            
	int* a, b;
	//a是指针类型
	//b是整形
}
```

`typedef`从某种方面可以理解一种全新的类型，因此下面的`*`就不存在和谁结合的问题了

```c
typedef int* int_p;

int main()
{
            
            
	int_p a, b;
	//a是指针类型
	//b也是指针类型
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F75c60c0836444314a35856681671cc03.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

而对于`#define`而言它就是一种文本替换了，因此

```c
#define int_p int*

int main()
{
            
            
	int_p a, b;
	//a是指针类型
	//b是整形
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0de96ed5a5c844d3ae9c83ff409689a0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120700541)

**第三**：使用`typedef`定义后的新类型，不能配合其他关键字使用

```c
#define INT_DE int
typedef int INT_TY;

int main()
{
            
            
	unsigned INT_DE a;//正确
	unsigned INT_TY b;//错误
}
```

---

# 总结

## （1）关键字分类

**数据类型关键字** ：

- `char`
- `short`
- `int`
- `long`
- `signed`
- `unsigned`
- `float`
- `double`
- `struct`
- `union`
- `enum`
- `void`

**控制语句关键字** ：  
**1：循环控制**

- `for`
- `do`
- `while`
- `break`
- `continue`

**2：条件语句**

- `if`
- `else`
- `goto`

**3：开关语句**

- `switch`
- `case`
- `default`

**4：返回语句**

- `return`

**存储类型关键字** ：

- `auto`
- `extern`
- `register`
- `static`
- `typedef`

这里需要补充一点：使用`typedef`时不能同时出现多个存储关键字

```c
typedef static int//错误
typedef register int//错误
```

**其他关键字** ：

- `const`
- `sizeof`
- `volatile`