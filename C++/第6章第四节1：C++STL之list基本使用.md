 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：list介绍](#list_5)
- [二：list常用接口](#list_17)
- - [（1）构造函数](#1_20)
  - [（2）增删查改](#2_59)
- [三：Java中的LinkedList（对比学习）](#JavaLinkedList_124)
- - [（1）介绍](#1_125)
  - [（2）使用](#2_131)
  - - [A：构造方法](#A_133)
    - [B：常见操作](#B_164)
    - [C：LinkedList遍历](#CLinkedList_235)

# 一：list介绍

**list：是可以在任意位置进行插入和删除的序列式容器，并且可以前后双向迭代，底层由双向循环链表实现**

**[官方文档](https://cplusplus.com/reference/list/list/)中给出了`list`的各种操作，和`string`、`vector`基本一致**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040715181597.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

**和`vector`一样，`list`也是类模板，它能承载的数据类型如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210407151908713.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

# 二：list常用接口

- **注意：** `list`和`string`、`vector`的接口、使用方法非常相似，所以这里不再详细介绍

## （1）构造函数

**构造函数：有如下4种构造方式**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210407152025482.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

---

```cpp
void print(const list<int> &l){
            
            
    for (auto e : l){
            
            
        cout << e << " ";
    }
    cout << endl;
}

int main() {
            
            
    vector<int> vec = {
            
            1, 2, 3, 4, 5};

    //无参构造
    list<int> test;
    //n个val
    list<int> test1(5 ,2);
    //迭代器构造
    list<int> test2(vec.begin(), vec.end());
    //拷贝构造
    list<int> test3(test2);

    print(test);
    print(test1);
    print(test2);
    print(test3);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8c84dc57a9d7480298a2737d20c23c04.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

## （2）增删查改

**构造函数：由于是链表，所以其增删查改相较于`vector`来说有些不同**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210407155850102.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

```cpp
void print(const list<int> &l){
            
            
    for (auto e : l){
            
            
        cout << e << " ";
    }
    cout << endl;
}

int main() {
            
            
    list<int> test;
    //尾插
    cout << "尾插4个元素" << endl;
    test.push_back(1);
    test.push_back(2);
    test.push_back(3);
    test.push_back(4);
    print(test);
    //头插
    cout << "头插4个元素" << endl;
    test.push_front(5);
    test.push_front(6);
    test.push_front(7);
    test.push_front(8);
    print(test);
    //尾删
    cout << "尾删2个元素" << endl;
    test.pop_back();
    test.pop_back();
    print(test);
    //头删
    cout << "头删2个元素" << endl;
    test.pop_front();
    test.pop_front();
    print(test);
    //查找
    cout << "查找元素5并在其后面插入3个9" << endl;
    auto pos = find(test.begin(), test.end(), 5);
    test.insert(pos, 3, 9);
    print(test);
    //删除
    cout << "删除元素5" << endl;
    auto pos1 = find(test.begin(), test.end(), 5);
    test.erase(pos1);
    print(test);
    cout << "删除9及其后面的元素" << endl;
    auto pos2 = find(test.begin(), test.end(), 9);
    test.erase(pos2, test.end());
    print(test);
    //清除
    cout << "清除元素" << endl;
    test.clear();
    print(test);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff67762e550dc4442a521492569870d7c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

# 三：Java中的LinkedList（对比学习）

## （1）介绍

**LinkedList：Java集合框架中，`LinkedList`就是双链表，它也实现了`List`接口**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc0dcc6922004473ca34d9f64d25f1fb8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

## （2）使用

### A：构造方法

**LinkedList构造方法：如下**

 -    `LinkedList()`：无参构造
 -    `public LinkedLIst(Collection<? extends E> c)`：使用其他集合容器中元素构造

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        //构造空的LinkedList
        List<Integer> list1 = new LinkedList<>();
        List<String> list2 = new LinkedList<>();

        //也可以使用ArrayList来构造LinkedList，此时顺序表将会被转换为对应的链式结构
        List<Integer> list3 = new ArrayList<>();
        list3.add(199);
        list3.add(299);
        list3.add(399);
        System.out.println(list3);
        List<Integer> list4 = new LinkedList<>(list3);
        System.out.println(list4);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd494b643d1364e899e96d874afa6461f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

### B：常见操作

**LinkedList常见操作：LinkedList提供了很多方法，其中常用方法如下**

- `boolean add(E, e)`：尾插`e`
- `void add(int index, E element)`：将`e`插入到`index`位置
- `boolean addAll(Collection<? extends E> c)`：尾插`c`中的元素
- `E remove(int index)`：删除`index`位置元素
- `boolean remove(Object o)`：删除遇到的第一个 `o`
- `E get(int index)`：获取下标`index`位置元素
- `E set(int index, E element)`：将下标`index`位置元素设置为`element`
- `void clear()`：清空
- `boolean contains(Object o )`：判断`o`是否在其中
- `int indexOf(Object o)`：返回第一个`o`所在下标
- `int lastIndexOf(Object o)`：返回最后一个`o`的下标
- `List<E> subList(int fromIndex, int toIndex)`：截取部分`List`

下面是一个测试例子，涵盖上面的方法

```java
package testLinkedList;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        LinkedList<Integer> list = new LinkedList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);
        list.add(6);
        list.add(7);
        System.out.println("list初始情况：" + list);

        list.add(0, 0);
        System.out.println("在起始位置插入元素0：" + list);

        list.removeFirst();//和list.remove()等价
        list.removeLast();
        System.out.println("删除第一个元素和最后一个元素：" + list);

        list.remove(3);
        System.out.println("删除index为3处的元素：" + list);

        System.out.println("检测元素5是否存在：" + list.contains(5));

        System.out.println("从前往后找到元素2的位置" + list.indexOf(2));

        System.out.println("获取index为1处的元素：" + list.get(1));

        list.set(1, 99);
        System.out.println("将index为1处的元素设置为99：" + list);

        List<Integer> sub = list.subList(1, 4);
        System.out.println("截取区间[1, 4)中元素并返回为一个新的LinkedList：" + sub);

        list.clear();
        System.out.println("清空list：" + list);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F399c4b504a664ade996bf455c68044b8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675653)

### C：LinkedList遍历

**①：使用`for`循环遍历**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        LinkedList<Integer> list = new LinkedList<>();
        list.add(11);
        list.add(22);
        list.add(33);
        list.add(44);
        list.add(55);
        list.add(66);
        list.add(77);

        for(int i = 0; i < list.size(); i++){
            
            
            System.out.print(list.get(i) + "-");
        }
        System.out.println();
    }
}
```

**②：使用`foreach`循环遍历**

```java
for(int e:list){
            
            
    System.out.println(e + " ");
}
System.out.println();
```

**③：使用迭代器**

```java
//正向迭代器
ListIterator<Integer> it = list.listIterator();
while(it.hasNext()){
            
            
    System.out.print(it.next() + " ");
}
System.out.println();

//反向迭代器
ListIterator<Integer> rit = list.listIterator(list.size());
while(rit.hasPrevious()){
            
            
    System.out.print(rit.previous() + " ");
}
System.out.println();
    }
```