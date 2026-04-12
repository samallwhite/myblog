---

title: "动手学深度学习Pytorch(2-4章)"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false

---

这是我第一篇博客。主播只用了2h看完了三章内容，想必是非常的不精的，于是整理一下。。。

# 2 预备知识
本部分主要讲解了一些pytorch的基本操作和一些数学知识：
### Tensor的初始化和一些特征
初始化张量：
```python
A = torch.tensor([[1,2,3],[4,5,6]])
```
好了，我们现在已经会创造tensor了，我们接下来应该尝试着去得到一些关于这个tensor的信息：
首先值得提及的是python自身的`len()`方法同样适用，但该方法只能显示第0轴的尺寸信息：
```python
len(A)
```
```bash
2
```
所以其实这玩意挺垃圾，我们需要用高级方法：`shape`
```python
A.shape
```
```bash
torch.Size([2,3])
```
我们可以通过使用`reshape()`方法改变张量的结构：
```python
a = A.reshape(3,2)
```
```bash
tensor([[1,2],[3,4],[5,6]])
```
像tensorflow一样，我们可以搞ones、zeros等东西：
```python
zero_tensor = torch.zero((2,3))
one_tensor = torch.ones((2,3))
```
生成随机张量：
```python
random_tensor = torch.randn((2,3))
```
btw，这个随机是从正态分布中随机抽样的
### Tensor的一些运算：
基本的运算没啥区别，我们特别说一下连接(concatenate)
```python
X = torch.arange(12, dtype=torch.float32).reshape((3,4))
Y = torch.tensor([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
torch.cat((X, Y), dim=0), torch.cat((X, Y), dim=1)
```
```bash
(tensor([[ 0., 1., 2., 3.],
[ 4., 5., 6., 7.],
[ 8., 9., 10., 11.],
[ 2., 1., 4., 3.],
[ 1., 2., 3., 4.],
[ 4., 3., 2., 1.]]),
tensor([[ 0., 1., 2., 3., 2., 1., 4., 3.],
[ 4., 5., 6., 7., 1., 2., 3., 4.],
[ 8., 9., 10., 11., 4., 3., 2., 1.]]))
```
求和：
```python
A.sum()
```
(非常的朴实无华)
你可以很方便的将张量跟nparray转换：
```python
A = X.numpy()
B = torch.tensor(A)
```
### 如何读取数据：
先别管那些有的没的，假设我们已经有了input和output了：
```python
inputs, outputs = data.iloc[:, 0:2], data.iloc[:, 2]
```
接下来只需要这样：
```python
X = torch.tensor(inputs.to_numpy(dtype=float))
y = torch.tensor(outputs.to_numpy(dtype=float))
```

### 一些线代知识：
首先，我们介绍Hadamard积(⊙)：

$$
\mathbf{A} ⊙\mathbf{B} =
\begin{bmatrix}
a_{11}b_{11} & a_{12}b_{12} & \cdots & a_{1n}b_{1n} \\
a_{21}b_{21} & a_{22}b_{22} & \cdots & a_{2n}b_{2n} \\
\vdots       & \vdots       & \ddots & \vdots       \\
a_{m1}b_{m1} & a_{m2}b_{m2} & \cdots & a_{mn}b_{mn}
\end{bmatrix}
$$

该Hadamard积可以表示为：
```python
A * B
```
接下来是降维操作，我们所指的降维操作是通过求和形式来实现的，就像刚才所提到的sum一样，我们同样使用sum，不过多了一些参数：
```python
A_sum_axis0 = A.sum(axis = 0) #对第0轴求和
```
但我们仍然可以在求和过程中保留维度：
```python
sum_A = A.sum(axis = 0, keepdims = true)
```
这很有用，他可以帮我们实现平均正则化：
```python
A / sum_A
```
```bash
tensor([[0.0000, 0.1667, 0.3333, 0.5000],
        [0.1818, 0.2273, 0.2727, 0.3182],
        [0.2105, 0.2368, 0.2632, 0.2895],
		[0.2222, 0.2407, 0.2593, 0.2778],
		[0.2286, 0.2429, 0.2571, 0.2714]])
```
若要计算累计总和，我们应该用cumsum：
```python
A.cumsum(axis=0)
>>
>>tensor([[ 0., 1., 2., 3.],
[ 4., 6., 8., 10.],
[12., 15., 18., 21.],
[24., 28., 32., 36.],
[40., 45., 50., 55.]])
```
点积(dot product):
```python
torch.dot(x,y)
```

