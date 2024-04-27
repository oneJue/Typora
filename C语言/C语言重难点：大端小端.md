 

### 文章目录

- [一：大端和小端](#_25)
- [二：经典问题](#_31)

我们知道，一个整形数据在内存中是连续排列的，它会占用内存连续的多个字节的空间，比如`int a=-10`，就会占用四个字节的空间  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fccc9f6da2a9243838f6380fc29441725.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)

```cpp
int a=-10;
原码：1000 0000 0000 0000 0000 0000 0000 1010
反码：1111 1111 1111 1111 1111 1111 1111 0101
补码：1111 1111 1111 1111 1111 1111 1111 0110
```

**因此对于-10，它对应的进制为`0xfffffff6`，既然整形占用4个字节，因此二进制的每8位（16进制每2位）分别存放内存中的一个字节中**，也即`ff`,`ff`,`ff`,`f6`（左侧为数据高位，右侧为数据低位）

运行程序后，查看内存状态，该变量在内存中状态为  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F01a40985d3c24364b0f1528c2a802924.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
因此这四部分是按照“**数据低位存放在内存低地址，数据高位存放在内存高地址**”来分布的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fed83ac2aa5234c6ebab5a4b872c46eb3.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
那么是否可以按照“**数据低位存放在内存高地址，数据高位存放在内存低地址**”来分布呢？答案是可以的，他们分别对应小端存储和大端存储

至于为什么有这样的问题，其实这是数据存储的问题，因为早期硬件厂商很多，每个人都有自己的标准，都认为自己的标准是最合理的，所以产生了很多分歧，就像吃香蕉一样，从头剥皮和从尾剥皮都是没有问题的。**当然这种分歧并不严重，只要约定好存取的规则，怎么存就怎么取，那么数据依然是正确无误的**

# 一：大端和小端

**大端（存储）模式（小小小），是指数据的低位保存在内存的高地址中，而数据的高位，保存在内存的低地中**  
**小端（存储）模式（大大大），是指数据的低位保存在内存的低地址中，而数据的高位,，保存在内存的高地址中**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513164550648.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)

# 二：经典问题

**1：如何判断当前机器的字节序**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513165051167.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
**2：下面的程序输出的是什么（64位操作系统）**

```cpp
#include <stdio.h>

struct task
{
            
            
	uint16_t id;//2个字节
	uint32_t value;//4个字节
	uint64_t timestamp;//8个字节

};


int main()
{
            
            
	struct task tas = {
            
            };
	uint64_t a = 0x00010001;
	memcpy(&tas, &a, sizeof(uint64_t));
	printf("%11u,%11u,%11u", tas.id, tas.value, tas.timestamp);

}

```

根据内存对齐的原则，id，value和timestamp所组成的结构体为16个字节![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513165512751.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)

接着对结构体进行初始化，全部为0  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513165741962.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)

变量a的存储布局  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513165833337.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
memcpy函数用法如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513165935840.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
也就是从a的位置开始，向后复制16个字节  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210513170434316.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116756301)  
故结果为1 0 0