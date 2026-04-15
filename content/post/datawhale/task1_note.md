---
title: "具身智能task1笔记"
date: 2026-04-15T12:00:00+08:00
draft: false
slug: "Embodied-notes"
tags:
  - "Embodied"
categories:
  - "Notes"
---

## 1.机器人空间描述与坐标变换

本部分的核心内容是旋转矩阵&齐次变换矩阵

### 旋转矩阵

首先我们考虑一个平面上的旋转，假设初角度为$\phi$旋转角度为$\theta$

若采用极坐标表示：
$$
\begin{align}
x = rcos(\phi+\theta)\\
y = rsin(\phi+\theta)

\end{align}
$$
对它进行三角函数展开：
$$
\begin{align}
x = rcos(\theta)cos(\phi)-rsin(\theta)sin(\phi)\\
y = rsin(\phi)cos(\theta) + rcos(\phi)sin(\theta)
\end{align}
$$
结合初始状态：
$$
\begin{align}
x_0 = rcos(\phi)\\
y_0 = rsin(\phi)

\end{align}
$$
可以得到：
$$
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
\begin{bmatrix}
\cos \theta & -\sin \theta \\
\sin \theta & \cos \theta
\end{bmatrix}
\begin{bmatrix}
x_0 \\
y_0
\end{bmatrix}
$$
若我们要在三维空间中表示该旋转矩阵，则需要扩展为：
$$
R_z(\theta) = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

### 齐次变换矩阵

我们希望把平移和旋转归纳到同一个矩阵中，这样更便于运算，这个步骤中我们需要对矩阵升维来实现三维中的非线性变换：
$$
T = \begin{bmatrix} R & P \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} r_{11} & r_{12} & r_{13} & p_x \\ r_{21} & r_{22} & r_{23} & p_y \\ r_{31} & r_{32} & r_{33} & p_z \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$

## 2.机器人运动学与DH参数

### Denavit-Hartenberg (DH) 参数法

描述相邻两个连杆 $i-1$ 和 $i$ 之间的关系，仅需 4 个参数：

| **参数**     | **符号**   | **物理含义**                                      | **测量基准**      |
| ------------ | ---------- | ------------------------------------------------- | ----------------- |
| **连杆长度** | $a_i$      | 沿 $x_i$ 轴，从 $z_{i-1}$ 到 $z_i$ 的距离         | 连杆本身结构      |
| **连杆扭角** | $\alpha_i$ | 绕 $x_i$ 轴，从 $z_{i-1}$ 旋转到 $z_i$ 的角度     | 连杆本身结构      |
| **关节偏距** | $d_i$      | 沿 $z_{i-1}$ 轴，从 $x_{i-1}$ 到 $x_i$ 的距离     | 关节变量 (移动副) |
| **关节角度** | $\theta_i$ | 绕 $z_{i-1}$ 轴，从 $x_{i-1}$ 旋转到 $x_i$ 的角度 | 关节变量 (旋转副) |

注：其中关节偏距&连杆长度都是投影

相邻连杆的变换矩阵$T_{i-1}^i$ 公式为：
$$
T_{i-1}^{i} = \text{Rot}_{z}(\theta_i) \cdot \text{Trans}_{z}(d_i) \cdot \text{Trans}_{x}(a_i) \cdot \text{Rot}_{x}(\alpha_i)
$$

## 3.机器人动力学

分为正动力学：给定力矩算加速度、运动轨迹&逆动力学：给运动轨迹计算例句

常用的方法是**拉格朗日法 (Euler-Lagrange)**，它基于能量守恒。

### Euler-Lagrange

拉格朗日函数 $L$ 定义为系统总动能 $K$ 与总势能 $P$ 之差：
$$
L(q, \dot{q}) = K(q, \dot{q}) - P(q)
$$
动力学方程（欧拉-拉格朗日方程）：
$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}_i} \right) - \frac{\partial L}{\partial q_i} = \tau_i
$$
最终通常整理为标准的机器人在动力学形式：
$$
M(q)\ddot{q} + C(q, \dot{q})\dot{q} + G(q) = \tau
$$

- $M(q)$: 惯性矩阵 (Mass Matrix)

- $C(q, \dot{q})$: 科里奥利力和离心力项

- $G(q)$: 重力项

$\tau_i$就是驱动第$i$个关节运动的输入（力或力矩），是广义坐标对应的“共轭力”

## 4.速度运动学与Jacobian矩阵

我们已经知道位置函数：
$$
x = f(q)
$$
对它$t$的导：
$$
\dot x = \frac{d}{dt}f(q) = \frac{\partial f}{\partial q} \dot q
$$
也就可以视为：
$$
\dot x = J(q)\dot q
$$
其中左侧的$\dot x$是末端运动速度，其形式通常是$[v,\omega]^T$，而右侧是每个关节自由度构成的向量（一般情况，如果讨论的是多自由度系统，则为矩阵）

那么我们就可以构成从各关节自由度向末端移动的映射，该映射由$J(q)$完成
$$
J(q) = \frac{\partial f}{\partial q} = \begin{bmatrix} \frac{\partial x}{\partial q_1} & \cdots & \frac{\partial x}{\partial q_n} \\ \vdots & \ddots & \vdots \\ \end{bmatrix}
$$
Jabcobian矩阵的每一列代表第i个关节移动对末端位置的影响。

