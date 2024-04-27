 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

# 三：list的实现

## （1）成员变量，无参构造，push\_back

list作为链表，其成员变量肯定是一个结点，所以要写出它的结点类型  
如下，在C语言中我们是这样定义的

```c
struct DlistNode
{
            
            
	struct DlistNode* next;
	struct DlstNode* prev;
	DataType val;
}
```

而在C++中，要注意使用类模板，其中的struct本质也是一个类。  
除了正常的成员变量外，大家可能注意到了，这个struct里面增添了构造函数，这个构造函数类似于在C语言中的申请结点的那个函数，**但是这样写更加方便，因为在C++中直接使用new还能自动调用其构造函数**，同时注意形参的写法

```cpp
struct __Mylist_node
{
            
            
	__Mylist_node(const T& x = T())
		:__value(x)
		, __next(nullptr)
		, __prev(nullptr)
	{
            
            }
public:
	T __value;
	__Mylist_node<T>* __next;
	__Mylist_node<T>* __prev;

};
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409145658678.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

对于list，它是一个双向带头结点的链表，所以链表为空时其实就是只有一个头结点head

```cpp
	Mylist()//无参构造
	{
            
            
		_head=new node();
		_head->__next=_head;
		_head->__prev=_head	;
	}
```

写尾插时还是之前讲过的那种思路，这里也就能看出C++的方便之处，一个new直接就可以申请结点并初始化

```cpp
	void push_back(const T& x)
	{
            
            
		node* newnode=new node(x);
		newnode->__next=_head;
		(_head->__prev)->next=newnode;
		newnode->__prev=_head->__prev;
		_head->__prev=newnode;
	}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409150257732.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

## （2）迭代器实现

list中的迭代器和vector中的迭代器很不一样，简单来说vector可以理解为原生的指针，**而list的迭代器为了满足一些简易的操作（比如说++，–，因为对于链表指针不能像数组那样连续操作），对指针进行了封装，重载了一些操作，形成了list的迭代器**

既然list中的迭代器是指针的封装，那么要建立一个关于迭代器的结构体或类，STL源码中也是这样做的，但是它竟然有三个模板参数，第一个参数毋庸置疑，无需解释，但第二个和第三个参数究竟是干什么用的我们首先在这里不直接说明，先用一个参数去实现

首先，迭代器的本质是指针，所以其成员变量一定是一个指向结点的指针

```cpp
template <class T>
struct _Mylist_iterator
{
            
            
	typedef _Mylist_node<T> node;

public:
	node* __Mylist_pointer;
};
```

然后咋类里面咋们把它的名字命名为iterator，我们知道每个迭代器都会有两个接口，分贝是begin，和end，可以写上它，**但是注意begin是头结点的下一个结点，而end是头结点**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409151343854.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
这里函数的返回值是iterator，而iterator本质就是咋们封装的结点指针，函数的返回值也就是结点指针，所以对应起来了。  
此时外界使用list迭代器时应该是这样的（假设类型是int）：`Mylist<int>::iterator it=test.begin()`，这里it实则就是一个对象，那么这样的操作其实就是赋值，而赋值和构造函数编译器会默认生成，但是iterator作为Mylist的内部类，或者是自定义类型，必须我们手动定义构造函数（复习一下）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409153753606.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
特别要注意迭代器内部不要写析构函数，因为这个指针不是迭代器的资源

好的，现在应该实现哪些迭代器的操作呢，说白了也就是一些指针的操作，回忆迭代器的遍历操作

```cpp
Mylist<int>::iterator it=test.begin()
while(it!=test.end())
{
            
            
	cout<<*it;
	++it;
}
```

所以像`!=` `==`，前置，后置`++`，解引用等都是必须重载的，为了书写方便我们迭代器类型定义为self，`typedef _Mylist_iterator<T> self;`

