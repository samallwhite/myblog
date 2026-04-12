---
title: "动手学深度学习（第2-4章）"
date: 2025-07-19T20:00:00+08:00
math: true
draft: false
categories:
  - "动手学深度学习"
tags:
  - "PyTorch"
  - "D2L"
---

杩欐槸鎴戠涓€绡囧崥瀹€備富鎾彧鐢ㄤ簡2h鐪嬪畬浜嗕笁绔犲唴瀹癸紝鎯冲繀鏄潪甯哥殑涓嶇簿鐨勶紝浜庢槸鏁寸悊涓€涓嬨€傘€傘€?
# 2 棰勫鐭ヨ瘑
鏈儴鍒嗕富瑕佽瑙ｄ簡涓€浜沺ytorch鐨勫熀鏈搷浣滃拰涓€浜涙暟瀛︾煡璇嗭細
### Tensor鐨勫垵濮嬪寲鍜屼竴浜涚壒寰?鍒濆鍖栧紶閲忥細
```python
A = torch.tensor([[1,2,3],[4,5,6]])
```
濂戒簡锛屾垜浠幇鍦ㄥ凡缁忎細鍒涢€爐ensor浜嗭紝鎴戜滑鎺ヤ笅鏉ュ簲璇ュ皾璇曠潃鍘诲緱鍒颁竴浜涘叧浜庤繖涓猼ensor鐨勪俊鎭細
棣栧厛鍊煎緱鎻愬強鐨勬槸python鑷韩鐨刞len()`鏂规硶鍚屾牱閫傜敤锛屼絾璇ユ柟娉曞彧鑳芥樉绀虹0杞寸殑灏哄淇℃伅锛?```python
len(A)
```
```bash
2
```
鎵€浠ュ叾瀹炶繖鐜╂剰鎸哄瀮鍦撅紝鎴戜滑闇€瑕佺敤楂樼骇鏂规硶锛歚shape`
```python
A.shape
```
```bash
torch.Size([2,3])
```
鎴戜滑鍙互閫氳繃浣跨敤`reshape()`鏂规硶鏀瑰彉寮犻噺鐨勭粨鏋勶細
```python
a = A.reshape(3,2)
```
```bash
tensor([[1,2],[3,4],[5,6]])
```
鍍弔ensorflow涓€鏍凤紝鎴戜滑鍙互鎼瀘nes銆亃eros绛変笢瑗匡細
```python
zero_tensor = torch.zero((2,3))
one_tensor = torch.ones((2,3))
```
鐢熸垚闅忔満寮犻噺锛?```python
random_tensor = torch.randn((2,3))
```
btw锛岃繖涓殢鏈烘槸浠庢鎬佸垎甯冧腑闅忔満鎶芥牱鐨?### Tensor鐨勪竴浜涜繍绠楋細
鍩烘湰鐨勮繍绠楁病鍟ュ尯鍒紝鎴戜滑鐗瑰埆璇翠竴涓嬭繛鎺?concatenate)
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
姹傚拰锛?```python
A.sum()
```
(闈炲父鐨勬湸瀹炴棤鍗?
浣犲彲浠ュ緢鏂逛究鐨勫皢寮犻噺璺焠parray杞崲锛?```python
A = X.numpy()
B = torch.tensor(A)
```
### 濡備綍璇诲彇鏁版嵁锛?鍏堝埆绠￠偅浜涙湁鐨勬病鐨勶紝鍋囪鎴戜滑宸茬粡鏈変簡input鍜宱utput浜嗭細
```python
inputs, outputs = data.iloc[:, 0:2], data.iloc[:, 2]
```
鎺ヤ笅鏉ュ彧闇€瑕佽繖鏍凤細
```python
X = torch.tensor(inputs.to_numpy(dtype=float))
y = torch.tensor(outputs.to_numpy(dtype=float))
```

### 涓€浜涚嚎浠ｇ煡璇嗭細
棣栧厛锛屾垜浠粙缁岺adamard绉?鈯?锛?
$$
\mathbf{A} 鈯橽mathbf{B} =
\begin{bmatrix}
a_{11}b_{11} & a_{12}b_{12} & \cdots & a_{1n}b_{1n} \\
a_{21}b_{21} & a_{22}b_{22} & \cdots & a_{2n}b_{2n} \\
\vdots       & \vdots       & \ddots & \vdots       \\
a_{m1}b_{m1} & a_{m2}b_{m2} & \cdots & a_{mn}b_{mn}
\end{bmatrix}
$$

璇adamard绉彲浠ヨ〃绀轰负锛?```python
A * B
```
鎺ヤ笅鏉ユ槸闄嶇淮鎿嶄綔锛屾垜浠墍鎸囩殑闄嶇淮鎿嶄綔鏄€氳繃姹傚拰褰㈠紡鏉ュ疄鐜扮殑锛屽氨鍍忓垰鎵嶆墍鎻愬埌鐨剆um涓€鏍凤紝鎴戜滑鍚屾牱浣跨敤sum锛屼笉杩囧浜嗕竴浜涘弬鏁帮細
```python
A_sum_axis0 = A.sum(axis = 0) #瀵圭0杞存眰鍜?```
浣嗘垜浠粛鐒跺彲浠ュ湪姹傚拰杩囩▼涓繚鐣欑淮搴︼細
```python
sum_A = A.sum(axis = 0, keepdims = true)
```
杩欏緢鏈夌敤锛屼粬鍙互甯垜浠疄鐜板钩鍧囨鍒欏寲锛?```python
A / sum_A
```
```bash
tensor([[0.0000, 0.1667, 0.3333, 0.5000],
        [0.1818, 0.2273, 0.2727, 0.3182],
        [0.2105, 0.2368, 0.2632, 0.2895],
		[0.2222, 0.2407, 0.2593, 0.2778],
		[0.2286, 0.2429, 0.2571, 0.2714]])
```
鑻ヨ璁＄畻绱鎬诲拰锛屾垜浠簲璇ョ敤cumsum锛?```python
A.cumsum(axis=0)
>>
>>tensor([[ 0., 1., 2., 3.],
[ 4., 6., 8., 10.],
[12., 15., 18., 21.],
[24., 28., 32., 36.],
[40., 45., 50., 55.]])
```
鐐圭Н(dot product):
```python
torch.dot(x,y)
```

鐭╅樀-鍚戦噺绉細
鐭╅樀鍙互琛ㄧず涓鸿鍚戦噺浣滀负鍏冪礌鐨勫垪鍚戦噺锛?

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


鍦ㄦ垜浠湁涓€涓悓缁村害鍚戦噺X鐨勬儏鍐典笅:


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


杩欏氨鏄煩闃?鍚戦噺绉細
```python
troch.mv(A, x)
```
鐭╅樀涔樻硶锛?鏃犻渶澶氳█锛?```python
torch.mm(X, Y)
```
**鑼冩暟(norm)**锛?杩欎釜姣旇緝灞岋紝涔熸瘮杈冮噸瑕?鎴戜滑棣栧厛璇翠竴涓?L_{2}$鑼冩暟锛屽叾瀹炶繖涓寖鏁板氨鏄鍓嶅鍒扮殑鍚戦噺鐨勬ā锛?
$$
\|\mathbf{x}\|_2 = \sqrt{ \sum_{i=1}^n x_i^2 }
\tag{2.3.14}
$$

瀵逛簬$L_{2}$鑼冩暟锛岄€氬父鐪佺暐$L$锛?```python
torch.norm(x)
```
涓庢鍚屾椂锛屾垜浠繕鏈?L_{1}$鑼冩暟锛?
$$
\|\mathbf{x}\|_1 = \sum_{i=1}^n |x_i|
\tag{2.3.15}
$$

涓?L_{2}$鑼冩暟鐩告瘮锛?L_{1}$鑼冩暟鍙楀紓甯稿€肩殑褰卞搷杈冨皬锛?```python
#鎴戜滑娌℃湁鍙互鐩存帴璁＄畻L1鑼冩暟鐨勬柟娉?torch.abs(u).sum()
```
鑰屼互涓婃墍鎻愬強鐨?L_{1}$鑼冩暟涓?L_{2}$鑼冩暟閮芥槸$L_{p}$鑼冩暟鐨勭壒渚嬶細

$$
\|\mathbf{x}\|_p = \left( \sum_{i=1}^{n} |x_i|^p \right)^{1/p}
\tag{2.3.16}
$$

鍚屾椂锛屾垜浠繕缁欏嚭Frobenius鑼冩暟锛岃繖涓寖鏁版槸鐭╅樀鐨勫厓绱犲钩鏂瑰拰鐨勫钩鏂规牴锛屽氨濂藉儚鏄煩闃电殑$L_{2}$鑼冩暟锛?
$$
\|\mathbf{X}\|_F = \sqrt{ \sum_{i=1}^{m} \sum_{j=1}^{n} x_{ij}^2 }
\tag{2.3.17}
$$

鎯宠姹傝ВFrobenius鑼冩暟涔熷緢绠€鍗曪細
```python
torch.norm(Mat)
```
鑼冩暟鍜岀洰鏍?鍦ㄦ繁搴﹀涔犱腑锛屾垜浠粡甯歌瘯鍥捐В鍐充紭鍖栭棶棰橈細鏈€澶у寲鍒嗛厤缁欒娴嬫暟鎹殑姒傜巼; 鏈€灏忓寲棰勬祴鍜岀湡瀹炶娴嬩箣闂寸殑璺濈銆傜敤鍚戦噺琛ㄧず鐗╁搧锛堝鍗曡瘝銆佷骇鍝佹垨鏂伴椈鏂囩珷锛夛紝浠ヤ究鏈€灏忓寲鐩镐技椤圭洰涔嬮棿鐨勮窛绂伙紝鏈€澶у寲涓嶅悓椤圭洰涔嬮棿鐨勮窛绂汇€傜洰鏍囷紝鎴栬鏄繁搴﹀涔犵畻娉曟渶閲嶈鐨勭粍鎴愰儴鍒嗭紙闄や簡鏁版嵁锛夛紝閫氬父琚〃杈句负鑼冩暟銆?
### 娌℃湁涓€浜涘井绉垎鐭ヨ瘑
鍥犱负涓诲寘杩樿寰?
### 鑷姩寰垎
鏁板鍘熺悊閮ㄥ垎绛夋垜瀛︿簡鐭╅樀鍒嗘瀽鍐嶈鍚?鐩存帴涓婁唬鐮侊細
```python
x = torch.arange(4.0)
>>> tensor([0.,1.,2.,3.])
x.requires_grad_(True)
x.grad # 榛樿鍊兼槸None
#鏁翠竴涓獃
y = torch.dot(x,x)
y
>>>tensor(28., grad_fn=<MulBackward0>)
y.backward()#鍙嶅悜浼犳挱鍑芥暟璁＄畻姣忎釜姊害
>>>tensor([ 0., 4., 8., 12.])
```
闈炴爣閲忓彉閲忕殑鍙嶅悜浼犳挱锛?褰搚涓嶆槸鏍囬噺鏃讹紝鍚戦噺y鍏充簬鍚戦噺x鐨勫鏁扮殑鏈€鑷劧瑙ｉ噴鏄竴涓煩闃点€傚浜庨珮闃跺拰楂樼淮鐨剏鍜寈锛屾眰瀵肩殑缁撴灉鍙互鏄竴涓珮闃跺紶閲忋€?褰撹皟鐢ㄥ悜閲忕殑鍙嶅悜璁＄畻鏃讹紝鎴戜滑閫氬父浼氳瘯鍥捐绠椾竴鎵硅缁冩牱鏈腑姣忎釜缁勬垚閮ㄥ垎鐨勬崯澶卞嚱鏁扮殑瀵兼暟銆傝繖閲岋紝鎴戜滑鐨勭洰鐨勪笉鏄绠楀井鍒嗙煩闃碉紝鑰屾槸鍗曠嫭璁＄畻鎵归噺涓瘡涓牱鏈殑鍋忓鏁颁箣鍜屻€?```python
# 瀵归潪鏍囬噺璋冪敤backward闇€瑕佷紶鍏ヤ竴涓猤radient鍙傛暟锛岃鍙傛暟鎸囧畾寰垎鍑芥暟鍏充簬self鐨勬搴︺€?# 鏈緥鍙兂姹傚亸瀵兼暟鐨勫拰锛屾墍浠ヤ紶閫掍竴涓?鐨勬搴︽槸鍚堥€傜殑
x.grad.zero_()
y = x * x
# 绛変环浜巠.backward(torch.ones(len(x)))
y.sum().backward()
x.grad
```

鍏充簬涓轰粈涔堟垜浠瀵箉.sum杩涜鑷姩寰垎锛?瀵逛簬锛?

$$
\mathbf{y} = f(\mathbf{x}) \in \mathbb{R}^n
$$


閭ｄ箞锛?

$$
\nabla_{\mathbf{x}} \mathbf{y} =
\begin{bmatrix}
\frac{\partial y_1}{\partial x_1} & \cdots & \frac{\partial y_1}{\partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial y_n}{\partial x_1} & \cdots & \frac{\partial y_n}{\partial x_n}
\end{bmatrix}
\in \mathbb{R}^{n \times n}
$$

杩欐槸涓€涓?**Jacobian锛堥泤鍙瘮鐭╅樀锛?*锛岃€屼笉鏄竴涓畝鍗曠殑鍚戦噺銆?鎵€浠ュ鏋滄垜浠笉鎸囧畾缁村害鐨勮瘽锛宲ytorch灏变笉鐭ラ亾浠ヤ粈涔堟柟鍚戝井鍒嗐€?
## 3.绾挎€х缁忕綉缁?### 绾挎€у洖褰掞細
#### 涓€浜涙湳璇細
鍦ㄦ満鍣ㄥ涔犵殑鏈涓紝璇ユ暟鎹泦绉颁负璁粌鏁版嵁闆嗭紙training data set锛夋垨璁粌闆嗭紙training set锛夈€傛瘡琛屾暟鎹紙姣斿涓€娆℃埧灞嬩氦鏄撶浉瀵瑰簲鐨勬暟鎹級绉颁负鏍锋湰锛坰ample锛夛紝涔熷彲浠ョО涓烘暟鎹偣锛坉atapoint锛夋垨鏁版嵁鏍锋湰锛坉ata instance锛夈€傛垜浠妸璇曞浘棰勬祴鐨勭洰鏍囷紙姣斿棰勬祴鎴垮眿浠锋牸锛夌О涓烘爣锛坙abel锛夋垨鐩?锛坱arget锛夈€傞娴嬫墍渚濇嵁鐨勮嚜鍙橀噺锛堥潰绉拰鎴块緞锛夌О涓虹壒寰侊紙feature锛夋垨鍗忓彉閲忥紙covariate锛夈€?
鍦ㄦ満鍣ㄥ涔犻鍩燂紝鎴戜滑閫氬父浣跨敤鐨勬槸楂樼淮鏁版嵁闆嗭紝寤烘ā鏃堕噰鐢ㄧ嚎鎬т唬鏁拌〃绀烘硶浼氭瘮杈冩柟渚裤€傚綋鎴戜滑鐨勮緭鍏ュ寘鍚?\( d \) 涓壒寰佹椂锛屾垜浠皢棰勬祴缁撴灉锛堥€氬父浣跨敤鈥滃皷瑙掆€濈鍙疯〃绀?\( y \) 鐨勪及璁″€硷級琛ㄧず涓猴細

$$
\hat{y} = w_1 x_1 + \cdots + w_d x_d + b
\tag{3.1.2}
$$



灏嗘墍鏈夌壒寰佹斁鍒板悜閲?\( $\mathbf{x} \in \mathbb{R}^d$\) 涓紝骞跺皢鎵€鏈夋潈閲嶆斁鍒板悜閲?( $\mathbf{w} \in \mathbb{R}^d$) 涓紝鎴戜滑鍙互鐢ㄧ偣绉舰寮忔潵绠€娲佸湴琛ㄨ揪妯″瀷锛?
$$
\hat{y} = \mathbf{w}^\top \mathbf{x} + b
\tag{3.1.3}
$$


鍦?(3.1.3) 涓紝鍚戦噺 $\mathbf{x}$瀵瑰簲浜庡崟涓暟鎹牱鏈殑鐗瑰緛銆傜敤绗﹀彿琛ㄧず鐨勭煩闃?$\mathbf{X} \in \mathbb{R}^{n \times d}$ 鍙互寰堟柟渚垮湴寮曠敤鎴戜滑鏁翠釜鏁版嵁闆嗙殑 (n)涓牱鏈€傚叾涓紝 $\mathbf{x}$鐨勬瘡涓€琛屾槸涓€涓牱鏈紝姣忎竴鍒楁槸涓€绉嶇壒寰併€?瀵逛簬鐗瑰緛闆嗗悎 $\mathbf{x}$锛岄娴嬪€?$\hat{\mathbf{y}} \in \mathbb{R}^n$鍙互閫氳繃鐭╅樀-鍚戦噺涔樻硶琛ㄧず涓猴細

$$
\hat{\mathbf{y}} = \mathbf{X} \mathbf{w} + b
\tag{3.1.4}
$$

閭ｄ箞鍒扮洰鍓嶄负姝紝鎴戜滑宸茬粡鏈変簡棰勬湡鐨勬ā鍨嬪舰寮忥紝鎴戜滑鐜板湪杩橀渶瑕佷袱浠朵簨锛氭ā鍨嬭川閲忕殑搴﹂噺鏂瑰紡鍜屾洿鏂版ā鍨嬩互鎻愰珮璐ㄩ噺鐨勬柟娉曘€?瀵逛簬搴﹂噺鏂瑰紡锛屾垜浠彁鍑烘崯澶卞嚱鏁?loss function):

