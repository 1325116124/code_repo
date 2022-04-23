# Pytorch

## 文档的查看和使用

- **dir()**
- **help()**

## pytorch加载数据

- **Dataset：提供一种方式取获取数据及其label**
- **Dataloader：为后面的网络提供不同的数据形式**

## dataset的使用

```python
from torch.utils.data import Dataset
from PIL import Image
import os

class MyData(Dataset):
    def __init__(self, root_dir, label_dir):
        self.root_dir = root_dir
        self.label_dir = label_dir
        self.path = os.path.join(self.root_dir, self.label_dir)
        self.img_path = os.listdir(self.path)

    def __getitem__(self, idx):
        img_name = self.img_path[idx]
        img_item_path = os.path.join(self.root_dir,self.label_dir,img_name)
        img = Image.open(img_item_path)
        label = self.label_dir
        return img,label

    def __len__(self):
        return len(self.img_path)

root_dir = "dataset/train"
ans_label_dir = "ants"
bees_label_dir = "bees"
ants_dataset = MyData(root_dir,ans_label_dir)
bees_dataset = MyData(root_dir,bees_label_dir)
train_dataset = ants_dataset + bees_dataset
img,label = ants_dataset[0]
img.show()
```

## tensorboard的使用

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 11:15
# @Author: 阳洪
# @File:writer.py
# @Software:PyCharm
from torch.utils.tensorboard import SummaryWriter
import numpy as np
from PIL import Image
writer = SummaryWriter("logs")
image_path = "dataset/train/ants_image/0013035.jpg"
img_PIL = Image.open(image_path)
img_array = np.array(img_PIL)
writer.add_image("test",img_array,dataformats="HWC")
for i in range(100):
    writer.add_scalar("y=x",i,i)
# tensorboard --logdir=logs
writer.close()
```

## transforms的使用

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 11:42
# @Author: 阳洪
# @File:transformsTest.py
# @Software:PyCharm
from PIL import Image
from torchvision import transforms
from torch.utils.tensorboard import SummaryWriter
# tensor数据类型只是对数据进行了一层包装，增加了神经网络所必要的参数属性
img_path = "dataset/train/ants_image/0013035.jpg"
img = Image.open(img_path)
writer = SummaryWriter("logs")
tensor_trans = transforms.ToTensor()
tensor_img = tensor_trans(img)
writer.add_image("Tensor_img",tensor_img)
writer.close()
print(tensor_img)
```

## 常用的transforms

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 11:59
# @Author: 阳洪
# @File:usefulTensorboard.py
# @Software:PyCharm
from PIL import Image
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms
writer = SummaryWriter("logs")
img = Image.open("dataset/train/ants_image/0013035.jpg")
trans_tensor = transforms.ToTensor()
img_tensor = trans_tensor(img)
writer.add_image("ToTensor",img_tensor)

# 归一化
print(img_tensor[0][0][0])
trans_norm = transforms.Normalize([6,3,2],[3,2,1])
img_norm = trans_norm(img_tensor)
print(img_norm[0][0][0])
writer.add_image("Normalize",img_norm)

# resize
print(img.size)
trans_resize = transforms.Resize((512,512))
img_resize = trans_resize(img)
img_resize = trans_tensor(img_resize)
writer.add_image("Resize",img_resize,0)
print(img_resize)

# Compose
trans_resize_2 = transforms.Resize(512)
trans_compose = transforms.Compose([trans_resize_2,trans_tensor])
img_resize_2 = trans_compose(img)
writer.add_image("Resize",img_resize_2,1)

# RandomCrop 随机裁剪
trans_random = transforms.RandomCrop(512)
trans_compose_2 = transforms.Compose([trans_random,trans_tensor])
for i in range(10):
    img_crop = trans_compose_2(img)
    writer.add_image("Random",img_crop,i)
writer.close()
```

## 下载官网的数据集

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 13:44
# @Author: 阳洪
# @File:datasetTest.py
# @Software:PyCharm
import torchvision
from torch.utils.tensorboard import SummaryWriter
dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])
train_set = torchvision.datasets.CIFAR10(root="./datasets",train=True,transform=dataset_transform,download=True)
test_set = torchvision.datasets.CIFAR10(root="./datasets",train=False,transform=dataset_transform,download=True)
print(test_set[0])
print(test_set.classes) # label的数组
img,target = test_set[0]
print(img)
print(target)
print(test_set.classes[target])
writer = SummaryWriter("p10")
for i in range(10):
    img,target = test_set[i]
    writer.add_image("test_set",img,i)
writer.close()
```

## DataLoader的使用

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 14:19
# @Author: 阳洪
# @File:dataloaderTest.py
# @Software:PyCharm
import torchvision
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter
test_data = torchvision.datasets.CIFAR10('./datasets',train=False,transform=torchvision.transforms.ToTensor())
test_loader = DataLoader(dataset=test_data,batch_size=64,shuffle=True,num_workers=0,drop_last=True)
writer = SummaryWriter('dataloader')
step = 0
for data in test_loader:
    imgs,targets = data
    writer.add_images("dataLoader",imgs,step)
    step = step + 1
