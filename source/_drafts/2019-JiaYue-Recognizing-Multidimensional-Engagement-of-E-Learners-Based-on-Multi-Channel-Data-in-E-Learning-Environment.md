---
title: >-
  2019(JiaYue) Recognizing Multidimensional Engagement of E-Learners Based on
  Multi-Channel Data in E-Learning Environment
categories:
  - 科研学习 - 文献阅读
tags:
  - Multimodal
  - E-learning
cover: /images/cover/scholar_logo.png
katex: true
---

从**情感，行为和认知状态**三个方面来识别学习参与度
这些方面分别由学习者的面部表情，眼球运动行为和简短学习视频的整体表现来表示。

三种状态对应三个数据通道：视频/图像序列，眼动信息；鼠标点击流数据，通过多通道数据融合策略将三个通道的时间序列特征串联。

# 主要贡献

1. 提出集成的计算框架，从情感，行为和认知三个方面来描述和量化在线学习环境中的多维参与度。
2. 通过迁移学习对参数进行微调，使用多渠道特征级融合预测学习者的认知表现。

# 模型架构

![学习专注度认知模型](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-06-21-25-32.png)

输入为三个通道的数据：

  - 视频
  - 眼动学习日志
  - 鼠标动态

输出为学习者专注度的三个要素：

  - 情感状态（面部表情）：开心，讨厌，悲伤，惊讶，恐惧，愤怒，平静
  - 认知状态（课程表现）：0-1，利用多特征融合模型预测
  - 行为状态（眼动行为）：观看视频，阅读教材，打字笔记

## 数据集

- ImageNet 预训练
- USTC-NVIE 学习亚洲人脸特征

## 面部特征提取/情绪状态识别

采用两步特征提取策略：

1. 使用卷积神经网络（CNN）学习表情状态帧的空间图像特征
2. 使用长短时学习（LSTM）学习面部表情的空间特征的时间特征。

![时空特征提取](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-15-39-29.png)

在空间特征提取阶段，根据迁移学习的方法：
首先在 ImageNet 数据集上预训练 CNN 模型（16 层 VGG16 或 572 层 Inception-ReNet-V2），并在 USTC-NVIE 数据集上微调卷积层参数。
然后，通过预训练的模型从输入图像序列中提取每个帧的空间特征，并通过 LSTM 学习其时间特征。 
最后，通过 Softmax 层预测七个基本的面部表情。

采用面部表情来表示学习者的情绪状态。 修改两个 CNN 模型（VGG16 和 Inception-ReNet-V2）网络结构的具体步骤：

1. 删除两个 CNN 模型的全连接层，并在 VGG16 中设置 512 个神经元或在 Inception-ReNet-V2 中设置1024 个神经元。
2. 调整 Softmax 层的输出类数。
3. 在最后一个卷积层后添加全局平均池化层，将四维张量转换为二维张量，同时减小参数量。
4. 添加参数值为 0.2 的 dropout 层，减少过拟合。

![网络参数](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-15-48-44.png)

其中，
  - $N$ 是面部图像序列帧的数量
  - 选择 VGG16 时, $S = 244, M = 512$
  - 选择 Inception-ReNet-V2 时，$S = 299, M = 2048$

## 时间序列特征提取/眼动行为状态识别

![眼动行为](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-15-52-44.png)

- (a) 表示**观看**（注视）
- (b) 表示**阅读**（水平移动，密度低）
- (c) 表示**打字**（水平移动，密度高）

使用两个指标（平均扫视幅度和水平运动比率）来指示在线学习者的三个凝视运动。

$$
S_{\text {average}}=\frac{D_{\text {total}}}{N-1}
$$

其中，
  - $S_{\text{average}}$ 表示注视运动的平均距离
  - $D_{\text{total}}$ 表示一段时间内的总移动距离
  - $N$ 表示当前时间，出现的注视点数量

$$
R_{h}=\frac{\sum_{n-1}^{n-N-1} 1\left\{4 V_{n}<H_{n}\right\}}{N-1}
$$

只要 $4 V_n < H_n$，即表示眼睛水平移动，其中，
  - $V_n, H_n$ 分别表示两个相邻凝视点之间的不同垂直和水平距离。
  - 如果不满足，则当时的眼睛移动应被视为垂直运动。

此外，从眼图数据序列中提取了 40 个一般特征，其中包括统计数据，小波和傅立叶变换。这些时间序列特征与眼动日志的对应特征一起应用于鼠标动态日志分析。