$$
l^{(i)}(\mathbf{w}, b) = \frac{1}{2} \left( \hat{y}^{(i)} - y^{(i)} \right)^2
$$


涓轰簡琛￠噺鍦ㄦ暣涓暟鎹泦涓婄殑鎹熷け锛屾垜浠loss鍙栧潎鍊硷紝鑾峰緱鍧囨柟璇樊(MSE):


$$
L(\mathbf{w}, b) = \frac{1}{n} \sum_{i=1}^{n} \frac{1}{2} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)^2
$$


鎵€浠ユ垜浠殑鐩殑鍏跺疄鏄細


$$
\mathbf{w}^*, b^* = \mathop{\arg\min}_{\mathbf{w}, b} L(\mathbf{w}, b)
$$

瀵逛簬鏇存柊妯″瀷锛屾垜浠噰鐢ㄦ搴︿笅闄嶏細
姊害涓嬮檷鏈€绠€鍗曠殑鐢ㄦ硶鏄绠楁崯澶卞嚱鏁帮紙鏁版嵁闆嗕腑鎵€鏈夋牱鏈殑鎹熷け鍧囧€硷級鍏充簬妯″瀷鍙傛暟鐨勫鏁帮紙鍦ㄨ繖閲屼篃鍙互绉颁负姊害锛夈€備絾瀹為檯涓殑鎵ц鍙兘浼氶潪甯告參锛氬洜涓哄湪姣忎竴娆℃洿鏂板弬鏁颁箣鍓嶏紝鎴戜滑蹇呴』閬嶅巻鏁翠釜鏁版嵁闆嗐€傚洜姝わ紝鎴戜滑閫氬父浼氬湪姣忔闇€瑕佽绠楁洿鏂扮殑鏃跺€欓殢鏈烘娊鍙栦竴灏忔壒鏍锋湰锛岃繖绉嶅彉浣撳彨鍋?**灏忔壒閲忛殢鏈烘搴︿笅闄嶏紙minibatchstochastic gradient descent锛夈€?*
**鏉冮噸鍚戦噺鏇存柊鍏紡锛?*


