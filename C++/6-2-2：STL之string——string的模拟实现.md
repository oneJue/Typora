 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [实现](#_10)
- - [（1）构造和析构](#1_11)
  - [（2）拷贝构造-深浅拷贝问题](#2_50)
  - - [A：默认拷贝构造的问题（浅拷贝）](#A_51)
    - [B：深拷贝](#B_62)
  - [（3）赋值重载](#3_88)
  - [（4）迭代器](#4_131)
  - [（5）push\_back，append和+=](#5push_backappend_146)
- [代码](#_157)

# 实现

## （1）构造和析构

首先来说构造，经过前面的讲解，大家可能也发现了我们最常使用的构造对象的方式是这样的：`string test_string("this is a test")`，也就是用常量字符串的方式赋值，而常量字符串是不能修改的，所以这里必须拷贝一份，注意要多申请一个空间，不要忘记最后一个字符串结束标志了

```cpp
class Mystring
{
            
            
public:
	Mystring(const char* str= "")//const类型
		:_str(new char[strlen(str)+1])
	{
            
            
		strcpy(_str, str);//拷贝过去
	}

private:
	char* _str;
};

int main()
{
            
            
	Mystring test("test string");
	return 0;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324150631741.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

- **需要注意的是构造函数的默认参数不能写成空指针，这样将导致一些内存错误。写成空字符串，C语言规定空字符串里面默认有一个’\\0’**

析构函数也比较简单

```cpp
	~Mystring()
	{
            
            
		delete[] _str;
		_str = nullptr;
	
	}
```

## （2）拷贝构造-深浅拷贝问题

### A：默认拷贝构造的问题（浅拷贝）

我们知道，拷贝构造作为C++的6个默认成员函数之一，如果用户没有显示定义拷贝构造函数，那么C++会自动生成一个默认的拷贝构造函数  
对于数值型来说，无非就是复制的操作了，对于指针则稍有不同。比如下面，利用test对象来拷贝构造对象test1，可以发现两个对象的字符串指针指向的地址竟然是同一个  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324152646136.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
**其实C++默认拷贝构造遵循的方式是浅拷贝，浅拷贝就是一种值拷贝。两个对象他们的各自用于一个指针变量，但是指针变量的值却是相同的，如果将运算符`[]`重载后，你会发现一个对象字符的修改也会引起另一个对象的修改。很明显这样拷贝的话，这两个对象就不能算作两个不同的对象**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032415571714.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

**对字符串来说，这种浅拷贝最大的弊端就在于析构。因为两个指针变量指向同一片空间，所以析构时就会导致最后一个析构的对象在析构时其指向空间已经被释放了，而在C++中是不允许对已经释放的空间再次释放的，容易造成程序崩溃**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324153700141.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
所以对于string，list这些类型，必须使用深拷贝，而深拷贝编译器是不能帮我们实现的，需要我们自己手动实现

### B：深拷贝

深拷贝的实现也很简单，如下

```cpp
	Mystring(const Mystring& s)
		:_str(new char[strlen(s._str)+1])
	{
            
            
		strcpy(_str, s._str);
	}
```

当使用深拷贝后，也就没有上面的那些问题了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324155749904.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

需要注意的是赋值重载，也就是`=`也会涉及到深浅拷贝的问题，所以如果不重载`=`，而直接使用C++默认的那个，也会造成相同的问题  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324162519418.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
上面的写法是传统写法，像string这类简单的容器，在拷贝构造时非常简单，但是像链表，二叉树这些稍微复杂的数据结构使用这种方式就显得有点麻烦。**所以如今的现代写法是这样的**

```cpp
Mystring(const Mystring& s)
	:_str(nullptr)
{
            
            
	string temp(s._str);
	swap(_str,temp);
}
```

**其基本思路是，以`Mystring s2(s1)`为例，就是使用s1的字符串先去构造一个对象temp，然后把temp和s2为例，并且释放s2的原来的空间（要注意要将`s2._str`置空，这样释放空指针就不会出现问题）**

## （3）赋值重载

赋值重载的深拷贝的不能直接：`strcpy(_str,s._str)`，因为被赋值的那个空间可能会不够，所以我们可以上来先把原来的空间释放掉，然后重新申请足够大小的空间，再进行拷贝，同时需要注意判断不要自己给自己赋值

```cpp
Mystring& operator=(const Mystring& s)
	{
            
            
		if (this != &s)
		{
            
            
			delete[] _str;
			_str = new char[strlen(s._str) + 1];
			strcpy(_str, s._str);
		}
		return *this;
	}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F202103241649465.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

而现代写法是这样写的

```cpp
Mystring& operator=(const Mystring& s)
{
            
            
	if(this!=&s)
	{
            
            
		string temp(s);
		swap(_str,temp.str);
	}
	return *this;
}
```

举个例子，`Mystring s1("Hello")` `Mystring s2("World")`,然后`s1=s2`会调用咋们写好的赋值重载，调用时`s2`传给形参`s`，然后在函数内部，使用`s`拷贝构造一个临时的对象`temp`，这样temp就指向了`s2`，然后把`s1`和`temp`交换，这样`s1`就拥有了`s2`的内容，同时`temp`就指向了`s1`，结束`temp`会把`s1`原来的空间给释放掉

其实更为简洁的写法是这样的

```cpp
Mystring& operator=(Mystring s)
{
            
            
	swap(_str,s._str);
	return *this;
}
```

这里要注意形参没有用引用，以上面那两个对象为例，这样写也就意味着传参时`s`就是`s2`的拷贝，然后直接进行交换，`s1`就得到了和`s2`相同的内容，而形参开辟的空间也被释放了

## （4）迭代器

前面我们将string的一些基本操作的时候，说到过迭代器，哪里只是在讲迭代器究竟怎么用，而现在我们就可以自己实现迭代器，从而明白迭代器究竟是什么东西  
string的迭代器其实本质上就是一个指针  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324200451524.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032420063450.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
现在你应该觉得迭代器没有那么神奇了吧

**指针就是最天然的迭代器，而迭代器的实现却不仅仅单纯的只涉及到指针（比如链表），迭代器的最大作用就是使得用户对这些容器的访问趋于统一操作，会用一个容器的迭代器，那么其余的也就八九不离十了。**

还记得前面我们展示过迭代器的两种重载形式吗  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324203057502.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
没错，上面展示的就是第一种。如果我们把上面的遍历操作封装为一个函数`print`，形参由于为了保护所以修饰为`const`类型的，那么再按上面的操作写迭代器时就会出现错误  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324203228640.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
我们需要做的就是搞出一个`const char*`的迭代器，注意begin和end函数也修饰为const  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324203643355.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

## （5）push\_back，append和+=

push\_back，append和+=都属于追加，一旦追加就有可能设计到增容，所以首先我们要模拟实现一个reserve  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032421141659.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
然后是push\_back;  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324211623192.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

然后是append  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324211931653.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)  
最后是+=，其实通过实现string类，可以发现+=本质还是调用的push\_back和append  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210324212705648.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)

# 代码

**//string.h**

```cpp
#pragma once
#define _CRT_SECURE_NO_WARNINGS 1
#include <iostream>
#include <assert.h>
#include <string.h>
using namespace std;

class Mystring
{
            
            

//友元函数，输入输出
friend ostream& operator<<(ostream& out, const Mystring& s);
friend istream& operator>>(istream& in, Mystring& s);

public:
	typedef char* iterator;


	Mystring(const char* str="");//构造函数
	~Mystring();//析构函数
	Mystring(const Mystring& s);//拷贝构造
	Mystring& operator=(Mystring s);//重载=


	iterator begin()//迭代器
	{
            
            
		return _str;
	}
	iterator end()//迭代器
	{
            
            
		return _str + _size;
	}


	//修改操作
	void push_back(char ch);//追加一个字符
	void append(const char* str);//追加一个字符串
	Mystring& operator+=(char ch);//重载+=
	Mystring& operator+=(const char* str);//重载+=
	void clear();//清空
	void Swap(Mystring& s);//交换两个对象



	//容量操作
	size_t size()const;//返回size
	size_t capacity()const;//返回capacity
	bool empty()const;//判断字符串是否为空
	void reserve(size_t New_capacity);//扩容不初始化
	void resize(size_t NewSize, char c = '\0');//设置size并初始化

	//访问操作
	char& operator[](size_t index);//重载[]
	const char& operator[](size_t index)const;//重载[]
	const char* c_str() const;//返回字符数组指针
	//关系判断
	bool operator<(const Mystring& s);//重载<
	bool operator<=(const Mystring& s);//重载<=
	bool operator>(const Mystring& s);//重载>
	bool operator>=(const Mystring& s);//重载>=
	bool operator==(const Mystring& s);//重载==
	bool operator!=(const Mystring& s);//重载!=


	//其他操作
	size_t find(char c, size_t pos = 0)const;//返回指定字符第一次出现的位置
	size_t find(const char* s, size_t pos = 0)const;//返回第一个匹配的字符串
	Mystring& insert(size_t pos, char c);//在某个位置插入一个字符
	Mystring& insert(size_t pos, const char* str);//某个位置插入一个字符串
	Mystring& erase(size_t pos, size_t len=-1);


private:
	char* _str;
	size_t _size;
	size_t _capacity;
};

```

**//string.cpp**

```cpp
#include "string.h"


Mystring::Mystring(const char* str)
{
            
            
	_size = strlen(str);
	_capacity = _size;
	_str = new char[_capacity + 1];
	strcpy(_str, str);


}


Mystring::~Mystring()
{
            
            
	delete[] _str;
	_str = nullptr;
}
Mystring::Mystring(const Mystring& s)
	:_str(nullptr)
{
            
            
	Mystring temp(s._str);
	swap(_str, temp._str);
	swap(_size,temp._size);
	swap(_capacity, temp._capacity);
}
Mystring& Mystring::operator=(Mystring s)
{
            
            
	swap(_str, s._str);
	swap(_size,s._size);
	swap(_capacity,s._capacity);

	return *this;



}

void Mystring::reserve(size_t New_capacity)
{
            
            
	if (New_capacity > _capacity)
	{
            
            
		char* temp = new char[New_capacity + 1];
		strcpy(temp, _str);
		delete[] _str;
		_str = temp;
		_capacity =New_capacity;
	}


}
void Mystring::push_back(char ch)
{
            
            
	if (_size == _capacity)
	{
            
            
		reserve(2 * _capacity);
	}
	_str[_size] = ch;
	_size += 1;
	_str[_size] = '\0';
}
void Mystring::append(const char* str)
{
            
            
	size_t len = strlen(str);
	if (_size + len > _capacity)
	{
            
            
		reserve(_size + len);
		
	}
	strcpy(_str + _size, str);
	_size += len;
}

Mystring& Mystring::operator+=(char ch)
{
            
            
	push_back(ch);
	return *this;
}
Mystring& Mystring::operator+=(const char* str)
{
            
            
	append(str);
	return *this;

}
void Mystring::clear()
{
            
            
	strcpy(_str, "");
	_size = 0;
	_capacity = 1;

}
void Mystring::Swap(Mystring& s)
{
            
            
	if (this != &s)
	{
            
            
		
		swap(_str, s._str);
		swap(_size, s._size);
		swap(_capacity, s._capacity);
	}
}

const char* Mystring::c_str() const
{
            
            
	return _str;
}

size_t Mystring::size()const
{
            
            
	return _size;
}
size_t Mystring::capacity()const
{
            
            
	return _capacity;
}
bool Mystring::empty()const
{
            
            
	if (strlen(_str) == 0)
	{
            
            
		return true;//空
	}
	else
	{
            
            
		return false;//不空
	}

}

void Mystring::resize(size_t NewSize, char c)
{
            
            
	if (NewSize < _size)//如果小于原先的空间
	{
            
            
		_str[NewSize] = '\0';//消减到这里
		_size = NewSize;
	}
	else//如果大于原先的空间
	{
            
            
		if (NewSize > _capacity)//如果大于空间
			reserve(NewSize);//扩容
		for (size_t i = _size; i < NewSize; i++)
		{
            
            
			_str[i] = c;//放进字符
		}
		_str[NewSize] = '\0';
		_size = NewSize;
	}
}
char& Mystring::operator[](size_t index)
{
            
            
	return _str[index];
}
const char& Mystring::operator[](size_t index)const
{
            
            
	return _str[index];
}
bool Mystring::operator<(const Mystring& s)
{
            
            
	if (strcmp(_str, s._str) < 0)
	{
            
            
		return true;
	}
	else
	{
            
            
		return false;
	}
}
bool Mystring::operator==(const Mystring& s)
{
            
            
	if (strcmp(_str, s._str) == 0)
		return true;
	else
		return false;

}
bool Mystring::operator<=(const Mystring& s)
{
            
            
	
	if (*this < s||*this == s)
	{
              
              
		return true;
	}
	else
	{
              
              
		return false;
	}
}
bool Mystring::operator>(const Mystring& s)
{
            
            
	if (!(*this <= s))
	{
            
            
		return true;
	}
	else
	{
            
            
		return false;
	}

}
bool Mystring::operator>=(const Mystring& s)
{
            
            
	if (!(*this < s))
		return true;
	else
		return false;
}
bool Mystring::operator!=(const Mystring& s)
{
            
            
	if (!(*this == s))
		return true;
	else
		return false;

}
ostream& operator<<(ostream& out, const Mystring& s)//输出，友元
{
            
            
	for (size_t i = 0; i < s.size(); i++)//输出所有有效字符
	{
            
            
		out << s[i];
	}
	return out;
}
istream& operator>>(istream& in,  Mystring& s)//输入，友元
{
            
            
	s.resize(0);
	char ch;
	while (1)
	{
            
            
		in.get(ch);
		if (ch == ' ' || ch == '\n')
			break;
		else
			s += ch;
	}
	return in;
}
size_t Mystring::find(char c, size_t pos)const

{
            
            
	for (size_t i = 0; i < this->size(); i++)
	{
            
            
		if ((*this)[i] == c)
		{
            
            
			return i;
		}
	}
	return -1;

}

size_t Mystring::find(const char* s, size_t pos)const
{
            
            
	/*
	字符串匹配，使用KMP算法解决，详解
	这篇博客 https://blog.csdn.net/qq_39183034/article/details/115139682?spm=1001.2014.3001.5502

 */

	//首先生成next数组
	size_t i = 0;
	int k = -1;
	int* next = new int[strlen(s)];
	next[0] = -1;
	while (i < strlen(s) - 1)
	{
            
            
		if (k==-1 || s[i] == s[k])
		{
            
            
			i++;
			k++;
			next[i] = k;
		}
		else
		{
            
            
			k = next[k];
		}
	}

	//下面是KMP算法的代码
	if (s == "")
		return 0;//如果子串是空的，返回0
	int m = (*this).size();
	int n = strlen(s);

	int  mm = 0;
	int  nn = 0;
	while (mm < m && nn < n)
	{
            
            
		if (nn == -1 || (*this)[mm] == s[nn])
		{
            
            
			mm++;
			nn++;
		}
		else
		{
            
            
			nn = next[nn];
		}
	}
	if (nn >= strlen(s))
	{
            
            
		return mm - strlen(s);//找到返回了索引
	}
	else
	{
            
            
		return -1;//找不到返回下标
	}
}
Mystring& Mystring::insert(size_t pos, char c)
{
            
            
	if (_size >=_capacity)
		reserve(2 * _capacity);
	for (size_t i = _size ; i > pos; i--)//注意整形提升问题
	{
            
            
		_str[i] = _str[i-1];
	}
	_str[pos] = c;
	_size += 1;
	_str[_size] = '\0';//防止后面输出乱码

	return *this;

}
Mystring& Mystring::insert(size_t pos, const char* str)
{
            
            
	while (_size >= _capacity)
		reserve(2 * _capacity);
	for (size_t i = 0; i < strlen(str); i++)//一个字符一个字符插入
	{
            
            
		char save = str[i];
		insert(pos, save);
		pos++;
	}
	return *this;


}

Mystring& Mystring::erase(size_t pos, size_t len)
{
            
            
	if (len == -1 || pos + len >= _size)//直接删到末尾
	{
            
            
		_str[pos] = '\0';//阻断
		_size = pos;
	}
	else
	{
            
            
		strcpy(_str + pos, _str + pos + len);
		_size -= len;
	}
	return *this;
}
```

**//test.cpp**

```cpp
#include "string.h"



void test1()//测试默认成员函数
{
            
            
	cout << "测试默认成员函数" << endl;
	cout << endl;
	cout << endl;
	cout << endl;

	cout << "构造函数:生成test" << endl;
	Mystring test("this is a test");
	cout <<"test:"<< test << endl;
	cout << endl;

	cout << "拷贝构造函数:由test生成test1" << endl;
	Mystring test1(test);
	cout << "test1:" << test1 << endl;
	cout << endl;

	cout << "赋值重载=  test3=test1" << endl;
	Mystring test3;
	test3 = test1;
	cout << "test3:" << test << endl;
	cout << endl;



}

void  test2()//测试修改操作
{
            
            
	Mystring test("Hello");
	Mystring test1("World");
	cout << "测试修改操作。测试用例为test："<<test<<"  test1:"<<test<<endl;
	cout << endl;
	cout << endl;
	cout << endl;

	cout << "test+=一个字符" << endl;
	test += 'y';
	cout << "test:" << test << endl;
	cout << endl;

	cout << "test+=一个字符串" << endl;
	test += " AAAA bbbb";
	cout << "test:" << test << endl;
	cout << endl;

	cout << "清空test" << endl;
	test.clear();
	cout << "test:" << test << endl;
	cout << endl;


	cout << "交换test和test1" << endl;
	test.Swap(test1);
	cout << "test:" << test << endl;
	cout << "tes1:" << test1 << endl;
	cout << endl;

}

void test3()
{
            
            
	Mystring test("Hello");
	Mystring test1("World");
	cout << "测试修改操作。测试用例为test：" << test << "  test1:" << test << endl;
	cout << endl;
	cout << endl;
	cout << endl;

	cout << "使用[]遍历test" << endl;
	for (size_t i = 0; i < test.size(); i++)
	{
            
            
		cout << test[i]<<"   ";
	}
	cout << endl;

	cout << "使用迭代器iterator遍历test1" << endl;
	Mystring::iterator p = test1.begin();
	while (p != test1.end())
	{
            
            

		cout << *p << "   ";
		p++;
	}

	cout << endl;

	cout << "输出test对象的字符串首地址" << endl;
	printf("%p\n", test.c_str());


}

void test4()
{
            
            
	Mystring test("Hello");
	Mystring test1("World");
	cout << "测试修改操作。测试用例为test：" << test << "  test1:" << test << endl;
	cout << endl;
	cout << endl;
	cout << endl;

	cout << "输出test的第一个l出现的位置" << endl;
	cout << test.find('l');
	cout << endl;

	cout << "输出test1的 or 出现的位置" << endl;
	cout << test1.find("or");
	cout << endl;

	cout << "在test的字母e后面插入字母x" << endl;
	test.insert(2, 'x');
	cout << "test:" << test << endl;
	cout << endl;

	cout << "在test1的字母r后面插入字符串this" << endl;
	test1.insert(2, "this");
	cout << "test1:" << test1 << endl;
	cout << endl;


	cout << "删除test的最后两个字母lo" << endl;
	test.erase(4);
	cout << "test:" << test << endl;
	cout << endl;

	

}

int main()
{
            
            
	test1();//测试默认成员函数
	test2();//测试修改操作
	test3();//访问操作和迭代器
	test4();//其他操作
}
```

**//测试情况**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210327154334176.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115004333)