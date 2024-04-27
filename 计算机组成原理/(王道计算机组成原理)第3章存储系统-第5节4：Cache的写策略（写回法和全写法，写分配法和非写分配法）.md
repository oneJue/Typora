 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_15)
- [一：写命中](#_22)
- - [（1）写回法\(write-back\)](#1writeback_29)
  - [（2）全写法\(write-through\)](#2writethrough_40)
- [二：写不命中](#_55)
- - [（1）写分配法\(write-allocate\)](#1writeallocate_62)
  - [（2）非写分配法（not-write-allocate）](#2notwriteallocate_67)
- [多级Cache](#Cache_74)

在[第六节2：Cache和主存的映射方式（全相联映射、直接映射和组相联映射）](https://blog.csdn.net/qq_39183034/article/details/119967515?spm=1001.2014.3001.5501)和[第六节3：页面置换算法（FIFO,近期最少使用算法-LRU，LFU）](https://blog.csdn.net/qq_39183034/article/details/120016603?spm=1001.2014.3001.5501)这两篇文章中分别探讨了主存、Cache之间的映射关系以及Cache的替换算法。关于Cache，最后需要解决的一个问题就是：**如何保持Cache与数据母体的一致性**？因为我们知道，Cache中保存的只是**主存数据的副本**，一旦对Cache进行写操作就一定会导致两部分数据出现不一致，而对于读操作则不关心。这就是Cache写策略所要探讨的问题。

**Cache写策略分写命中和写不命中两种情况**

- **写命中**：有全写法和写回法
- **写不命中**：有写分配法和非写分配法两种

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd0d319980da349498d76041f5e3b54c2.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)

# 一：写命中

**写命中：写命中时主存块被调入Cache中，也即要被修改的单元在Cache中。此时有两种方法**

- 写回法
- 全写法

## （1）写回法\(write-back\)

**写回法\(write-back\)：是指当CPU写命中时，只修改Cache中的内容，而不立即写入主存，只有当此块被换出时才写回主存。如下图，绿色主存块写命中，修改时只在Cache上修改，而不立即写入主存，只有当绿色块被替换时才会写回主存**

- **优点**：减少了访存次数
- **缺点**：存在数据不一致的隐患

**这种方法需要判断Cache是否被修改过，因此在对应Cache块还会增加一个“脏位”，用于标识是否修改过，如果对应位为1表示修改过，那么在被替换时该Cache块中的内容会被写回至标记位所定位的主存块上**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc671374cd7da4126822c27d60c43e03e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)

## （2）全写法\(write-through\)

**全写法\(write-through\)：是指当CPU写命中时，必须把数据同时写入Cache和主存。当某一块需要替换时，不必把这一块写回主存，新调入的块直接覆盖即可**

- **优点**：实现简单，更能保持数据的一致性
- **缺点**：增加了访存次数，降低了Cache的效率

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9bc7078200cf4a0ca9c1027130281852.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)

**为了减少全写法直接写入主存时所产生时间损耗，通常会在Cache和主存之间加入写缓冲（Write Buffer）。CPU同时写数据到Cache和写缓冲中，写缓冲再控制将内容写入主存，写缓冲是一个FIFO队列，可以解决速度不匹配的问题**

- 注意频繁写会导致缓冲区溢出  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbf9725de52f44699b8220de780c6bea3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff63892a161ce487999489e19960bd0e1.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)

# 二：写不命中

**写不命中：写不命中时被修改的单元不在Cache中。此时有两种方法**

- 写分配法
- 非写分配法

## （1）写分配法\(write-allocate\)

**写分配法：是指当写不命中时，会把主存的块加载到Cache中，然后更新此Cache块，通常会结合写回法使用**

## （2）非写分配法（not-write-allocate）

**非写分配法：是指当写不命中时，CPU直接对主存的块进行修改，而不调入Cache中（注意只有读操作才将主存块调入Cache中），通常会结合全写法使用**

# 多级Cache

现代计算机的Cache一般是多级的（通常三级）。对于三级Cache，**按离CPU的远近可命名为L1 Cache、 L2 Cache、 L3Cache，离CPU越远，访问速度就越慢，容量也越大，反之相反。** 其中指令Cache与数据Cache分离一般在L1级，此时通常**为写分配法和写回法**合用

下图是一个含有两级的Cache系统，**L1对L2使用全写法，L2对主存使用写回法**，由于L2的存在，其访问速度远大于主存，因此避免了因频繁写时导致的缓冲区溢出  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff7544f26845644a392d959835fe6ea63.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)

如下图是资源管理器中显示的Cache信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc9c3e3ac37104e2bbd38d2602e6ff69e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120044483)