 

### 文章目录

- [一：什么是Matplotlib](#Matplotlib_3)
- [二：实现一个简单的Matplotlib图](#Matplotlib_19)
- [三：Matplotlib图像结构](#Matplotlib_35)
- [四：Matplotlib三层结构](#Matplotlib_42)
- - [（1）容器层](#1_53)
  - [（2）辅助显示层](#2_88)
  - [（3）图像层](#3_100)

# 一：什么是Matplotlib

**Matplotlib 是 Python 2D-绘图领域使用最广泛的套件**。它能让使用者很轻松地将数据图形化，并且提供多样化的输出格式

- Mat是指Matrix：表示矩阵（二维表）
- plot：画图
- lib是指library：表示库

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fda1afae64f564540b48ad3e6c14b0f7e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

不管是Matplotlib、又或者是MatLab，使用他们的一个很大的目的就是为了**数据可视化**，它可以帮助我们清晰的展示、理解数据

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbfa2f36170634a068b6c7bf15b62b19b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

# 二：实现一个简单的Matplotlib图

代码如下

```python
import matplotlib.pyplot as plt  # 导入
#  matplotlib inline 如果使用jupyter，需要加入


plt.figure()  # 创建一块画布
plt.plot([1, 0, 9], [4, 5, 6])  # 描点画图：(1,4)、(0,5)、(9,6)
plt.show()  # 展示
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe4df031653ce415ebe9abddac7a47129.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

# 三：Matplotlib图像结构

Matplotlib绘制出的图像结构、基本要素如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F404e07881c904866b7610ff95b01863c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

# 四：Matplotlib三层结构

Matplotlib所绘制出的图像由三层构成

- 容器层
- 辅助显示层
- 图像层

这部分内容我认为非常重要，把它们之间的逻辑关系理清有助于我们更好的绘图

## （1）容器层

容器层是绘图的**基础**，由以下部分构成

- **画板层（Canvas）**：位于最底层（这是一个系统层），用于放置画布（Figure），程序运行后直接就给你显示了一个画板
- **画布层（Figure）**：位于Canvas上方（这是一个应用层），可以通过`plt.figure()`设定画布的大小、分辨率等等， 一个Figure可以划分出多个绘图区域Axes（用户自定，如果你不设定那么一个Figure就一个绘图区）
- **绘图区/坐标系（Axes）**：这是数据的绘图区域，可以通过`plt.subplots()`划分出来。每个Axes上有Axis，也即坐标轴。**辅助显示层和图像层均位于其上**

**因此**

- 一个画布\(Figure\)可以包含多个坐标系/绘图区（Axes），但是一个Axes只能属于一个Figure
- 一个坐标系/绘图区（Axes）可以包含多个坐标轴（Axis）；如果两个Axis就是二维的

比如

```python
import  matplotlib.pyplot as plt
plt.figure(figsize=(6,6), dpi=80)
plt.figure(1)
ax1 = plt.subplot(221)
plt.plot([1,2,3,4],[4,5,7,8], color="r",linestyle = "--")
ax2 = plt.subplot(222)
plt.plot([1,2,3,5],[2,3,5,7],color="y",linestyle = "-")
ax3 = plt.subplot(212)
plt.plot([1,2,3,4],[11,22,33,44],color="g",linestyle = "-.")

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9da890e33533413894613fc8b5a4c3e5.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

## （2）辅助显示层

辅助显示层是为了**使图像能够更加直观、更加容易被用户理解**，比如PS中的参考线。例如

- 边框线（spines）
- 坐标轴（axis）
- 坐标轴名称（axis label）
- 坐标轴刻度（tick）
- 刻度标签（tick label）
- 网格线（grid）

## （3）图像层

图像层指的就是用户通过`plot`、`scatter`、`bar`、`pie`等绘制出的图像

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fabd3c99cdb7b45d8abe5f18d814f58f3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124643752)

---

**总的来说**

- Canvas位于最底层，用户不用管
- Figure建立在Canvas之上
- Axes建立在Figure之上，可以划分多个
- axis、legend等辅助显示层以及图像层建立在Axes之上