---

title: "动手学深度学习Pytorch(7章)"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false

---

这一章主要介绍了一些现代神经网络
在这一部分，我们将学习到很多现代卷积神经网络的结构，而在这个过程中，输入层和输出层的张量的维度是十分重要的，我们仍然给出计算公式：
$$
\mathbf{W}_{out} = \lfloor\frac{\mathbf{W}_{in}+2\cdot padding-kernel size}{stride}\rfloor +1
$$
## 使用nn.Sequential和class name(nn.Module)的区别

| 方式                   | 描述             | 优点            | 缺点             | 适用场景                               |
| -------------------- | -------------- | ------------- | -------------- | ---------------------------------- |
| `nn.Sequential`      | 顺序堆叠层          | 简洁、易写         | 不灵活，不能写分支逻辑、跳连 | 简单前馈网络，如 LeNet、MLP、AlexNet         |
| 自定义类（继承 `nn.Module`） | 实现 `forward()` | 极度灵活、能写任意前向逻辑 | 代码稍复杂          | 几乎所有中高级模型，如 ResNet、Transformer、RNN |
## AlexNet
2012年，AlexNet的问世首次证明了，学习到的特征可以超过手工设计的特征。
相较于LeNet， AlexNet要深很多，AlexNet由八层组成：五个卷积层、两个全连接隐藏层和一个全连接输出层。
另，AlexNet使用我们最喜欢的ReLU作为激活函数。
### 架构：
```python
import torch  
from torch import nn  
  
net = nn.Sequential(  
    nn.Conv2d(1, 96, kernel_size= 11, stride=4,padding=1),nn.ReLU(),  
    nn.MaxPool2d(kernel_size=3, stride=2),  
    nn.Conv2d(96, 256, kernel_size=5, padding=2),nn.ReLU(),  
    nn.MaxPool2d(kernel_size=3, stride=2),  
    nn.Conv2d(256, 384, kernel_size=3, padding=1), nn.ReLU(),  
    nn.MaxPool2d(kernel_size=3, stride=2),  
    nn.Conv2d(256, 384,kernel_size=3, padding=1),nn.ReLU(),  
    nn.Conv2d(384, 384, kernel_size=3, padding=1), nn.ReLU(),  
    nn.Conv2d(384, 256, kernel_size=3, padding=1), nn.ReLU(),  
    nn.MaxPool2d(kernel_size=3, stride=2),  
    nn.Flatten(),  
    nn.Linear(6400, 4096), nn.ReLU(),  
    nn.Dropout(p=0.5),  
    nn.Linear(4096, 4096),nn.ReLU(),  
    nn.Dropout(p=0.5),  
    nn.Linear(4096, 10)  
)
```
我们一直在变换通道数：
我们增加通道数，是为了：
1. 提取更丰富的特征；
2. 在压缩空间尺寸的同时维持甚至增强表示能力；
3. 使网络能识别更复杂、更抽象的内容
另外，对于AlexNet，我们采用暂退法防止过拟合


## VGG
核心思想就是使用小卷积代替大卷积
```python
def vgg_block(num_convs, in_channels, out_channels):
	layers = []
	for _ in range(num_convs):
		layers.append(nn.Conv2d(in_channels, out_channels,
								kernel_size=3, padding=1))
		layers.append(nn.ReLU())
		in_channels = out_channels
	layers.append(nn.MaxPool2d(kernel_size=2,stride=2))
	return nn.Sequential(*layers)
```
从代码很好看出来VGG块就是若干个卷积层和ReLU的堆叠，且这之中不涉及通道数变化，最终加入一个最大池化。
对于VGG神经网络，我们有一个超参数`conv_arch`定义每个VGG块的卷积层个数和输出通道数。
```python
conv_arch = ((1,64),(1,128),(2,256),(2,512),(2,512))
```
以此为例，我们实现一个VGG-11：
```python
def vgg(conv_arch):
	conv_blks = []
	in_channels = 1
	for(num_convs, out_channels) in conv_arch:
		conv_blks.append(vgg_block(num_convs, in_channels, out_channels))
		in_channels = out_channels

	return nn.Sequential(
		*conv_blks, nn.Flatten(),
		nn.Linear(out_channels*7*7,4096),nn.ReLU(),nn.Dropout(0.5),
		nn.Linear(4096, 4096), nn.ReLU(), nn.Dropout(0.5),
		nn.Linear(4096, 10))
net = vgg(conv_arch)
```

