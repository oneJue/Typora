 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [Linux基本指令](#Linux_4)
- - [（1）pwd（显示所在目录）](#1pwd_5)
  - [（2）ls（列出文件或目录）](#2ls_24)
  - [（3）cd（改变目录）](#3cd_71)
  - [（4）touch（创建文件）](#4touch_101)
  - [（5）mkdir（创建目录）](#5mkdir_128)
  - [（6）rmdir和rm（删除）](#6rmdirrm_151)
  - [（7）man（查询）](#7man_168)
  - [（8）cp（复制）](#8cp_239)
  - [（9）mv（移动或改名）](#9mv_262)
  - [（9）cat（查看文件内容）](#9cat_281)
  - [（10）more（逐行查看文件内容）](#10more_309)
  - [（11）less（弃more用less）](#11lessmoreless_326)
  - [（12）head和tail（查看文件头或尾局部内容）](#12headtail_350)
  - [（13）时间相关](#13_368)
  - - [A：显示](#A_369)
    - [B：设定](#B_387)
    - [C：时间戳](#C_388)
  - [（14）cal（日历）](#14cal_393)
  - [（15）find（查找）](#15find_402)
  - [（16）grep（行过滤）](#16grep_449)
  - [（17）zip/unzip（解压和压缩）](#17zipunzip_470)
  - [（18）tar（在线解压）](#18tar_487)
  - [（19）bc（计算器）](#19bc_506)
  - [（20）uname -r（查看linux内核版本）](#20uname_rlinux_513)

# Linux基本指令

## （1）pwd（显示所在目录）

**功能：**显示用户当前所在目录  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120164314174.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**补充：**

1.  Linux——**一切皆文件**，与Windows操作系统不同，Windows系统下的文件目录 是以**“\\”**分割的，而Liunx则以 **“/”** 分割。而网页url也是以 "/"进行分割的，这是因为网页的服务器端使用的操作系统是“Linux”，那么反应在前端也正是这样子的。  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120195302214.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

2.  Linux和Windows都是多用户操作系统。购买云服务器后，默认用户名是root，**root是系统中唯一的超级管理员，掌握最高权限，其权限等同于操作系统**，不像Windows一样，正因为root的权限太高，所以日常中如果用root登录，那么某些操作就具有危险性，极有可能危害系统。所以我们一般要创建一个普通用户以供正常使用，登录时使用普通用户登录即可。而且要注意普通用户的密码和root的密码绝对不能一样。  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120195229815.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

3.  root登录和普通用户登录的区别。  
    普通用户登录时（比如我的用户名是zhangxing），目录会默认在**/home/zhangxing**。所有普通用户的全部保存在/home下  
    root登录时，**root在直接就在根目录下**，root的这个目录和home相当于是平级了。  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121142024848.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

---

## （2）ls（列出文件或目录）

**功能：**若为目录，列出该目录内所有的子目录和文件；若为文件，列出文件名及其他信息。  
**语法：**

```powershell
ls 【选项】 【目录或文件】
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120195840190.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项参数：**

| 参数 | 功能 |
| --- | --- |
| \-a | 列出目录下的所有文件，包含以"."开头的隐含文件 |
| \-d | 将目录像文件一样显示，而不是显示其下的文件 |
| \-i | 输出文件的i节点的索引信息 |
| \-k | 以k字节的形式表示文件的大小 |
| \-l | 列出文件的详细信息 |
| \-n | 用数字的UID，GID代替名称 |
| \-F | 在每个文件名后面附上一个字符，用来说明该文件的类型（“\*”表示可执行的普通文件；“/”表示目录；“\@”表示符号链接；"="表示套接字） |
| \-r | 对目录进行反向排序 |
| \-t | 以时间进行排序 |
| \-s | 在文件名后面输出该文件大小 |
| \-R | 列出所有子目录下的而文件 |
| \-1 | 一行只输出一个文件 |

1.  关于ls -a  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120202855693.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120204051518.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)
2.  关于文件  
    在Windows系统下，创建一个文件，而不进行编辑，称这种文件为空文件，同时侧边信息也显示其字节为0。但是**空文件也是占用磁盘空间的**。因为**文件=文件内容+文件属性**，而文件属性也属于一种属性（比如说文件的类别），所以它也就会被保存下来因而占用空间
3.  Windows与Linux保存文件的区别  
    与Windows不同，**Linux的文件类型与文件后缀名没有直接关系**，但是我们在建立文件时，为了符号人的习惯，因此加上后缀名  
    那么Linux是如何区分文件类型的呢？如下，使用ls-a列出文件信息  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120215225558.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

| 缩写 | 表示类型 |
| --- | --- |
| d | 目录文件 |
| l | 符号链接（可以理解Windows中的快捷方式） |
| s | 套接字文件 |
| b | 块设备文件；二进制文件 |
| c | 字符设备文件 |
| p | 命名管道文件 |
| \- | 普通文件 |

---

## （3）cd（改变目录）

**前言：**Linux

1.  文件夹结构为**树形结构**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120204937518.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120205133678.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

2.  相对路径和绝对路径  
    如有test.c文件，想要运行它  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012020590637.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

**功能：**改变工作目录  
**语法：**

```powershell
cd 【目录名】
```

```powershell
cd /  返回根目录
cd ~  返回用户目录
cd -  在两个目录之间来回切换
cd ..  返回上级目录
cd /home/exercise/test/  绝对路径
```

---

## （4）touch（创建文件）

**功能：**可以更改文件信息或创造新的文件  
**语法：**

```powershell
touch 【选项】 文件名
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120212136915.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项：**

| 选项 | 功能 |
| --- | --- |
| \-a | 仅仅更改Access时间 |
| \-c | 不建立任何文档 |
| \-d | 使用指定日期时间，而非此刻时间 |
| \-f | 负责解决BSD版本touch指令的兼容性问题 |
| \-m | 仅仅更改Modify时间 |
| \-r | 将指定文档或目录的日期时间全部设成参考文档或目录的日期时间 |

**补充：**

1.  “stat”命令用于查看文件的信息，Linux文件信息中有三个时间分别为**Access,Modify,Change**时间  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120212908465.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

---

## （5）mkdir（创建目录）

**功能：**在当前目录下创建目录  
**语法：**

```powershell
mkdir [选项] [目录名]
```

**选项：**

| 选项 | 功能 |
| --- | --- |
| \-p | 创建一串目录 |
| **补充：** |  |

 1.     “tree”命令可以以**树状**的形式显示目录的层级结构  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120220455869.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
    注意如果没有tree命令，请安装

```powershell
yum install -y tree
```

---

## （6）rmdir和rm（删除）

（remdir只能删除空目录，大多数情况主要使用rm）  
**语法：**

```powershell
rm [选项][目录或文件名]
```

**选项：**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120221750626.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210120222019903.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**特别说明：**从删库到跑路

```powershell
rm -rf /  慎用！！！（Linux是没有回收站的）
```

---

## （7）man（查询）

**功能：**使用联机手册，查询相关命令  
**语法：**

```powershell
man 【选项】 【需要查询的命令】

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121144912622.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

| 选项 | 功能 |
| --- | --- |
| \-k | 根据关键字搜索 |
| num\(1-8\) | 在第num章节中查找 |
| \-a | 将所有章节都显示出来 |

**汉化：**汉化时确保使用root账号的登录，步骤如下。

> 原文链接[Linux man命令中文汉化](https://blog.csdn.net/weixin_30471561/article/details/95298568?utm_medium=distribute.pc_relevant_download.none-task-blog-BlogCommendFromBaidu-1.nonecase&depth_1-utm_source=distribute.pc_relevant_download.none-task-blog-BlogCommendFromBaidu-1.nonecas)

 1.     在线获取汉化包

```powershell
wget  https://src.fedoraproject.org/repo/pkgs/man-pages-zh-CN/manpages-zh-1.5.1.tar.gz/13275fd039de8788b15151c896150bc4/manpages-zh-1.5.1.tar.gz
```

 2.     解压安装

```powershell
tar xf manpages-zh-1.5.1.tar.gz
```

```powershell
cd manpages-zh-1.5.1/
```

```powershell
./configure --disable-zhtw  --prefix=/usr/local/zhman
```

```powershell
make && make install
```

 3.     不要覆盖man命令，有可能会使用英文版，使用cman

```powershell
cd ~
```

```powershell
echo "alias cman='man -M /usr/local/zhman/share/man/zh_CN' " >>.bash_profile
```

```powershell
source .bash_profile
```

 4.     试一下

```powershell
cman ls
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012114564192.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

---

## （8）cp（复制）

**功能：**复制文件或目录  
**语法:**

```powershell
cp [选项] [src] [des]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121155128125.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项**

| 选项 | 功能 |
| --- | --- |
| \-f | 强行复制，无论文件或目录是否存在 |
| \-i | 覆盖文件前先询问用户 |
| \-r | 递归处理目录 |
| **补充** |  |

1.  关于递归拷贝：Linux拷贝时默认拷贝的是文件，拷贝目录时要目录下的子目录及其文件全部拷贝，就要使用参数“r”  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121155732137.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)
2.  和Windows一样，Linux中相同目录内不准出现同名文件，所以拷贝时要进行重命名  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121160238487.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

---

## （9）mv（移动或改名）

**语法：**

```powershell
mv [选项] [源文件或目录] [目标文件或目录]
```

**功能：**mv发挥移动还是改名，由其命令中第二个参数而定，**如果第二个参数为文件时，将会改名**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121161610448.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
当第二个参数为已经存在的目录时，**将会把源文件或目录移动到目标目录中去**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121161745501.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项**

| 选项 | 功能 |
| --- | --- |
| \-f | 如果目标文件存在，不会询问直接覆盖 |
| \-i | 如果目标文件存在，先进行询问 |

---

## （9）cat（查看文件内容）

**echo命令：**echo命令可以将内容写到文件当中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210123144452943.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

**语法：**

```powershell
cat [选项] [文件]
```

**功能：**查看文件内容  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012116283826.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项**

| 选项 | 功能 |
| --- | --- |
| \-b | 对非空输出行进行编号 |
| \-n | 对输出的所有航进行编号 |
| \-s | 不输出多行空行 |
| **补充** |  |

 1.     关于tac和cat  
    为了方便讲解，使用下面的shell脚本生成多行内容的文件

```powershell
count=0; while [ $count -le 10 ]; do echo "hello $count"; let count++; done > file.txt
```

cat输出时是**从首行到尾行**，且具有-n参数，而tac则是**反向输出，并且不具有-n参数**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012116445069.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （10）more（逐行查看文件内容）

cat命令的缺陷在于，对于多行文件，它会一次性全部显示完成，所以不方便查看特定行的内容  
**语法：**

```powershell
more [选项][文件]
```

**功能：**逐行查看内容。对于多行内容每显示满一屏，他会自动停止，**按下回车则会显示下一屏**。同时可以通过“/1000”这样的方式**跳转到指定行**。需要注意的是：**more只能往下跳转，不能向上跳转**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121184409519.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**选项**

| 选项 | 功能 |
| --- | --- |
| \-n | 对所有输出行进行编号 |
| q | 退出more |

---

## （11）less（弃more用less）

**less和more的区别**：

 1.     less可以**向后翻也可以向前翻**，而more只能向后翻
 2.     使用less就可以使用**“pageup，pagedown”**这些按钮实现翻页操作
 3.     less的搜索功能更加强大  
    **语法：**

```powershell
less [选项][文件]
```

**选项**

| 选项 | 功能 |
| --- | --- |
| \-i | 搜索时，忽略大小写 |
| \-N | 显示每行行号 |
| \-字符串 | 向下搜索“字符串” |
| \?字符串 | 向上搜索“字符串” |
| n | 重复前一个搜索（与/或？有关） |
| N | 反向重复前一个搜索（与/或？有关） |
| q | 退出 |

---

## （12）head和tail（查看文件头或尾局部内容）

**语法：**

```powershell
head -[查看多少行][文件名]
tail -[查看多少行][文件名]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121190021157.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121190025144.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**补充**

**

1.  对于一个具有1000行的代码需要查看它的第500-510行该怎样做？可以这样：先保存其前510行于一个文件中，再从这个文件中提取后10行。  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121190732215.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)
2.  管道  
    可以发现上述做法很麻烦，所以可以简化成下面这样  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121190946137.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （13）时间相关

### A：显示

**

**语法：**

```powershell
date+格式控制符
```

**格式控制符**

| 格式控制符 | 含义 |
| --- | --- |
| \%H | 小时 |
| \%M | 分钟 |
| \%S | 秒 |
| **\%X** | 等于\%H:\%M:\%S |
| \%d | 日 |
| \%m | 月份 |
| \%Y | 完整年份 |
| **\%F** | 等于\%Y-\%m-\%d |
| ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121194925693.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245) |  |

### B：设定

### C：时间戳

**时间->时间戳**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121195833680.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
**时间戳->时间**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210121195954481.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （14）cal（日历）

**语法**

```powershell
cal -参数
```

**功能**  
日历功能  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122152210952.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （15）find（查找）

**功能**：按参数查看想要的文件  
**语法**：

```powershell
find [path][option][-print]

```

- path：查找的目录
- print：参数
- print：将查找结果打印

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122155516168.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

**常用参数**：

- `-type`：按文件类型查找，可以有`f`，`d`，`b`，`c`，`l`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210207155342663.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

- `-name/-iname`：按文件名称查找，-iname不区分大小写  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210207155829877.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

- `-user`：按照文件所属用户查找  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210207155942657.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

- `-size`：按照文件大小查找  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210207160236349.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

- `-maxdepth n`：最多搜索n-1级别

- `-mindepth n`：从第n级开始查找

- `-empty`：查找空文件

- `-delete`：对找到文件进行删除  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210207161024200.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

> [find命令详解](<https://blog.csdn.net/l_liangkk/article/details/81294260?ops_request_misc=%7B%22request%5Fid%22%3A%22161130163416780266228068%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=161130163416780266228068&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~hot_rank-1-81294260.pc_search_result_cache&utm_term=linux find命令&spm=1018.2226.3001.4187>)

**补充**

1.  关于**which命令**。像ls，pwd这些在linux中也是文件，它等同于Windows中的快捷方式，使用which命令可以查找到这些可执行命令的路径  
    ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122155745398.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)
2.  关于**whereis命令**。可以帮助我们查找安装位置

---

## （16）grep（行过滤）

**功能：**在某文件中，搜索满足条件的所在行  
**语法：**

```powershell
grep [选项][搜索的字符串][文件]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122161045633.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
该功能最常用于日志文件，日志文件中在错误出一半会有error一类的提示符，所以可以很快进行定位  
**选项：**

| 选项 | 功能 |
| --- | --- |
| \-i | 忽略大小写 |
| \-n | 顺便输出行号 |
| \-v | 反选 |
| \-E | 查看以某字母开始的行 |

- 在Linux系统中，为了找到try\_grep中含有以字母a开头的行的，命令是`grep -E ^a try_grep`

---

## （17）zip/unzip（解压和压缩）

**功能：**解压和压缩功能  
**语法：**

```powershell
压缩：zip [生成的压缩文件和后缀名.zip][要打包的文件或目录]
解压：unzip [压缩文件名]（默认会解压到当前目录下）
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122163305771.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122163526262.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)  
解压到指定目录下

```powershell
unzip [压缩文件名] -d [目录名]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122163723308.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （18）tar（在线解压）

**功能：**除具备基本的解压和压缩功能，其还具有其他高级功能  
**语法：**tar的参数较多，一般需要组合使用，以下是出场率最高的几个组合

```powershell
//压缩
tar -czf [生成的压缩文件和后缀名.tgz] [需要打包的目录或文件]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122165011940.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

```powershell
//解压
tar -xzf [需要进行解压的压缩文件] -C [解压到的目录]
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122165249495.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

```powershell
//查看压缩文件
```

## （19）bc（计算器）

**语法：**

```powershell
直接输入bc
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122220003923.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

## （20）uname -r（查看linux内核版本）

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210122220112559.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F112894245)

---