---
title: >-
  2019(Sauhaarda Chowdhuri) MultiNet: Multi-Modal Multi-Task Learning for
  Autonomous Driving
categories:
  - 科研学习 - 文献阅读
tags:
  - Multimodal
  - E-learning
cover: /images/cover/scholar_logo.png
katex: true:
---

使用多模态多任务学习，在单个深度神经网络中预测多种不同行为模式。 

对与主要任务相关的辅助任务进行训练，可以增强主要任务的学习能力。这些辅助任务，例如车道或车辆检测，可以提高训练质量，从而提高主要任务的性能。

![训练方式](/images/2019-Sauhaarda-Chowdhuri-MultiNet-Multi-Modal-Multi-Task-Learning-for-Autonomous-Driving/2020-11-08-17-19-15.png)

多模态方法通过自动编码器或连接，在网络的某个阶段将多个输入融合为一个共享表示。然后由后期的融合网络处理该共享表示，以产生所需的输出。

# 主要贡献

将行为模式用作网络的第二种输入类型，将其与输入图像序列连接在一起，允许在单个多模态网络中预测驾驶行为。

# 模型架构

![模型架构](/images/2019-Sauhaarda-Chowdhuri-MultiNet-Multi-Modal-Multi-Task-Learning-for-Autonomous-Driving/2020-11-08-17-30-39.png)

网络包含两个卷积层，及两个全连接层。在每个卷积层之后进行 max pooling 和批归一化。 
- 最大池化降低维数
- 批归一化防止内部协变量移位

网络以端到端的方式进行训练。

## 模态信息

收集电动机，转向和图像数据时，还存储了汽车的行为模式。

行为信息为三通道的二进制张量，每个通道代表一个不同的行为模态。
为了与卷积网络特征连接在一起，在空间维度上复制行为信息以形成大小为 $3 \times 13 \times 26$ 的二进制张量。
网络中的行为模式信息插入点被选择为位于Z2Color中的第一个卷积层之后（图6），从而允许较早的卷积层可以对输入数据的基本图像处理进行泛化，而无需考虑各个模态的行为。 这在猕猴中复制了视觉数据的处理，在猕猴中，早期视觉皮层从额叶皮层的反馈连接接收来自更高视觉皮层的上下文信息。  [29]。 上下文信息对于输入图像的初始处理不是必需的，因此可以稍后插入到深度神经网络中，如最近的工作所证明的[15，6]。 鲁德解释说，这种初始的硬参数共享通过允许模型找到可以捕获所有任务的表示，从而减少了过度拟合的机会。[23] 

## 损失函数

$$
MSE_{t r a i n}=\frac{1}{2 n}\left(\sum_{t=1}^{n}\left(s_{t}^{\prime}-s_{t}\right)^{2}+\left(m_{t}^{\prime}-m_{t}\right)^{2}\right)
$$