 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_6)
- [一：随机算法（RAND）](#RAND_27)
- [二：先进先出算法（FIFO）](#FIFO_113)
- [三：近期最少使用算法（LRU）——效率最高](#LRU_166)
- [四：最不经常使用算法（LFU）](#LFU_314)

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffc0005737c894515a0cf94a9e90390c2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120016603)

在[第六节2：Cache和主存的映射方式（全相联映射、直接映射和组相连映射）](https://blog.csdn.net/qq_39183034/article/details/119967515?spm=1001.2014.3001.5501)中我们讲到了Cache和主存之间的映射关系，细致分析了三种映射方式各自的特点。那么下一个亟待解决的问题就是：**Cache是很小的，主存却很大，如果Cache满了应该怎么办？** 这就是本节的主题——**Cache的替换算法**。当然，不同的映射方式其替换机制也会有所不同

- **全相联映射：Cache完全满了才需要替换，需要在全局中选择替换哪一块**
- **直接映射：如果对应位置为空则直接替换，无需考虑替换算法**
- **组相联映射：分组内满了才需要替换，需要在分组内选择替换哪一块**

**本节以全相联映射为例，介绍以下四种替换算法**

- **随机算法（RAND）**
- **先进先出算法（FIFO）**
- **近期最少使用算法（LRU）**
- **最不频繁使用算法（LFU）**

**在讲解之前大家一定明白一点，CPU每访问一个内存块，都会立即把该内存块调入Cache中**

# 一：随机算法（RAND）

**随机算法（RAND）：若Cache已满，则随机选择一块进行替换**

- **通过以下叙述可知：随机算法十分简单，但是它完全没有考虑到局部性原理，命中率很低，实际效果很不稳定**

如下有4个Cache块，初始状态下4个Cache块均为空，采用**全相连映射**，CPU访问主存块的顺序为：\{ <!-- -->1,2,3,4,1,2,5,1,2,3,4,5\}，CPU**每访问一个内存块，都会立即把该内存块调入Cache中**，前四次调入时由于都有空闲Cahce块，所以不会发生替换

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 |  |  |  |  |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 |  |  |  |  |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 |  |  |  |  |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。由于1,2主存块已经被调入了Cache，所以**直接命中**

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 |  |  |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 |  |  |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 |  |  |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。对于5号主存块，它并没有调入Cache中，因此需要立即被调入，**但此时已经没有空闲Cache块了，所以需要使用替换算法选择一块换出，然后再把5号主存块调入。这里采用的是随机算法，所以我们可以任意挑选一块调入，比如把3号主存块给替换出去**

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 |  |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 |  |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 |  |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。直接命中

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。和上面一样，随机挑选一块换出

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。和上面一样，随机挑选一块换出

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 4 |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 | 5 |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 否 |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 是 |  |  |

访存结束，\{1,2,3,4,1,2,5,1,2,3,4,5\}。直接命中

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 4 | 4 |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 | 5 | 5 |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 | 3 |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 否 | 是 |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 是 | 否 |  |

# 二：先进先出算法（FIFO）

**先进先出算法（FIFO）：若Cache已满，则替换最先被调入Cache的块**

- **通过以下叙述可知：先进先出算法实现也很简单，但该算法依然没有考虑到局部性原理，因为最先被调入的Cache块也有可能是会频繁访问到的。而且此算法容易产生抖动现象（—刚换上去的块又立马被换下）**

仍然采用之前的例子，直接进行到这一步

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 |  |  |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 |  |  |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 |  |  |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。对于5号主存块，它并没有调入Cache中，因此需要立即被调入，但此时已经没有空闲Cache块了，所以需要使用替换算法选择一块换出，然后再把5号主存块调入。**这里采用的是FIFO算法，根据先进先出原则，最先被调入Cache的最先被替换，因此1号被替换**

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 5 |  |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 3 |  |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 |  |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 |  |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。此时应该替换2号

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 5 | 5 |  |  |  |  |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 1 |  |  |  |  |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 3 | 3 |  |  |  |  |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 |  |  |  |  |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 否 |  |  |  |  |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 是 |  |  |  |  |  |

后续步骤不再详细演示，最终结束状态如下

| 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 5 | 5 | 5 | 5 | 4 | 4 |  |
| **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 1 | 1 | 1 | 1 | 5 |  |
| **Cache #2** |  |  | 3 | 3 | 3 | 3 | 3 | 3 | 2 | 2 | 2 | 2 |  |
| **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 | 3 |  |
| **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 否 | 否 | 否 | 否 | 否 |  |
| **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 是 | 是 | 是 | 是 | 是 |  |

# 三：近期最少使用算法（LRU）——效率最高

**近期最少使用算法（LRU）：该算法会为每一个Cache块设置一个计数器，用于记录每个Cache块究竟有多长时间没有被访问了。在替换时直接选取计数器最大的替换即可**

- **通过以下叙述可知：LRU算法是基于局部性原理的，近期访问过的主存块，在不久的将来很有可能会被再次访问到，因此这种淘汰机制是合理的。LRU算法的实际运行效果也很优秀，Cache命中率也高**

**计数器的变化规则如下**

- **命中时：所命中的块的计数器清零，计数器比其低的块的计数器+1，其余不变**
- **未命中且还有空闲块时：新装入的块的计数器置为0，其余非空闲块的计数器全+1**
- **未命中且没有空闲块时：计数器最大的块被淘汰，新装入块的计数器置为0，其余块的计数器+1**

如下表格表示初始状态

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** |  |  |  |  |  |  |  |  |  |  |  |  |  |

继续访存，\{ <!-- -->1,2,3,4,1,2,5,1,2,3,4,5\}。由于1装入了第一个Cache块，属于**未命中且还有空闲块**，因此该块计数器置为0，其余非空闲块计数器全+1

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** | 1 |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 |  |  |  |  |  |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。情况同上

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **Cache #0** | 1 | 1 |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  | 2 |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 |  |  |  |  |  |  |  |  |  |  |  |

第三、四个主存块亦是如此

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 3 | **Cache #0** | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 |  |  |  |  |  |  |  |  |  |
| 1 | **Cache #2** |  |  | 3 | 3 |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。**此时Cache命中，因此需要将所命中块的计数器清零，比其低的块的计数器+1，其余不变**

- 这一点其实就体现了LRU算法的核心，它能保证最近访问的块的计数器一定很低

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |  |
| 3 | **Cache #1** |  | 2 | 2 | 2 | 2 |  |  |  |  |  |  |  |  |
| 2 | **Cache #2** |  |  | 3 | 3 | 3 |  |  |  |  |  |  |  |  |
| 1 | **Cache #3** |  |  |  | 4 | 4 |  |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 |  |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}，情况同上

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |  |
| 3 | **Cache #2** |  |  | 3 | 3 | 3 | 3 |  |  |  |  |  |  |  |
| 2 | **Cache #3** |  |  |  | 4 | 4 | 4 |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。此时属于 **“未命中且没有空闲行”，所以计数器最大的块会被淘汰（淘汰3号主存块），新装入块的计数器置为0，其余块计数器全+1**

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |
| 1 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 |  |  |  |  |  |  |
| 3 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 |  |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。直接命中

- **注意**：只需要将“比该块计数器值小的块的计数器+1”即可，大的不变，因此上面表格中的3号Cache的计时器就不用动了，这里很容易犯错

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |
| 1 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 |  |  |  |  |  |
| 3 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 |  |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。直接命中

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |
| 0 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |
| 2 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 |  |  |  |  |
| 3 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 |  |  |  |  |

继续访存，\{1,2,3,4,1,2,5,1,2,3,4,5\}。未命中，且没有空行

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |
| 1 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |
| 3 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。未命中，且没有空行

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 3 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 | 4 |  |  |
| 1 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 否 |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 是 |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。未命中，且没有空行

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 5 |  |
| 3 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |
| 1 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 | 4 | 4 |  |
| 2 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 | 3 |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 否 | 否 |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 是 | 是 |  |

# 四：最不经常使用算法（LFU）

**最不经常使用算法（LFU）：该算法会为每一个Cache块设置一个计数器，用于记录每个Cache块被访问过几次。在替换时直接选取计数器最小的替换即可**

- **通过以下叙述可知：LFU算法并没有很好地遵循局部性原理，比如微信聊天相关的块，在某个时间段内使用率会很高，但是一段时间后使用率会很低，并不科学**

**计数器的变化规则为：**

- **新调入的块计数器为0，之后每访问一次计数器就+1。需要替换时，选择计数器最小的一行替换**
- **若有多个计数器最小的行，可以按照行号递增或FIFO策略进行选择**

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** |  |  |  |  |  |  |  |  |  |  |  |  |  |

继续访存，也即\{ <!-- -->1,2,3,4,1,2,5,1,2,3,4,5\}

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | **Cache #0** | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #1** |  | 2 | 2 | 2 |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 |  |  |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 |  |  |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。发生命中，计数器+1

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |  |
| 1 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 |  |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 | 4 | 4 |  |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 |  |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 |  |  |  |  |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。**选择计数器最小的那一行，但是这里有两行相同（都是0），所以再按照FIFO策略选择3号主存块淘汰**

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |  |  |
| 1 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 |  |  |  |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 |  |  |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 |  |  |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 |  |  |  |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。1,2命中，计数器+1

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 |  |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 |  |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 |  |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 |  |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。需要进行替换，**这里我们再采用行号递增的规则淘汰5号主存块**

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 3 |  |  |  |
| 0 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 4 |  |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 |  |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 |  |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。命中，计时器+1

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 3 | 3 |  |  |
| 1 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 |  |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 是 |  |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 否 |  |  |

继续访存，也即\{1,2,3,4,1,2,5,1,2,3,4,5\}。需要替换，只剩一个最小的了，替换3号主存块即可

| 计时器 | 访问主存块 | 1 | 2 | 3 | 4 | 1 | 2 | 5 | 1 | 2 | 3 | 4 | 5 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 | **Cache #0** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  |
| 2 | **Cache #1** |  | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |  |
| 0 | **Cache #2** |  |  | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 3 | 3 | 5 |  |
| 1 | **Cache #3** |  |  |  | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 |  |
|  | **是否命中？** | 否 | 否 | 否 | 否 | 是 | 是 | 否 | 是 | 是 | 否 | 是 | 否 |  |
|  | **是否替换？** | 否 | 否 | 否 | 否 | 否 | 否 | 是 | 否 | 否 | 是 | 否 | 是 |  |