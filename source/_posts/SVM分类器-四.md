---
title: SVM分类器(四)
categories:
  - 科研学习 - 计算机视觉与机器学习
tags:
  - 计算机视觉
  - 机器学习
  - 数据聚类
cover: /images/cover/svm.jpg
katex: true
abbrlink: 53b406f4
date: 2020-03-09 10:58:59
---


# SVM 的起源

上个世纪70年代，苏联的两位科学家 Vladimir Vapnik，Alexey Chervonenkis 提出了 “**VC dimension**” 这一概念。VC dimension是衡量一个分类器复杂度的度量。它与分类器的抽象能力有关，能够用来**预测分类器的分类错误上界**。

# VC dimension

## VC dimension的定义 

一个拥有参数集 $\theta$ 的分类模型和一个拥有 $n$ 个数据的数据集 $D=(x_1, x_2, ..., x_n)$ 如果不论怎么给 $n$ 个数据赋予标签，总能找到一个参数集 $\theta$, 使得模型 $f(x, \theta)$ 能够正确分类全部 $n$ 个数据，那么我们 称该模型可以 **shatter** $n$ 个数据。

VC dimension 是模型能够 shatter 的最大数据数 $\max{(n)}$。 

线性分类器的VC dimension是3。

## VC dimension的意义

VC dimension可用来预测分类模型 $f(x, \theta)$ 的分类错误的上界

$$
\text{training error} + \sqrt{\frac{h(\log{\frac{2N}{h}}) + \log{\frac{\eta}{4}}}{N}}
$$

$h$ 是分类器的 VC dimension，$N$ 是训练集的样本数。

## VC dimension最小的线性分类器

![](/images/SVM分类器-四/2020-03-09-08-42-06.png)

有无数条直线可以把图中的两类数据正确分开。Margin 最大的直线对应 VC dimension 最小的线性分类器。

SVM 的目标就是求得 VC dimension 最小的线性分类器。

# SVM算法

##  Separable Case

**训练样本**：$S = \{x^{(i)}, y^{(i)}\}_{i=1}^n , x^{(i)} \in R^d, y^{(i)} \in \{-1, 1\}$
  
**Structural Model**: 线性函数 $h(x) = w^T x + b$, 拥有参数集 $(w, b)$

针对线性可分数据集 S ，找到一个**超平面** $(w^*, b^*)$ ，使 $S$ 中所有数据点均满足：

$$\begin{aligned}
\text{对于} y^{(i)} = +1, w^T x^{(i)} + b \ge 0 \\
\text{对于} y^{(i)} = -1, w^T x^{(i)} + b \le 0
\end{aligned}$$

上述两个不等式可以合并为：$y^{(i)}(w^T x^{(i)} + b) \ge 0, \forall i$

若 $(w, b)$ 满足上述不等式，则 $(aw, ab)$ 也满足。 为了保证解的唯一性，我们将公式归一化为：

$$
y^{(i)}(w^T x^{(i)} + b) \ge 1, \forall i
$$

### 超平面的几何特性

![](/images/SVM分类器-四/2020-03-09-08-59-43.png)

$$
h(x) = w^T x + b, d = \frac{1}{\|w\|} (w^T x + b)
$$

1. 对于超平面上的任意两点 $x_1$ 和 $x_2$, 均有 $w^T(x_1 - x_2) = 0$。因此，$w$ 是该超平面的法向量 (normal vector)。

2. 对于超平面上的任一点 $x_h$，均有 $w^T x_h = -b$。 
    
3. 任意一点 $x$ 到超平面的带符号距离(signed distance)：
   
   $$
   d = \frac{w}{\|w\|} (x-x_0) = \frac{1}{\|w\|} (w^T x - w^T x_0) = \frac{1}{\|w\|} (w^T x + b)
   $$

    $x_0$ 是法向量与超平面的交点。

4. 原点到超平面的垂直距离为 $\frac{|b|}{||w||}$。 
   
5. 使不等式 $y^{(i)}(w^T x^{(i)} + b) \ge 1$ 取等号的点是位于超平面 $w^T x + b = \pm 1$ 上的点。它们到分隔超平面 $w^T x + b$ 的距离均为 $\frac{1}{||w||}$。 

### Margin的定义及SVM损失函数 

- 用 $d^+$ 和 $d^-$ 分别表示分隔超平面到最近的正、负样本间距离，则超平面的 margin 定义为 $d^+ + d^-$。 

- 距离分隔超平面最近的正负样本分别位于平面 $H1, H2$ 上，因此 margin 等于：

$$
d^+ + d^- = \frac{1}{\|w\|} + \frac{1}{\|w\|} = \frac{2}{\|w\|}
$$

