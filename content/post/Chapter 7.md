---
title: "D2L Chapter 7"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false
categories:
  - "d2l"
---

杩欎竴绔犱富瑕佷粙缁嶄簡涓€浜涚幇浠ｇ缁忕綉缁?鍦ㄨ繖涓€閮ㄥ垎锛屾垜浠皢瀛︿範鍒板緢澶氱幇浠ｅ嵎绉缁忕綉缁滅殑缁撴瀯锛岃€屽湪杩欎釜杩囩▼涓紝杈撳叆灞傚拰杈撳嚭灞傜殑寮犻噺鐨勭淮搴︽槸鍗佸垎閲嶈鐨勶紝鎴戜滑浠嶇劧缁欏嚭璁＄畻鍏紡锛?$$
\mathbf{W}_{out} = \lfloor\frac{\mathbf{W}_{in}+2\cdot padding-kernel size}{stride}\rfloor +1
$$
## 浣跨敤nn.Sequential鍜宑lass name(nn.Module)鐨勫尯鍒?
| 鏂瑰紡                   | 鎻忚堪             | 浼樼偣            | 缂虹偣             | 閫傜敤鍦烘櫙                               |
| -------------------- | -------------- | ------------- | -------------- | ---------------------------------- |
| `nn.Sequential`      | 椤哄簭鍫嗗彔灞?         | 绠€娲併€佹槗鍐?        | 涓嶇伒娲伙紝涓嶈兘鍐欏垎鏀€昏緫銆佽烦杩?| 绠€鍗曞墠棣堢綉缁滐紝濡?LeNet銆丮LP銆丄lexNet         |
| 鑷畾涔夌被锛堢户鎵?`nn.Module`锛?| 瀹炵幇 `forward()` | 鏋佸害鐏垫椿銆佽兘鍐欎换鎰忓墠鍚戦€昏緫 | 浠ｇ爜绋嶅鏉?         | 鍑犱箮鎵€鏈変腑楂樼骇妯″瀷锛屽 ResNet銆乀ransformer銆丷NN |
## AlexNet
2012骞达紝AlexNet鐨勯棶涓栭娆¤瘉鏄庝簡锛屽涔犲埌鐨勭壒寰佸彲浠ヨ秴杩囨墜宸ヨ璁＄殑鐗瑰緛銆?鐩歌緝浜嶭eNet锛?AlexNet瑕佹繁寰堝锛孉lexNet鐢卞叓灞傜粍鎴愶細浜斾釜鍗风Н灞傘€佷袱涓叏杩炴帴闅愯棌灞傚拰涓€涓叏杩炴帴杈撳嚭灞傘€?鍙︼紝AlexNet浣跨敤鎴戜滑鏈€鍠滄鐨凴eLU浣滀负婵€娲诲嚱鏁般€?### 鏋舵瀯锛?```python
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
鎴戜滑涓€鐩村湪鍙樻崲閫氶亾鏁帮細
鎴戜滑澧炲姞閫氶亾鏁帮紝鏄负浜嗭細
1. 鎻愬彇鏇翠赴瀵岀殑鐗瑰緛锛?2. 鍦ㄥ帇缂╃┖闂村昂瀵哥殑鍚屾椂缁存寔鐢氳嚦澧炲己琛ㄧず鑳藉姏锛?3. 浣跨綉缁滆兘璇嗗埆鏇村鏉傘€佹洿鎶借薄鐨勫唴瀹?鍙﹀锛屽浜嶢lexNet锛屾垜浠噰鐢ㄦ殏閫€娉曢槻姝㈣繃鎷熷悎


## VGG
鏍稿績鎬濇兂灏辨槸浣跨敤灏忓嵎绉唬鏇垮ぇ鍗风Н
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
浠庝唬鐮佸緢濂界湅鍑烘潵VGG鍧楀氨鏄嫢骞蹭釜鍗风Н灞傚拰ReLU鐨勫爢鍙狅紝涓旇繖涔嬩腑涓嶆秹鍙婇€氶亾鏁板彉鍖栵紝鏈€缁堝姞鍏ヤ竴涓渶澶ф睜鍖栥€?瀵逛簬VGG绁炵粡缃戠粶锛屾垜浠湁涓€涓秴鍙傛暟`conv_arch`瀹氫箟姣忎釜VGG鍧楃殑鍗风Н灞備釜鏁板拰杈撳嚭閫氶亾鏁般€?```python
conv_arch = ((1,64),(1,128),(2,256),(2,512),(2,512))
```
浠ユ涓轰緥锛屾垜浠疄鐜颁竴涓猇GG-11锛?```python
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
鍦ㄦ瘡涓儚绱犵殑閫氶亾涓婁娇鐢∕LP
```python

def nin_block(in_channels, kernel_size, strides, padding):
	return nn.Sequential(
		nn.Conv2d(in_channels, out_channels, kernel_size, strides, padding),
		nn.ReLU(),
		nn.Conv2d(out_channels, out_channels, kernel_size=1),nn.ReLU(),
		nn.Conv2d(out_channels, out_channels, kernel_size=1), nn.ReLU()
		)
