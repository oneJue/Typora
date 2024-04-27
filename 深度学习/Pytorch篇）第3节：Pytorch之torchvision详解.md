 

### 文章目录

- [一：torchvision概述](#torchvision_3)
- [二：torchvision.datasets](#torchvisiondatasets_11)
- - [（1）官方数据集](#1_16)
  - [（2）自定义数据集类](#2_73)
  - [（3）ImageFolder手动实现](#3ImageFolder_145)
- [三：torchvision.transforms](#torchvisiontransforms_243)
- [四：torchvision.models](#torchvisionmodels_306)

# 一：torchvision概述

**torchvision：`torchvision`是Pytorch的一个图形库，主要用来构建计算机视觉模型，`torchvision`由以下四个部分构成**

- `torchvision.datasets`：包括一些加载数据的函数和常用的数据集接口
- `torchvision.models`：包含常用的模型结构（含预训练模型），例如AlexNet、ResNet等等
- `torchvision.transforms`：包含一些常见的图片变换，例如裁剪、旋转等等
- `torchvision.utils`：其他用法

# 二：torchvision.datasets

**torchvision.datasets：该模块下既有官方提供的数据集，也有自定义数据集的类，两者都是`torch.utils.data.Dataset`的子类，因此可以直接输入到`torch.utils.data.DataLoader`中去**

## （1）官方数据集

**`torchvision.datasets`中提供的官方数据如下，这些数据集详细介绍见此文：[数据集介绍](https://blog.csdn.net/Threelights/article/details/88680540?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161924921816780255250307%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=161924921816780255250307&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-88680540.first_rank_v2_pc_rank_v29&utm_term=torchvision.datasets)**

```bash
MNIST
Fashion-MNIST
KMNIST
EMNIST
FakeData
COCO
Captions
Detection
LSUN
​ImageFolder
DatasetFolder
Imagenet-12
CIFAR
STL10
SVHN
PhotoTour
SBU
Flickr
VOC
Cityscapes
...
```

**这里我们以MNIST数据集为例，演示一下这些官方数据集如何加载，其余数据集的加载和MNIST一致**

如下，使用`torchvision.datasets.MNIST`加载MNIST数据集

```python
train_data = dataset.MNIST(root='./mnist/',
                           train=True,
                           transform=transforms.ToTensor(),
                           download=True)
test_data = dataset.MNIST(root='./mnist/',
                           train=False,
                           transform=transforms.ToTensor(),
                           download=False)
```

- **`root`**：表示数据集待存放的目录
- **`train`**：如果为`true`将会使用训练集的数据集（`training.pt`），如果为`false`将会使用测试集数据集（`test.pt`）
- **`download`**：如果为`true`将会从网络上下载并放入`root`中，如果数据集已下载则不会再次下载
- **`transform`**：接受PIL图片并返回转换后的图片，常用的就是转换为`tensor`（这里便会调用`torchvision.transform`）

数据集加载成功后，文件布局如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbfb74d210e16420688de1457514e1115.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)

## （2）自定义数据集类

**这里的自定义数据集类指的主要是`torchvision.datasets.ImageFolder()`，它继承自 `torchvision.datasets.DatasetFolder()`，后者又继承自 `torchvision.datasets.VisionDataset()`，而`VisionDataset` 则是 `torch.utils.data.Dataset` 的子类**

**以`torchvision.datasets.CIFAR`数据集为例说明如何使用`torchvision.datasets.ImageFolder()`，这里的`torchvision.datasets.CIFAR`我已经将其转换为png格式存储**

- [下载链接](https://download.csdn.net/download/qq_39183034/87356613?spm=1001.2014.3001.5503)
- CIFAR10有60000张图片，共分为10个类别，其中50000张为训练图片（每个类别5000张），10000张为测试图片（每个类别1000张）

**图片文件布局如下，`torchvision.datasets.ImageFolder()`要求你的图片数据必须按照以下方式进行组织**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe98fc5a632f14fba9cf6ba3d68d0fffc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)

**`torchvision.datasets.ImageFoler`参数说明**

 -    **`root`**：图片存储的根目录，即各类别文件夹所在目录的上一级目录
 -    **`transform`**：对图片进行预处理的操作\(函数\)，原始图片作为输入，返回一个转换后的图片
 -    **`target_transform`**：对图片类别进行预处理的操作，输入为 `target`，输出对其的转换。如果不传该参数，即对 `target` 不做任何转换，返回的顺序索引 0,1, 2…
 -    **`loader`**：表示数据集加载方式，通常默认加载方式即可

```python
torchvision.datasets.ImageFolder(root,transform,target_transform,loader)
```

**如下，使用`torchvision.datasets.ImageFoler`对前面的图片进行加载**

 -    **注意**：`transforms`部分可暂时忽略

```python
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
print(len(train_dataset))
print(len(test_dataset))
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F672ea7c2b3c34e569d238e6b7dc40def.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)

**同时，通过`torchvision.datasets.ImageFolder`生成的`train_dataset`和`test_dataset`还有如下3个成员变量**

 -    **`self.classes`**：使用一个`list`保存类别名称
 -    **`self.class_to_idx`**：类别对应的索引
 -    **`self.imgs`**：是一个`list`，每个元素是一个`tuple`，每个`tuple`保存的是`（img-path, class）`

```python
print(train_dataset.classes[: 5])
print("-"*30)
print(train_dataset.class_to_idx)
print("-"*30)
print(train_dataset.imgs[: 5])
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe716e05321be4e59930953a8edec6401.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)

## （3）ImageFolder手动实现

**仍然以上述CIFAR10数据集为例，我们手动实现一下`ImageFolder`，这对你理解它大有帮助**

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
    transforms.RandomResizedCrop((28, 28)),
    transforms.RandomHorizontalFlip(),
    transforms.RandomVerticalFlip(),
    transforms.RandomRotation(90),
    transforms.RandomGrayscale(0.1),
    transforms.ColorJitter(0.3, 0.3, 0.3, 0.3),
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


if __name__ == '__main__':
    im_train_list = glob.glob(r'./data/train/*/*.png')
    im_test_list = glob.glob(r'./data/test/*/*.png')

    train_dataset = MyDataset(im_train_list, transform=train_transforms)
    test_dataset = MyDataset(im_test_list, transform=transforms.ToTensor())
    print(len(train_dataset))
    print(len(test_dataset))

    train_loader = DataLoader(dataset=train_dataset, batch_size=6, shuffle=True, num_workers=0)
    test_loader = DataLoader(dataset=test_dataset, batch_size=6, shuffle=False, num_workers=0)

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8d48a2b5d8b44ef3bedf248d9dc00100.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)

# 三：torchvision.transforms

**torchvision.transforms：该模块是Pytorch中的图像预处理包，包含了一些常用的图像变换，主要实现对数据集的预处理、数据增强，转化为`tensor`等操作**

**使用时如果有很多变换，那么一般会使用Compose将这些步骤给整合到一起**

```python
train_transforms = transforms.Compose([
    transforms.RandomResizedCrop((28, 28)),
    transforms.RandomHorizontalFlip(),
    transforms.RandomVerticalFlip(),
    transforms.RandomRotation(90),
    transforms.RandomGrayscale(0.1),
    transforms.ColorJitter(0.3, 0.3, 0.3, 0.3),
    transforms.ToTensor()
])

train_dataset = torchvision.datasets.ImageFolder(root='./data/train', transform=train_transforms

```

**如果变换时只有一种，那么一般会直接给到形参，比如最常使用到的`ToTensor`**

```python
test_dataset = torchvision.datasets.ImageFolder(root='./data/test', transform=transforms.ToTensor)
```

**`torchvision.transforms`涉及变换主要有以下4类**

- **裁剪**：

  - **中心裁剪**：`transforms.CenterCrop`
  - **随机裁剪**：`transforms.RandomCrop`
  - **随机长宽比裁剪**：`transforms.RandomResizedCrop`
  - **上下左右中心裁剪**：`transforms.FiveCrop`
  - **上下左右中心裁剪后翻转**：`transforms.TenCrop`

- **翻转和旋转**

  - **依概率p水平翻转**：`transforms.RandomHorizontalFlip(p=0.5)`
  - **依概率p垂直翻转**：`transforms.RandomVerticalFlip(p=0.5)`
  - **随机旋转**：`transforms.RandomRotation`

- **图像变换和转换**

  - **变换为某一尺寸**：`transforms.Resize`
  - **标准化**：`transforms.Normalize`
  - **转化为`tensor`并归一化**：`transforms.ToTensor`
  - **填充**：`transforms.Pad`
  - **修改亮度、对比度和饱和度**：`transforms.ColorJitter`
  - **转化为灰度图**：`transforms.Grayscale`
  - **线性变化**：`transforms.LinearTransformation`
  - **仿射变换**：`transforms.RandomAffine`
  - **依概率p转化为灰度图**：`transforms.RandomGrayscale`
  - **将数据转化为PILImage**：`transforms.ToPILImage`

- **其他操作**

  - **对`transforms`操作使数据增强更灵活**：`transforms.RandomChoice(transforms)`
  - **从给定的一系列`transforms`中选定一个操作**：`transforms.RandomApply(transforms, p=0.5)`
  - **给一个`transform`加上概率进行操作**：`transforms.RandomOrder`

# 四：torchvision.models

**torchvision.models：该模块提供了很多图像处理中的常用模型，并且提供了预训练版本，使用时导入如下包**

```python
from torchvision import models
```

**在`models`中定义有如下模型**

```python
from .alexnet import *
from .convnext import *
from .densenet import *
from .efficientnet import *
from .googlenet import *
from .inception import *
from .mnasnet import *
from .mobilenet import *
from .regnet import *
from .resnet import *
from .shufflenetv2 import *
from .squeezenet import *
from .vgg import *
from .vision_transformer import *
from .swin_transformer import *
from .maxvit import *
from . import detection, optical_flow, quantization, segmentation, video
from ._api import get_model, get_model_builder, get_model_weights, get_weight, list_models

```

**如何使用这些模型，以及如何修改，请参照下面的这批文章**

- [链接](https://www.cnblogs.com/winlsr/p/15700070.html)

**总之我们对模型的修改可能主要集中在全连接层，比如原始模型最后全连接格式为**

```python
(fc): Linear(in_features=512, out_features=1000, bias=True)
```

**如果处理的是cifar10数据集，那么`out_features`应该是10，所以可以这样改**

```python
resnet = models.resnet18()
resnet.fc = nn.Linear(resnet.fc.in_features, 10)
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffd82796dd5a74f67a866bcb9280c6f80.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128518732)