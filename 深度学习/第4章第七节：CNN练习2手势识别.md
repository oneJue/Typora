 

### 文章目录

- [一：Sebastien Marcel Static Hand Posture Database（静态手势数据集）介绍](#Sebastien_Marcel_Static_Hand_Posture_Database_4)
- [二：模型设计](#_34)
- [三：代码编写](#_49)
- - [（1）数据预处理文件](#1_51)
  - [（2）MyDataset编写](#2MyDataset_243)
  - [（3）模型编写](#3_298)
  - [（4）训练脚本编写](#4_448)
  - [（5）tensorboard查看](#5tensorboard_597)
  - [（6）训练脚本编写](#6_606)

# 一：Sebastien Marcel Static Hand Posture Database（静态手势数据集）介绍

- [数据集下载]()

Sebastien Marcel Static Hand Posture Database提供了6种手势姿势，如下图，分别代表

- A
- B
- C
- five
- point
- V

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F429d57dc8d7746e783886ca6c8ad6ba3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

图片格式为`.ppm`

- PBM 是位图（bitmap），仅有黑与白，没有灰
- PGM 是灰度图（grayscale）
- PPM 是通过RGB三种颜色显现的图像（pixmaps）  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F209839b3428b456ca455d90aee71f9b4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

数据下载并解压后，格式如下

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F654f10e1deae4c99a02da06eab82bd2b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

# 二：模型设计

**设计网络结构如下**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcc7c25d1250c40fc96f699c975369c62.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

**其中MTB结构如下图所示**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2ea45df5ae3540eb837fda57402b0f15.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

# 三：代码编写

## （1）数据预处理文件

**`preprocess.py`：原始数据集每个文件夹图片数据均不一样，并且在测试集中还存在两类测试图片，分别表示不同环境拍摄（`complex`和`uniform`），所以这里对所有数据集进行整合操作，将训练集和测试集进行合并，然后再按照`8:2:1`的比例分别分配给训练集、验证集和测试集。最终会生成`train_hand_gesture.txt`、`eval_hand_gesture.txt`、`test_hand_gesture.txt`三个文件，后续所有操作将会针对这三个文件进行，而不在原始数据集上进行**

以`train_hand_gesture.txt`为例，每一行表示图片的路径和其所属类别

 -    “A”：0
 -    “B”：1
 -    “C”：2
 -    “Five”：3
 -    “Point”：4
 -    “V”：5

```c
0|./data/Marcel-Train\A\A-train1043.ppm
0|./data/Marcel-Train\A\A-train0577.ppm
2|./data/Marcel-Train\C\C-train455.ppm
0|./data/Marcel-Train\A\A-train1291.ppm
4|./data/Marcel-Train\Point\Point-train0220.ppm
0|./data/Marcel-Train\A\A-train1072.ppm
0|./data/Marcel-Train\A\A-train0599.ppm
4|./data/Marcel-Train\Point\Point-train0060.ppm
2|./data/Marcel-Test\C\complex\C-complex07.ppm
4|./data/Marcel-Train\Point\Point-train0944.ppm
4|./data/Marcel-Train\Point\Point-train1333.ppm
4|./data/Marcel-Train\Point\Point-train0778.ppm
0|./data/Marcel-Train\A\A-train0795.ppm
3|./data/Marcel-Train\Five\Five-train148.ppm
3|./data/Marcel-Train\Five\Five-train394.ppm
3|./data/Marcel-Train\Five\Five-train441.ppm
...
```

代码如下，关键之处有注释说明

```python
import os
import json
from Parameters import parameters
import random
from PIL import Image

random.seed(parameters.seed)


# 获取某个文件夹下面所有后缀名为suffix的文件，并返回其path的list
import os
import json
from Parameters import parameters
import random
from PIL import Image

random.seed(parameters.seed)


# 获取某个文件夹下面所有后缀名为suffix的文件，并返回其path的list
def recursive_fetching(root, suffix):
    all_file_path = []

    # get_all_files函数会被递归调用
    def get_all_files(path):
        all_file_list = os.listdir(path)
        # 遍历path文件夹下的所有文件和目录
        for file in all_file_list:
            filepath = os.path.join(path, file)
            # 如果是目录则再次递归
            if os.pa
            th.isdir(filepath):
                get_all_files(filepath)
            # 如果是文件则保存其文件路径和文件名到all_file_path中
            elif os.path.isfile(filepath):
                all_file_path.append(filepath)

    # 把根目录传入
    get_all_files(root)
    # 筛选所有后缀名为suffix的文件
    file_paths = [it for it in all_file_path if os.path.split(it)[-1].split('.')[-1].lower() in suffix]

    return file_paths

# 加载meta文件
def load_meta(meta_path):
    with open(meta_path, 'r') as fr:
        return [line.strip().split('|') for line in fr.readlines()]

# 加载图片
def load_image(image_path):
    return Image.open(image_path)

# 构建类别到id的映射
cls_mapper = {
            
            
    "clsToid": {
            
            "A": 0, "B": 1, "C": 2, "Five": 3, "Point": 4, "V": 5},
    "idTocls": {
            
            0: "A", 1: "B", 2: "C", 3: "Five", 4: "Point", 5: "V"}
}
if not os.path.exists(parameters.cls_mapper_path):
    json.dump(cls_mapper, open(parameters.cls_mapper_path, 'w'))

train_items = recursive_fetching(parameters.train_data_root, 'ppm')  # 获取Marcel-Train文件夹下数据路径
test_items = recursive_fetching(parameters.test_data_root, 'ppm')  # 获取Marcel-Test文件夹下数据路径
dataset = train_items + test_items  # 合并
random.shuffle(dataset)  # 打乱数据集
dataset_num = len(dataset)
print("数据集总数目：", dataset_num)



"""
    最终dataset_dict大概长这样子
    dataset_dict = {
    0: ["./data/Marcel-Test/A/complex/A-complex32.ppm", "./data/Marcel-Test/A/complex/A-complex31.ppm", ...]
    1: ["./data/Marcel-Train/B/B-train119.ppm", "./data/Marcel-Test/B/uniform/B-uniform04.ppm", ...]
    ...
    5: [...]
}
"""
dataset_dict = {
            
            }
for it in dataset:
    # 例如"./data/Marcel-Train/B/B-train119.ppm"，cls_name就是B, cls_id就是1
    cls_name = os.path.split(it)[-1].split('-')[0]
    cls_id = cls_mapper["clsToid"][cls_name]
    # 例如，把所有属于B的训练数据和图片数据放到一个列表中，该列表的k值为1
    if cls_id not in dataset_dict:
        dataset_dict[cls_id] = [it]
    else:
        dataset_dict[cls_id].append(it)

# 每个列表按照比例分配到train、eval、test中
train_ratio, eval_ratio, test_ratio = 0.8, 0.1, 0.1
train_set, eval_set, test_set = [], [], []

for idx, set_list in dataset_dict.items():
    length = len(set_list)
    train_num, eval_num = int(length*train_ratio), int(length*eval_ratio)
    test_num = length - train_num - eval_num
    random.shuffle(set_list)
    train_set.extend(set_list[:train_num])
    eval_set.extend(set_list[train_num:train_num+eval_num])
    test_set.extend(set_list[train_num+eval_num:])

random.shuffle(train_set)
random.shuffle(eval_set)
random.shuffle(test_set)
# print(train_set)
# print(eval_set)
# print(test_set)
# print(len(train_set) + len(eval_set) + len(test_set))

# 写入metafile

with open(parameters.metadata_train_path, 'w') as fw:
    for path in train_set:
        cls_name = os.path.split(path)[-1].split('-')[0]
        cls_id = cls_mapper["clsToid"][cls_name]
        fw.write("%d|%s\n" % (cls_id, path))


with open(parameters.metadata_eval_path, 'w') as fw:
    for path in eval_set:
        cls_name = os.path.split(path)[-1].split('-')[0]
        cls_id = cls_mapper["clsToid"][cls_name]
        fw.write("%d|%s\n" % (cls_id, path))

with open(parameters.metadata_test_path, 'w') as fw:
    for path in test_set:
        cls_name = os.path.split(path)[-1].split('-')[0]
        cls_id = cls_mapper["clsToid"][cls_name]
        fw.write("%d|%s\n" % (cls_id, path))



# 测试，看一下所有图片的颜色模式和对应大小
mode_set, size_set = [], []
for _, path in load_meta(parameters.metadata_train_path):
    img = load_image(path)
    mode_set.append(img.mode)
    size_set.append(img.size)


print(set(mode_set), set(size_set))


```

结果显示这批图片颜色模式均为RGB，但是大小各异，所以在后面加载的时候需要统一图片大小

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc0705a4fe07b4d918f283f0f0863226b.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

## （2）MyDataset编写

Pytorch中的`DataLoader`在需要传入你自己封装好的`MyDataset`，它是`torch.utils.data.Dataset`的子类，至少重写`__getitem(self)__`、`__len(self)__`方法。另外在编写`MyDataset`时我们还要引入数据增强，也即`transofroms.Compose`

```python
import torch
from torch.utils.data import DataLoader
from Parameters import parameters
from preporcess import load_meta, load_image
from torchvision import transforms

transform_train = transforms.Compose(
    [
        transforms.Resize((112, 112)),  # 保证输入图像大小为112×112
        transforms.RandomRotation(degrees=45),  # 减小倾斜图片影像
        transforms.GaussianBlur(kernel_size=(3, 3)),  # 抑制模糊图片影响
        transforms.RandomHorizontalFlip(),  # 左右手
        transforms.ToTensor(),
        transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))  # 标准化
    ]
)

transform_test = transforms.Compose(
    [
        transforms.Resize((112, 112)),  # 保证输入图像大小为112×112
        # transforms.RandomRotation(degrees=45),  # 减小倾斜图片影像
        # transforms.GaussianBlur(kernel_size=(3, 3)),  # 抑制模糊图片影响
        # transforms.RandomHorizontalFlip(),  # 左右手
        transforms.ToTensor(),
        transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))  # 标准化
    ]
)

class MyDataset(torch.utils.data.Dataset):
    def __init__(self, metadata_path):
        self.dataset = load_meta(metadata_path)  # [(0, image_path), (), ...]
        self.metadata_path = metadata_path

    def __getitem__(self, idx):
        item = self.dataset[idx]
        cls_id, path = int(item[0]), item[1]
        img = load_image(path)

        if self.metadata_path == parameters.metadata_train_path or self.metadata_path == parameters.metadata_eval_path:
            return transform_train(img), cls_id

        # 对于测试集不需要数据增强
        return transform_test(img), cls_id
    def __len__(self):
        return len(self.dataset)
```

## （3）模型编写

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

from Parameters import  parameters

# 定义mish激活函数
def mish(x):
    return x * torch.tanh(F.softplus(x))

# 封装mish激活函数
class Mish(nn.Module):
    def __init__(self):
        super(Mish, self).__init__()

    def forward(self, x):
        return mish(x)


# 深度可分离卷积
class DSConv2d(nn.Module):
    def __init__(self, in_channels, out_channels, kernel_size):
        super(DSConv2d, self).__init__()
        # 保证kernel_size必须是奇数
        assert kernel_size % 2 == 1, "kernel_size必须为奇数"
        self.depth_conv = nn.Conv2d(
            in_channels=in_channels,
            out_channels=out_channels,
            kernel_size=kernel_size,
            padding=(kernel_size//2, kernel_size//2),
            groups=in_channels
        )
        self.pointwise_conv = nn.Conv2d(
            in_channels=in_channels,
            out_channels=out_channels,
            kernel_size=1
        )

    def forward(self, input_x):
        out = self.depth_conv(input_x)
        out  = self.pointwise_conv(out)

        return out

# 编写MTB模块（残差网络）
class MTB(nn.Module):
    def __init__(self, in_channels):
        super(MTB, self).__init__()

        self.left_flow = nn.Sequential(
            # 点卷积
            nn.Conv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=1),
            nn.BatchNorm2d(in_channels),
            Mish(),
            # 深度可分离卷积
            DSConv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=3),
            nn.BatchNorm2d(in_channels),
            Mish(),
            # 7×7卷积
            nn.Conv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=7, padding=(7//2, 7//2)),
        )

        self.right_flow = nn.Sequential(
            # 7×7卷积
            nn.Conv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=7, padding=(7 // 2, 7 // 2)),
            nn.BatchNorm2d(in_channels),
            Mish(),
            # 深度可分离卷积
            DSConv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=3),
            nn.BatchNorm2d(in_channels),
            Mish(),
            # 点卷积
            nn.Conv2d(in_channels=in_channels, out_channels=in_channels, kernel_size=1),
        )

    def forward(self, input_ft):
        left = self.left_flow(input_ft)
        right = self.right_flow(input_ft)
        out = left + right + input_ft

        out = mish(out)
        return out


# [N, 3, 112, 112] -> [N, 256, 7, 7]
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()

        self.conv = nn.Sequential(
            nn.Conv2d(in_channels=parameters.data_channels, out_channels=64, kernel_size=3, padding=(3//2, 3//2)),
            nn.BatchNorm2d(64),
            Mish(),
            # MTB模块
            MTB(in_channels=64),
            nn.MaxPool2d(kernel_size=2, stride=2),

            nn.Conv2d(in_channels=64, out_channels=128, kernel_size=3, padding=(3 // 2, 3 // 2)),
            nn.BatchNorm2d(128),
            Mish(),
            # MTB模块
            MTB(in_channels=128),
            nn.MaxPool2d(kernel_size=2, stride=2),

            nn.Conv2d(in_channels=128, out_channels=256, kernel_size=3, padding=(3 // 2, 3 // 2)),
            nn.BatchNorm2d(256),
            Mish(),
            # MTB模块
            MTB(in_channels=256),
            nn.MaxPool2d(kernel_size=2, stride=2),

            MTB(in_channels=256),
            MTB(in_channels=256),
            MTB(in_channels=256),
            nn.MaxPool2d(kernel_size=2, stride=2),
        )

        self.fc = nn.Sequential(
            nn.Linear(in_features=256*7*7, out_features=2048),
            Mish(),
            nn.Dropout(parameters.fc_dropout_prob),

            nn.Linear(in_features=2048, out_features=1024),
            Mish(),
            nn.Dropout(parameters.fc_dropout_prob),

            nn.Linear(in_features=1024, out_features=parameters.classes_num)
        )

    def forward(self, input_x):
        out = self.conv(input_x)  # [N, 256, 7, 7]
        out = torch.flatten(out, start_dim=1)
        out = self.fc(out)

        return out


# 验证
if __name__ == '__main__':
    net = Net()
    x = torch.randn(size=(5, 3, 112, 112))
    y_pred = net(x)
    print(y_pred.size())

```

## （4）训练脚本编写

```python
import os
import torch
import random
import numpy as np
from tensorboardX import SummaryWriter
from argparse import ArgumentParser
from torch.utils.data import DataLoader


from Parameters import parameters
from dataloader import MyDataset
from model import Net

if parameters.device == 'cuda':
    print("GPU上运行")
else:
    print("CPU上运行")
    

# 日志记录
logger = SummaryWriter('./log')

# 随机种子
torch.manual_seed(parameters.seed)  # CPU随机种子
if parameters.device == 'cuda':
    torch.cuda.manual_seed(parameters.seed)  # GPU随机种子（若有）
random.seed(parameters.seed)  # random随机种子
np.random.seed(parameters.seed)  # numpy随机种子

# 使用验证集对模型进行评估
def evaluate(model, val_loader, loss_func):
    #  进入eval模式
    model.eval()
    sum_loss = 0.
    #  with torch.no_grad()含义：https://blog.csdn.net/qq_42251157/article/details/124101436
    with torch.no_grad():
        for batch in val_loader:
            X, y = batch
            X = X.to(parameters.device)
            y = y.to(parameters.device)
            pred = model(X)
            loss = loss_func(pred, y)
            sum_loss += loss.item()
    #  特别注意返回train模式
    model.train()
    return sum_loss / len(val_loader)

# 保存模型
def save_checkpoint(model, epoch, optimizer, checkpoint_path):
    save_dict = {
            
            
        'epoch': epoch,
        'model_state_dict': model.state_dict(),
        'optimizer_state_dict': optimizer.state_dict()
    }

    torch.save(save_dict, checkpoint_path)



# 训练函数
def train():
    # 有关argparse.ArgumentParser用法：https://blog.csdn.net/u011913417/article/details/109047850
    # 其作用是解析命令行参数，目的是在终端窗口(ubuntu是终端窗口，windows是命令行窗口)输入训练的参数和选项
    parser = ArgumentParser(description='Model Training')
    parser.add_argument(
        '--c',
        # 当模型再次训练时选择从头开始还是从上次停止的地方开始
        default=None,  # 当参数未在命令行中出现时使用的值
        type=str,  # 参数类型
        help='from head or last checkpoint?'  # 参数说明
    )
    args = parser.parse_args()

    #  模型实例
    model = Net()
    model = model.to(parameters.device)

    # 损失函数（这里比较简单所以直接定义，否则需要新建文件loss.py存放）
    loss_func = torch.nn.CrossEntropyLoss()  # 交叉熵损失函数

    # 优化器
    optimizer = torch.optim.Adam(model.parameters(), parameters.init_lr)

    # 训练数据加载
    trainset = MyDataset(parameters.metadata_train_path)
    train_loader = DataLoader(trainset, batch_size=parameters.batch_size, shuffle=True, drop_last=True, num_workers=8)
    # 验证数据加载（在evaluation函数中进行评估）
    valset = MyDataset(parameters.metadata_eval_path)
    val_loader = DataLoader(valset, batch_size=parameters.batch_size, shuffle=True, drop_last=False,  num_workers=8)

    # 起始训练轮数， 步数
    start_epoch, step = 0, 0

    # 判断参数，是否需要从检查点开始训练
    # 主要针对大型数据，可能会训练几个小时或几天，所以容易出现问题
    if args.c:
        checkpoint = torch.load(args.c)  # 加载模型
        #  加载参数（权重系数、偏置值、梯度等等）
        start_epoch = checkpoint['epoch']
        model.load_state_dict(checkpoint['model_state_dict'])
        optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
        print("参数加载成功")

    else:
        print("从头开始训练")

    # 关于model.train()说明：https://blog.csdn.net/weixin_44211968/article/details/123774649
    model.train()  # 启用 batch normalization 和 dropout

    # 训练过程
    for epoch in range(start_epoch, parameters.epochs):
        print("-----------当前epoch：{}-----------".format(epoch))
        for i, batch in enumerate(train_loader):
            print("-----------当前batch：{}/{}-----------".format(i, len(train_loader)))
            X, y = batch
            X = X.to(parameters.device)
            y = y.to(parameters.device)
            pred = model(X)
            loss = loss_func(pred, y)

            optimizer.zero_grad()
            loss.backward()
            optimizer.step()

            logger.add_scalar('loss/train', loss, step)

            # 每10步进行验证集评估并保存
            if not step % parameters.verbose_step:
                eval_loss = evaluate(model, val_loader, loss_func)
                logger.add_scalar('loss/val', eval_loss, step)
            if not step % parameters.save_step:
                model_path = "epoch{}_step{}.pth".format(epoch, step)
                save_checkpoint(model, epoch, optimizer, os.path.join('model_save', model_path))

            step += 1
            logger.flush()
            print("当前step：{}；当前train_loss：{:.5f}；当前val_loss：{:.5f}".format(step, loss.item(), eval_loss))
    logger.close()

if __name__ == '__main__':
    train()


```

## （5）tensorboard查看

选择"`epoch86_step6000.pth`"作为最佳模型

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F071cca6cae424df0a9fbada7f8a9dca7.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

## （6）训练脚本编写

查看单个图片

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F07f3904741b848879820bdbcd617b43a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

查看总正确率

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa308012d4d9042a787045e377f852f1e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F128934763)

```python
import torch
from torch.utils.data import DataLoader
from dataloader import MyDataset
from model import Net
from Parameters import parameters
import numpy as np
# import cv2


#  网络实例
model = Net()
#  加载模型：观察tensorboard可知，迭代600次时模型收敛
checkpoint = torch.load('./model_save/epoch86_step6000.pth', map_location=parameters.device)
#  加载模型参数
model.load_state_dict(checkpoint['model_state_dict'])

#  加载测试数据
testset = MyDataset(parameters.metadata_test_path)
test_loader = DataLoader(testset, batch_size=parameters.batch_size, shuffle=True, drop_last=False)

#  预测时，进入eval模式
model.eval()

#  预测正确的个数
correct_num = 0

# json文件
cls_mapper = {
            
            
    "clsToid": {
            
            "A": 0, "B": 1, "C": 2, "Five": 3, "Point": 4, "V": 5},
    "idTocls": {
            
            0: "A", 1: "B", 2: "C", 3: "Five", 4: "Point", 5: "V"}
}

with torch.no_grad():
    for batch in test_loader:
        X, y = batch
        X = X.to(parameters.device)
        y = y.to(parameters.device)
        pred = model(X)
        correct_num += (torch.argmax(pred, 1) == y).sum()
        
        """
        如果需要查看图片请去掉注释，并导入OpenCV
        X = X.numpy()
        y = y.numpy()
        pred = pred.numpy()

        for index in range(np.shape(X)[0]):

            image_data = X[index]
            image_label = y[index]
            image_pred = np.argmax(pred[index])

            # RGB -> BGR
            image_data = image_data.transpose(1, 2, 0)
            print("此图预测值为{}，真实值为{}".format(cls_mapper["idTocls"][image_pred], cls_mapper["idTocls"][image_label]))
            cv2.imshow("image_data", image_data)
            cv2.waitKey(0)
            
        """

print("测试数据{}个，正确预测{}个，预测准确率：{}%".format(len(testset), correct_num, (correct_num / len(testset)) * 100))


```