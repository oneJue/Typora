端对端训练

平移不变性

顺序不变性

非线性变换

层级结构（特征一层一层抽取，一层比一层更抽象，更高级）

**GCN 是谱图卷积（spectral graph convolution） 的局部一阶近似（localized first-order approximation）**



### GAT

$$
\alpha_{i j}=\operatorname{softmax}\left(\sigma\left(\vec{a}^T\left[W \overrightarrow{h_i} \| W \overrightarrow{h_j}\right]\right)\right)
$$

### 梯度消失

在深度神经网络中，由于每一层的输出都是前一层输出的函数，因此，当我们计算梯度时，需要将每一层的梯度相乘，这就涉及到了链式法则。如果这些梯度的绝对值都小于1，那么随着层数的增加，整个梯度的值会以指数速度衰减，最终接近于。这就是所谓的梯度消失问题]。

梯度消失问题的主要后果是，靠近输入层的参数很难得到有效的更新。因为当梯度接近于0时，参数的更新步长也会接近于0，这意味着参数几乎不会改变。这就导致了网络无法有效地学习到数据的表示，特别是在处理长序列或深层网络结构时

### Dropout


Dropout的基本思想是在训练过程中随机忽略（即“丢弃”）神经网络中的一部分神经元

“暂时移除"是指在训练神经网络的每个批次中，我们随机选择一部分神经元并将其"关闭”。这意味着这些神经元在当前批次的前向传播和反向传播过程中都不会有任何贡献，就好像它们被从网络中移除了一样。然而，这种移除只是暂时的，因为在下一个批次中，这些神经元可能会被"打开"，而其他的神经元可能会被"关闭"。这就是Dropout中"暂时移除"神经元的含义。这种随机性使得网络在训练过程中更加强健，因为它不能依赖于任何一个特定的神经元。这也是Dropout能有效防止过拟合的原因之一。 

使用注意事项： 在训练阶段，我们随机丢弃一部分神经元，但在测试阶段，我们使用所有神经元。为了保证测试阶段神经元的输出期望与训练阶段一致，我们需要对训练阶段未被dropout的神经元的输出进行缩放，即乘以一个1/(1 - dropout rate)的比例。

### 1 

$$
H^{(l+1)}=\sigma\left(\tilde{D}^{-\frac{1}{2}} \tilde{A} \tilde{D}^{-\frac{1}{2}} H^{(l)} W^{(l)}\right) .
$$



GCN 消息传递机制版本:
$$
\begin{aligned}
& h_i^{(l+1)}=\sigma\left(b^{(l)}+\sum_{j \in \mathcal{N}(i)} \frac{1}{c_{j i}} h_j^{(l)} W^{(l)}\right) \\
& c_{j i}=\sqrt{|\mathcal{N}(j)|} \sqrt{|\mathcal{N}(i)|}
\end{aligned}
$$
$\mathrm{GCN}$ 矩阵版本:
$$
\begin{aligned}
& H=\hat{A} X W \\
& \hat{A}=\tilde{D}^{-\frac{1}{2}} \tilde{A} \tilde{D}^{-\frac{1}{2}}
\end{aligned}
$$

GAT 消息传递机制版本:
$$
\begin{aligned}
& h_i^{(l+1)}=\sum_{j \in \mathcal{N}(i)} \alpha_{i, j} W^{(l)} h_j^{(l)} \\
& \alpha_{i j}^l=\operatorname{softmax}_i\left(e_{i j}^l\right) \\
& e_{i j}^l=\operatorname{LeakyReLU}\left(\vec{a}^T\left[W h_i \| W h_j\right]\right)
\end{aligned}
$$
邻接矩阵 

A

 或者其归一化版本 

A^

 被视为**低通滤波器**。这是因为当它们作用于一个图信号（例如，节点特征向量）时，它们倾向于平滑该信号。具体来说，当你将 

A

 或 

A^

 与一个图信号相乘时，每个节点的新特征是其邻居特征的平均值（或加权平均值）。这种平均化过程倾向于保留邻近节点之间的相似性（即低频成分），而消除邻近节点之间的差异（即高频成分）。

或 

L^

 与一个图信号相乘时，每个节点的新特征是其特征与其邻居特征的差的总和。这种差异化过程倾向于强调邻近节点之间的差异（即高频成分），而消除邻近节点之间的相似性（即低频成分）