writer.close()
```

## 神经网络的基本骨架nn.Module的基本使用

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 14:36
# @Author: 阳洪
# @File:nn_Module.py
# @Software:PyCharm
import torch
from torch import nn
class Tudui(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self,input):
        output = input + 1
        return output

tudui = Tudui()
x = torch.tensor(1.0)
output = tudui(x)
print(output)
```

## 卷积操作

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 14:55
# @Author: 阳洪
# @File:nn_conv.py
# @Software:PyCharm
import torch
import torch.nn.functional as F
input = torch.tensor([[1,2,0,3,1],
                      [0,1,2,3,1],
                      [1,2,1,0,0],
                      [5,2,3,1,1],
                      [2,1,0,1,1]])
kernel = torch.tensor([[1,2,1],
                       [0,1,0],
                       [2,1,0]])
input = torch.reshape(input,(1,1,5,5))
kernel = torch.reshape(kernel,(1,1,3,3))
output = F.conv2d(input,kernel,stride=1)
print(output)
```

## 卷积层

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 15:19
# @Author: 阳洪
# @File:nn_conv2d.py
# @Software:PyCharm
import torch
import torchvision
from torch import nn
from torch.nn import Conv2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

dataset = torchvision.datasets.CIFAR10("./datasets",train=False,transform=torchvision.transforms.ToTensor(),
                                       download=True)
dataloader = DataLoader(dataset,batch_size=64)
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui,self).__init__()
        self.conv1 = Conv2d(in_channels=3,out_channels=6,kernel_size=3,stride=1,padding=0)

    def forward(self,x):
        x = self.conv1(x)
        return x
tudui = Tudui()
step = 0
writer = SummaryWriter("conv2")
for data in dataloader:
    imgs,targets = data
    output = tudui(imgs)
    print(imgs.shape)
    print(output.shape)
    output = torch.reshape(output,(-1,3,30,30))
    writer.add_images("input",imgs,step)
    writer.add_images("output",output,step)
    step = step + 1
```

## 最大池化

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-22 15:49
# @Author: 阳洪
# @File:maxpoolTest.py
# @Software:PyCharm
import torch
import torchvision
from torch import nn
from torch.nn import MaxPool2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

dataset = torchvision.datasets.CIFAR10("./datasets",train=False,transform=torchvision.transforms.ToTensor(),download=True)

dataloader = DataLoader(dataset,batch_size=64)
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui,self).__init__()
        self.maxpool1 = MaxPool2d(kernel_size=3,ceil_mode=False)

    def forward(self,input):
        output = self.maxpool1(input)
        return output

tudui = Tudui()
writer = SummaryWriter("maxpool")
step = 0
for data in dataloader:
    imgs,targets = data
    writer.add_images("input",imgs,step)
    output = tudui(imgs)
    writer.add_images("output", output, step)
    step = step + 1

writer.close()
```

## 非线性激活

```python
import torch
import torchvision
from torch import nn
from torch.nn import ReLU, Sigmoid
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

input = torch.tensor([[1, -0.5],
                      [-1, 3]])

input = torch.reshape(input, (-1, 1, 2, 2))
print(input.shape)

dataset = torchvision.datasets.CIFAR10("../data", train=False, download=True,
                                       transform=torchvision.transforms.ToTensor())

dataloader = DataLoader(dataset, batch_size=64)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.relu1 = ReLU()
        self.sigmoid1 = Sigmoid()

    def forward(self, input):
        output = self.sigmoid1(input)
        return output

tudui = Tudui()

writer = SummaryWriter("../logs_relu")
step = 0
for data in dataloader:
    imgs, targets = data
    writer.add_images("input", imgs, global_step=step)
    output = tudui(imgs)
    writer.add_images("output", output, step)
    step += 1

writer.close()
```

## 线性层

```python
# -*- coding=utf-8 -*-
# @Time:2021-09-23 10:53
# @Author: 阳洪
# @File:linearTest.py
# @Software:PyCharm
import torch
import torchvision
from torch import nn
from torch.nn import Linear
from torch.utils.data import DataLoader
dataset = torchvision.datasets.CIFAR10("datasets",train=False,transform=torchvision.transforms.ToTensor(),download=True)
dataloader = DataLoader(dataset,batch_size=64,drop_last=True)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.linear1 = Linear(196608,10)

    def forward(self,input):
        output = self.linear1(input)
        return output

tudui = Tudui()
for data in dataloader:
    imgs,targets = data
    print(imgs.shape)
    output = torch.flatten(imgs)
    print(output.shape)
    output = tudui(output)
    print(output.shape)
```

## Sequential的使用

```python
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.tensorboard i
mport SummaryWriter


class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

tudui = Tudui()
print(tudui)
input = torch.ones((64, 3, 32, 32))
output = tudui(input)
print(output.shape)

writer = SummaryWriter("../logs_seq")
writer.add_graph(tudui, input)
writer.close()
```

