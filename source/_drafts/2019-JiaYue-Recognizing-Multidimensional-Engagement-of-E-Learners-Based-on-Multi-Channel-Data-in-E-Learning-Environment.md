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

