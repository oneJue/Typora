 

### 文章目录

- [一：cifar10数据集介绍](#cifar10_2)
- [二：代码](#_104)
- - [（1）数据加载脚本编写](#1_106)
  - [（2）模型搭建](#2_219)
  - - [①：VGG](#VGG_221)
    - [②：ResNet](#ResNet_424)
    - [③：MobileNetV1](#MobileNetV1_508)
    - [④：InceptionNet](#InceptionNet_579)
  - [（3）训练脚本](#3_677)

# 一：cifar10数据集介绍

**cifar10数据集：CIFAR-10数据集是8000万微小图片的标签子集**

- [数据集下载链接](https://download.csdn.net/download/qq_39183034/87356613?spm=1001.2014.3001.5503)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F71ff86cc5b9742308426242aae8d50b8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

**数据集由6万张32\*32的彩色图片组成，一共有10个类别。每个类别6000张图片。其中有5万张训练图片及1万张测试图片。使用`torchvision.datasets.CIFAR10`可进行下载**

```python
train_dataset = datasets.CIFAR10(root=parametes.data_path, train=True,
                                 transform=transforms.ToTensor(), download=False)
test_dataset = datasets.CIFAR10(root=parametes.data_path, train=False,
                                transform=transforms.ToTensor(), download=False)
```

**如下，下载后会生成5个训练块文件和1个测试块文件，每一个块文件10000张图片**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Febb6e300a2c647339bc19e4a5cc8bd1d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

**这种文件并非图像文件，为了后续更好的训练，且能查看到训练过程中的图像变化，所以我们需要把它们转换为图像文件，转换代码如下**

```python
import pickle
import numpy as np
import glob
import os
import cv2

# 提取函数
def unpickle(file):
    with open(file, 'rb') as fo:
        dict = pickle.load(fo, encoding='bytes')
    return dict
# 类别名字
label_name = [
    "airplane",
    "automobile",
    "bird",
    "cat",
    "deer",
    "dog",
    "frog",
    "horse",
    "ship",
    "truck"
]

# 使用train_list和test_list拿到对应文件名字
train_list = glob.glob('./cifar-10-batches-py/data_batch_*')
# test_list = glob.glob('./cifar-10-batches-py/test_batch')

# 保存路径
save_path = './train/'

# 遍历
for l  in train_list:
    print(l)
    l_dict = unpickle(l)
    """
        映射为字典，有4个key
            batch_label：该图片属于哪一个batch
            labels：所属类别
            data：图像数组
            filenames：文件名
    """
    print(l_dict.keys())

    for im_idx, im_data in enumerate(l_dict[b'data']):
        # 获取类别和名字
        im_label = l_dict[b'labels'][im_idx]
        im_name = l_dict[b'filenames'][im_idx]

        # print(im_label, im_name, im_data)
        # 映射为英文名
        im_label_name = label_name[im_label]
        # 将此一维数组转为三维并交换维度
        im_data = np.reshape(im_data, [3, 32, 32])
        im_data = np.transpose(im_data, (1, 2, 0))

        # cv2.imshow("im_data", cv2.resize(im_data, (200, 200)))
        # cv2.waitKey(0)

        # 每个文件夹下创建对应类别文件夹，相同类别图片写入相同文件夹
        if not os.path.exists(os.path.join(save_path, im_label_name)):
            os.mkdir(os.path.join(save_path, im_label_name))
        cv2.imwrite(os.path.join(save_path, im_label_name, im_name.decode('utf-8')), im_data)
```

**转换后文件结构如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9d76b87bb51a4ff0ac59d74fb78fe603.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc75884a2bbc6468791224c65924c5f61.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F808dfe41bba542bfaf30026ca28878cb.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

# 二：代码

## （1）数据加载脚本编写

```python
import torchvision.datasets
from torchvision import transforms
from torch.utils.data import DataLoader, Dataset
import os
from PIL import Image
import numpy as np
import glob


# 类别名字
label_name = [
    "airplane",
    "automobile",
    "bird",
    "cat",
    "deer",
    "dog",
    "frog",
    "horse",
    "ship",
    "truck"
]
# 类比名字映射索引
label_dict = {
            
            }
for idx, name in enumerate(label_name):
    label_dict[name] = idx


def default_loader(path):
    return Image.open(path).convert("RGB")

train_transforms = transforms.Compose([
    transforms.RandomHorizontalFlip(),
    transforms.ToTensor()
])

class MyDataset(Dataset):
    """
        im_list：是一个列表，每一个元素是图片路径
        transform：对图片进行增强
        loader：使用PIL对图片进行加载
    """
    def __init__(self, im_list, transform=None, loader=default_loader):
        super(MyDataset, self).__init__()
        # imgs为二维列表，每一个子列表中第一个元素存储im_list，第二个通过label_dict映射为索引
        imgs = []

        for im_item in im_list:
            # 路径'./data/test/airplane/aeroplane_s_000002.png'中倒数第二个是标签名
            im_label_name = im_item.split("\\")[-2]
            imgs.append([im_item, label_dict[im_label_name]])

        self.imgs = imgs
        self.transform = transform
        self.loader = loader

    def __getitem__(self, index):
        im__path, im_label = self.imgs[index]

        # 会调用PIL加载图片数据
        im_data = self.loader(im__path)
        # 如果给了transoform那么就对图片进行增强
        if self.transform is not None:
            im_data = self.transform(im_data)

        return im_data, im_label

    def __len__(self):
        return len(self.imgs)

im_train_list = glob.glob(r'./data/train/*/*.png')
im_test_list = glob.glob(r'./data/test/*/*.png')
train_dataset = MyDataset(im_train_list, transform=train_transforms)
test_dataset = MyDataset(im_test_list, transform=transforms.ToTensor())

if __name__ == '__main__':
    im_train_list = glob.glob(r'./data/train/*/*.png')
    im_test_list = glob.glob(r'./data/test/*/*.png')

    train_dataset = MyDataset(im_train_list, transform=train_transforms)
    test_dataset = MyDataset(im_test_list, transform=transforms.ToTensor())
    print(len(train_dataset))
    print(len(test_dataset))

    train_loader = DataLoader(dataset=train_dataset, batch_size=6, shuffle=True, num_workers=0)
    test_loader = DataLoader(dataset=test_dataset, batch_size=6, shuffle=False, num_workers=0)

    """
    train_transforms = transforms.Compose([
        transforms.RandomResizedCrop((28, 28)),
        transforms.RandomHorizontalFlip(),
        transforms.RandomVerticalFlip(),
        transforms.RandomRotation(90),
        transforms.RandomGrayscale(0.1),
        transforms.ColorJitter(0.3, 0.3, 0.3, 0.3),
        transforms.ToTensor()
    ])

    train_dataset = torchvision.datasets.ImageFolder(root='./data/train', transform=train_transforms)
    test_dataset = torchvision.datasets.ImageFolder(root='./data/test', transform=transforms.ToTensor)
    print(train_dataset.classes[: 5])
    print("-"*30)
    print(train_dataset.class_to_idx)
    print("-"*30)
    print(train_dataset.imgs[: 5])
    
    """
```

## （2）模型搭建

### ①：VGG

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F051b6ed81ef8476990a0505b46dedc86.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

```python
# 给定字典选择模型
cfgs = {
            
            
    'vgg11': [64, 'M', 128, 'M', 256, 256, 'M', 512, 512, 'M', 512, 512, 'M'],
    'vgg13': [64, 64, 'M', 128, 128, 'M', 256, 256, 'M', 512, 512, 'M', 512, 512, 'M'],
    'vgg16': [64, 64, 'M', 128, 128, 'M', 256, 256, 256, 'M', 512, 512, 512, 'M', 512, 512, 512, 'M'],
    #'vgg16': [16, 16, 'M', 32, 32, 'M', 64, 64, 64, 'M', 128, 128, 128, 'M', 128, 128, 128, 'M'],
    'vgg19': [64, 64, 'M', 128, 128, 'M', 256, 256, 256, 256, 'M', 512, 512, 512, 512, 'M', 512, 512, 512, 512, 'M'],
}


# 生成卷积层
def create_conv(cfg):
    layers = []
    in_chaneels = parametes.init_in_chaneels

    # 遍历列表
    for c in cfg:
        # 如果遇到"M"，则增加一个最大池化层，其kernel_size=2, stride=2
        if c == 'M':
            layers += [nn.MaxPool2d(kernel_size=2, stride=2)]
        # 如果是数字，则代表该卷积核输出，卷积核统一为3×3，填充为1
        else:
            Conv2d = nn.Conv2d(in_channels=in_chaneels, out_channels=c, kernel_size=3, padding=1)
            layers += [Conv2d, nn.ReLU(True)]
            # 下一个输入通道等于现在的输出通道
            in_chaneels = c

    return nn.Sequential(*layers)


# VGG16网络
class VGG16(nn.Module):
    def __init__(self, conv, num_classes, init_weights=False):
        super(VGG16, self).__init__()
        self.conv = conv
        self.fc = nn.Sequential(
        	# 图片输入为224×224的前提下
            nn.Linear(512*7*7, 4096),
            nn.ReLU(True),
            nn.Dropout(p=0.5),
            nn.Linear(4096, 4096),
            nn.ReLU(True),
            nn.Dropout(p=0.5),
            nn.Linear(4096, num_classes)
        )
        if init_weights:
            self._initialize_weights()

    def forward(self, x):
        x = self.conv(x)
        x = torch.flatten(x, start_dim=1)
        x = self.fc(x)

        return x

    # 参数初始化（KAIMING）
    def _initialize_weights(self):
        for m in self.modules():
            if isinstance(m, nn.Conv2d):
                nn.init.kaiming_normal_(m.weight, mode='fan_out', nonlinearity='relu')
                if m.bias is not None:
                    nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.Linear):
                nn.init.normal_(m.weight, 0, 0.01)
                nn.init.constant_(m.bias, 0)

# 初始化网络
cfg = model.cfgs['vgg16']
net = model.VGG16(model.create_conv(cfg), parametes.num_classes, True)
net = net.to(parametes.device)


```

如下是VGG13，这种写法比较臃肿但清晰

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class VGG13(nn.Module):
    def __init__(self):
        super(VGG13, self).__init__()

        # N * 3 * 32 * 32
        self.conv1_1 = nn.Sequential(
            nn.Conv2d(3, 64, kernel_size=3, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU()
        )
        self.conv1_2 = nn.Sequential(
            nn.Conv2d(64, 64, kernel_size=3, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU()
        )
        self.max_pooling1 = nn.MaxPool2d(kernel_size=2, stride=2)

        # N * 64 * 16 * 16
        self.conv2_1 = nn.Sequential(
            nn.Conv2d(64, 128, kernel_size=3, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU()
        )
        self.conv2_2 = nn.Sequential(
            nn.Conv2d(128, 128, kernel_size=3, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU()
        )
        self.max_pooling2 = nn.MaxPool2d(kernel_size=2, stride=2)

        # N * 128 * 8 * 8
        self.conv3_1 = nn.Sequential(
            nn.Conv2d(128, 256, kernel_size=3, padding=1),
            nn.BatchNorm2d(256),
            nn.ReLU()
        )
        self.conv3_2 = nn.Sequential(
            nn.Conv2d(256, 256, kernel_size=3, padding=1),
            nn.BatchNorm2d(256),
            nn.ReLU()
        )
        self.max_pooling3 = nn.MaxPool2d(kernel_size=2, stride=2)

        # N * 256 * 4 * 4
        self.conv4_1 = nn.Sequential(
            nn.Conv2d(256, 512, kernel_size=3, padding=1),
            nn.BatchNorm2d(512),
            nn.ReLU()
        )
        self.conv4_2 = nn.Sequential(
            nn.Conv2d(512, 512, kernel_size=3, padding=1),
            nn.BatchNorm2d(512),
            nn.ReLU()
        )
        self.max_pooling4 = nn.MaxPool2d(kernel_size=2, stride=2)

        # N * 512 * 2 * 2
        self.conv5_1 = nn.Sequential(
            nn.Conv2d(512, 512, kernel_size=3, padding=1),
            nn.BatchNorm2d(512),
            nn.ReLU()
        )
        self.conv5_2 = nn.Sequential(
            nn.Conv2d(512, 512, kernel_size=3, padding=1),
            nn.BatchNorm2d(512),
            nn.ReLU()
        )
        self.max_pooling5 = nn.MaxPool2d(kernel_size=2, stride=2)

        # N * 512 * 1 * 1

        # 全连接层
        self.fc = nn.Sequential(
            nn.Linear(512 * 1 * 1, 4096),
            nn.ReLU(True),
            nn.Dropout(p=0.5),
            nn.Linear(4096, 4096),
            nn.ReLU(True),
            nn.Dropout(p=0.5),
            nn.Linear(4096, 10)
        )

    def forward(self, x):
        out = self.conv1_1(x)
        out = self.conv1_2(out)
        out = self.max_pooling1(out)

        out = self.conv2_1(out)
        out = self.conv2_2(out)
        out = self.max_pooling2(out)

        out = self.conv3_1(out)
        out = self.conv3_2(out)
        out = self.max_pooling3(out)

        out = self.conv4_1(out)
        out = self.conv4_2(out)
        out = self.max_pooling4(out)

        out = self.conv5_1(out)
        out = self.conv5_2(out)
        out = self.max_pooling5(out)

        out = torch.flatten(out, start_dim=1)

        out = self.fc(out)
        return out

```

### ②：ResNet

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff8aee2faf3a147f2b290c04745c1c225.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

# 基本跳连单元
class ResBlock(nn.Module):
    def __init__(self, in_channel, out_channel, stride=1):
        super(ResBlock, self).__init__()
        # 主干分支
        self.layer = nn.Sequential(
            nn.Conv2d(in_channel, out_channel, kernel_size=3, stride=stride, padding=1),
            nn.BatchNorm2d(out_channel),
            nn.ReLU(),
            nn.Conv2d(out_channel, out_channel, kernel_size=3, stride=1, padding=1),
            nn.BatchNorm2d(out_channel),
        )

        # 跳连分支
        self.shortcut = nn.Sequential()
        # 如果输入通道和输出通道不相等或者步长不是1，那么跳连分支必须再进行卷积
        if in_channel != out_channel or stride > 1:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_channel, out_channel, kernel_size=3, stride=stride, padding=1),
                nn.BatchNorm2d(out_channel),
            )
    def forward(self, x):
        # 主干
        out1 = self.layer(x)
        # 分支
        out2 = self.shortcut(x)
        # 最终结果 = 主干+分支
        out = out1 + out2
        out = F.relu(out)

class ResNet(nn.Module):
    def make_layer(self, block, out_channel, stride, num_block):
        layer_list = []
        for i in range(num_block):
            if i == 0:
                in_stride = stride
            else:
                in_stride = 1
            layer_list.append(block(self.in_channel, out_channel, in_stride))
            self.in_channel = out_channel

        return nn.Sequential(*layer_list)

    def __init__(self):
        super(ResNet, self).__init__()
        # 第一层采用普通卷积
        self.conv1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=3, stride=1, padding=1),
            nn.BatchNorm2d(32),
            nn.ReLU
        )

        # 四层ResNet
        self.in_channel = 32
        self.layer1 = self.make_layer(ResBlock, 64, 2, 2)
        self.layer2 = self.make_layer(ResBlock, 128, 2, 2)
        self.layer3 = self.make_layer(ResBlock, 256, 2, 2)
        self.layer4 = self.make_layer(ResBlock, 512, 2, 2)

        self.fc = nn.Linear(512, 10)


    def farward(self, x):
        out = self.conv1(x)
        out = self.layer1(out)
        out = self.layer2(out)
        out = self.layer3(out)
        out = self.layer4(out)
        out = F.avg_pool2d(out, 2)
        out = out.view(out.size(0), -1)
        out = self.fc(out)

        return out
```

### ③：MobileNetV1

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F57bc5b03ce954237b4b2e21702b24e49.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F


class MobileNet(nn.Module):

    # 深度可分离卷积由分组卷积和点卷积构成（基本单元）
    def conv_dw(self, in_channel, out_channel, stride):
        return nn.Sequential(
            # 分组卷积
            nn.Conv2d(in_channel, in_channel, kernel_size=3, stride=stride, padding=1, groups=in_channel,
                      bias=False),
            nn.BatchNorm2d(in_channel),
            nn.ReLU(),
            # 点卷积
            nn.Conv2d(in_channel, out_channel, kernel_size=1, stride=1, padding=0, bias=False),
            nn.BatchNorm2d(out_channel),
            nn.ReLU(),
        )

    def __init__(self):
        super(MobileNet, self).__init__()
        # 标准卷积层
        self.conv1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=3, stride=1, padding=1),
            nn.BatchNorm2d(32),
            nn.ReLU()
        )

        # 深度可分离卷积
        self.convdw2 = self.conv_dw(32, 32, 1)
        self.convdw3 = self.conv_dw(32, 64, 2)

        self.convdw4 = self.conv_dw(64, 64, 1)
        self.convdw5 = self.conv_dw(64, 128, 2)

        self.convdw6 = self.conv_dw(128, 128, 1)
        self.convdw7 = self.conv_dw(128, 256, 2)

        self.convdw8 = self.conv_dw(256, 256, 1)
        self.convdw9 = self.conv_dw(256, 512, 2)

        # 全连接层
        self.fc = nn.Linear(512, 10)

    def forward(self, x):
        out = self.conv1(x)
        out = self.convdw2(out)
        out = self.convdw3(out)
        out = self.convdw4(out)
        out = self.convdw5(out)
        out = self.convdw7(out)
        out = self.convdw8(out)
        out = self.convdw9(out)

        out =  F.avg_pool2d(out, 2)
        out = out.view(-1, 512)
        out = self.fc(out)

        return out



```

### ④：InceptionNet

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc486c8569e7040578197bbcbe8430e0e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128575928)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F


"""
    InceptionNet属于网中网结构，对输入进行
    不同分支的卷积，最后进行concat
"""

def ConvBNRelu(in_channel, out_channel, kernel_size):
    return nn.Sequential(
        nn.Conv2d(in_channel, out_channel, kernel_size=kernel_size, stride=1, padding=kernel_size//2),
        nn.BatchNorm2d(out_channel),
        nn.ReLU()
    )


class BaseInception(nn.Module):
    def __init__(self, in_channel, out_channel_list, reduce_channel_list):
        super(BaseInception, self).__init__()

        # 定义4个分支
        # 第1个分支
        self.branch1_conv = ConvBNRelu(in_channel, out_channel_list[0], 1)
        # 剩余分支为减少计算量必须先进行压缩
        self.branch2_conv1 = ConvBNRelu(in_channel, reduce_channel_list[0], 1)
        self.branch2_conv2 = ConvBNRelu(reduce_channel_list[0], out_channel_list[1], 3)

        self.branch3_conv1 = ConvBNRelu(in_channel, reduce_channel_list[1], 1)
        self.branch3_conv2 = ConvBNRelu(reduce_channel_list[1], out_channel_list[2], 5)

        self.branch4_pool = nn.MaxPool2d(kernel_size=3, stride=1, padding=1)
        self.branch4_conv = ConvBNRelu(in_channel, out_channel_list[3], 3)


    def forward(self, x):
        out1 = self.branch1_conv(x)

        out2 = self.branch2_conv1(x)
        out2 = self.branch3_conv2(out2)

        out3 = self.branch3_conv1(x)
        out3 = self.branch3_conv2(out3)

        out4 = self.branch4_pool(x)
        out4 = self.branch4_conv(out4)

        out = torch.cat([out1, out2, out3, out4], dim=1)

        return out

class InceptionNet(nn.Module):
    def __init__(self):
        super(InceptionNet, self).__init__()
        self.block1 = nn.Sequential(
            nn.Conv2d(3, 64, kernel_size=7, stride=2, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU()
        )

        self.block2 = nn.Sequential(
            nn.Conv2d(64, 128, kernel_size=3, stride=2, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU(),
        )

        self.block3 = nn.Sequential(
            BaseInception(in_channel=128, out_channel_list=[64, 64, 64, 64], reduce_channel_list=[16, 16]),
            nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
        )

        self.block4 = nn.Sequential(
            BaseInception(in_channel=256, out_channel_list=[96, 96, 96, 96], reduce_channel_list=[32, 32]),
            nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
        )

        self.fc = nn.Linear(384, 10)


    def forward(self, x):
        out = self.block1(x)
        out = self.block1(out)
        out = self.block1(out)
        out = self.block1(out)

        out = F.avg_pool2d(out, 2)
        out = out.view(out.size(0), -1)
        out = self.fc(out)

        return out
```

## （3）训练脚本

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader
import os
from tensorboardX import SummaryWriter

from VGG13 import VGG13
from dataloader import train_dataset, test_dataset

######################## 参数指定 ###################################
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")  # 设备
batch_size = 64  # batch_size大小
epochs = 200 # 总训练轮数

# 训练和验证数据加载
train_loader = DataLoader(dataset=train_dataset, batch_size=batch_size, shuffle=True, num_workers=0)
test_loader = DataLoader(dataset=test_dataset, batch_size=batch_size, shuffle=True, num_workers=0)

# 网络结构指定
net = VGG13()
net = net.to(device)

# 损失函数（交叉熵）
loss_function = nn.CrossEntropyLoss()

# 优化器（Adam）
optimizer = torch.optim.Adam(net.parameters(), lr=0.01)

# 学习率衰减
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=5, gamma=0.9)

# 日志
logger = SummaryWriter('./log')

step = 1
# 训练
for epoch in range(1, epochs):
    print("-----------当前epoch：{}-----------".format(epoch))
    net.train()
    for i, (X, y) in enumerate(train_loader):
        print("\r-----------当前trainbatch：{}/{}-----------".format(i, (len(train_loader))), end="")
        X = X.to(device)
        y = y.to(device)

        outputs = net(X)
        loss = loss_function(outputs, y)


        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        _, pred = torch.max(outputs.data, dim=1)
        correct = pred.eq(y.data).cpu().sum()
        accurate = correct * 100.0 / batch_size
        # print("train_loss: ", loss.item(), "accurate: ", accurate.item())
        logger.add_scalar('loss/train', loss, step)
        logger.add_scalar('accurate/train', accurate, step)

    net.eval()
    sum_loss = 0.
    sum_correct = 0
    with torch.no_grad():
        for i, (X, y) in enumerate(test_loader):
            print("\r-----------当前testbatch：{}/{}-----------".format(i, (len(test_loader))), end="")
            X = X.to(device)
            y = y.to(device)
            outputs = net(X)
            _, pred = torch.max(outputs.data, dim=1)
            correct = pred.eq(y.data).cpu().sum()
            loss = loss_function(outputs, y)
            sum_loss += loss.item()
            sum_correct += correct.item()

        test_loss = sum_loss * 1.0 / len(test_loader)
        test_accurate = sum_correct * 100.0 / len(test_loader) / batch_size
        logger.add_scalar('loss/test', test_loss, epoch)
        logger.add_scalar('accurate/test', test_accurate, epoch)
        print("test_loss: ", test_loss.item(), "test_accurate: ", test_accurate.item())
    step += 1
    # 学习率衰减
    scheduler.step()

logger.close()


```