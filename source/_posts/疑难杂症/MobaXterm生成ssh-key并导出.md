---
title: MobaXterm生成ssh_key并导出
categories:
  - 疑难杂症
tags:
  - MobaXterm
  - ssh密钥
cover: /images/cover/mobaxterm.jpg
abbrlink: '496913e0'
date: 2020-02-17 12:05:23
---


# 生成密钥

打开菜单`Tools -> MobaKeyGen(SSH key generator)  `：

![菜单选项](/images/Problem-MobaXterm生成ssh-key并导出/菜单选项.png)

选择`RSA`和`2048`参数：

![参数选择](/images/Problem-MobaXterm生成ssh-key并导出/参数选择.png)

# 导出密钥

不要通过点击上图中的`Save public key`和`Save private key`来导出密钥，这样导出的密钥文件并不规范，不能被其他软件识别。

* 导出私钥

  点击菜单栏的`Conversions -> Export OpenSSH key`：

  ![导出私钥](/images/Problem-MobaXterm生成ssh-key并导出/导出私钥.png)

* 导出公钥

  手动复制公钥部分保存至`ssh-key.pub`文件：

  ![公钥部分](/images/Problem-MobaXterm生成ssh-key并导出/公钥部分.png)

