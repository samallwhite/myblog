---

title: "动手学深度学习Pytorch(6章)"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false

---

这一章主要讲的是传统的卷积神经网络(CNN)。
首先应该注意，CNN包含以下部分：
	卷积层、汇聚层(池化层？)，
	而一些常用的参数：填充和步幅、通道。

CNN的概念源自空间不变性的系统化，空间不变性包括：
- 平移不变性：
	简要来说就是如果一个图像中的某个特征在位置 $(i, j)$出现能被识别，那么它在$(i+1, j+1)$出现也应该能被识别。
- 局部性
## 由MLP到CNN
通常情况下，MLP可以用以下公式表示：
$$
\mathbf{H}_{i,j} = \mathbf{U}_{i,j} + \sum_k \sum_l \mathbf{W}_{i,j,k,l} \cdot \mathbf{X}_{k,l} 
= \mathbf{U}_{i,j} + \sum_a \sum_b \mathbf{V}_{i,j,a,b} \cdot \mathbf{X}_{i+a,j+b}
\tag{1}
$$
由此式我们经过一些变换得到CNN的表示：
- 由平移不变性，我们MLP原来的公式中$\mathbf{V}$是与$i,j$相关的量，也就是说$\mathbf{V}$是和位置有关的，既然我们平移不变，那我们就可以假设所有位置的$\mathbf{V}$都是一个相同的值，对于偏置$u$也是同理，于是我们由1式得到下式：

$$
\mathbf{H}_{i,j} = u + \sum_a \sum_b \mathbf{V}_{a,b} \cdot \mathbf{X}_{i+a,j+b}\tag{2}
$$
- 接下来我们考虑第二个原则：局部性。也就是在一定范围之外的$\mathbf{V}$可以被设置为0，于是我们由2式得到下式：
$$
\mathbf{H}_{i,j} = u + \sum_{a=-\Delta}^{\Delta} \sum_{b=-\Delta}^{\Delta} \mathbf{V}_{a,b} \cdot \mathbf{X}_{i+a,j+b} \tag{3}
$$
至此，我们得到了卷积层的数学表述,$\mathbf{V}$被称为卷积核(convolution kernel)或滤波器(filter)，显然，我们的参数规模大幅减小，但代价是以上所有的学习权重都依赖于归纳偏置，偏置不行就寄寄了。
## 数学中的卷积
两个函数之间的卷积被定义为：
$$
(f * g)(\mathbf{x}) = \int f(\mathbf{z}) g(\mathbf{x} - \mathbf{z}) \, d\mathbf{z}
\tag{4}
$$


$$
(f * g)(i) = \sum_a f(a) g(i - a)
\tag{5}
$$
对于二维张量，则为f的索引(a, b)和g的索引$(i − a, j − b)$上的对应加和：

$$
(f * g)(i, j) = \sum_a \sum_b f(a, b) g(i - a, j - b)
\tag{6}
$$
## 互相关运算
![[CNN.png]]
这很直观明了，且我们的输出与输入和核函数的关系是：
$$
(n_{h}-k_{h}+1)\times(n_{w}-k_{w}+1)\tag{7}
$$
实现该运算的方式：
```python
def corr2d(X, K): 
	h, w = K.shape
	Y = torch.zeros((x.shape[0] - h + 1, X.shape[1] - w + 1))
	for i in range(Y.shape[0]):
		for j in range(Y,shape[1]):
			Y[i. j] = (X[i:i + h, j:j + w] * K).sum()

	return Y
```

卷积层：
```python
class Conv2D(nn.Module):
	def __init__(self, kernel_size):
		super().__init__()
		self.weight = nn.Parameter(torch.rand(kernel_size))
		self.bias = nn.Parameter(torch.zeros(1))
	def forward(self, x):
		return corr2d(x, self.weight) + self.bias
```

