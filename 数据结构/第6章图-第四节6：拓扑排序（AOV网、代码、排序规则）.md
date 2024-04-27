 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：拓扑排序基本概念](#_4)
- - [（1）AOV网](#1AOV_6)
  - [（2）拓扑序列](#2_17)
- [二：拓扑排序](#_28)
- - [（1）拓扑排序](#1_30)
  - [（2）拓扑排序规则](#2_36)
- [三：拓扑排序代码实现](#_70)
- - [（1）准备工作](#1_71)
  - [（2）代码](#2_83)
  - [（3）代码分析](#3_143)

# 一：拓扑排序基本概念

## （1）AOV网

**AOV网\(Activity On Vertex network\)：如果从英文角度理解就是活动在顶点的网。它是一种以顶点表示活动，以边表示活动的先后次序且没有回路的有向图**

比如下图是一个电影制作的流程图，其中**某些活动的发生会受到其他活动是否发生或完成的限制**，比如在拍摄时，不可能也不能出现连场地、演员都没有的现象

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F134472a7a75942db930cef4fc48f394e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

## （2）拓扑序列

**拓扑序列：设 G G G\=\( V V V, E E E\)是一个具有 n n n个顶点的有向图， V V V中的顶点序列 v 1 v\_\{1\} v1​、 v 2 v\_\{2\} v2​、…、 v n v\_\{n\} vn​满足从顶点 v i v\_\{i\} vi​到 v j v\_\{j\} vj​有一条路径，且在顶点序列中顶点 v i v\_\{i\} vi​必须在顶点 v j v\_\{j\} vj​之前，那么我们称这样的顶点序列为一个拓扑序列**  
**拓扑排序并不唯一**，比如上面AOV网的拓扑排序可以有

- v 0 − v 1 − v 2 − v 3 − v 4 − v 5 − v 6 − v 7 − v 8 − v 9 − v 10 − v 11 − v 12 − v 13 − v 14 − v 15 − v 16 v\_\{0\}-v\_\{1\}-v\_\{2\}-v\_\{3\}-v\_\{4\}-v\_\{5\}-v\_\{6\}-v\_\{7\}-v\_\{8\}-v\_\{9\}-v\_\{10\}-v\_\{11\}-v\_\{12\}-v\_\{13\}-v\_\{14\}-v\_\{15\}-v\_\{16\} v0​−v1​−v2​−v3​−v4​−v5​−v6​−v7​−v8​−v9​−v10​−v11​−v12​−v13​−v14​−v15​−v16​

也可以有

- v 0 − v 1 − v 4 − v 3 − v 2 − v 7 − v 6 − v 5 − v 8 − v 10 − v 9 − v 12 − v 11 − v 14 − v 13 − v 15 − v 16 v\_\{0\}-v\_\{1\}-v\_\{4\}-v\_\{3\}-v\_\{2\}-v\_\{7\}-v\_\{6\}-v\_\{5\}-v\_\{8\}-v\_\{10\}-v\_\{9\}-v\_\{12\}-v\_\{11\}-v\_\{14\}-v\_\{13\}-v\_\{15\}-v\_\{16\} v0​−v1​−v4​−v3​−v2​−v7​−v6​−v5​−v8​−v10​−v9​−v12​−v11​−v14​−v13​−v15​−v16​

# 二：拓扑排序

## （1）拓扑排序

**拓扑排序：所谓拓扑排序，其实就是对一个有向图构造拓扑序列的过程。构造时有如下两种情况**

- 如果此网的顶点**全部被输出，说明他是一个不存在环的AOV网**
- 如果输出**顶点数目不全，哪怕少了一个，就说明该网存在环，并不是AOV网**

## （2）拓扑排序规则

- 注意：以下内容借鉴自这位大佬的文章[数据结构与算法—拓扑排序](https://blog.csdn.net/qq_40693171/article/details/100536278?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163801639416780255264377%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163801639416780255264377&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-100536278.first_rank_v2_pc_rank_v29&utm_term=%E5%B0%B1%E6%AF%94%E5%A6%82%E5%AD%A6%E4%B9%A0java%E7%B3%BB%E5%88%97+%E6%8B%93%E6%89%91%E6%8E%92%E5%BA%8F&spm=1018.2226.3001.4187)
- 作者：[Big sai](https://blog.csdn.net/qq_40693171/article/details/100536278?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163801639416780255264377%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163801639416780255264377&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-100536278.first_rank_v2_pc_rank_v29&utm_term=%E5%B0%B1%E6%AF%94%E5%A6%82%E5%AD%A6%E4%B9%A0java%E7%B3%BB%E5%88%97+%E6%8B%93%E6%89%91%E6%8E%92%E5%BA%8F&spm=1018.2226.3001.4187)

以如下图为例  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe8ac6af62d9049648abc09506578b291.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

---

**拓扑排序规则：简单来说就是每次删除入度为0的顶点然后输出。具体来说：**

- 从有向无环图中找到一个**没有前驱**的顶点输出
- 删除**以这个顶点为起点的边**（也即它发出的边要删除，这是为了找到下一个没有前驱的顶点）
- 重复以上步骤，**直至最后一个顶点被输出**，如果还有顶点未输出，则说明有环

**描述如下：**

- 删除1或2输出  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F59f0af9cce5c4675851f2dbbcd11a950.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

- 删除2或3以及对应的边  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa09079d5a7a44780a5acee785ff77491.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

- 删除3或4以及对应的边  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F469cc5fb13dd424ca7d2cc90f87c6699.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

- 重复  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F238866b53bd6456cacc4208d78dccd8c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

# 三：拓扑排序代码实现

## （1）准备工作

**确定采用的数据结构**：由于此过程需要删除顶点，因此为AOV网建立一个**邻接表**，又因为要频繁查找入度为0的顶点，所以在原来的结构中，需要**增加一个入度域`in`** 进行标识  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5e9749c2db4b4d24a7b466922f30f744.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)  
另外还要采用**栈**，用于存储处理过程中入度为0的顶点，**目的是为了避免每次查找时都要去遍历顶点表去寻找是否有入度为0的顶点**

以如下AOV网为例  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F44d02fe437aa42ea8b5b8a0a200069c6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)  
对应邻接表如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8619ed33da114e63b7da1caa52df3c9a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

## （2）代码

邻接表定义如下

```c
typedef struct ArcNode
{
            
            
	int adjvex;
	struct ArcNode* nextarc;
}ArcNode;

typedef struct VNode
{
            
            
	char data;
	int in;//顶点入度
	ArcNode* firstarc;
}VNode;

typedef struct
{
            
            
	VNode adjlist[MaxSize];
	int n,e;
}AGraph;
```

**拓扑排序代码如下**

```c
1	bool ToplogicalSort(AGraph G)
2	{
            
            
3		ArcNode* e;
4		int i,k,gettop;
5		int top=0;//栈指针下标
6		int count=0;//统计输出顶点的个数
7		int* stack;//建立一个栈存储入度为0的顶点
8		stack=(int*)malloc(G->n*sizeof(int));
9		for(int i=0;i<G->n;i++)
10			if(G->adjlist[i].in==0)
11				stack[++top]=i;//将入度为0的顶点入栈
12		while(top!=0)
13		{
            
            
14			gettop=stacl[top--];//出栈
15			printf("%c -> ",G->adjlist[gettop.data]);//打印此顶点
16			count++;//统计输出顶点数目
17			for(e=G->adjlist[gettop].firstarc;e;e=e->next)
18			{
            
            //遍历此顶点的边表
19				k=e->adjvex;
20				if(--G->adjlist[k].in==0)//将k号顶点邻接点入度减一
21					stack[++top]=k;//若为0则入栈，以便下次循环输出
22			}
23		}
24		if(count<G->n)//如果count小于顶点数目，说明存在环
25			return false;
26		else
27			return true;
28	}
```

## （3）代码分析

1：程序开始运行，第3\~7行均为变量定义

2：第8\~10行把入度为0的顶点下标都入栈。于是在上图中 v 0 v\_\{0\} v0​、 v 1 v\_\{1\} v1​、 v 3 v\_\{3\} v3​应该入栈

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F140ddb189f6147deb511915c7d9516fb.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

3：第12\~23行是一个while循环，当栈中有数据元素时一直循环

4：第14\~16行， v 3 v\_\{3\} v3​出栈，于是有gottop=3，打印，然后count++

5：第17\~22行，对 v 3 v\_\{3\} v3​顶点对应的边表进行遍历（下图灰色部分）。找到 v 2 v\_\{2\} v2​和 v 13 v\_\{13\} v13​，由于 v 3 v\_\{3\} v3​的删除，于是将他们的入度减少1

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F16e305cdae8f44f1823b523bbb5ed597.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa53c621038ab4aa6bc9e28e197518534.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

6：再次循环。此时处理的是顶点 v 1 v\_\{1\} v1​，经过一系列步骤，对 v 1 v\_\{1\} v1​到 v 2 v\_\{2\} v2​、 v 4 v\_\{4\} v4​和 v 5 v\_\{5\} v5​的边进行了遍历，并减少了入度。这里 v 2 v\_\{2\} v2​入度会变为0，于是在第20\~21行后， v 2 v\_\{2\} v2​会入栈（如下图所示）（**如果不这样做，那么对于新产生的这个入度为0的顶点 v 2 v\_\{2\} v2​，就必须要再次进行循环判断了，因此这属于典型的以空间换时间的手法**）

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fefad30755d3e41038f55618f2c8b84dc.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)  
7：剩余过程就是重复了

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff606b6c99d724435bbce8ff60ce46b36.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd1be8679b2c64220b07ddfb63409b077.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121583081)

8：最终拓扑排序结果为：3-1-2-6-0-4-5-8-7-12-9-10-13-11