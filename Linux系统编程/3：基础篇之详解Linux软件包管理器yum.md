 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）什么是软件包](#1_4)
  - - [A：软件包](#A_5)
    - [B：注意事项](#B_8)
    - [C：yum基本使用](#Cyum_11)
  - [（2）安装rzsz](#2rzsz_19)

## （1）什么是软件包

### A：软件包

- 区别Windows，在Linux下安装软件，第一种方法是下载程序源代码，然后进行编译，就得到了程序；第二种方法是rpm包安装。但是这两种方法都不推荐使用
- 我们推荐使用**yum\(Yellow dog updater,Modified\)**安装。 为了解决这样的麻烦，一些人将软件提前编译好并做成软件包（类似于Windows上的安装程序.exe）放置于服务器上，通过**包管理器**（yum正是一种包管理器）获取到软件包直接进行安装。**软件包和包管理器类似于“App”和“App store”的关系**。**yum具有查找，下载，安装和卸载的功能**

### B：注意事项

- 软件包名称为：主版本号-次版本号-源程序发行号-软件包发行号-主机平台-CPU架构
- 使用yum向服务器获取软件时，不止是把这个软件包下载下来，同时也会获取这个软件包依赖的相关信息，比如说版本号（`yum makecache`）

### C：yum基本使用

- `sudo yum install -y [软件名]//yum安装软件基本语法（-y表示无需确认）`
- `sudo yum remove[软件名]//yum卸载软件`
- `sudo yum list//列出软件`
- `yum list updates//列出所有可更新的安装包`
- `yum list installed//列出已经安装的安装包`

## （2）安装rzsz

**作用**：该工具可以用于Windows机器和远端Linux及其通过xshell传输文件  
**安装**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210124155815514.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144487)

**使用**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210124160021295.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144487)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210124160427191.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116144487)