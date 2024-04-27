 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

串的匹配是一个非常重要的话题，我们在Word中经常使用的**搜索**功能所反映的就是串的匹配问题，相应的算法也是层出不穷，各有优缺点，本节主要涉及两种算法：**朴素算法**和**KMP算法**

在讲解之前，有几个术语需要掌握

- **主串**
- **模式串**
- **子串**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa3e6f93173cc480a80f96227124d5a9d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
**字符串模式匹配：在主串中找到与模式串相同的子串，并返回其所在位置**

---

### 文章目录

- [一：朴素的模式匹配算法](#_17)
- [二：KMP算法](#KMP_80)
- - [（1）暴力匹配的缺点](#1_84)
  - [（2）最长相同前缀和后缀](#2_93)
  - [（3）究竟怎么回溯](#3_103)
  - [（3）next数组](#3next_111)
  - [（4）求解next数组](#4next_135)
  - - [A：next\[0\]=-1](#Anext01_166)
    - [B：next\[j\]=k](#Bnextjk_176)
    - [C：k=next\[k\]](#Cknextk_191)
  - [（5）KMP算法代码](#5KMP_212)

# 一：朴素的模式匹配算法

**字符串模式匹配：是一种纯暴力算法。将主串中所有长度为 $m$的子串依次与模式串比较，直到找到一个完全匹配的子串，或所有子串都不匹配为止**

![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3bee166316cf41bf9d88a7dff382efca.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

**具体描述：以下面的主串S和模式串T为例。如果当前子串匹配失败：主串指针指向下一个子串的第一个位置，模式串指针回溯至模式串的第一个位置**

- **主串S：“ $goodgoogle$”**
- **模式串T：“ $google$”**

1：主串S从第一位开始，S和T前三位字母都匹配成功，**但是S第四个字母是 $d$而T的则是 $g$,所以第一位匹配失败**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdfb7cdda58c24f6ca2a8deb3c4effc56.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
2：主串S从第二位开始，主串S首字母是 o o o，**但是需要匹配的T的首字母是 g$gg$，第二位匹配失败**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F616260f126094826934bfd9738ba8c56.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

3：主串从第三位开始，主串S首字母是$o$，**但是需要匹配的T的首字母是 $g$，第三位匹配失败**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd34759a61ddc4746a33c74d50ac6210a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
4：主串从第四位开始，主串S首字母是$d$，**但是需要匹配的T的首字母是 g g g，第四位匹配失败**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd75c25bf116c4c91b8836d1b681af480.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
5：主串从第五位开始，**6个字母全部匹配成功**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F89f853c582314e07b0412b2d239532cc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

**代码**

```c
int Index(String S,String T,int pos)
{
            
            
	//字符串从1位置开始存储
	int i=1;//i指针指向主串S
	int j=1;//j指针指向模式串T
	int k=i;//k用来记录下次主串回溯的位置
	while(i<=S.len && j<=T.len)
	{
            
            
		if(S[i]==T[j])
		{
            
            
			++i;
			++j;
		}
		else//不匹配
		{
            
            
			j=1;//模式串从第一个位置继续开始匹配
			i=++k;//主串从k+1位置开始比较
		}
	}
	if(j>T.len)//如果是模式串扫描完了，说明匹配到了，那么此时k记录的就是模式串起始位置
		return k;
	else
		return 0;
}
```

**效率分析**

- **最好情况**：时间复杂度为$O(1)$
- **平均情况**：平均查找次数为 $2\frac{n+m}{2}2$，时间复杂度为 $O(n+m)$
- **最坏情况**：每次不成功的匹配都发生在T的最后一个字符，时间复杂度为 $O((n-m+1)*m)$

# 二：KMP算法

为了方便介绍，将主串和目标串（模式串）设置为下面这样（只是为了说明情况，虽然主串中是没有目标串的）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323202857145.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

## （1）暴力匹配的缺点

朴素模式匹配算法的最大问题就是**回溯太过于频繁**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323202940448.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
下面这种情况中，如果让你选择回溯的话，你肯定会这样干（一次移动多个）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323203122141.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
**为什么？因为主串的第2,3位都不是A，在第一位不匹配的情况下，回溯到2,3位也自然是无济于事的，既然这样还不如直接从第4位开始比较**

所以KMP算法的核心就是：**不要回溯到无效的地方，让其回溯到有效的位置**

## （2）最长相同前缀和后缀

什么是一个字符串的**最长相同前缀和后缀**？关于这个问题，其实很多数据结构的书解释的都太过复杂了，甚至会拿出一套公式把你劝退

要想知道什么是最长相同前缀和后缀，首先得明白什么是字符串的前缀和后缀,看完下面这个图相信你就不难理解了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323204109949.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

最长相同前缀和后缀你也应该能找出来了吧  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323204209304.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
你可能会奇怪不应该是最下面的那个吗？其实这里就要说明一点：**最长相同前后缀不能是字符串本身**。因为如果你认为最长相同前后缀可以是字符串本身的话，那么这就是一个恒成立的命题了，也就不具有任何讨论的意义了

## （3）究竟怎么回溯

前面讲过，KMP算法的核心问题就是处理如何有效回溯的问题。  
继续观察，你有没有发现它回溯的位置有些特点？没错！**就是最长公共前后缀重合的地方，也就是说让最长相同前缀对齐至后缀**。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323203122141.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

**请问目标串发生不匹配时的索引是多少？是5对吧，然后咋们回溯的时候请问目标串的哪个位置（或者说索引）与主串的不匹配位置（也就是5）“对齐了”？很明显是目标串的2号位置（字母C），那你有没有发现，这个2恰好就是目标串发生不匹配位置前的字符串的最长前缀和后缀的长度！**

## （3）next数组

经过前面的叙述，我们可以意识到，这种回溯是有规律的，并不是凭空臆想的，我们再举几个例子

如下目标串的索引为3号的位置发生不匹配  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323205642147.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
其3号位置前的字符串的最长相同前后缀的长度是1，所以就应该让目标串索引为1的位置与该不匹配处“对齐”  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021032320575654.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
再来，如下目标串的索引为5号的位置发生不匹配  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323205859556.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
其5号位置前的字符串的最长相同前后缀的长度是2，所以就应该让目标串索引为2的位置与该不匹配处“对齐”  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323205934792.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
其实从本质上来说匹配是根本“不需要”主串，**因为每个位置发生不匹配时，总有一个值能确定其应该回溯的位置。这个值就是不匹配位置前面的字符串的最长相同前后缀的长度**

**我们把目标串每个位置发生不匹配时，目标串应该回溯的位置给记录下来，形成一个数组，这个数组就是next数组,next\[i\]=j所表示的意思是索引为`i`的位置发生不匹配，就让其回溯的`j`的位置继续匹配**

next数组的计算方法其实前面已经讲完了，如下为一个目标串的next数组，**需要注意的是由于第一个位置也就是next\[0\]前面没有串，因此要记为-1**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323210510280.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
| A |B | C| A |B |C|M|N  

|–|–|-- |–|–|–|–|–|–|  
|next\[0\] | next\[1\] |next\[2\] | next\[3\] | next\[4\] | next\[5\]|next\[6\] |next\[7\] |  
| \-1 |0 | 0| 0 | 1 | 2|3 |0 |


现在根据next数组，我们就能确定目标串的回溯位置了，比如下面假设目标串和主串在索引为4的位置发生不匹配且next\[4\]=1，所以进行下面操作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323211144950.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

## （4）求解next数组

KMP算法其思想接受起来并不是特别困难，但是这个算法为什么劝退了很多人呢？**其实就是在于其求解next数组的代码，虽然很短，但是理解起来有难度**

```c
void getnext(string target_str,int next[])
{
            
            
	int j=0,k=-1;
	next[0]=-1;
	while(j<target_str.size())
	{
            
            
		if(k==-1 || str[j]==str[k])
		{
            
            
			j++;
			k++;
			next[j]=k;
		}
		else
		{
            
            
			k=next[k];
		}
	}
}
```

上述代码中主要有以下三个理解难点

- `next[0]=-1`
- `next[j]=k`
- `k=next[k]`

### A：next\[0\]=-1

首先是`next[0]=-1`，如下当目标串的第一个位置（索引为0的位置）发生不匹配时，应该让目标串索引为-1的位置与当前不匹配的地方对齐比较  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323213346641.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
这里你肯定会有一个疑问：怎么可能有-1的位置，这明显越界”。先不管这些，这种情况下我们首先能够确定的操作是：**目标串的第一个位置与主串的下一个位置进行比较** ，这里我先截取KMP算法的一小段（不要担心你肯定看得懂）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323213527766.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
**上面的`j=next[j]`很明就是回溯，因此此时执行结果为`-1=next[0]`，也就是`j=-1`了，然后重新回到循环，继续判断，此时`j=-1`，进入if内，然后`i++`，`j++`。所以`j++`会等于0，同时`i++`就正好实现了让目标串的第一位与主串的下一位比较**

这也就是为什么要设置成`next[0]=-1`

### B：next\[j\]=k

对于这句代码，大家的疑问可能是：**为什么下标为j的元素的值是k**

这个语句处于一个if判断内，根据上面的代码自然是`k==-1`或`str[j]=str[k`\]的时候执行

好的现在，我们先考虑`k=-1`，当`k=-1`时，总会有`k++`，使得k变为`0`，这样的话`next[j]=0`，表明前面没有相同的前后缀

那么一旦`k`不是-1，满足这个if的唯一条件就是`str[j]==str[k]`了。**那么`str[j]=str[k]`就表示此时的位置可以算作相同的前后缀了，我们所做的就是判断下一对了**

比如虾米，`j`和`k`他们之前都各自在B的位置，由于之前的字符判断相同\(`str[j]==str[k]`\)，才来到了现在位置，现在他们要判断此刻对应的位置是否也可以将其算入相同的前后缀  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323220058887.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
非常幸运是相同的，于是`j++，k++，next[j]=3`

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323220520976.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

### C：k=next\[k\]

KMP算法最难理解的部分就是在这里

如下，`j`和`k`准备比较下一个  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323221150320.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

很遗憾这个位置由于不相同，因此无法使其成为长度为4的相同前后缀，那么根据代码只能执行`k=next[k]`了  
这样画比较难受，我把前缀直接拿到下面来  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323221323906.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
**有没有似曾相识的感觉？没错，好像是主串和目标串，但是这不是原来的那个两个串，而是后缀作为了主串，前缀作为了目标串，好的，现在前缀（目标串）的下标为3的位置发生了不匹配，该怎么做？自然而然，当然是让前缀（目标串）下标为1的位置（因为next\[3\]=1已经记录）与该不匹配处重新比较**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323221650245.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
好的，重新组装回去，毕竟这不是真的主串和目标串  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323221818442.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)  
现在if判断满足，对应位置相同，那么继续`i++`，`j++`，于是next\[9\]=2，此时next数组全部赋值完成。

**仔细想一想，下面的这种情况中已经不可能有ABAB,ABAC相等的情况存在了，因为一旦相同，那么就有next\[9\]=4，也就是说C之前不可能有长度为4的相同的前后缀，但是没有这么长的或许也有短的呀，可以看咋们上面的那个AB=AB不就是一个短的情况吗，所以这也就是为什么有k=next\[k\]这样的回溯的情况。其实就是在进行前缀和后缀在比较，只不过一旦遇到不相等，前人栽树后人乘凉罢了！**

如果你能理解最后的那句谚语，那么这个算法你应该掌握的八九不离十了，这里也可以看出这个算法有多么的牛逼，简直即使巧妙之际，不得不佩服发明[KMP算法](https://baike.baidu.com/item/kmp%E7%AE%97%E6%B3%95/10951804?fromtitle=KMP&fromid=10158450&fr=aladdin)的这三位大牛的聪明才智啊  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210323221323906.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121324942)

## （5）KMP算法代码

我觉得不用我再解释了吧

```c
int KMP(string main_str,string target_str)
{
            
            
	int next[MaxSize],i=0,j=0;
	getnext(target_str,next);
	while(i<main_str.size() && j<target_str.size())
	{
            
            
		if(j==-1 || main_str[i]==target_str[j])
		{
            
            
			i++;
			j++;
		}
		else
		{
            
            
			j=next[j];
		}
	}
	if(j>=target_str.size())
		return i-target_str.size();//返回目标串在主串的第一个字符的位置
	else
		return -1;//找不到返回-1

}
```