$$
\mathbf{w} \leftarrow \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$


**鍋忕疆鏇存柊鍏紡锛?*


$$
b \leftarrow b - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

鍦ㄤ笂寮忎腑锛?\eta$鏄涔犵巼锛?\mathcal{B}$鏄瘡涓€涓皬鎵归噺鐨勬暟鎹釜鏁帮紝涔熷彨batch size锛岃繖涓や釜鏄秴鍙傛暟锛屾槸**鎵嬪姩璋冩暣**鐨勩€?鎴戜滑鍙互閫氳繃楂樻柉鍣０姝ｆ€佸垎甯冨拰鏋佸ぇ浼肩劧浼拌鏉ヨ瘉鏄嶮SE鐨勫悎鐞嗘€э紝浣嗘湰绗旇涓嶅璧樿堪銆傛垜鍦ㄨ瘉鏄庤繃绋嬩腑鍙戠幇浜嗕竴涓碉紝杩欐湁鐐规剰鎬濓細瀹?鍦ㄦ帹鐞嗘垨閲囨牱涓墠鏄惧紡鍑虹幇
濡傛灉浣犳槸鎼?*璐濆彾鏂缓妯?*鎴?*鐢熸垚妯″瀷**锛屾瘮濡傦細
- 璐濆彾鏂嚎鎬у洖褰?- GAN / VAE
- Diffusion Models
杩欐椂鍊欎綘浼氱湅鍒?系 琚綋浣溾€滃櫔澹版簮鈥濇潵鏄惧紡寤烘ā锛岀敤浜庯細
- 妯℃嫙涓嶇‘瀹氭€э紱
- 鍋氶噰鏍锋垨閲嶆瀯銆?#### 绠€鍗曞疄鐜?(涔︿腑鏈塻crach閮ㄥ垎锛屼絾鎴戜笉鍠滄閫犺疆瀛愶紝鎴戝枩娆㈢敤杞瓙)
鎴戜滑鍏堢粰瀹氱湡鐨剋鍜宐鐢ㄤ互鐢熸垚鏁版嵁iterator锛?```python
true_w = torch.tensor([2, -3.4])
true_b = 4.2
features, labels = d2l.synthetic_data(true_w, true_b, 1000)
def load_array(data_arrays, batch_size, is_train=True): #@save
"""鏋勯€犱竴涓狿yTorch鏁版嵁杩唬鍣?""
	dataset = data.TensorDataset(*data_arrays)
	return data.DataLoader(dataset, batch_size, shuffle=is_train)
```
瀹氫箟妯″瀷锛?```python
from torch import nn
net = nn.Sequential(nn.Linear(2,1))
```
鍏朵腑Linear鐨勭涓€涓弬鏁版槸杈撳叆缁村害锛岀浜屼釜鍙傛暟鏄緭鍑虹淮搴?鍒濆鍖栨ā鍨嬪弬鏁帮細
```python
net[0].weight.data.normal_(0, 0.01)
net[1].bias.data.fill_(0)
```
閫氳繃net\[0]閫夋嫨缃戠粶涓殑绗竴涓浘灞傦紝鐒跺悗浣跨敤weight.data鍜宐ias.data鏂规硶璁块棶鍙傛暟銆?瀹氫箟鎹熷け鍑芥暟锛?```python
loss = nn.MSELoss()
```
瀹氫箟浼樺寲绠楁硶锛?```python
trainer = torch.optim.SGD(net.parameters(), lr=0.03)
```
鏁翠綋璁粌娴佺▼锛?```python
num_epochs = 3
for epoch in range(num_epochs):
	for X, y in data_iter:
		l = loss(net(X) ,y)
		trainer.zero_grad()
		l.backward()
		trainer.step()#杩欎竴姝ヨ繘琛屽弬鏁版洿鏂?	l = loss(net(features), labels)
	print(f'epoch {epoch + 1}, loss {l:f}')
```