## NiN
在每个像素的通道上使用MLP
```python

def nin_block(in_channels, kernel_size, strides, padding):
	return nn.Sequential(
		nn.Conv2d(in_channels, out_channels, kernel_size, strides, padding),
		nn.ReLU(),
		nn.Conv2d(out_channels, out_channels, kernel_size=1),nn.ReLU(),
		nn.Conv2d(out_channels, out_channels, kernel_size=1), nn.ReLU()
		)
```
构建一个NiN：
```python
net = nn.Sequential(
	nin_block(1, 96, kernel_size=11, strides=4, padding=0),
	nn.MaxPool2d(3, stride=2),
	nin_block(96, 256, kernel_size=5, strides=1, padding=2),
	nn.MaxPool2d(3, stride=2),
	nin_block(256, 384, kernel_size=3, strides=1, padding=1),
	nn.MaxPool2d(3, stride=2),
	nn.Dropout(0.5),
	nin_block(384, 10, kernel_size=3, strides=1, padding=1),
	nn.AdaptiveAvgPool2d((1, 1)),
	
nn.Flatten())
```
并且，NiN最终并没有全连接层，这是与其他形式的卷积神经网络不同之处，他只需要一个全局平均汇聚层。

## GoogleNet
### Inception块
这是一个非常复杂的结构：
![[Inception Block.png]]
```python
class Inception(nn.Module):
	def __init__(self, in_channels, c1, c2, c3, c4, **kwargs):
		super(Inception, self).__init__(**kwargs)
		self.p1_1 = nn.Conv2d(in_channels, c1, kernel_size=1)
		self.p2_1 = nn.Conv2d(in_channels, c2[0],kernel_size=1)
		self.p2_2 = nn.Conv2d(c2[0], c2[1], kernel_size=1)
		self.p3_1 = nn.Conv2d(in_channels, c3[0], kernel_size=1)
		self.p3_2 = nn.Conv2d(c3[0], c3[1],kernel_size=5, padding=2)
		self.p4_1 = nn.MaxPool2d(kernel_size=3, stride=1, padding=1)
		self.p4_2 = nn.Conv2d(c4[0], c4[1], kernel_size=1)
	
	def forward(self,x):
		p1 = F.relu(self.p1_1(x))
		p2 = F.relu(self.p2_2(F.relu(self.p2_1(x))))
		p3 = F.relu(self.p3_2(F.relu(self.p3_1(x))))
		p4 = F.relu(self.p4_2(self.p4_1(x)))

		return torch.cat((p1, p2, p3, p4), dim = 1)
```
![[googlenet all.png]]
## 批量规范化(Batch Normalization)
可以持续加深神经网络的收敛速度
请注意，如果我们尝试使用大小为1的小批量应用批量规范化，我们将无法学到任何东西。这是因为在减去均值之后，每个隐藏单元将为0。所以，只有使用足够大的小批量，批量规范化这种方法才是有效且稳定的。请注意，在应用批量规范化时，批量大小的选择可能比没有批量规范化时更重要。
1.  **批量归一化核心运算 (BN):**
    $$
    \mathrm{BN}(\mathbf{x})=\boldsymbol{\gamma} \odot \frac{\mathbf{x}-\hat{\boldsymbol{\mu}}_{\mathcal{B}}}{\hat{\boldsymbol{\sigma}}_{\mathcal{B}}}+\boldsymbol{\beta}
    $$

2.  **小批量样本均值 ($\hat{\boldsymbol{\mu}}_{\mathcal{B}}$):**
    $$
    \hat{\boldsymbol{\mu}}_{\mathcal{B}}=\frac{1}{|\mathcal{B}|} \sum_{\mathbf{x} \in \mathcal{B}} \mathbf{x}
    $$

3.  **小批量样本方差 ($\hat{\boldsymbol{\sigma}}_{\mathcal{B}}^{2}$):**
    $$
    \hat{\boldsymbol{\sigma}}_{\mathcal{B}}^{2}=\frac{1}{|\mathcal{B}|} \sum_{\mathbf{x} \in \mathcal{B}}(\mathbf{x}-\hat{\boldsymbol{\mu}}_{\mathcal{B}})^{2}+\epsilon
    $$
