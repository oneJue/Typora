 

### 文章目录

- [一：散点图（scatter）](#scatter_30)
- [二：柱状图（bar）](#bar_115)
- [三：直方图（hist）](#hist_117)
- [四：饼图（pie）](#pie_119)

Matplotlib可以绘制很多图形，具体可参加官网：[点击跳转](https://www.runoob.com/matplotlib/matplotlib-tutorial.html)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2c657b03b6f4440da56a1cefc6824ee7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

日常学习、生活中，以下图形的使用频次较高

- **折线图（plot）**：以**折线的上升或下降**来表示统计数量的**增减变化**；它能够显示数据的**变化趋势**，反应事物的**变化情况**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F08b8d829b3d142dbbee0b99fd8095ff2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

- **散点图（scatter）**：用两组数据构成多个坐标点，考察**坐标点的分布**，判断两变量间是否存在**某种关联**或总结坐标点的**分布模式**；它可以判断变量之间是否存在**数量关联趋势**以及展示**分布规律**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9af64d0ac6f94f2e8ae6bfd726f9c67f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

- **柱状图（bar）**：排列在工作表的列或行中的数据可以绘制在柱状图中；它绘制出的是**离散的数据**，能够一眼看出**各个数据的大小**，比较数据之间的**差别**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd5d039904f894f74b65922b604f6ff5e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

- **直方图（hist）**：由一系列高**度不等的纵向条纹**或线段表示数据分布的情况，一般用**横轴表示数据范围，纵轴表示分布情况**；它可以绘制**连续性的数据**展示一组或多组数据的**分布状况**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc4547a3dbeca4249a5f9070421c803b1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

- **饼图（pie）**：用于表示不同分类的**占比情况**，通过**弧度大小**来对比各种分类；可以表示分类数据的**占比情况**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F34271c9c7d544617a3f40440ad0f5121.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

---

# 一：散点图（scatter）

准备下列数据

```python
x = np.random.uniform(50, 100, 50)
y = 1/x
```

代码如下

```python
import matplotlib.pyplot as plt  # 导入
import random
plt.rcParams['font.sans-serif'] = ['SimHei']   # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False   # 用来正常显示负号


#  数据
x = np.random.uniform(50, 100, 50)
y = 1/x

#  创建画布
plt.figure(figsize=(16, 5), dpi=100)

#  绘制图像
plt.scatter(x, y)

# 显示图像
plt.show()
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F91cc3030cd394287847f8fa86296d990.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

---

对于图的修饰和折线图中所讲的没有区别

```python
import matplotlib.pyplot as plt  # 导入
import random
plt.rcParams['font.sans-serif'] = ['SimHei']   # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False   # 用来正常显示负号


#  数据
x1 = np.random.uniform(0, 2, 50)
y1 = np.random.uniform(1, 3, 50)
x2 = np.random.uniform(6, 8, 50)
y2 = np.random.uniform(6, 8, 50)


#  创建画布
plt.figure(figsize=(16, 5), dpi=100)

#  绘制图像
plt.scatter(x1, y1, color='blue', label='A')
plt.scatter(x2, y2, color='red', label='B')

# 修改刻度
plt.xticks(range(0, 10, 1))
plt.yticks(range(0, 10, 1))

# 显示图例
plt.legend(frameon=False)

# 显示网格
plt.grid(True, linestyle='--', alpha=0.3, color='black')

# 添加坐标轴描述信息
plt.xlabel("X")
plt.ylabel("Y")
plt.title("两组数据的散点图")

# 显示图像
plt.show()
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0afc377c412948c8a5cf3065a11020c2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F124679987)

# 二：柱状图（bar）

# 三：直方图（hist）

# 四：饼图（pie）