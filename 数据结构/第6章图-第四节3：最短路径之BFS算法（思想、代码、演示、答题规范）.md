 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：BFS算法基本思想](#BFS_27)
- [二：BFS算法代码](#BFS_34)
- [三：反思](#_117)

**最短路径shortestpath\)：主要有以下两类最短路径问题**

****单源最短路径问题：一个顶点到其他顶点最短路径****

- [迪杰斯特拉算法\(dijkstra\)（带权图、无权图）](https://blog.csdn.net/qq_39183034/article/details/121531499)\-点击跳转
- BFS算法\(无权图\)-本节讲解

****各顶点间最短路径问题：也即每一对顶点间最短路径****

- [弗洛伊德算法](https://blog.csdn.net/qq_39183034/article/details/121568352)\-点击跳转

最短路径在通信、交通等领域有重要应用  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2b5261ba5eea494eabac9db551bc7d29.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_19%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121530132)

---

# 一：BFS算法基本思想

- 注意：无权图可以视为权值均为1的带权图

**BFS算法：从某一个顶点 A A A开始找到它的邻接点，对应最短路径为1，接着再通过邻接点找到邻接点的邻接点，于是最短路径增加1，以此类推**![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc621984d4eea4e57932b31482bd0b4ce.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121530132)

# 二：BFS算法代码

BFS算法可以解决很多很多问题（在LeetCode上都有涉及），比如

- [LeetCode 111：二叉树最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree)
- [LeetCode752：打开转盘锁](https://leetcode-cn.com/problems/open-the-lock)

算法的基本框架如下

```c
// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
            
            
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
            
            
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            
            
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj())
                if (x not in visited) {
            
            
                    q.offer(x);
                    visited.add(x);
                }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}

```

---

回到正题，对于单源最短路径问题，BFS算法中会涉及如下三个数组

 -    **`d[]`数组**：其中d\[ i i i\]表示从顶点 u u u到顶点 i i i的最短路径数值
 -    **`path[]`数组**：假如起点为 a a a， a a a到 e e e的最短路径为 a a a\-> c c c\-> f f f\-> e e e，那么就有`path[e]=f`,`path[f]=c`,`path[c]=a`,`path[a]=-1`
 -    **`visited[]`数组**:visited\[i\]=true表示顶点 i i i已被访问

```c
void BFS(Graph G,int u)
{
            
            

	for(int i=0;i<G.vexnum;++i)//初始化数组
	{
            
            
		d[i]=MAX;//一个很大的数，开始肯定没有路径
		path[i]=-1;//记录的路径
	}

	d[u]=0;
	visited[u]=True;
	EnQueue(Q,u);
	while(!isEmpty(Q))//BFS算法主体
	{
            
            
		DeQueue(Q,u);
		for(w=firstNeighbor(G,u);w>=0;w=nextNeighbor(G,u,w))
		{
            
            
			if(!visited[w])//w为u的尚未被访问的邻接点
			{
            
            
				d[w]=d[u]+1;//路径长度+1
				path[w]=u;//从u到w
				visited[w]=True;
				EnQueue(Q,w);
			}
		}
	}
}
```

# 三：反思

BFS算法我们在二叉树的层次遍历中就已经接触过了，我认为相较于图来说，二叉树最容易说明问题的本质

在BFS算法中有一个非常大特点就是`while`循环套一个`for`循环：其实 **`while`循环控制一层一层往下走，`for`循环控制从左到右每一层遍历**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9cda312da242446dacbbedaa45635a5e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121530132)

- 图片来源：【labuladong】

其实大家一定要明白：**图的BFS算法无非就是把结构抽象为了一个图，使用的还是二叉树层级遍历那一套流程罢了**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6914bb91d34f4b5e842390106e05285e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121530132)

- BFS算法会生成一个高度最小的生成树