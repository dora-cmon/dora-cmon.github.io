---
title: 线性分类器(三)
categories:
  - 科研学习 - 计算机视觉与机器学习
tags:
  - 计算机视觉
  - 机器学习
  - 数据聚类
cover: /images/cover/线性分类器.jpg
katex: true
abbrlink: e81cb54b
date: 2020-03-04 10:17:52
---


# 线性回归法

**训练样本**：$S = \{x^{(i)}, y^{(i)}\}^n_{i = 1},\ x^{(i)} \in R^d,\ y^{(i)} \in R$

**Structural Model**: 线性函数 $f(x) = \beta^T x + b$，拥有参数集 $(\beta, b)$

**损失函数**：平方差之和（RSS）

  $$
  \begin{aligned}
  RSS(\beta) &= \sum_{i=1}^n (y^{(i)} - f(x^{(i)}))^2 \\
             &= \sum_{i=1}^n (y^{(i)} - \beta^T x^{(i)} - b)^2
  \end{aligned}
  $$

**求解算法**：梯度下降法

## 线性回归法求解

**引入**：

  - $d + 1$ 维横向量 $x_t = [1, (x^{(i)})^T]$ </br></br>

  - $n \times (d + 1)$ 数据集矩阵 $X = [x_1, ..., x_n]^T$ </br></br>

  - $n$ 维纵向量 $y = [y^{(1)}, y^{(2)}, ... ,y^{(n)}]^T$ </br></br>
  
  - $d + 1$ 维纵向量 $\beta = \begin{bmatrix} b \\ \beta \end{bmatrix}$

**损失函数**可以改写为：

  $$
  \begin{aligned}
  RSS(\beta) &= \sum_{i=1}^n (y^{(i)} - f(x^{(i)}))^2 \\
             &= \sum_{i=1}^n (y^{(i)} - b - \beta^T x^{(i)})^2 \\
             &= \sum_{i=1}^n (y^{(i)} - x_i \beta)^2 \\
             &= (y - X\beta)^T(y - X\beta)
  \end{aligned}
  $$

对 $\beta$ 求导，并设其为零：

$$
\frac{\partial RSS(\beta)}{\partial \beta} = \frac{\partial}{\partial \beta} ((y - X\beta)^T (y-X\beta)) = -2 X^T (y - X\beta) = 0
$$

由此得到解析解：

$$
\beta^* = (X^TX)^{-1} X^T y
$$

新数据 $x$ 的预测值 **$\hat y$ 可由下式求得**：

$$
\hat y = [1, x]^T \beta^*
$$

## 解的物理意义：

$n \times (d+1)$ 数据集矩阵可表示为：$X = \begin{bmatrix} x_1 \\ \vdots \\ x_n \end{bmatrix} = [1, z_1, ..., z_d]$ </br></br>

线性回归法求得的解是把损失函数最小化的解：

$$
\beta^* = \arg \min RSS(\beta) = \|y - X\beta\|^2$$

由 $\beta^*$ 得到的数据 $X$ 的预测值 $\hat y$ 具有如下特性：$y - \hat y$ 与由 $z_1, ..., z_d$ 构成的子空间垂直，因为：

$$
\begin{matrix}
&X^T (y - X\beta^*) = 0 \\
&\ \ \ \ \ \downarrow \\
&\ \ \ \ \ \hat y
\end{matrix}
$$

![](/images/cvml-线性分类器-三/2020-03-03-22-38-29.png)

## 线性回归法的缺点

普通线性回归法的最优解是 $\beta^* = (X^TX)^{-1} X^Ty $

当自变量 $z_1, ...,z_d$ 具有很强关联性时， $X^TX$ 将成为退化矩阵，无法得到 $(X^TX)^{-1}$

$$
X = \begin{bmatrix} x_1 \\ \vdots \\ x_n \end{bmatrix}
  = [1, z_1, ..., z_d]
$$

# Ridge 线性回归法

**引入**：

  - $d + 1$ 维横向量 $x_t = [1, (x^{(i)})^T]$ </br></br>

  - $n \times (d + 1)$ 数据集矩阵 $X = [x_1, ..., x_n]^T$ </br></br>

  - $n$ 维纵向量 $y = [y^{(1)}, y^{(2)}, ... ,y^{(n)}]^T$ </br></br>
  
  - $d + 1$ 维纵向量 $\beta = \begin{bmatrix} b \\ \beta \end{bmatrix}$

**损失函数**：

$$
RSS(\beta) = (y - X\beta)^T (y-X\beta) + \lambda \beta^T\beta
$$

**最优解**是：

$$
\beta^{ridge^*} = (X^TX + \lambda I)^{-1} X^T y
$$

## Ridge 线性回归法的优点

Ridge 线性回归法的最优解是 $\beta^{ridge^*} = (X^TX + \lambda I)^{-1} X^T y$，有效解决了矩阵退化的问题。

它把 $y$ 投影到固有值向量方向，然后对其进行收缩。对不重要的方向收缩较大，重要方向收缩较小。

