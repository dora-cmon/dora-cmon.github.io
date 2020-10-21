---
title: Boosting分类器(五)
categories:
  - 科研学习 - 计算机视觉与机器学习
tags:
  - 计算机视觉
  - 机器学习
  - 数据聚类
cover: /images/cover/线性分类器.jpg
katex: true
abbrlink: d3271583
date: 2020-03-14 13:13:27
---


# 概览: Boosting Methods

Boosting 整合许多弱分类器的输出来生成一个强分类器

**目标**：提升给定学习算法的准确率

**假设**：每一个弱分类器的误差率都略微比随机猜测好，因此有 $err_m \lt 0.5$

![](/images/Boosting分类器-五/2020-03-09-16-28-52.png)

# AdaBoost 方法

## 术语

**训练数据**：$T = \{x_i, y_i\}_{i=1}^n$

两类**输出变量**：$y_i \in \{-1, +1\}$

**分类器**：$f(x) \in \{-1, +1\}$

**训练误差**：$\overline{err} = \frac{1}{N} \sum_{i=1}^N I(y_i \ne f(x))$

**弱分类器**：$f_m(x) \in \{-1, +1\},\ m = 1,2,...,M$

**最终分类器**：加权多数投票

$$
F(x) = sign(\sum_{m=1}^M c_m f_m(x))
$$

# 离散 AdaBoost

## 算法步骤

1. 开始时 $w_i = \frac{1}{N},\ i = 1,2,...,N$
   
2. 重复以下操作 $m = 1,2,...,M$

    a) 使用训练数据的权重 $w_i$ 确定分类器 $f_m(x) \in \{-1, 1\}$ </br></br>

    b) 计算 $err_m = \frac{\sum_{i=1}^N w_i I(y_i \ne f_m(x))}{\sum_{i=1}^N w_i},\ c_m = \log{\frac{1-err_m}{err_m}}$ </br></br>
    
    c) 令 $w_i \leftarrow w_i e^{c_m I(y_i \ne f_m(x))},\ i=1,2,...,N$ 并且重新正规化 $\sum_i w_i = 1$ </br></br>

3. 分类器输出 $sign[\sum_{m=1}^M c_m f_m(x)]$

**注意**：离散表示基础分类器返回一个离散的标签 $f(x) \in \{-1, 1\}$

## 示例

给定下表所示的训练数据。假设弱分类器由 $x \lt v$ 或 $x \gt v$ 产生，其阈值 $v$ 使该分类器在训练数据集上分类误差最低。使用 AdaBoost 算法学习一个强分类器。

|序号| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|-|-|-|-|-|-|-|-|-|-|-|
| x | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| y | 1 | 1 | 1 | 1 |-1 |-1 |-1 | 1 | 1 |-1 |

**解**：初始化数据权值分布：

$$\begin{aligned}
D_1 = (w_11, w_12,...,w_110)\\
w_{1i} = 0.1,\ i=1,2,...,10
\end{aligned}$$

- **对 $m = 1$**
    a) 在权值分布为 $D_1$ 训练数据上，阈值 $v$ 取 $2.5$ 时分类误差率最低，故基本分类器为：

    $$
    G_1(x) = \left\{
      \begin{aligned}
      1 & & if\ x \lt 2.5 \\
      -1 & & if\ x \gt 2.5 \\
      \end{aligned}
    \right.
    $$

    b) $G_1(x)$ 在训练数据集上的误差率为 $0.3$：$e_1 = \frac{\sum_{i=1}^N w_i I(y_i \ne f_m(x_i))}{\sum_{i=1}^N} = 0.3$

    c) 计算 $G_1(x)$ 的系数：$c_1 = \frac{1}{2} \log{\frac{1-e_1}{e_1}} = 0.4236$

    d) 更新训练数据的权值分布：

    $$\begin{aligned}
    &D_2 = (w_21, w_22,...,w_210)\\
    &w_{2i} = \frac{w_{1i}}{Z_1} e^{-c_1 y_1 G_1(x_1)}
    \end{aligned}$$

    $$\begin{aligned}
    &D_2 = (.0715, .0715, .0715, .0715, .0715, .0715, .1666, .1666, .1666, .0715) \\
    &f2(x) = 0.4236 G_1(x)
    \end{aligned}$$

    分类器 $sign[f_1(x)]$ 在训练数据集上有 3 个误分类点。

