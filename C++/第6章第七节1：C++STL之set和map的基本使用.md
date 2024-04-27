 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：set](#set_20)
- - [（1）概述](#1_22)
  - [（2）set的使用](#2set_36)
  - - [A：set常用接口](#Aset_55)
    - [B：set使用示例](#Bset_77)
- [二：map](#map_188)
- - [（1）概述](#1_190)
  - [（2）key-value](#2keyvalue_202)
  - [（3）map的使用](#3map_225)
  - - [A：map的常用接口](#Amap_245)
    - [B：map使用示例](#Bmap_261)
  - [（4）关于map中运算符"\[\]"的重载说明（了解）](#4map_327)
- [三：multiset和multimap](#multisetmultimap_393)
- - [（1）multiset](#1multiset_395)
  - [（2）multimap](#2multimap_413)
- [四：set和map的使用案例](#setmap_431)
- - [（1）最喜欢吃的水果](#1_434)
  - [（2）前k个高频单词](#2k_492)
- [五：Java中的map和set（对比学习）](#Javamapset_525)

**STL中，容器按其底层结构不同，可分为如下两类**

- **序列式容器**：前面的`vector`，`list`等均为序列式容器，其**底层为线性结构**
- **关联式容器**：与序列式容器不同，关联式容器内存储的是**键值对**，也即`<key,value>`结构。

**关联式容器又分为两类：树型结构（底层结构为红黑树）与哈希结构（底层结构为哈希表），它们应用在不同的场景中。其中树形结构又分为如下四种**：

- `map`
- `set`
- `multimap`
- `multiset`

# 一：set

## （1）概述

**set：`set`意味集合，所以它与数学中的集合非常相似，具体来说**

- `map/multimap`中存放是键值对`<key,value>`，而`set`中**只存放`value`**

- `set`中所有每个元素（`value`）都是**唯一的**，所以可以使用set具有**去重**的功能

- `set`中元素**不允许修改**

- `set`中所有元素具有**次序**，其排序规则由**仿函数**控制

- `set`底层结构为**红黑树**，因此在`set`中查找某个元素的**时间复杂度为 l o g 2 \( n \) log\_\{2\}\(n\) log2​\(n\)**

## （2）set的使用

**使用`set`需要引入如下头文件**

```cpp
#include <set>
```

**`set`的模板参数列表如下**

 -    **`class T`**：`set`中存放的数据类型
 -    **`class Compare=less<T>`**：仿函数，`set`中元素默认会按照小于的方式比较
 -    **`class Alloc=allocator<T>`** ：空间配置器

```cpp
template<class T, class Compare=less<T>, class Alloc=allocator<T>> 
```

### A：set常用接口

**set常用接口：如下表**

| 函数声明 | 接口作用 |
| --- | --- |
| `bool empty() const` | 判断`set`是否为空 |
| `size_type size() const` | 返回`set`中有效元素的个数 |
| `pair<iterator,bool> insert(const value type& x)` | 在`set`中插入元素`x`，实际插入了`<x,x>`的键值对，如果成功，返回`<位置,true>`，失败则返回`<位置，false>` |
| `void erase(iterator position)` | 删除某位置元素 |
| `size_type erase(const key_type& x)` | ，删除`set`中值为`x`的元素，返回删除元素个数 |
| `void erase(iterator first,iterator last)` | 删除迭代器区间内的元素 |
| `void clear()` | 将`set`清空 |
| `void swap(set<k,c,a>& st)` | 交换`set`中的元素 |
| `iterator find(const key_type& x)const` | 返回`set`中值为`x`的元素的位置 |
| `sizet_type count (const key_type& x) const` | 统计`set`中值为`x`的元素的个数 |

### B：set使用示例

```cpp
int main()
{
            
            
    // 给出一个具有重复元素的数组，然后根据该数组构造set

    int array[] = {
            
            19, 11, 3, 3, 3, 5, 5, 7, 9};
    set<int> test_set(array, array + sizeof(array) / sizeof(array[0]));
    cout << "根据数组构造set" << endl;
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 插入元素
    cout << "插入元素" << endl;
    test_set.insert(18);
    test_set.insert(21);
    test_set.insert(18);
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 删除元素5
    cout << "删除元素5" << endl;
    test_set.erase(5);
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 找到元素7
    cout << "找到元素：" << *(test_set.find(7)) << endl;
    // 统计元素3的出现次数
    cout << "统计元素3的出现次数：" << test_set.count(3) << endl;
    // 将set清空
    test_set.clear();
}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F591403592f774a9782ab14581243d2ad.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

**如果将上面的`set`换成`multiset`，在不会去重**

```cpp
#include <iostream>
#include <set>
using namespace std;

void print(const multiset<int> &test_set){
            
            
    auto it = test_set.begin();
    while (it != test_set.end())
    {
            
            
        cout << *it << " ";
        ++it;
    }
    cout << endl;
}

int main()
{
            
            
    // 给出一个具有重复元素的数组，然后根据该数组构造set

    int array[] = {
            
            19, 11, 3, 3, 3, 5, 5, 7, 9};
    multiset<int> test_set(array, array + sizeof(array) / sizeof(array[0]));
    cout << "根据数组构造set" << endl;
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 插入元素
    cout << "插入元素" << endl;
    test_set.insert(18);
    test_set.insert(21);
    test_set.insert(18);
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 删除元素5
    cout << "删除元素5" << endl;
    test_set.erase(5);
    print(test_set);
    cout << "------------------------------------------" << endl;
    // 找到元素7
    cout << "找到元素：" << *(test_set.find(7)) << endl;
    // 统计元素3的出现次数
    cout << "统计元素3的出现次数：" << test_set.count(3) << endl;
    // 将set清空
    test_set.clear();
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fedc35d8480614100b64da928e53c9c55.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)  
**`algorithm`模块中虽然也`find`函数，因为`set`中成员函数`find`时间复杂度可以达到 l o g 2 \( n \) log\_\{2\}\(n\) log2​\(n\)**

```cpp
int main(){
            
            
    set<int> test_set;
    test_set.insert(9);
    test_set.insert(8);
    test_set.insert(8);
    test_set.insert(8);
    test_set.insert(7);
    test_set.insert(7);
    test_set.insert(6);

    auto ret1 = test_set.find(7);
    if (ret1 != test_set.end()){
            
            
        cout << "set成员函数find找到" << endl;
    }
    auto ret2 = find(test_set.begin(), test_set.end(), 7);
    if (ret2 != test_set.end()){
            
            
        cout << "algorithm的find找到" << endl;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8a8d0d446d8a4e2ca33e1ff185589de5.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

# 二：map

## （1）概述

**map：`map`意味映射。存储的是由键`key`和值`value`所组合而成的元素，被描述为`pair`类型，注意**

- `key`用于**唯一标识元素和排序**，`value`存储的是**与`key`对应或关联的内容**。`key`和`value`的**类型可以不同**

- `map`支持**下标访问**，也就是`[]`运算符，即`map[key]=value`

- map底层结构为**红黑树**

## （2）key-value

**key-value：** `key-value`本质是一个**结构体**，用来表示**数据与数据之间一一对应的关系**，该结构中一般只包含两个成员变量`key`和`value`。**`key`为键，`value`为键所对应的值**

STL中（SGI版本）关于键值对的定义如下

```cpp
template <class T1, class T2>
struct pair
{
            
             
	typedef T1 first_type; 
	typedef T2 second_type;
	T1 first;
	T2 second;
	 
	pair(): first(T1()), second(T2())
	{
            
            }
	pair(const T1& a, const T2& b): first(a), second(b)
 {
            
            }
};
```

## （3）map的使用

**使用`map`时需要引入如下头文件**

```cpp
#include <map>
```

**`map`的模板参数列表如下**

 -    **`class key`**：`key`的类型
 -    **`class T`**：`value`的类型
 -    **`class Compare=less<T>`**：仿函数，`map`中元素默认会按照小于的方式比较
 -    **`class Alloc=allocator<T>`** ：空间配置器

```cpp
template<class key, class T, class Compare=less<T>, class Alloc=allocator<T>> 
```

### A：map的常用接口

| 函数声明 | 接口作用 |
| --- | --- |
| `bool empty() const` | 判断`map`是否为空 |
| `size_type size() const` | 返回`map`中有效元素的个数 |
| `mapped_type& operator[](const key_type& k)` | 返回`key`对应的`value` |
| `pair<iterator,bool> insert(const value type& x)` | 在`map`中插入键值对`<key,value>` |
| `void erase(iterator position)` | 删除某位置元素 |
| `size_type erase(const key_type& x)` | 删除键为`x`的元素 |
| `void erase(iterator first,iterator last)` | 删除迭代器区间内的元素 |
| `void clear()` | 将`map`清空 |
| `void swap(set<k,t，c,a>& st)` | 交换两个`map` |
| `iterator find(const key_type& x)const` | 返回`map`中`key`为`x`的元素的位置 |
| `sizet_type count(const key_type& x) const` | 返回`map`中`key`为`x`的个数，由于`key`是唯一的，因此其返回值要么是0，要么是1，可以用于判断一个`key`是否在`map`当中 |

### B：map使用示例

**示例1：做如下操作**

- 构造一个空的`map`，然后不断插入`key-value`（使用`pair`的匿名对象构造）

  - 注意也可以使用`make_pair`构造，例如`make_pair("melon","西瓜")`

  - 使用迭代器进行访问，每次访问的是一个`pair`，使用`first`访问它的`key`，使用`second`访问它的`value`

```cpp
int main(){
            
            
    map<string, string> m;
    m.insert(pair<string, string>("melon", "西瓜"));
    m.insert(pair<string, string>("pitaya", "火龙果"));
    m.insert(pair<string, string>("grapefruit", "柚子"));
    m.insert(pair<string, string>("Cherry", "樱桃"));

    map<string, string>::iterator it = m.begin();
    while (it != m.end())
    {
            
            
        cout << "英文中" << it->first << "对应的中文意思是：" << it->second << endl;
        it++;
    }

    cout << "===========================================================================" << endl;
    for (auto& e : m)
    {
            
            
        cout << "英文中" << e.first << "对应的中文意思是：" << e.second << endl;
    }
}
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8a5ba48120e24eab84e97baf0210e53f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

**示例2**：有一个`string`数组，里面含有多个`string`，每个`string`表示一种水果，其中有些`string`是重复的，**统计出有多少种水果，并且每个水果各有多少个**

```cpp
int main(){
            
            
    string fruits[] = {
            
             "苹果", "樱桃", "苹果", "香蕉", "苹果", "樱桃", "梨儿", "火龙果", "香蕉" };
    map<string, int> each_fruit_number;
    for (auto &e : fruits)
    {
            
            
        auto ret = each_fruit_number.find(e);// 查找是否存在key为“e”的元素，如果没有ret将会返回each_fruit_number.end
        if (ret != each_fruit_number.end())//说明存在
        {
            
            
            ret->second++;//相应水果数目增加
        }
        else // 说明不存在
        {
            
            
            each_fruit_number.insert(make_pair(e, 1)); //如果不存在就增加一种水果，初始数量为1
        }
    }

    cout << "一共有" << each_fruit_number.size() << "种水果" << endl;
    for (auto& e : each_fruit_number)
    {
            
            
        cout << e.first << "共有：" << e.second << "个" << endl;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F91fa564c1db84cb4b8d4e8d35a1495f0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

## （4）关于map中运算符"\[\]"的重载说明（了解）

**上面例子中，用于统计水果出现次数的代码，可以简化为下面这样，其中“取`key`所对应`value`”的操作可以直接用"`[]`"代替，也即`map[key] = value`**

```cpp
void test5()
{
            
            
	string fruits[] = {
            
             "苹果","樱桃","苹果","香蕉","苹果","樱桃","梨儿","火龙果","香蕉" };

	map<string, int> each_fruit_number;

	for (auto &f : fruits)
	{
            
            
		each_fruit_number[f]++;
	}

	for (auto &e : each_fruit_number)
	{
            
            
		cout << e.first << "共有：" << e.second << "个" << endl;
	}

}
```

**map中关于运算符"`[]`"的重载是这样完成的**

 -    `mapped_type`：其实就是第二个模板参数`class T`，也即是`value`的类型
 -    `key_type`：其实就是第二个模板参数`class key`，也即是`key`的类型

```cpp
mapped_type& operator[](const key_type &k);
```

**实际运行时，当实参key传递给形参k时，会有两种情形**

- **`key`匹配到了对应的`value`**：此时就会返回`key`对应的`value`
- **`key`没有匹配到对应的`value`**：此时会向该`map`内插入一个新的元素，其`value`也会被设定为`key`，然后返回`value`的引用

**该重载函数的返回值比较复杂**

```cpp
(*((this->insert(make_pair(k,mapped_type()))).first)).second
```

**分解如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210616191310115.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

**也就是该函数内的逻辑实际上是下面这样的**

```cpp
Value& operator[](const K& k)
{
            
            
	pair<iterator,bool> ret=this->insert(k,V());
	return ret.first->second;
}
```

**因此，map中的`[]`主要有两层作用**

- **如果`k`不存在**：插入`pair(k,v)`，然后返回`v`的引用
- **如果`k`存在**：不插入，返回与`k`相等的那个`pair`的`v`的引用

**所以`[]`既可以用来插入，也可以用于修改**

# 三：multiset和multimap

## （1）multiset

**multiset：相较于`set`，`multiset`允许容器内出现重复的`key`。因此`multiset`无法完成去重功能，但仍然可以排序**

```cpp
int main(){
            
            
    int array[] = {
            
            19, 11, 3, 3, 3, 5, 5, 7, 9};
    multiset<int> m_set(array, array + sizeof(array)/sizeof (array[0]));
    for(auto e: m_set){
            
            
        cout << e << " ";
    }
    cout << endl;
    cout << "5出现了" << m_set.count(5) << "次" << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2402796603404ddf861503b09fb10679.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

## （2）multimap

**multimap：相较于`map`，`multimap`允许容器内出现重复的`key`**

```cpp
int main(){
            
            
    multimap<string, string> translate;
    translate.insert(make_pair("compact", "协议"));
    translate.insert(make_pair("compact", "紧密的"));
    translate.insert(make_pair("compact", "压紧"));
    auto iter = translate.begin();
    while (iter != translate.end()){
            
            
        cout << iter->first << "的意思可以是" << iter->second << endl;
        iter++;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7a19bdf54d0c4cf7893309e5cd1cb1df.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

# 四：set和map的使用案例

## （1）最喜欢吃的水果

> 本公司现在要给公司员工发波福利，在员工工作时间会提供大量的水果供员工补充营养。由于水果种类比较多，但是却又不知道哪种水果比较受欢迎，然后公司就让每个员工报告了自己最爱吃的k种水果，并且告知已经将所有员工喜欢吃的水果存储于一个数组中。然后让我们统计出所有水果出现的次数，并且求出大家最喜欢吃的前k种水果

**这里可以使用`map`解决，每种水果会一个数字对应，代表喜欢该种水果的员工数目，然后进行排序即可。但`map`中的排序是按照`key`进行的，所以这里我们可以把`map`中每个`pair`的迭代器放入一个数组中，使用`sort`对数组进行排序，排序准则由仿函数给出**

```cpp
#include <iostream>
#include <set>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;

int main(){
            
            
    string fruits[] = {
            
             "苹果", "樱桃", "苹果", "香蕉", "苹果", "樱桃", "梨儿", "火龙果", "香蕉", "西瓜", "西瓜", "西瓜", "西瓜"};
    map<string, int> fruit_number;
    // 统计每个水果的出现次数
    for (auto &f : fruits){
            
            
        fruit_number[f]++;
    }
    for (auto &e : fruit_number){
            
            
        cout << e.first << ": " << e.second << "次" << " ";
    }
    cout << endl;

    // 存储map中pair的迭代器
    vector<map<string, int>::iterator> v;
    auto iter = fruit_number.begin();
    while (iter != fruit_number.end()){
            
            
        // 迭代器入数组
        v.push_back(iter);
        iter++;
    }
    // 仿函数，根据<string, int>中int进行比较
    struct Compare{
            
            
        bool operator()(const map<string, int>:: iterator it1, const map<string, int>:: iterator it2){
            
            
            return it2->second < it1->second; //降序
        }
    };

    // 排序
    sort(v.begin(), v.end(), Compare());

    // 输出出现次数最多的k个水果
    int k = 2;
    for (auto &e: v){
            
            
        cout << e->first << endl;
        k--;
        if (k == 0)
            break;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9289c18461c34fb6a2191dca6f696d4b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

## （2）前k个高频单词

- [LeetCode链接：692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/description/)

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5318e4114d8b46bb97a58bc00b603773.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F118616590)

**这个问题不能直接使用快速排序，因为不稳定，可能会导致最后的输出顺序有无。但是在建立`map`的时候它是按照字母顺序建立的，所以咋建立完毕之后可以再利用一个`multimap`进行排序**

```cpp
vector<string> topFrequent(vector<string> &words, int k){
            
            
    map<string, int> words_number;
    // 统计单词次数
    for(auto &e: words){
            
            
        words_number[e]++;
    }
    // 根据出现次数对单词进行排序，有可能单词不同但次数一直，所以使用multimap
    multimap<int, string, greater<int>> m_sort;
    for(auto &e: words_number){
            
            
        m_sort.insert(make_pair(e.second, e.first));
    }
    vector<string> ret;
    auto iter = m_sort.begin();
    while(k--){
            
            
        ret.push_back(iter->second);
        ++iter;
    }
    return ret;
}
```

# 五：Java中的map和set（对比学习）

- 由于内容较多，所以这里不便展开，感兴趣可以点击：[第十四章第七节2：Java集合框架之map和set](https://zhangxing-tech.blog.csdn.net/article/details/127215050)