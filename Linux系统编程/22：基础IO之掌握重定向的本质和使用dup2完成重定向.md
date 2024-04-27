 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242)

### 文章目录

- - [（1）总结](#1_5)
  - [（2）使用系统调用dup2完成重定向](#2dup2_8)

## （1）总结

**从讲文件描述符开始，我们就引入了重定向的概念。读到这里，大家应该能够明白，重定向产生的原因就是文件描述符在分配时趋向于数值小的，而在用户层，stdout这个文件指针指向的文件已经封装了，并且它的fd就是1，这是不能修改的，所以我们一上来关闭了1号文件，然后新创建了一个文件它的文件描述符就会分配为被1，同时此时写入时，像printf这类函数默认使用的输出流就是stdout，但是我们知道它的1指向的已经是我们新生成的那个文件了，所以这就重定向的本质**

## （2）使用系统调用dup2完成重定向

前面的那种关闭1号文件而完成重定向的操作显得有些不合理，因为标准输入，标准输出和标准错误作为默认的文件，不应该被关闭  
所以我们可以使用`dup2`完成重定向。`dup2`的函数原型如下

```cpp
int dup2(int oldfd,int newfd);
```

关于其参数中的`oldfd`和`newfd`不是特别好解释，大家只要会用即可  
举个例子，还是上面的要求，我们要把本来输出到屏幕上的的内容输出到一个新打开的文件（由于不使用关闭文件的方式，所以其fd=3），**那么也就是要将1的内容重定向到3，那么就输入`dup2(3,1)`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210328185230187.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232918)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032818540510.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116232918)