$$
\begin{aligned}
X \hat \beta^{ridge} &= X(X^TX + \lambda I)^{-1} X^T y \\
                     &= UD(D^2 + \lambda I)^{-1} D U^T y \\
                     &= \sum_{j=1}^p u_j \frac{d_j^2}{d_j^2 + \lambda} u_j^T y, 
\end{aligned}
$$

$XU = UD$
$X = UDU^{-1}$

其中，$U$ 是矩阵 $X$ 的固有向量矩阵，$D$ 是对角固有值矩阵，每个对角要素 $d_i$ 都是 $X$ 的固有值。

![](/images/cvml-线性分类器-三/2020-03-03-23-05-31.png)

# Lasso 线性回归法

**损失函数**：$ RSS(\beta) = (y - X\beta)^T(y-X\beta) + \lambda|\beta|$

**最优解**：无解析解，需通过QP算法求得

**特性**：最优解 $beta$ 是稀疏向量

![](/images/cvml-线性分类器-三/2020-03-03-23-07-35.png)

# 基于线性回归法的分类器

假设训练集的 $N$ 个数据可被分为 $K$ 类，为每一 $k$ 类定义一个指示向量 $y_k = [y_{1k}, y_{2k}, ... , y_{Nk}]^T$，$y_{ik}$ 表示 $x_i$ 属于第 $k$ 类，$y_{ik}=0$ 表示 $x_i$ 不属于第 $k$ 类。

定义一个 $N \times K$ 指示向量矩阵 $Y = [y_1, y_2, ..., y_K]$，用线性回归法可获得 $K$ 个分类器：$B^* = (X^TX)^{-1} X^T Y$

当新数据 $x$ 到来时，他的类别可由下述方法获得：
  - 计算 $K$ 维向量 $f(x) = [f_1(x), ..., f_K(x)] = [(1, x)B^*]$
  - $x$ 的类标签是 $l = \arg \max\limits_k f_k(x)$


## 线性回归法分类器的优缺点

**优点**：计算简单可得到全局最优解

**缺点**：
  - 分类器的损失函数为 $RSS(\beta) = \|y - X\beta\|^2$，并不适合分类目的
  - 无法分开类似如下的数据：
  
  ![](/images/cvml-线性分类器-三/2020-03-03-23-15-06.png)

# 逻辑回归(Logistic Regression)分类器

逻辑回归模型试图达到下述目标：

- 使用 $x$ 的线性函数来建立模型；
- 模型取值区间为 $[0, 1]$，求和为 $1$。
  
$$
\begin{matrix} 
  \log \frac{Pr(G=1 | X=x)}{Pr(G=K | X=x)} = \beta_{10} + \boldsymbol{\beta_1^T x} \\\\
  \log \frac{Pr(G=2 | X=x)}{Pr(G=K | X=x)} = \beta_{20} + \boldsymbol{\beta_2^T x} \\\\
  \vdots\\\\
  \log \frac{Pr(G=K-1 | X=x)}{Pr(G=K | X=x)} = \beta_{(K-1)0} + \boldsymbol{\beta_{K-1}^T x}
\end{matrix}
$$

- 尽管模型中使用最后一类的概率做分母，实际上对坟墓的选择是任意的。

上述公式可转换为：

$$
Pr(G=k|X=x) = \frac{\exp(\beta_{k0} + \beta^T_k x)}{1 + \sum^{K-1}_{l=1} \exp(\beta_{l0} + \beta^T_l x)}, k=1,2,...,K-1
$$
$$
Pr(G=K|X=x) = \frac{1}{\sum^{K-1}_{l=1} \exp(\beta_{l0} + \beta^T_l x)}
$$

显然，上述 structural model 符合取值区间为 $[0, 1]$，和为 $1$ 等条件

**二项逻辑回归模型**具有如下条件概率分布：


$$
\begin{aligned}
&Pr(Y=1|x) = \frac{\exp(\beta_0 + \beta^T x)}{1 + \exp(\beta_0 + \beta^T x)} \\
&Pr(Y=0|x) = \frac{1}{1 + \exp(\beta_0 + \beta^T x)} \\
&\log \frac{P(Y=1|x)}{P(Y=0|x)} = \beta_0 + \beta^T x
\end{aligned}
$$

![](/images/cvml-线性分类器-三/2020-03-03-23-25-06.png)

## 逻辑回归模型损失函数

Log-likelihood(对数似然函数)：

  $$
  L(\beta) = \sum \limits_{k=1}^K I(y = k) \log p(y=k | x; \beta)
  $$

对于二分问题，损失函数变为：

  $$
  L(\beta) = y \log p(x;\beta) + (1-y) \log (1-p(x;\beta))
  $$

其中，

  $$
  \begin{matrix}
  p(y=1|x;\beta) = p(x;\beta)   &               \\
                                & y \in \{0,1\} \\
  p(y=0|x;\beta) = 1-p(x;\beta) &               \\
  \end{matrix}
  $$

## 优化算法：牛顿迭代法

**损失函数**变换：

