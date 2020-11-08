---
title: 'Lecture 1: 课程介绍'
categories: 
  - 科研学习 - 多任务学习
tags:
  - 多任务学习
  - Multi task learning
katex: true
cover: /images/cover/CS330.jpg
abbrlink: bb424e94
date: 2020-10-23 16:16:08
---


课程主页: http://cs330.stanford.edu/
视频教程: https://www.youtube.com/playlist?list=PLoROMvodv4rMC6zfYmnD7UG3LVvwaITY5

# 为什么要使用 多任务学习和元学习

- 更加通用的机器学习系统；

- 没有大型数据库；

- 数据有很长的尾巴；

    ![长尾数据](/images/Lecture-1-课程介绍/2020-10-23-15-45-05.png)

- 需要快速学习新知识;

# 什么是任务

$$
\begin{array}{c}
    \text{dataset } \mathcal{D} \\
    \text{loss function } \mathcal{L}
\end{array}
\longrightarrow
\text{model } f_{\theta}
$$

# 关键假设

坏消息：不同的任务需要共享某种结构。(如果不具备该性质，最好使用单任务学习)

好消息： 许多任务都具有共享结构

# 非形式化定义

多任务学习问题：相比于独立学习，能够更快/更熟练地学习所有任务。
元学习问题：给定之前任务的数据/经验，可以更快/更熟练地学习新任务。

利用数据来自不同任务这一事实,可以获得更好的性能。