-  SVM 的目标就找出满足约束条件  $y^{(i)}(w^T x^{(i)} + b) \ge 1, \forall i$，并使 margin 最大化的超平面。

- ** 上述目标可表示为**:
  
  $$\begin{aligned}
  &\min_{w, b} {\frac{1}{2} w^T w} \\
  &s.t.\ y^{(i)} (w^T x^{(i)} + b) \ge 1, \forall i
  \end{aligned}$$

- 由于这是一个凸优化问题，获得全 局最优解是有保证的。

## Non-Separable Case 

实际情况中，大多数据集都不是线性可分的。 若仍使用上述 SVM，则许多点将被错分。

![](/images/SVM分类器-四/2020-03-09-09-15-06.png)


为了使 SVM 适用于不可分情况，我们为每一个点 $x^{(i)}$ 引入正松弛变量 (positive slack variable) $\xi$，并将限制条件转化为：

$$
y^{(i)}(w^T x^{(i)} + b) \ge 1 - \xi_i, \xi_i \ge 0 \ \forall i
$$

若 $\xi_i \ge 1$，则意味着 $x^{(i)}$ 被分错。因此 $\frac{1}{n}\sum^n_{i=1} \xi_i$ 可被当作分类错误的上界。 

- 不可分情况下的 SVM 损失函数可定义为：

  $$\begin{aligned}
  &\min_{w,b,\xi_i} {\frac{1}{2} w^T w} + \frac{C}{n} \sum^n_{i=1} \xi_i, \\
  &s.t.\ y^{(i)}(w^T x^{(i)} + b) \ge 1 - \xi_i, \xi_i \ge 0 \ \forall i
  \end{aligned}$$

  这也是一个凸优化问题，可以获得全局最优解。

## SVM推导：优化问题定义 

**训练样本**：$S = \{x^{(i)}, y^{(i)}\}_{i=1}^n , x^{(i)} \in R^d, y^{(i)} \in \{-1, 1\}$
  
**Structural Model**: 线性函数 $h(x) = w^T x + b$, 拥有参数集 $(w, b)$

**SVM建模问题可定义为**：利用数值计算算法找出 参数集 $(w, b)$，使得下列损失函数最小：

$$\begin{aligned}
&\min_{w,b,\xi_i} {\frac{1}{2} w^T w} + \frac{C}{n} \sum^n_{i=1} \xi_i, \\
&s.t.\ y^{(i)}(w^T x^{(i)} + b) \ge 1 - \xi_i, \xi_i \ge 0 \ \forall i
\end{aligned}$$

拉格朗日乘数法是解决约束最优化问题 (constrained optimization problem) 的最好数学工具。

## SVM求解：拉格朗日乘数法

导入非负拉格朗日乘数 $\boldsymbol \alpha = \{\alpha_i\}$ 和 $\boldsymbol \beta = \{\beta_i\}$ 来定义拉格朗日函数：

  $$
  L_p = \frac{1}{2} w^T w + \frac{C}{n} \sum^n_{i=1} \xi_i - \sum^n_{i=1} \alpha_i (y^{(i)} (w^T x^{(i)} + b) - 1 +\xi_i) - \sum^n_{i=1} \beta_i \xi_i
  $$

对 $w, b, \xi_i$ 分别求导，得到以下公式：

$$\begin{matrix}
\frac{\partial L_p}{\partial w} = w - \sum^n_{i=1} \alpha_i y^{(i)} x^{(i)} = 0 & \Rightarrow & w = \sum_{i=1}^n \alpha_i y^{(i)} x^{(i)} \tag{1}
\end{matrix}$$
$$\begin{matrix}
\frac{\partial L_p}{\partial b} = -\sum_{i=1}^n \alpha_i y^{(i)} = 0 & \Rightarrow & 0 = \sum_{i=1}^n \alpha_i y^{(i)} \tag{2}
\end{matrix}$$
$$\begin{matrix}
\frac{\partial L_p}{\partial \xi_i} = \frac{C}{n} - \alpha_i -\beta_i = 0 & \Rightarrow & \alpha_i = \frac{C}{n} - \beta_i \tag{3}
\end{matrix}$$

将(1), (2), (3)式代入到拉格朗日函数 $L_P$ 中，得到对偶损失函数(dual objective function)：

$$
L_D = \sum_{i=1}^n \alpha_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y^{(i)} y^{(j)} x^{(i)} \cdot x^{(j)}
$$

最小化原始损失函数 $L_P$ 等价于最大化下列对偶损失函数：

$$\begin{aligned}
&\max_\alpha {L_D (\boldsymbol{\alpha}))} = \sum_{i=1}^n \alpha_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y^{(i)} y^{(j)} x^{(i)} \cdot x^{(j)} \\
&s.t.\ \ \sum_{i=1}^n \alpha_i y^{(i)} = 0,\ 0 \le \alpha_i \le \frac{C}{n}\ \forall i
\end{aligned}$$