### Softmax鍥炲綊锛?softmax鍥炲綊瑙ｅ喅鐨勬槸鍒嗙被闂锛屽畠涔熸槸涓€绉嶅崟灞傜缁忕綉缁滐紝杈撳嚭灞傚悓鏍锋槸鍏ㄨ繛鎺ュ眰銆?
$$
\hat{\mathbf{y}} = \text{softmax}(\mathbf{o}), \quad \text{鍏朵腑} \quad \hat{y}_j = \frac{\exp(o_j)}{\sum_k \exp(o_k)}
$$

softmax杩愮畻鑾峰彇涓€涓悜閲忓苟灏嗗叾鏄犲皠涓烘鐜囥€?鍦ㄦ閮ㄥ垎鎴戜滑浠嶇劧鏈夊皬鎵归噺澶勭悊锛屼粬澶уぇ鎻愰珮浜唌v杩愮畻銆?#### 浜ゅ弶鐔垫崯澶卞嚱鏁?
$$
l = - \sum_{j=1}^{q} y_j \log(\hat{y}_j)
$$

#### 鍥剧墖鏁版嵁闆嗙殑璇诲彇
鎴戜滑鍙互閫氳繃torch鍐呯疆鐨勬柟娉曡鍙栵細
```python
trans = transforms.ToTensor()
mnist_train = torchvision.datasets.FashionMNIST(
				root="../data", train=True, transform=trans, download=True)
mnist_test = torchvision.datasets.FashionMNIST(
				root="../data", train=False, transform=trans, download=True)
```
褰撶劧锛屾垜浠彲浠ヨ幏鍙栬繖涓暟鎹殑涓€浜涢噺锛?```python
len(mnist_train), len(mnist_test)
>>>(60000, 10000)
mnist_train[0][0].shape
>>>torch.Size([1, 28, 28])
```
鍏跺疄鐜板湪杩欎釜train鎴杢est鏄竴涓?缁村紶閲?```bash
tensor([60000,1,28,28])
```
鍏充簬涓轰粈涔堜笉鎶婄淮搴︿负1鐨勬秷闄ゆ帀锛岃繘琛宑oncatenate锛実pt鏄繖涔堣鐨勶細