- **对 $m = 2$**
    a) 在权值分布为 $D_2$ 训练数据上，阈值 $v$ 取 $8.5$ 时分类误差率最低，故基本分类器为：

    $$
    G_2(x) = \left\{
      \begin{aligned}
      1 & & if\ x \lt 8.5 \\
      -1 & & if\ x \gt 8.5 \\
      \end{aligned}
    \right.
    $$

    b) $G_2(x)$ 在训练数据集上的误差率为 $e_2 = 0.2143$

    c) 计算 $G_2(x)$ 的系数：$c_2 = 0.6496$

    d) 更新训练数据的权值分布：

    $$\begin{aligned}
    &D_2 = (.125, .125, .125, .102, .102, .102, .065, .065, .065, .125) \\
    &f2(x) = 0.4236 G_1(x) + 0.6496 G_2(x)
    \end{aligned}$$

    分类器 $sign[f_2(x)]$ 在训练数据集上有 3 个误分类点。

- **对 $m = 3$**
    a) 在权值分布为 $D_3$ 训练数据上，阈值 $v$ 取 $5.5$ 时分类误差率最低，故基本分类器为：

    $$
    G_2(x) = \left\{
      \begin{aligned}
      1 & & if\ x \lt 5.5 \\
      -1 & & if\ x \gt 5.5 \\
      \end{aligned}
    \right.
    $$

    b) $G_3(x)$ 在训练数据集上的误差率为 $e_3 = 0.1820$

    c) 计算 $G_3(x)$ 的系数：$c_3 = 0.7514$

    d) 更新训练数据的权值分布：

    $$\begin{aligned}
    &D_3 = (.125, .125, .125, .102. .102, .102, .102, .065, .065, .065, .125) \\
    &f3(x) = 0.4236 G_1(x) + 0.6496 G_2(x) + 0.7514 G_3(x)
    \end{aligned}$$

    分类器 $sign[f_3(x)]$ 在训练数据集上有 0 个误分类点。

于是，最终的分类器为：

$$
G(x) = sign[f_3(x)] = sign[0.4236 G_1(x) + 0.6496 G_2(x) + 0.7514 G_3(x)]
$$

## 加法模型

**架构模型**:

- 采用基函数的展开形式
  
  $$
  f(x) = \sum_{m=1}^M \beta_m f_m(x) = \sum_{m=1}^M \beta_m b(x; \gamma_m)
  $$

  其中，$\beta_m$ 是第 $m$ 个偏置函数的权重。

- 基函数通常是由参数 $\gamma_m$ 表示的多元参数 $x$ 的简单函数 ...
  
  $$
  f_m(x) = b(x; \gamma_m)
  $$

**损失函数**:

可以是任何损失函数，指数损失和对数似然函数是最常用的函数。

**优化方法**: Forward stage-wise

最小化以下损失函数是计算密集型的：

$$
\min _{\left\{\beta_{m}, \gamma_{m}\right\}_{l} } \sum_{i=1}^{N} L\left(y_{i}, \sum_{m=1}^{M} \beta_{m} b\left(x_{i} ; \gamma_{m}\right)\right)
$$

可以通过解决子问题或拟合单个偏置函数的方式来简化计算：

- 在第 $m$ 次迭代，固定参数 $f_{m-1}(x) = \sum_{m=1}^{m-1} \beta_m b(x_i \gamma_m)$，然后优化 $b(x; \gamma_m)$ 及其对应的系数 $\beta_m$

- 将拟合的 $\beta_m b(x; \gamma_m)$ 应用于 $f_{m-1}(x)$