## 图像中目标的边缘检测：
一开始我们手搓一个kernel：
```bash
tensor([[1., 1., 0., 0., 0., 0., 1., 1.],
		[1., 1., 0., 0., 0., 0., 1., 1.],
		[1., 1., 0., 0., 0., 0., 1., 1.],
		[1., 1., 0., 0., 0., 0., 1., 1.],
		[1., 1., 0., 0., 0., 0., 1., 1.],
		[1., 1., 0., 0., 0., 0., 1., 1.]])
```
对于这个图像，我们手搓一个`tensor([1,-1])`当kernel，运行得到结果：
```bash
tensor([[ 0., 1., 0., 0., 0., -1., 0.],
		[ 0., 1., 0., 0., 0., -1., 0.],
		[ 0., 1., 0., 0., 0., -1., 0.],
		[ 0., 1., 0., 0., 0., -1., 0.],
		[ 0., 1., 0., 0., 0., -1., 0.],
		[ 0., 1., 0., 0., 0., -1., 0.]])
```
他很好的完成了检测边缘的工作。
如果我们只需寻找黑白边缘，那么以上[1, -1]的边缘检测器足以。然而，当有了更复杂数值的卷积核，或者连续的卷积层时，我们不可能手动设计滤波器。那么我们是否可以学习由X生成Y的卷积核呢？
```python
# 构造一个二维卷积层，它具有1个输出通道和形状为（1，2）的卷积核
conv2d = nn.Conv2d(1,1, kernel_size=(1, 2), bias=False)
# 这个二维卷积层使用四维输入和输出格式（批量大小、通道、高度、宽度），
# 其中批量大小和通道数都为1
X = X.reshape((1, 1, 6, 8))
Y = Y.reshape((1, 1, 6, 7))
lr = 3e-2 # 学习率
for i in range(10):
	Y_hat = conv2d(X)
	l = (Y_hat - Y) ** 2
	conv2d.zero_grad()
	l.sum().backward()
	# 迭代卷积核
	conv2d.weight.data[:] -= lr * conv2d.weight.grad
	if (i + 1) % 2 == 0:
		print(f'epoch {i+1}, loss {l.sum():.3f}')
```
最后我们得到的卷积核：
`tensor([[ 1.0010, -0.9739]])`
## 特征映射和感受野
特征映射就是一个卷积层的输出，感受野是前向传播过程中可能影响计算的所有元素(来自所有先前层)。
## 填充和步幅
填充就是一个图片太小，我们需要扩大他的感受野方便卷积核学习；
```python
conv2d = nn.Conv2d(1, 1, kernel_size=3, padding=1)
conv2d = nn.Conv2d(1, 1, kernel_size=(5, 3), padding=(2, 1))#高度，宽度
```
步幅就是一个图片太大，我们要让卷积核在一次移动中移动大于一个像素单位
```python
conv2d = nn.Conv2d(1, 1, kernel_size=3, padding=1, stride=2)
conv2d = nn.Conv2d(1, 1, kernel_size=(3, 5), padding=(0, 1), stride=(3, 4))
```
## 通道
我们到目前为止的讨论都是基于单色图片来进行的，但是事实上我们大多数图片都是由三种颜色构成，对于卷积来说就是三通道。
所以，对于一个通道数为$c_{i}$的图片，此时我们的图片是一个$c_{i}\times w\times h$的张量，而对于这样的感受野，我们将采取一个$c_{i}\times w_{n}\times h_{n}$的卷积核，对于每一层的卷积结果相加得到输出：
![[multi-channel-cnn.png]]
多输出通道指的是卷积层会使用多个不同的卷积核，每个核与所有输入通道进行卷积操作，生成一个输出特征图。所有输出特征图共同构成输出张量的多个通道。每个输出通道可能学习到输入图像中的不同特征模式，如边缘、纹理、形状等。
## $1\times 1$卷积核
我们主要使用$1\times 1$卷积核调整通道数量和参数复杂性：
![[单位卷积核.png]]
## 汇聚层(池化层)
我们可以用pooling来缓解卷积层的位置敏感
![[pooling.png]]我们有最大池化和平均池化，对于池化层，卷积层中的padding、stride和多通道都是相同的。


最终，作为第六章的学习效果检测，我选择手动搭建LeNet[[My_LeNet]]