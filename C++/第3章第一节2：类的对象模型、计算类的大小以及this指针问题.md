 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：类对象模型](#_6)
- - [（1）如何计算类对象的大小](#1_7)
  - [（2）类对象的存储方式](#2_24)
- [二：this指针](#this_50)
- - [（1）一个问题](#1_51)
  - [（2）this指针](#2this_85)
  - [（3）this指针存在哪里？](#3this_122)

# 一：类对象模型

## （1）如何计算类对象的大小

根据C语言中所学知识：**请问下面这个类实例化后所占空间多少？**

```cpp
class A{
            
            
public:
    void printA(){
            
            
    cout << _a << endl;
}
private:
    char _a;
};
```

**你可能会这样思考**：这个类中有一个成员变量`_a`和一个成员函数`printA`，其中`char`占用1个字节，`printA`实际上是一个函数地址，而在32位平台下，一个指针占用4个字节，所以总共是5个字节，然后根据内存对齐，因此结果为8个字节。那么我们结果究竟是不是这样呢，我们可以运行程序  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F751651cc738a44d88f6d381a64baf314.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113873994)

**结果显示只有1个字节，不是预想的8个字节。实际上：类在计算大小时是不管成员函数的，只管成员变量和内存对齐。为什么会这样？待大家了解了类对象的存储方式之后就明白了**

## （2）类对象的存储方式

**类对象的存储方式：一个类可以实例化出很多对象，这些对象可能各不相同，但他们所能执行的方法都是那一套。**因此实例化时，如果这些函数也像成员变量那样需要占用空间，显然会造成不必要的浪费。所以解决方法是：**在对象空间中只保存成员变量，成员函数将存放在公共的代码段\(常量区\)，如下图**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210220151817996.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113873994)

**注意空类大小不是0，编译器为空类提供了一个字节用于标识**

```cpp
class A{
            
            
public:
    void printA(){
            
            }
};
class B{
            
            

};
int main() {
            
            
    A a;
    B b;
    cout << "对象a：" << sizeof(a) << endl;;
    cout << "对象b：" << sizeof(b) << endl;;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc0119b8c6934452b84a77b644f27c7a6.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113873994)

# 二：this指针

## （1）一个问题

如下有一个**日期类**，代码没有任何问题，也能正常运行，但这里我想问大家一个问题：**我们知道，类中成员函数是公用的，它并不存在于类中，既然如此，那么在执行`d1.SetDate(2021,2,20)`这条语句时，编译器是如何确定是`d1`这个对象调用了成员函数，而不是`d2`调用的呢？**

- **这里大家可能会有疑问**：“函数前面不是已经写了一个`d1`吗，怎么可能不知道它是`d1`调用的？”这里大家一定要跳出这个误区，如果函数是放在类里的，那无可厚非。但是当放在公用的代码段后，不要说用实例化的类去调用它了，假如能够知道函数的地址，我直接可以用函数地址调用了，那还用得着对象调用

因此这就产生了一个问题：**当很多对象调用成员函数时，如何区分调用者？**

```cpp
class Date
{
            
            
public:
	void SetDate(int year,int month,int day)
	{
            
            
		_year=year;
		_month=month;
		_day=day;
	}
private:
	int _year;
	int _month;
	ibt _day;
}

int main()
{
            
            
	Date d1;
	Date d2;
	d1.SetDate(2021,2,20);
	d2.SetDate(2022,8,19);
}
```

## （2）this指针

- 上述问题C++是通过引入**this指针**来解决的

**this指针：C++编译器会给每个非静态的成员函数增加了一个隐藏的指针参数，让该指针指向当前对象（即函数运行时调用该函数的对象），在函数体中所有成员变量的操作，一律通过该指针访问**

- **非静态的成员函数**：就是那种普通的函数，前面没有加`static`

**而在C++中this指针会被隐藏，所以上述代码如果补充完整，应该是下面这样的**

```cpp
class Date
{
            
            
public:
	void SetDate(Date* this,int year,int month,int day)
	{
            
            
	//生成一个类指针
		this->_year=year;
		this->_month=month;
		this->_day=day;//通过指针的指向来辨别对象
	}
private:
	int _year;
	int _month;
	ibt _day;
}

int main()
{
            
            
	Date d1;
	Date d2;
	d1.SetDate(&d1,2021,2,20);
	d2.SetDate(&d2,2022,8,19);所以实参里，其实就是类的地址
}
```

## （3）this指针存在哪里？

- **注意**：这是一个出现频率极高的面试题

**如下图，可以看出this指针作为形参，自然存在于栈区**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210220162802470.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113873994)

---

**请问下面这段程序运行时是否会崩溃**

 -    **注意：**`A* p= NULL`，表明p是一个空的类指针，下面代码中并不是在取成员，只是在突破类域，所以这不是代码错误

```cpp
class A
{
            
            
public:
	void printA()
	{
            
            
		std::cout << _a << std::endl;
	}

	void show()
	{
            
            
		std::cout << "Show()" << std::endl;
	}

private:
	int _a;
};

int main()
{
            
            
	A* p = NULL;
	p->printA();
	p->show();
	
}
```

经测试：如果只执行`p->printA()`就会崩溃，如果只执行`p->show()`就不会崩溃

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210220193503877.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F113873994)

调用`printA()`时，将这个`p`指针作为`this`指针传了过去，那么在函数中就会进行`std::cout<<this->_a<<std::endl;`的操作，自然而然会出现问题，而另一个函数没有崩溃是因为它就没有使用到这个`this`指针