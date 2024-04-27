 

- [部分参考：菜鸟教程](https://www.runoob.com/numpy/numpy-array-manipulation.html)  

  ### 文章目录

  - [一：修改数组形状](#_7)
  - [二：翻转数组](#_69)
  - [三：修改数组维度](#_136)
  - [四：连接数组](#_195)
  - [五：分割数组](#_241)
  - [六：数组元素的添加与删除](#_269)

# 一：修改数组形状

**①：使用`reshape()`可以在不改变数据的条件下修改数组形状，格式如下**

```python
numpy.reshape(arr, newshape, order='C')
```

- `arr`：要修改形状的数组
- `newshape`：整数或者整数数组，新的形状应当兼容原有形状
- `order`：‘C’ – 按行，‘F’ – 按列，‘A’ – 原顺序，‘k’ – 元素在内存中的出现顺序

**举例**

```python
a = np.random.randint(0, 100, 9)  # 一维数组
print(a)
print("-"*20)
b = a.reshape(3, 3)  # 修改为二维数组(行优先)
c = reshape(a, [3, 3], 'F')  # 修改为二维数组(列优先)
print(b)
print("-"*20)
print(c)

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4650ea87f55b4d1b9da2e721bc40a0f0.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**②：使用`flatteen(order = 'C')`可以展开数组**

- `order`：‘C’ – 按行，‘F’ – 按列，‘A’ – 原顺序，‘k’ – 元素在内存中的出现顺序

**举例**

```python
a = np.random.randint(0, 10, size=[6, 6])
print(a)
print("-"*20)
b = a.flatten()  # 按行展开
c = a.flatten('F')  # 按列展开
print(b)
print("-"*20)
print(c)
print("-"*20)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F656de564d3a749dca12ec94c8540504e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

# 二：翻转数组

**①：使用`transpose()`可以对换数组维度，格式如下**

```python
numpy.transpose(arr, axes)
```

- `arr`：要操作的数组
- `axes`：整数列表，对应维度，通常所有维度都会对换

**举例**

```python
a = np.random.randint(0, 10, size=[3, 4])
print(a)
print("-"*20)
b = np.transpose(a)
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5b069714cb94443dbf074e7a9910bb3a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**②：`numpy.rollaxis()` 向后滚动特定的轴到一个特定位置，格式如下**

```python
numpy.rollaxis(arr, axis, start)
```

- `arr`：数组
- `axis`：要向后滚动的轴，其它轴的相对位置不会改变
- `start`：默认为零，表示完整的滚动。会滚动到特定位置

---

**③：`numpy.swapaxes()`用于交换数组的两个轴，格式如下**

```python
numpy.swapaxes(arr, axis1, axis2)
```

- `arr`：输入的数组
- `axis1`：对应第一个轴的整数
- `axis2`：对应第二个轴的整数

**举例**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7ff8d58987e94c34ad58bfd9a7a9edf4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

```python
a = np.random.randint(0, 10, size=[2, 3, 3])
print(a)
print("-"*20)
b = np.swapaxes(a, 0, 2)  # 也即[2, 3, 3]变为[3, 3, 2]
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F853bebc5095944cc92cc2c8a0bbd4681.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

# 三：修改数组维度

**①使用`numpy.expand_dims()` 通过在指定位置插入新的轴来扩展数组形状，函数格式如下**

```python
numpy.expand_dims(arr, axis)
```

- `arr`：输入数组
- `axis`：新轴插入的位置

**举例**

如下，二维数组是（axis0, axis1），三维数组是（axis0, axis1, axis2）。在axis=0处插入轴，所以（3, 3）变为（1, 3, 3）；在axis=1处插入轴，所以（3, 3）变为（3, 1, 3）

```python
a = np.random.randint(0, 10, size=[3, 3])
print(a)
print("-"*20)
b = np.expand_dims(a, axis=0)
c = np.expand_dims(a, axis=1)
print(b)
print("-"*20)
print(c)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffd95818cd25a42439806eea76c61f400.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**②：`numpy.squeeze()` 从给定数组的形状中删除一维的条目，用于降维，函数格式如下**

```python
numpy.squeeze(arr, axis)
```

- `arr`：输入数组
- `axis`：整数或整数元组，用于选择形状中一维条目的子集

**举例**

```python
a = np.random.randint(0, 10, size=[1, 3, 3])
print(a)  # 三维
print("-"*20)
b = np.squeeze(a)  # 二维
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5882069dbfc44788b6fa5d688909c111.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

# 四：连接数组

**①：`numpy.concatenate()` 用于沿指定轴连接相同形状的两个或多个数组，格式如下**

```python
numpy.concatenate((a1, a2, ...), axis)
```

- `a1, a2, ...`：相同类型的数组
- `axis`：沿着它连接数组的轴，默认为 0（简单来说：axis 0向下；axis 1向右）

**举例**

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])
c = np.concatenate((a, b), axis=0)
d = np.concatenate((a, b), axis=1)
print(a)
print("-"*20)
print(b)
print("-"*20)
print(c)
print("-"*20)
print(d)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe005e0d2c45e43c0b9dc00bb52951a2a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**②：`numpy.stack()` 用于沿新轴连接数组序列，格式如下**

```python
numpy.stack(arrays, axis)
```

- `arrays`：相同形状的数组序列
- `axis`：返回数组中的轴，输入数组沿着它来堆叠

# 五：分割数组

**①：`numpy.split()` 可以沿特定的轴将数组分割为子数组，格式如下**

```python
numpy.split(ary, indices_or_sections, axis)
```

- `ary`：被分割的数组
- `indices_or_sections`：如果是一个整数，就用该数平均切分，如果是一个数组，为沿轴切分的位置（左开右闭）
- `axis`：设置沿着哪个方向进行切分，默认为 0，横向切分，即水平方向。为 1 时，纵向切分，即竖直方向

**举例**

```python
a = np.random.randint(0, 10, size=[2, 4, 4])
print(a)
print("-"*20)
b = np.split(a, 2, 0)
print(b[0])
print("-"*20)
c = np.split(b[0], 2, 1)
print(c)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc4dd40acdb744fbda45897528e495b07.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

# 六：数组元素的添加与删除

**①：`numpy.resize()` 可以返回指定大小的新数组，如果新数组大小大于原始大小，则包含原始数组中的元素的副本**

```python
numpy.resize(arr, shape)
```

- `arr`：要修改大小的数组
- `shape`：返回数组的新形状

**举例**

```python
a = np.random.randint(0, 10, size=[2, 3])
print(a)
print("-"*20)
b = np.resize(a, [3, 2])
c = np.resize(a, [3, 3])
print(b)
print("-"*20)
print(c)  # 数组变大后元素会重复出现
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2aca5f31ff334dcca1ff443c071a5ef3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**②：`numpy.append()`可以 在数组的末尾添加值。 追加操作会分配整个数组，并把原来的数组复制到新数组中。 此外，输入数组的维度必须匹配否则将生成`ValueError`；且该函数返回的始终是一个一维数组**

```python
numpy.append(arr, values, axis=None)
```

- `arr`：输入数组
- `values`：要向arr添加的值，需要和arr形状相同（除了要添加的轴）
- `axis`：默认为 None。当axis无定义时，是横向加成，返回总是为一维数组！当axis有定义的时候，分别为0和1的时候。当axis有定义的时候，分别为0和1的时候（列数要相同）。当axis为1时，数组是加在右边（行数要相同）

**举例**

```python
a = np.random.randint(0, 10, size=[2, 3])
print(a)
print("-"*20)
b = np.append(a, [np.random.randint(0, 10, 3)])  # 向数组添加元素
print(b)
print("-"*20)
c = np.append(a, [np.random.randint(0, 10, 3)], axis=0)  # 向轴0添加元素
print(c)
print("-"*20)
d = np.append(a, [np.random.randint(0, 10, 3), np.random.randint(0, 10, 3)], axis=1)  # 向轴1添加元素
print(d)
print("-"*20)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd28311b5da764abaaae4abcefa39e30c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)

---

**③：`numpy.delete()`可以返回从输入数组中删除指定子数组的新数组**

```python
Numpy.delete(arr, obj, axis)
```

- `arr`：输入数组
- `obj`：可以被切片，整数或者整数数组，表明要从输入数组删除的子数组
- `axis`：沿着它删除给定子数组的轴，如果未提供，则输入数组会被展开

**举例**

```python
a = np.arange(12).reshape(3, 4)
print(a)
print("-"*20)
b = np.delete(a, 5)  # 如果未传递axis参数，数组会被展开为一维
print(b)
print("-"*20)
c = np.delete(a, 1, axis=1)  # 删除第二列
print(c)
print("-"*20)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdd78203eeeb74668beea9e0f7ed0531a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124347296)