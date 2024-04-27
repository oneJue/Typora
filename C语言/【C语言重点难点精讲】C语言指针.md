 

### 文章目录

- [一：指针入门](#_4)
- [二：数组入门](#_25)
- - [（1）数组的内存空间布局](#1_26)
  - [（2）区分\&arr\[0\]和\&arr](#2arr0arr_34)
- [三：指针和数组的关系](#_90)
- - [（1）以指针的形式访问和以数组形式访问](#1_94)
  - [（2）为什么C语言要这样设计？](#2C_122)
- [四：指针数组和数组指针](#_168)
- [五：多维数组和多级指针](#_189)
- - [（1）二维数组](#1_190)
  - [（2）二级指针](#2_278)
  - [（3）多级指针](#3_297)
- [六：数组参数和指针参数](#_317)
- - [（1）一级指针传参](#1_318)
  - [（2）一维数组传参](#2_375)
  - [（4）二维数组传参](#4_394)
- [七：函数指针](#_422)
- - [（1）函数指针](#1_423)
  - [（2）函数指针数组](#2_449)
  - [（3）指向函数指针数组的指针](#3_482)

指针是C语言中的一大重点，有关指针的理解一些基础性使用不在本篇文章介绍范围内，读者可以阅读站内其它博主的文章，写得都比较棒，本文主要是针对指针中的一些重点，难点做一些总结，以及最重要的是和数组的关系

# 一：指针入门

如果读者能够较深刻理解下面语句的含义，那么对于指针就算是达到入门的标准了

```c
//第一组
int a=10;
int* p=&a;
//第二组
p=10;
int* q=p;
//第三组
*p=10;
int b=*p;
```

- 第一组：定义一个整形变量`a`，并赋值10，然后定义一个指针变量`p`,用`a`的地址初始化
- 第二组：把10赋值给指针变量`p`，此时指针`p`指向一个地址为10的地址，然后把`p`的内容同样赋值给一个同样类型的指针变量`q`
- 第三组：`*p`表示解引用，将p所指向的空间里的内容改为10，然后赋值给变量`b`

# 二：数组入门

## （1）数组的内存空间布局

数组是**整体申请**空间的，然后将地址最低的空间，作为`a[0]`元素

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6d2bec868bc24d4d81175eccca8f1f84.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
**这就是为什么我们访问数组或者用指针访问数组时采用的是++，本质就是其元素排布时按照地址增大方向排布**

## （2）区分\&arr\[0\]和\&arr

首先我们需要搞清楚**指针+1**究竟是什么含义，看如下代码

```c
int main()
{
            
            
	char* c = NULL;
	short* s = NULL;
	int* i = NULL;
	double* d = NULL;

	printf("%d\n", c);
	printf("%d\n\n", c + 1);

	printf("%d\n", s);
	printf("%d\n\n", s + 1);

	printf("%d\n", i);
	printf("%d\n\n", i + 1);

	printf("%d\n", d);
	printf("%d\n\n", d + 1);

}
```

其运行结果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F25c99c2de076481e8401b8e611e860e7.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
因此，**指针+1实际加上的是该指针类型的大小**

回到正题

```c
int main()
{
            
            
	char arr[10] = {
            
             0 };
	printf("%p\n", &arr[0]);
	printf("%p\n", &arr[0]+1);

	printf("%p\n", &arr);
	printf("%p\n", &arr+1);
}
```

- `&arr`，`sizeof(arr)`：`arr`表示整个数组，
- `&arr[0]`：表示数组首元素的地址，因为\[\]的优先级更高
- `&arr[0]+1`：由于是char类型的数组，所以+1
- `&arr+1`：这是数组的地址，可以理解横跨整个数组

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd7de50b2538e4e0da0dcd8252cbe6a75.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

还有，当**数组名做右值**时，代表数组首元素的地址，本质等价于`&arr[0]`；但是，数组名**不可以充当左值**，能够充当左值的，必须是有空间且可被修改的

# 三：指针和数组的关系

**指针和数组没有关系，他们是完全不同的两套规范，只不过在操作上有交集**

## （1）以指针的形式访问和以数组形式访问

C语言中保存字符串中有两种形式，如下

```c
int main()
{
            
            
	char* str = "abcdef";//str指针在栈上保存，“abcdef”在字符常量区，不可修改
	char arr[] = "abcdef";//整个数组都在栈上保存，可以被修改
}
```

**1：分别以指针和以下标的方式访问指针**

```c
int len = strlen(str);
for (int i = 0; i < len; i++)
{
            
            
	printf("%c ", *(str + i));//以指针方式访问
	printf("%c ", str[i]);//以数组方式访问
}
printf("\n");
```

**指针和数组指向或者表示一块空间的时候，访问方式是可以互通的，具有相似性，但注意它们不是同一个东西**

## （2）为什么C语言要这样设计？

我们知道数组传参时可以采用这种方式

```c
void Show(int arr[], int num)
{
            
            
	for (int i = 0; i < num; i++) {
            
            
		printf("%d ", arr[i]);
	}
	printf("\n");
}

int main()
{
            
            
	int arr[] = {
            
             1,2,3,4,5 };
	int num = sizeof(arr) / sizeof(arr[0]);
	Show(arr, num);
}
```

其中的`sizeof(arr)/sizeof(arr[0])`语句不能放入在函数内进行，否则将会只打印一个元素，相信初学者在这里也犯过很多错误。

指针和数组互通的好处之一就在这里，因为如果不互通，**那么在传参时对于数组这样庞大的东西就会浪费非常多的额外空间，所以干脆在传参时将其降维成指针，直接让指针访问即可**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa6d48511d38e4777a81bd27872e9948a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
那么既然这样，形参中的`int arr[]`其实就可以写作`int* arr`

```c
void Show(int* arr, int num)
{
            
            
	for (int i = 0; i < num; i++) {
            
            
		printf("%d ", arr[i]);
	}
	printf("\n");
}

int main()
{
            
            
	int arr[] = {
            
             1,2,3,4,5 };
	int num = sizeof(arr) / sizeof(arr[0]);
	Show(arr, num);
}
```

因此：**所有的数组在传参时都会降维成指针——指向其内部类型的指针，一维数组当然就是对应数据类型的指针，二维数组就是一维数组的数组指针**

# 四：指针数组和数组指针

**指针数组：** 它是一个数组，里面的元素均是指针，即`int* p1[10]`

**数组指针：** 它是一个指针，指向了一个数组，即`int (*p2)[10]`

- `[]`的优先级大于`“*”`
- `int* p1[10]`可以理解为`int* [10] p1`
- `int (*p2)[10]`可以理解为`int[10]* p2`
- `int (*p[4])[5]`中`[4]`是一个数组，这个数组存放了四个数组指针`p`,分别指向含有5个`int`类型的数组

数组指针赋值时一定要注意

```c
int a[10]={
            
            0};
int(*p)[10]=&a;
```

# 五：多维数组和多级指针

## （1）二维数组

需要注意多维数组其内存空间布局仍然是**线性连续递增的**

```c
int main()
{
            
            
	char c[4][3] = {
            
             0 };
	for (int i = 0; i < 4; i++)
	{
            
            
		for (int j = 0; j < 3; j++)
		{
            
            
			printf("&a[%d][%d]:%p\n", i, j, &c[i][j]);
		}
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4829dc13d6db49bf9fba6c319448fa9e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F84b5d4bbd0db495f89456eb2349fd8eb.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
就二维数组而言，我们认为：**二维也可以看做一维数组，只不过其内部元素仍然是一维数组**

因此下面这三种地址虽然值是相同的但是其含义完全不同

```c
char c[4][3] = {
            
             0 };
printf("%p\n", &c);//这个“二维”数组的地址
printf("%p\n", c);//“二维”数组中第一个元素，也即第一个数组的地址
printf("%p\n", &c[0][0]);//“二维”数组第一个元素的第一个元素的地址
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc7197f3236504121a81a582a00420ac8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
那么相应的+1操作也有各自的含义

```c
char c[4][3] = {
            
             0 };
printf("%p\n", &c);
printf("%p\n", &c+1);//直接越过12个元素
printf("\n");
printf("%p\n", c);
printf("%p\n", c+1);//越过“1”个元素，也即1维数组，也即3个char
printf("\n");
printf("%p\n", &c[0][0]);
printf("%p\n", &c[0][0]+1);//越过1个char
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc4b189ee377942489bc9965c9cf17a2e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

下面的练习题可以帮助你很好理解上面的概念

```c
int a[3][4] = {
            
            0};

//求整个数组的大小
printf("%d\n",sizeof(a));//(3×4)×4=48
//求单个元素的小
printf("%d\n",sizeof(a[0][0]))//1×4=4
//二维数组第一个“元素”，即第一个一维数组的大小
printf("%d\n",sizeof(a[0]))//4×4=16
//当sizeof()内部只含有一个数组名时表示整个数组，当内部数组名参与运算时表示单个元素
//因此这里就是第一个元素也即第一个数组的首元素地址进行偏移，仍然是一个指针
printf("%d\n",sizeof(a[0]+1))//4
//第一个元素也即第一个数组的首元素偏移至第一个数组的第二个元素进行解引用，也即a[0][1]
printf("%d\n",sizeof(*(a[0]+1)))//a[0][1]，4
//表示该二维数组的第一个数组的地址，然后偏移至第二个数组的地址处，注意仍然是地址
printf("%d\n",sizeof(a+1));//4
//这是第二个数组的地址，对齐解引用相当于第二个数组的数组名
printf("%d\n",sizeof(*(a+1)));//4×4=16
//a[0]表示第一个数组，&a[0]第一个数组的地址，然后+1，偏移值第二个数组的地址处
printf("%d\n",sizeof(&a[0]+1));//4
//同上，它是第二个数组的地址，然后解引用就是第二个数组的大小
printf("%d\n",sizeof(*(&a[0]+1))); //4×4=16
//相当于*(a+0)，于是表示第一个数组，然后解引用就是第一个数组的大小
printf("%d\n",sizeof(*a)); //4×4=16
//第四个一维数组
printf("%d\n",sizeof(a[3])); //16
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe94c6ad293324c698acee3a6db178263.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
**其实以下写法是等价的**

- `a[2]`等价于`*(a+2);`
- `a[2][3]`等价于`*(*(a+2)+3)`

## （2）二级指针

准确理解以下例子的含义即可

```c
int main()
{
            
            
int a = 10;
int* p = &a;
int** pp = &p;

p = 100;//将p指针的内容改为100
*p = 100;//将p指针指向的空间也即变量a的内容改为100
pp = 100;//pp之前指向了一级指针，现在更改为指向100
*pp = 100;//pp保存的是p的地址，因此现在相当于将p的指向改为了100
**pp = 100;//两次解引用就是修改变量a的内容100

}
```

## （3）多级指针

下面的这个例子有助于你深刻理解多级指针，其输出结果为`“ink”`

```c
#include<stdio.h>

int main()
{
            
            
    static char *s[] = {
            
            "black", "white", "pink", "violet"};
    char **ptr[] = {
            
            s+3, s+2, s+1, s}, ***p;
    p = ptr;
    ++p;
    printf("%s", **p+1);
    return 0;
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F28cb003f00c244179ab8a2f50c1636e6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_17%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

# 六：数组参数和指针参数

## （1）一级指针传参

由于指针变量的特殊性，所以才传参时很多人就会将其特殊化，但是指针传参也会拷贝，**函数内部和外部两个指针不是一个指针**

```c
void test(char* p)
{
            
            
	printf("test函数内的p的地址%p\n", &p);
}


int main()
{
            
            
	char* p = "hello world";
	printf("main中p的地址:%p\n", &p);
	test(p);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F628e36decb6044fb8088924c7b4486c8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
因此，很多人会犯下面这样的错误

```c
void GetStr(char* p)//传过来一个字符指针，然后为其申请空间，将字符串拷贝进去
{
            
            
	p = malloc(sizeof(char) * 10);
	strcpy(p, "hello");
}


int main()
{
            
            
	char* p = NULL;
	GetStr(p);//调用该函数相当于执行赋值功能
	printf("%s\n", p);
}
```

结果显而易见，什么也不会打印，这是因为传过去的是一个一级指针，依然用一级指针接受的话，发生的是**值拷贝**，那个字符串拷贝函数只是对局部变量进行拷贝，函数调用结束，局部变量销毁相当于什么也没用做  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffb6f3819f2df462b8bbda152e292e1fb.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
因此，正确的做法是采用二级指针

```c
void GetStr(char** pp)//使用二级指针接受
{
            
            
	*pp = malloc(sizeof(char) * 10);
	strcpy(*pp, "hello");//拷贝是就是拷贝到了一级指针对应的空间，这个一级指针是真正的main函数的一级指针
}


int main()
{
            
            
	char* p = NULL;
	GetStr(&p);//传入一级指针地址
	printf("%s\n", p);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6d76bc59e80c44e1a5823959433ce171.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

## （2）一维数组传参

```cpp
void test(int arr[])//比较常用的写法。注意传过来的是数组，但本质已经降维为了指针
{
            
            }
void test2(int arr[10])//同上
{
            
            }
void test3(int* arr)//数组名就是首元素地址，可以
{
            
            }
void test4(int** arr)//指针数组每个元素都是指针，可以用二级指针
{
            
            }
int main()
{
            
            
	int arr[10] = {
            
             0 };//整形数组
	int* arr2[20] = {
            
             10 };//指针数组
}
```

## （4）二维数组传参

**数组传参一定要发生降维，降维成指针，对于二维数组就会降维成数组指针**

```c
void test(int(*a)[5])//数组指针
{
            
            }

int main()
{
            
            
	int a[6][5] = {
            
             0 };
	test(a);
}
```

传参时当然可以使用`int arr[6][5]`这样的形式，由于它是依靠列来确定组数的，**所以行可以省略，但是列不能省略**，因此二维数组传参还可以这样写

```c
void test(int a[][5])//数组指针
{
            
            }

int main()
{
            
            
	int a[6][5] = {
            
             0 };
	test(a);
}
```

# 七：函数指针

## （1）函数指针

函数是代码的一部分，程序运行时也要加载进内存，以供CPU后续寻址。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd6e65e45a0cd4719bbc9cd2eb1896462.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7906adc5c59c46e5816cfabb43b80ba9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

函数指针的定义和数组指针基本类似，**其定义和调用方式如下**

```c
int add(int x, int y)
{
            
            
	int z = x + y;
	return z;
}

int main()
{
            
            
	int a = 10;
	int b = 20;
	int(*p)(int, int) = &add;//这个指针指向了一个函数，其参数有两个类型分别为int和int，所指向函数的返回值为int
	printf("%d\n", (*p)(a, b));//第一种调用方式
	printf("%d\n", p(a, b));//第二种调用方式
}
```

## （2）函数指针数组

**函数指针数组本质是一个数组，存放的元素类型是函数指针**

如下`Add()`、`Sub()`这两个函数形参列表相同，返回值相同，因此可以将他们的地址存放在函数指针数组中

```c
int Add(int x, int y)
{
            
            
	int z = x + y;
	return z;
}

int Sub(int x, int y)
{
            
            
	int z = x - y;
	return z;
}

int main()
{
            
            
	int(*parr[2])(int, int) = {
            
             &Add, Sub };//函数指针数组的定义
	for (int i = 0; i < 2; i++)
	{
            
            
		printf("%d\n", parr[i](2, 3));//函数指针数组的使用
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa77da86d1dd844bd90483e65cbf3990f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)

函数指针数组能够很好的保存一组具有相同参数类型，相同返回值的函数的地址。它的一个经典例子就是“转移表”。比如在计算器例子中，使用switch case语句，如果使用普通方式，要增加一些其他运算时，其case语句要多次增加，显得很臃肿，而运用函数指针数组，则能避免这种情况，且在后期增加新的相同类型的运算时，在主函数内只需增加新函数地址

## （3）指向函数指针数组的指针

```c
int arr[10] = {
            
             0 };
int(*p) = arr;//指向整形数组的指针

int(*parr[4])(int, int);//函数指针数组
int(*(*pparr)[4])(int, int) = &parr;//指向函数指针数组的指针
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6c93f55f7d9d417ebdd192e0249a8403.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120842292)