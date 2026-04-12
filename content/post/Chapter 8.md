---
title: "D2L Chapter 8"
date: 2025-07-23T14:36:00+08:00
math: true
draft: false
categories:
  - "d2l"
---
杩欓儴鍒嗚瑙ｄ簡浼犵粺鐨凴NN锛屽叾涓秹鍙婄殑鍐呭鍜屽師鐞嗛潪甯稿锛岄渶瑕佷粩缁嗘⒊鐞嗐€?**棣栧厛锛孯NN鏄敤鏉ュ鐞嗗簭鍒楁暟鎹殑**
瀵逛簬涓€涓椂闂村簭鍒?x_{i}$锛屾垜浠兂瑕侀娴嬩粬鐨?x_{t}$锛屽彲浠ラ€氳繃缁撳悎杩囧線鏃堕棿姝ョ殑鏁版嵁棰勬祴锛?$$
x_t鈥嬧埣P(x_{t}鈥嬧垼x_{t鈭?}鈥?鈥?x_{1}鈥?
$$
鑰岃瀹炵幇杩欑绛栫暐锛屾垜浠湁浠ヤ笅鍑犱釜鏂瑰紡锛?## 鍥炲綊妯″瀷
涓昏鍒嗕负浠ヤ笅涓ょ绛栫暐锛?### 鑷洖褰掓ā鍨?鍦ㄧ幇瀹炴儏鍐典笅鐩稿綋闀跨殑搴忓垪$x_{t-1},\dots,x_{1}$鍙兘鏄笉蹇呰鐨勶紝鍥犳鎴戜滑鍙渶瑕佽娴嬫弧瓒抽暱搴?\tau$鐨勫簭鍒?$x_{t-1},\dots,x_{t-\tau}$鍗冲彲锛岃繖鏍锋湁涓€涓ソ澶勫氨鏄垜浠幏寰楃殑鍙傛暟鐨勬€婚噺鏄笉鍙樼殑銆?### 闅愬彉閲忚嚜鍥炲綊妯″瀷
淇濈暀瀵硅繃鍘昏娴嬬殑鎬荤粨$h_{t}$锛屽苟鍚屾椂鏇存柊$\hat{x}銆乭_t$锛岀敱浜?h_{t}$浠庢湭琚娴嬪埌锛屾墍浠ョО涓洪殣鍙橀噺鑷洖褰掓ā鍨嬨€?![[latent autoregressive model.png]]
浣嗗洖褰掓ā鍨嬫湁涓棶棰橈紝鎴戜滑涓嶈兘鐢ㄦ柊棰勬祴鍑烘潵鐨勭粨鏋滀綔涓洪娴嬬殑鏁版嵁锛岃繖浼氬鑷翠弗閲嶇殑鍋忕Щ銆?鑱斿悎姒傜巼妯″瀷锛?$$
P(x_1, x_2, \dots, x_T) = \prod_{t=1}^{T} P(x_t \mid x_1, \dots, x_{t-1})
$$
鑱斿悎姒傜巼妯″瀷澶毦绠椾簡锛岄拡瀵硅繖涓€鐐规垜浠繘琛屼紭鍖栥€?### 椹皵绉戝か姒傜巼妯″瀷锛?鎴戜滑绠€鍖栬涓?x_{t}$浠呬笌$x_{t-1}$鏈夊叧锛岄偅涔堟垜涔堝彲浠ュ緱鍒帮細
$$
P(x_1, x_2, \dots, x_T) = P(x_1) \cdot \prod_{t=2}^{T} P(x_t \mid x_{t-1})
$$
杩欏ぇ澶х畝鍖栦簡璁＄畻骞朵笖鍙互浣跨敤鍔ㄦ€佽鍒掔殑鏂规硶(琛ョエ)銆?鎴戜滑杩樺彲浠ヨ繘琛岃竟闄呭寲锛岃繖鏍锋垜浠笉鐢ㄧ煡閬撲腑闂寸姸鎬佸氨鍙互棰勬祴锛?$$
P(x_{t+1} \mid x_{t-1}) = \sum_{x_t} P(x_{t+1} \mid x_t) \cdot P(x_t \mid x_{t-1})
$$
鍏充簬**涓轰粈涔坘-step姣?-step鍏锋湁鍔ｅ娍鐨勮璁?*锛?- **涓嶇敤鏂扮敓鎴愭暟鎹?鈮?瀹规槗**锛涘畠鍙槸閬垮厤鈥滆緭鍏ユ薄鏌撯€濓紝浣嗘妸 **鏇撮暱鐨勬椂搴忎緷璧?* 鍜?**鏇村ぇ鐨勪笉纭畾鎬?* 涓€娆″帇缁欐ā鍨嬨€?- 濡傛灉鏁版嵁瑙勫緥绠€鍗曘€佹ā鍨嬭冻澶熷己锛宬鈥憇tep 浼氭洿绋充篃鍙兘鏇村噯锛涜嫢妯″瀷娆犳嫙鍚堟垨搴忓垪甯﹂珮鍣０锛屽垯 1鈥憇tep 婊氬姩鍙兘鍙嶈€屾晥鏋滄洿濂姐€?- 瀹炲姟涓父瑙佹姌琛凤細
    - **Scheduled鈥疭ampling / Professor鈥疐orcing**锛氳缁冩椂娣峰悎鐪熷€煎拰棰勬祴鍊硷紝璁╅€掑綊寮忔彁鍓嶅浼氬閿欙紱
    - **澶氬昂搴︽垨鍒嗘杈撳嚭**锛氱煭鏈熺洿鎺ャ€侀暱鏈熼€掑綊锛?    - **Seq2Seq + Attention**锛氭棦鍙竴娆′骇鍑哄姝ワ紝鍙堣妯″瀷鍐呴儴閫愭瑙ｇ爜銆?## 鏂囨湰棰勫鐞嗭細
杩欓儴鍒嗙函绾噯澶囧伐浣滐紝鍚湁浠ヤ笅鏂归潰锛?1. 灏嗘枃鏈綔涓哄瓧绗︿覆鍔犺浇鍒板唴瀛樹腑銆?2. 灏嗗瓧绗︿覆鎷嗗垎涓鸿瘝鍏冿紙濡傚崟璇嶅拰瀛楃锛夈€?3. 寤虹珛涓€涓瘝琛紝灏嗘媶鍒嗙殑璇嶅厓鏄犲皠鍒版暟瀛楃储寮曘€?4. 灏嗘枃鏈浆鎹负鏁板瓧绱㈠紩搴忓垪锛屾柟渚挎ā鍨嬫搷浣溿€?### 璇嶅厓鍖栵細
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
灏辨槸鎶婂畠鎷嗗紑锛屽彟锛岃繖閮ㄥ垎鐢ㄥ埌浜嗗緢澶氳凯浠ｅ櫒銆?### 璇嶈〃锛?鐢ㄦ潵灏嗚瘝鍏冩槧灏勫埌浠?寮€濮嬬殑绱㈠紩涓細
鎴戜滑鍏堝皢璁粌闆嗕腑鐨勬墍鏈夋枃妗ｅ悎骞跺湪涓€璧凤紝瀵瑰畠浠殑鍞竴璇嶅厓杩涜缁熻锛屽緱鍒扮殑缁熻缁撴灉绉颁箣涓鸿鏂欙紙corpus锛夈€傜劧鍚庢牴鎹瘡涓敮涓€璇嶅厓鐨勫嚭鐜伴鐜囷紝涓哄叾鍒嗛厤涓€涓暟瀛楃储寮?```python
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

## 璇█妯″瀷鍜屾暟鎹泦
### 璇█妯″瀷
鍦ㄧ粰瀹氱殑妯″瀷搴忓垪锛岃瑷€妯″瀷鐨勭洰鏍囨槸浼拌搴忓垪鑱斿悎姒傜巼锛屼絾浠栧彧鑳界敓鎴愬悎璇硶鐨勫彞瀛愯€岄潪鐞嗚В璇彞銆?### 椹皵绉戝か妯″瀷涓巒鍏冭娉?閫氬父锛屾秹鍙婁竴涓€佷袱涓拰涓変釜鍙橀噺鐨勬鐜囧叕寮忓垎鍒绉颁负涓€鍏冭娉曪紙unigram锛夈€佷簩鍏冭娉曪紙bigram锛夊拰涓夊厓璇硶锛坱rigram锛夋ā鍨嬨€?$$
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2)P(x_3)P(x_4),
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2 | x_1)P(x_3 | x_2)P(x_4 | x_3),
P(x_1, x_2, x_3, x_4) = P(x_1)P(x_2 | x_1)P(x_3 | x_1, x_2)P(x_4 | x_2, x_3)
$$


## RNN
![[latent RNN.png]]