| 椤圭洰 | 瑙ｉ噴 |
|------|------|
| `nn.Conv2d(in_channels, out_channels, kernel_size)` | 瑕佹眰杈撳叆蹇呴』鏄?4D 寮犻噺 \`[N, C, H, W]\`锛屽摢鎬?\`C=1\`锛屼篃蹇呴』鍐欐垚 \`[1, 28, 28]\`銆?|
| `DataLoader` 杈撳嚭 | 鑷姩缁勭粐鎴?\`[batch_size, 1, 28, 28]\`锛屾柟渚垮悗缁搷浣溿€?|
| 澶氫换鍔?杩佺Щ瀛︿範 | 濡傛灉浠ュ悗澶勭悊褰╄壊鍥惧儚锛屼繚鐣?\`channel\` 鏇村ソ鎵╁睍銆?|
| `BatchNorm2d` / `MaxPool2d` 绛?| 閮借姹傛樉寮忛€氶亾缁达紝鍚﹀垯鎶ラ敊銆?|
#### softmax绠€娲佸疄鐜帮細
鍒濆鍖?```python
# PyTorch涓嶄細闅愬紡鍦拌皟鏁磋緭鍏ョ殑褰㈢姸銆傚洜姝わ紝
# 鎴戜滑鍦ㄧ嚎鎬у眰鍓嶅畾涔変簡灞曞钩灞傦紙flatten锛夛紝鏉ヨ皟鏁寸綉缁滆緭鍏ョ殑褰㈢姸
net = nn.Sequential(nn.Flatten(), nn.Linear(784, 10))#灏?*28*28灞曞钩鎴?84
def init_weights(m):
if type(m) == nn.Linear:
nn.init.normal_(m.weight, std=0.01)
net.apply(init_weights);
```
瀹氫箟鎹熷け锛?```python
loss = nn.CrossEntropyLoss(reduction='none')
```
鍏充簬reduction鍙傛暟锛?
|鍙栧€紎鍚箟|
|---|---|
|`'none'`|涓嶈繘琛屾眹鎬伙紝**淇濈暀姣忎釜鏍锋湰鐨勬崯澶卞€?*锛岃緭鍑?shape 涓?`[batch_size]`銆倈
|`'mean'`锛堥粯璁わ級|**瀵规墍鏈夋牱鏈殑鎹熷け鍙栧钩鍧?*锛岃緭鍑烘槸涓€涓爣閲忋€傚父鐢ㄤ簬澶у鏁拌缁冨満鏅€倈
|`'sum'`|瀵规墍鏈夋牱鏈殑鎹熷け**鐩存帴姹傚拰**锛岃緭鍑烘槸涓€涓爣閲忋€倈
浼樺寲渚濇棫锛?```python
trainer = torch.optim.SGD(net.parameters(), lr=0.1)
```
璁粌鎬昏锛?```python
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

            # 鍓嶅悜浼犳挱
            y_hat = net(X)
            loss = loss_fn(y_hat, y)

            # 鍙嶅悜浼犳挱涓庝紭鍖?            optimizer.zero_grad()
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

