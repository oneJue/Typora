 

- [部分参考](https://www.runoob.com/numpy/numpy-ndarray-object.html)  

  ### 文章目录

  - [一：Numpy了解](#Numpy_5)
  - [二：Numpy之Ndarray对象](#NumpyNdarray_33)
  - [三：Numpy之数据类型](#Numpy_115)
  - - [（1）数据类型](#1_130)
    - [（2）类型转换](#2_145)
  - [四：Numpy之数据类型对象\(dtype\)](#Numpydtype_174)

# 一：Numpy了解

**Numpy：Numpy是Python的一种开源的数值计算扩展。这种工具可用来存储和处理大型矩阵，它提供了许多高效的数值编程工具。Numpy常常和 Matplotlib（绘图库）一起使用，用于替代MatLab，是一个强大的科学计算环境，有助于我们通过 Python 学习数据科学或者机器学习**

安装好Numpy后，常用导入方法有三种

```python
import numpy
import numpy as np
from numpu import *

#  如果只想要导入特定功能
from numpy import reshape  # 转置功能
```

输入下面语句，如果正确显示了对角矩阵，那么表明你正确安装和导入了Numpy

```python
print(np.eye(4))
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F09c7bf7a81834396bd7922057a65e789.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

# 二：Numpy之Ndarray对象

**Ndarray对象：Numpy中的所有操作都围绕一个`Ndarrary`对象展开，其本质是一个N维数组。**

**①**：NumPy 中的N 维数组对象 `ndarray`**是一系列同类型数据元素的集合，每个元素都有索引，从0开始**。换句话说，`ndarray`对象是一个存放了同类型元素的**多维数组**，`ndarray`中每个元素在内存中都有**相同存储大小的区域**，由以下内容组成

- 一个指向数据（内存或内存映射文件中的一块数据）的**指针**（在Python中就是一个变量）

- 数据类型或 `dtype`：描述在**数组中的每个元素所占大小**

- 一个表示数组形状（`shape`）的元组：表示**各维度大小**

- 一个跨度元组（`stride`）：其中的整数指的是**为了前进到当前维度下一个元素需要"跨过"的字节数**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7c4b29f7737542ceb148b7bda1310e39.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

这里，你可以调用`array()`函数创建一个简单的`ndarray`对象

 -    为了便于演示，采用直接传入列表的方式创建

```python
a = np.array([1, 2, 3, 4])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F070f89b1867a4f7881e7fa434573d1ff.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

**②**：`Nadarry`对象较难理解的就是它的**维度**，当然这个问题日后会细说。这里有一个很简单的方法去判断数组的维度：**第一个元素前面有几个左中括号`[`它就是几维的**，例如

```python
a = np.array([1, 2, 3])  # 一维
b = np.array([[1, 2, 3], [1, 2, 3]])  # 二维
c = np.array([[[1, 2, 3], [1, 2, 3]], [[1, 2, 3], [1, 2, 3]]])  # 三维
print(a)
print("-"*20)
print(b)
print("-"*20)
print(c)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fea3dca4b823a4cee808e7fe9e62e57f7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

**③**：`naddary`和Python中`List`**并不是一个东西**，其中List是可以追加**任何数据类型的**，而`naddary`并不可以

```python
a = np.linspace(0, 1, 11)  # 等差数列
l = [0, 0.1, 0.2,  0.3, 0.4, 0.5, 0.6, 0.7, 0.8,  0.9, 1]
print(type(a))
print(type(l))
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F23f36725edb54d6f80256daa57f7f360.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

而且`naddary`对象在相加或进行其它运算时会**直接作用于单个元素**

```python
a = np.array([[1, 1, 1], [1, 1, 1]])  # 二维
b = np.array([[1, 1, 1], [1, 1, 1]])  # 二维
c = a + b
print(c)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9fd78125df7440b49fb9dc1a44c0419b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

# 三：Numpy之数据类型

Numpy底层使用**C/C++实现**，只是**接口做得和Python一样**。所以在使用时会存在一些细节问题，例如Python中的某些数值如果设置的不合理放在Numpy中就会溢出。所以这里有必要了解一下Numpy的数据类型

```python
import numpy as np

a = np.arange(0, 10)
print(a)
a[2] = 10000000000
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcb49efd35f854ca5ab9164c837b48375.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

## （1）数据类型

**数据类型：下表为Numpy中的基本数据类型**

- Numpy 的数值类型实际上是 `dtype` 对象的实例，并对应唯一的字符，包括 `np.bool_`，`np.int32`，`np.float32`，等等

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc65ced0e5edf418c8d472cdde9b30bf9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

## （2）类型转换

**①**：可以通过`asarray()`函数进行类型转换

```python
a = np.array(random.randint(0, 10, size=[3, 3]), dtype=float)
print(a)
print("----------------------------")
a = np.asarray(a, dtype=int)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F788926e170c34022b22b737d3f119705.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)

**②**：也可以**通过`astype()`函数**进行类型转换（推荐）

```python
a = np.array(random.randint(0, 10, size=[3, 3]), dtype=float)
print(a)
print("----------------------------")
a = a.astype(int)
print(a)
```

# 四：Numpy之数据类型对象\(dtype\)

数据类型对象`dtype`是`Numpy.dtype`类的实例（和`tuple`、`dict`一样，它也是个类）

```python
a = np.dtype
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffddc3697e6334b37a4c4d696c3dbd9c9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124226590)  
到目前为止，对于Numpy中的数组，我们仅仅使用了如`int`、`float`这样基本的数据类型，且数组中所有元素数据类型一样。**但`dtype`对象可以包含基本数据类型的组合，在`dtype`的帮助下，我们能够创建“结构化数组”（或称为“记录数组”），结构化数组可以为每列提供不同的数据类型，有点像数据库中的记录**

---

**`dtype`描述了数据的以下方面**

- 数据的类型（整数、浮点数或者Python对象）
- 数据的大小（例如，整数需要使用多少个字节来存储）
- 数据的字节顺序（小端存储`<`或者大端存储`>`）
- 在结构化类型的情况下，字段的名称、每个字段的数据类型和每个字段所取的内存块部分
- 如果数据类型是子数组，那么它的形状和数据类型又是什么

**其构造格式如下**

```python
numpy.dtype(object, align, copy)
```

- `object`：要转换为的数据类型对象
- `align`：若设置为`true`，填充字段使其类似C的结构体
- `copy`：赋值`dtype`对象，若为`false`，则是对内置数据类型对象的引用