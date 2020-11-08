---
title: 'Lecture 3: 元学习与黑盒元学习'
categories: 科研学习 - 多任务学习
tags:
  - 多任务学习
  - Multi task learning
  - 元学习
  - Meta learning
  - 黑盒
  - Black-Box
katex: true
cover: /images/cover/CS330.jpg  :
---

# 元学习基础

## 问题描述

两种视角下的元学习算法

- 机械视角
  读取整个数据集并预测新数据点的深度网络
  训练该网络使用一个元数据集，该元数据集本身包含许多数据集，每个数据集用于不同的任务

- 概率视角
  从一组任务中提取先验知识，从而高效地学习新任务
  使用先验（小的）训练集学习新任务，来推断最可能的后验参数

### 问题定义

**监督学习**

数据集：

$$
\mathcal{D} = \left\{(x_1, y_1), \cdots, (x_k, y_k) \right\}
$$

其中，
- $x_i$ 是输入
- $y_i$ 是标签

监督学习的目标：

$$
\begin{aligned}
&\arg{\max_{\phi}{\log{p(\phi | \mathcal{D})}}} \\
=&\arg{\max_{\phi}{\log{p(\mathcal{D} | \phi) + \log{p(\phi)}}}} \\
=&\arg{\max_{\phi}{\sum_{i}{\log{p(y_i | x_i, \phi) + \log{p(\phi)}}}}}
\end{aligned}
$$

其中，
- $\phi$ 是模型参数
- $\mathcal{D}$ 是训练数据
- $\log{p(\mathcal{D} | \phi)}$ 是数据似然函数
- $\log{p(\phi)}$ 是正则项（权重衰减）

然而，较好的模型通常都会需要大量的有标签数据；如果数据集较小，则非常容易过拟合；同时，某些任务的标签数据可能非常有限。

**元学习**

如何引入额外的数据？

$$
\begin{aligned}
&\mathcal{D}_{\text{meta-train}} = \left\{\mathcal{D}_1, \cdots, \mathcal{D}_n \right\} \\
&\mathcal{D}_i = \left\{(x^i_1, y^i_1), \cdots, (x^i_k, y^i_k) \right\}
\end{aligned}
$$

监督学习的目标转变为：

$$
\arg{\max_{\phi}{\log{p(\phi | \mathcal{D}, \mathcal{D}_{\text{meta-train}})}}}
$$

在训练新任务时，如何不访问 $\mathcal{D}_{\text{meta-train}}$ 也能训练？

学习元参数 $\theta : p(\theta | \mathcal{D}_{\text{meta-train}})$
元参数 $\theta$ 包含一切在学习新任务时需要从 $\mathcal{D}_{\text{meta-train}}$ 获得的信息。

假设任务特定参数 $\phi$ 与用元训练集学习到的参数 $\theta$ 条件独立： $\phi \perp\!\!\!\!\perp \mathcal{D}_{\text {meta-train}} \mid \theta$ 

$$
\begin{aligned}
\log{p(\phi | \mathcal{D}, \mathcal{D}_{\text{meta-train}}}) 
&= \log \int_{\Theta}{p(\phi | \mathcal{D}, \theta) p(\theta | \mathcal{D}_{\text{meta-train}}}) d\theta \\
&\approx \log{p(\phi | \mathcal{D}, \theta^*)} + \log{p(\theta^* | \mathcal{D}_{\text{meta-train}})}
\end{aligned}
$$

其中，
- $\phi$ 相当于特定于任务的参数
- $\theta$ 相当于共享参数
- $\log{p(\phi | \mathcal{D}, \theta^*)}$ 新任务需要学习的任务特定参数
- $\log{p(\theta^* | \mathcal{D}_{\text{meta-train}})}$ 对应于元训练

**元学习问题**

给定来自 $\mathcal{T}_1, \cdots, \mathcal{T}_n$ 的数据，快速解决新任务 $\mathcal{T}_{\text{test}}$

**关键假设**：元训练任务与元测试任务具有相同的任务分布 $\mathcal{T}_1, \cdots, \mathcal{T}_n \sim p(\mathcal{T}), \mathcal{T}_j \sim p(\mathcal{T})$，类似多任务学习中，任务必须共享结构。

学习元参数 $\theta^*$，即优化 $\theta$ 的过程是元训练：

$$
\theta^* = \arg{\max_{\theta}{}p(\theta | \mathcal{D}_{\text{meta-train}})}
$$

为生成 $\phi$ 而进行的优化是元测试：

$$
\phi^* = \arg{\max_{\theta}{}p(\phi | \mathcal{D}, \theta^*)}
$$

监督学习的目标最终转变为：

$$
\arg{\max_{\phi}{\log{p(\phi | \mathcal{D}, \mathcal{D}_{\text{meta-train}})}}}
\approx \arg{\max_{\phi}{\log{p(\phi | \mathcal{D}, \theta^*)}}}
$$

### 举例

如下的一个五分类任务：

![五分类任务](/images/Lecture-3-元学习与黑盒元学习/2020-10-28-20-26-54.png)

数据严重不足，直接进行监督学习非常容易过拟合。

当拥有其他数据集及分类任务时，可以引入元学习：

![元学习](/images/Lecture-3-元学习与黑盒元学习/2020-10-28-20-29-01.png)

图中，$\mathcal{T}_1, \mathcal{T}_2$ 均为其他任务，它们拥有各自的训练集和测试集。对其训练以优化元参数 $\theta$，该过程为**元训练**。

使用之前的分类任务及其数据集，在 $\theta^*$ 的基础上优化任务特定参数 $\phi$ 的过程为 **元测试**。

![支持集和查询集](/images/Lecture-3-元学习与黑盒元学习/2020-10-28-20-34-42.png)

元训练和元测试均有其训练数据集和测试数据集，这里：
- 训练集 $\mathcal{D}^{\text{tr}}_i$ 被称为**支持集**
- 测试集 $\mathcal{D}^{\text{test}}_i$ 被称为**查询集**

### 多任务学习 VS 迁移学习 VS 元学习

|多任务学习|迁移学习|元学习|
|---|---|---|
| 同时解决多个任务 $\mathcal{T}_1, \cdots, \mathcal{T}_T$ <br> $\min_{\theta}{\sum_{i=1}^{T}{\mathcal{L}_{i}{(\theta, \mathcal{D}_i)}}}$ | 在解决源任务 $\mathcal{T}_a$ 后，将从 $\mathcal{T}_a$ 所学的知识迁移，从而解决目标任务 $\mathcal{T}_b$ | 给定来自 $\mathcal{T}_1, \cdots, \mathcal{T}_n$ 的数据，快速解决新任务 $\mathcal{T}_{\text{test}}$ |

在转移学习和元学习中：通常无法访问到先前的任务（数据集）。
在三种设置中：**任务都必须共享结构**。