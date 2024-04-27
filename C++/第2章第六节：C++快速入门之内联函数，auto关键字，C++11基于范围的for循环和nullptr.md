 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：内联函数](#_3)
- - [（1）内联函数的概念](#1_28)
  - [（2）注意](#2_44)
- [二：auto关键字\(C++11新特性\)](#autoC11_51)
- - [（1）auto简介](#1auto_52)
  - [（2）注意事项](#2_61)
- [三：基于范围的for循环\(C++11新特性\)](#forC11_84)
- [四：指针空值nullptr\(C++11新特性\)](#nullptrC11_113)
- - [（1）NULL的缺陷](#1NULL_114)
  - [（2）nullptr](#2nullptr_133)

# 一：内联函数

函数在调用时会有**一定开销**，有限次调用倒无伤大雅，但如果一个函数被反复调用，那么这个开销就显得没有必要了。所以为了解决这样的问题，在之前C语言的学习中，我们会使用**宏函数**，让这个函数直接在**预处理阶段展开**，从而不会产生压栈开销

```c
# define max(a,b) a>b?a:b
int main()
{
            
            
	int ret=max(1,2);
	return 0;
}
```

但我们知道，**宏函数缺陷非常大**，主要体现在

- 不能调试
- 没有安全类型检查，因为宏直接就是替换
- 写法复杂，较容易出错

所以在C++中，可以使用 **内联函数** 解决

## （1）内联函数的概念

**内联函数：被`inline`修饰的函数叫做内联函数。编译时会在调用内联函数的地方直接展开，它没有函数压栈的开销，因此提高了程序运行的效率**

例如下面的例子中，`add()`是一个普通函数，因此其汇编代码中**存在`call`指令**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210218223405203.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

如果使用`inline`将其修饰为内联函数，**在编译期间编译器会用函数体替换函数的调用**，可以发现其汇编代码中**不存在`call`指令**

- **注意**：在`debug`模式，编译器不会对代码进行优化，因此只能在`release`模式下查看汇编代码中是否存在`call`指令

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021021822372495.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

## （2）注意

- 内联函数只是一种**建议**。不是说所有函数都要设置为内联函数（否则会导致代码急剧膨胀），内联函数适合于那些**频繁调用的小函数**

- 内联函数本质就是**以空间换时间**，因此对于**递归，循环**这类情况则不适合作为内联函数

- **不要把内联函数的声明和定义分离（也就是只能在源文件写）**。分离会导致**链接错误**，因为一旦`inline`被展开，就没有函数地址了（这一点特别注意）

# 二：auto关键字\(C++11新特性\)

## （1）auto简介

C++11中`auto`的含义为：**`auto`是一个新的类型指示符来指示编译器，`auto`声明的变量必须由编译器在编译时期经推导获得。也就说`auto`可以自动推导出等式右边的变量类型**

- **使用`auto`定义变量时必须对其进行初始化**，在编译阶段编译器需要根据初始化表达式来推导`auto`的实际类型，所以`auto`并非是一种类型的声明，而是一种类型的“**占位符**”，编译器会在编译时会将`auto`替换为实际的变量类型
- **注意**：这里由于相关知识较少，所以你会觉得`auto`有点“鸡肋”，但在后面的学习中大家会发现使用`auto`十分省事，尤其在迭代器、STL里面

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219160740801.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

## （2）注意事项

①：`auto`声明指针类型时，既可以使用`auto`也可以使用`auto*`，**但是在声明引用时必须加上`&`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219161845500.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

②：在同一行声明多个变量时，这些变量必须是**同一类型**

```cpp
int main()
{
            
            
	auto a=1,b=2;//正确
	auto c=3,d=4.0;//错误
}
```

③：`auto`**不能作为函数参数**

```cpp
void test(auto a)
{
            
            
	//编译错误，调用函数需要开栈帧，但此时编译器无法对a的类型进行推导
}
```

④：`auto`**不能用于声明数组**

# 三：基于范围的for循环\(C++11新特性\)

在C/C++中遍历一个数组，首先想到的做法是这样的

```cpp
int main()
{
            
            
	int array[]={
            
            1,2,3,4,5};
	for(int i=0;i<sizeof(array)/sizeof(array[0]);++i))
	{
            
            
		std::count<<array[i];
	}
}
```

对于这种有范围的集合，**C++11中引入了基于范围的for循环来实现它**

---

如下，改写代码

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219164336522.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)  
当然也可以结合引用

- **注意**：这个例子中不加引用也是可以修改的，但是这两种方式区别非常大，属于深浅拷贝问题，后续会详细探讨

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219164755103.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

# 四：指针空值nullptr\(C++11新特性\)

## （1）NULL的缺陷

“C语言定义指针时如果没有合理指向，就指向`NULL`”，这一点大家都明白

```c
int main()
{
            
            
	int* p=NULL;
}
```

但是这种定义是有缺陷的，甚至有时会干扰到我们。下面是`NULL`被定义的地方，**可以发现`NULL`可能被定义为了字面常量0或`void*`**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021021916590999.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

如下有两个同名函数，形参分别为`int`型和`int*` 型，在主函数中调用该函数，参数依次为`0`，`NULL`, `(int*)NULL`

结果显示，前两个全部调用了第一个函数。其中参数为`NULL`的那一个我们原本是想让它调用第二个函数，但在这里被解释为了0，所以调用了第一个。而参数为 `(int*)NULL`那一个由于进行了强制类型转换，被解释为了指针类型，所以才调用了第二个

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219170503941.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)

## （2）nullptr

为了解决上述缺陷，C++11引入了`nullptr`。继续执行上述步骤，可以发现，它调用的正是我们想要它调用的函数

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210219211735400.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113837275)