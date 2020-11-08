---
title: 'Lecture 2: 有监督的多任务学习和迁移学习'
categories: 
  - 科研学习 - 多任务学习
tags:
  - 多任务学习
  - Multi task learning
  - 元学习
  - Meta learning
  - 迁移学习
katex: true
cover: /images/cover/CS330.jpg
date: 2020-10-26 21:52:56
---


# 多任务学习

## 问题陈述

### 符号说明

![深度学习示意图](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-16-41-50.png)

- 单任务学习
    数据集： $\mathcal{D} = \left\{(x, y)_k \right\}$

    目标： $\min_{\theta} \mathcal{L}(\theta, \mathcal{D})$

    典型损失函数-负对数似然：$\mathscr{L}(\theta, \mathscr{D})=-\mathbb{E}_{(x, y) \sim \mathscr{D}}\left[\log f_{\theta}(\mathbf{y} \mid \mathbf{x})\right]$

- 任务
    任务的形式化定义：$\mathscr{T}_{i} \triangleq\left\{p_{i}(\mathbf{x}), p_{i}(\mathbf{y} \mid \mathbf{x}), \mathscr{L}_{i}\right\}$，其中，$p_i$ 为数据生成的分布

    对应数据集；$\mathcal{D}^{\text{tr}}_i, \mathcal{D}^{\text{test}}_i$，其中将 $\mathcal{D}^{\text{tr}}_i$ 计为 $\mathcal{D}_i$

### 基本设置

![基本设置](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-16-51-58.png)

MTL 目标：$\min_\theta{\sum^T_{i=1}{\mathcal{L}_i(\theta, \mathcal{D}_i)}}$

## 模型，目标，优化

### 模型

**任务的训练**

- 两种极端情况
    1. 使用独立的网络对每个任务单独进行训练，不共享参数
        ![单独训练](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-08-56.png)

    2. 将 $\mathbf{z}_i$ 的输入或激活层连接起来，共享全部参数
        ![全部共享](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-10-40.png)
  
- 折衷 MTL 架构

    将 $\theta$ 拆分为共享参数 $\theta^{\text{sh}}$ 和任务特定参数 $\theta^{i}$，从而 MTL 的目标为：
    $$
    \min _{\theta^{\sin }, \theta^{1}, \ldots, \theta^{T}} \sum_{i=1}^{T} \mathscr{L}_{i}\left(\left\{\theta^{s h}, \theta^{i}\right\}, \mathscr{D}_{i}\right)
    $$

    任务特定参数对其特定的任务进行优化；共享参数对所有任务进行优化。

    选择如何训练任务 $\mathbf{z}_i$ 等效于 选择如何/在哪里共享参数。

**通用设置**

1. 基于串联
    ![串联](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-28-30.png)
    简单地连接特征表示作为输入，送入一个线性层来产生输出层。

2. 基于附加
    ![附加](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-23-46.png)
    先将特征表示映射到偏置向量，再将偏置向量添加到输入。

**基于串联或附加的方法实际上是等效的**
![等效](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-32-19.png)

3. 多头架构
    ![多头](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-33-24.png)

4. 基于乘积
    ![乘积](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-34-06.png)
    类似于附加的方法，将特征表示映射到 scaling 向量，再与输入相乘。

    它在每一层都更具表现力，并且使用乘法门也非常自然，它更加一般化.

5. 更复杂的设置
    ![复杂设置](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-23-17-47-36.png)

这些方法目前还是具有一些缺陷：

- 独立于问题，泛化性不足
- 高度依赖于经验或直觉

### 目标

$$
\min_\theta{\sum^T_{i=1}{\mathcal{L}_i(\theta, \mathcal{D}_i)}}
$$

通常对任务赋予不同权值：

$$
\min_\theta{\sum^T_{i=1}{\omega_i \mathcal{L}_i(\theta, \mathcal{D}_i)}}
$$

### 优化

**步骤**
1. 对任务进行小批量采样 $\mathcal{B} \sim \{\mathcal{T}_i\}$
2. 对每个任务小批量采样数据点 $\mathcal{D}^b_i \sim \mathcal{D}_i$
3. 计算小批量的损失 $\hat\mathcal{L}(\theta, \beta) = \sum_{\mathcal{T} \in \beta}{\mathcal{L}_k(\theta, \mathcal{D}_k^b)}$
4. 反向传播损失以计算梯度 $\Delta_\theta{\hat\mathcal{L}}$
5. 将梯度应用于合适的神经网络优化器（例如，Adam）

**注意**
- 该流程确保无论数据量如何，均可以对任务进行统一的采样
- 对于回归问题，需要确保任务标签的大小相同

## 挑战

### 负向迁移

在某些情况下，独立的网络更优，如：

![独立优](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-26-17-52-46.png)

**原因**
- 优化挑战：跨任务干扰，任务学习速率不同
- 可表示性有限：多任务学习通常需要更大的网络

**解决方案**

$$
\min _{\theta^{5}, \theta^{1}, \ldots, \theta^{T}} \sum_{i=1}^{T} \mathscr{L}_{i}\left(\left\{\theta^{s h}, \theta^{i}\right\}, \mathscr{D}_{i}\right)+\sum_{t=1}^{T}\left\|\theta^{t}-\theta^{t^{\prime}}\right\|
$$

这里， $\sum_{t=1}^{T}\left\|\theta^{t}-\theta^{t^{\prime}}\right\|$ 是软参数共享，它可以平滑地约束两个网络之间的权重，允许参数共享的程度非常灵活：

![软参数共享](/images/Lecture-2-有监督的多任务学习和迁移学习/2020-10-26-18-07-16.png)

- 若该值限定为 $0$，则任务之间不共享参数；
- 若该值限定非常大，则类似于硬参数共享；
然而这样会带来新的超参数，需要对其进行优化。

### 过拟合

**原因**：共享不足
**解决方案**：共享更多信息，形成更加强的规则形式

# 迁移学习

## 多任务学习 VS 迁移学习

|多任务学习|迁移学习|
|---|---|
|同时解决多个任务 $\mathcal{T}_1, \cdots, \mathcal{T}_T$|在解决源任务 $\mathcal{T}_a$ 后，将从任务 $\mathcal{T}_a$ 所学的知识迁移，从而解决目标任务 $\mathcal{T}_b$|

关键假设：迁移过程中不能访问数据 $\mathcal{D}_a$
注：任务 $\mathcal{T}_a$ 本身可能包含多个任务

迁移学习是多任务学习的一个有效解决方案，但反之则不然。

## 通过微调进行迁移学习

$$
\phi \leftarrow \theta - \alpha \Delta_\theta{\mathcal{L}(\theta, \mathcal{D}^\text{tr})}
$$

其中，
- $\mathcal{D}^\text{tr}$ 是新任务的训练数据
- $\theta$ 是预训练参数

**如何获取预训练参数**

- ImageNet 分类
- 在大型语料库（如BERT，LMs）上训练的模型
- 其他无监督学习技术
- 任何大型的多样化数据集

**通用实践方法**
- 以较小的学习速率进行微调
- 为靠前的层赋予较小的学习速率
- 冻结较靠前的层，并逐渐解冻
- 重新初始化最后一层
- 通过交叉值搜索超参数
- 架构选择（如，ResNets）