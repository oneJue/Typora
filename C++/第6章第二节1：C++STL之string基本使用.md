 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：string概述](#string_3)
- - [（1）概述](#1_5)
  - [（2）关于size和capacity](#2sizecapacity_37)
- [二：string使用](#string_43)
- - [（1）构造函数](#1_51)
  - [（2）容量操作](#2_118)
  - [（3）访问操作](#3_244)
  - [（4）迭代器](#4_277)
  - [（5）修改操作](#5_308)
  - [（6）非成员函数](#6_561)
- [三：Java中的String（对比学习）](#JavaString_604)
- - [（1）字符串构造](#1_606)
  - - [A：常用构造方法](#A_608)
    - [B：注意](#B_637)
  - [（2）字符串比较](#2_659)
  - [（3）字符串查找](#3_767)
  - [（4）字符串转化](#4_884)
  - - [A：数值和字符串的转换](#A_886)
    - [B：大小写转换](#B_924)
    - [C：字符串转数组](#C_948)
    - [D：格式化](#D_974)
  - [（5）字符串替换](#5_992)
  - [（6）字符串拆分](#6_1027)
  - [（7）字符串截取](#7_1101)

# 一：string概述

## （1）概述

**string：`string`是C++ STL中专门用于处理字符串的模板类。其源码实现框架大致如下**

```cpp
template <class T>

class string
{
            
            
public:
	//构造
	//析构
	//其他成员函数

private:
	//字符串
	//字符指针
	//其他成员变量
}
```

**在[官方文档](http://www.cplusplus.com/reference/string/string/)中给出了`string`的各种操作**

- **注意**：`string`的输入和输出已经被重载

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210319134510871.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （2）关于size和capacity

**其实不止时`string`，其它容器（例如vector）也有`size`和`capacity`。它们的关系可用下面一张图表示，其本质是一个动态的顺序表，`capacity`代表占用的空间，`size`描述了元素实际占用的大小**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb5c27e224527488382feec6fbfcdaa79.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

# 二：string使用

**注意：使用`string`需要包含如下文件**

```cpp
#include <string>
```

## （1）构造函数

**构造函数：C++98中`string`有如下7种构造函数，这里我们只给出常用的构造函数示例**

---

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feb9a0a1294944fd7a56661c66e27a651.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

---

**①`string()：`构造一个空字符串**

```cpp
int main() {
            
            
    string str;
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F50ca05e39fa3498d80f7202590062c61.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②`string(const char *s)`：使用C字符串构造**

```cpp
int main() {
            
            
    string str("hello world!");
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa1d2736429f74a5dae37096ce190371b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③`string(const string &str)`：拷贝构造**

```cpp
int main() {
            
            
    string str("hello world!");
    string str_copy(str);
    cout << str_copy << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F67376a518d964ab5a3a37b3b8bcbba8e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④`string(const string& str,size_t pos,size_t len=npos)`：使用字符串构造，即将字符串`str`区间为`[pos, pos+len-1]`内的串作为一个新串**

```cpp
int main() {
            
            
    string str("hello world!");
    string sub_str(str, 0, 5);
    string sub_str1(str, 3, 100);//最多会到末尾
    cout << sub_str << endl;
    cout << sub_str1 << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F87cc7e246a904c399f94dc59d6d06ce7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （2）容量操作

**容量操作：`string`涉及容量操作如下**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210319141319763.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

---

**①`size和length`：返回字符串的长度**

 -    **注意**：之所以会有两个求字符串长度的函数，是一个历史问题。因为`string`类是早于STL的，而STL中容器中求个数的问题统一用的是`size`接口。所以这里更推荐使用`size`

```cpp
int main() {
            
            
    string str("hello world!");
    cout << str.size() << endl;
    cout << str.length() << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0e2c20d0714441e494101092802f39fc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②`capacity`：返回目前占用空间大小**

 -    **注意**：关于`size`和`capacity`我们会在下一节介绍

```cpp
int main() {
            
            
    string str("hello world!");
    cout << str.capacity() << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6f555a3c4a6c46ee9d4920ecf9b0f45f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③`reserve`：设置空间**

 -    **注意**：如果你设置的空间大小要小于当前空间大小，将不会生效

```cpp
int main() {
            
            
    string str("hello world!");
    cout << str.capacity() << endl;
    str.reserve(100);
    cout << str.capacity() << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fad9acdad825a4774930f6bdbf25e75c2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④`resize`：相较于`reserve`，`resize`不仅分配空间，而且可以初始化**

```cpp
int main() {
            
            
    string str;
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;
    str.reserve(30);
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;
    str.resize(20, 'a');
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;

    cout << "str: " << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fac88974bdd7c4598a578929523e526af.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑤`clear`：清空字符串**

```cpp
int main() {
            
            
    string str("hello world");
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;
    str.clear();
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffeb071311e89471fb54cd6726de84f31.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑥`empty`：判断是否为空串（即`size`是否为0），如果是空串返回`true`**

```cpp
int main() {
            
            
    string str("hello world");
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;
    if(str.empty()){
            
            
        cout << "是空串" << endl;
    }else{
            
            
        cout << "不是空串" << endl;
    }
    str.clear();
    cout << "capacity: " << str.capacity() << endl;
    cout << "size: " << str.size() << endl;
    if(str.empty()){
            
            
        cout << "是空串" << endl;
    }else{
            
            
        cout << "不是空串" << endl;
    }

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff244a2e1f69f45b3a1f55f0eee2ebea2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （3）访问操作

**访问操作：`string`涉及访问操作如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031915103940.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

---

**①`operator[]`：将`[]`进行重载，使用户可以像字符数组那样去访问字符串中的每一个字符**

 -    **注意**：其返回类型为引用类型，这表示它既可以读也可以修改（前提条件是`string`非`const`）

```cpp
int main() {
            
            
    string str("hello world");

    for(int i = 0; i < str.size(); i++){
            
            
        cout << str[i] << str[i] - '0' << endl;
        str[i] += 1;
    }
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff6741b75ea1b4ff9847bd01802f80d61.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②`back和front`：返回头尾字符**

## （4）迭代器

**迭代器：要访问顺序容器（顺序表）和关联容器（链表）的元素，可以通过迭代器（`iterator`）进行。迭代器是一个变量，相当于容器和操纵容器的算法之间的中介，迭代器可以指向容器中的某个元素，通过迭代器就可以读写它指向的元素，和指针很相似。STL设计迭代器如下**

- **注意**：`r`表示反向的意思

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210319160534382.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)  
**这里我们以字符串为例，演示一下迭代器是如何使用的，其余容器类似**

```cpp
int main(){
            
            
    string str("hello world!");
    //间接写法：auto str_iterator = str.begin
    string::iterator str_iterator = str.begin();
    while(str_iterator != str.end()){
            
            
        //和指针用法类似，所以也可以解引用
        (*str_iterator) = 'g';
        cout << (*str_iterator);
        str_iterator++;
    }
    cout << endl;
    
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7425bb15c17f48ffa374129140f79cfb.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （5）修改操作

**修改操作：`string`涉及修改操作如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210320111516668.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032012183929.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

---

**①`push_back`：追加一个字符**

```cpp
int main(){
            
            
    string str("hello world!");
    str.push_back('w');
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6845e42487324927b4874a713ab2dd54.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②`append`：追加一个字符串**

 -    **注意**：它有很多重载形式，只不过我们经常用它追加字符串

```cpp
int main(){
            
            
    string str("hello world!");
    str.append("hello C++");
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F917ac3d95b034f25bdfd22831a88691c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③`operator+=`：追加字符串或字符，这个最为常用**

```cpp
int main(){
            
            
    string str("hello world!");
    cout << str << endl;
    str += 'a';
    cout << str << endl;
    str += "this is it";
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff9d04cdb6f76408a83333d16e502822a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④`insert`：插入字符或字符串**

- **在某个位置，插入某个对象** :`string& insert(size_t pos,const string& str)`

- **在某个位置，插入字符串** :`string& insert(size_t pos,const char* s)`

  - **从某个位置开始，插入某个字符串的n个字符**： `string& insert(size_t pos,const char*s,size_t n)`

```cpp
int main(){
            
            
    string str("hello world!");
    string tmp(" nice shot ");
    cout << str << endl;
    str.insert(5, tmp);
    cout << str << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa8641bdd866a4a53afafc96245989c5a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑤`c_str`：返回字符数组指针**

```cpp
int main(){
            
            
    string str("hello world!");
    const char *c = nullptr;
    //获取到字符数组指针
    c = str.c_str();
    str.clear();
    str += "Nice Shot!";

    //使用指针输出
    cout << c << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fba819106700a48c7a957f4f6b28b86b2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑥`find`：在字符串中查找内容（可以是对象、字符串、字符等等），注意**

- `size_ t find(const string& str,size_t pos=0)const`

- **查找成功时**：返回的就是一个位置索引

  - **查找失败时**：会返回`npos`，你可以认为`npos=-1`

```cpp
int main(){
            
            
    string str1("hello world!");
    string str2("hello");

    //如果不写开始位置，那么默认就从头开始
    if(str1.find(str2) != string::npos){
            
            
        cout << str2 << "在" << str1 << endl;
    }else{
            
            
        cout << str2 << "不在" << str1 << endl;
    }

    //如果写了开始位置，那么就会从开始位置处查找
    if(str1.find(str2, 5) != string::npos){
            
            
        cout << str2 << "在" << str1 << endl;
    }else{
            
            
        cout << str2 << "不在" << str1 << endl;
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb6a97e6551ae442690cd246ae08e3aee.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑦`rfind`：从后向前查找**

 -    **`find`缺陷**：`find`在查找类似于后缀名时会出现一些错误，比如在Linux中，虽然不以文件的后缀名区分文件的，但是为了方便，程序会在处理文件的时候自动将后缀名进行叠加。于是可能会出现形如`test.txt.tar.zip`这样的形式，而此时如果继续使用find查找，会发现位置不是我们要的

```cpp
int main(){
            
            
    string filname("test.txt.tar.zip");
    int pos = filname.find(".");
    cout << "索引：" << pos << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8fb0e87dcf6945d5aa56d6e943808d2f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

而如果使用`rfind`则可以使其从后向前查找

```cpp
int main(){
            
            
    string filname("test.txt.tar.zip");
    int pos = filname.rfind(".");
    cout << "索引：" << pos << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdd56b1b6e8d64428b515918ce2fd3994.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑧`substr`：用于从一个字符串中截取出子串**

 -    第一个参数是**起始位置**
 -    第二个参数是你要**截取多少个**

```cpp
int main(){
            
            
    string filname("test.txt.tar.zip");
    int pos = filname.rfind(".");
    //如果不写结束位置，那么默认会截取到末尾
    string suffix = filname.substr(pos);
    cout << suffix << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F857991f1d2d242ecbf4e5e89d7231f79.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

这里我们可以做一个练习，把某个网址拆分为三部分

```cpp
int main(){
            
            
    string url("https://www.cplusplus.com/reference/string/string/");
    int pos1 = url.find(":"); //pos1定位至冒号的位置
    int pos2 = pos1 + 3; //pos2定位到W
    int pos3 = url.find(".com/");
    pos3 += 4;


    string first = url.substr(0, pos1);
    string second = url.substr(pos2, pos3 - pos2);
    string third = url.substr(pos3 + 1);


    cout << first << endl;
    cout << second << endl;
    cout << third << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fecfcd9a27fbc4547852afd36388aee9a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑨`erase`：清除字符**

```cpp
int main() {
            
            
    string str("hello world");
    cout << str << endl;
    str.erase();
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd70ab66d4fdf41ef9842d83039b0f44f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑩`find_first_of`：寻找第一个满足给定字符的位置**

- **注意**：如果参数是一个字符串，那么这些字符之间是或的关系，表示只要寻找到一个字符就返回位置

下面有一个例子，要求把一个字符串中的所有元音字母全部替换为`“*”`。所以首先使用`find_first_of`得到第一个元音字母的位置，然后使用循环再从下一个字母开始找起，直到`find_first_of`返回值为`nops`时表示找到了末尾

```cpp
int main(){
            
            
    string str("Hello World");
    cout << str << endl;
    size_t found = str.find_first_of("aeiou");
    while(found != string::npos){
            
            
        str[found] = '*';
        found = str.find_first_of("aeiou", found + 1);
    }
    cout << str << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff69de30918644b80959a7caffefb0103.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （6）非成员函数

**非成员函数：如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210320143218553.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**①`relational operators`：用于比较两个`string`的大小关系，有很多重载形式**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210320143335829.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

```cpp
int main(){
            
            
    string str1("Hello World");
    string str2("Hello World");
    string str3("Hello C++");
    
    cout << (str1 == str2) << endl;
    cout << (str1 == "c++") << endl;
    cout << (str1 > "c++") << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe3f0361bf10e41f1a9c95e46bb6c3f36.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②`operator+`：**

```cpp
int main(){
            
            
    string str1("Hello World");
    string str2("Hello C++");
    string str3;
    str3 = str1 + ' ' +str2;

    cout << str3 << endl;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Faf4878e25f12490c81b0192a1acb6218.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

# 三：Java中的String（对比学习）

## （1）字符串构造

### A：常用构造方法

**字符串构造：构造一个字符串方法有很多，常用的有以下三种**

 -    使用常量字符串构造
 -    直接`newString`对象
 -    使用字符数组构造

```java
public class TestDemo {
            
            
    public static void main(String[] args){
            
            
        //使用常量字符串构造
        String str1 = "Hello World1";
        //直接newString对象
        String str2 = new String("Hello World2");
        //使用字符数组进行构造
        char[] arr = {
            
            'H','e','l','l','o','W','o','r','l','d','3'};
        String str3 = new String(arr);

        System.out.println(str1);
        System.out.println(str2);
        System.out.println(str3);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F22ffa972b25f45acadb29d71ca8f504d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

### B：注意

**String是引用类型，但是其内部并不存储字符串本身，实际回报存在一个char类型的数组中**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe4b61f3db0b948e4af75e62bde1e45fd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**完整的关系应该是下面这样**

```java
public static void main(String[] args){
            
            
	//s1和s2引用的不是同一个对象
	//s1和s3引用的是同一个对象
	String s1 = new String("hello");
	String s2 = new String("world");
	String s3 = s1;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbd7c64edb4f84884b5c256e30116fc8e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （2）字符串比较

**①：`==`用于比较是否引用的是同一个对象**

 -    **内置类型**：比较的是变量的值
 -    **引用类型**：比较的是地址

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        int a = 10;
        int b = 20;
        int c = 10;

        //对于基本类型，比较的是值
        System.out.println(a == b);
        System.out.println(a == c);

        System.out.println("----------------------------------------------");

        //对于引用类型，比较是否引用的是同一个对象
        String s1 = new String("hello");
        String s2 = new String("hello");
        String s3 = new String("world");
        String s4 = s1;
        System.out.println(s1 == s2);
        System.out.println(s2 == s3);
        System.out.println(s1 == s4);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F80ee61123a7c43c9b157b6e5d5353507.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②：`boolean equals(Object anObject)`方法会按照字典序进行比较。如下，String类中重写了父类Object中的equals方法**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F543d82c48a584549934abf1eab55460b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("hello");
        String s2 = new String("hello");
        String s3 = new String("hello");

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb2d120e1a27a460197d21e166826358c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③：`int compareTo(String s)`方法也会按照字典序进行比较，但与`equals`有所不同的是，它返回的是`int`。具体比较规则如下**

 -    **先按照字典序进行比较，如果出现不等的字符，直接返回这两个字符的大小差值**
 -    **如果前k个字符相等（k为两个字符长度最小值），则返回两个字符串长度差值**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("abc");
        String s2 = new String("ac");
        String s3 = new String("abc");
        String s4 = new String("abcdef");

        System.out.println(s1.compareTo(s2));//不同输出字符差值 -1
        System.out.println(s1.compareTo(s3)); //相同为零
        System.out.println(s1.compareTo(s4)); //前k个字符完全相同，输出长度差值 -3
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff0779b4dc57e485e83aef79401c4e62d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④：`int compareTolgnoreCase(String str)`方法：与compareTo方法相同，但是忽略大小写**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("abc");
        String s2 = new String("ac");
        String s3 = new String("ABc");
        String s4 = new String("abcdef");
        System.out.println(s1.compareToIgnoreCase(s2));
        System.out.println(s1.compareToIgnoreCase(s3));//相同
        System.out.println(s1.compareToIgnoreCase(s4));
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3602ee2025e145dba12bab12fe6ba17b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （3）字符串查找

**①：`char charAt(inde index)`：返回`index`位置上字符**

 -    如果`index`为负数或者越界，抛出"`indexOutofBoundsException`"异常

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("Hello");
        String s2 = new String("World");

        char c1 = s1.charAt(2);
        System.out.println(c1);
        System.out.println("-----------------------------------");

        for(int i = 0; i < s2.length(); i++){
            
            
            System.out.println(s2.charAt(i));
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdd9aa73fede14b8e88009b24984d0520.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②：`int indexOf(int ch)`：返回字符`ch`第一次出现的位置**

 -    如果没有`ch`这个字符则会返回-1

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("Hello");
        String s2 = new String("World");

        System.out.println(s1.indexOf('e'));
        System.out.println(s2.indexOf('c'));
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fffb825de291a432895498c0deeadb482.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③：`int indexOf(int ch, int fromIndex)`：从“`fromIndex`”位置开始寻找字符“`ch`”第一次出现的位置**

 -    如果没有`ch`这个字符则会返回-1

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("Hello World!");

        System.out.println(s1.indexOf('e', 1));
        System.out.println(s1.indexOf('o', 5));
        System.out.println(s1.indexOf('e', 5));
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff75b733993f14e048219a82fbcf7cf7e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④：`int indexOf(String str)`：返回字符串str第一次出现的位置**

 -    如果没有`str`这个字符串则会返回-1

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("Hello World!");

        System.out.println(s1.indexOf("World"));
        System.out.println(s1.indexOf("Worls"));

    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbe97a5534c8849ef8744fd65164dfbdc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑤：`int indexOf(String str, int fromIndex)`：从“`fromIndex`”位置开始寻找字符串“`str`”第一次出现的位置**

 -    如果没有`str`这个字符串则会返回-1

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = new String("one two three one four nine");

        System.out.println(s1.indexOf("one", 4));
        System.out.println(s1.indexOf("one", 15));
        
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F31f51be9e5c5493f83658eccdb443c96.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**⑥：`int lastIndexOf(int ch)`、`int lastIndexOf(int ch, int fromIndex)`、`int lastIndexOf(String str)`、`int lastIndexOf(String str, int fromIndex)`：和前面类似，只不过是从后向前找**

## （4）字符串转化

### A：数值和字符串的转换

**数字转换为字符串可以使用`String.valueOf()`**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = String.valueOf(123);
        String s2 = String.valueOf(3.14);
        String s3 = String.valueOf(true);
        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F79c159f848be4e03a6215953668f82c4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**字符串转化为数字常用方法如下**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        int num1 = Integer.parseInt("1234");
        int num2 = Integer.parseInt("64", 8);//8表示八进制，八进制下的64对应十进制下的52
        double num3 = Double.parseDouble("3.14");
        System.out.println(num1);
        System.out.println(num2);
        System.out.println(num3);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F72088e9c934746539a5526da628f4590.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

### B：大小写转换

**字母大小写转换可以使用`toUpperCase()`和`toLowerCase()`。需要注意是，转换完成后，原来字符串的值并不会改变**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String s1 = "hello";
        String s2 = "HELLO";

        //小写转换为大写
        System.out.println(s1 + "->" + s1.toUpperCase());
        //大写转换为小写
        System.out.println(s2 + "->" + s2.toLowerCase());
    }
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbc0b6f1e9cc14c20880e95f0ad0b1941.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

### C：字符串转数组

**可以使用`String.toCharArray()`将一个字符串转为一个字符数组**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String str = "Hello!";
        char[] ch = str.toCharArray();

        for(int i = 0; i < ch.length; i++){
            
            
            System.out.println(ch[i]);
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9ea16314804344ff9355d40d10812d32.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

### D：格式化

**使用`String.format()`可以将一个字符串格式化为目标字符串**

```java
public class TestDemo {
            
            
    public static void main(String[] args) {
            
            
        String str = String.format("%d-%d-%d", 2000, 10, 12);
        System.out.println(str);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F172c9c3e1bf34187b748848833a25202.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （5）字符串替换

**①：`String replaceAll(String regex, String replacement)`：将原字符串中`regex`替换为`replacement`**

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String s1 = "HelloWorld,Hello You";
        String s2 = s1.replaceAll("Hello", "Bye"); //将Hello替换为Bye
        String s3 = s1.replace('H', 'S'); //将H替换为S
        System.out.println(s2);
        System.out.println(s3);
    }
}
```

**②：`String replaceFirst(String regex, String replacement)`：仅替换首次出现的`regex`**

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String s1 = "HelloWorld,Hello You";
        String s2 = s1.replaceFirst("Hello", "Bye"); //将第一个Hello替换为Bye
        System.out.println(s2);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5b1868c85fb9483a87fbbb58ce0d01f9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （6）字符串拆分

**①：`String[] split(String regex)`：以regex为分割标志，将原字符串拆分为多个字符串**

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String str = "中国-北京市-东城区-长安街-天安门广场";
        String[] ret = str.split("-");
        for(String s : ret){
            
            
            System.out.println(s);
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2a974061cc244b64a0551c105cd0c30c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**②：`String[] split(String regex, int limit)`：以`regex`为分割标志，将原字符串拆分为`limit`字符串**

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String str = "中国-北京市-东城区-长安街-天安门广场";
        String[] ret = str.split("-", 3);
        for(String s : ret){
            
            
            System.out.println(s);
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fba30b1fd270346d39ee4eb0d848004c1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**③：一旦以"`|`“、”`*`“、”`+`“等作为分割标志时，前面都得加上”`\\`“，而且如果是”`\`“，那么就得写上”`\\\\`"**

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String str = "www.baidu.com";
        String[] ret = str.split("\\.");
        for(String s : ret){
            
            
            System.out.println(s);
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feaba9e2e11a445f0bcfd7b3f6ff9422a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

**④：如果第一次拆分出来的字符串中还需要进行拆分，那么就要使用下面这种多次拆分的写法**

 -    首先以`&`为分割标志拆分出`name=zhangsan`和`age=18`
 -    然后再以=为分割标志拆分出`name`、`zhangsan`、`age`和`18`

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String str = "name=zhangsan&age=18";
        String[] result = str.split("&");
        for(int i = 0; i < result.length;i++){
            
            
            String[] temp = result[i].split("=");
            System.out.println(temp[1]);
        }
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1d6d15ac1db74dd9bea84257cee7b04f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)

## （7）字符串截取

**使用`String substring(int beginIndex, int endIndex)`可以截取`[beginIndex, endIndex)`区间内的字符串**

 -    如果不写`endIndex`，那么表示从`beginIndex`开始一直截取到末尾

```python
public class TestDemo{
            
            
    public static void main(String[] args) {
            
            
        String str = "-Hello_World";
        String str1 = str.substring(7);
        String str2 = str.substring(1,6);
        System.out.println(str1);
        System.out.println(str2);
    }
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F29442225cb0749679d5aa1460fea3330.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117637873)