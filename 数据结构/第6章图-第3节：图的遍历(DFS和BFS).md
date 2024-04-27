 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：图的深度优先遍历\(DFS\)](#DFS_5)
- - [（1）回溯算法和DFS](#1DFS_6)
  - - [A：回溯算法的本质](#A_24)
    - [B：回溯算法的框架](#B_31)
    - [C：全排列](#C_69)
  - [（2）图的DFS](#2DFS_131)
  - - [A：DFS思想](#ADFS_132)
    - [B：动画演示](#B_195)
    - [C：代码](#C_205)
- [二：图的广度优先遍历\(BFS\)](#BFS_248)

# 一：图的深度优先遍历\(DFS\)

## （1）回溯算法和DFS

图的深度优先遍历其本质就是回溯算法，所以这里我们先介绍回溯算法

**原创声明**  
本人在学习回溯算法时也感觉比较困惑，但是有幸看到一本非常好的算法书籍，也算是解决了我很多疑惑，我发现有些东西不是我智商不够，而是缺乏训练，尤其是有目的，有逻辑的训练。  
本文皆是我在阅读它的书后所做的一些整理，发表一下自己的看法。如果有兴趣的小伙伴可以移步

> [labuladong的算法小抄](https://labuladong.gitbook.io/algo/)

下面的讲解以[LeetCode 46：全排列](https://leetcode-cn.com/problems/permutations/)这道题为主

### A：回溯算法的本质

回溯算法本质是一个**暴力穷举**的过程，你会发现涉及回溯算法的题目都有一个共同特点那就是：**列出所有满足的情况**  
而且做得多了，你也会有种感觉，就是每个能用回溯算法解决的题目总能画出一个二叉树来，“全排列”就是这样一道题目，其对应的二叉树如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408095236757.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

**在回溯算法中，我们称这样的树为决策树，解决一个回溯问题，其实就是一个决策树的遍历过程**

### B：回溯算法的框架

遍历整个决策树时，你只需要思考三个问题：

- **路径**：你已经做出的选择
- **选择列表**：也就是你当前可以做的选择
- **结束条件**：到达决策树底层，无法再做选择的条件

**想要完成全排列，这三个位置的数字是不能够重复的。当你站在根节点上时，你的选择列表可以是1、2和3；假如你选择了2，也就是到达了红色的结点，那么现在的路径中就要加入2，它记录了你已经做出的选择，此时你的选择列表变成了1或3；当你再次做出选择时，比如选择了3，那么此时路径即为2-3，然后选择列表就只剩下了1，最后将1选择完，到达了树的底层，此时决策也已经结束，路径中的2-3-1就是满足条件的一种情况**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408095559289.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
因此，这类问题在思考时，是可以按照一种笼统的框架进行分析的

**下面result通常设为一个全局变量，用于返回结果，前面我们就说过，回溯算法本质就是在遍历决策树，所以那个backtrack其实就是在实现这样的需求，下面的for循环是在做出选择，然后再进行backtrack递归。在做选择时你的选择一定是合理的选择，就拿全排列而言，做选择时一定要选择那些在选择列表中的元素，处于路径中的元素已经选择过了，不能再进行选择**

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return

    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

上面框架中，有一点没有说到，那就是“撤销选择”。那么为什么必须要这一步呢？  
**大家可以想一想，你的一次选择结束了，你肯定要返回到当时进入递归时的状态，然后进行另外的选择。如果不撤销，你只会得到一个结果，就是一直遍历左子树，最后只会找到一条路径**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408100428365.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

好的至此，基本的问题就讲清楚了，我相信大家肯定还有诸多疑惑，但是没有关系，做完全排列你就会读回溯有了新的理解

### C：全排列

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408101404133.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

首先我们需要仔细观察函数的**返回值和形参**等（这里我使用的C++，因为它处理数组，字符串特别方便），这里形参是一个vector的引用，也就是一个数组，那么**它肯定可以作为选择列表，返回值则是一个二维数组，二维数组中每一个元素保存的是一个正确的返回结果**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408101551871.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

**既然返回的是二维数组，而二维数组中的每一项又都是一个vector，所以咋们的路径也一定是一个vector**，每次达到结束条件之后，把一个路径作为二维数组的一项添加进去。

于是在全局中定义二维数组，在函数内部定义路径  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408101849295.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
好的下一步就是回溯算法核心：**遍历决策树**  
**遍历一个决策树的递归函数backtrack**，其形参分别是路径`track`和选择列表**nums**，  
我们说过此函数内部就是**做选择，递归，撤销选择**循环过程。而做选择时候在遍历选择列表，于是我们可以写成这样  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408102441447.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
但是这样对吗，肯定是有问题的。前面我们说过，**做选择要求的是做出正确的选择**，而全排列要求数字全部不重复，所在做出选择之前，我们要进行判断，要是当前的选择已经出现过了那么就直接跳过。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408102907196.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
最后也是最为重要的，那就是一定要有结束条件，这里结束条件就是选够三个数字之后就可以结束了，选择够三个数字，此时的track就可以作为ret的一项被添加进去了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040810354879.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
好的代码如下，这里还需要强调C++的一个问题：形参最好写成引用，不然会引发很大拷贝，空间代价太高了

```c
class Solution {
            
            
public:
    vector<vector<int>> ret;//返回

    vector<vector<int>> permute(vector<int>& nums)
    {
            
            
        vector<int> track;//路径
        backtrack(nums,track);//递归

        return ret;
    }

    void backtrack(vector<int>& nums,vector<int> track)
    {
            
            
        if(track.size()==nums.size())//达到结束条件，一个全排列生成
        {
            
            
            ret.push_back(track);//把一种结果添加进去
            return;

        }

        for(int i=0;i<nums.size();i++)
        {
            
            
            vector<int>::iterator it;//找一下nums[i]是否已经有了，有了的话直接接下一个
            it=find(track.begin(),track.end(),nums[i]);
            if(it!=track.end())
                continue;

            track.push_back(nums[i]);//做出选择
            backtrack(nums,track);//遍历决策树
            track.pop_back();//撤销选择
        }
    }
};
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210408103823625.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

## （2）图的DFS

### A：DFS思想

**图的DFS算法：从图中某个顶点 v v v出发，访问此顶点，然后从 v v v的未被访问的邻接点中挑选一个再进行DFS，直到图中所有和 v v v有路径的顶点都被访问到**

- **对于非连通图**：只需要对它的连通分量分别进行DFS即可

---

如下有一张图，图的遍历就是指：**如何从顶点A开始\(其他顶点也可以\)走完图中所有顶点**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F64f0d00c251f41f7ab4a908dbb732b0c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_17%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)  
首先从顶点A开始走，先做标记，在A的前面两条路：分别是B和F，**这里我们制定一条规则：在没有碰到重复顶点的情况下，始终向顶点（从顶点角度看）右面走**，于是走到了顶点B

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe4c34f470c1e4a59b98d2e5b00845eb5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

来到顶点B又有三个选择，我们选择C

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa408f120e61c4aa0a9df2d141cb44e42.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

如果一直按照这样的走法，会走到顶点F  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8b089709fe76484b8bffc80d895c7e86.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

此时如果继续选择向右行走的话，你会发现来到了顶点A，但是顶点A在第一次访问时就已经做了标记，根据我们的规则，标记过的就不再访问了，**所以我们再进入A判断失败后，继续返回到顶点F**

不能选择最右面的，那么就选择次右面的，所以此次选择顶点G

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F830d2b325a944dcda98021d95867456a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

来到G后，继续选择H  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2f583be208c84b31aa59b7e554e879a9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

面对H，你会发现此时的D和E也已经被访问完毕了

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F13c3bd097aee4966a0d369c853ef9d1c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

走到这里，遍历仍然没有完成，因为像结点 I I I还没有访问到。于是我们在H处返回到G，G的三个顶点也访问过了，于是继续退回到F，继续退回到E（顶点H已经被G访问了），继续退回到D，对于D来说，H和G都访问完了，只有一个I没有访问，于是访问后记录。最后继续返回，直至返回到A，遍历结束

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8f5aabcac4fc48d28b79d2ca2fb60f64.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

从上至下你会发现：**图的DFS其实就是一棵树的前序遍历**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc9b103e611f54cefb4128a3111c3716e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

---

### B：动画演示

图的深度优先遍历\(DFS\)演示

### C：代码

这里采用邻接表结构

```c
typedef struct ArcNode//边表
{
            
            
	int adjvex;//存储该点在顶点表中的下标
	struct ArcNode* nextarc;
	int info;//对于网，要加上权值信息
}ArcNode;

typedef struct//顶点表结点
{
            
            
	char data;//顶点
	ArcNode* firstarc;//指向边表第一个结点
}VNode;
typedef struct
{
            
            
	VNode adjlist[MaxSize];//顶点表数组
	int n,e;//顶点数和边数
}

```

```c
int Vist[];//标记
void DFS(int v,AGraph* G)
{
            
            
	Vist[v]=1;//每进入一次递归，都把当前结点标记为已经访问
	ArcNode* q=G->adjlist[v].firstarc;//拿到它的第一个邻接点
	if(q!=NULL)//存在
	{
            
            
		if(Vist[q->adjv]==0)//如果没有访问
			DFS(q->adjv,G);//访问
		q=q->next;
	}

}
```

# 二：图的广度优先遍历\(BFS\)

**图的BFS算法：类似于树的层次遍历，拿到一个顶点后，就把与之相邻接点的顶点都入队列，然后从队列出顶点重复上述操作，直至队列为空。相比于DFS，BFS在拿到一个顶点后，要尽最大能力访问完该顶点的所有邻接点**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8bf3e1769dbc46528cb3869bcd79b46e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121516262)

动画演示如下

图的广度优先遍历\(BFS\)

代码如下

```c
int visit[maxSize]={
            
            0};
void BFS(AGraph* G,int b,int visit[maxSize])
{
            
            
	ArcNode* p;//用于辅助进队列
	int queue[maxSize],front=0,rear=0;//队列初始化
	int j;
	
	访问操作
	
	visit[v]=1;
	rear=(rear+1)%maxSize;
	queue[rear]=v;//进队列
	while(front!=rear)
	{
            
            
		front=(front+1)%maxSize;
		j=queue[front];
		p=G->adjlist[j].firstarc;
		while(p!=NULL)
		{
            
            
			if(visit[p->adjv]==0)
			访问操作
			visit[p->adjv]=1;
			rear=(rear+1)%maxSize;
			queue[rear]=p->adjv;
			p=p->next;
		}
	}
}
```