## 4 澶氬眰鎰熺煡鏈?MLP)
灏辨槸绁炵粡鍏冨彔涔愰珮銆?鎴戜滑鐜板湪鏈変簡涓€浜涙潈濞佺殑鍑芥暟锛歊eLU,tanh,sigmoid绛夌瓑锛孯eLU姣旇緝鏉冨▉鍥犱负濂界畻銆?璋冪敤ReLU涔熷緢瀹规槗锛?```python
y = torch.relu(x)
```
鑰屼笖锛屽畠鐨勬眰瀵艰〃鐜板緱鐗瑰埆濂斤細瑕佷箞璁╁弬鏁版秷澶憋紝瑕佷箞璁╁弬鏁伴€氳繃銆傝繖浣垮緱浼樺寲琛ㄧ幇寰楁洿濂斤紝骞?涓擱eLU鍑忚交浜嗗洶鎵颁互寰€绁炵粡缃戠粶鐨勬搴︽秷澶遍棶棰樸€?
### MLP绠€娲佸疄鐜?```python
net = nn.Sequential(nn.Flatten(),
					nn.Linear(784, 256),
					nn.ReLU(),
					nn.Linear(256, 10))
def init_weights(m):
	if type(m) == nn.Linear:
		nn.init.normal_(m.weight, std=0.01)
		
