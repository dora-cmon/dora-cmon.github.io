---
title: 2018(Thung) A brief review on multi-task learning
categories: 科研学习 - 多任务学习
tags:
  - 多任务学习
  - Multi task learning
katex: true
cover: /images/cover/scholar_logo.png
---

传统 MTL 算法的典型公式如下：

$$
\min _{\mathbf{W}=\left[\mathbf{w}^{1} \mathbf{w}^{2} \ldots \mathbf{w}^{M}\right]} \sum_{m=1}^{M} L\left(\mathbf{X}^{m}, \mathbf{y}^{m}, \mathbf{w}^{m}\right)+\lambda \operatorname{Reg}(\mathbf{W})
$$

