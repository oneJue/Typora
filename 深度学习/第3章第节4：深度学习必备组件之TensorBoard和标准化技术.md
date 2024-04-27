 

### 文章目录

- [一：TensorBoard](#TensorBoard_2)
- - [（1）TensorBoard介绍](#1TensorBoard_4)
  - [（2）Pytorch安装TensorBoard](#2PytorchTensorBoard_14)
  - [（3）TensorBoard使用](#3TensorBoard_23)
  - [（4）服务器tensorboard本地显示](#4tensorboard_55)
  - [（5）AutoDL等算力平台tensorboard使用](#5AutoDLtensorboard_80)
- [二：正则化](#_109)
- - [（1）为什么需要Normalization](#1Normalization_122)
  - [（2）Batch Normalization](#2Batch_Normalization_137)
  - [（3）Layer Normalization](#3Layer_Normalization_177)
  - [（4）Instance Normalization](#4Instance_Normalization_200)
  - [（5）Group Normalization](#5Group_Normalization_227)

# 一：TensorBoard

## （1）TensorBoard介绍

**TensorBoard：TenosrBoard是Google开发的一个机器学习可视化工具。其主要用于记录机器学习、深度学习训练过程，例如：**

- 记录**损失变化、准确率变化**等
- 记录**图片变化、语音变化、文本变化等**，例如在做GAN时，可以过一段时间记录一张生成的图片**绘制模型**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9182f8a981d041568a92092ce523fdcf.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （2）Pytorch安装TensorBoard

**Pytorch中并没有TensorBoard，它借助的是TensorFlow中的TensorBoard，所以我们最终要安装的是TensorBoardX，安装步骤分为如下两步**

```python
conda install tensorboard # 必须先安装tensorboard
conda install tensorboardX
```

## （3）TensorBoard使用

**TensorBoard使用：首先需要new一个`SummaryWriter`对象**

```python
from torch.utils.tensorboard import SummaryWriter
writer = SummaryWriter(log_dir='runs/mock_accuracy')
```

**当运行完该行代码后，可以看到当前目录下生成了一个`runs/mock_accuracy`文件夹，并且里面有`event`日志**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F01529914e3fb4289bbc5225cef0dd7f8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**这个`writer`中可以添加很多内容以供`TensorBoard`查看，例如图像、文本等。这里我们添加最基本也是最常用的准确率`accuracy`，因为在验证集上我们要验证模型的准确率。当然你也可以添加训练损失等，但这里我们只以`accuracy`为例进行演示**

**进入到你存放日志的文件夹之下，例如上图中的`runs`，然后输入如下命令**

```bash
tensorbaord --logdir = ./ 
```

**接着会显示如下信息**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5ede57ae6ca1441cb5db0ab3bbb3b927.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**将上面的地址复制到浏览器后，显示如下**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F10b91adaa44949c1a75a114753b1efb4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （4）服务器tensorboard本地显示

**很多时候我们的训练代码会在服务器上进行训练，这就导致本地无法查看，但其实我们可以将其端口转发至本地查看**

**如下图，使用XShell，依次点击【文件-属性-连接-SSH-隧道】，然后将源主机改为`127.0.0.1`即可，侦听端口和目标端口均设置为6006**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbba0418c3f284502b99eb300a27172d6.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**服务器上正常运行即可**

```bash
tensorboard --logdir = ./ # 日志文件夹
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F796dd8f582bb43a7bff5cc4f88f4ce68.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**然后在本地浏览器直接地址即可**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F1947261aadb44e77881565639c859a15.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （5）AutoDL等算力平台tensorboard使用

**很多算力平台其实本身已经把tensorboard给你配置好了，无需自信设置端口转发。这里以AutoDL算力平台为例，介绍一下如何使用**

大家可以点击下方链接查看，我感觉这个算力平台价格相当良心，1小时才1.5元，而且认证学生的话会更低，相当划算

- [AutoDL算力云官网](https://www.autodl.com/register?code=b68ce530-fd82-4b2a-81fa-a247c7dc017b)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F99bcc38dafc84ba69027127f1b40a557.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**言归正传，输入如下命令，结束默认的tensorboard进程，然后进入你的日志文件夹再执行命令**

```bash
ps -ef | grep tensorboard | awk '{print $2}' | xargs kill -9
tensorboard --port 6007 --logdir /path/to/your/tf-logs/direction
```

**然后点击AutoPannel即可查看**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8cec30237d1445deb1ba1504c60d0544.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feab387fea06f4f12a6f9e70d68144302.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

# 二：正则化

**深度学习=80\%数据+20\%模型**，对于输入数据，我们期望数据要**独立同分布**

- 独立：掷色子时，第一次丢的结果不会影响第二次的结果
- 同分布：第一次丢和第二次丢得到任意一面的概率是相同的

所以数据在输入到网络之前要进行**白化**，主要目的是

- 去除特征的相关性，即独立
- 使特征具有相同的均值和方差

## （1）为什么需要Normalization

Normalization主要要解决深度学习中的**ICS问题**。，它是指深度神经网络会涉及很多层的叠加，每一个的参数更新会导致上层的输入数据分布发生变化，通过层层叠加，高层的输入分布就会变得非常距离，**因此高层需要不断去重新适应底层的参数更新**

- **ICS问题**：每一层的输入作为一个分布看待，由于底层的参数随着训练更新，导致相同的输入分布得到的输出分布改变了。而机器学习中有个很重要的假设：IID独立同分布假设，就是假设训练数据和测试数据是满足相同分布的，这是通过训练数据获得的模型能够在测试集获得好的效果的一个基本保障。那么，细化到神经网络的每一层间，每轮训练时分布都是不一致，那么相对的训练效果就得不到保障

**Normalization主要有以下类型**：

- **Batch Normalization（批标准化）**：适用于大的batchsize，不适用于文本、语音这类变长数据，适用于图像数据
- **Layer Normalization（每层输出上标准化）**：适用于序列变长数据，RNN/Transformer
- **Instance Normalization（通道上作标准化）**：适用于GAN等
- **Group Normalization**（结合了Layer Normalization和Instance Normalization）

## （2）Batch Normalization

**方法如下**

- 均值
- 方差
- 标准化
- 利用参数 γ \\gamma γ和 β \\beta β进行更新并输出（仿射变换）

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8f4986811c4643f38d07828adfb0eb50.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F542966e7a097426d8696f4a5f5f8b234.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

**Batch Normalization的缺点有**

 -    对batchsize的大小敏感，如果选择太小的话就不能代表样本的真实分布了，如果太大的话硬件又吃不消
 -    Batch Normalization对**RNN不友好**，因为RNN的深度不是固定的

```python
import torch

# 前一层输出
# 图像数据，NHWC（数量、高、宽、通道数）
output  = torch.rand(size=(8, 224, 224, 16))
batch_normizaliton = torch.nn.BatchNorm2d(num_features=16)  # 输入通道数
# 注意输入维度应该为NCHW,所以要进行维度切换
norm_out = batch_normizaliton(output.permute(0, 3, 1, 2 ))
print(norm_out.shape)

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff429d62415fd4bcd813c9acefb7880dd.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （3）Layer Normalization

Layer Normlization会针对**网络中某一层的所有神经元**的输入按以下公式进行标准化

u l = 1 H ∑ i = 1 H α i l u\^\{l\} = \\frac\{1\}\{H\}\\mathop\{\}\\sum\_\{i=1\}\^\{H\}\\alpha\_\{i\}\^\{l\} ul\=H1​i\=1∑H​αil​

σ l = 1 H ∑ i = 1 H \( α i l − u l \) 2 \\sigma\^\{l\} = \\sqrt\{ \\frac\{1\}\{H\} \\mathop\{\}\\sum\_\{i=1\}\^\{H\}\(\\alpha\_\{i\}\^\{l\}-u\^\{l\}\)\^\{2\} \} σl\=H1​i\=1∑H​\(αil​−ul\)2 ​

```python
layer_normalization = torch.nn.LayerNorm([224, 224, 16])
norm_out2 = layer_normalization(output)
print(norm_out2.shape)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0a6d21b1015d4f62a3d82d7877be0c11.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （4）Instance Normalization

Batch Normalization注重对每个batch进行标准化，保证数据分布一致，但是在某些情形下，如果生成结果主要依赖于**单个实例**（例如GAN），那么这种这操作显然不合理。所以Instance Normlization会对单个通道进行标准化

y t i j k = x t i j k − u t i σ t i 2 + ϵ y\_\{tijk\} = \\frac\{x\_\{tijk\}-u\_\{ti\}\}\{\\sqrt\{ \\sigma\_\{ti\}\^\{2\}+ \\epsilon\}\} ytijk​\=σti2​+ϵ ​xtijk​−uti​​

u t i = 1 H W ∑ i = 1 W ∑ m = 1 H x t i l m u\_\{ti\} = \\frac\{1\}\{HW\}\\mathop\{\}\\sum\_\{i=1\}\^\{W\}\\mathop\{\}\\sum\_\{m=1\}\^\{H\}x\_\{tilm\} uti​\=HW1​i\=1∑W​m\=1∑H​xtilm​

σ t i 2 = 1 H W ∑ l = 1 W ∑ m = 1 H \( x t i l m − μ u t i \) 2 \\sigma\_\{ti\}\^\{2\}=\\frac\{1\}\{HW\}\\mathop\{\}\\sum\_\{l=1\}\^\{W\}\\mathop\{\}\\sum\_\{m=1\}\^\{H\}\(x\_\{tilm\}-\\mu\_\{uti\}\)\^\{2\} σti2​\=HW1​l\=1∑W​m\=1∑H​\(xtilm​−μuti​\)2

```python
instance_normlization = torch.nn.InstanceNorm2d(16)
# 注意输入维度应该为NCHW,所以要进行维度切换
norm_out3 = instance_normlization(output.permute(0, 3, 1, 2))
print(norm_out3.shape)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9f62f4109473414991ab7b5d26e0bd15.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)

## （5）Group Normalization

Group Normlization按通道方向分组，也即输入变为\[N, H, group\_number, C//group\_number\]极端情况下

 -    当`group_numer`\=1时，GN就是LN
 -    当`group_numer`\=通道数目时，GN就是IN

```python
group_normlization = torch.nn.GroupNorm(num_groups=4, num_channels=16)
norm_out4 = group_normlization(output.permute(0, 3, 1, 2))
print(norm_out4.shape)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff2931fe613b5400aa351da09fd7be195.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128769025)