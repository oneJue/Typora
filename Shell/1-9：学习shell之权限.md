 

### 文章目录

- [前言：](#_1)
- [一：所有者，组成员和其他人](#_17)
- [二：读取，写入和执行](#_39)
- - [（1）chmod-更改文件权限](#1chmod_60)
  - - [A：八进制数字表示法](#A_63)
    - [B：符号表示法](#B_70)
  - [（2）umask-设置默认权限](#2umask_86)
- [三：更改身份](#_102)
- - [（1）su-以其他用户和组ID的身份运行shell](#1suIDshell_103)
  - [（2）sudo-提升权限执行命令](#2sudo_109)
  - [（3）chown-更改文件所有者和所属群组](#3chown_121)

# 前言：

**Linux不止是多任务操作系统，更关键的他还是多用户操作系统，这意味着同一时间内可以有多个用户使用同一台计算机**  
如果计算机连接到一个网络或者互联网中，远程用户可以通过ssh（云服务器就是这样）登录并操作这台计算机。  
用户一多，就有很多出现一些错乱，比如说一个用户访问到了另一个用户的文件，所以用户管理就显得格外重要

**本章设计命令如下**

- `id`：显示用户身份标识
- `chmod`：修改权限
- `umask`：设置默认权限（粘滞位）
- `su`：以另一个身份运行shell
- `sudo`：以另一个用户的身份运行命令
- `chown`：更改文件所有者
- `chgrp`：更改文件所属组
- `passwd`：更改用户密码

# 一：所有者，组成员和其他人

Linux把用户分为了如下三类，假如针对一个文件

- user：文件的创建者
- group：文件所在组的其他成员
- other：外人

如果使用cat命令查看`/etc/shadow`，会发现出现访问拒绝的提示  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311150532100.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
这是因为该文件里保存了所有用户密码等相关信息，作为普通用户是无法访问这个文件的。

> 在UNIX安全模型中，一个用户可以拥有自己创建的文件的控制权，同时这个用户又属于某一个群组（当然也可以属于更多的群组），该群组有一个或者多个用户组成，组中用户对文件的权限是由文件的创建者授予的

使用id命令可以获得用户身份标识信息，输出结果如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311150935481.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

在创建用户账户时，用户将分配一个称为`user ID`或`uid`的号码，同时会分配一个有效组`ID`或`gid`

> 用户账户定义在`/etc/passwd`中，用户组定义在文件`/etc/group`中，在创建用户账户和群组时，这些文件随着文件`/etc/shadow`的变动而修改，`/etc/shadow`中保存了用户的密码信息

# 二：读取，写入和执行

前面我们使用cat命令查看`/etc/shadow`时，我们的角色就是一个外人，所以没有读取权限。而且之前的`ls \-l`命令列出的长列表中，有关信息也揭示了这一点

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311152119707.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

- 第二个矩形方框框住的区域“什么都没有”，解释了对于外人来说没有读取，写入和执行操作

而`/etc/passwd`文件普通用户是可以查看的，它又是另外一种情形  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031115225930.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

- 可以发现针对外人具有读权限

> 上图中前10个字符表示的意思如下  
> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311152413918.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

**r，w，x三个的意思分别如下**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311152634618.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311152653852.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

**所以对于/etc/passwd文件的权限可以解释为：对于文件的创建者root来说没有读权限，但是可以写入和执行，而组成员和其他人则相反只能读取**

## （1）chmod-更改文件权限

chmod可以更改文件或目录的权限，但是需要注意的是只有文件创建者和超级用户才有权利更改权限  
更改文件权限有两种方式：**八进制数字表示法**和**符号表示法**

### A：八进制数字表示法

**每个八进制数字可以唯一对应一个三位的二进制数字**。对应关系如下，简单来说就是对应位置是1表示有权限，对应位置为0表示没这个权限  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311193814263.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
随意这种对应关系熟记后，可以分别为用户，组成员，其他人快速设置权限  
比如下面我要把test.c这个文件的所有权限全部更改为`rw-`，那么根据上述描述这种权限模式对应的八进制是6，故可以设置如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311194448151.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

### B：符号表示法

上述八进制的表示方法其实是最为快捷的方法，但是初学者在前期对应起来还是有点麻烦的。使用符号表示法，理解起来非常简单  
**简单来说，符号表示法就是通过特定字符对应特定用户，来为用户增加或减少权限**

对应特定用户的符号如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311194759443.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
给定好用户好之后，如果我想要去掉这个用户的读权限，那么就在其后面跟上`-r`，依次类推  
如下例  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311195157854.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

关于目录的权限在这里要统一说明一下：

- 目录读权限：是指是否能列出该目录下的文件
- 目录的写权限：是指是否能在该目录下创建文件
- 目录的执行权限：**就是指是否可以cd这个目录。由于目录的读写权限都是建立在执行权限上的，所以一旦执行权限被关闭，那么其读写权限也同样会被关闭**

## （2）umask-设置默认权限

如下，我一次性创建10个文件和5个文件夹，**其中值得注意的一点就是同类型文件创建时他们的权限都是一样的**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311200348782.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
**所以`umask`命令控制着创建文件或目录时给文件的默认权限**

关于这个`umask`不是特别好理解，在命令行中输入`umask`，可以查看此时的umask值，采用的是八进制表示法

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031120061232.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
**这个umask码就决定着默认权限，它的解释是这样的：由于是八进制，只看其后三位，比如说上图中是0022，只看022,按照之前的八进制表示方法它每一位数字对应一种用户，所以如果用二进制，022对应的user就是`000`，对应的group就是`010`，对应的other就是`010`，文件是没有执行权限的，所以原始最高权限为“`rw- rw- rw-`”，然后对应位置与二进制相比较，只要是出现1的位置全部去掉这个权限，0的位置保留，所以这也是为什么文件创建时默认权限是`rw- r-- r--`**

而对于目录来说最高权限为“`rwx rwx rwx`”，由于umsk为“`000 010 010`”，所以目录创建时默认权限为`rwx r-x r-x`

一般情况下umsk值不需要你来设置，但是一些高安全级别条件除外  
比如我的目的是创建出的文件的默认权限是“`r-- \-w- \-w-`”，问你umask值应该设置为多少？这里由于文件的最高权限是“`rw- rw- rw-”`，所以umask应该设置为“`010 100 100`”，也即是`0244`  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210311202401141.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

# 三：更改身份

## （1）su-以其他用户和组ID的身份运行shell

su的意思就是直接切换成其他用户  
比如现在的用户是deepin，我要切换到zhangxing可以这样输入  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312121634539.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
注意如果输入`su \-`，就会切换超级用户  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021031212185981.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

## （2）sudo-提升权限执行命令

sudo你可以理解为暂时提升你的用户权限执行某些命令，用法就是输入命令的时候在前面加上`sudo`，比如之前说到过的`/etc/shadow`文件，如果是普通用户是无法查看的，而使用这种方式短暂提升权限，则可以查看  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312122254746.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
**需要注意以下几点**

- 使用sudo时输入的不是超级用户的密码，而是普通用户的密码
- **su与sudo的重要区别在于sudo不需要启动一个新的shell，也就是不需要加载另一个用户的运行环境**
- 大家在Windows中时常需要以管理员身份启动某个引用，其实和这里我们将的有异曲同工之妙  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312122802973.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)

其实之所以要存在sudo是因为：很多用户喜欢“偷懒”，为了屏蔽掉那些烦人的权限不足的通知，经常就会以超级用户方式操作系统，这样就会使一些病毒有了入侵计算机的机会。Linux的安全性就和Windows一样低了，所以sudo的存在可以让用户在不切换超级用户的情况下短时间提高自己的权限等级，以此来完成某些必要操作，而不至于一直大开超级用户权限。

## （3）chown-更改文件所有者和所属群组

可以看出该命令其实有两个功能：一是更改文件所有者，二是更改文件所属群组。到底要发挥哪种功能，其实就看其如何搭配，搭配的方式如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312135214198.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
更改文件所有者为deepin  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312135327392.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
再比如更改文件所有者为zhangxing，文件所属组为root  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312135454493.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
还有，文件所有者不要变，只更改文件所属组为deepin  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312135615596.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)  
最后，把文件所有者改为root，文件所属组跟随root  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210312135739647.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114666535)