使用 Kolmogorov-Smirnov（KS）Test 和 Benjamini-Yekutieli（BY）procedure 来过滤相关且鲁棒的特征。 由于 KS 测试仅适用于二元分类或回归问题，因此将多个眼行为状态分类转换为三个二元分类问题 $P_W, P_R, P_T$：

$$
\begin{aligned}
P_{W} &=\{(\text { Watching }),(\text {Reading, Typing})\} \\
P_{R} &=\{(\text {Reading}),(\text {Watching, Typing})\} \\
P_{T} &=\{(\text {Typing}),(\text {Watching, Reading})\}
\end{aligned}
$$

然后，依次对二分类问题 $(P)$ 的每个特征集进行 KS Test 和 BY procedure，以获得过滤后的特征：

$$
\begin{aligned}
F_{W} &=\text { filter }\left(\text { feature }\left(P_{W}\right)\right) \\
F_{R} &=\text { filter }\left(\text { feature }\left(P_{R}\right)\right) \\
F_{T} &=\text { filter }\left(\text { feature }\left(P_{T}\right)\right)
\end{aligned}
$$

眼行为的综合特征可以表示为 $F=F_{W} \cup F_{R} \cup F_{T}$。
最后，使用主成分分析（PCA）减小特征尺寸，增强泛化能力。

## 认知状态预测

通过学习者在短视频课程中的表现，以及对知识的掌握程度来研究他们的认知状态。使用 $[0-1]$的 特定值来表示每个短视频中学习者的表现。 

**多源特征融合方法**

![多源特征融合方法](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-16-21-09.png)

首先，分别提取同一时间间隔内学习者的面部表情，视线移动和鼠标动态的时间序列特征。多通道数据的时间序列特征的对齐如（b）所示。
然后，应用 KS Test 和  BY Procedure 来筛选最相关的特征，并使用 PCA 方法减小特征尺寸。
最后，连接三个通道处理后的特征，形成一个融合向量。

**学习者的认知状态标签**

![修正自我评估标签](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-16-27-42.png)

结合学习者的先验学科知识水平，自我评估和测验分数，提高学习者自我报告数据的可靠性。 采取步骤如下：

1. 数据标准化至 [0,1]，归一化公式：

    $$
    S_c = \frac{S - S_{\min}}{S_{\max} - S_{\min}}
    $$

    其中，
      - $S$ 表示每个学习者的自我评估值，
      - $S_{\min}$ 是 $S$ 的最小值，
      - $S_{\max}$ 是 $S$ 的最大值。
 
2. 修正自我评估值 $(S)$，参考每个学习者的先验知识水平和测验分数，重新计算新的标签值。将先验知识水平作为测验分数的影响因素，表示为 $\sigma \in \{1, 2, \cdots, l\}$。用于计算最终标签值（$S_n$）的公式为：

    $$
    S_{n}=\alpha S_{c}+\beta \frac{S_{a}}{\sigma}, \quad \alpha+\beta=1
    $$

    其中，
    - $\alpha$ 是学习者的自我评估（$S_c$）的权重
    - $\beta$ 是学习者的测验分数（$S_a$）的权重，
    - $S_a$ 应该归一化为 $[0,1]$。 

# 实验

## 实验设置与设计

使用三个外部设备，
- 位于计算机屏幕顶部的 Microsoft LifeCam网络摄像头
- 固定在屏幕底部的 Tobii Eye Tracker 
- 一个普通的有线鼠标

三个通道样本数据的参数和规格：

![参数规格](/images/2019-JiaYue-Recognizing-Multidimensional-Engagement-of-E-Learners-Based-on-Multi-Channel-Data-in-E-Learning-Environment/2020-11-08-16-46-38.png)

眼动日志记录的四个维度分别表示为：
```xml
<event_type，x，y，timestamp>
```

其中，
- **event_type** 表示注视的数据类型（BEGIN/DATA/END)，
- **x, y** 是注视的坐标，
- **timestamp** 时间戳

鼠标动态日志记录为：
```xml
<message，wheel，x，y，window，timestamp>
```
其中，
- **message** 表示单击事件的类型，
- **wheel** 表示滚轮的方向，
- **window** 表示当前的活动窗口，
- **x,y** 表示鼠标光标的坐标，
- **timestamp** 时间戳。

## 面部表情模型评估

