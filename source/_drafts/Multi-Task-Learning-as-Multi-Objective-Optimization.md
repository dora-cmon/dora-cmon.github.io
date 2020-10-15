---
title: Multi-Task Learning as Multi-Objective Optimization
categories:
- 科研学习 - 多任务学习
tags:
- Multi-task learning
- katex: true
- cover: /images/cover/scholar_logo.png
---

# 摘要

通常 multi-task learning 的任务优化的是多个任务的线性组合，但这只在多个任务是不竞争的情况下是有效的。文章将 multi-task learning 的任务当做 multi-objective optimization 的问题来处理，寻找帕累托最优解。

作者使用基于梯度的多目标优化 (gradient-based multi-objective optimization) 来解决多目标优化问题。

但该算法不能直接应用于参数量巨大和任务多的情况下，因此作者提出优化一个上界，并证明在现实情况下，优化这个上界可以得到帕累托最优解。

# 引言

统计学中最令人震惊的结论之一是 Stein 悖论。Stein（1956）认为，若要估计高斯随机变量，最好是从所有样本中估计三个或三个以上变量的均值，而不是分别单独进行估计，即使这些高斯分布是相互独立的。Stein 悖论是探索多任务学习（MTL）（Caruana，1997）的早期动机。多任务学习是一种学习范式，其中来自多任务的数据被用来获得优于独立学习每个任务的性能。MTL 的潜在优势超出了 Stein 悖论的直接含义，因为即便是真实世界中看似无关的任务也因数据共享的过程而存在很强的依赖性。例如，尽管自动驾驶和目标操纵看似无关，但相同的光学规律、材料属性以及动力学都对基础数据产生了影响。这启发人们在学习系统中使用多任务作为归纳偏好。

典型的 MTL 系统被给定一组输入点和每点各种任务的目标集。设置跨任务的归纳偏好的常用方法是设计一个参数化假设类，它会在不同任务中共享一些参数。一般而言，可以通过为每个任务最小化经验风险的加权和这种优化问题来学习这些参数。但是，只有当一个参数组在所有任务中都有效时，这样的线性组合公式才有意义。换言之，只有当任务之间不存在竞争关系时，最小化经验风险的加权和才有效，但这种情况比较少有。目标冲突的 MTL 需要对任务之间的权衡进行模型，但这已经超出了线性组合能够实现的范围。

MTL 的另一个目标是找到不受任何其它方案主导的解决方案。这种方案就是帕累托最优（Pareto optimal）。本文从寻找帕累托最优解的角度出发探寻 MTL 的目标。

在给定多个标准的情况下，寻找帕累托最优解的问题也被称为多目标优化。目前已有多种多目标优化算法，其中一种叫多梯度下降算法（MGDA），使用基于梯度的优化，证明了帕累托集合上的点是收敛的（Désidéri，2012）。MGDA 非常时候具有深层网络的多任务学习。它可以用每个任务的梯度解决优化问题来更新共享参数。但有两个技术性的问题阻碍了 MGDA 的大规模应用。（i）基本的优化问题无法扩展到高维度梯度，而后者会自然出现在深度网络中。（ii）该算法要求明确计算每个任务的梯度，这就导致反向迭代的次数会被线性缩放，训练时间大致会乘以任务数量。

我们在本文中开发了基于 Frank-Wolfe 且可以扩展到高维问题的优化器。此外，我们还给 MGDA 优化目标提供了一个上界，并表明可以在没有明确特定任务梯度的情况下通过单次反向迭代来计算该优化目标，这使得该方法的计算成本小到可以忽略不计。本文证明，用我们的上界可以在现实假设情况下得到帕累托最优解。最终我们得到了一个针对深度网络多目标优化问题的精确算法，计算开销可以忽略不计。

我们在三个不同的问题上对提出的方法进行了实证评估。首先，我们在 MultiMNIST（Sabour 等人，2017）上做了多数字分类的延伸评估。其次，我们将多标签分类作为 MTL，并在 CelebA 数据集（Liu 等人，2015b）上进行了实验。最后，我们将本文所述方法应用于场景理解问题中。具体而言，我们在 Cityscapes 数据集（Cordts 等人，2016）上做了联合语义分割、实例分割以及深度估计。在我们的评估中，任务数量从 2 到 40 不等。我们的方法明显优于所有基线。

# Multi-Task Learning as Multi-Objective Optimization

## Multi-Task Learning（MTL）

假设有一组任务 ${\mathcal Y^t }_{t\in[T]}$，以及独立同分布的数据 $\{x_i,y_i^1,\ldots,y_i^T\}_{i\in[N]}$，其中 $y_i^t$ 是第 $i$ 个数据点的第 $t$ 个任务的标签。考虑预测函数 $f^t(x;\theta^{sh},\theta^t):\mathcal X \to \mathcal Y^t$ ,其中 $\theta^{sh}$ 是不同任务共享的参数， $\theta^{t}$ 是任务相关的参数。定义任务相关的损失函数 $L^t(\cdot, \cdot):\mathcal Y^t \times \mathcal Y^t\to \mathbb R^+$。

一般的MTL任务优化下面的经验损失：
$$
\min_{\theta^{sh}, \theta^i,i=1,\ldots,T}\sum_{t=1}^Tc^t \hat \mathcal L^t(\theta^{sh}, \theta^{t})
$$ 

$c^t$ 是任务的系数, $\hat \mathcal L^t(\theta^{sh}, \theta^{t})$ 是经验损失，定义为 $\frac{1}{N}\sum_i\mathcal L(f^t(x_i;\theta^{sh},\theta^t),y_i^t)$。

## Multi-Objective Optimization

MTL可以表示成 multi-objective optimization，也就是优化一组相互竞争的目标。multi-objective optimization 的目标函数是:

$$
\min_{\theta^{sh}, \theta^i,i=1,\ldots,T}L(\theta^{sh},\theta^1,\ldots,\theta^T)=\min_{\theta^{sh}, \theta^i,i=1,\ldots,T} (\hat \mathcal L^t(\theta^{sh}, \theta^{1}),\ldots,\hat \mathcal L^t(\theta^{sh}, \theta^{T}))^\top
$$

注意这里和 MTL 不同，这里的目标不是 scalar，而是 vector。

求解的目标是帕累托最优。MTL的帕累托最优定义如下：

    (a) 


# Multiple Gradient Descent Algorithm

multi-objective optimization 可以用梯度下降法求解，其中一个方法是多梯度下降算法 (Multiple Gradient Descent Algorithm)，MGDA 利用 KKT 条件。对于上面的问题KKT条件是：



注意KKT条件是必要条件。满足上面条件的点称为帕累托不动点。考虑下面的问题：