$$
\begin{aligned}
L(\beta) &= \sum_{i=1}^N \{y_i \log p(x_i; \beta) + (1-y_i) \log(1-p(x_i;\beta))\} \\
         &= \sum_{i=1}^N \{y_i \beta^T x_i - \log(1+e^{\beta^T x_i}\}
\end{aligned}
$$

$$
p(x_i; \beta) = \frac{\exp(\beta^T x_i)}{1 + \exp(\beta^T x_i)}
$$

- **$\beta$ 的一次求导**为：

    $$
    \frac{\partial L(\beta)}{\partial \beta} 
    = \sum_{i=1}^N x_i (y_i - p(x_i; \beta)) 
    = 0
    $$

    这是 $d + 1$ 个关于 $\beta$ 的非线性方程。

    因为 $x_i$ 的第一维是 $1$，所以 $\sum_{i=1}^N y_i = \sum_{i=1}^N p(x_i; \beta)$，意思是标签为 1 的样本个数与期望个数相等。

- **$\beta$ 的二次求导**为：

    $$
    \frac{\partial^2 L(\beta)}{\partial \beta \partial \beta^T} 
    = \sum_{i=1}^N x_i x_i^T p(x_i; \beta) (1-p(x_i; \beta)) 
    $$

**Newton-Rapthon 算法**：

$$
\beta^{new} = \beta^{old} - (\frac{\partial^2 L(\beta)}{\partial \beta \partial \beta^T})^{-1} \frac{\partial L(\beta)}{\partial \beta}
$$

## 优化算法的矩阵表示

**引入**：

  - $d + 1$ 维横向量 $x_t = [1, (x^{(i)})^T]$ </br></br>

  - $n \times (d + 1)$ 数据集矩阵 $X = [x_1, ..., x_n]^T$ </br></br>

  - $n$ 维纵向量 $y = [y^{(1)}, y^{(2)}, ... ,y^{(n)}]^T$ </br></br>
  
  - $n$ 维纵向量 $p = [p(x_1; \beta^{old}), ..., p(x_n; \beta^{old})]^T$ </br></br>
  
  - $n \times n$ 的对角矩阵 $W, w_i = p(x_i; \beta^{old})(1-p(x_i; \beta^{old}))$

**$\beta$ 的一次二次求导可写为**：

$$
\begin{aligned}
&\frac{\partial l(\beta)}{\partial \beta} 
= \sum_{i=1}^N x_i (y_i - p(x_i; \beta)) = X^T (y-p) \\
&\frac{\partial^2 l(\beta)}{\partial \beta \partial \beta^T}
= -\sum_{i=1}^N x_i x_i^T p(x_i; \beta)(1 - p(x_i; \beta)) 
= -X^T W X
\end{aligned}
$$

故而牛顿迭代步骤为：

$$
\begin{aligned}
\beta^{new} &= \beta^{old} - (\frac{\partial^2 l(\beta)}{\partial \beta \partial \beta^T})^{-1} \frac{\partial l(\beta)}{\partial \beta} \\
            &= \beta^{old} + (X^T W X)^{-1} X^T (y - p) \\
            &= (X^T W X)^{-1} X^T W (X \beta^{old} + W^{-1} (y-p)) \\
            &= (X^T W X)^{-1} X^T W z
\end{aligned}
$$

其中，$z = X \beta^{old} + W^{-1} (y-p)$

上述问题可利用 IRLS(Iteratively Reweighted Least Squares) 算法求解：

$$
\beta^{new} \leftarrow \arg \min_\beta (z - X \beta)^T W (z - X \beta)
$$

迭代初值可设为 $\beta = 0$

# $L_1$ 正规化逻辑回归

LASSO 思想运用于 Logistic Regression，加入一种惩罚函数项得到一个较为精炼的模型，可以在参数估计的同时实现变量或者特征的选择，以提高模型的解释性和预测精度。

$$
\max_{\beta_0, \beta} \{\sum_{i=1}^N [y_i \beta^T x_i - \log(1 + e^{\beta^T x_i})] - \lambda \sum_{j=1}^m |\beta_i|\}
$$

该方程的求解可以用非线性规划的方法和迭代加权 LASSO 算法。

## 逻辑回归算法的特性

损失函数可写为：

$$
\begin{aligned}
L(\beta) &= \sum_{i=1}^N \{y_i \log p(x_i; \beta) + (1-y_i) \log(1-p(x_i;\beta))\} \\
         &= \sum_{i=1}^N \{y_i \beta^T x_i - \log(1+e^{\beta^T x_i}\} \\
         &= \rightarrow -\sum_{i=1}^N \log(1+e^{-y_i^* \beta^T x_i})
\end{aligned}         
$$

其中，$y_i^* \in \{-1, 1\}, p(x_i; \beta) = \frac{\exp(\beta^T x_i)}{1 + \exp(\beta^T x_i)}$


该函数曲线与 SVM 损失函数相似，应与SVM有类似的分类特性。

逻辑回归法的输出直接就是概率 $p(x_i)$，很多情况下比 SVM 还好用。

# 更多

参考[科研学习-计算机视觉与机器学习](/categories/科研学习-计算机视觉与机器学习/)