矩阵-向量积：
矩阵可以表示为行向量作为元素的列向量：


$$
\mathbf{A} =
\begin{bmatrix}
\mathbf{a}_1^{\top} \\
\mathbf{a}_2^{\top} \\
\vdots \\
\mathbf{a}_m^{\top}
\end{bmatrix}
\tag{2.3.5}
$$


在我们有一个同维度向量X的情况下:


$$
\mathbf{A} \mathbf{x} =
\begin{bmatrix}
\mathbf{a}_1^{\top} \\
\mathbf{a}_2^{\top} \\
\vdots \\
\mathbf{a}_m^{\top}
\end{bmatrix}
\mathbf{x}
$$



$$
=
\begin{bmatrix}
\mathbf{a}_1^{\top} \mathbf{x} \\
\mathbf{a}_2^{\top} \mathbf{x} \\
\vdots \\
\mathbf{a}_m^{\top} \mathbf{x}
\end{bmatrix}
\tag{2.3.6}
$$


这就是矩阵-向量积：
```python
troch.mv(A, x)
```
矩阵乘法：
无需多言：
```python
torch.mm(X, Y)
```
**范数(norm)**：
这个比较屌，也比较重要
我们首先说一下$L_{2}$范数，其实这个范数就是此前学到的向量的模：

$$
\|\mathbf{x}\|_2 = \sqrt{ \sum_{i=1}^n x_i^2 }
\tag{2.3.14}
$$

对于$L_{2}$范数，通常省略$L$：
```python
torch.norm(x)
```
与此同时，我们还有$L_{1}$范数：

$$
\|\mathbf{x}\|_1 = \sum_{i=1}^n |x_i|
\tag{2.3.15}
$$

与$L_{2}$范数相比，$L_{1}$范数受异常值的影响较小：
```python
#我们没有可以直接计算L1范数的方法
torch.abs(u).sum()
```
而以上所提及的$L_{1}$范数与$L_{2}$范数都是$L_{p}$范数的特例：

$$
\|\mathbf{x}\|_p = \left( \sum_{i=1}^{n} |x_i|^p \right)^{1/p}
\tag{2.3.16}
$$

同时，我们还给出Frobenius范数，这个范数是矩阵的元素平方和的平方根，就好像是矩阵的$L_{2}$范数：

$$
\|\mathbf{X}\|_F = \sqrt{ \sum_{i=1}^{m} \sum_{j=1}^{n} x_{ij}^2 }
\tag{2.3.17}
$$

想要求解Frobenius范数也很简单：
```python
torch.norm(Mat)
```
范数和目标
在深度学习中，我们经常试图解决优化问题：最大化分配给观测数据的概率; 最小化预测和真实观测之间的距离。用向量表示物品（如单词、产品或新闻文章），以便最小化相似项目之间的距离，最大化不同项目之间的距离。目标，或许是深度学习算法最重要的组成部分（除了数据），通常被表达为范数。

### 没有一些微积分知识
因为主包还记得

### 自动微分
数学原理部分等我学了矩阵分析再说吧
直接上代码：
```python
x = torch.arange(4.0)
>>> tensor([0.,1.,2.,3.])
x.requires_grad_(True)
x.grad # 默认值是None
#整一个y
y = torch.dot(x,x)
y
>>>tensor(28., grad_fn=<MulBackward0>)
y.backward()#反向传播函数计算每个梯度
>>>tensor([ 0., 4., 8., 12.])
```
非标量变量的反向传播：
当y不是标量时，向量y关于向量x的导数的最自然解释是一个矩阵。对于高阶和高维的y和x，求导的结果可以是一个高阶张量。
当调用向量的反向计算时，我们通常会试图计算一批训练样本中每个组成部分的损失函数的导数。这里，我们的目的不是计算微分矩阵，而是单独计算批量中每个样本的偏导数之和。
```python
# 对非标量调用backward需要传入一个gradient参数，该参数指定微分函数关于self的梯度。
# 本例只想求偏导数的和，所以传递一个1的梯度是合适的
x.grad.zero_()
y = x * x
# 等价于y.backward(torch.ones(len(x)))
y.sum().backward()
x.grad
```