- 在早期迭代中无需调整 $\{\beta_m, \gamma_m\}_l^{m-1}$ 优化
  
    $$\begin{aligned}
    &for\ m = 1 \rightarrow\ M\ do \\
    &\ \ \ \ (1) Compute\ \{\beta_m, \gamma_m\} = \arg \min_{\beta, \gamma} \sum_{i=1}^N L(y_i, f_{m-1}(x_i) + \beta b(x_i; \gamma)) \\
    &\ \ \ \ (2)Set f_m(x) = f_{m-1}(x) + \beta_m b(x; \gamma_m)\\
    &end\ for
    \end{aligned}$$

对于平方差误差损失 $L(y, f(x)) = (y - f(x))^2$，则有：

$$
L\big(y_i, f_{m-1}(x_i) + \beta b(x_i; \gamma)\big) = \big(y_i - f_{m-1}(x_i) - \beta b(x_i; \gamma)\big)^2
$$

- $r_{im} = y_i - f_{m-1}(x_i)$ 是当前模型在第 $i$ 个观测值上的的残差

- 优化就是拟合当前的残差

平方差误差损失对于分类器来说不够好，AdaBoost 使用指数损失。

## AdaBoost的本质

**架构模型: 加法模型**

  $$
  F_M(x) =  \sum_{m=1}^M \beta_m b(x; \gamma_m),\ where\ \Theta = \{\beta_m. \gamma_m\}_{m=1}^M
  $$

**误差函数: 指数损失**

$$
L(y, F_M(x; \Theta)) = e^{-y F_m(x; \Theta)}
$$

**优化器: Forward stage-wise 算法**

$$\begin{aligned}
&F_m(x; \Theta) = F_{m-1}(x; \Theta_{m-1}) + \beta_m b(x;\gamma_m),
where\ F_{m-1}(x_i;\Theta_{m-1}) = \sum_{i=1}^{m-1} \beta_i b(x; \gamma_i) \\
&(\beta_m, f_m) = \arg{\min_{\beta, f} \sum_{i=1}^N \exp{[-y_i (F_{m-1}(x_i;\Theta_{m-1}) + \beta b(x_i; \gamma))}]}
\end{aligned}$$

**精准算法需对下述函数求解，复杂度极高**
$$
\min_{ \{\beta_m, \gamma_m\}_l^M} \sum_{i=1}^N \exp{\Big\{-y_i \sum_{m=1}^M \beta_m b(x_i; \gamma_m)\Big\} }
$$

## 离散 AdaBoost

**基函数**：弱分类器 $f_m(x) \in \{-1, +1\}$

**损失函数**：指数损失函数 $L(y, f(x)) = e^{-y F(x)}$

每一步必须解决：

$$
(\beta_m, f_m) = \arg{\min_{\beta, f} \sum_{i=1}^N \exp{[-y_i (F_{m-1}(x_i) + \beta_m f_m(x_i))}]}
$$

可以表示为：

$$\begin{aligned}
&(\beta_m, f_m) = \arg{\min_{\beta, f} \sum_{i=1}^N w_i^{(m)} \exp{[-y_i \beta_m f_m(x_i))}]} \\
&where\ w_i^{(m)} = \exp{(-y_i F_{m-1}(x_i))}
\end{aligned}$$

因为 $w_i^{(m)}$ 依赖于 $F_{m-1}(x_i)$，因此权重值在每次迭代中随 $m$ 改变。

**求解**：

$$
(\beta_m, f_m) = \arg{\min_{\beta, f} \sum_{i=1}^N w_i^{(m)} \exp{[-y_i \beta_m f_m(x_i))}]} \tag{1}
$$

1. 对于所有 $\beta \gt 0$ 的值，$f_m = \arg \min\limits_f \sum\limits_{i=1}^N w_i^{(m)} I(y_i \ne f(x_i))$

2. 将 $f_m$ 代入等式(1)，并求解 $\beta_m$
   
   $$
   err_m = \frac{\sum_{i=1}^N w_i I(y_i \ne f_m(x))}{\sum_{i=1}^N w_i},\ 
   \beta_m = \frac{1}{2} \log{\frac{1-err_m}{err_m}}
   $$

