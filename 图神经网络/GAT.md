# GAT

## 
$$
\begin{aligned}
& h_i^{(l+1)}=\sum_{j \in \mathcal{N}(i)} \alpha_{i, j} W^{(l)} h_j^{(l)} \\
& \alpha_{i j}^l=\operatorname{softmax}_i\left(e_{i j}^l\right) \\
& e_{i j}^l=\operatorname{LeakyReLU}\left(\vec{a}^T\left[W h_i \| W h_j\right]\right)
\end{aligned}
$$
**首先一个共享参数 W 的线性映射对于顶点的特征进行了增维，当然这是一种常见的特征增强（feature augment）方法**；

显然学习顶点 i,j 之间的相关性，就是通过可学习的参数 W 和映射 a(⋅) 完成的。**

## **Mask graph attention**

注意力机制的运算只在邻居顶点上进行

区别于**Global graph attention**，保留了图结构

