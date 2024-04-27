 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：C语言的动态内存管理方式](#C_4)
- [二：C++内存管理方式](#C_15)
- - [（1）new和delete](#1newdelete_17)
  - - [①：针对内置类型](#_22)
    - [②：针对自定义类型](#_76)
  - [（2）new和delete使用示例](#2newdelete_135)
  - [（3）operator new和operator delete函数](#3operator_newoperator_delete_185)

# 一：C语言的动态内存管理方式

**此部分内容是C语言中的重点，这里我们只做简单回顾**

**`malloc()`函数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F202103110937545.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)  
**`calloc()`函数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311093913446.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)  
**`realloc()`函数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311093957819.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)  
**`free()`函数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311094029192.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)

# 二：C++内存管理方式

## （1）new和delete

**new和delete：C++中，会使用关键字`new`和`delete`完成对空间的申请和释放工作**

### ①：针对内置类型

**针对内置类型：如果申请的是内置类型，`new`/`delete`和`malloc`/`free`作用等价。需要注意的是**

- `new/delete`申请和释放的是**单个元素**
- `new[]`和`delete[]`申请和释放的是**连续的空间**
- `new`在申请空间失败时会**抛出异常**
- `malloc`在申请空间失败时会**返回`NULL`**

**演示**

①：申请**一个字节**的空间

```cpp
#include <iostream>
using namespace std;

int main()
{
            
            
	//C语言的申请方式
	int* ptr1 = (int*)malloc(sizeof(int));
	free(ptr1);
	
	//C++的申请方式
	int* ptr2 = new int;//申请方式1：申请不初始化
	int* ptr3 = new int(10);//申请方式2：申请并初始化为10
	delete(ptr2);//注意不是free
	delete(ptr3);
}
```

②：申请**整型数组**

 -    **注意**：释放数组时写法为`delete[]`

```cpp
#include <iostream>
using namespace std;

int main()
{
            
            
	//C语言的申请方式
	int* ptr1 = (int*)malloc(sizeof(int) * 10);
	free(ptr1);

	//C++的申请方式
	int* ptr2 = new int[10];
	delete[] ptr2;//注意释放数组的写法
}
```

### ②：针对自定义类型

**针对自定义类型：`new`和`delete`在自定义类型中才能发挥出它真正的作用。因为`new`和`delete`不止负责开辟和释放空间，而且还负责调用构造和析构函数，具体来说**

**①：`new`的原理**

- 调用`operator new`函数申请空间
- 在申请的空间上**执行构造函数，完成对象的构造**

**②：`delete`的原理**

- 在空间上**执行析构函数，完成对象的资源清理工作**
- 调用`operator delete`函数释放对象空间

**③：`new class[N]`的原理**

- 调用`operator new[]`函数，在`operator new[]`中实际调用`operator new`函数完成N个对象的空间的申请
- 在申请的空间上执行**N次构造函数**

**④：`delete[]`的原理**

- 在需要释放的对象空间上执**行N次析构函数，完成N个对象的资源的清理**
- 调用`operator delete[]`释放空间，实际在`operator delete[]`中调用了`operator delete`释放空间

例如下面有一个日期类`Date`

```cpp
class Date
{
            
            
public:
	Date()
		:_year(1998)
		,_month(12)
		,_day(20)
	{
            
            
		cout << "调用了构造函数" << endl;
	}
	~Date()
	{
            
            
		cout << "调用了析构函数" << endl;
	}
	

private:
	int _year;
	int _month;
	int _day;
};
```

如果使用`malloc`和`free`，会发现控制台什么都没有输出，且初始值为随机值，这表明没有调用构造和析构函数。**也就是说`malloc`和`free`只负责开辟空间和释放空间**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031110142662.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)

如果使用`new`和`delete`，会发现控制台输出了内容，且调用了构造和析构函数。**也就是说，`new`和`delete`不止负责开辟空间和释放空间，而且还会调用构造和析构函数**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311102332743.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)

## （2）new和delete使用示例

**`new`和`delete`可以自动调用构造和析构函数，这意味着对于自定义类型我们不再需要关心对象的构造和清理工作，这些编译器可以帮助我们完成。这可以大大简化代码逻辑，使代码风格简明，而且还可以避免内存泄漏等风险**

下面两份代码分别是单链表在C语言和C++中的实现

```c
typedef struct ListNode
{
            
            
	struct ListNode* _next;
	int _val;
}ListNode;
void CreatNode(int val)
{
            
            
	ListNode* NewNode=(ListNode*)malloc(sizeof(ListNode));
	NewNode->next=NULL;
	NewNode->_val=val;
	return NewNode;
}

int main()
{
            
            
	ListNode* NewNode=CreatNode(3);

}
```

```cpp
struct ListNode
{
            
            
	ListNode(int val)//构造函数
		:_val(val)
		,next(NULL);
	{
            
            }

	int _val;//成员变量
	ListNode* next;

};
int main()
{
            
            
	ListNode* NewNode=new ListNode(3);

}

```

## （3）operator new和operator delete函数

- **注意**：`operator new`和`operator delete`不是`new`和`delete`的重载

**`operator new`和`operator delete`是C++的全局函数，`new`在底层调用`operator new`来申请空间，`delete`在底层调用`operator delete`来释放空间。他们的关系如下图所示**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312102736631.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114652313)