关于为什么我们要对y.sum进行自动微分：
对于：


$$
\mathbf{y} = f(\mathbf{x}) \in \mathbb{R}^n
$$


那么：


$$
\nabla_{\mathbf{x}} \mathbf{y} =
\begin{bmatrix}
\frac{\partial y_1}{\partial x_1} & \cdots & \frac{\partial y_1}{\partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial y_n}{\partial x_1} & \cdots & \frac{\partial y_n}{\partial x_n}
\end{bmatrix}
\in \mathbb{R}^{n \times n}
$$

这是一个 **Jacobian（雅可比矩阵）**，而不是一个简单的向量。
所以如果我们不指定维度的话，pytorch就不知道以什么方向微分。

## 3.线性神经网络
### 线性回归：
#### 一些术语：
在机器学习的术语中，该数据集称为训练数据集（training data set）或训练集（training set）。每行数据（比如一次房屋交易相对应的数据）称为样本（sample），也可以称为数据点（datapoint）或数据样本（data instance）。我们把试图预测的目标（比如预测房屋价格）称为标（label）或目
（target）。预测所依据的自变量（面积和房龄）称为特征（feature）或协变量（covariate）。

在机器学习领域，我们通常使用的是高维数据集，建模时采用线性代数表示法会比较方便。当我们的输入包含 \( d \) 个特征时，我们将预测结果（通常使用“尖角”符号表示 \( y \) 的估计值）表示为：

$$
\hat{y} = w_1 x_1 + \cdots + w_d x_d + b
\tag{3.1.2}
$$



将所有特征放到向量 \( $\mathbf{x} \in \mathbb{R}^d$\) 中，并将所有权重放到向量 ( $\mathbf{w} \in \mathbb{R}^d$) 中，我们可以用点积形式来简洁地表达模型：

$$
\hat{y} = \mathbf{w}^\top \mathbf{x} + b
\tag{3.1.3}
$$


在 (3.1.3) 中，向量 $\mathbf{x}$对应于单个数据样本的特征。用符号表示的矩阵 $\mathbf{X} \in \mathbb{R}^{n \times d}$ 可以很方便地引用我们整个数据集的 (n)个样本。其中， $\mathbf{x}$的每一行是一个样本，每一列是一种特征。
对于特征集合 $\mathbf{x}$，预测值 $\hat{\mathbf{y}} \in \mathbb{R}^n$可以通过矩阵-向量乘法表示为：

$$
\hat{\mathbf{y}} = \mathbf{X} \mathbf{w} + b
\tag{3.1.4}
$$

那么到目前为止，我们已经有了预期的模型形式，我们现在还需要两件事：模型质量的度量方式和更新模型以提高质量的方法。
对于度量方式，我们提出损失函数(loss function):

$$
l^{(i)}(\mathbf{w}, b) = \frac{1}{2} \left( \hat{y}^{(i)} - y^{(i)} \right)^2
$$


为了衡量在整个数据集上的损失，我们对loss取均值，获得均方误差(MSE):


$$
L(\mathbf{w}, b) = \frac{1}{n} \sum_{i=1}^{n} \frac{1}{2} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)^2
$$


所以我们的目的其实是：


$$
\mathbf{w}^*, b^* = \mathop{\arg\min}_{\mathbf{w}, b} L(\mathbf{w}, b)
$$