net.apply(init_weights);
```
閭ｄ箞寰堝鏄撶煡閬撹繖鏄釜涓ゅ眰鐨勭缁忕綉缁滐紝鐜板厛灞曞钩->绾挎€у眰->ReLU->绾挎€у眰->杈撳嚭.
璁粌杩囩▼鐨勫疄鐜颁笌鎴戜滑瀹炵幇softmax鍥炲綊鏃跺畬鍏ㄧ浉鍚岋紝杩欑妯″潡鍖栬璁′娇鎴戜滑鑳藉灏嗕笌妯″瀷鏋舵瀯鏈夊叧鐨勫唴瀹圭嫭绔嬪嚭鏉ワ細
```python
batch_size, lr, num_epochs = 256, 0.1, 10
loss = nn.CrossEntropyLoss(reduction='none')
trainer = torch.optim.SGD(net.parameters(), lr=lr)
```
<font color="#ff0000">鐩墠涓烘锛屾垜浠凡缁忓彲浠ユ€荤粨鍑鸿缁冧竴涓ā鍨嬬殑鍩烘湰娴佺▼浜?/font>[[train_mlp_pytorch]]
### 瀵逛簬妯″瀷鐨勮皟鏁?- 鎴戜滑鎸囧嚭锛岃缁冨彲鑳戒細瀵艰嚧杩囨嫙鍚堬紝涔熷氨鏄缁冭宸檷浣庤€屾硾鍖栬宸崌楂樸€?- 鍘熷垯涓婏紝鍦ㄦ垜浠‘瀹氭墍鏈夌殑瓒呭弬鏁颁箣鍓嶏紝鎴戜滑涓嶅笇鏈涚敤鍒版祴璇曢泦銆傚鏋滄垜浠湪妯″瀷閫夋嫨杩囩▼涓娇鐢ㄦ祴璇曟暟鎹紝鍙兘浼氭湁杩囨嫙鍚堟祴璇曟暟鎹殑椋庨櫓锛岄偅灏遍夯鐑﹀ぇ浜嗐€傚鏋滄垜浠繃鎷熷悎浜嗚缁冩暟鎹紝杩樺彲浠ュ湪娴嬭瘯鏁版嵁涓婄殑璇勪及鏉ュ垽鏂繃鎷熷悎銆備絾鏄鏋滄垜浠繃鎷熷悎浜嗘祴璇曟暟鎹紝鎴戜滑鍙堣鎬庝箞鐭ラ亾鍛紵
- 瑙ｅ喅姝ら棶棰樼殑甯歌鍋氭硶鏄皢鎴戜滑鐨勬暟鎹垎鎴愪笁浠斤紝闄や簡璁粌鍜屾祴璇曟暟鎹泦涔嬪锛岃繕澧炲姞涓€涓獙璇佹暟鎹泦锛坴alidation data set锛夛紝涔熷彨楠岃瘉闆嗭紙validation set锛夈€?- 娴佽鐨勮В鍐虫柟妗堟槸閲囩敤**K鎶樹氦鍙夐獙璇?*
#### 姝ｅ垯鍖?鏉冮噸琛板噺鏄渶骞挎硾搴旂敤鐨勬鍒欏寲鏂规硶锛岄€氬父涔熻绉颁负$L_{2}$姝ｅ垯鍖栥€?涓轰簡鎯╃綒鏉冮噸鍚戦噺鐨勫ぇ灏忥紝鎴戜滑蹇呴』浠ユ煇绉嶆柟寮忓湪鎹熷け鍑芥暟涓坊鍔犫垾w鈭?锛屼絾鏄ā鍨嬪簲璇ュ浣曞钩琛¤繖涓柊鐨勯澶栨儵缃氱殑鎹熷け锛熷疄闄呬笂锛屾垜浠€氳繃姝ｅ垯鍖栧父鏁拔绘潵鎻忚堪杩欑鏉冭　锛岃繖鏄竴涓潪璐熻秴鍙傛暟锛屾垜浠娇鐢ㄩ獙璇佹暟鎹嫙鍚堬細

$$
L(\mathbf{w}, b) + \frac{\lambda}{2} \|\mathbf{w}\|^2
$$

鎴戜滑閲囩敤$L_{2}$鑼冩暟涓昏鏄洜涓?L_{2}$姝ｅ垯鍒嗗竷鏋勬垚缁忓吀鐨勫箔鍥炲綊锛岃繖浣垮緱鎴戜滑鐨勫涔犵畻娉曞亸鍚戜簬鍦ㄥぇ閲忕壒寰佷笂鍧囧寑鍒嗗竷鏉冮噸鐨勬ā鍨嬨€?鎵€浠ユ垜浠湁浜嗘柊鐨凷GD锛?
$$
\mathbf{w} \leftarrow (1 - \eta \lambda) \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

姝?\lambda$涔熸槸瓒呭弬鏁般€?#### 鏆傞€€娉?鏆傞€€娉曞湪鍓嶅悜浼犳挱杩囩▼涓紝璁＄畻姣忎竴鍐呴儴灞傜殑鍚屾椂涓㈠純涓€浜涚缁忓厓銆傜敤鏉ラ伩鍏嶈繃鎷熷悎
```python
net = nn.Sequential(nn.Flatten(),
					nn.Linear(784, 256),
					nn.ReLU(),
					# 鍦ㄧ涓€涓叏杩炴帴灞備箣鍚庢坊鍔犱竴涓猟ropout灞?					nn.Dropout(dropout1),
					nn.Linear(256, 256),
					nn.ReLU(),
					# 鍦ㄧ浜屼釜鍏ㄨ繛鎺ュ眰涔嬪悗娣诲姞涓€涓猟ropout灞?					nn.Dropout(dropout2),
					nn.Linear(256, 10))