一旦求得最优 $\alpha$，SVM模型可定义为：

$$
f_\alpha(x) = sign{(\sum_{i=1}^n \alpha_i y^{(i)} (x^{(i)} \cdot x) + b)}
$$


## 支持向量(Support Vector) 

从(1)式可以看出，SVM 的最优参数 $w$可以表示为训练样本的线性混合：

$$
\sum_{i=1}^n \alpha_i y^{(i)} x^{(i)}
$$

称 $\alpha_i \ne 0$ 的数据点 $x^{(i)}$ 为支持向量(support vector)，因为 $w$ 主要是由这些点决定的。

对偶损失函数 $L_D$ 拥有以下 KKT 条件：

$$
\alpha_i (y^{(i)} (w^T x^{(i)} + b) - 1 + \xi_i) = 0 \tag{4}
$$
$$
\beta_i \xi_i = 0 \tag{5}
$$

从(4)式知，任意位于边缘超平面 $(\alpha_i > 0, \xi_i = 0)$ 的支持向量点 $x^{(i)}$ 都可以用来计算偏移量 $b$，实际应用中，b一般由用户设定。

## Kernel Trick

原始 SVM 是一个线性分类器。而核(kernel trick)的引入，可以使 SVM 变成一个**非 线性分类器**，显著提高分类精度。

使用一个核函数，将原有特征空间映射到一个超高维的特征空间，并使用线性分类器在这一超高维空间上分类。超高维空间上的线性边界，对应着原特征空间上的非线性边界，这使原先线性不可分的数据集获得了更好的划分。

在原始空间中线性不可分的数据点投影到高维空间后变成为线性可分。图中使用了核函数 $\Phi(x) = [1, \sqrt 2 x, x^2]$

![](/images/SVM分类器-四/2020-03-09-10-07-06.png)

在前文讨论中，拉格朗日对偶函数和 SVM 分类器分别为：

$$\begin{aligned}
L_D = \sum_{i=1}^n \alpha_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y^{(i)} y^{(j)} x^{(i)} \cdot x^{(j)} \\
f_\alpha(x) = sign{(\sum_{i=1}^n \alpha_i y^{(i)} (x^{(i)} \cdot x) + b)}
\end{aligned}$$

上述式子中，特征向量都以内积(dot product)的形式出现，这意味着我们不需要考虑单个向量，而只需知道他们的内积即可。

