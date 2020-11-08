---
title: >-
  2019(MathieuPageFortin) Multimodal Multitask Emotion Recognition using Images,
  Texts and Tags
categories:   
  - 科研学习 - 多任务学习
tags:
  - Multimodal
  - Multitask
cover: /images/cover/scholar_logo.png
katex: true
---

提出一种使用多任务框架的多模式模型，该模型可以使用任意数量的模态，同时在缺少某模态数据时还可以执行预测。 

# 模型

![模型](/images/2019-MathieuPageFortin-Multimodal-Multitask-Emotion-Recognition-using-Images-Texts-and-Tags/2020-11-07-17-31-42.png)

模型由如下子模块组成：

- 三个特征提取器（分别用于图像输入、文本输入、标签输入）
- 三个单模分类器
- 三个双模分类器
- 一个三模分类器

## 图像特征提取器

使用在 ImageNet 上预训练的 DenseNet121。将 DenseNet-121 的最后一层替换为 300 个神经元的全连接层，以产生视觉表示 $r_i \in R^{300}$。

## 文本特征提取器

使用 CNN。 由于 CNN 需要固定大小的输入，因此将最大单词数限制为150。
使用 Word2Vec 预训练表示来嵌入输入文本。 
使用 Dropout 减少过度拟合。
使用三个平行的卷积层（100个 filter）来提取窗口大小为 $h_i \times 300$ 的特征，其中 $h_1 = 3，h_2 = 4，h_3 = 5$，以捕获不同的 n-gram 模式。 在每个卷积层 $i$ 之后，使用非线性激活 ReLU 生成特征图谱。 然后计算全局 max, average, min pooling：

$$
pool_i = [\max{(c_i)}, \text{avg}{c_i}, \min{c_i}] \in R^{100 \times 3}
$$

将三个 $\text{Pool}_i$ 层的输出连接起来，并在具有300个神经元的全连接层中生成文本表示 $r_t \in R^{300}$。

## 标签特征提取器

将输入标签的最大数量限制为 10 个。仅保留前 10 个标签，并在必要时填充 0。
首先将标签嵌入 250 维，并随机初始化嵌入矩阵。 使用 300 个 filter 的卷积层，窗口大小为 $1 \times 250$，再接 ReLU。最后使用全局最大池化来获得标签表示 $r_\# \in R^{300}$。

## 单模分类器

三个单模分类器分别使用 $r_i, r_t, r_\#$ 作为输入预测 $\hat{y}_i, \hat{y}_t, \hat{y}_\#$。
每个分类器都具有两个全连接层，分别有 256 和 128 个神经元，且使用 ReLU 激活，最后接 softmax 分类层。

## 双模分类器

三个双模分类器分别使用 $(r_i，r_t),(r_i, r_\#), (r_t, r_\#)$ 作为输入。 使用串联将特征融合融合。 然后，使用类似于单模分类器的结构分别预测 $\hat{y}_{it}, \hat{y}_{i\#}, \hat{y}_{t\#}$。

## 三模分类器

使用 $(r_i，r_t，r_\#)$ 作为输入。特征向量通过串联融合。 然后使用与其他分类器相似的架构进行预测。

# 多任务训练流程

根据以下损失函数来更新相应的分类器和特征提取器：

$$
\begin{aligned}
L=&\ L_{i t \#}\left(\hat{y}_{i t \#}, y\right) \\
&+L_{i t}\left(\hat{y}_{i t}, y\right)+L_{t \#}\left(\hat{y}_{t \#}, y\right)+L_{i \#}\left(\hat{y}_{i \#}, y\right) \\
&+L_{i}\left(\hat{y}_{i}, y\right)+L_{t}\left(\hat{y}_{t}, y\right)+L_{\#}\left(\hat{y}_{\#}, y\right)
\end{aligned}
$$

其中,
  - $L(\cdot)$ 是交叉熵损失
  - $i，t，\#$ 分别表示图像，文本或标签信息

从某种意义上说，方法是多任务的，它可以同时使用所有模式组，同时执行七个分类任务。 
它可以在缺少一种或两种模态数据的情况下仅训练模型的某些部分（将等式中的相应项设为零）。