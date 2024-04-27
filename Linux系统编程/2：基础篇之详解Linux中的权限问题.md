 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）超级用户和普通用户](#1_5)
  - [（2）Linux权限管理](#2Linux_13)
  - - [A：文件访问者的分类](#A_14)
    - [B：文件类型和访问权限](#B_20)
    - - [A：文件类型](#A_25)
      - [B：基本权限](#B_36)
      - [C：权限的表示方法](#C_44)
      - [D：权限的设置](#D_51)
      - [E：粘滞位](#E_126)
- [补：](#_145)

## （1）超级用户和普通用户

- 超级用户：想干什么就干什么
- 普通用户：权限受到部分限制  
  **普通用户切换到超级用户**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122220319897.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
  **切换到其他用户**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122220438719.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

## （2）Linux权限管理

### A：文件访问者的分类

Linux把文件的访问者分为如下三类

- u（user）：文件或文件目录的所有者
- g（group）：文件和文件目录的所有者所在组的用户
- o（other）：其他用户

### B：文件类型和访问权限

使用ls-al列出文件的属性 信息  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122221200382.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
各种信息所展示的含义如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122221124475.jpg&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

#### A：文件类型

| 缩写 | 表示类型 |
| --- | --- |
| d | 文件夹 |
| l | 软链接（可以理解Windows中的快捷方式） |
| s | 套接字文件 |
| b | 块设备文件；二进制文件 |
| c | 字符设备文件 |
| p | 命名管道文件 |
| \- | 普通文件 |

#### B：基本权限

| 权限 | 说明 |
| --- | --- |
| 读（r） | 对于文件就是读文件；对于目录就是浏览目录信息 |
| 写（w） | 对于文件就是修改文件内容；对于目录就是移动删除目录内的文件 |
| 执行（x） | 对文件就是执行文件；对于目录就是进入目录 |
| \- | 无权限 |

#### C：权限的表示方法

**字符表示方法**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123135620171.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
**8进制表示方法**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123135706183.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
**示例**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123135942346.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

#### D：权限的设置

\(**只有文件的拥有者和root才可以更改权限**\)  
**命令一：chmod（修改权限）**

```powershell
//基本格式
chmod [参数][权限][文件名]

//[权限]格式
[用户符号]（+/-/=）[权限字符]
```

其中的用户符号

- `u`：拥有者
- `g`：拥有者
- `o`：其他用户
- `a`：所有用户

其中的权限字符

- `+`：增加权限符号所代表的权限
- `-`：去掉权限符号所代表的权限
- `=`：向权限范围赋予权限符号所代表的权限

示例一：去掉拥有者的读权限  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123141959139.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
示例二：批量化操作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123142836318.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
示例三：目录权限与文件权限

- 目录的读权限：是指允许查看目录下的文件（和目录下的文件没有关系）
- 目录的写权限：是指允许在该目录下创建文件（和目录下的文件没有关系）

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123145850385.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123150254351.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

- 目录的执行权限：就是指是否可以cd这个目录。由于**目录的读写权限都是建立在执行权限上的，所以一旦执行权限被关闭，那么其读写权限也同样会被关闭**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123151002442.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

**命令二：chown（修改文件拥有者和组）**

```powershell
chown [-参数（-R是递归，修改目录时加上）][目标用户名][文件名]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123154023935.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

```powershell
chgrp [-参数（-R是递归，修改目录时加上）][目标用户组名][文件名]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123154743629.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
**命令三：umask（默认权限）**  
创建普通文件或目录是他们的默认的权限是下面这样的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123192500667.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
这些权限的默认值是由**umask**的值来决定的，可通过终端输入查看umask的值  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123192936504.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
**对于root用户，其umask值为0022，对于普通用户则为0002**  
umask一共有四个数组，第一位用于定义特殊权限，剩下的三位与权限有关。**对于目录，用户所能拥有的最大权限为777，对于文件则是目录的最大权限去掉执行权限，是666**，因为执行权限是目录所必须要有的，而对于普通文件则不必要。  
由于root的umask默认为022，如下，使用root创建目录时，其权限就是最大权限777去掉相应位置umask的权限，对于所属组和其他人则去掉写权限；而使用root创建文件时，也是如此，即为644

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123194058344.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
设定计算默认权限时，不应该直接将最大权限减去umask值，计算公式为`mask& ~umask`。比如目录的最大权限是777，对应则为"`111 111 111"`，umask值被设为2,对应为“000 000 010”,\~umask则为`“111 111 101`”,进行与操作，则为“`111 111 101”`，则为775，对应权限字符为`“rwx rwx r-x`”。再比如文件的最大权限为666，对应则为`“110 110 110”`，进行与操作则为“110 110 100”，对应的权限字符为`“rw- rw- r--`”  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123201036317.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

再比如将umask值设置0003，会是多少呢？**对于文件**：则为“`110 110 100”`，即为664，对于目录则为“111 111 100”，则为774  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123201335990.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

附：

> [is not in the sudoers file解决](https://blog.csdn.net/weixin_42489582/article/details/86736752)

#### E：粘滞位

（注意：设置粘滞位一般是给目录设置的）  
如下：test2这个文件夹隶属于root，但是普通用户却可以删除，这一点不可不谓危险  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123162704498.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)

可以通过**粘滞位**解决，只需对目标目录设置粘滞位

```powershell
chmod +t [目标目录]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123163204299.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
当一个目录被设置粘滞位时，只有由以下三种人删除

- 超级管理员
- 该目录所有者
- 该文件的所有者

# 补：

**1：批量删除当前目录下所有后缀名为“.c”的文件的ml**

- [ ]  `rm *.c`
- [ ]  `find .-name "*.c" -maxdepth 1 | xargs rm`
- [ ]  `find .-name "*.c" | xargs rm`

大部分人会错选`rm *.c`，但是实际答案是`find .-name "*.c" \-maxdepth 1 | xargs rm`，因为容易忽略**当前目录**。关于这个maxdepth本文已经做介绍。这里注意`xargs`这个命令，它的作用是将管道的命令行作为参数传递给后面的命令执行

比如在创建文件时我们会使用`touch a.c b.c e.c`这样的方式，配合`echo` 命令也可以这样写![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210228161319512.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116143912)  
所以 `find .-name "*.c" \-maxdepth 1 | xargs rm`命令可以解释为：利用find在当前目录下找到所有以“.c”结尾的文件，结果传递给后面的rm执行，删除

2.查看Linux系统资源的一些命令

1.  `top`查看cpu资源使用状态
2.  `netstat`查看网络状况
3.  `free`查看内存资源状态
4.  `df`查看磁盘分区状态