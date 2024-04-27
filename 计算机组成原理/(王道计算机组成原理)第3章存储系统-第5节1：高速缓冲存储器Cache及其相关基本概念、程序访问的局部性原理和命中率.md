 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)

- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_7)
- [一：Cache基本原理](#Cache_17)
- [二：程序访问的局部性原理](#_28)
- [三：主存块](#_74)
- [四：命中率和缺失率](#_94)

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F77f5edefb0244fd8998f5c08f0aaba28.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

**由于程序的转移概率和数据分布的离散性很大，所以想要仅仅通过提高主存系统的并行性以此来提升存储器带宽的想法是不现实的。因此这就我们必须从系统结构上加以改进，也即采用[第一节：存储器分类、多级存储系统和存储器性能指标](https://blog.csdn.net/qq_39183034/article/details/119715863)中讲到的多级存储体系**

- Cache-主存层次
- 主存-辅存层次

# 一：Cache基本原理

以微信为例，当你打开微信时，与微信有关的数据和代码将会被加载进主存，比如文字数据、支付数据、运动数据等等。这些数据很多，涉及各个功能，但有的人使用微信可能只偏好于某些方面（比如视频聊天）。所以在这样的情况下，CPU在较长时间内使用到的只是微信的部分程序和数据  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F67741604e93244f99985c97528fc8963.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

**所以可以把把这一部分的数据复制一份给Cache，由于Cache的速度和CPU十分接近，这样的话CPU会直接和Cache交流，整机性能会有明显提升**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F11803461671342a8a9941692e3f7a667.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

# 二：程序访问的局部性原理

如下是一段简单的C语言程序

```c
int sumarrayrows(int a[M][N])
{
            
            
	int i,j,sum=0;
	for(i = 0 ;i < M ;i++)
		for(j = 0;j < N;j++
			sum+=a[i][j];
	return sum;
}
```

该程序在运行后会被加载进内存，程序的本质就是**指令和数据**，所以这段程序在主存中分布情况可能是下面这样

- 假定M、N为2048，按字节编址，int占用4个字节
- 这个二维数组看似是二维的，实则在主存中是一维的，相当于把第二行接到了第一行的尾巴后面。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3fcc1b68232a4045acfeabfc78d30a9b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

**程序访问的局部性原理包括空间局部性和时间局部性**

**空间局部性**：**是指最近未来要用到的信息，很可能与现在使用的信息在存储空间上是邻近的**。例如上例中形参是一个数组，在一个元素访问完毕之后，下一个元素的物理位置和它其实是相邻的

**时间局部性**：**是指最近未来要用到的信息，很可能就是现在正在使用的信息**。比如上例中for循环内的`sum+=a[i][j]`，这一条语句明显会被重复使用多次

**Cache+局部性原理：可以把CPU目前正在访问的元素的邻近数据放到Cache中，之后CPU的访存操作大多数就会针对Cache进行，程序的执行速度的也会得到提升**

---

下面是一个空间局部性很差的程序，它只是在上面程序的基础上把“**一行一行的访问”变为了“一列一列的访问**”。之前，访问完`a[0][0]`，下一个访问的就是`a[0][1]`，而现在下一个却变成了`a[1][0]`了。因为每次访问都要跳过2048个数组元素，也就是8192字节，假如主存与Cache的交换单位较小，**那么每访问一个数组元素都需要装入一个主存块到Cache中**

```c
int sumarrayrows(int a[M][N])
{
            
            
	int i,j,sum=0;
	for(i = 0 ;i < N ;i++)
		for(j = 0;j < M;j++
			sum+=a[i][j];
	return sum;
}
```

# 三：主存块

**主存块：这是主存与Cache之间交换数据的最小单位。也即将主存的存储空间分块，比如每1KB为一块，主存与Cache之间就会以块为单位进行数据交换**

例如下图数组，对于`a[0][0]`我们先判断它属于哪一块，确定好之后再将它所在的块复制到Cache中去

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8e09774e17fb4a4580c445f95868aa2b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

---

**假设主存大小为4M，每1KB为一块，由于4M=4096KB，因此会被分为4096块，然后对其编号（0-4095）**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1bfcb1d6d0284c4185040ceafb0bfb7c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)  
**由于 2 22 = 4194304 2\^\{22\}=4 194 304 222\=4194304，所以这些地址至少需要22位才能全部表示，我们将22位地址拆分为两个部分，前12位表示块号（ 2 12 = 4096 2\^\{12\}=4096 212\=4096），后10位表示块内地址（ 2 10 = 1024 2\^\{10\}=1024 210\=1024）**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feb375b69519f4bcdb22fb1434c9fceb2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119952152)

# 四：命中率和缺失率

**命中率 H H H： CPU欲访问的信息已经在Cache中的比率**

**缺失率： CPU欲访问的信息未经在Cache中的比率，为 1 − H 1-H 1−H**

- 假设某程序执行期间，Cache的的总命中次数为 N c N\_\{c\} Nc​，访问主存的总次数为 N M N\_\{M\} NM​，**那么 H H H\= N c N c + N m \\frac\{N\_\{c\}\}\{N\_\{c\}+N\{m\}\} Nc​+NmNc​​**

- **命中率 H H H越接近1越好**

- 设 t c t\_\{c\} tc​为命中时的Cache访问时间， t m t\_\{m\} tm​为未命中时的访问时间,则**Cache-主存系统的平均访问时间 T a = H t c + \( 1 − H \) t m T\_\{a\}=Ht\_\{c\}+\(1-H\)t\_\{m\} Ta​\=Htc​+\(1−H\)tm​**