```
鏋勫缓涓€涓狽iN锛?```python
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
骞朵笖锛孨iN鏈€缁堝苟娌℃湁鍏ㄨ繛鎺ュ眰锛岃繖鏄笌鍏朵粬褰㈠紡鐨勫嵎绉缁忕綉缁滀笉鍚屼箣澶勶紝浠栧彧闇€瑕佷竴涓叏灞€骞冲潎姹囪仛灞傘€?
## GoogleNet
### Inception鍧?杩欐槸涓€涓潪甯稿鏉傜殑缁撴瀯锛?![[Inception Block.png]]
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
## 鎵归噺瑙勮寖鍖?Batch Normalization)
鍙互鎸佺画鍔犳繁绁炵粡缃戠粶鐨勬敹鏁涢€熷害
璇锋敞鎰忥紝濡傛灉鎴戜滑灏濊瘯浣跨敤澶у皬涓?鐨勫皬鎵归噺搴旂敤鎵归噺瑙勮寖鍖栵紝鎴戜滑灏嗘棤娉曞鍒颁换浣曚笢瑗裤€傝繖鏄洜涓哄湪鍑忓幓鍧囧€间箣鍚庯紝姣忎釜闅愯棌鍗曞厓灏嗕负0銆傛墍浠ワ紝鍙湁浣跨敤瓒冲澶х殑灏忔壒閲忥紝鎵归噺瑙勮寖鍖栬繖绉嶆柟娉曟墠鏄湁鏁堜笖绋冲畾鐨勩€傝娉ㄦ剰锛屽湪搴旂敤鎵归噺瑙勮寖鍖栨椂锛屾壒閲忓ぇ灏忕殑閫夋嫨鍙兘姣旀病鏈夋壒閲忚鑼冨寲鏃舵洿閲嶈銆?1.  **鎵归噺褰掍竴鍖栨牳蹇冭繍绠?(BN):**
    $$
    \mathrm{BN}(\mathbf{x})=\boldsymbol{\gamma} \odot \frac{\mathbf{x}-\hat{\boldsymbol{\mu}}_{\mathcal{B}}}{\hat{\boldsymbol{\sigma}}_{\mathcal{B}}}+\boldsymbol{\beta}
    $$

2.  **灏忔壒閲忔牱鏈潎鍊?($\hat{\boldsymbol{\mu}}_{\mathcal{B}}$):**
    $$
    \hat{\boldsymbol{\mu}}_{\mathcal{B}}=\frac{1}{|\mathcal{B}|} \sum_{\mathbf{x} \in \mathcal{B}} \mathbf{x}
    $$

3.  **灏忔壒閲忔牱鏈柟宸?($\hat{\boldsymbol{\sigma}}_{\mathcal{B}}^{2}$):**
    $$
    \hat{\boldsymbol{\sigma}}_{\mathcal{B}}^{2}=\frac{1}{|\mathcal{B}|} \sum_{\mathbf{x} \in \mathcal{B}}(\mathbf{x}-\hat{\boldsymbol{\mu}}_{\mathcal{B}})^{2}+\epsilon
    $$
瀹炵幇锛?```python
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
绠€鍗曟潵璇达紝瀵逛簬涓€涓猂esNet锛屾垜浠殑鏁版嵁鍒嗕负涓ら儴鍒嗭紝涓€涓槸鏈€鍒濈殑鏁版嵁(鍙兘缁忚繃鍗曚綅鍗风Н灞傚彉鎹㈠舰鐘?鍜岀粡杩囨潈閲嶅眰鐨勬暟鎹紝瀵逛簬娈嬪樊鍧楋紝瀹炵幇濡備笅锛?![[ResNet.png]]
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
鎺ヤ笅鏉ユ垜浠疄鐜癛esNet锛?```python
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
鎺ヤ笅鏉ユ垜浠啀ResNet鍔犲叆鎵€鏈夋畫宸潡锛?```python
b2 = nn.Sequential(*resnet_block(64, 64, 2, first_block=True))
b3 = nn.Sequential(*resnet_block(64, 128, 2))
b4 = nn.Sequential(*resnet_block(128, 256, 2))
b5 = nn.Sequential(*resnet_block(256, 512, 2))

net = nn.Sequential(b1, b2, b3, b4, b5,
					nn.AdaptiveAvgPool2d((1,1)),
					nn.Flatten(), nn.Linear(512, 10))
```

## DenseNet
鍙互鐞嗚В涓篟esNet杩涢樁鐗堬紝瓒呯骇澶氶」寮忔嫙鍚堬紝瀹炵幇杩囩▼涓婄殑鍖哄埆鍦ㄤ簬锛屽浜嶥enseNet锛屾垜浠娇鐢╟oncatenate銆?```python
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
鐢变簬绋犲瘑鍧椾細甯︽潵閫氶亾鏁板鍔狅紝鎴戜滑浣跨敤鍗曚綅鍗风Н灞傚噺灏忛€氶亾鏁般€佷娇鐢╯tride=2骞冲潎姹犲寲鍑忓崐楂樺拰瀹斤細
```python
def transition_block(input_channels, num_channels):
	return nn.Sequential(
						nn.BatchNorm2d(input_channels), nn.ReLU(),
						nn.Conv2d(input_channels, num_channels, kernel_size=1),
						nn.AvgPool2d(kernel_size=2,stride=2))
```

鏋勫缓DenseNet鐨勬柟娉曚笌涓婅堪澶у悓灏忓紓锛屼笉鍐嶈禈杩般€
