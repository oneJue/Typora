 

### 文章目录

- [一：什么是Pytorch](#Pytorch_8)
- [二：Pytorch优势](#Pytorch_27)
- [三：Pytorch三大核心概念](#Pytorch_39)
- - [（1）tensor（张量）](#1tensor_43)
  - [（2）autograd（自动微分-变量）](#2autograd_49)
  - [（3）nn.Module（神经网络）](#3nnModule_56)
- [四：tensor和机器学习、深度学习关系](#tensor_75)

```python
import torch
```

# 一：什么是Pytorch

**Pytorch：首先，torch是一个有大量机器学习算法支持的科学计算框架，其诞生已有十年之久，具体来说torch是一个经典的对多维数据进行操作的张量库（tensor），在机器学习和其他数学密集型应用有广泛应用**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9b2b57cf4f024d49aeb2c845c40bf6ee.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128241730)

**与Tensorflow的静态计算图不同，Pytorch的计算图是动态的，可以根据计算需要实时改变计算图，但由于torch采用的语言是Lua，比较小众，所以推广十分困难**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc54e7c57ecbf46468e16bbae277e7e50.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128241730)

**Pytorch是torch的Python版本，是由Facebook开源的神经网络框架，专门针对GPU加速的DNN编程。作为经典机器学习库 Torch 的端口，PyTorch 为 Python 语言使用者提供了舒适的写代码选择。PyTorch既可以看做加入了GPU支持的Numpy，同时也可以看成一个拥有自动求导功能的强大的深度神经网络**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe1aea6be94d44ed4b15d6d9fe6d3fbc4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128241730)

# 二：Pytorch优势

**Pytorch：与TensorFlow等主流深度学习框架相比，Pytorch之所以可以与之分庭抗礼，是因为它有如下优点**

- **很简洁**：Pytorch的设计追求最少的封装，尽量避免重复造轮子，API设计的相当简洁一致。**Pytorch的设计遵循tensor→variable\(autograd\)→nn.Module 三个由低到高的抽象层次，分别代表高维数组（张量）、自动求导（变量）和神经网络（层/模块）**，而且这三个抽象之间联系紧密，可以同时进行修改和操作
- **速度快**：Pytorch的灵活性不以速度为代价，在许多评测中，Pytorch的速度表现胜过TensorFlow和Keras等框架
- **易调试**：由于Pytorch采用**动态图**，所以你可以像调试普通Python代码一样调试Pytorch
- **代码美**：Pytorch是所有的框架中**面向对象设计**的最优雅的一个。PyTorch的面向对象的接口设计来源于Torch，而Torch的接口设计以灵活易用而著称
- **社区活**：：Pytorch提供了完整的文档，循序渐进的指南，作者亲自维护的论坛 供用户交流和求教问题。Facebook 人工智能研究院对PyTorch提供了强力支持，作为当今排名前三的深度学习研究机构，FAIR的支持足以确保PyTorch获得持续的开发更新，不至于像许多由个人开发的框架那样昙花一现

# 三：Pytorch三大核心概念

**Pytorch三大核心概念：前面说够，。Pytorch的设计遵循tensor→variable\(autograd\)→nn.Module 三个由低到高的抽象层次，分别代表高维数组（张量）、自动求导（变量）和神经网络（层/模块）**

## （1）tensor（张量）

**tensor（张量）：tensor是Pytorch中一种统一的数据组织方式，它认为标量、向量和矩阵都是tensor，只不过纬度不同**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F28bd0712004b4f55bee4e836c527c3ca.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128241730)

## （2）autograd（自动微分-变量）

**autograd（自动微分）：我们知道，神经网络是需要依靠反向传播求解梯度后来更新参数，求梯度是一个极其繁琐且容易出错的过程，而Pytorch可以帮助我们自动完成梯度运算，这边是Pytorch的autograd（自动微分）机制**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa4b15ba0b61c4aaa98ac9bd5ef09f8f8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128241730)

## （3）nn.Module（神经网络）

**nn.Module（神经网络）：nn.Module是专门为神经网络设计的模块化接口，构建于autograd之上，可以用来定义和运行神经网络，是搭建神经网络时需要继承的父类**

```python
import torch

class MyNet(torch.nn.Module):
	def __init__(self, ):
		super(MyNet, self).__init__()
		#  层次结构
	def forward(self, ):
		#  前向计算

# 实例化
mynet = MyNet()
	
```

# 四：tensor和机器学习、深度学习关系

**不管是机器学习还是深度学习，面对一个特定问题，我们的目的是求解出一个模型，该模型可以对未知问题进行预测。求解模型本质就是在求解最佳的模型参数**

Y = W X + b Y=WX+b Y\=WX+b

- X X X：数据或者样本
- Y Y Y：模型的输出结果
- W , b W,b W,b：模型参数，在Pytorch称之为变量

**tensor（张量）是Pytorc中一种统一的数据组织方式，无论是数据、输出或者参数在Pytorch中统一会转化为tensor进行处理**