 

### 文章目录

- [一：文件系统树](#_2)
- [二：当前工作目录-pwd](#pwd_11)
- [三：列出目录内容-ls](#ls_19)
- [四：更改当前工作目录-cd](#cd_25)
- - [（1）绝对路径](#1_28)
  - [（2）相对路径](#2_33)

# 一：文件系统树

提起Windows大家一定很熟悉，但是大家有没有观察过Windows的文件层级结构呢。很明显，**是一个树形结构**  
**其实许多类UNIX系统都是以分层目录结构的方式组织文件的**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304134503585.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)

- 需要注意，Windows操作系统，每个存储设备都有一个独立的文件系统树（C,D,E盘），**而在Linux中，无论多少存储设备都会被算为一个系统树，存储设备会被挂载到系统树的不同位置**
- 使用`tree`命令可以查看文件系统树  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304134726812.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)

# 二：当前工作目录-pwd

当前工作目录说白了就是当前你现在所在的目录。比如说我现在就在`/root/home/zhangxing`这个目录之下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304135304320.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304135348302.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
使用命令`pwd`可以打印我现在所在的工作目录  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304135437570.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
**第一次登录系统时，当前工作目录就是该普通用于所在的目录**，该普通用户只能在这里进行它的操作

# 三：列出目录内容-ls

使用`ls`命令可以列出当前工作目录的文件和目录  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030413562692.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)

- ls的功能非常强大，不止这么简单，更多细节后续文章再讨论

# 四：更改当前工作目录-cd

使用`cd`命令可以更改当前工作目录，只需要输入cd，然后再输入工作目录的路径名即可，关于这个路径名需要好好讨论一下，**路径名主要有两种：绝对路径和相对路径**

## （1）绝对路径

类比于Windows，绝对路径就是你在Windows中选定一个文件，然后选择该文件的地址，这个地址就是这个文件的绝对路径  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304140223397.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
如下，当前处于根目录，然后使用绝对路径的方式进入用户目录\(zhangxing\)下的debug文件夹中  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304140759871.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)

## （2）相对路径

相对路径着重体现在相对二字。举一个生活中的例子，我身高1.75米，姚明相对我来说很高，而我相对潘长江来说比较高。

所以对于文件或工作路径也是一样的，**为了体现这种相对位置，我们通常用`.`表示当前目录，`..`表示当前目录的上一目录**。

所以接着绝对路径的那个例子，我现在已经进入了debug文件夹下，然后，我想返回到上一个目录，也就是zhangxing目录下，可以这样做  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304141434909.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
那么zhangxing目录下有其他目录，还是按照相对路径的方式，需要进入Desktop下可以这样做  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210304141541770.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F114366502)  
总结及补充如下

```bash
cd /  返回根目录
cd ~  返回用户目录
cd -  在两个目录之间来回切换
cd ..  返回上级目录
cd /home/exercise/test/  绝对路径
```