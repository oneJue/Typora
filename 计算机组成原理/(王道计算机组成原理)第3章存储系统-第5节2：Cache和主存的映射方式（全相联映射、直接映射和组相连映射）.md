 

- [王道考研复习指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378?p=7281)
- [专栏目录首页：【专栏必读】王道考研408计算机组成原理万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)

### 文章目录

- [本节思维导图](#_6)
- [一：全相联映射](#_40)
- - [（1）如何映射](#1_42)
  - [（2）如何访存](#2_54)
- [二：直接映射](#_62)
- - [（1）如何映射](#1_64)
  - [（2）如何访存](#2_82)
- [三：组相联映射](#_90)
- - [（1）如何映射](#1_91)
  - [（2）如何访存](#2_102)
- [四：三种方式各自优缺点](#_107)

# 本节思维导图

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F430beca7970e4647a664cc5566b7d1f3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

前面说过，Cache中保存的实际是主存中的数据副本，所以这里就会涉及一个很重要的问题：**主存内容和Cache中的内容是如何对应，也即是如何映射的？** 地址映射的方法有以下三种

- **全相联映射**：主存块可以放在Cache的**任何位置**
- **直接映射**：每个主存块**只能放到一个特定的位置**，由**主存块号\%Cache总块数**来确定
- **组相联映射**：将Cache块分为**若干组**，每个主存块可以放到**特定分组中的任意一个位置**，其中**组号=主存块号\%分组数**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3dd43866df824352b1f6d95aceedc1c2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

把主存块放到Cache中后：

- **要给每个Cache块增加一个“标记位”**，记录**对应的主存块号**
- **再给每个Cache块增加一个“有效位”**，用于**控制其是否生效**，以免产生冲突  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe6efd3c032b44f12a1f4ceb3d21742e2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

**分块后需要对主存进行编号，假设某个计算机的主存地址空间大小为256MB，按字节编址，Cache有8个Cache行（也即Cache块），行长（也即块大小）为64B。**

- **主存块号编号**：块大小为64B=26B、主存大小为256MB=228B，那么就有228/26个主存块，所以主存编号为从0到222\-1
- **块内地址**：这22位是用于区分主存块的，所以剩下的28-22=6位则为每个主存块的地址范围（或空间）。也即先利用高22位确定是哪一块，然后在该块中用低6位确定地址

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8eb06afbfea84294afc78c9b1e89d37a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

# 一：全相联映射

## （1）如何映射

**全相联映射：主存块可以放在Cache的任何位置**

例如下图中的0号主存块，它就可以放置到Cache的3号位置，每行的标记号用于指出该行取自主存的哪一块，同时将对应的有效位置为1

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcba5aa8f5027451e8c1fac4afa35392f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

## （2）如何访存

**以上图紫色主存块为例，其地址为1…1101001110，在全相联映射下，CPU访存时首先会取该地址的前22位，也即主存块号，来和Cache中每一行的标记进行对比**

- **若标记号=块号且有效位为1**：说明**Cache命中**，也就是说此时访问的数据在Cache中是有副本的，接着**只需在Cache中访问后6位地址所定位的单元**即可
- **若标记号不匹配或匹配但有效位为0**：此时说明**Cache未命中**，则正常访问主存，也即要从主存中取数据

# 二：直接映射

## （1）如何映射

**直接映射：每个主存块只能放到一个特定的位置，由主存块号\%Cache总块数来确定**

例如下图中的0号主存块，由于0\%8=0，因此它**只能放到Cache的0号位置**；对于8号主存块，由于8\%8=0，所以它也要放到0号位置，**而且需要把之前的0号主存块给腾空**，相应的标记位也要修改  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F22e2a223cf47445a94134c9a0dd45225.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

**“\%”运算具有一些特性，这里Cache块数=8=23，其指数部分为3，这意味着主存块号中的后3位直接反映了该主存块在Cache中的位置。例如上图中的0号和8号，其主存块号的后三位均为000，这正好对应了它们在Cache的第0行**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6f871aa4cdb748cfbba2064b7161b75d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)  
**因此标记可以直接取主存块号的前19位，相应地址形式会变化为下面这样**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6a7f8fba03414d55846dd50532a9a56e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_15%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

## （2）如何访存

**以上图橙色主存块为例，其地址为0…01000001110。在直接映射下，CPU访存时首先会根据主存块号的后三位确定Cache行（而不用挨个比较），接着会判断前19位和标记号是否匹配并同时判断有效位是否为1**

# 三：组相联映射

## （1）如何映射

**组相联映射：将Cache块分为若干组，每个主存块可以放到特定分组中的任意一个位置，其中组号=主存块号\%分组数**

以2路组相联为例（2块为一组，分为四组）。对于下图中1号主存块，由于1\%4=1，因此它会被放入**第一组的任意位置**；对于222\-3号主存块也会放入第一组，它会放到该组另一个空闲位置  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F21c73108c94d44b0bb72fc36b958de3f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

**和直接映射一样，由于“\%”的运算特性，这里分组数=4=22，这意味着主存块号中的后2位反映了该主存块在哪一个组。所以标记号只需取前20位，相应地址形式会变化为下面这样**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa70c89c74dfe42a0a0ec25599a802c85.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_16%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F119967515)

## （2）如何访存

**以上图橙色主存块为例，其地址为1…0100001110。在组相联映射下，CPU访存时首先会根据主存块号的后两位确定所属分组号，接着会判断主存块号的前20位与分组内的某个标记号是否匹配同时判断有效位是否为1**

# 四：三种方式各自优缺点

**全相联映射**

- **优点**：Cache存储空间利用充分，命中率高
- **缺点**：查找慢，有时可能要比对所有行的标记

**直接映射**

- **优点**：对于任意一个位置，只需对比一个标记，速度最快
- **缺点**：缺点就是Cache存储空间利用不充分，命中率低

**组相联映射**：**综合效果较好**