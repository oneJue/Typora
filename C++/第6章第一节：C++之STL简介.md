 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：什么是STL](#STL_4)
- [二：STL的版本](#STL_7)
- [三：STL的六大组件](#STL_17)
- [四：如何学习STL](#STL_61)

# 一：什么是STL

**标准模板库STL\(Standard Template Library\)：是C++标准库的重要组成部分，不仅是一个可复用的组件库，而且是一个包罗数据结构与算法的软件框架**

# 二：STL的版本

**①祖师爷版本**： Alexander Stepanov、Meng Lee 在惠普实验室完成的原始版本，本着开源精神，他们声明允许任何人任意运用、拷贝、修改、传播、商业使用这些代码，无需付费。唯一的条件就是也需要向原始版本一样做开源使用

**Windos-P.J版本**： 由P. J. Plauger开发，继承自HP版本，**被Windows Visual C++采用**，不能公开或修改，缺陷：可读性比较低，符号命名比较怪异

**SGI版本**：由Silicon Graphics Computer Systems，Inc公司开发，继承自HP版本。**被GCC\(Linux\)采**用，可移植性好，可公开、修改甚至贩卖，从命名风格和编程 风格上看，阅读性非常高

# 三：STL的六大组件

**STL的六大组件：STL由如下六个部分构成**

- **仿函数**

  - greater
  - less
  - …

- **空间配置器**

  - allocator

- **算法**

  - find
  - swap
  - reverse
  - sort
  - merge
  - …

- **容器**

  - string：字符串
  - vector：动态数组
  - list：链表
  - deque：双端队列
  - map：哈希表
  - set：集合
  - multimap
  - myltiset

- **迭代器**

  - iterator
  - const\_iterator
  - reverse\_iterator
  - const\_reverse\_iterator

- **配接器**

  - stack：栈
  - queue：队列
  - priority\_queue：优先级队列

# 四：如何学习STL

**把STL称之为“艺术品”丝毫不为过，因为它是C++泛型编程的优秀代表，这里面蕴含着极为高超的编程技巧。所以学习时注意以下几点**

- STL的内容非常多，函数、接口各式各样，学习的时候不要贪多，因为有些东西可能这辈子你都用不到，一定要学精
- 学习完之后要多加使用，否则你会学完就忘，最好的方法就是做算法题，因为算法题中会常常使用到他们
- 不要浅尝辄止，不要因为只学习了几个接口就代表你会STL了，所以对于其内部实现也要去研究，否则你可能连`size`和`capacity`的区别都不知道。当然也不要着火入门，这个东西研究到一定地步就可以了
- 学习的时候尝试和Java中的集合框架做以对比，两者各有优缺点，会给你的学习带来意想不到的收获的