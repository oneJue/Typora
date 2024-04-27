 

- [部分参考：菜鸟教程](https://www.runoob.com/numpy/numpy-array-attributes.html)

### 文章目录

- [一：生成数组](#_8)
- - [（1）直接创建数组](#1_10)
  - [（2）numpy.array\(\)函数创建数组](#2numpyarray_131)
  - [（3）从数值范围创建数组](#3_170)
- [二：数组属性](#_266)
- - [（1）N维数组（Ndarray）](#1NNdarray_268)
  - [（2）轴的概念（axis）](#2axis_308)
  - [（3）数组属性](#3_324)

# 一：生成数组

## （1）直接创建数组

**①**：**使用`numpy.empty()`创建一个指定形状（`shape`）、数据类型（`dtype`）且未初始化的数组。相当于先把数组框架搭好，但是内部具体是什么以后再给出**

 -    创建好之后数组内部是**随机值**

```python
numpy.empty(shape, dtype = float, order = 'C')
```

- `shape`：数组形状
- `dtype`：数组类型（可选）
- `order`：‘C’表示行优先；‘F’表示列优先

**例如**

```python
a = np.empty([3, 2], dtype = int)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F55cca884743a4b11940e2834a15dbce3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

**接着，可以使用`fill()`方法将数组设定为指定值**

 -    注意：如果`fill()`中传入的数据类型和原有数组中数据的类型不一致时，**会向原有的类型发生转换**

```python
a = np.empty([3, 3], dtype=int)
print(a)
print("-"*20)
a.fill(10)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F876f264e4131471885b6396bb0e0a28c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

---

**②**：**可以使用`numpy.zeros`和`numpy.ones`生成全0或全1的数组，格式如下**

```python
numpy.zeros(shape, dtype = float, order = 'C')
numpy.ones(shape, dtype = None, order = 'C')
```

- `shape`：数组形状
- `dtype`：数组类型（可选）
- `order`：‘C’表示行优先；‘F’表示列优先

**例如**

```python
a = np.zeros([3, 3], dtype=int)
b = np.ones(10, dtype=int)
print(a)
print('-'*20)
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F291f19438456476789f40a3686d424b9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

---

**③**：**利用`numpy.eye()`创建对角矩阵**

```python
a = np.eye(5, dtype=int)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe2a322b1b3904829b45dbeab447d1d53.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

---

**④**：**使用`random.rand()`等函数生成随机数**

 -    读者可自行查阅相关文档，探索更多的随机数生成方法

```python
a = np.random.rand(5)
b = np.random.randn(5)  # 满足正态分布
c = np.random.randint(1, 10, 10)  # 10个随机整数
d = np.random.uniform(50, 100, 20)  # 50-100之间的随机数
print(a)
print(b)
print(c)
print(d)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd6cdfed263f34a87bfa3b3b59f248753.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

注意多维随机数组写法

```python
a = np.random.randint(0, 10, size=[3, 3])
print(a)

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F912560c2660349eaa0a024ca11a386fe.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

## （2）numpy.array\(\)函数创建数组

**`np.array()`函数，其格式如下**

```python
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)
```

 -    `object` ：数组或嵌套的数列
 -    `dtype` ：数组元素的数据类型，可选
 -    `copy` ：对象是否需要复制，可选
 -    `order` ：创建数组的样式，C为行方向，F为列方向，A为任意方向（默认）
 -    `subok`：默认返回一个与基类类型一致的数组
 -    `ndmin`：指定生成数组的最小维度

```python
a = np.array([[1, 2, 3], [3, 4, 5], [5, 6, 7], [7, 8, 9]], dtype=complex)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4680bb0a04b94f90b6b85955d609dd4b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

**`np.array()`函数传入的对象种类很多，大家一定要灵活使用**

```python
a = np.array([zeros(5), ones(5), random.randint(0, 5, 5)], dtype=int)
b = np.array([zeros([3, 3], dtype=int), ones([3, 3], dtype=int), random.randint(0, 5, size=[3, 3])])
print(a)
print("-"*20)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdae968cad7c44587a42253af9e6644bd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

## （3）从数值范围创建数组

**①**：**使用`arange()`可以生成整数序列，它会根据 `start` 与 `stop` 指定的范围以及 `step` 设定的步长进行操作，基本格式如下**

```python
numpy.arange(start, stop, step, dtype)
```

- `start`：起始值，默认为0
- `stop`：终止值（不包含）
- `step`：步长，默认为1
- `dtype`：数据类型

比如

```python
a = np.arange(0, 10, 2, int)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F080096b590f54c25893830917d58712c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

因此生成多维数组可以这样操作

```python
a = np.array([arange(0, 5, 2), arange(3), arange(3)])
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff8ece86a9e374e8488b82c816a249adc.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

当然还可以这样写

```python
import numpy as np

a = np.arange(9).reshape([3, 3])
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd2e01cb444934a6b8c99aab7b1235b49.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

---

**②**：**使用`linspace()`可以生成等差数列**，基本格式如下

```python
np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
```

- `start`：序列的起始值
- `stop`：序列的终止值，**如果`endpoint`为`true`，该值包含于数列中**
- `num`：要生成的等步长的样本数量，默认为50
- `endpoint`：该值为 `true` 时，数列中包含`stop`值，反之不包含，默认是`True`
- `retstep`：如果为 `True` 时，生成的数组中会显示间距，反之不显示
- `dtype`：数据类型

比如

```python
a = np.linspace(1, 10, 10)
b = np.linspace(10, 20, 5, endpoint=False, dtype=int)  # 不包含终止值
print(a)
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1346afb51845447abd5e79480481fbaf.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

# 二：数组属性

## （1）N维数组（Ndarray）

**N维数组（Ndarray）**：NumPy最为核心的数据类型便是**N维数组`The N-dimensional array (ndarray)`**，它可以**看成homogenous\(同质\) items的集合**，与之密切相关的两种类型是`Data type objects (dtype)`和`Scalars`

- **数据类型对象\(`np.dtype`\)**：用来描述与数组对应的内存区域如何使用。比如数据的类型、大小等
- **NumPy数据类型\(`scalar types`\)**：上一节已有介绍

**如下是常见的1维、2维、3维数组**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F391670ee69dd4cf893bb5c30e908b5b1.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

**①**：比如下面是一个二维数组，也即3×5矩阵

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fece1a5793f684c7db11110091141a732.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

再比如下面是一个三维数组，可以翻译为2个3×5的矩阵

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F19f7a6f5e4bd4872b2db609a013eb3ef.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

**②**：对于初学者来说，维度这个概念特别不容易把握。**具体判断时，有几个中括号就代表是几维度（注意隐含括号）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa34017e1ebcc4e2d84ab9f88840c71eb.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

## （2）轴的概念（axis）

**轴（axis）**：Numpy数组的维数称之为**秩（rank）**，**它表示轴的数量**，在Numpy中**每一个线性的数组称为一个轴**（axis）

- 类似于笛卡尔坐标系，简单的二维坐标系只有 x x x轴和 y y y轴，使用它们可以在平面空间中唯一地确定一个位置

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F48f819f712d64e58b3320745d41ed65d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_17%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)  
**而Numpy中的轴指的就是行、列的方向**

- **`axis=0`：表示沿着第 0 轴进行操作，即对每一列进行操作**
- **`axis=1`：表示沿着第1轴进行操作，即对每一行进行操作**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F681e1f7857c14fd3ae39c1bc8c2a5644.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_17%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)

## （3）数组属性

一个`ndarrary`对象生成后，它就具有以下属性

| 属性 | 说明 |
| --- | --- |
| `ndarrary.ndim` | 秩（轴的数量或者维度） |
| `ndarray.shape` | 数组的维度，对于矩阵，n 行 m 列 |
| `ndarray.size` | 数组元素的总个数，相当于 .shape 中 n\*m 的值 |
| `ndarray.dtype` | ndarray 对象的元素类型 |
| `ndarray.itemsize` | ndarray 对象中每个元素的大小，以字节为单位 |
| `ndarray.flags` | ndarray 对象的内存信息 |
| `ndarray.real` | ndarray元素的实部 |
| `ndarray.imag` | ndarray元素的虚部 |

```python
a = np.zeros((3, 4, 2), dtype=int)  # 三维数组
print(a.ndim)
print(a.shape)
print(a.size)
print(a.dtype)
print(a.itemsize)
print(a.flags)

#  结果
3
(3, 4, 2)
24
int32
4
  C_CONTIGUOUS : True
  F_CONTIGUOUS : False
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  WRITEBACKIFCOPY : False
  UPDATEIFCOPY : False

```

另外注意`flags` 所列信息含义

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0ff81bdefa3e4497a9819aaf055ffb66.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124229549)