对于更新模型，我们采用梯度下降：
梯度下降最简单的用法是计算损失函数（数据集中所有样本的损失均值）关于模型参数的导数（在这里也可以称为梯度）。但实际中的执行可能会非常慢：因为在每一次更新参数之前，我们必须遍历整个数据集。因此，我们通常会在每次需要计算更新的时候随机抽取一小批样本，这种变体叫做
**小批量随机梯度下降（minibatchstochastic gradient descent）。**
**权重向量更新公式：**


$$
\mathbf{w} \leftarrow \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$


**偏置更新公式：**


$$
b \leftarrow b - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

在上式中，$\eta$是学习率，$\mathcal{B}$是每一个小批量的数据个数，也叫batch size，这两个是超参数，是**手动调整**的。
我们可以通过高斯噪声正态分布和极大似然估计来证明MSE的合理性，但本笔记不多赘述。我在证明过程中发现了一个ϵ，这有点意思：它 在推理或采样中才显式出现
如果你是搞**贝叶斯建模**或**生成模型**，比如：
- 贝叶斯线性回归
- GAN / VAE
- Diffusion Models
这时候你会看到 ϵ 被当作“噪声源”来显式建模，用于：
- 模拟不确定性；
- 做采样或重构。
#### 简单实现
(书中有scrach部分，但我不喜欢造轮子，我喜欢用轮子)
我们先给定真的w和b用以生成数据iterator：
```python
true_w = torch.tensor([2, -3.4])
true_b = 4.2
features, labels = d2l.synthetic_data(true_w, true_b, 1000)
def load_array(data_arrays, batch_size, is_train=True): #@save
"""构造一个PyTorch数据迭代器"""
	dataset = data.TensorDataset(*data_arrays)
	return data.DataLoader(dataset, batch_size, shuffle=is_train)
```
定义模型：
```python
from torch import nn
net = nn.Sequential(nn.Linear(2,1))
```
其中Linear的第一个参数是输入维度，第二个参数是输出维度
初始化模型参数：
```python
net[0].weight.data.normal_(0, 0.01)
net[1].bias.data.fill_(0)
```
通过net\[0]选择网络中的第一个图层，然后使用weight.data和bias.data方法访问参数。
定义损失函数：
```python
loss = nn.MSELoss()
```
定义优化算法：
```python
trainer = torch.optim.SGD(net.parameters(), lr=0.03)
```
整体训练流程：
```python
num_epochs = 3
for epoch in range(num_epochs):
	for X, y in data_iter:
		l = loss(net(X) ,y)
		trainer.zero_grad()
		l.backward()
		trainer.step()#这一步进行参数更新
	l = loss(net(features), labels)
	print(f'epoch {epoch + 1}, loss {l:f}')
```

### Softmax回归：
softmax回归解决的是分类问题，它也是一种单层神经网络，输出层同样是全连接层。

$$
\hat{\mathbf{y}} = \text{softmax}(\mathbf{o}), \quad \text{其中} \quad \hat{y}_j = \frac{\exp(o_j)}{\sum_k \exp(o_k)}
$$

softmax运算获取一个向量并将其映射为概率。
在此部分我们仍然有小批量处理，他大大提高了mv运算。
#### 交叉熵损失函数

$$
l = - \sum_{j=1}^{q} y_j \log(\hat{y}_j)
$$

#### 图片数据集的读取
我们可以通过torch内置的方法读取：
```python
trans = transforms.ToTensor()
mnist_train = torchvision.datasets.FashionMNIST(
				root="../data", train=True, transform=trans, download=True)
mnist_test = torchvision.datasets.FashionMNIST(
				root="../data", train=False, transform=trans, download=True)
```
当然，我们可以获取这个数据的一些量：
```python
len(mnist_train), len(mnist_test)
>>>(60000, 10000)
mnist_train[0][0].shape
>>>torch.Size([1, 28, 28])
```
其实现在这个train或test是一个4维张量
```bash
tensor([60000,1,28,28])
```
关于为什么不把维度为1的消除掉，进行concatenate，gpt是这么说的：