def init_weights(m):
	if type(m) == nn.Linear:
		nn.init.normal_(m.weight, std=0.01)
net.apply(init_weights);
```
#### 鍓嶅悜浼犳挱涓庡弽鍚戜紶鎾?- 鍓嶅悜浼犳挱鍦ㄧ缁忕綉缁滃畾涔夌殑璁＄畻鍥句腑鎸夐『搴忚绠楀拰瀛樺偍涓棿鍙橀噺锛屽畠鐨勯『搴忔槸浠庤緭鍏ュ眰鍒拌緭鍑哄眰銆?- 鍙嶅悜浼犳挱鎸夌浉鍙嶇殑椤哄簭锛堜粠杈撳嚭灞傚埌杈撳叆灞傦級璁＄畻鍜屽瓨鍌ㄧ缁忕綉缁滅殑涓棿鍙橀噺鍜屽弬鏁扮殑姊害銆?- 鍦ㄨ缁冩繁搴﹀涔犳ā鍨嬫椂锛屽墠鍚戜紶鎾拰鍙嶅悜浼犳挱鏄浉浜掍緷璧栫殑銆?#### 鏁板€肩ǔ瀹氬拰妯″瀷鍒濆鍖?- 姊害娑堝け鍜屾搴︾垎鐐告槸娣卞害缃戠粶涓父瑙佺殑闂銆傚湪鍙傛暟鍒濆鍖栨椂闇€瑕侀潪甯稿皬蹇冿紝浠ョ‘淇濇搴﹀拰鍙傛暟鍙互寰楀埌寰堝ソ鐨勬帶鍒躲€?- 闇€瑕佺敤鍚彂寮忕殑鍒濆鍖栨柟娉曟潵纭繚鍒濆姊害鏃笉澶ぇ涔熶笉澶皬銆?- ReLU婵€娲诲嚱鏁扮紦瑙ｄ簡姊害娑堝け闂锛岃繖鏍峰彲浠ュ姞閫熸敹鏁涖€?- 闅忔満鍒濆鍖栨槸淇濊瘉鍦ㄨ繘琛屼紭鍖栧墠鎵撶牬瀵圭О鎬х殑鍏抽敭銆?- Xavier鍒濆鍖栬〃鏄庯紝瀵逛簬姣忎竴灞傦紝杈撳嚭鐨勬柟宸笉鍙楄緭鍏ユ暟閲忕殑褰卞搷锛屼换浣曟搴︾殑鏂瑰樊涓嶅彈杈撳嚭鏁伴噺鐨勫奖鍝嶃€?#### 鐜鍜屽垎甯冨亸绉?- 鍦ㄨ澶氭儏鍐典笅锛岃缁冮泦鍜屾祴璇曢泦骞朵笉鏉ヨ嚜鍚屼竴涓垎甯冦€傝繖灏辨槸鎵€璋撶殑鍒嗗竷鍋忕Щ銆?- 鐪熷疄椋庨櫓鏄粠鐪熷疄鍒嗗竷涓娊鍙栫殑鎵€鏈夋暟鎹殑鎬讳綋鎹熷け鐨勯鏈熴€傜劧鑰岋紝杩欎釜鏁版嵁鎬讳綋閫氬父鏄棤娉曡幏寰楃殑銆?- 缁忛獙椋庨櫓鏄缁冩暟鎹殑骞冲潎鎹熷け锛岀敤浜庤繎浼肩湡瀹為闄┿€傚湪瀹炶返涓紝鎴戜滑杩涜缁忛獙椋庨櫓鏈€灏忓寲銆?- 鍦ㄧ浉搴旂殑鍋囪鏉′欢涓嬶紝鍙互鍦ㄦ祴璇曟椂妫€娴嬪苟绾犳鍗忓彉閲忓亸绉诲拰鏍囩鍋忕Щ銆傚湪娴嬭瘯鏃讹紝涓嶈€冭檻杩欑鍋忕Щ鍙兘浼氭垚涓洪棶棰樸€?- 鍦ㄦ煇浜涙儏鍐典笅锛岀幆澧冨彲鑳戒細璁颁綇鑷姩鎿嶄綔骞朵互浠や汉鎯婅鐨勬柟寮忓仛鍑哄搷搴斻€傚湪鏋勫缓妯″瀷鏃讹紝鎴戜滑蹇呴』鑰冭檻鍒拌繖绉嶅彲鑳芥€э紝骞剁户缁洃鎺у疄鏃剁郴缁燂紝骞跺鎴戜滑鐨勬ā鍨嬪拰鐜浠ユ剰鎯充笉鍒扮殑鏂瑰紡绾犵紶鍦ㄤ竴璧风殑鍙兘鎬ф寔寮€鏀炬€佸害銆?


鏈€缁堬紝浣滀负2-4绔犲涔犳晥鏋滆瘎浼帮紝鎴戦€夋嫨鎵嬪姩鎼缓MLP锛屽弬闃咃細[[My MLP]]