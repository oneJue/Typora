 

### 文章目录

- [一：基本索引和切片](#_12)
- [二：高级索引和切片](#_156)
- - [（1）整数数组索引](#1_158)
  - [（2）布尔索引](#2_187)
- [三：切片的本质是引用](#_204)

Numpy数组创建好之后，其最常用、最重要的操作就是**索引**和**切片**。下面的例子中以**二维、三维数组**居多，这样更具代表性

- **提前说明**：Numpy中索引时，使用`,`区分维度

# 一：基本索引和切片

**①**：最基本的索引访问

```python
a = np.random.randint(0, 10, size=[3, 3, 3]) # 三维数组
print(a)
print("-----------")
print(a[0])
print("-----------")
print(a[0, 2])
print("-----------")
print(a[0, 2, 1])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3b9ef3a8c41a46c19d8ecdab5b8eaad1.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

索引后进行修改时一定**保证等式两边类型相同**

```python
a = np.random.randint(0, 10, size=[3, 3, 3])  # 三维数组
print(a)
a[0] = np.zeros([3, 3], dtype=int)  # 二维数组
print("-------------")
print(a)
a[0, 1] = np.ones(3, dtype=int)  # 一维数组
print("-------------")
print(a)
a[0, 1, 2] = 9  # 一个数
print("-------------")
print(a)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd6eb8e9405ee46519fb1737a381c904b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

---

**②**：切片可以通过`start:stop:step`来操作，而且也支持负索引

 -    **正向**：第一个元素索引为0，然后从左向右依次增大
 -    **反向**：第一个元素索引为-1，然后从右向左依次减小

```python
a = np.random.randint(0, 10, size=[3, 3, 3])  # 三维数组
print(a)
print("--------------")
print(a[0:2:1])  # 选中第一个和第二个二维数组
print("--------------")
print(a[0, 0:2:1])  # 选中第一个二维数组的第一行和第二行
print("---------------")
print(a[0, 0, -1::])  # 选中第一个二维数组的第一行的最后一个数
```

```python
结果
[[[1 1 3]
  [9 7 1]
  [1 7 7]]

 [[0 6 9]
  [0 1 2]
  [5 2 8]]

 [[4 0 9]
  [5 5 8]
  [7 9 2]]]
--------------
[[[1 1 3]
  [9 7 1]
  [1 7 7]]

 [[0 6 9]
  [0 1 2]
  [5 2 8]]]
--------------
[[1 1 3]
 [9 7 1]]
---------------
[3]

```

---

**③：** 上面的操作选中都是行，如果希望选中的是列，或者是一些特殊行列可以这样操作

```python
a = np.random.randint(0, 10, size=[6, 6])
print(a)
print("-"*20)
print(a[:, 0:5:2])  # 选中第1、3、5列
print("-"*20)
print(a[0, 3:5])  # 选中第一行的第4、5个元素
print("-"*20)
print(a[4:, -2:])  # 选中最后两行的最后两列元素
print("-"*20)
print(a[2:5:2, 0::2])  # 选中第3、5行的奇数列



############################答案###################################
[[9 9 4 6 7 6]
 [8 5 2 0 9 4]
 [0 6 8 9 6 5]
 [8 0 4 9 8 7]
 [5 8 6 0 4 2]
 [5 1 5 1 4 3]]
--------------------
[[9 4 7]
 [8 2 9]
 [0 8 6]
 [8 4 8]
 [5 6 4]
 [5 5 4]]
--------------------
[6 7]
--------------------
[[4 2]
 [4 3]]
--------------------
[[0 8 6]
 [5 6 4]]

Process finished with exit code 0

```

# 二：高级索引和切片

## （1）整数数组索引

进行索引时可以将列表传入，**Numpy将会选取各列表对应索引位置处的元素**。例如，你希望选取`(0,0)`，`(1,1)`和`(2,0)`处的元素，那么就可以传入`[0,1,2]`, `[0,1,0]`

```python
a = np.random.randint(0, 10, size=[3, 3, 3])
print(a)
print("-"*20)
print(a[[0, 1], [1, 1], [2, 0]])  # 选取 (0,1,2) (1,1,0)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc33b004c6f374e4fb3fedeb1dbda0e12.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

当然也可以加入切片

```python
a = np.random.randint(0, 10, size=[3, 3, 3])
print(a)
print("-"*20)
print(a[0:2, -2:, [1, 2]])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F21208727a7034f698c5f671480e81a79.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

## （2）布尔索引

可以通过一个**布尔数组**来索引目标数组，需要注意的是布尔数组长度必须和原数组长度相等

```python
a = np.random.randint(0, 10, 10)
print(a)
print("------------------------------------")
mask = np.array(random.randint(0, 2, 10), dtype=bool)  # 布尔类型数组
print(mask)
b = a[mask]
print(b)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7882988db9f1432291f50b92f1fb74ad.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

# 三：切片的本质是引用

**切片在内存中使用的是引用机制**

- 关于引用如果有不理解的，可以参照C++中的这一节[2-5：C++快速入门之引用,引用和指针的区别](https://zhangxing-tech.blog.csdn.net/article/details/116564088)
- 注意，这种现象不会出现在Python中的列表

**引用意味着Python并没有为b分配新的空间，而是让b指向了a的内存空间，那么改变b就会改变a**

```python
a = np.array([1, 2, 3, 4, 5])
b = a[0:3]  # 切片操作
b[0] = 999  # 修改的是b
print(a)  # 打印a
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0e4c7003331c4fe0a860c01dbd938013.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)

显而易见，Numpy这样设计的目的**就是为了节省空间**，但是这种机制缺陷也很大，容易把原来的值也修改了

为了解决这个问题，**在Numpy中可以使用`copy()`方法进行复制，这样它就会再开辟另外一个空间，独立于原来的空间**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa7f6c18cc86147c2b4ba7bf63ddfc7ed.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124530663)