3. 更新加法模型
   
   $$
   F(x) = F_{m-1}(x) + \beta_m f_m(x),\ w_i^{(m+1)} \leftarrow w_i^{(m)} \exp{\big(-y_i \beta_m f_m(x) \big)} \tag{2}
   $$

   由于有 $-y_if_m(x)i) = 2I(y_i \ne f_m(x_i)) - 1$

   因此，(2)式变为 $\ w_i^{(m+1)} \leftarrow w_i^{(m)} \exp{\big(2\beta_m I(y_i \ne f_m(x_i)) \big)} \cdot \exp{(-\beta_m)}$

# 连续 AdaBoost

**引理 1**：$E_{p(y)} (e^{-y F(x)}|x)$ 在 $F(x) = \frac{1}{2} \log{\frac{P(y=1|x)}{y=-1|x}}$ 时取得最小值

证明：

$$\begin{aligned}
&E(e^{-y F(x)}|x) = p(y=1|x) e^{-F(x)} + p(y=-1|x) e^{F(x)}, \\\\
&\frac{\partial E(e^{-y F(x)}|x)}{\partial F(x)} = p(y=1|x) e^{-F(x)} + p(y=-1|x) e^{F(x)} = 0
\end{aligned}$$
$$
e^{2F(x)} = \frac{p(y=1|x)}{p(y=-1|x)},\ 
F(x) = \frac{1}{2} \log{\frac{P(y=1|x)}{y=-1|x}} \tag{3}
$$

从(3)式中可得：

$$
P(y=1|x) = \frac{e^{F(x)}}{e^{-F(x)} + e^{F(x)}},
P(y=-1|x) = \frac{e^{-F(x)}}{e^{-F(x)} + e^{F(x)}},
$$

## 算法步骤

1. 开始时 $w_i = \frac{1}{N},\ i = 1,2,...,N$
   
2. 重复以下操作 $m = 1,2,...,M$

    a) 使用训练数据的权重 $w_i$ 拟合分类器来获得一个类别的可能性估计 $p_m(x) = p'_m(y=1|x) \in [0,1]$</br></br>

    b) 令 $f_m(x) \leftarrow \frac{1}{2} \log{\frac{p_m(x)}{1-p_m(x)}} \in R$</br></br>
    
    c) 令 $w_i \leftarrow w_i e^{-y_i f_m(x)},\ i=1,2,...,N$ 并且重新正规化 $\sum_i w_i = 1$ </br></br>

3. 分类器输出 $sign[\sum_{m=1}^M f_m(x)]$

# LogitBoost(二分类)

**架构模型: 加法模型**

  $$
  F_M(x) =  \sum_{m=1}^M f_m b(x; \gamma_m)
  $$

**误差函数: 对数似然**

$$\begin{aligned}
&l(y, p(x)) = y \log{p(x)} + (1-y) \log{(1 - p(x))} = - \log{1 + e^{-2 y^* F(x)}} \\
&where\ P(x) = \frac{e^{F(x)}}{e^{-F(x) + e^{F(x)}}}
\end{aligned}$$

**优化器: Forward stage-wise 算法**

## 推导

更新 $F(x) + f(x)$ 并且使用对数似然：

$$\begin{aligned}
E\big(L(F+f)\big) &= E\big(y \log{p(x)} + (1-y) \log{(1- p(x))}\big) \\
                  &= E\big(2y \cdot (F(x) + f(x)) - \log{(1+e^{2(F(x) + f(x))})} \big) \\
where\ P(x) &= \frac{e^{F(x)+f(x)}}{e^{-F(x)-f(x)} + e^{F(x)+f(x)}}
\end{aligned}$$

计算 $f(x)$ 的一阶导和二阶导：

$$\begin{aligned}
&s(x) = \frac{\partial El(F(x) + f(x))}{\partial f(x)}
     = 2 E(y^* - p(x)|x), \\
&H(x) = \frac{\partial^2 E l(F(x) + f(x))}{\partial f(x)^2}
      = -4 E(p(x)(1-p(x))|x)
\end{aligned}$$

进行 Newton update:

