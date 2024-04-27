 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：C语言实现](#C_4)
- [二：C++实现](#C_270)
- [三：Java实现](#Java_466)
- [四：Python实现](#Python_646)

# 一：C语言实现

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1dce1cb849ee4cf89a71cf22f940ed4f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F126864028)

`SeqList_test.c`

```c
#include "SeqList.h"

//测试插入删除元素
void Test1(SeqList* ps)
{
            
            
	SeqListInit(ps);//初始化
	SeqListPushBack(ps, 1);
	SeqListPushBack(ps, 2);
	SeqListPushBack(ps, 3);
	SeqListPushBack(ps, 4);
	SeqListPushBack(ps, 5);//尾部插入依次元素
	SeqListPrint(ps);

	SeqListPopBack(&s);
	SeqListPopBack(&s);
	SeqListPopBack(&s);//尾部依次删除元素
	SeqListPrint(&s);

	SeqListPushFront(&s, 9);
	SeqListPushFront(&s, 8);
	SeqListPushFront(&s, 7);
	SeqListPushFront(&s, 6);
	SeqListPushFront(&s, 5);//头部依次插入元素
	SeqListPrint(&s);

	SeqListPopFront(&s);
	SeqListPopFront(&s);
	SeqListPopFront(&s);//头部依次删除元素
	SeqListPrint(&s);

	SeqListInsert(&s, 1, 5);
	SeqListInsert(&s, 1, 6);//向指定位置插入元素
	SeqListPrint(&s);

	SeqListErase(&s, 1);
	SeqListErase(&s, 1);//把指定位置元素删除
	SeqListPrint(&s);

}
//测试接受用户输入删除指定元素删除
void Test2(SeqList* ps)
{
            
            
	printf("请输入要删除的元素\n");
	int res = 0;
	scanf_s("%d", &res);
	int index = SeqListFind(ps, res);
	if (index == -1)
		printf("没有此元素");
	else
	{
            
            
		printf("删除后为\n");
		SeqListErase(ps, index);
		SeqListPrint(ps);
	}
}
int main()
{
            
            
	SeqList s;
	Test1(&s);//测试插入删除元素
	Test2(&s);//测试用户指定元素删除
	return 0;
}

```

`"SeqList.h"`

```c
#pragma once
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>
typedef int SLDataType;

typedef struct SeqList
{
            
            
	SLDataType* Array;
	int Size;//有效数据个数
	int Capacity;//容量大小
}SeqList;

void SeqListInit(SeqList* ps);//初始化
void SeqListDestory(SeqList* ps);//销毁

void SeqListPrint(SeqList* ps);//屏幕输出操作
void SeqListCheckCapacity(SeqList* ps);//检查容量是否满了


void SeqListPushBack(SeqList* ps, SLDataType x);//尾插（可以与任意位置插入复用）
void SeqListPopBack(SeqList* ps);//尾删除（可以与任意位置删除复用）
void SeqListPushFront(SeqList* ps, SLDataType x);//头插（可以与任意位置插入复用）
void SeqListPopFront(SeqList* ps);//头删（可以与任意位置删除复用）
void SeqListInsert(SeqList* ps, int pos, SLDataType x);//任意位置插入
void SeqListErase(SeqList* ps, int pos);//任意位置删除

int  SeqListFind(SeqList* ps, SLDataType x);//使用顺序查找法查找某个元素的位置
int  SeqListFindvalue_Bind(SeqList* ps, int pos);//使用二分查找法查找某个元素的位置

```

`SeqList.c`

```c
#include "SeqList.h"

void SeqListInit(SeqList* ps)//初始化
{
            
            
	/*也可以这样申请
	s.Size = 0;
	s.Array = NULL;
	s.Capacity = 0;
	*/
	ps->Array = (SLDataType*)malloc(sizeof(SLDataType) * 4);
	if (ps->Array == NULL)
	{
            
            
		printf("申请失败\n");
		exit(-1);
	}
	ps->Size = 0;
	ps->Capacity = 4;
}
void SeqListDestory(SeqList* ps)//销毁
{
            
            
	free(ps->Array);
	ps = NULL;
	ps->Capacity = ps->Size = 0;
}
void SeqListPrint(SeqList* ps)//打印
{
            
            
	assert(ps);
	for (int i = 0; i < ps->Size; ++i)
	{
            
            
		printf("%d ", ps->Array[i]);
	}
	printf("\n");


}
void SeqListCheckCapacity(SeqList* ps)//检查容量是否满了
{
            
            
	if (ps->Size == ps->Capacity)
	{
            
            
		ps->Capacity *= 2;
		ps->Array = (SLDataType*)realloc(ps->Array, sizeof(SLDataType)*ps->Capacity);
		if (ps->Array == NULL)//扩容失败
		{
            
            
			printf("扩容失败\n");
			exit(-1);
		}
	}
	

}

void SeqListPushBack(SeqList* ps, SLDataType x)//尾插
{
            
            
	/*尾插就是size位置上的插入
	assert(ps);//警告，指针为空
	SeqListCheckCapacity(ps);//满了就扩容
	ps->Array[ps->Size] = x;//size表示有效个数，但同时是下一个元素的下标
	ps->Size++;//元素加1
	*/
	SeqListInsert(ps,ps->Size, x);
}
void SeqListPopBack(SeqList* ps)//尾删除
{
            
            
	assert(ps);
	ps->Size--;

}
void SeqListPushFront(SeqList* ps, SLDataType x)//头插
{
            
            
	/*头插是0位置上的插入
	assert(ps);
	SeqListCheckCapacity(ps);//满了就扩容
	for (int i = ps->Size-1; i >= 0; i--)//头插法：从最后一个元素开始，依次向后移，把0的位置空出来
	{
		ps->Array[i+1] = ps->Array[i];
	}
	ps->Array[0] = x;
	ps->Size++;
	*/
	SeqListInsert(ps, 0, x);

}
void SeqListPopFront(SeqList* ps)//头删
{
            
            
	/*头删是0位置的删除
	assert(ps);
	for (int i = 0; i < ps->Size; i++)
	{
		ps->Array[i] = ps->Array[i + 1];//删除时直接覆盖即可
	}
	ps->Size--;
	*/
	SeqListErase(ps, 0);
	

}
void SeqListInsert(SeqList* ps, int pos, SLDataType x)//任意位置插入
{
            
            
	assert(ps);
	assert(pos <= ps->Size && pos>= 0);//插入位置有误
	SeqListCheckCapacity(ps);
	for (int i = ps->Size - 1; i >= pos; i--)//从插入位置以后依次向后挪动
	{
            
            
		ps->Array[i + 1] = ps->Array[i];
	}
	ps->Array[pos] = x;
	ps->Size++;

}
void SeqListErase(SeqList* ps, int pos)//任意位置删除
{
            
            
	assert(ps);
	assert(pos < ps->Size && pos >= 0);//删除位置有误
	for (int i = pos; i < ps->Size; i++)
	{
            
            
		ps->Array[i] = ps->Array[i + 1];
	}
	ps->Size--;
}

int  SeqListFind(SeqList* ps, SLDataType x)//顺序查找法
{
            
            
	assert(ps);
	int i = 0;
	while (i<ps->Size)
	{
            
            
		if (ps->Array[i] == x)
			return i;
		++i;
	}
	return -1;


}
int  SeqListFindvalue_Bind(SeqList* ps, int pos)//二分查找法（前提要排序）
{
            
            
	int low = 0;
	int high = ps->Size - 1;
	while (low < high)
	{
            
            
		int mid = (low + high) / 2;
		if (pos < ps->Array[mid])
			high = mid - 1;
		else if (pos > ps->Array[mid])
			low = mid + 1;
		else
			return mid;
	}
}

```

# 二：C++实现

 -    C++中顺序表对应STL中的vector，所以这里以实现vector为例
 -    有关语法细节可以学习：[C++教程](https://blog.csdn.net/qq_39183034/category_10813323.html?spm=1001.2014.3001.5482)

```cpp
#pragma once
 
#include<iostream>
#include<Windows.h>
#include<assert.h>
using namespace std;
//使用模板
template<class T>
class vector{
            
            
public:
	typedef T * iterator;
	typedef const T* const_iterator;
	//迭代器
	iterator begin(){
            
            
		return _start;
	}
	iterator end(){
            
            
		return _finish;
	}
	const_iterator cbegin(){
            
            
		return _start;
	}
	const_iterator cend(){
            
            
		return _finish;
	}
	
	vector()
		:_start(nullptr)
		, _finish(nullptr)
		, _endofstorage(nullptr)
	{
            
            }
	vector(size_t n, const T& val = T())
		:_start(nullptr)
		, _finish(nullptr)
		, _endofstorage(nullptr)
	{
            
            
		reserve(n);
		while (_finish != (_start + n)){
            
            
			*_finish = val;
			_finish++;
		}
 
	}
 
	//不好调用Swap函数，不能调用拷贝构造函数，只能调构造函数
	vector(const vector<T>& v)
		:_start(nullptr)
		, _finish(nullptr)
		, _endofstorage(nullptr)
	{
            
            
		reserve(v.capacity());
		for (size_t i = 0; i < v.size(); i++){
            
            
			*_finish = v[i];
			_finish++;
		}
 
	}
 
	vector<int>& operator=(vector<int> v){
            
            
		Swap(v);
		return *this;
 
	}
	~vector(){
            
            
		delete[] _start;
		_start = _finish = _endofstorage = nullptr;
	}
 
	T& operator[](size_t pos){
            
            
		assert(pos < size());
		return _start[pos];
	}
	const T& operator[](size_t pos)const{
            
            
		assert(pos < size());
		return _start[pos];
	}
 
	void push_back(const T& val){
            
            
		//if (_finish == _endofstorage){
            
            
		//	size_t newcapacity = capacity() == 0 ? 2 : 2 * capacity();
		//	reserve(newcapacity);
		//}
		//*_finish = val;
		//_finish++;
		insert(_finish, val);
 
	}
 
 
	iterator insert(iterator pos, const T& val){
            
            
		assert(pos <= _finish);
		if (_finish == _endofstorage){
            
            
			//扩容后pos位置迭代器失效了，记录位置，重新更新pos位置
			size_t sz = pos - _start;
			size_t newcapacity = capacity() == 0 ? 2 : 2 * capacity();
			reserve(newcapacity);
			//更新pos位置
			pos = _start + sz;
		}
		
		iterator end = _finish - 1;
		while (end >= pos){
            
            
			*(end + 1) = *end;
			end--;
		}
		
		*pos = val;
		_finish++;
		return pos;
	}
 
	void pop_back(){
            
            
		assert(_finish != _start);
		//_finish--;
 
		erase(_finish - 1);
	}
 
	iterator erase(iterator pos){
            
            
		assert(pos < _finish);
 
		iterator tmp = pos;
		while (tmp < _finish){
            
            
			*tmp = *(tmp + 1);
			tmp++;
		}
		_finish--;
		return pos;
	}
 
 
	size_t size()const{
            
            
		return _finish - _start;
	}
 
	size_t capacity()const{
            
            
		return _endofstorage - _start;
	}
 
	vector<T>& reserve(size_t n){
            
            
		if (n > capacity()){
            
            
			size_t oldsize = size();
			T *tmp = new T[n];
			if (_start){
            
            
				//使用赋值，调用T的赋值操作符重载函数
				for (size_t i = 0; i < size(); i++){
            
            
					tmp[i] = _start[i];
				}
				delete[] _start;
			}
			_start = tmp;
			_finish = _start + oldsize;
			_endofstorage = _start + n;
	
		}
		return *this;
	}

	void resize(size_t n, const T& val = T()){
            
            
		if (n > size()){
            
            
			if (n > capacity()){
            
            
                //如果进来没有容量
				size_t newcapacity = capacity() == 0 ? 2 : 2 * capacity();
				reserve(newcapacity);
			}
			while (_finish != (_start + n)){
            
            
				*_finish = val;
				_finish++;
			}
		}
		else{
            
            
			_finish = _start + n;
		}	
	}
 
	void Swap(vector<int>& v){
            
            
		swap(_start, v._start);
		swap(_finish, v._finish);
		swap(_endofstorage, v._endofstorage);
	}
 
private:
	iterator _start;
	iterator _finish;
	iterator _endofstorage;
};
```

# 三：Java实现

- Java中顺序表对应其集合框架中的ArrayList，所以这里以实现ArrayList为例
- 有关语法细节可以学习：[JavaSE教程](https://blog.csdn.net/qq_39183034/category_11869604.html?spm=1001.2014.3001.5482)
- 相比C++来说，我更喜欢Java实现，因为它更面向对象也更安全

`MyArrayList.java`

```java
import javafx.geometry.Pos;

import java.awt.font.TextMeasurer;
import java.util.Arrays;

public class MyArrayList {
            
            
    public int[] elem;//核心数组
    public int usedSize;//记录有效数据个数
    public static final int DefaultCapacity = 2;//默认顺序表容量

    //构造函数
    public MyArrayList(){
            
            
        this.elem = new int[DefaultCapacity];
    }

    //打印顺序表
    public void display(){
            
            
        for(int i = 0; i < this.usedSize; i++){
            
            
            System.out.print(elem[i] + " ");
        }
        System.out.println();
    }

    //新增元素（默认新增在最后）
    public void add(int data){
            
            
        //如果数组已经满了，那么进行扩容（2倍）
        if(isFull()){
            
            
            elem = Arrays.copyOf(elem, 2*elem.length);//可查看Arrays用法
        }
        elem[usedSize] = data;
        usedSize++;
    }

    //新增元素（在pos位置新增）
    public void add(int pos, int data){
            
            
        try{
            
            
            //检查位置是否合法
            checkAddPos(pos-1);
            //如果数组已经满了，那么进行扩容（2倍）
            if(isFull()){
            
            
                elem = Arrays.copyOf(elem, 2*elem.length);//可查看Arrays用法
            }
            //如果合法则继续执行
            for(int i = usedSize-1; i>=pos-1; i--){
            
            
                elem[i+1] = elem[i];
            }
            elem[pos-1] = data;
            usedSize++;
        }catch (PosIndexNotLegalException e){
            
            
            //如果非法抛出异常后捕获
            e.printStackTrace();
        }

    }

    //判断顺序表是否满
    public boolean isFull(){
            
            
        return usedSize == elem.length;
    }

    //检查add元素时，pos位置是否合法？
    private void checkAddPos(int pos){
            
            
        if(pos < 0 || pos > usedSize){
            
            
            throw  new PosIndexNotLegalException("所给位置非法");//抛出异常
        }
    }

    //检查get元素时，pos位置是否合法？（注意区别上面，因为add元素时，pos位置是可以等于usedSize的）
    private void checkGetPos(int pos){
            
            
        if(pos < 0 || pos >= usedSize)
            throw new PosIndexNotLegalException("所给位置非法");
    }

    //判断是否包含某个元素
    public boolean contains(int data){
            
            
        for(int i = 0; i < usedSize; i++){
            
            
            if(elem[i] == data)
                return true;
        }
        return false;
    }

    //返回某个元素对应位置，如果返回-1，表示该元素不存在
    public int indexOf(int data){
            
            
        for(int i = 0; i < usedSize; i++){
            
            
            if(elem[i] == data)
                return i;
        }
        return -1;
    }

    //获取pos位置的元素
    public int get(int pos){
            
            
        int retVal = -1;
        try{
            
            
            checkGetPos(pos-1);
            retVal = elem[pos-1];
        }catch (PosIndexNotLegalException e) {
            
            
            e.printStackTrace();
        }
        return retVal;
    }

    //将pos位置处的元素设置为data
    public void set(int pos, int data){
            
            
        checkGetPos(pos-1);
        elem[pos-1] = data;
    }

    //删除第一次出现的data
    public void del(int data){
            
            
        //首先检查此元素是否存在
        int index = indexOf(data);
        if(index == -1){
            
            
            System.out.println("此元素不存在");
            return;
        }
        for(int i = index; i <usedSize-1; i++){
            
            
            elem[i] = elem[i+1];
        }
        usedSize--;
    }

    //销毁顺序表
    public void clear(){
            
            
        usedSize = 0;
    }
}

```

`PosIndexNotLegalException.java`：这是一个自定义异常，用于抛出位置越界

```java
public class PosIndexNotLegalException extends RuntimeException{
            
            
    public PosIndexNotLegalException(){
            
            

    }
    public PosIndexNotLegalException(String msg){
            
            
        super(msg);
    }
}

```

`TestDemo.java`：这是测试文件，类`MyArryList`将在此处实例化并测试

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        MyArrayList myArrayList = new MyArrayList();
        myArrayList.add(25);
        myArrayList.add(456);
        myArrayList.add(54);
        myArrayList.add(44);
        myArrayList.add(89);
        myArrayList.display();
        myArrayList.add(4,67);
        myArrayList.display();
        System.out.println(myArrayList.get(4));
        System.out.println(myArrayList.indexOf(67));
    }
}
```

# 四：Python实现

```python
class SeqList(object):
    def __init__(self,max = 10):
        self.max = max #默认顺序表最多容纳10个元素
        #初始化顺序表数组
        self.num = 0
        self.data = [None] * self.max #占位了10个
    
    def is_empty(self): #判断线性表是否为空
        return self.num is 0
    
    def is_full(self): #判断线性表是否全满
        return self.num is self.max
    
    def __getitem__(self,index):#获取线性表中某一位置的值
        if not isinstance(index,int):
            raise TypeError
        if 0 <= index <self.max:
            return self.data[index]
        else:
            raise IndexError
    def __setitem__(self,index,value): # 修改线性表中的某一位置的值
        if not isinstance(index,int):
            raise TypeError
        if 0<=index<self.max:
            self.data[index] = value
        else:
            raise IndexError
    
    def locate_item(self,value): #按值查找第一个等于该值得索引
        for i in range(self.num):
            if self.data[i] == value:
                return i
        return -1
    
    def count(self): # 返回线性表中元素的个数
        return self.num
    
    def append_last(self,value): #在表尾部插入一个元素
        if self.num > self.max:
            print("list is full")
        else:
            self.data[self.num] = value
            self.num += 1
            
    def insert(self,index,value):  # 在表中任意位置插入一个元素
        if self.num>=self.max:
            print("list is full")
        if not isinstance(index,int):
            raise TypeError
        if index<0 or index>self.num:
            raise IndexError
        print('num :',self.num)
        for i in range(self.num,index,-1):
            self.data[i]=self.data[i-1]
        self.data[index]=value
        self.num += 1
    
    def remove(self,index): #删除表中某一位置的值
        if not isinstance(index,int):
            raise TypeError
        if index < 0 or index >= self.num:
            raise IndexError
        for i in range(index,self.num):
            self.data[i] = self.data[i+1]
        self.num -= 1
    
    
    def print_list(self): #输出操作
        for i in range(0,self.num):
            print(self.data[i])
    
    def destroy(self):
        self.__init__() 

```