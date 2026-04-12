---
title: "D2L Chapter 5"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false
categories:
  - "d2l"
---

这一章并没有介绍新的模型或数据集，主要介绍了一些torch的操作，起到一个承上启下。

## 架构
首先，我们从一种比较抽象的层面来讨论pytroch中神经网络结构是如何组织构成的。
单个神经网络是一个很简单的结构，他有三个功能：接受输入，产生输出，携带一些参数，而对于一个MLP也是一样的。
为了实现这些复杂的网络，我们引入了神经网络块的概念。块（block）可以描述单个层、由多个层组成的组件或整个模型本身。使用块进行抽象的一个好处是可以将一些块组合成更大的组件，这一过程通常是递归的。
而我们一般通过class实现block，在定义我们自己的block时，由于自动微分已经定义了一些后端实现，我们只需要考虑前向传播和一些必要的参数。
### 自定义block
我们首先来看看block的一些功能：
1. 将输入数据作为其前向传播函数的参数。
2. 通过前向传播函数来生成输出。请注意，输出的形状可能与输入的形状不同。例如，我们上面模型中的第一个全连接的层接收一个20维的输入，但是返回一个维度为256的输出。
3. 计算其输出关于输入的梯度，可通过其反向传播函数进行访问。通常这是自动发生的。
4. 存储和访问前向传播计算所需的参数。
5. 根据需要初始化模型参数。
```python
class MLP(nn.Module):
	def __init__(self):
			super().__init__()
			self.hidden = nn.Linear(20, 256)
			self.out = nn.Linear(256, 20)

	def forward(self, X):
		return self.out(F.relu(self.hidden(X)))
```
### MySequential
在我们目前的情境下，自己写一个Sequential来连接神经网络的各个层是没什么必要的，但到后面接触到ResNet等结构，这也显得必要了(因为我们要自己写forward)。
```python
class MySequential(nn.Module):
	def __init__(self, *args):
		super().__init__()
		for idx, module in enumerate(args):
			self._modules[str(idx)] = module

	def forward(self, x):
		for block in self._modules.values():
			X = block(X)
		return X
```
所以，其实Sequential类就是一个列表存储这些个层。
## 参数管理
### 参数访问
```python
net[2].state_dict()
>>>OrderedDict([('weight', tensor([[-0.0427, -0.2939, -0.1894, 0.0220, -0.1709, -0.1522, -0.0334, -0.2263]])), ('bias', tensor([0.0887]))])
```
这样访问的就是MLP中第三层的参数
我们同样可以精细地访问：
```python
print(type(net[2].bias))
print(net[2].bias)
print(net[2].bias.data)
```
```shell
<class 'torch.nn.parameter.Parameter'>
Parameter containing:
tensor([0.0887], requires_grad=True)
tensor([0.0887])
```
```python
net[2].weight.grad == None
>>>True
```
一次性访问所有参数：
```python
print(*[(name, param.shape) for name, param in net[0].named_parameters()])
print(*[(name, param.shape) for name, param in net.named_parameters()])
```
```shell
('weight', torch.Size([8, 4])) ('bias', torch.Size([8]))
('0.weight', torch.Size([8, 4])) ('0.bias', torch.Size([8])) ('2.weight', torch.Size([1, 8])) ('2.bias',torch.Size([1]))
```
这也为我们提供了另一种访问参数的方式：
```python
net.state_dict()['2.bias'].data
>>>tensor([0.0887])
```
### 参数初始化
首先，我们有丰富的内置初始化器：
```python
def init_normal(m):
	if type(m) == nn.Linear:
		nn.init.normal_(m.weight, mean=0, std=0.01)
		nn.init.zeros_(m.bias)
net.apply(init_normal)
net[0].weight.data[0], net[0].bias.data[0]
```
```shell
(tensor([-0.0214, -0.0015, -0.0100, -0.0058]), tensor(0.))
```
当然了，通过一点小小的修改就可以把参数初始化为1
```python
	nn.init.constant_(m.weight, 1)
```
我们还可以使用大名鼎鼎的Xavier初始化：
```python
def init_xavier:
	if type(m) == nn.Linear:
		nn.init.xavier_uniform_(m.weight)
def init_42(m):
	if type(m) == nn.Linear
		nn.init.constant_(m.weight, 42)
net[0].apply(init_xavier)
net[2].apply(init_42)
```
我们还有延后初始化，这个放在CNN部分阐明
## 自定义层
我们直接讨论如何定义带参数的层：
```python
class MyLinear(nn.Module):
	def __init__(self, in_units, units):
		super().__init__()
		self.weight = nn.Parameter(torch.randn(in_units, units))
		self.bias = nn.Parameter(torch.randn(units,))
	def forward(self, X):
		linear = torch.matmul(X, self.weight.data) + self.bias.data
		return F.relu(linear)
```

## 读写文件
比较重要的就是存参数和读取参数：
```python
#对于一个net网络
torch.save(net.state_dict(), 'mlp.params')#save
#如何恢复模型
clone = MLP()
clone.load_state_dict(torch.load('mlp.params'))
clone.eval()
```
## GPU
这部分就纯纯gpu技术文档，现用现查完了