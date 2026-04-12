---
title: "D2L Chapter 5"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false
categories:
  - "d2l"
---

杩欎竴绔犲苟娌℃湁浠嬬粛鏂扮殑妯″瀷鎴栨暟鎹泦锛屼富瑕佷粙缁嶄簡涓€浜泃orch鐨勬搷浣滐紝璧峰埌涓€涓壙涓婂惎涓嬨€?
## 鏋舵瀯
棣栧厛锛屾垜浠粠涓€绉嶆瘮杈冩娊璞＄殑灞傞潰鏉ヨ璁簆ytroch涓缁忕綉缁滅粨鏋勬槸濡備綍缁勭粐鏋勬垚鐨勩€?鍗曚釜绁炵粡缃戠粶鏄竴涓緢绠€鍗曠殑缁撴瀯锛屼粬鏈変笁涓姛鑳斤細鎺ュ彈杈撳叆锛屼骇鐢熻緭鍑猴紝鎼哄甫涓€浜涘弬鏁帮紝鑰屽浜庝竴涓狹LP涔熸槸涓€鏍风殑銆?涓轰簡瀹炵幇杩欎簺澶嶆潅鐨勭綉缁滐紝鎴戜滑寮曞叆浜嗙缁忕綉缁滃潡鐨勬蹇点€傚潡锛坆lock锛夊彲浠ユ弿杩板崟涓眰銆佺敱澶氫釜灞傜粍鎴愮殑缁勪欢鎴栨暣涓ā鍨嬫湰韬€備娇鐢ㄥ潡杩涜鎶借薄鐨勪竴涓ソ澶勬槸鍙互灏嗕竴浜涘潡缁勫悎鎴愭洿澶х殑缁勪欢锛岃繖涓€杩囩▼閫氬父鏄€掑綊鐨勩€?鑰屾垜浠竴鑸€氳繃class瀹炵幇block锛屽湪瀹氫箟鎴戜滑鑷繁鐨刡lock鏃讹紝鐢变簬鑷姩寰垎宸茬粡瀹氫箟浜嗕竴浜涘悗绔疄鐜帮紝鎴戜滑鍙渶瑕佽€冭檻鍓嶅悜浼犳挱鍜屼竴浜涘繀瑕佺殑鍙傛暟銆?### 鑷畾涔塨lock
鎴戜滑棣栧厛鏉ョ湅鐪媌lock鐨勪竴浜涘姛鑳斤細
1. 灏嗚緭鍏ユ暟鎹綔涓哄叾鍓嶅悜浼犳挱鍑芥暟鐨勫弬鏁般€?2. 閫氳繃鍓嶅悜浼犳挱鍑芥暟鏉ョ敓鎴愯緭鍑恒€傝娉ㄦ剰锛岃緭鍑虹殑褰㈢姸鍙兘涓庤緭鍏ョ殑褰㈢姸涓嶅悓銆備緥濡傦紝鎴戜滑涓婇潰妯″瀷涓殑绗竴涓叏杩炴帴鐨勫眰鎺ユ敹涓€涓?0缁寸殑杈撳叆锛屼絾鏄繑鍥炰竴涓淮搴︿负256鐨勮緭鍑恒€?3. 璁＄畻鍏惰緭鍑哄叧浜庤緭鍏ョ殑姊害锛屽彲閫氳繃鍏跺弽鍚戜紶鎾嚱鏁拌繘琛岃闂€傞€氬父杩欐槸鑷姩鍙戠敓鐨勩€?4. 瀛樺偍鍜岃闂墠鍚戜紶鎾绠楁墍闇€鐨勫弬鏁般€?5. 鏍规嵁闇€瑕佸垵濮嬪寲妯″瀷鍙傛暟銆?```python
class MLP(nn.Module):
	def __init__(self):
			super().__init__()
			self.hidden = nn.Linear(20, 256)
			self.out = nn.Linear(256, 20)

	def forward(self, X):
		return self.out(F.relu(self.hidden(X)))
```
### MySequential
鍦ㄦ垜浠洰鍓嶇殑鎯呭涓嬶紝鑷繁鍐欎竴涓猄equential鏉ヨ繛鎺ョ缁忕綉缁滅殑鍚勪釜灞傛槸娌′粈涔堝繀瑕佺殑锛屼絾鍒板悗闈㈡帴瑙﹀埌ResNet绛夌粨鏋勶紝杩欎篃鏄惧緱蹇呰浜?鍥犱负鎴戜滑瑕佽嚜宸卞啓forward)銆?```python
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
鎵€浠ワ紝鍏跺疄Sequential绫诲氨鏄竴涓垪琛ㄥ瓨鍌ㄨ繖浜涗釜灞傘€?## 鍙傛暟绠＄悊
### 鍙傛暟璁块棶
```python
net[2].state_dict()
>>>OrderedDict([('weight', tensor([[-0.0427, -0.2939, -0.1894, 0.0220, -0.1709, -0.1522, -0.0334, -0.2263]])), ('bias', tensor([0.0887]))])
```
杩欐牱璁块棶鐨勫氨鏄疢LP涓涓夊眰鐨勫弬鏁?鎴戜滑鍚屾牱鍙互绮剧粏鍦拌闂細
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
涓€娆℃€ц闂墍鏈夊弬鏁帮細
```python
print(*[(name, param.shape) for name, param in net[0].named_parameters()])
print(*[(name, param.shape) for name, param in net.named_parameters()])
```
```shell
('weight', torch.Size([8, 4])) ('bias', torch.Size([8]))
('0.weight', torch.Size([8, 4])) ('0.bias', torch.Size([8])) ('2.weight', torch.Size([1, 8])) ('2.bias',torch.Size([1]))
```
杩欎篃涓烘垜浠彁渚涗簡鍙︿竴绉嶈闂弬鏁扮殑鏂瑰紡锛?```python
net.state_dict()['2.bias'].data
>>>tensor([0.0887])
```
### 鍙傛暟鍒濆鍖?棣栧厛锛屾垜浠湁涓板瘜鐨勫唴缃垵濮嬪寲鍣細
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
褰撶劧浜嗭紝閫氳繃涓€鐐瑰皬灏忕殑淇敼灏卞彲浠ユ妸鍙傛暟鍒濆鍖栦负1
```python
	nn.init.constant_(m.weight, 1)
```
鎴戜滑杩樺彲浠ヤ娇鐢ㄥぇ鍚嶉紟榧庣殑Xavier鍒濆鍖栵細
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
鎴戜滑杩樻湁寤跺悗鍒濆鍖栵紝杩欎釜鏀惧湪CNN閮ㄥ垎闃愭槑
## 鑷畾涔夊眰
鎴戜滑鐩存帴璁ㄨ濡備綍瀹氫箟甯﹀弬鏁扮殑灞傦細
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

## 璇诲啓鏂囦欢
姣旇緝閲嶈鐨勫氨鏄瓨鍙傛暟鍜岃鍙栧弬鏁帮細
```python
#瀵逛簬涓€涓猲et缃戠粶
torch.save(net.state_dict(), 'mlp.params')#save
#濡備綍鎭㈠妯″瀷
clone = MLP()
clone.load_state_dict(torch.load('mlp.params'))
clone.eval()
```
## GPU
杩欓儴鍒嗗氨绾函gpu鎶€鏈枃妗ｏ紝鐜扮敤鐜版煡瀹屼簡
