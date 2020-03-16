---
title: 绪论(一)
categories:
  - 科研学习 - 计算机视觉与机器学习
tags:
  - 计算机视觉
  - 机器学习
  - 绪论
cover: /images/cover/计算机视觉.jfif
katex: true
abbrlink: 8435c4a5
date: 2020-02-18 16:27:17
---


本系列为西安交通大学“计算机视觉与机器学习”课程的笔记。

# 机器学习算法

## 什么是机器学习算法

机器学习是人工智能的一个分支，它是关于从数据进行学习的研究。

机器学习算法的一般应用框架：

- 定义需要实现的功能
- 采集足够多的正例与负例样本：$T=\{x_i, y_i\}_l^N$
- 利用训练样本 $T=\{x_i, y_i\}_l^N$ 通过迭代训练，得到模型 $y = f(x, \Theta)$
- 如果标签 $y_i \in \{-1, +1\}$ 是离散的，就是一个分类问题；如果 $y_i$ 是连续的，就是一个回归问题

## 机器学习算法的三个重要方面

- Structural model：选择哪一类函数 $f(x, \Theta)$ 来建立模型

  - Gaussian or Gaussian Mixture Model(GMM):
    $$
    f(\boldsymbol x, \Theta) = \sum_i{w_iN(\boldsymbol x; \mu_i, \Sigma_i)}, where\ \Theta = \{w_i, \mu_i, \Sigma_i\}_{i=1}^N
    $$

  - Linear Model:
    $$
    f(\boldsymbol x,\Theta) = \boldsymbol w\cdot x+b, where\ \Theta = \{\boldsymbol w,b\}
    $$
    SVM 是线性模型与hinge损失函数的组合：
    $$
    L(y,f(\boldsymbol x;\Theta)) = \max(0,(1-yf(\boldsymbol x;\Theta)))
    $$

  - Logistic Regression Model:
    $$
    ln(\frac{p}{1-p}) = \beta_0 + \boldsymbol\beta^T \cdot \boldsymbol x
    $$

  - Additive Model:
    $$
    F_M(\boldsymbol x, \Theta) = \sum_{m=1}^M\beta_mb_m(\boldsymbol x; \gamma_m), where\ \Theta = \{\beta_m, \gamma_m\}_{m=1}^M
    $$

    - 当 $b(x, \gamma_m) $ 是sigmoid函数时（例如 $tanh(\gamma_m \cdot x)$ ），$F_m(x, \Theta)$ 代表单隐藏层神经网络
      $$
      z_i = thanh(\sum_{j=1}^n w_{ij}x_j + b_i), y_k = \sum_l \beta_{kl}z_l = \sum_l \beta_{kl}tanh(\sum_j w_{lj}x_j+b_l)
      $$

    - 当 $b(x; \gamma_m)$ 是高斯函数时，$F_M(x,\Theta)$ 代表GMM
    - 当 $b(x; \gamma_m)$ 是小波函数时，$F_M(x,\Theta)$ 代表小波变换

- Error model：选择哪一类损失函数（loss function） $L(y, f(x, \Theta))$ 来做训练，为模型的选择制定考核标准

  - 平方差误差（Squared error）：
    $$
    L(y,f(x;\Theta)) = (y-f(x;\Theta))^2
    $$
    不适用于分类任务。

  - 绝对误差（Absolute error）：
    $$
    L(y,f(x;\Theta)) = ||y-f(x;\Theta)||
    $$
    不适用于分类任务。

  - 指数误差（Exponential loss）：
    $$
    L(y,f(x;\Theta)) = exp(-yf(x;\Theta))
    $$

  - Hinge loss:
    $$
    L(y,f(x;\Theta)) = \max(0, (1-yf(x;\Theta)))
    $$

  - Log-likelihood:
    $$
    L(y,f(x;\Theta)) = \sum_{k=1}^{K} I(y=k)\log p(y=k|x)
    $$
    对于二分类任务：
    $$
    \begin{aligned}
    &L(y,f(x;\Theta)) = y\log p(x)+(1-y)\log(1-p(x)) = -\log(1+e^{-yF(x)}),\\
    &where\ p(x) = \frac{e^{F(x)}}{e^{F(x)}+e^{-F(x)}}
    \end{aligned}
    $$

  - Softmax loss:

    深度学习卷积神经网络常用损失函数，即归一化+Cross entropy

    ![1582012586830](/images/cvml-绪论-一/1582012586830.png)

  **什么是好的损失函数**：①平滑、易求导；②必须是 $yF(x)$的单调函数；③对错误的惩罚不应太大（鲁棒性）

