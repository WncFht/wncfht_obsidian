---
url: https://blog.csdn.net/PolarisRisingWar/article/details/116069338
title: 60 分钟闪击速成 PyTorch（Deep Learning with PyTorch： A 60 Minute Blitz）学习笔记_deep-learning-with-pytorch step and step-CSDN 博客
date: 2024-10-08 23:24:26
tag: 
summary: 文章浏览阅读3w次，点赞176次，收藏953次。PyTorch官方教程：60分钟闪击速成PyTorch（Deep Learning with PyTorch: A 60 Minute Blitz）的学习笔记。_deep-learning-with-pytorch step and step
---

[诸神缄默不语 - 个人 CSDN 博文目录](https://blog.csdn.net/PolarisRisingWar/article/details/116396744)

本笔记是我学习 [Deep Learning with PyTorch: A 60 Minute Blitz](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) 这一 [PyTorch](https://so.csdn.net/so/search?q=PyTorch&spm=1001.2101.3001.7020) 官方教程后的学习笔记。  
该教程在官网上更新过，因此未来还可能继续更新。以后的读者所见的版本可能与我学的不同。  
以下将按照教程中的顺序撰写笔记：

1.  Tensor（张量）。
2.  torch.autograd（自动求导包）。
3.  Neural Networks（神经网络）。
4.  Training a Classifier（图片分类任务示例）。
5.  可选学习：Data Parallelism（数据并行）。

并最后整理教程中建议的衍生学习材料作为第 6 部分。

本文中不详细介绍代码相关的内容。对函数的解释建议翻阅 PyTorch 文档，也可参考我写的 [PyTorch Python API 详解大全（持续更新 ing…）_诸神缄默不语的博客 - CSDN 博客](https://blog.csdn.net/PolarisRisingWar/article/details/116070083) 一文。

文中所使用的 notebook 文件下载并修改自原教程。  
每一节都可以点击该图标下载 notebook 文件：  

![](https://i-blog.csdnimg.cn/blog_migrate/5f7dcc6421540567c66e560898b445d5.png#pic_center)

由于原教程 notebook 文件中部分 Markdown 内容显示有问题，因此我将我下载的 notebook 文件上传到了 GitHub 公开项目 [PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial: 60 分钟闪击速成 PyTorch（Deep Learning with PyTorch: A 60 Minute Blitz）相关文件](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial)（除数据平行部分，该部分直接上传了 py 后缀的代码脚本文件），这些 notebook 文件中的 Markdown 部分已经修改为了可以正常显示的形式（我是使用 VSCode 打开 notebook 文件，因此仅限于保证在 VSCode 中打开可以正常显示），并添加了一些我个人的学习笔记，可供参考。源代码与教程顺序的对应关系见后文。  
此外在该项目中还放了一个原教程中置于 colab 的 notebook 文件。详情见下文。

原教程的 notebook 文件基本就是网页内容本身。有条件的读者也可以直接从原教程网页跳转到 colab 运行代码，点击教程每页上方这个图标即可：

![](https://i-blog.csdnimg.cn/blog_migrate/93c75f8bd108743e88ec337cfe3449b3.png#pic_center)

  
以后如果有缘可能会撰写 colab 使用方面的笔记。

_建议读者提前学过线性代数和神经网络常识，会用 numpy，已经安装好 torch 和 torchvision 包_

#### 文章目录

*   [1. Tensor](#1_Tensor_29)
*   [2. Autograd](#2_Autograd_54)
*   [3. Neural Networks 3. 神经网络](#3_Neural_Networks_92)
*   [4. CIFAR10 (Example: Image Classification)  
    4. CIFAR10（示例：图像分类）](#4_CIFAR10_Example_Image_Classification_212)
*   *   [Step1：下载并规范化数据集](#Step1_221)
    *   [Step2：定义一个卷积神经网络](#Step2_225)
    *   [Step3：定义损失函数和优化器](#Step3_257)
    *   [Step4：训练神经网络](#Step4_266)
    *   [Step5：测试神经网络](#Step5_302)
    *   [在 GPU 上训练](#GPU_327)
*   [5. 多 GPU 数据并行训练](#5_GPU_336)
*   [6. 衍生学习资料](#6__369)
*   [7. 其他正文及脚注未提及的参考资料](#7__381)

在教程首页有一个 YouTube 链接的视频，这只是一个两分钟的简介，没有干货，如果没有条件使用 YouTube 的读者也不用刻意去看。

## 1. Tensor

教程 notebook：[https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/tensor_tutorial.ipynb](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/tensor_tutorial.ipynb)

1.  什么是 Tensor？  
    torch 中的 Tensor 是一种数据结构，其实在使用上与 Python 的 list、numpy 的 array、ndarray 等数据结构比较类似，可以当成一个多维数组来用。  
    在数学上对张量这一专业名词有特定的定义，但是反正大概理解成一个多维数组就够用了。
2.  如何生成 Tensor？  
    torch 包中提供了一系列**直接**生成 Tensor 的函数，如 `zeros()`、`ones()`、`rand()` 等。  
    此外，可以用 `tensor(data)` 函数直接将**某一表示数组的数据**（接受`list`、`numpy.ndarray`等格式）转换为 Tensor。  
    也可以通过 `from_numpy(data)` 函数将 **numpy.ndarray** 格式的数据转换为 Tensor。  
    还可以生成一个与**其他 Tensor** 具有相同 dtype 和 device 等属性的 Tensor，使用 torch 的 `ones_like(data)` 或 `rand_like(data)` 等函数，或 Tensor 的 `new_ones()` 等函数。
3.  Tensor 的属性：shape（返回 torch.Size 格式）（也可以用`size()`函数），dtype，device（可见 [PyTorch Python API 详解大全（持续更新 ing…）_诸神缄默不语的博客 - CSDN 博客](https://blog.csdn.net/PolarisRisingWar/article/details/116070083)第 0 节的相关介绍）
4.  Tensor 可以进行的操作：类似 numpy 的 API；改变原数据的原地操作在函数后面加`_`就可以（一般不建议这么操作）
    1.  索引
    2.  切片
    3.  join：`cat(tensors)`或`stack(tensors)`  
        join：`cat（Tensors）`或`stack（Tensors）`
    4.  加法：`add()`或`+`
    5.  乘法：对元素层面的乘法`mul()`或`*`，矩阵乘法`matmul()`或`@`
    6.  resize
        1.  `reshape()`或`view()`（建议使用`reshape()`，因为仅使用`view()`可能会造成 Tensor 不 contiguous 的问题，可参考 [PyTorch Python API 详解大全（持续更新 ing…）_诸神缄默不语的博客 - CSDN 博客](https://blog.csdn.net/PolarisRisingWar/article/details/116070083)一文脚注 3 的介绍）
        2.  `squeeze()`去掉长度为 1 的维度
        3.  `unsqueeze()`增加一个维度（长度为 1）
        4.  `transpose()`转置 2 个维度
5.  `Tensor.numpy()`可以将 Tensor 转换为 numpy 数据。反向的操作见上面序号 2 部分。  
    注意这两方向的转换的数据对象都是占用同一储存空间，修改后变化也会体现在另一对象上。
6.  `item()`函数返回仅有一个元素的 Tensor 的该元素值。

## 2. Autograd

教程 notebook：[https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/autograd_tutorial.ipynb](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/autograd_tutorial.ipynb)

1.  torch.autograd 是 PyTorch 提供的自动求导包，非常好用，可以不用自己算神经网络偏导了。
2.  神经网络构成、常识部分这里就不再详细介绍了，总之大概就是：
    1.  神经网络由权重、偏置等参数决定的函数构成，这些参数在 PyTorch 中都储存在 Tensor 里
    2.  神经网络的训练包括**前向传播**和**反向传播**两部分，前向传播就是用函数计算预测值，反向传播就是通过这一预测值产生的 error/loss 来更新参数（通过梯度下降的方式）  
        对反向传播算法的介绍，教程中提供了 3b1b 的视频作为参考。原链接是 YouTube 视频，不方便的读者可以看 B 站上面的：[【官方双语】深度学习之反向传播算法 上 / 下 Part 3 ver 0.9 beta  下篇：反向传播的微积分原理](https://www.bilibili.com/video/BV16x411V7Qg?p=2)  
        （对上一视频所属的 3B1B 深度学习视频系列，我也撰写了学习笔记，可参考：[3B1B 深度学习系列视频学习笔记_诸神缄默不语的博客 - CSDN 博客](https://blog.csdn.net/PolarisRisingWar/article/details/117168680) ）
3.  神经网络的一轮训练：
    1.  前向传播：`prediction = model(data)`
    2.  反向传播
        1.  计算 loss
        2.  `loss.backward()`（autograd 会在这一步计算参数的梯度，存在相应参数 Tensor 的 grad 属性中）
        3.  更新参数
            1.  加载 optimizer（通过 torch.optim）
            2.  `optimizer.step()`对参数使用梯度下降的方法进行更新（梯度来源自参数的 grad 属性）

本节以下内容都属于原理部分，可以直接跳过

4.  autograd 实现细节：一个示例
    1.  将 Tensor 的 requires_grad 属性设置为 True，可以追踪 autograd 在其上每一步的操作
    2.  示例中，提供了两个 requires_grad 为 True 的 Tensor（含两个元素的向量）a 和 b，设其损失函数 Q = 3 a 3 − b 2 Q = 3a^3 - b^2 Q=3a3−b2
    3.  注意：对 Q 计算梯度时，需要在`backward()`函数中添加 gradient 参数，这个 gradient 是和当前 Tensor 形状相同的 Tensor，包含当前 Tensor 的梯度，比如示例中使用的是： d Q d Q = 1 \frac{dQ}{dQ} = 1 dQdQ​=1（因为 Q 是向量而非标量，参考`backward()`的[文档](https://pytorch.org/docs/stable/autograd.html#torch.Tensor.backward)。为了避免这个问题也可以直接将 Q 转化为标量然后使用`backward()`方法，如`Q.sum().backward()`）
    4.  计算梯度：  
        `external_grad = torch.tensor([1., 1.])`  
        `Q.backward(gradient=external_grad)`
    5.  现在 Q 相对于 a 和 b 的梯度向量就分别储存在了 a.grad 和 b.grad 中，可以直接查看
5.  教程中提供了 aotugrad 矢量分析方面的解释，我没看懂，以后学了矢量分析看懂了再说。
6.  autograd 的计算图
    1.  autograd 维护一个由 [Function 对象](https://pytorch.org/docs/stable/autograd.html#torch.autograd.Function)组成的 DAG 中的所有数据和操作。这个 DAG 是以输入向量为叶，输出向量为根。autograd 从根溯叶计算梯度
    2.  在前向传播时，autograd 同时干两件事：计算输出向量，维护 DAG 中操作的 gradient function
    3.  反向传播以根节点调用`backward()`方法作为开始，autograd 做以下三件事：用数据的 grad_fn 属性计算梯度，将梯度分别加总累积到各 Tensor 的 grad 属性中，根据链式法则传播到叶节点
    4.  如图，前序号 4 部分示例 Q = 3 a 3 − b 2 Q = 3a^3 - b^2 Q=3a3−b2 的 DAG（箭头是前向传播的方向，节点是前向传播过程中每个操作的 backward functions，蓝色的叶节点是 a 和 b）  
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/608b68b9016f7f083f6e5fc8bd4c9312.png#pic_center)
        
    5.  注意：PyTorch 中的 DAG 是动态的，每次调用`backward()`方法都重新填出一个 DAG
7.  将 Tensor 的 requires_grad 属性设置为 False，可以将其排除在 DAG 之外，autograd 就不会计算它的梯度。  
    在神经网络中，这种不需要计算梯度的参数叫 frozen parameters。可以冻结不需要知道梯度的参数（节省计算资源），也可以在微调预训练模型时使用（此时往往冻结绝大多数参数，仅调整 classifier layer 参数，以在新标签上做预测）。  
    类似功能也可以用上下文管理器`torch.no_grad()`实现。

## 3. Neural Networks

教程 notebook：[https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/neural_networks_tutorial.ipynb](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/neural_networks_tutorial.ipynb)

1.  神经网络可以通过 torch.nn 包搭建（torch.nn 包里预定义的层调用了 torch.nn.functional 包的函数）
2.  nn.Module 包含了网络层
3.  `forward(input)`方法返回输出结果
4.  示例：简单前馈神经网络 convnet  
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a90442307a08eb1c843e51ad9c32b870.png#pic_center)
    
5.  典型的神经网络训练流程：（从下一序号开始对每一部分进行详细介绍）
    1.  定义具有可训练参数（或权重）的神经网络
    2.  用数据集进行多次迭代
        1.  前向传播
        2.  计算 loss
        3.  计算梯度
        4.  使用梯度下降法更新参数
6.  **定义网络**  
    只需要定义`forward()`方法，`backward()`方法会自动定义（因为用了 autograd）。在`forward()`方法中可以进行任何 Tensor 操作。  
    本部分代码定义了一个卷积→池化→卷积→池化→仿射变换→仿射变换→仿射变换的叠叠乐网络。  
    （这个网络我有一点没搞懂，就是仿射变换前一步，既然已知数据维度是 16*6*6，为什么还要用`num_flat_features()`这个方法算一遍啊……？）

```
import torch
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):

    def __init__(self):
        super(Net, self).__init__()
        #Conv2d是在输入信号（由几个平面图像构成）上应用2维卷积
        # 1 input image channel, 6 output channels, 3x3 square convolution kernel
        self.conv1 = nn.Conv2d(1, 6, 3)
        self.conv2 = nn.Conv2d(6, 16, 3)
        # an affine operation: y = Wx + b
        #affine仿射的
        self.fc1 = nn.Linear(16 * 6 * 6, 120)
        #16是conv2的输出通道数，6*6是图像维度
        #（32*32的原图，经conv1卷后是6*30*30，经池化后是6*15*15，经conv2卷后是16*13*13，经池化后是16*6*6）
        #经过网络层后的维度数计算方式都可以看网络的类的文档来查到
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        # Max pooling over a (2, 2) window
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        # If the size is a square, you can specify with a single number
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        x = x.view(-1, self.num_flat_features(x))
        #将x转化为元素不变，尺寸为[-1,self.num_flat_features(x)]的Tensor
        #-1的维度具体是多少，是根据另一维度计算出来的
        #由于另一维度是x全部特征的长度，所以这一步就是把x从三维张量拉成一维向量
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

    def num_flat_features(self, x):
    #计算得到x的特征总数（就是把各维度乘起来）
        size = x.size()[1:]  # all dimensions except the batch dimension
        num_features = 1
        for s in size:
            num_features *= s
        return num_features

net = Net()
print(net)

```

输出：

```
Net(
  (conv1): Conv2d(1, 6, kernel_size=(3, 3), stride=(1, 1))
  (conv2): Conv2d(6, 16, kernel_size=(3, 3), stride=(1, 1))
  (fc1): Linear(in_features=576, out_features=120, bias=True)
  (fc2): Linear(in_features=120, out_features=84, bias=True)
  (fc3): Linear(in_features=84, out_features=10, bias=True)
)

```

模型的可学习参数存储在`net.parameters()`中。这个方法的返回值是一个迭代器，包含了模型及其所有子模型的参数

7.  **前向传播**：`out = net(input)`
8.  **反向传播**：先将参数梯度缓冲池清零（否则梯度会累加），再反向传播（此处使用一个随机矩阵）  
    `net.zero_grad()`[1](#fn1)  
    `out.backward(torch.randn(1, 10))`  
    如果有计算出损失函数，上一行代码应为：`loss.backward()`
9.  注意：torch.nn 只支持 mini-batch，所以如果只有一个输入数据的话，可以用`input.unsqueeze(0)`方法创造一个伪 batch 维度
10.  **损失函数**  
    torch.nn 包中定义的损失函数文档：[https://pytorch.org/docs/nn.html#loss-functions](https://pytorch.org/docs/nn.html#loss-functions)  
    以 MSELoss 为例：  
    `criterion = nn.MSELoss()`  
    `loss = criterion(output, target)`

对如此得到的 loss，其 grad_fn 组成的 DAG 为：  

![](https://i-blog.csdnimg.cn/blog_migrate/e43f322fb061d4c5ba9c467fd3f00404.png#pic_center)

  
所以，调用`loss.backward()`后，所有张量的梯度都会得到更新  
直观举例：

```
print(loss.grad_fn)  # MSELoss
print(loss.grad_fn.next_functions[0][0])  # Linear
print(loss.grad_fn.next_functions[0][0].next_functions[0][0])  # ReLU

```

输出：

<MseLossBackward object at 0x0000015F84F7E388>  
<AddmmBackward object at 0x0000015FFFF98E48>  
<AccumulateGrad object at 0x0000015F84F7E388>

11.  **更新网络中的权重**：`step()`  
    使用 torch.optim 中的优化器（`lr`入参是学习率，这个学习率也可以通过 torch.optim.lr_scheduler 包实现 learning rate scheduling 操作 [2](#fn2)）

```
import torch.optim as optim

# create your optimizer
optimizer = optim.SGD(net.parameters(), lr=0.01)

# in your training loop:
optimizer.zero_grad()   #原因见前
output = net(input)
loss = criterion(output, target)
loss.backward()
optimizer.step()    # Does the update

```

## 4. CIFAR10 (Example: Image Classification)

教程 notebook：[https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/cifar10_tutorial.ipynb](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/cifar10_tutorial.ipynb)

1.  各种形式的数据都可以通过 Python 标准库转换为 numpy 数组格式，然后再转换为 Tensor 格式
    1.  图像：Pillow, OpenCV
    2.  音频：scipy and librosa
    3.  文本：raw Python or Cython based loading, or NLTK and SpaCy
2.  对计算机视觉任务，PyTorch 有专门的包 torchvision，可以直接通过`torchvision.datasets`和`torch.utils.data.DataLoader`下载 Imagenet, CIFAR10, MNIST 等常用数据集并对其进行数据转换
3.  在本教程中使用的是 CIFAR10。图片是 3 通道，大小为 32*32。标签为图像类别（共 10 类）  
      
    

### Step1：下载并规范化数据集

通过`torch.utils.data.DataLoader`加载`torchvision.datasets`中的数据集，返回迭代器  
使用 torchvision.transforms 包进行规范化  
  

### Step2：定义一个卷积神经网络

这个神经网络和第 3 部分神经网络里的模型相似，只是将数据维度做了修改。  
这里的数据特征尺寸在网络层之间的变化是 3 ∗ 32 ∗ 32 → ( c o n v 1 ) 6 ∗ 28 ∗ 28 → ( p o o l ) 6 ∗ 14 ∗ 14 → ( c o n v 2 ) 16 ∗ 10 ∗ 10 → ( p o o l ) 16 ∗ 5 ∗ 5 → ( f c 1 ) 120 → ( f c 2 ) 84 → ( f c 3 ) 10 3*32*32\xrightarrow{(conv1)}6*28*28\xrightarrow{(pool)}6*14*14\xrightarrow{(conv2)}16*10*10\xrightarrow{(pool)}16*5*5\xrightarrow{(fc1)}120\xrightarrow{(fc2)}84\xrightarrow{(fc3)}10 3∗32∗32(conv1) ​6∗28∗28(pool) ​6∗14∗14(conv2) ​16∗10∗10(pool) ​16∗5∗5(fc1) ​120(fc2) ​84(fc3) ​10

```
import torch.nn as nn
import torch.nn.functional as F


class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x


net = Net()

```

### Step3：定义损失函数和优化器

```
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)

```

### Step4：训练神经网络

```
for epoch in range(2):  # loop over the dataset multiple times

    running_loss = 0.0
    for i, data in enumerate(trainloader, 0):
        # get the inputs; data is a list of [inputs, labels]
        inputs, labels = data

        # zero the parameter gradients
        optimizer.zero_grad()

        # forward + backward + optimize
        outputs = net(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # print statistics
        running_loss += loss.item()  
        #loss.item()文档：https://pytorch.org/docs/stable/tensors.html?highlight=item#torch.Tensor.item
        #Returns the value of this tensor as a standard Python number.
        if i % 2000 == 1999:    # print every 2000 mini-batches
            print('[%d, %5d] loss: %.3f' %
                  (epoch + 1, i + 1, running_loss / 2000))
            running_loss = 0.0

print('Finished Training')

```

将模型保存到本地：

```
PATH = './cifar_net.pth'
torch.save(net.state_dict(), PATH)

```

对模型存取的更多细节详见：[SERIALIZATION SEMANTICS](https://pytorch.org/docs/stable/notes/serialization.html)  
  

### Step5：测试神经网络

加载模型文件：

```
net = Net()
net.load_state_dict(torch.load(PATH))

```

用测试集输出向量中最大的元素代表的类作为输出

```
correct = 0
total = 0
with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predicted = torch.max(outputs.data, 1)
        total += labels.size(0)
        correct += (predicted == labels).sum().item()

print('Accuracy of the network on the 10000 test images: %d %%' % (
    100 * correct / total))

```

### 在 GPU 上训练

```
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

net.to(device)
inputs, labels = data[0].to(device), data[1].to(device)  #把每一步的输入数据和目标数据也转到GPU上

```

注意，直接调用 `my_tensor.to(device)` 将返回一个在 GPU 上的 `my_tensor` 的副本而不是直接重写 `my_tensor`，因此在后续训练的过程中需要将其赋予一个新 Tensor，然后用新 Tensor 来训练。  
  

## 5. 多 GPU 数据并行训练

原教程：[DATA PARALLELISM](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html)

代码：[Note-of-PyTorch-60-Minutes-Tutorial/dp.py at master · PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/dp.py)

这个教程主要讲如何使用 `DataParallel` 这个类（简称 DP）。文档：[DataParallel — PyTorch 1.10 documentation](https://pytorch.org/docs/stable/generated/torch.nn.DataParallel.html)  
PyTorch 常用的另一个多卡训练的类是 `DistributedDataParallel`（简称 DDP。文档：[DistributedDataParallel — PyTorch 1.10.0 documentation](https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html)）。那个类怎么用我还没搞懂，我就先把这个 `DataParallel` 搞懂了来写一写……

核心代码：

```
model = nn.DataParallel(model)

```

在单卡上写好的 model 直接调用这个类，然后别的都跟单卡形式下的一样就可以了。程序会自动把数据拆分放到所有已知的 GPU 上来运行。  
看我在 GitHub 上写的代码，数据是直接从第一维拆开平均放到各个 GPU 上，相当于每个 GPU 放 `batch_size/卡数` 个样本。

设置已知的 GPU，可以在运行代码的 `python` 加上 `CUDA_VISIBLE_DEVICES` 参数，举例：

```
CUDA_VISIBLE_DEVICES=0,1,2,3 python example.py

```

注意如果要使用 nohup 的话，这个参数要加在 nohup 的还前面，举例：

```
CUDA_VISIBLE_DEVICES=0,1,2,3 nohup python -u example.py >> nohup_output.log 2>&1 &

```

如果不设置则默认为所有 GPU。

对 GPU 数量的计数可以使用 `torch.cuda.device_count()` 代码。

原理我还没怎么搞懂，但是据说直接用 `DataParallel` 不太好，有各卡空间不均衡之类的问题，建议使用 `DistributedDataParallel`。我学会那个类的使用方法以后大约也会写篇笔记博文的。

其他多卡运行 PyTorch 模型的资料可参考：

1.  [PyTorch 分布式训练简介](https://pytorch.org/tutorials/beginner/dist_overview.html)
2.  [Distributed communication package - torch.distributed — PyTorch 1.10.0 documentation](https://pytorch.org/docs/stable/distributed.html)

## 6. 衍生学习资料

1.  [微调 torchvision 模型教程](https://pytorch.org/tutorials/beginner/finetuning_torchvision_models_tutorial.html)
2.  [autograd 具体机制](https://pytorch.org/docs/stable/notes/autograd.html)
3.  [逆向自动求导法应用实例 colab 版](https://colab.research.google.com/drive/1VpeE6UvEPRz9HmsHh1KS0XxXjYu533EC) 由于众所周知的有些读者可能无法登入 colab，因此我也下载了原 notebook 文件放在了 GitHub 公开项目上供便捷下载，网址：[https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/Simple_Grad.ipynb](https://github.com/PolarisRisingWar/Note-of-PyTorch-60-Minutes-Tutorial/blob/master/Simple_Grad.ipynb)
4.  [训练神经网络玩视频游戏 REINFORCEMENT LEARNING (DQN) TUTORIAL](https://pytorch.org/tutorials/intermediate/reinforcement_q_learning.html)
5.  [在 ImageNet 数据集上训练 ResNet ImageNet training in PyTorch](https://github.com/pytorch/examples/tree/master/imagenet)
6.  [用 GAN 生成人脸 Deep Convolution Generative Adversarial Networks](https://github.com/pytorch/examples/tree/master/dcgan)
7.  [用 Recurrent LSTM networks 训练一个词级别的语言模型 Word-level language modeling RNN](https://github.com/pytorch/examples/tree/master/word_language_model)
8.  [更多 PyTorch 应用示例](https://github.com/pytorch/examples)
9.  [更多 PyTorch 教程](https://github.com/pytorch/tutorials)
10.  [在论坛上讨论 PyTorch](https://discuss.pytorch.org/)
11.  [在 Slack 上与其他 PyTorch 学习者交流](https://pytorch.slack.com/messages/beginner/)

## 7. 其他正文及脚注未提及的参考资料

1.  [pytorch tutorial : A 60 Minute Blitz_Gitabytes 的博客 - CSDN 博客](https://blog.csdn.net/mumei_eli/article/details/101123853)：这一篇在原教程的基础上还有所衍生，比如讲了个用 torch.optim.lr_scheduler.ExponentialLR 的代码例子。
2.  [torch.nn.Module.zero_grad() 的使用_敲代码的小风的博客 - CSDN 博客_torch zero_grad](https://blog.csdn.net/m0_46653437/article/details/112587642)：这一篇用代码演示了一下 PyTorch 的梯度积累和清零的过程。
3.  [optimizer.zero_grad() 和 net.zero_grad()_前进 ing_嘟嘟的博客 - CSDN 博客](https://blog.csdn.net/wendygelin/article/details/123332622)

1.  注意这个`zero_grad()`方法在此处是用在了 net（一个网络（nn.Module 子类）实例）上，后文的`zero_grad()`方法则是用在了 optimizer（优化器）上。  
    前者的文档见 [https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.zero_grad](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module.zero_grad)，是将其所有参数梯度清零。  
    后者的文档见 [https://pytorch.org/docs/stable/optim.html#torch.optim.Optimizer.zero_grad](https://pytorch.org/docs/stable/optim.html#torch.optim.Optimizer.zero_grad)，是将优化器上所有参数梯度清零。  
    注意到我们往优化器中传的就是这个网络的所有参数：`optimizer = optim.SGD(net.parameters(), lr=0.01)`，所以我觉得这两种写法应该是一样的（因为`model.parameters()`返回一个 Tensor 的迭代器，Tensor 作为一个可变 object 应该是直接传入引用，所以应该一样）。但是我还没有试验过，如果有闲情逸致的话可以试试。  
    另，如果有 frozen parameters，或者优化器只优化一部分模型中的参数，可能不等价。这一部分我也还没有试验过。  
    此外参考[为什么要使用 zero_grad()？_马鹏森的博客 - CSDN 博客_net.zero_grad()](https://mapengsen.blog.csdn.net/article/details/115188863) 一文，还可以实现对特定 Variable 梯度清零：`Variable.grad.data.zero_()` [↩︎](#fnref1)
    
2.  这个东西我会在写李宏毅机器学习笔记的时候细讲的。 [↩︎](#fnref2)