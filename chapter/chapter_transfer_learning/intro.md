# 微调

数据标注是一个很贵的事情，我们希望在经过大量的数据训练以后。我们的模型具备了一定的学习能力，在以后只需要给一点点的提示就能够学会一个新的事物。就是迁移学习的概念，名字不一样而已。[1]

（1）网络架构

一个神经网络可以分为两块

特征收取将原始像素变为能够线性分割的特征
线性分类器来做分类

那么微调是怎么做的呢？

有一个神经网络net，它的训练来自于A数据集，而我的项目是B数据集。我希望能够net经过微小的调整之后，也能够用在B数据集上。也就是特征提取的那一部分仍起作用，只是说分类层需要重新训练。

训练：
是一个目标数据集上的正常任务，但是用更强的正则化：

- 使用更小的学习率
- 使用更少的数据迭代
- 如果源数据及远复杂于目标数据集，通常微调的效果是不错的。
- 重用分类器权重：
- 前面说分类层需要重新训练，但是如果源数据集有和目标数据集中相同的标号，也可以把以前的分类器的对应标号权重拿出来做初始化，其余的随机初始化。

固定一些层：

神经网络通常在浅层学习的都是一些比较通用的特征，而高层学习的是更加细节的特征。所以我们可以将浅层的权重固定，只训练高层的神经网络。这样的话，也就意味着神经网络变小了，收敛自然也就越快了。

（2）总结

- 微调通过使用已经训练好的大数据模型来初始化权重来提升精度。
- 预训练的质量是很重要的。
- 微调通常速度更快，精度更高。

（二）代码实现

这里用到的一个数据集是作者自己找的一个数据集，叫做热狗集。

下载地址：http://d2l-data.s3-accelerate.amazonaws.com/hotdog.zip

下载好，之后解压到你放数据的文件夹里

```py
%matplotlib inline
import torch
from torch import nn
from d2l import torch as d2l
import torchvision
import matplotlib.image as img
import matplotlib.pyplot as plt
```

```py
train_imgs = torchvision.datasets.ImageFolder(root="../data/hotdog/train/")
test_imgs = torchvision.datasets.ImageFolder(root="../data/hotdog/test/")
```

```py
# 前面1000张是热狗，后面1000张是非热狗
img1 = img.imread(train_imgs.imgs[0][0])
img2 = img.imread(train_imgs.imgs[1000][0])
plt.imshow(img1)
plt.show()
plt.imshow(img2)
plt.show()

hotdogs = [train_imgs[i][0] for i in range(8)]
not_hotdogs = [train_imgs[-i - 1][0] for i in range(8)]
d2l.show_images(hotdogs + not_hotdogs, 2, 8, scale=1.4)

# 我们微调的对象是ResNet18,它的源数据集是imagenet，
# 使用RGB通道的均值和标准差，以标准化每个通道。（resnet18在imagenet上也是这么做的）
normalize = torchvision.transforms.Normalize(
    mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])

train_augs = torchvision.transforms.Compose([
    torchvision.transforms.RandomResizedCrop(224), # 随即裁剪
    torchvision.transforms.RandomHorizontalFlip(), # 随机水平翻转
    torchvision.transforms.ToTensor(),
    normalize])

test_augs = torchvision.transforms.Compose([
    torchvision.transforms.Resize(256), # 根据宽高按比例缩小
    torchvision.transforms.CenterCrop(224), # 居中裁剪
    torchvision.transforms.ToTensor(),
    normalize])
# 定义模型，参数pretrain就是加载模型参数
# 如果在jupyter上运行报错，可以到命令行里面先下载就行了
finetune_net = torchvision.models.resnet18(pretrained=True)
# print(finetune_net)
# print(finetune_net.fc)

# 将最后一层分类层替换掉，并初始化权重
finetune_net.fc = nn.Linear(finetune_net.fc.in_features, 2)
nn.init.xavier_uniform_(finetune_net.fc.weight)
```

```py
# 如果param_group=True，输出层中的模型参数将使用十倍的学习率
# 因为前面说过了，在特征提取层，我们希望学习率不那么大，但是分类器是全新的，所以我们需要大的learning rate进行快速收敛

def train_fine_tuning(net, learning_rate, batch_size=128, num_epochs=5, param_group=True):
    # 获取数据集和测试集
    train_iter = torch.utils.data.DataLoader(torchvision.datasets.ImageFolder(root="../data/hotdog/train/",transform=train_augs),batch_size=batch_size,num_workers=0,shuffle=True)
    test_iter = torch.utils.data.DataLoader(torchvision.datasets.ImageFolder(root="../data/hotdog/test/",transform=test_augs),batch_size=batch_size,num_workers=0,shuffle=True)
    # 配置参数
    devices = d2l.try_all_gpus()
    loss = nn.CrossEntropyLoss(reduction="none")
    if param_group:
        # 网络微调用较小的学习率，分类器用较大的学习率
        params_1x = [param for name, param in net.named_parameters() if name not in ["fc.weight", "fc.bias"]]
        trainer = torch.optim.SGD([
                {"params":params_1x},
                {"params":net.fc.parameters(),"lr":10*learning_rate},
            ],lr=learning_rate, weight_decay=0.001)
    else:
        # 普通的优化器
        trainer = torch.optim.SGD(net.parameters(),lr=learning_rate,weight_decay=0.01)
    d2l.train_ch13(net, train_iter, test_iter, loss, trainer, num_epochs,
                   devices)
```

```py
train_fine_tuning(finetune_net, 5e-5)
```

[1]: https://www.jianshu.com/p/7c9efbdae1ff
