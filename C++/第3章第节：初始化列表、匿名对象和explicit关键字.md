 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：初始化列表](#_5)
- - [（1）真的是初始化吗](#1_6)
  - [（2）初始化列表](#2_84)
  - - [A：概述](#A_85)
    - [B：必须在初始化列表进行初始化的成员](#B_125)
    - [C：关于初始化列表的一个坑](#C_294)
- [二：匿名对象](#_330)
- [三：explicit关键字](#explicit_370)

# 一：初始化列表

## （1）真的是初始化吗

**在前面的讲解中，我们使用构造函数目的是为了在对象被实例化的同时初始化某些变量**

```cpp
class Date
{
            
            
public:
	Date(int year=1998,int month=12,int day=20)
	{
            
            
		_year=year;
		_month=month;
		_day=day;
	}

private:
	int _year;
	int _month;
	int _day;
}

int main()
{
            
            
	Date d1(2000,2,2);
}
```

**但相信你还记得，我说过这种初始化方式不是真正的初始化 。真正的初始化在对象被实例化的同时就要赋值，因此这种方式实际上叫做构造函数内的赋值**

**很经典的一个例子就是`const`对象**：如果把一个`int`型变量修饰为`const`，那么这个`const int`必须在定义的时候就初始化，任何后续的赋值将不被允许，这一点大家是非常明白的

```cpp
int main()
{
            
            
	//定义的时候就必须初始化
	const int a=10;
	//这种方式错误
	const int a;
	a=10;
}
```

回到上面的例子中，我们在之前的成员中加上一个`const`成员，然后按照这种方式初始化，如果最终报错就说明这种方式并不是真正的初始化

```cpp
class Date
{
            
            
public:
	Date(int year=1998,int month=12,int day=20,int n=100)
	{
            
            
		_year=year;
		_month=month;
		_day=day;
		_n=n;
	}

private:
	int _year;
	int _month;
	int _day;
	
	const int _n;
}

int main()
{
            
            
	Date d1(2000,2,2,200);
}
```

答案显而易见，这并不是真正意义上的初始化。那么像`const int`这种类型的成员究竟应该怎么样初始化呢？在C++中可以使用初始化列表解决

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210305140922394.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

## （2）初始化列表

### A：概述

**初始化列表：以一个冒号开始，接着是一个以逗号分隔的数据成员列表。每个数据成员列表由“成员变量”和“初值或表达式”构成。每个成员变量在初始化列表中只能出现一次**

```cpp
#include <iostream>
using namespace  std;

class Date
{
            
            
public:
    Date(int year=2000, int month=1, int day=1, int n = 3):_year(year), _month(month) ,_day(day), _n(n)
    {
            
            }

    void DisPlay() const
    {
            
            
        cout << _year << "-" << _month << "-" << _day << "-" << _n << endl;
    }

private:
    int _year;
    int _month;
    int _day;

    const int _n;

};

int main(){
            
            
    const Date d1(2012, 12, 20, 100);
    d1.DisPlay();
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5bb2974ec39a452496cb98c23384ea81.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

### B：必须在初始化列表进行初始化的成员

- **注意**：以下类型成员必须在初始化列表中初始化，其它类型成员可以放在构造函数体内。但为了统一，对于所有成员我们都会将其放在初始化列表中初始化

**①：`const`成员**

```cpp
class Date
{
            
            
public:
	Date(int year = 1998, int month = 12, int day = 20, int n = 20)
		:_n(n)//const类必须放在初始化列表
	{
            
            
		_year = year;
		_month = month;
		_day = day;
	}
	void print()
	{
            
            
		std::cout << _year << "-" << _month << "-" << _day << "-" << _n << std::endl;
	}
private:
	int _year;
	int _month;
	int _day;

	const int _n;
};

int main()
{
            
            
	Date d1(2000, 2, 2);
	d1.print();
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210305143624549.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

---

**②：引用成员**

 -    **注意**： 之所以输出随机值是因为引用的是形参，在调用下次`print()`时已经销毁了

```cpp
#include <iostream>
using namespace  std;

class Date
{
            
            
public:
    Date(int year=2000, int month=1, int day=1):_year(year), _month(month) ,_day(day), _ref(year)
    {
            
            }

    void DisPlay() const
    {
            
            
        cout << _year << "-" << _month << "-" << _day << "-" << _ref << endl;
    }

private:
    int _year;
    int _month;
    int _day;

    int &_ref;

};

int main(){
            
            
    const Date d1(2012, 12, 20);
    d1.DisPlay();
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F033f03a81c954e26a7edcb64d4a52f92.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

---

**③：自定义类型**

如下，增加一个`Time`类，写上具有一个参数的构造函数；然后在`Date`类中增加 一个`Time`类型成员

```cpp
class Time
{
            
            
public:
	Time(int hour)
		:_hour(hour)
	{
            
            }

private：
	int _hour;
};

class Date
{
            
            
public:
    Date(int year=2000, int month=1, int day=1):_year(year), _month(month) ,_day(day)
    {
            
            }

    void DisPlay()
    {
            
            
        cout << _year << "-" << _month << "-" << _day << "-" << _ref << endl;
    }

private:
    int _year;
    int _month;
    int _day;

    Time _t;

};

int main(){
            
            
    Date d1(2012, 12, 20);
    d1.DisPlay();
}
```

编译器报错，显示没有合适的默认构造函数  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd5936fbc9cae4d2d9237827f7423e11a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

所以在这种情况下，就要我们手动传参，**且必须放在初始化列表**，写法如下

```cpp
#include <iostream>
using namespace  std;


class Time{
            
            
public:
    Time(int hour):_hour(hour){
            
            
        cout << "调用Time构造函数" << endl;
    }
private:
    int _hour;
};

class Date
{
            
            
public:
    Date(int year=2000, int month=1, int day=1, int t = 999):_year(year), _month(month) ,_day(day), _t(t)
    {
            
            }

    void DisPlay()
    {
            
            
        cout << _year << "-" << _month << "-" << _day << "-" << endl;
    }

private:
    int _year;
    int _month;
    int _day;

    Time _t;

};

int main(){
            
            
    Date d1(2012, 12, 20, 2000);
    d1.DisPlay();
}
```

### C：关于初始化列表的一个坑

**使用初始化列表时一定要保证，成员变量在类中的声明次序必须和初始化列表中的初始化顺序一致，否则会出现一些奇怪的错误**

举个例子，下面这段代码理应输出`1, 1`

```cpp
#include <iostream>
using namespace  std;


class A
{
            
            
public:
    A(int a):_a1(a),_a2(_a1)
    {
            
            }
    void print()
    {
            
            
        cout << _a1 << " " << _a2<<endl;
    }
private:
    int _a2;
    int _a1;
};
int main()
{
            
            
    A a(1);
    a.print();
}
```

但运行后结果却成了下面这样  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210305162316303.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)

**这是因为：声明次序先是`_a2`，再是`_a1`，所以初始化时也会按照这样的顺序初始化，导致`_a2`在初始化时获得了随机值**

# 二：匿名对象

**匿名对象**：一般我们在实例化对象时都是这样操作的，如下，该对象的生命周期是在`main`函数之内

```cpp
class A
{
            
            
public:
	A(int a)
	:_a(a);
	{
            
            }
private:
	int _a;
}

int main()
{
            
            
	A aa(1);
}
```

但如果按照下面的方式实例化，**它的生命周期仅仅在一行，从下一行开始它的生命周期也就结束了**，这样的对象称之为**匿名对象**

```cpp
class A
{
            
            
public:
	A(int a)
	:_a(a);
	{
            
            }
private:
	int _a;
}

int main()
{
            
            
	A aa(1);//声明周期是main函数
	A(1);//匿名对象，生命周期仅仅在一行
}
```

# 三：explicit关键字

下面的例子中，在没有重载`=`号的前提下，**却成功把一个整型直接赋值给了一个对象**，这似乎不合乎常理。**实际上，上述例子中，编译器会“偷偷地”用`2019`创造出一个匿名对象，然后用该匿名对象给`d1`进行赋值，这是一种隐式转换**

```cpp
class Date
{
            
            
public:
	Date(int year)
		:_year(year)
	{
            
            }
private:
	int _year;
};
int main()
{
            
            
	Date d1(2020);
	d1 = 2019;//直接用一个整型给对象赋值
}
```

**为了能够禁止这种单参数的构造函数的隐式转换，可以用`explicit`关键字修饰构造函数**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210305190442239.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637695)