- Optimization procedure：选择哪一种数值计算方法来活去最优模型 $f^*(x, \Theta)$

  - Steepest gradient decent
  - Forward stagewise algorithm
  - Newton-Raphson algorithm (needs to compute second-order derivative)
  - E-M algorithm (Expectation, Maximization)

## 模型是什么

模型是用来描述某个特定现象或事务的。

#### 模型的种类

- 归纳模型（Inductive inference）：由一个数学公式构成，变量都具有明确物理意义，能描述目标系统的规律。

- 预测模型（Predictive inference）：由一个万能函数构成，参数一般不具备物理意义，只能模拟或预测目标系统的输出。

- 直推模型（Transductive inference）：没有明确模型或函数，但可计算出模型在特定点的值。

  - 每个数据都是对目标世界的取样
  - 对所在世界取样足够全面与密集时，就获得了对这个世界的完整描述
  - 当未知数据到来，只要找到与它最相似的样本就可得到该数据的属性和含义

  **对比总结**

  |          | Inductive inference<br />（归纳模型） | Predictive inference<br />（预测模型） | Transductive inference<br />（直推模型） |
  | :------: | :-----------------------------------: | :------------------------------------: | :--------------------------------------: |
  |   目标   |          发现事物的真正规律           |              发现预测规律              |       评估未知预测函数在某些点的值       |
  |  复杂度  |               比较困难                |                相对容易                |                  最容易                  |
  |  适用性  |         少数变量描述简单世界          |          多个变量描述复杂世界          |           多个变量描述复杂世界           |
  | 计算成本 |                  低                   |                   高                   |                   最高                   |
  | 泛化能力 |                  低                   |                   高                   |                   最高                   |

# 机器学习算法分类

## 非监督学习与监督学习

- 非监督学习：不需要训练样本的机器学习算法，如数据聚类算法
- 监督学习：需要训练样本 $T=\{x_i, y_i\}_l^N$ 的机器学习算法，如大多数分类、回归算法

## 生成模型与判别模型（generative vs. discriminative）

- 生成模型计算数据 $x$ 与标签 $y$ 的联合概率 $P(x, y)$，用下列公式计算分类概率：$P_x(y|x) = \frac{P(x, y)}{P(x)}$
- 判别模型直接计算分类概率：$P(y|x)$
- 简单数据模型与复杂数据模型
  - 简单数据模型：被用来处理相互独立的简单数据
  - 复杂数据模型：被用来处理具有时空关联性的复杂数据

# 概率论基本知识

## 条件概率

**定义（definition）：**

假设 $P(B)>0$ ，在 $b$ 已经发生的前提下， $a$ **发生的概率就是条件概率**：
$$
P(a|b) = \frac{P(a,b)}{P(b)}
$$

## 贝叶斯概率理论

**定理（Bayes' Theorem）：**

将全集划分为 $k$ 个部分 $a_1,...,a_k$，且对于每个 $i$ 都有 $P(a_i)>0$，若 $P(b) > 0$，则对每个 $i=1,...,k$ 均有：
$$
P(a_i|b) = \frac{P(b|a_i)P(a_i)}{\sum_j P(b|a_j)P(a_j)}
$$

# 更多

参考[科研学习-计算机视觉与机器学习](/categories/科研学习-计算机视觉与机器学习/)