## 5.实战部分

## 6.手眼协调

### *Def*

主要用于统一视觉系统与机器人的坐标系，具体来说，就是确定摄像头与机器人的相对姿态关系

> 当我们希望使用视觉引导机器人去抓取物体时，需要知道三个相对位置关系，即
> 
> 1. 末端执行器与机器人底座之间相对位置关系
> 2. 摄像头与末端执行器之间相对位置关系
> 3. 物体与摄像头之间的相对位置和方向
> 
> 手眼标定主要解决其中第二个问题，即确定“手”与安装在其上“眼”之间的空间变换关系，即求解相机坐标系和机器人坐标系之间的变换矩阵。  这里的机器人末端执行器称为手，摄像头称为眼。

手眼标定分为两种形式：

1.摄像头安装在机械手末端，称之为眼在手上（Eye in hand） 

2.摄像头安装在机械臂外的机器人底座上，则称之为眼在手外（Eye to hand）

### 手眼标定的数学模型

不管是眼在手上还是眼在手外，它们求解的数学方程是一样的，都为 $AH = HB$ ，首先定义如下坐标系：

$F_b$: **基座坐标系**(Base Frame),固定在机械臂的底座上，是机械臂运动的全局参考坐标系。

$F_e$: **末端执行器坐标系**(End-Effector Frame)固定在机械臂末端执行器（例如夹爪或工具）上。

$F_c$: **相机坐标系**（Camera Frame）,固定在相机光心的位置，是视觉感知的参考系。

$F_t$: **标定目标坐标系**（Calibration Target Frame）：固定在标定目标（如棋盘格、圆点板）上。

我们仍然可以用齐次变换矩阵来进行上述坐标系之间的变换

#### Eye In Hand

$$
T^b_t = T^b_{e1}\ T^{e1}_{c1}\ T^{c1}_{t} = T^b_{e2}\ T^{e2}_{c2}\ T^{c2}_{t}\\
\text{对上述等式进行变换}\\
(T^b_{e2})^{-1}T^b_{e1}T^{e1}_{c1}T^{c1}_{t} = T^{e2}_{c2}T^{c2}_{t}\\
(T^b_{e2})^{-1}T^b_{e1}T^{e1}_{c1} = T^{e2}_{c2}T^{c2}_{t}(T^{c1}_{t})^{-1}\\
T^{e2}_{e1}T^{e1}_{c1} = T^{e2}_{c2}T^{c2}_{c1}\\
T^{e2}_{e1}T^{e}_{c} = T^{e}_{c}T^{c2}_{c1}\\
\text{又：}AH = HB
$$

则：其中`T^e_c`就是最终需要求解的`H`

#### Eye To Hand

```math
T^e_t = T^{e1}_bT^{b}_{c}T^{c}_{t1} = T^{e2}_bT^{b}_{c}T^{c}_{t2}
```
对上述等式进行变换：
```math
T^{e1}_bT^{b}_{c}T^{c}_{t1} = T^{e2}_bT^{b}_{c}T^{c}_{t2} \\

(T^{e2}_b)^{-1}T^{e1}_bT^{b}_{c}T^{c}_{t1} = T^{b}_{c}T^{c}_{t2} \\
(T^{e2}_b)^{-1}T^{e1}_bT^{b}_{c} = T^{b}_{c}T^{c}_{t2}(T^{c}_{t1})^{-1} \\
(T^b_{e2}T^{e1}_b)T^{b}_{c} = T^{b}_{c}(T^{c}_{t2}T^{t1}_c) \\
AH = HB
```

#### 求解AH = HB

##### Park方法求解旋转

```math
\theta_X=(\mathbf M^T\mathbf M)^{-\frac{1}{2}}\mathbf M^T\\
\text{其中：}\mathbf M = \sum_{i=1}^k \beta_i\alpha_i^T
```

##### Park方法求旋转

```math
b_X = (\mathbf C^T \mathbf C)^{-1}\mathbf C^T\mathbf D
```

## 7.PID控制算法

PID控制器通过以下三个基本组件来计算控制输入：
- 比例（P）：直接对误差进行比例放大，可以迅速减少误差，但可能导致系统不稳定。
- 积分（I）：对误差进行积分，可以消除系统的静态误差，但可能导致系统响应变慢。
- 微分（D）：对误差的导数进行预测，可以预测误差的变化趋势，从而提前进行调整，提高系统的响应速度和稳定性。
- $e(t)$ 是当前误差，即期望输出与实际输出之间的差值。
- $K_p$ 是比例系数。
- $K_i$ 是积分系数。
- $K_d$ 是微分系数。
- 实现步骤

计算误差：计算当前输出与期望输出之间的误差。

比例项：根据比例系数和误差计算比例项。

积分项：对误差进行积分，并根据积分系数计算积分项。

微分项：计算误差的导数，并根据微分系数计算微分项。

计算控制输入：将比例项、积分项和微分项相加，得到控制输入。

应用控制输入：将控制输入应用于被控系统。

```math
u(t) = K_p\cdot e(t)+K_i\int_o^te(\tau)d\tau+K_d\cdot\frac{de(t)}{dt}
```

