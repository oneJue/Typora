 

### 文章目录

- [一：Pandas了解](#Pandas_2)
- - [（1）什么是Pandas](#1Pandas_4)
  - [（2）Pandas优势](#2Pandas_14)
  - [（3）Pandas安装](#3Pandas_30)
  - [（4）Pandas核心数据结构](#4Pandas_55)
  - [（5）Pandas导入数据](#5Pandas_63)
- [二：DataFrame类型](#DataFrame_107)
- - [（1）DataFrame结构](#1DataFrame_110)
  - [（2）DataFrame构造方法](#2DataFrame_133)
  - [（3）DataFrame索引](#3DataFrame_169)
  - - [A：设置方法](#A_172)
    - [B：索引方法](#B_228)
  - [（4）其他操作](#4_287)
- [三：Series类型](#Series_362)

# 一：Pandas了解

## （1）什么是Pandas

**Pandas**：Pandas是2008年WesMcKinney开发出的库，是一个专门用于**数据挖掘**的开源Python库，**它以Numpy为基础，借力Numpy模块在计算方面性能高的优势**，又基于Matplotlib，可以简便的画图，而且还拥有自己独特的数据结构

- Pandas=Panel+data+analysis

## （2）Pandas优势

在学习完Numpy和Matplotlib后还要学习Pandas的原因在于：**Pandas纳入了大量库和一些标准的数据模型，提供了高效地操作大型数据集所需要的工具，也提供了大量使我们能够快速处理数据的函数和方法**。具体来说：

- Pandas 可以从各种文件格式比如 CSV、JSON、SQL、Microsoft Excel 导入数据。

- Pandas 可以对各种数据进行运算操作，比如归并、再成形、选择，还有数据清洗和数据加工特征。

- Pandas 广泛应用在学术、金融、统计学等各个数据分析领域

## （3）Pandas安装

- 自行查阅，安装非常简单

安装好之后，可以通过下面的语句导入Pandas库以及查看其是否能够正常运行

```python
import pandas as pd


mydataset = {
            
            
  'sites': ["Google", "Runoob", "Wiki"],
  'number': [1, 2, 3]
}

myvar = pd.DataFrame(mydataset)

print(myvar)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa203742d70a44824a7c1de06f39602d5.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

## （4）Pandas核心数据结构

**在Pandas中有两类数据结构**

- `Series`：是一种类似于**一维数组**的对象，它由一组数据（各种Numpy数据类型）以及一组与之相关的数据标签（即索引）组成；它可以保存**不同种类的数据类型**
- `DataFrame`：是一个**表格型**的数据结构（Excel、SQL等），它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）。DataFrame 既有行索引也有列索引，**它可以被看做由 Series 组成的字典（共同用一个索引）**

## （5）Pandas导入数据

①：Pandas导入数据有很多种方式，**这里以最常用的`csv`数据为例**，导入鸢尾花数据集

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
print(data)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd3affb743a4d45cb84019c724460ce98.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

②：另外需要注意，如果**数据量很大**，那么Pandas在读入时花费的时间也会成倍增长，这不利于代码的编写。所以在实际应用中，我们常编写以下代码，**首次读入后就将该数据读入缓存**，下次就能直接读入了

```python
if os.path.exists('Iris.chache'):
    data = pd.read_pickle('Iris.chache')

else:
    data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
	data.to_pickle('Iris.chache')
```

③：如果你不想导入标题行，那么可以这样写

```python
data = pd.read_csv('./test.csv', header=None)
```

④：导入数据时，默认分隔符是`,`，如果是空格可以这样写

```python
data = pd.read_csv('./test.csv',sep=' ')

# 如果有多个空格
data = pd.read_csv('./test.csv',sep=' +')
```

# 二：DataFrame类型

## （1）DataFrame结构

DataFrame 是一个表格型的数据结构，它含有**一组有序的列**，每列可以是不同的值类型（数值、字符串、布尔型值）。**DataFrame 既有行索引也有列索引，它可以被看做由 `Series` 组成的字典（共同用一个索引）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4ebf828b73344bc5a043cced31a60d06.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**Pandas与Numpy数组对应关系如下**

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
print(data)
print(data.values)
print(data.columns)
print(data.index)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8165162f6077499da99c9fcd23958532.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

## （2）DataFrame构造方法

**DataFrame 构造格式下**：

```python
pandas.DataFrame( data, index, columns, dtype, copy)
```

- `data`：一组数据\(`ndarray`、`series`, `map`, `lists`, `dict` 等类型\)。

- `index`：索引值，或者可以称为行标签。

- `columns`：列标签，默认为 RangeIndex \(0, 1, 2, …, n\) 。

- `dtype`：数据类型。

- `copy`：拷贝数据，默认为 False

**举例**

```python
a = pd.DataFrame(np.random.randn(4, 5))  # numpy二维随机数组
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb04ce54a886a4a0a9a3dcb8484d2f7b8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)  
从以上输出结果可以知道， **DataFrame 数据类型一个表格，包含 `rows`（行） 和 `columns`（列）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc8e1b03ce4b946b4a80cc2f2d001ff91.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

## （3）DataFrame索引

### A：设置方法

**①**：Pandas中index默认给出的是一个列表生成器，可以自定义索引，如下

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
new_index = []
for i in data.index:
    new_index.append('第' + str(i+1) + '行')
data.index = new_index
print(data)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8740bee718b44de3ad3a0bca600d0fe6.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)  
还有其他写法

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
new_index = []
for i in data.index:
    new_index.append('第%03d行' % (i+1))
data.index = new_index
print(data)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5d10ce608e924b34b195b0a1c6e2c21d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**②**：设置columns时可以传入一个列表

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
data.columns = ['序号','花瓣长度', '花瓣宽度', '花萼长度', '花萼宽度', '种类']
print(data)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc2a52e7c9a4847d4be99f270e5afeac3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**③**：在设置索引时，还可以传入**字典**，如下

```python
a = pd.DataFrame(
    {
            
            
        "month": [1, 4, 7, 10],
        "year": [2012, 2014, 2013, 2020],
        "sale": [55, 40, 84, 31]
    }
)
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbaec4b29448947b5b32bfd5490eabd61.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

### B：索引方法

**①**：Pandas默认是**对列进行索引**而不是行。要索引行应该这样做

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
new_index = []
for i in data.index:
    new_index.append("第"+str(i+1)+"行")
data.index = new_index
print(data.loc['第4行'])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcd230f43e5fe456aadc34b991400309b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

进行列索引时，和Numpy索引基本一致（注意多列索引时必须加入中括号`[]`）

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
new_index = []
for i in data.index:
    new_index.append("第"+str(i+1)+"行")
data.index = new_index
print(data[['Id', 'Species']])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F322a08b80045489aa63977943a09c191.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

当然，行列索引可以配合使用

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
data.columns = ['序号','花瓣长度', '花瓣宽度', '花萼长度', '花萼宽度', '种类']
sel1 = [i for i in range(0, 30, 3)]
sel2 = data['序号'] % 10 == 0
print(data.loc[sel1, ['序号', '花瓣长度', '花瓣宽度']])
print("="*20)
print(data.loc[sel2, ['序号', '花瓣长度', '花瓣宽度']])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fad4313c4cd8b41139e49ddfd198ec23c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbd26fbffb9e24e4cb80615f288905442.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**②**：索引时，**pandas中的`iloc`函数更为简单**，它可以同时对行列进行任意索引（注意索引都是数字）

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
data.columns = ['序号','花瓣长度', '花瓣宽度', '花萼长度', '花萼宽度', '种类']
print(data.iloc[[i for i in range(0, 150, 20)], [3, 4, 5]])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd63e73e5c60245a98a94420bab605647.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

## （4）其他操作

**①**：Pandas可以直接对列进行运算，例如

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')

test = data['SepalLengthCm']  # 拿出seriese
print(test > 5)  # 返回大于5的数据，如果大于3则显示true
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb71a569ffacf4ac1b98a8ac4c541585e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')

test = data['SepalLengthCm']  # 拿出seriese
nums = pd.value_counts((test > 5))  # 统计大于5的个数
print(nums)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F537c587538e441448172d2fbb2ff1187.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**②**：可以追加多个条件进行索引

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
new_index = []
for i in data.index:
    new_index.append('第'+str(i+1)+'行')
data.index = new_index
select1 = data['Species'] == 'Iris-setosa'  # 选择类别为Iris-setosa
select2 = data['SepalLengthCm'] > 5  # 选择SepalLengthCm>5
select = select1 & select2  # 同时满足select1和select2
ret = data.loc[select]
print(ret)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F305b735dfd2940b487778a13362694a1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

**③**：Pandas也可以对数据进行分组（groupBy），操作如下

```python
data = pd.read_csv(r'E:\Postgraduate\Dataset\Iris.csv')
data_g = data.groupby(by='Species')  # 以Species对原数据进行分组，分为3组
data_g_size = data_g.size()
print(data_g_size.index)
for flower_type in data_g_size.index:
    print("=" * 20)
    print(flower_type)
    flower_type_data = data_g.get_group(flower_type)  # 通过index获取该组的数据
    print(flower_type_data)
    print("="*20)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffaed3736cce049028ac9ca40d0495993.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

# 三：Series类型

Series 类似表格中的一个列（`column`），是**一种带索引（只有行索引）的一维数组**，或者说**DataFrame是Series的容器**，格式如下

```python
pandas.Series( data, index, dtype, name, copy)
```

- `data`：一组数据\(ndarray 类型\)

- `index`：数据索引标签，如果不指定，默认从 0 开始

- `dtype`：数据类型，默认会自己判断

- `name`：设置名称

- `copy`：拷贝数据，默认为 False

例如

```python
a = pd.Series([1, 3, 'str', np.nan, 7], index=['A', 'B', 'C', 'D', 'E'])
print(a)
print("-"*20)
print(a['C'])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F712b42bcc93a4aa8b3a378de71f9bc91.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)

---

注意：可以使用`pandas.index.name`来为索引列起一个名字

```python
a = pd.Series([1, 3, 'str', np.nan, 7], index=['A', 'B', 'C', 'D', 'E'])
print(a)
print("-"*20)
a.index.name = '索引'
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F14e3de2545e14bb0b800d2bd80237769.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124570763)