| 项目 | 解释 |
|------|------|
| `nn.Conv2d(in_channels, out_channels, kernel_size)` | 要求输入必须是 4D 张量 \`[N, C, H, W]\`，哪怕 \`C=1\`，也必须写成 \`[1, 28, 28]\`。 |
| `DataLoader` 输出 | 自动组织成 \`[batch_size, 1, 28, 28]\`，方便后续操作。 |
| 多任务/迁移学习 | 如果以后处理彩色图像，保留 \`channel\` 更好扩展。 |
| `BatchNorm2d` / `MaxPool2d` 等 | 都要求显式通道维，否则报错。 |
#### softmax简洁实现：
初始化
```python
# PyTorch不会隐式地调整输入的形状。因此，
# 我们在线性层前定义了展平层（flatten），来调整网络输入的形状
net = nn.Sequential(nn.Flatten(), nn.Linear(784, 10))#将1*28*28展平成784
def init_weights(m):
if type(m) == nn.Linear:
nn.init.normal_(m.weight, std=0.01)
net.apply(init_weights);
```
定义损失：
```python
loss = nn.CrossEntropyLoss(reduction='none')
```
关于reduction参数：

|取值|含义|
|---|---|
|`'none'`|不进行汇总，**保留每个样本的损失值**，输出 shape 为 `[batch_size]`。|
|`'mean'`（默认）|**对所有样本的损失取平均**，输出是一个标量。常用于大多数训练场景。|
|`'sum'`|对所有样本的损失**直接求和**，输出是一个标量。|
优化依旧：
```python
trainer = torch.optim.SGD(net.parameters(), lr=0.1)
```
训练总览：
```python
import torch
from torch import nn

def train(net, train_iter, test_iter, loss_fn, num_epochs, optimizer, device=None):
    if device is None:
        device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    net.to(device)

    for epoch in range(num_epochs):
        net.train()
        total_loss, total_correct, total_samples = 0.0, 0, 0

        for X, y in train_iter:
            X, y = X.to(device), y.to(device)

            # 前向传播
            y_hat = net(X)
            loss = loss_fn(y_hat, y)

            # 反向传播与优化
            optimizer.zero_grad()
            loss.mean().backward()
            optimizer.step()

            total_loss += loss.sum().item()
            total_correct += (y_hat.argmax(dim=1) == y).sum().item()
            total_samples += y.numel()

        train_acc = total_correct / total_samples
        test_acc = evaluate_accuracy(net, test_iter, device)
        print(f"epoch {epoch + 1}, loss {total_loss / total_samples:.4f}, "
              f"train acc {train_acc:.3f}, test acc {test_acc:.3f}")

def evaluate_accuracy(net, data_iter, device=None):
    if device is None:
        device = next(net.parameters()).device
    net.eval()
    correct, total = 0, 0

    with torch.no_grad():
        for X, y in data_iter:
            X, y = X.to(device), y.to(device)
            pred = net(X).argmax(dim=1)
            correct += (pred == y).sum().item()
            total += y.numel()
    return correct / total

```

## 4 多层感知机(MLP)
就是神经元叠乐高。
我们现在有了一些权威的函数：ReLU,tanh,sigmoid等等，ReLU比较权威因为好算。
调用ReLU也很容易：
```python
y = torch.relu(x)
```
而且，它的求导表现得特别好：要么让参数消失，要么让参数通过。这使得优化表现得更好，并
且ReLU减轻了困扰以往神经网络的梯度消失问题。

### MLP简洁实现
```python
net = nn.Sequential(nn.Flatten(),
					nn.Linear(784, 256),
					nn.ReLU(),
					nn.Linear(256, 10))
def init_weights(m):
	if type(m) == nn.Linear:
		nn.init.normal_(m.weight, std=0.01)
		
