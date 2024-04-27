 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：栈基本概念](#_3)
- - [（1）栈的定义](#1_4)
  - [（2）压栈和出栈](#2_21)
  - [（3）进栈出栈变化形式](#3_34)
  - [（4）栈的操作](#4_55)
- [二：队列基本概念](#_73)
- - [（1）队列的定义](#1_75)
  - [（2）入队和出队](#2_86)
  - [（3）队列的操作](#3_100)
- [三：stack和queue使用](#stackqueue_116)
- - [（1）stack](#1stack_119)
  - [（2）queue](#2queue_127)
- [四：适配器](#_164)

# 一：栈基本概念

## （1）栈的定义

**栈\(stack\)：是一种将插入和删除操作限制在一端进行的线性表。其中允许插入和删除的一端称之为栈顶\(top\)，另一端则称之为栈底\(bottom\)。不含任何数据元素的栈称之为空栈。栈中元素遵循后进先出原则**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff911e12aac4b431da074d68826839230.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- 类似于弹夹中的子弹，先压进去的子弹是最后出来的  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0b3ec0b6666b4671b275900173d25046.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- 一些软件（例如Word，Photoshop）其撤销操作本质也是栈结构

## （2）压栈和出栈

**压栈：是栈的插入操作，也叫做进栈、入栈**

**出栈：是栈的删除操作，也叫做弹栈**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F64d3143dcb4841849a2af5358504fbc5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)  
如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd4c2a4df585f4db4b389b07b4030800a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- **进栈顺序**： a 1 a\_\{1\} a1​\-> a 2 a\_\{2\} a2​\-> a 3 a\_\{3\} a3​\-> a 4 a\_\{4\} a4​\-> a 5 a\_\{5\} a5​
- **出栈顺序**： a 5 a\_\{5\} a5​\-> a 4 a\_\{4\} a4​\-> a 3 a\_\{3\} a3​\-> a 2 a\_\{2\} a2​\-> a 1 a\_\{1\} a1​

## （3）进栈出栈变化形式

值得注意的是，栈对线性表的插入和删除是在**位置**上进行了限制，但是并没有对**进出时机**进行限制。也就说，**刚进去的元素也可以立即出栈，只要保证位置是栈顶即可**

比如现在有3个元素 1 1 1、 2 2 2、 3 3 3依次进栈，会有哪些出栈次序呢？

- **第一种：** 1 1 1、 2 2 2、 3 3 3进，然后再 3 3 3、 2 2 2、 1 1 1。出栈次序为 3 3 3 2 2 2 1 1 1
- **第二种**： 1 1 1进、 1 1 1出； 2 2 2进、 2 2 2出； 3 3 3进、 3 3 3出。出栈次序为 123 123 123
- **第三种**： 1 1 1进、 2 2 2进、 2 2 2出、 1 1 1出、 3 3 3进、 3 3 3出。出栈次序为 213 213 213
- **第四种**： 1 1 1进、 1 1 1出、 2 2 2进、 3 3 3进、 3 3 3出、 2 2 2出。出栈次序为 132 132 132
- **第五种**： 1 1 1进、 2 2 2进、 2 2 2出、 3 3 3进、 3 3 3出、 1 1 1出。出栈次序为 231 231 231

列举完毕共有5种情况。除此之外，像 312 312 312这种情况是绝对不可能出现的。因为 3 3 3先出栈，就意味着 3 3 3曾经进栈，既然 3 3 3都进栈了，那么也就意味着 1 1 1和 2 2 2已经进栈了，此时 2 2 2一定是在 1 1 1的上面，出栈只能是 321 321 321

**其实， n n n个不同元素进栈，出栈元素不同排列的个数可由卡特兰\( C a t a l a n Catalan Catalan\)数确定：**  
N = 1 n + 1 C 2 n n N=\\frac\{1\}\{n+1\}C\^\{n\}\_\{2n\} N\=n+11​C2nn​

- 比如上面的3个元素就有 1 3 + 1 C 6 3 = 1 4 × 6 × 5 × 4 3 × 2 × 1 = 5 \\frac\{1\}\{3+1\}C\_\{6\}\^\{3\}=\\frac\{1\}\{4\}×\\frac\{6×5×4\}\{3×2×1\}=5 3+11​C63​\=41​×3×2×16×5×4​\=5种
- 卡特兰数证明有兴趣可以移步[点击跳转](https://blog.csdn.net/qq_40877281/article/details/115662453?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%8D%A1%E7%89%B9%E5%85%B0%E6%95%B0%E8%AF%81%E6%98%8E&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-115662453.nonecase&spm=1018.2226.3001.4187)

## （4）栈的操作

一个栈的基本操作如下

- **`InitList(&L)`**：初始化表
- **`DestoryList(&L)`**：销毁操作
- **`ListInsert(&L,i,e)`**：插入操作
- **`ListDelete(&L,i,&e)`**：删除操作
- **`LocateElem(L,e)`**：按值查找
- **`GetElem(L,i)`**：按位查找

其它常用操作

- **`Length(L)`**：求表长
- **`PrintList(L)`**：输出操作
- **`Empty(L)`**：判空操作

# 二：队列基本概念

## （1）队列的定义

**队列\(Queue\)：是一种只允许在一端插入，在另一端删除的线性表。我们把允许插入的一端称之为队尾\(rear\)，把允许删除的一端称之为队头\(front\)。不含任何数据元素的队列称之为空队列。队列遵循先进先出\(FIFO\)原则**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F50235da9c55d42c38d9db32ccdde8f68.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- 生活中的排队就是典型的队列  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb913d5c223394e4c875449a2c7fedec9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- 有时我们使用电脑时，会突然感觉电脑卡住了，乱点鼠标、乱按键盘也没有什么效果，然后突然它就像清醒了一般，把你刚才的所有操作全部执行了一遍。其实这是因为操作系统中的多个程序需要通过一个通道输出，而按先后次序排队等待造成的

## （2）入队和出队

**入队：是队列的插入操作**

**出队：是队列的删除操作**

如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F430d8595f05d4a8394e26033d9769c84.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

- **入队顺序**： a 1 a\_\{1\} a1​\-> a 2 a\_\{2\} a2​\-> a 3 a\_\{3\} a3​\-> a 4 a\_\{4\} a4​\-> a 5 a\_\{5\} a5​
- **出队顺序**： a 1 a\_\{1\} a1​\-> a 2 a\_\{2\} a2​\-> a 3 a\_\{3\} a3​\-> a 4 a\_\{4\} a4​\-> a 5 a\_\{5\} a5​

## （3）队列的操作

一个队列的基本操作如下

- **`InitQueue(&Q)`**：初始化队列
- **`DestoryQueue(&Q)`**：销毁队列
- **`EnQueue(&Q,x)`**：入队
- **`DeQueue(&Q,&x)`**：出队
- **`GetHead(Q,&x)`**：读队头元素
- **`QueueEmpty(Q)`**：队列判空

---

# 三：stack和queue使用

## （1）stack

**stack：接口如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210415083250172.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4bf1b8712f4944d9bb9d7fd04ff470e4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

## （2）queue

**queue：接口如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210415083426147.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

```cpp
int main() {
            
            
    queue<int> q;
    // 压入元素
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    //查看队头元素
    cout <<  "此时队头元素为：" << q.front() << endl;
    //查看队尾元素
    cout <<  "此时队尾元素为：" << q.back() << endl;
    //出队列
    q.pop();
    cout <<  "此时队头元素为：" << q.front() << endl;
    cout <<  "此时队尾元素为：" << q.back() << endl;
    //返回队列元素个数
    cout << q.size() << endl;
    //判断队列是否为空
    cout << q.empty() << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1f7b424bddec4c93996a44d06793c5b6.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)

# 四：适配器

如下是STL中`stack`的`queue`的源码，只用了不到100行便实现了，属实简短

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416151215347.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)  
有别于之前所学习过的`string`、`vector`等，**`statck`和`queue`并非容器，而是适配器**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416151333876.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675709)  
提到适配器，我们首先想到的就是**电源适配器**，其作用就是将220v的电压转化为适合当前设备的输入电压。之前所学习的`vector`和`list`就等同于这里的220v电压，**其实，这两个容器完全可以完成栈和队列的所有操作**，但是，如果直接拿上这两个容器当作栈来提供给别人使用的话，很容易出现一些`bug`（220v的电可以用来充手机），比如栈的`pop_front`操作。**所以我们必须在原有容器的基础上进行改造，限制某些操作，形成一个新的容器，称之为适配器。所以`stack`和`queue`便是专门为适配栈和队列这两种结构而形成的适配器**

以下是`stack`和`queue`实现的大体逻辑  
**stack**

```cpp
#pragma once

#include <iostream>
using namespace std;

template <class T,class container>

class Mystack
{
            
            
public:
	void push(const T& x)
	{
            
            
		_stack.push_back(x);
	}
	void pop()
	{
            
            
		_stack.pop();
	}
	const T& top()
	{
            
            
		return _stack.back();
	}
	size_t size()
	{
            
            
		return _stack.size();
	}
	bool empty()
	{
            
            
		return _stack.empty();
	}

private:
	container _stack;
};

```

**queue**

```cpp
#pragma once

#include <iostream>
using namespace std;

template <class T, class container>

class Myqueue
{
            
            
public:
	void push(const T& x)
	{
            
            
		_queue.push_back(x);
	}
	void pop()
	{
            
            
		_queue.pop();
	}
	const T& back()
	{
            
            
		return _queue.back();
	}
	const T& front()
	{
            
            
		return _queue.front();
	}
	size_t size()
	{
            
            
		return _queue.size();
	}
	bool empty()
	{
            
            
		return _queue.empty();
	}

private:
	container _queue;
};
```

**大家可以发现，第二个模板参数就是容器，待容器传入后，就会根据这个容器上面封装（其实就是调用）处适合`stack`或`queue`的接口，比如`stack`就没有`push_front`这样的接口，因此也不会封装**

```cpp
#include "mystack.h"
#include <vector>
#include <list>
#include "myqueue.h"

int main()
{
            
            
	Mystack<int, vector<int>> test;
	test.push(1);
	test.push(2);
	test.push(3);
	test.push(4);

	Myqueue<int, list<int>> test1;
	test1.push(1);
	test1.push(2);
	test1.push(3);
	test1.push(4);

}
```

**但是大家可能也发现了，在实际使用STL的`stack`和`queue`的时候，模板参数那里只需要传入第一个就可以了，但是我们现在实现的必须要传入第二个参数，这表明第二个模板参数应该是一个缺省值。但是这第二个参数中默认应该使用哪个容器呢？`vector`还是`list`\?看来都不行，因为他们各自对`stack`和`queue`都有或多或少的局限性。所以会使用一个新的容器叫做双端队列，也就是`deque`**

```cpp
template <class T, class Sequence = deque<T> >
```