假设我们需要将原始数据扩大到欧几里得高维（也可能是无限维）空间 $\mathcal N$ 中，使用的映射函数为 $\Phi: R^d \rightarrow \mathcal N$，在高维扩大特征空间 $\mathcal N$ 中，由于 SVM 只依赖内积 $\Phi(x^{(i)} \cdot \Phi(x^{(j)})$，若存在一个核函数 $K$ 使 $K(x^{(i)}, x^{(j)}) = \Phi(x^{(i)} \cdot \Phi(x^{(j)})$，在计算中，我们就不再需要确切知道 $\Phi$ 的函数表达式，只需要使用 $K$ 便可。故而非线性 SVM 为：

$$\begin{aligned}
f_\alpha(x) &= sign(\sum_{i=1}^n \alpha_i y^{(i)} \Phi(x^{(i)}) \cdot \Phi(x) + b) \\
            &= sign(\sum_{i=1}^n \alpha_i y^{(i)} K(x^{(i)}, x) + b)
\end{aligned}$$

用一个例子来说明核函数的概念。假设输入的特征空间是二维的 $x = [x_1, x_2]$，使用二元多项式核函数(degree-2 polynomial kernel):

$$\begin{aligned}
K(\boldsymbol x, \boldsymbol x') 
    &= (1 + \boldsymbol x \cdot \boldsymbol x')^2 \\
    &= (1 + x_1 x_1' + x_2 x_2')^2 \\
    &= 1 + 2 x_1 x_1' + 2 x_2 x_2')^2 + (x_1 x_1')^2 + (x_2 x_2')^2 + 2 x_1 x_1' x_2 x_2'
\end{aligned}$$

若我们选择映射函数为：

$$
\Phi(x) = [1, \sqrt 2 x_1, \sqrt 2 x_2, x_1^2, x_2^2, \sqrt 2 x_1 x_2]^T
$$

函数 $\Phi$ 将二位特征空间映射到 **六** 维，同时也满足：

$$
K(x, x') = \Phi(x) \cdot \Phi(x')
$$

由于 SVM 只依赖数据的内积，在超高空间里，我们仅仅需要知道核函数 $K(x, x') = \Phi(x) \cdot \Phi(x')$

一旦确定了核函数，我们不再需要精确地将原 始特征空间中的数据点一一映射到高维空间中 去，实际上，我们甚至不需要知道映射函数 $\Phi(x)$ 是什么。

因此，使用核函数可以使我们在不改动算法框架，并且最小计算成本的前提下，将线性 SVM 泛化到非线性 SVM。

**常用核函数**

- Degree-d polynomial: $K(x, x') = (1 + x \cdot x')^d$
  
- Radial basis: $K(x, x') = \exp{(- \frac{\|x-x'\|^2}{c})}$

- Neural network: $K(x, x') = tanh(\kappa_1 x \cdot x' + \kappa_2)$

**线性SVM VS. 核SVM**

![](/images/SVM分类器-四/2020-03-09-10-30-41.png)

## SVM求解

### Quadratic Programming (QP) 

目标是求出满足下列约束条件，并使对偶损失函数 $L_D$ 最大化的 $\alpha$：

$$\begin{aligned}
&\max_\alpha {L_D (\boldsymbol{\alpha})} = \sum_{i=1}^n \alpha_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y^{(i)} y^{(j)} x^{(i)} \cdot x^{(j)} \\
&s.t.\ \ \sum_{i=1}^n \alpha_i y^{(i)} = 0,\ 0 \le \alpha_i \le \frac{C}{n}\ \forall i
\end{aligned}$$

可应用传统 QP 算法求出上述问题最优解。传统 QP 算法需对 $n \times n$ 矩阵做迭代计算，面临严重的扩展性问题。

### Sequential Minimal Optimization(SMO)

把传统的 SVM QP 问题分解成最小 QP 子问题， 即一次计算两个拉格朗日乘数 $\alpha_1, \alpha_2$

**SMO算法梗概**:

1. 利用先验规则选出两个需优化拉格朗日乘数 $\alpha_1, \alpha_2$
   
2. 固定其它拉格朗日乘数，求出满足下列约束条件，并使对偶损失函数 $L_D$ 最大化的 $\alpha_1, \alpha_2$：
   
   $$
   \sum_{i=1}^n \alpha_i y^{(i)} = 0,\ 0 \le \alpha_i \le \frac{C}{n}\ \forall i
   $$
   
3. 重复步骤1、2直到所有拉格朗日乘数都得到优化

**SMO算法优点**:

- 不需计算 $n \times n$ 矩阵，彻底解决了扩展性问题。

- 尽管传统 SVM QP 问题被分解成大量 QP 子问题，但每个子问题求解速度很快，不会加重计算成本。

### 随机梯度下降法

SVM的损失函数还可以表示为：

$$
L = \frac{\lambda}{2} w^T w + \sum_{i=1}^n \max{[0, (1-y^{(i)} (w^T x^{(i)} + b))]}
$$

针对 $w$ 和 $b$ 求导，我们得到：

$$\begin{aligned}
&\frac{\partial L}{\partial w}
    = \left\{
      \begin{aligned}
      &\lambda w - y^{(i)} x^{(i)}, & & if\ y^{(i)} (w^T x^{(i)} + b) \lt 1 \\
      &\lambda w, & & if\ y^{(i)} (w^T x^{(i)} + b) \ge 1
      \end{aligned}
    \right.
    \\
&\frac{\partial L}{\partial b}
    = \left\{
      \begin{aligned}
      &- y^{(i)}, & & if\ y^{(i)} (w^T x^{(i)} + b) \lt 1 \\
      &0, & & if\ y^{(i)} (w^T x^{(i)} + b) \ge 1
      \end{aligned}
    \right.
\end{aligned}$$

重复以下步骤，直到收敛为止:

1. 随机选出一个训练样本，利用上述公式计算 $\frac{\partial L}{\partial w}, \frac{\partial L}{\partial b}$。
   
2. 用以下公式更新 $w, b$:

$$\begin{aligned}
&w_t = w_{t-1} - \eta (\lambda w_{t-1} - y^{(i)} x^{(i)}) = (1 - \eta\lambda)w_{t-1} - \eta y^{(i)} x&{(i)} \\
&b_t = b_{t-1} + \eta y^{(i)}
\end{aligned}$$

**随机梯度下降法的优点**:

每次迭代传统梯度下降法需针对所有训练样本求导，而随机梯度下降法**只针对一个训练样本求导**，因而大大提高运算速度。

**SVM损失函数曲线**:

![](/images/SVM分类器-四/2020-03-09-10-55-44.png)

$$
L = \frac{\lambda}{2} w^T w + \sum_{i=1}^n \max{[0, (1-y^{(i)} (w^T x^{(i)} + b))]}
$$

# 更多

参考[科研学习-计算机视觉与机器学习](/categories/科研学习-计算机视觉与机器学习/)