net.apply(init_weights);
```
那么很容易知道这是个两层的神经网络，现先展平->线性层->ReLU->线性层->输出.
训练过程的实现与我们实现softmax回归时完全相同，这种模块化设计使我们能够将与模型架构有关的内容独立出来：
```python
batch_size, lr, num_epochs = 256, 0.1, 10
loss = nn.CrossEntropyLoss(reduction='none')
trainer = torch.optim.SGD(net.parameters(), lr=lr)
```
<font color="#ff0000">目前为止，我们已经可以总结出训练一个模型的基本流程了</font>[[train_mlp_pytorch]]
### 对于模型的调整
- 我们指出，训练可能会导致过拟合，也就是训练误差降低而泛化误差升高。
- 原则上，在我们确定所有的超参数之前，我们不希望用到测试集。如果我们在模型选择过程中使用测试数据，可能会有过拟合测试数据的风险，那就麻烦大了。如果我们过拟合了训练数据，还可以在测试数据上的评估来判断过拟合。但是如果我们过拟合了测试数据，我们又该怎么知道呢？
- 解决此问题的常见做法是将我们的数据分成三份，除了训练和测试数据集之外，还增加一个验证数据集（validation data set），也叫验证集（validation set）。
- 流行的解决方案是采用**K折交叉验证**
#### 正则化
权重衰减是最广泛应用的正则化方法，通常也被称为$L_{2}$正则化。
为了惩罚权重向量的大小，我们必须以某种方式在损失函数中添加∥w∥2，但是模型应该如何平衡这个新的额外惩罚的损失？实际上，我们通过正则化常数λ来描述这种权衡，这是一个非负超参数，我们使用验证数据拟合：

$$
L(\mathbf{w}, b) + \frac{\lambda}{2} \|\mathbf{w}\|^2
$$

我们采用$L_{2}$范数主要是因为$L_{2}$正则分布构成经典的岭回归，这使得我们的学习算法偏向于在大量特征上均匀分布权重的模型。
所以我们有了新的SGD：

$$
\mathbf{w} \leftarrow (1 - \eta \lambda) \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

此$\lambda$也是超参数。
#### 暂退法
暂退法在前向传播过程中，计算每一内部层的同时丢弃一些神经元。用来避免过拟合
```python
net = nn.Sequential(nn.Flatten(),
					nn.Linear(784, 256),
					nn.ReLU(),
					# 在第一个全连接层之后添加一个dropout层
					nn.Dropout(dropout1),
					nn.Linear(256, 256),
					nn.ReLU(),
					# 在第二个全连接层之后添加一个dropout层
					nn.Dropout(dropout2),
					nn.Linear(256, 10))
def init_weights(m):
	if type(m) == nn.Linear:
		nn.init.normal_(m.weight, std=0.01)
net.apply(init_weights);
```
#### 前向传播与反向传播
- 前向传播在神经网络定义的计算图中按顺序计算和存储中间变量，它的顺序是从输入层到输出层。
- 反向传播按相反的顺序（从输出层到输入层）计算和存储神经网络的中间变量和参数的梯度。
- 在训练深度学习模型时，前向传播和反向传播是相互依赖的。
#### 数值稳定和模型初始化
- 梯度消失和梯度爆炸是深度网络中常见的问题。在参数初始化时需要非常小心，以确保梯度和参数可以得到很好的控制。
- 需要用启发式的初始化方法来确保初始梯度既不太大也不太小。
- ReLU激活函数缓解了梯度消失问题，这样可以加速收敛。
- 随机初始化是保证在进行优化前打破对称性的关键。
- Xavier初始化表明，对于每一层，输出的方差不受输入数量的影响，任何梯度的方差不受输出数量的影响。
#### 环境和分布偏移
- 在许多情况下，训练集和测试集并不来自同一个分布。这就是所谓的分布偏移。
- 真实风险是从真实分布中抽取的所有数据的总体损失的预期。然而，这个数据总体通常是无法获得的。
- 经验风险是训练数据的平均损失，用于近似真实风险。在实践中，我们进行经验风险最小化。
- 在相应的假设条件下，可以在测试时检测并纠正协变量偏移和标签偏移。在测试时，不考虑这种偏移可能会成为问题。
- 在某些情况下，环境可能会记住自动操作并以令人惊讶的方式做出响应。在构建模型时，我们必须考虑到这种可能性，并继续监控实时系统，并对我们的模型和环境以意想不到的方式纠缠在一起的可能性持开放态度。



最终，作为2-4章学习效果评估，我选择手动搭建MLP，参阅：[[My MLP]]