实现：
```python
net = nn.Sequential(
	nn.Conv2d(1, 6, kernel_size=5), nn.BatchNorm2d(6), nn.Sigmoid(),
	nn.AvgPool2d(kernel_size=2, stride=2),
	nn.Conv2d(6, 16, kernel_size=5), nn.BatchNorm2d(16), nn.Sigmoid(),
	nn.AvgPool2d(kernel_size=2, stride=2), nn.Flatten(),
	nn.Linear(256, 120), nn.BatchNorm1d(120), nn.Sigmoid(),
	nn.Linear(120, 84), nn.BatchNorm1d(84), nn.Sigmoid(),
	nn.Linear(84, 10))
```

## ResNet
简单来说，对于一个ResNet，我们的数据分为两部分，一个是最初的数据(可能经过单位卷积层变换形状)和经过权重层的数据，对于残差块，实现如下：
![[ResNet.png]]
```python
class Residual(nn.Module):
	def __init__(self, input_channels, num_channels,
				use_1x1conv=False, strides=1):
		super().__init__()
		self.conv1 = nn.Conv2d(input_channels, num_channels,
								kernel_size=3, padding=1, stride=strides)
		self.conv2 = nn.Conv2d(num_channels, num_channels,
								kernel_size=1, padding=1)
		if use_1x1conv:
			self.conv3 = nn.Conv2d(input_channels, num_channels,
									kernel_size=1, stride=strides)
		else:
			self.conv3 = None
		self.bn1 = nn.BatchNorm2d(num_channels)
		self.bn2 = nn.BatchNorm2d(num_channels)

def forward(self, x):
	Y = F.relu(self.bn1(self.conv1(x)))
	Y = self.bn2(self.conv2(x))
	if self.conv3:
		X = self.conv3(x)
	Y += X
	return F.relu(x)			
```
接下来我们实现ResNet：
```python
def resnet_block(input_channels, num_channels, num_residuals,
						first_block=False):
	blk = []
	for i in range(num_residuals):
		if i == 0 and not first_block:
			blk.append(Residual(input_channels, num_channels,
								use_1x1conv=True, strides=2))
		else:
			blk.append(Residual(num_channels, num_channels))
	return blk
```
接下来我们再ResNet加入所有残差块：
```python
b2 = nn.Sequential(*resnet_block(64, 64, 2, first_block=True))
b3 = nn.Sequential(*resnet_block(64, 128, 2))
b4 = nn.Sequential(*resnet_block(128, 256, 2))
b5 = nn.Sequential(*resnet_block(256, 512, 2))

net = nn.Sequential(b1, b2, b3, b4, b5,
					nn.AdaptiveAvgPool2d((1,1)),
					nn.Flatten(), nn.Linear(512, 10))
```

## DenseNet
可以理解为ResNet进阶版，超级多项式拟合，实现过程上的区别在于，对于DenseNet，我们使用concatenate。
```python
def conv_block(input_channels, num_channels):
	return nn.Sequential(
						nn.BatchNorm2d(input_channels),nn.ReLU(),
					    nn.Conv2d(input_channels, num_channels, kernel_size=3,                             padding=1))
```
```python
class DenseBlock(nn.Module):
	def __init__(self, num_convs, input_channels, num_channels):
		super(DenseBlock, self).__init__()
		layer = []
		for i in range(num_convs):
			layer.append(conv_block(
				num_channels * i + input_channels, num_channels))
		self.net = nn.Sequential(*layer)
	def forward(self, X):
		for blk in self.net:
			Y = blk(X)
			X = torch.cat((X, Y), dim=1)
		return X
```
由于稠密块会带来通道数增加，我们使用单位卷积层减小通道数、使用stride=2平均池化减半高和宽：
```python
def transition_block(input_channels, num_channels):
	return nn.Sequential(
						nn.BatchNorm2d(input_channels), nn.ReLU(),
						nn.Conv2d(input_channels, num_channels, kernel_size=1),
						nn.AvgPool2d(kernel_size=2,stride=2))
```

构建DenseNet的方法与上述大同小异，不再赘述。