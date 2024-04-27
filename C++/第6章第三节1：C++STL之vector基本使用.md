 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：vector介绍](#vector_4)
- [二：vector的常用接口](#vector_26)
- - [（1）构造函数](#1_27)
  - [（2）迭代器](#2_111)
  - [（3）容量操作](#3_169)
  - [（4）元素访问](#4_190)
  - [（5）增删查改](#5_209)
- [三：Java中的ArrayList（对比学习）](#JavaArrayList_292)
- - [（1）介绍](#1_293)
  - [（2）使用](#2_305)
  - - [A：构造方法](#A_308)
    - [B：常见操作](#B_337)
    - [C：ArrayList遍历](#CArrayList_410)
    - [D：ArrayList扩容](#DArrayList_476)

# 一：vector介绍

**vector：和数组一样，`vector`也采用连续的空间来存储元素，这意味着`vector`可以用下标对元素访问。与普通数组不同之处在于，其大小是动态可变的**

**其定义如下，可以发现`vector`是一个模板**

 -    **注意**：第二个模板参数表**示内存池**，是一种池化技术，是为了避免反复向操作系统申请空间，此阶段学习时可以忽略它

```cpp
template < class T, class Alloc = allocator<T> > class vector; // generic template
```

**[官方文档](https://cplusplus.com/reference/vector/vector/)中给出了vector的各种操作，和`string`非常相似**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210331195011740.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**`vector`作为类模板可以承载很多数据类型**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210331195945686.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

# 二：vector的常用接口

## （1）构造函数

**构造函数：C++98中`vector`有如下5种构造函数，这里我们只给出常用的构造函数示例**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210331200705492.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

---

**①`vector()`：无参构造，`vector`内什么都没有**

```cpp
int main(){
            
            
    vector<int> vec;
    cout << vec.size() << endl;
    cout << vec.capacity() << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9935d665a7c54a73bb694a1906c5b600.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**②`vector(size_type n,const value& val=value_type())：` 构造并初始化`n`个`value`**

```cpp
int main(){
            
            
    vector<int> vec(4, 10);
    for(int i = 0; i < vec.size(); i++){
            
            
        cout << vec[i] << " ";
    }
    cout << endl;
    cout << vec.size() << endl;
    cout << vec.capacity() << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8ad341c087764eed8012512a59057115.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**③`vector(const vector& x);` 拷贝构造**

```cpp
int main(){
            
            
    vector<string> vec(4, "str");
    vector<string> vec_copy(vec);
    for(int i = 0; i < vec_copy.size(); i++){
            
            
        cout << vec_copy[i] << " ";
    }

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5bd229d8578b457f927647d1ae21604b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**④`vector(inputlterator first.inputlterator last);` 使用迭代器构造，范围是`[first,last)`**

 -    **注意**：不止可以使用自己的迭代器，而且可以使用别的容器的迭代器

```cpp
int main(){
            
            
    string str("hello world");
    vector<char> vec_char(str.begin(), str.end());
    for(int i = 0; i < vec_char.size(); i++){
            
            
        if(i == 5){
            
            
            cout << " ";
        }else{
            
            
            cout << vec_char[i];
        }
    }
    cout << endl;
    
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F52ab38881fb1405bb39539f98240656e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

## （2）迭代器

**注意反向迭代器（`reverse_iterator`）`rbegin`是从尾部开始的，遍历时是`++`，不是`--`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210331205309377.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

```cpp
//普通迭代器
void normal_iterator(){
            
            
    vector<int> vec(5 ,2);
    vector<int>::iterator it = vec.begin();

    while(it != vec.end()){
            
            
        (*it) = 1;
        cout << (*it) << " ";
        it++;
    }
    cout << endl;
}

//const对象需要使用const迭代器
void const_iterator(){
            
            
    const vector<int> vec(5 ,2);
    vector<int>:: const_iterator it = vec.begin();

    while(it != vec.end()){
            
            
        cout << (*it) << " ";
        it++;
    }
    cout << endl;
}

//反向迭代器
void reverses_iterator(){
            
            
    int arr[] = {
            
            1, 2, 3, 4};
    vector<int> vec(arr, arr + sizeof(arr)/sizeof(int));
    vector<int>::reverse_iterator it = vec.rbegin();

    while(it != vec.rend()){
            
            
        cout << (*it) << " ";
        it++;
    }
    cout << endl;
}


int main(){
            
            
    normal_iterator();
    const_iterator();
    reverses_iterator();

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8e064410ffb349628454cefe959e39d9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

## （3）容量操作

- 相比于`reserve`，`vector`更多使用的是`resize`

**`void resize (size_type n, value_type val = value_type())`：将所有元素设置为`n`个`val`**

```cpp
int main(){
            
            
    vector<int> test;
    test.resize(10, 1);
    vector<int>::iterator it = test.begin();
    while(it != test.end()){
            
            
        cout << (*it) << endl;
        it++;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F64a6d33cee71497cbd6479fc0573e17b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

## （4）元素访问

**`operator []`：可以和普通数组一样使用`[]`访问元素**

```cpp
int main(){
            
            
    int arr[] = {
            
            1, 2, 3, 4, 5};
    vector<int> test(arr, arr+sizeof(arr)/sizeof(int));
    for(int i = 0; i < test.size(); i++){
            
            
        cout << test[i] << " ";
    }
    cout << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fde3be9b1ddaf4ea49302dadb297d0ba2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

## （5）增删查改

**①`find(iterator begin, iterator end, val)`：查找元素，查找成功返回第一次查找成功时的迭代器，查找失败则返回末尾的迭代器**

 -    **注意**：`find`并不是`vector`的接口，属于STL的算法模块（`algorithm`）

```cpp
int main(){
            
            
    int arr[] = {
            
            11, 22, 33, 44, 55, 66, 77, 88, 99};
    vector<int> test(arr, arr+sizeof(arr)/sizeof(int));
    vector<int>::iterator pos = find(test.begin(), test.end(), 55);

    if(pos != test.end()){
            
            
        cout << "查找成功" << endl;
    }else{
            
            
        cout << "查找失败" << endl;
    }
    cout << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdbf62b0d805a4942921f8a84e94ac0a2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**②`insert(iterator position, const value_type &val)`：在迭代器指定位置插入元素**

 -    **迭代器失效**：对于元素的插入、删除等操作有时会导致迭代器失效。因为迭代器本质是一个指针，在插入和删除会可能会使空间发生变化，导致指针指向错误，进而引发异常

```cpp
int main(){
            
            
    int arr[] = {
            
            11, 22, 33, 44, 55, 66, 77, 88, 99};
    vector<int> test(arr, arr+sizeof(arr)/sizeof(int));
    vector<int>::iterator pos = find(test.begin(), test.end(), 55);
    test.insert(pos, 999);

    for(int i = 0; i < test.size(); i++){
            
            
        cout << test[i] << " ";
    }
    cout << endl;
    
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F29e38eb4cddd4145bda730b8552f3199.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

**③`erase(iterator begin, iterator end)`：清除元素**

```cpp
int main(){
            
            
    int arr[] = {
            
            11, 22, 33, 44, 55, 66, 77, 88, 77, 99};
    vector<int> test(arr, arr+sizeof(arr)/sizeof(int));
    vector<int>::iterator pos = find(test.begin(), test.end(), 55);
    //清除55
    test.erase(pos);
    //此时迭代器指向66
    cout << (*pos) << endl;

    for(int i = 0; i < test.size(); i++){
            
            
        cout << test[i] << " ";
    }
    cout << endl;

    //清除66及其后面的元素
    test.erase(pos, test.end());

    for(int i = 0; i < test.size(); i++){
            
            
        cout << test[i] << " ";
    }
    cout << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5b5c7e6be82247699805fb41c400539a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

# 三：Java中的ArrayList（对比学习）

## （1）介绍

**ArrayList：Java集合框架中，`ArrayList`就是顺序表（可动态扩容），它是一个普通的类，实现了`List`接口**

- 实现了`RandomAccess`接口表明`ArrayList`可以**随机访问**
- 实现了`Cloneable`接口表明`ArrayList`可以`clone`
- 实现了`Serializable`接口表明`ArrayList`是支持**序列化的**
- `ArrayList`**线程不安全**，适用于单线程。在多线程下选择`Vector`

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3e696a171e9943ea8960988c6ee9d662.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

## （2）使用

### A：构造方法

**ArrayList 构造方法：如下**

- `ArrayList()`：无参构造
- `ArrayList(Collection <? extends E> c)`：利用其它`Collection`构建`ArrayList`
- `ArrayList(int intialCapacity)`：指定顺序表初始容量

**在实际使用中，最推荐的写法是用父类引用子类对象（多态），例如下面这样。此时该list对象可以调用`List`和`ArrayList`的共有方法，不可调用`ArrayList`的特有方法。但在一般情况下，`List`中提供的方法已经完全够用，如果确实有额外需求，可以再修改为`ArrayList`即可**

```java
List<String> list = new ArrayList<String>
```

**其他构造方式如下**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        ArrayList <String> arrayList_String = new ArrayList<>(); //无参构造
        ArrayList <Double> arrayList_Double = new ArrayList<>(10);//指定容量为10
        List<Integer> list = new ArrayList<>(); //利用List接口引用
        ArrayList <Double> arrayList_Double2 = new ArrayList<>(arrayList_Double);//也可以这样
    }
}
```

### B：常见操作

**ArrayList常见操作：ArrayList提供了很多方法，其中常用方法如下**

- `boolean add(E e)` ：尾插 e
- `void add(int index, E element)` ：将 e 插入到 index 位置
- `boolean addAll(Collection<? extends E> c)` ：尾插 c 中的元素
- `E remove(int index)` ：删除 index 位置元素
- `boolean remove(Object o)` ：删除遇到的第一个 o
- `E get(int index)` ：获取下标 index 位置元素
- `E set(int index, E element)`： 将下标 index 位置元素设置为 element
- `void clear()` ：清空
- `boolean contains(Object o)` 判断 o 是否在线性表中
- `int indexOf(Object o)` ：返回第一个 o 所在下标
- `int lastIndexOf(Object o)` ：返回最后一个 o 的下标
- `List<E> subList(int fromIndex, int toIndex)` ：截取部分 list

下面是一个测试例子，涵盖上面的方法

```java
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.List;

public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        List<String> list = new ArrayList<>();
        list.add("JavaSE");
        list.add("JavaWeb");
        list.add("JavaEE");
        list.add("JVM");
        list.add("软件测试");
        System.out.println(list);
        System.out.println("获取list中有效元素个数：" + list.size());

        System.out.println("获取索引为2上的元素：" + list.get(2));

        list.set(2, "Javaee");
        System.out.println("设置索引为2上的元素为Javaee：" + list);

        list.add(2, "java数据结构");
        System.out.println("在list的索引为2处插入Java数据结构：" + list);

        list.remove("JVM");
        System.out.println("删除list中的JVM：" + list);

        list.remove(1);
        System.out.println("删除list中索引为1处的元素：" + list);

        System.out.println("坚持list中是否存在数据库这个元素？" + list.contains("数据库"));

        System.out.println("从后往前查找Javaee第一次出现的位置：" + list.indexOf("Javaee"));

        List<String> ret = list.subList(1, 3);
        System.out.println("使list中[1,3)之间的元素构成一个新的List返回：" + ret);

        list.clear();
        System.out.println("清空list并打印其大小：" + list.size());
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff0ac459d86674e508047b2e3a9541b78.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675583)

### C：ArrayList遍历

**①：使用`for`循环**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        List<Integer> list = new ArrayList<>();
        list.add(23);
        list.add(68);
        list.add(91);
        list.add(53);
        list.add(44);

        for(int i = 0; i < list.size(); i++){
            
            
            System.out.print(list.get(i) + " ");
        }
        System.out.println();
    }
}
```

**②：使用`foreach`遍历循环**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        List<Integer> list = new ArrayList<>();
        list.add(23);
        list.add(68);
        list.add(91);
        list.add(53);
        list.add(44);

        for(Integer integer : list){
            
            
            System.out.print(integer + " ");
        }
        System.out.println();
    }
}
```

**③：使用迭代器**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        List<Integer> list = new ArrayList<>();
        list.add(23);
        list.add(68);
        list.add(91);
        list.add(53);
        list.add(44);

        Iterator<Integer> iter = list.iterator();
        while(iter.hasNext()){
            
            
            System.out.print(iter.next() + " ");
        }
        System.out.println();
    }
}
```

### D：ArrayList扩容

**ArrayList扩容：扩容规则如下**

①：检测是否真正需要扩容，如果是调用`grow`准备扩容

②：预估需要扩容的大小

- 初步预估按照1.5倍大小扩容
- 如果用户所需大小超过预估1.5倍，则按照用所需大小扩容
- 真正扩容之前检测是否能扩容成功，防止太大导致扩容失败

③：使用`copyOf`进行扩容