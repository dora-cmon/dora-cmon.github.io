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

两种视角下的元学习算法

- 机械视角
    读取整个数据集并预测新数据点的深度网络
    训练该网络使用一个元数据集，该元数据集本身包含许多数据集，每个数据集用于不同的任务