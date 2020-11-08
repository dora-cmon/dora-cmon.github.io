---
title: >-
  2020(HuaizhengZhang) Look, Read and Feel: Benchmarking Ads Understanding with
  Multimodal Multitask Learning
categories: 
  - 科研学习 - 多任务学习
tags:
  - 多任务学习
  - Multi task learning
katex: true
cover: /images/cover/scholar_logo.png:
---

深度多模态多任务框架，该框架集成了多种模式，可以有效地同时实现主题和情感的预测，以实现广告理解。

# 模型框架

## 总体框架

![总体框架](/images/2020-HuaizhengZhang-Look-Read-and-Feel-Benchmarking-Ads-Understanding-with-Multimodal-Multitask-Learning/2020-11-08-10-21-41.png)

$$
\textbf{Input:} \text{AD IMAGE} \rightarrow \textbf{Output:} \{(Y_i^T, Y_i^S\}
$$

框架由三个部分组成

  - 使用预训练模型和 OCR 从广告中提取多模态特征，并通过三个子网络学习多模态特征
  - 两个分层的多模态注意力处理特征，输出任务特定表示
  - 使用多任务学习将学习到的特征转换为最终预测

## 参数解释

- $y^{T}=\left\{y_{j}^{T}\right\}_{j=1}^{q}$ 表示带有 $q$ 类标签的主题标签空间
- $y^{S}=\left\{y_{k}^{S}\right\}_{k=1}^{p}$ 表示带有 $p$ 类标签的情感标签空间
- $\mathcal{D}=\left\{\left(e_{i}, Y_{i}^{T}, Y_{i}^{S}\right) \mid 1 \leq i \leq n\right\}, Y_{i}^{T} \subseteq \mathcal{y}^{T} \text{ and } Y_{i}^{S} \subseteq \mathcal{Y}^{S}$ 表示关于第 $i$ 个图片 $e_i$ 的两组主题和情感标签

最终目标是学习模型 $F$，该模型为给定的广告图像分配适当的主题和情感标签。

## 共享底层模块

$$
\text { Input: AD IMAGE } \rightarrow \text { Output: }\left\{\left(z_{i}^{V},\left\{z_{i j}^{O}\right\}_{j=1}^{L},\left\{z_{i j}^{W}\right\}_{j=1}^{M}\right)\right\}
$$

框架的第一部分是共享的底层模块，包含三个组件：

  - 对象检测识别
  - 图像整体描述
  - 文本检测识别

**对象检测识别**

使用预训练对象检测模型（例如，faster-rcnn）从第 $i$ 个广告图像的 bounding box proposals 中提取对象级特征 $\{x_{i1}^O, x_ {i2} ^ O，\cdots, x_ {iL} ^ O\}$：

$$
\left\{x_{i j}^{O}\right\}_{j=1}^{L}=f_{o b j}\left(\mathrm{e}_{i} ; \theta_{f i x}\right)
$$

构建自动编码器（包含一个编码器和一个解码器）：

$$
\begin{aligned}
z_{i j}^{O} &=f_{\text {enc}}\left(x_{i j}^{O} ; \theta\right) \\
\hat{x}_{i j}^{O} &=f_{\text {dec}}\left(z_{i j ;}^{O} \theta\right)
\end{aligned}
$$

- 编码器将对象特征投影到一个潜在空间中，其中具有相似含义的特征 $\left\{z_{i 1}^{O}, z_{i 2}^{O}, \ldots, z_{i L}^{O}\right\}$ 被聚类在一起
- 解码器将潜在特征重构为 $\left\{\hat{x}_{i 1}^{O}, \hat{x}_{i 2}^{O}, \ldots, \hat{x}_{i L}^{O}\right\}$，使 $x_{ij}^O$ 类似于原始特征 $x$。在推理阶段，使用 $z_{ij}^O$ 作为对象表示。

由于视觉修辞没有监督信息，因此以无监督的方式通过重建损失 $\mathcal{L}_{\text{rec}}$ 来训练：

$$
\mathcal{L}_{r e c}=\frac{1}{B} \frac{1}{L} \sum_{i=1}^{B} \sum_{j=1}^{L}\left(\hat{x}_{i j}^{O}-x_{i j}^{O}\right)^{2}
$$

其中，$B$ 为 batch size。

**整体图像描述**：

提取广告的全局特征：

$$
\begin{aligned}
x_{i}^{V}&= f_{\text {pool}}\left(f_{\text {img}}\left(e_{i} ; \theta_{f i x}\right)\right) \\
 z_{i}^{V}&=f_{M L P}\left(x_{i}^{V} ; \theta\right)
\end{aligned}
$$

首先，采用预训练图像分类模型 $f_{\text{img}}$，将原始图像转换为特征图 $\mathbf{S}^{C, H, W}$，其中 $C$ 是通道数，$H, W$ 是特征图的高度和宽度。
然后，应用池化技术（例如，最大值或平均值）将特征图变换为特征向量。 
最后，将特征向量输入到 MLP 中，以学习更紧凑的特征表示形式。MLP 还可以将特征向量的维度与其他模态特征的维度对齐。

**文本检测识别**：

提取和理解广告图像中包含的文字

首先，使用 OCR 模型从广告中检测和识别 $M$ 个单词 $\{w_{i1}, w_{i2}, \cdots, w_{iM}\}$。
然后，使用 FastText 将这些单词嵌入到连续的单词向量 $\{x_{i1}^W, x_{i2}^W，\cdots, x_{iM}^W\}$中。
最终，将获得的单词嵌入输入到序列处理模型（例如，BLSTM）中，以学习紧凑的单词表示$\{z_{i1}^O, z_{i1}^O，\cdots, z_{i1}^O\}$：

$$
\begin{aligned}
\left\{w_{i j}\right\}_{j=1}^{M} &= f_{O C R}\left(e_{i} ; \theta_{f i x}\right) \\
x_{i j}^{W} &= f_{FastText}\left(w_{i j} ; \theta_{f i x}\right) \\
z_{i j}^{W} &= f_{BLSTM}\left(x_{i j}^{W} ; \theta\right)
\end{aligned}
$$

## 多层多模态注意力

$$
\text { Input: } \left\{\left(z_{i}^{V},\left\{z_{i j}^{O}\right\}_{j=1}^{L},\left\{z_{i j}^{W}\right\}_{j=1}^{M}\right)\right\} \rightarrow \text { Output: } \{r_i^T, r_i^S\}
$$

所有特征都通过分层多模态注意（HMA）模块集成为特定于任务的特征向量。该模块由两个部分组成：
  - 模态内注意
  - 模态间注意

**模态内注意力**

$\{z_{ij}\}_{j=1}^N$ 为特征向量，其中 $N$ 为某种模态中的向量数。
首先， 通过点积对每个向量采用 kernel $q$ 进行滤波，产生相应的有效值 $p_{ij}$。
然后，将所有向量的值传递给 softmax 函数，以生成正权重 $\{a_{ij}\}$，其中 $\sum_{j=1}^N a_{ij} = 1$。

$$
\begin{array}{c}
p_{i j}=\boldsymbol{q}^{\top} z_{i j} \\
a_{i j}=\frac{\exp \left(p_{i j}\right)}{\sum_{j^{\prime}=1}^{N} \exp \left(p_{i j^{\prime}}\right)}
\end{array}
$$

一个模态的最终表示：

$$
\boldsymbol{m}_{i}=\sum_{j=1}^{N} a_{i j} z_{i j}
$$

模态内注意力模拟了人类的视觉系统。它通过对模态分配更大的权重，并将所有特征汇总到的一个特征向量中，从而感知到更重要的信息。

可以使用标准的反向传播和梯度法同时训练两种模态的核矢量 $q$。 

**模态间注意**