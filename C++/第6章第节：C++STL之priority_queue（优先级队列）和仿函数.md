 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：priority\_queue（优先级队列）](#priority_queue_3)
- - [（1）堆与堆排序](#1_9)
  - [（2）基本使用](#2_14)
  - [（3）“TOP K” 问题](#3TOP_K__44)
  - [（4）模拟实现](#4_87)
- [二：仿函数](#_232)
- - [（1）仿函数是什么](#1_234)
  - [（2）使用仿函数完成大顶堆和小顶堆的构建](#2_268)

# 一：priority\_queue（优先级队列）

**priority\_queue（优先级队列）：在头文件中，除了基本的`queue`外，还有一个特殊的`priority_queue`，翻译过来是优先级队列的意思，其本质是数据结构中的堆。堆有一个特性就是它的根节点存储的永远是一组数据中最大或最小的那一个元素**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416195320860.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

## （1）堆与堆排序

**在使用`priority_queue`前，还是希望你能够熟知堆和堆排序的概念，由于内容较多，所以这里不便展开，具体见下面这篇文章**

- [数据结构-堆与堆排序](https://blog.csdn.net/qq_39183034/article/details/113618253)

## （2）基本使用

**`priority_queue`相关接口及含义如下表所示，特别注意`priority_queue`默认会构建大顶堆**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021041619583555.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

**使用举例**

```cpp
int main() {
            
            
    priority_queue<int> priorityQueue;
    // 压入元素
    priorityQueue.push(2);
    priorityQueue.push(8);
    priorityQueue.push(3);
    priorityQueue.push(1);
    priorityQueue.push(6);
    priorityQueue.push(4);

    while(!priorityQueue.empty()){
            
            
        cout << priorityQueue.top() << endl;
        priorityQueue.pop();
    }

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff6529b12d8a64f4cbe6455b8306f4fdd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

## （3）“TOP K” 问题

**“TOP K” 问题：在未排序的数组找到第K个最大的元素**

- [LeetCode 215：数组中的第 k 大的数字](https://leetcode.cn/problems/xx4gT2/)

**解决思路**：此题最容易想到的一种方法就是对数组**排升序**，然后找到**第K个元素**即可，思路可行，但如果数据量很大，那么**时间复杂度会非常多**，导致超出时间限制。所以此题可以用**堆**解决，**我们可以先将前K个数建立为小顶堆，然后从第K+1个数开始依次和堆顶元素比较。如果大于堆顶元素就入堆，然后再次调整，最终所有元素比较完时，堆里面保存的就是前K大的元素，并且堆顶就是第K个大的元素**

**代码如下，需要注意，由于`priority_queue`默认建立的是大顶堆，如果要建立小顶堆，则需要使用仿函数来告诉`priority_queue`应该如何比较，这一点我们会在后面说明**

**`priority_queue`构造函数完整形式如下**

```cpp
priority_queue<_Tp, _Sequence, _Compare>
```

- **`_Tp`**：数据类型
- **`_Sequence`**：底层容器的类型，默认为`vector<_Tp>`。如果`_Compare`给出，则`_Sequence`必须显式给出
- **`_Compare`**：仿函数，用于指定堆中元素的排序规则，默认为`less_Sequence::value_type`，将会建立大顶堆

**代码如下**

```cpp
class Solution {
            
            
public:
    int findKthLargest(vector<int>& nums, int k) {
            
            
        // 先对前k个数构建小顶堆
        priority_queue<int, vector<int>, greater<int>> min_heap(nums.begin(), nums.begin()+k);
        // 从第k+1数开始，依次和堆顶元素比较，如果大于，那么就入堆
        for(int i = k; i < nums.size(); i++){
            
            
            if(nums[i] > min_heap.top()){
            
            
                min_heap.pop();
                min_heap.push(nums[i]);
            }
        }
        //最后堆内元素保存的是前k大元素，堆顶元素自然是第k大元素
        return min_heap.top();
    }
};
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F99abfce37da948acaf5e8ed27f44202c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

## （4）模拟实现

**模拟实现：实现时底层结构采用`vector`，建立大顶堆**

 -    **`push`（堆的插入）**：插入到最后一个结点，然后使用向上调整算法调整为堆
 -    **`pop`（堆的删除）**：将根节点交换到数组最后一个位置处，然后删除，接着从根节点开始利用向下调整算法调整为堆

```cpp
#include <iostream>
#include <vector>
#include <cassert>

using namespace std;
template<class T, class Container = vector<T>>

class my_priority_queue
{
            
            
public:
    //向下调整算法
    void AdjustDown(int root)
    {
            
            
        size_t parent = root;
        size_t child = 2 * root + 1;
        while (child < _con.size())
        {
            
            
            if (child + 1 < _con.size() && _con[child + 1] > _con[child])
                child++;
            if (_con[child] > _con[parent])
            {
            
            
                swap(_con[child], _con[parent]);
                parent = child;
                child = 2 * parent + 1;
            }
            else
            {
            
            
                break;
            }
        }
    }

    //向上调整算法
    void AdjustUp(int child)
    {
            
            
        int parent = (child - 1) / 2;
        while (child > 0)
        {
            
            
            if (_con[child] > _con[parent])
            {
            
            
                swap(_con[child], _con[parent]);
                child = parent;
                parent = (child - 1) / 2;
            }
            else
            {
            
            
                break;
            }
        }
    }

    //入堆
    void push(const T& x)
    {
            
            
        _con.push_back(x); //直接插入到数组最后处，然后从该元素开始向上调整
        AdjustUp(_con.size()-1);
    }

    //出堆
    void pop()
    {
            
            
        assert(_con.size() > 0);//防止堆已经空了，还出堆
        swap(_con[0], _con[_con.size() - 1]);//把堆顶元素放到数组末尾然后pop，接着从根节点开始向下调整
        _con.pop_back();
        AdjustDown(0);
    }

    //获取堆顶元素
    const T& top()
    {
            
            
        return _con[0];
    }

    size_t size()
    {
            
            
        return _con.size();
    }
private:
    Container _con;
};
```

**测试如下，可以发现它已经实现了优先级队列的基本功能**

```cpp
int main(){
            
            
    my_priority_queue<int> myPriorityQueue;
    myPriorityQueue.push(2);
    myPriorityQueue.push(5);
    myPriorityQueue.push(1);
    myPriorityQueue.push(7);
    myPriorityQueue.push(9);
    myPriorityQueue.push(3);

    while(myPriorityQueue.size() != 0){
            
            
        cout << myPriorityQueue.top() << " ";
        myPriorityQueue.pop();
    }

    cout << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F41cd1a1063de4d4283e4897571fd2535.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

**关于它的构造函数需要特别说明一下，我们构建堆的一般方式是利用一个数组，从最后一个非叶结点开始依次调用向下调整算法，所以这里要使用区间构造，模板参数选用选用`InputIterator`**

```cpp
 // 区间构造
template<class InputIterator>
my_priority_queue(InputIterator first, InputIterator last) :_con(first, last){
            
            
    for(int i = (_con.size() - 1 ) / 2; i >= 0; i--){
            
            
        AdjustDown(i);
    }
}

int main(){
            
            
    vector<int> v = {
            
            2, 5, 1, 7, 9, 3};
    my_priority_queue<int> myPriorityQueue(v.begin(), v.end());

    while(myPriorityQueue.size() != 0){
            
            
        cout << myPriorityQueue.top() << " ";
        myPriorityQueue.pop();
    }

    cout << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F29b50a27e3564dc08596546775065d59.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

# 二：仿函数

## （1）仿函数是什么

**仿函数：仿函数实则是一个类的对象，它重载了运算符`（）`，因此在使用时就好像在调用函数。如下是一个仿函数的例子**

```cpp
template<class T>
class imitate{
            
            
public:
    bool operator()(const T &a, const T &b){
            
            
        return a < b;
    }
};

int main(){
            
            
    //仿函数对象
    imitate<int> test;
    //该类在调用函数时就像直接调用一般
    if(test(1, 2)){
            
            
        cout << "1小于2" << endl;
    }else{
            
            
        cout << "1大于等于2" << endl;

    }

    return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa7d2fb25fdcf43b1a020d4228d30f9c7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)

**在`priority_queue`中如果需要建立小顶堆，则传入`greater(int)`即可**

## （2）使用仿函数完成大顶堆和小顶堆的构建

**我们分别建立`Greater`（大顶堆）和`Less`（小顶堆）两个类，然后在优先级队列模板参数那里加入参数`class Compare=Greater<int>`，表明默认建立的是大顶堆，然后分别在向上调整和向下调整算法中实例化一个`Compare`类的对象`cmp`（具体是`Greater`还是`Less`，要看传入的参数）。于是凡是比较大小的地方都会调用类中重载的`（）`，这样就可以根据外部传入的类型，来具体决定生成大顶堆还是小顶堆**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210417200541275.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115715046)