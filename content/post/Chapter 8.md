这部分讲解了传统的RNN，其中涉及的内容和原理非常多，需要仔细梳理。
**首先，RNN是用来处理序列数据的**
对于一个时间序列$x_{i}$，我们想要预测他的$x_{t}$，可以通过结合过往时间步的数据预测：
$$
x_t​∼P(x_{t}​∣x_{t−1}​,…,x_{1}​)
$$
而要实现这种策略，我们有以下几个方式：
## 回归模型
主要分为以下两种策略：
### 自回归模型
在现实情况下相当长的序列$x_{t-1},\dots,x_{1}$可能是不必要的，因此我们只需要观测满足长度$\tau$的序列
$x_{t-1},\dots,x_{t-\tau}$即可，这样有一个好处就是我们获得的参数的总量是不变的。
### 隐变量自回归模型
保留对过去观测的总结$h_{t}$，并同时更新$\hat{x}、h_t$，由于$h_{t}$从未被观测到，所以称为隐变量自回归模型。
![[latent autoregressive model.png]]
但回归模型有个问题，我们不能用新预测出来的结果作为预测的数据，这会导致严重的偏移。
联合概率模型：
$$
P(x_1, x_2, \dots, x_T) = \prod_{t=1}^{T} P(x_t \mid x_1, \dots, x_{t-1})
$$
联合概率模型太难算了，针对这一点我们进行优化。
### 马尔科夫概率模型：
我们简化认为$x_{t}$仅与$x_{t-1}$有关，那么我么可以得到：
$$
P(x_1, x_2, \dots, x_T) = P(x_1) \cdot \prod_{t=2}^{T} P(x_t \mid x_{t-1})
$$
这大大简化了计算并且可以使用动态规划的方法(补票)。
我们还可以进行边际化，这样我们不用知道中间状态就可以预测：
$$
P(x_{t+1} \mid x_{t-1}) = \sum_{x_t} P(x_{t+1} \mid x_t) \cdot P(x_t \mid x_{t-1})
$$
关于**为什么k-step比1-step具有劣势的讨论**：
- **不用新生成数据 ≠ 容易**；它只是避免“输入污染”，但把 **更长的时序依赖** 和 **更大的不确定性** 一次压给模型。
- 如果数据规律简单、模型足够强，k‑step 会更稳也可能更准；若模型欠拟合或序列带高噪声，则 1‑step 滚动可能反而效果更好。
- 实务中常见折衷：
    - **Scheduled Sampling / Professor Forcing**：训练时混合真值和预测值，让递归式提前学会容错；
    - **多尺度或分段输出**：短期直接、长期递归；
    - **Seq2Seq + Attention**：既可一次产出多步，又让模型内部逐步解码。
## 文本预处理：
这部分纯纯准备工作，含有以下方面：
1. 将文本作为字符串加载到内存中。
2. 将字符串拆分为词元（如单词和字符）。
3. 建立一个词表，将拆分的词元映射到数字索引。
4. 将文本转换为数字索引序列，方便模型操作。
### 词元化：
```python
def tokenize(lines, token='word'):
	if token == 'word':
		return [line.split() for line in lines]
	elif token == 'char':
		return [list(line) for line in lines]
	else:
		print('error')
tokens = tokenize(lines)
```
就是把它拆开，另，这部分用到了很多迭代器。
### 词表：
用来将词元映射到从0开始的索引中：
我们先将训练集中的所有文档合并在一起，对它们的唯一词元进行统计，得到的统计结果称之为语料（corpus）。然后根据每个唯一词元的出现频率，为其分配一个数字索引
```python
class Vocab:
	def __init__(self, tokens=None, min_freq=0, reserved_tokens=None):
		if tokens is None:
			tokens = []
		if reserved_tokens is None:
			reserved_tokens = []

		counter = ccount_corpus(tokens)
		self._token_freqs = sorted(counter.items(), key=lambda x: x[1],
									reverse=True)
		self.idx_to_token = ['<unk>'] + reserved_tokens
		self.token_to_idx = {token: idx 
								for idx, token in enumerate(self.idx_to_token)}
		for token, freq in self._token_freqs:
			if freq < min_freq:
				break
			if token not in self.token_to_idx:
				self.idx_to_token.append(token)
				self.token_to_idx[token] = len(self.idx_to_token) - 1
	def __len__(self):
		return len(self.idx_to_token)
	def __getitem__(self, token):
		if not isinstance(tokens, (list, tuple)):
			return self.token_to_idx.get(tokens, self.unk)
		return [self.__getitem__(token) for token in tokens]

	def to_tokens(self, indices):
			if not isinstance(indicies, (list,tuple)):
				return self.idx_to_token[indicies]
			return [self.idx_to_token for index in indicies]

	def unk(self):
		return 0

	def token_freqs(self):
		return self._token_freqs

def count_corpus(tokens):
	if len(tokens) == 0 or isinstance(tokens[0], list):
		tokens = [token for line in tokens for token in line]
	return collections.Counter(tokens)

```

## 语言模型和数据集
### 语言模型
在给定的模型序列，语言模型的目标是估计序列联合概率，但他只能生成合语法的句子而非理解语句。
### 马尔科夫模型与n元语法
通常，涉及一个、两个和三个变量的概率公式分别被称为一元语法（unigram）、二元语法（bigram）和三元语法（trigram）模型。
$$
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2)P(x_3)P(x_4),
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2 | x_1)P(x_3 | x_2)P(x_4 | x_3),
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2 | x_1)P(x_3 | x_1, x_2)P(x_4 | x_2, x_3)
$$


## RNN
![[latent RNN.png]]