$$\begin{aligned}
&F(x) \leftarrow F(x)-H(x)^{-1} s(x)
  = F(x) + \frac{1}{2} \frac{E(y^* -p(x)|x)}{E(p(x)(1-p(x))|x)}
  = F(x) + \frac{1}{2} E_w\Big(\frac{y^* -p(x)}{p(x)(1-p(x))}|x \Big) \\
&Where\ w(x) = p(x)(1-p(x)),and\ 
E_w[g(x,y)|x] = \frac{E[w(x,y) g(x,y)|x]}{E[w(x,y)|x]}
\end{aligned}$$

## 算法步骤

1. 开始时 $w_i = \frac{1}{N},\ i = 1,2,...,N$， $F(x) = 0$ 且可能性估计为 $p(x_i) = \frac{1}{2}$
   
2. 重复以下操作 $m = 1,2,...,M$

    a) 计算结果和权重

    $$\begin{aligned}
    z_i = \frac{y_i^* - p(x_i)}{p(x_i)(1- p(x_i))}, \\
    w_i = p(x_i)(1- p(x_i)).
    \end{aligned}$$
    
    b) 使用 $w_i$ 对有关 $x_i$ 的 $z_i$ 的加权最小二乘回归来拟合 $f_m(x)$

    c) 更新 $ F(x) \leftarrow $

    b) 令 $f_m(x) \leftarrow \frac{1}{2} \log{\frac{p_m(x)}{1-p_m(x)}} \in R$</br></br>
    
    c) 更新 $F(x) \leftarrow F(x) + \frac{1}{2} f_m(x),and\ p(x) \leftarrow \frac{e^{F(x)}}{e^{F(x)} + e^{-F(x)}}$</br></br>

3. 分类器输出 $sign[F(x)] = sign\big[\sum_{m=1}^M f_m(x)\big]$

# 总结

## AdaBoost 与 Boosting 算法的本质

![](/images/cvml-Boosting分类器-五/2020-03-16-15-57-32.png)

## Boosting 算法比较

|           | 离散 AdaBoost       | 连续 AdaBoost       | LogitBoost    |
|---        | ---                | ---        | ---       |
|损失函数    | $e^{-yf}$          | $e^{-yf}$     | $-\log{(1+ e^{-2yf})}$     |
|权重$w_i$  | $w_i \leftarrow w_i e^{\beta_i I(y_i \ne f_m(x_i))}$</br>$\beta_i = \log{\frac{1-err_m}{err_m}}$    |$w_i \leftarrow w_i e^{-y_i f_m(x_i)}$  | $w_i = p(x_i)(1-p(x_i))$</br>$p(x)=\frac{e^{F(x)}}{e^{F(x)} + e^{-F(x)}}$ |
|弱分类器    | $sign\big[\sum\limits_{m=1}^M \beta_m f_m(x)\big]$</br>$f_m(x) \in \{-1,1\}$</br>利用加权训练样本训练获得 | $sign\big[\sum\limits_{m=1}^M f_m(x)\big]$</br>$f_m(x) = \frac{1}{2} \log{\frac{p_m(x)}{1-p_m(x)}}$</br>$p_m(x) = p(y=1\vert x) \in [0,1]$</br>利用加权训练样本训练获得 | $sign\big[\sum\limits_{m=1}^M f_m(x)\big]$</br>$f_m(x)$</br>利用加权训练样本训练获得 | 

## 不同Boosting算法的性能比较

![](/images/cvml-Boosting分类器-五/2020-03-16-16-38-19.png)

## 总结

- SVM 是线性分类器。应用 kernel 技巧，SVM 可转变成非线性分类器，但 kernel SVM 计算量很大。

- AdaBoost 与其它 Boosting 分类器是当今最强大的分类器之一:

  - 能描述非常复杂的非线性分类边界
  - 具有线性 SVM 的速度，及 kernel SVM 的强大分类能力

- LogitBoost性能比AdaBoost更优越,因为后者不断加大错分训练样本的权重,导致训练后续弱分类器时正负训练样本发生不平衡。
  
# 更多

参考[科研学习-计算机视觉与机器学习](/categories/科研学习-计算机视觉与机器学习/)