```cpp
bool operator!=(const self& s) const
	{
            
            
		return __Mylist_pointer!=s.__Mylist_pointer;
	}
	bool operator==(const self& s) const
	{
            
            
		return !(*this!=s);
	}
	//解引用
	T& operator*()
	{
            
            
		return __Mylist_pointer->__val;
	}
	//后置++
	self& operator++()
	{
            
            
		__Mylist_pointer=__Mylist_pointer->__next;
		return *this;
	}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040915463489.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
写后置++和后置–的时候形参加入一个类型，目的是让后置++和后置–与前置++和前置–构成函数重载

```c
//后置
self operator++()
{
            
            
	self temp(*this);//先保存
	++(*this)//复用前置++
	return temp;
}
```

STL的list迭代器中还重载了一个`->`运算符，它返回的是一个指针，对于内置类型用处不大，但是对于自定义类型访问时就可以使用取成员的方式访问

```cpp
T* operator->()
{
            
            
	return &__Mylist_pointer->__val;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411104920892.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

比如对于自定义类型POINT，其内部含有两个变量x和y，如果重载了`->`，那么使用迭代器时，迭代器就像一个结构体指针，就能通过这样的方式访问到内部变量了，可读性明显很强

最后回到最开始的问题，STL中的list关于迭代器，有三个模板参数，那么第二个和第三个模板参数的作用到底是什么呢？  
在vector中，我要实现一个const的迭代器，直接`typedef const T const_iterator`就可以了，但是在我们的这个list中呢，如果要实现一个const的迭代器，由于迭代器是被封装的，所以你必须再复制一份出来，但是这样的操作很明显不划算，因为涉及const的只有两个接口。所以我们可以在模板参数上做手脚，我们将迭代器的模板参数列表定义为 **`template <class T,class Ref,class Ptr>`，其中第二个和第三个分别指的是引用和指针。** 然后在类中定义const\_iterator，对应的参数列表改为const类型即可  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409170411650.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409170519534.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
所以  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409171938250.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210409172001686.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

## （3）插入和删除

插入和删除的逻辑实现在这里就不过多重复了，插入返回的是插入后的元素的位置的迭代器，删除时返回的是下一个元素的迭代器，注意删除时不能删除头结点。

```cpp
iterator insert(iterator pos, const T& x)
{
            
            
	node* newnode = new node(x);
	node* current = pos.__Mylist_pointer;
	node* prev = current->__prev;

	newnode->__next = current;
	newnode->__prev = prev;
	prev->__next = newnode;
	current->__prev = newnode;

	return newnode;
}
iterator erase(iterator pos)
{
            
            
	assert(pos != (*this).end());

	node* current = pos.__Mylist_pointer;
	node* prev = current->__prev;
	node* next = current->__next;

	prev->__next = next;
	next->__prev = prev;

	return next;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411113305473.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
尾插在end之前插入，头插在bengin之前插入

```cpp
void push_back(const T& x)
{
            
            
	insert((*this).end(), x);

}
void push_front(const T& x)
{
            
            
	insert((*this).begin(), x);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411114048418.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
尾删是删除end之前的元素，头删是删除begin

```cpp
void pop_back()
{
            
            
	erase(--((*this).end()));
}
void pop_front()
{
            
            
	erase((*this).begin());
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411115111485.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)

## （4）拷贝构造，区间构造，赋值重载

list的拷贝构造仍然会涉及到深浅拷贝的问题，拷贝构造的目的是生成一个和原来一模一样且独立的对象，所以需要使用深拷贝。实现时我们可以复用尾插，然后把原来对象的元素依次尾插至新的元素

```cpp
Mylist(const Mylist<T>& lt)
{
            
            
	_head = new node();
	_head->__next = _head;
	_head->__prev = _head;

	auto it = lt.begin();
	while (it != lt.end())
	{
            
            
		push_back(*it);
		++it;
	}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411122256264.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
区间构造如下

```c
template<class Inputiterator>
Mylist(Inputiterator first, Inputiterator last)
{
            
            
	_head = new node();
	_head->__next = _head;
	_head->__prev = _head;

	while (first != last)
	{
            
            
		push_back(*first);
		first++;
	}

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411124417770.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
有了区间构造后，就可以使用拷贝构造的现代写法了

```cpp
Mylist(const Mylist<T>& lt)
{
            
            
	_head = new node();
	_head->__next = _head;
	_head->__prev = _head;

	Mylist<T> temp(lt.begin(), lt.end());
	
	swap(_head,temp._head);//头结点交换

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411130157715.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)  
最后，赋值重载如下

```cpp
Mylist<T>& operator=(Mylist<T> lt)
{
            
            
	_head = new node();
	_head->__next = _head;
	_head->__prev = _head;


	swap(_head, lt._head);

	return *this;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210411130539728.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115486493)