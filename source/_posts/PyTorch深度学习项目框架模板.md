---
title: PyTorch深度学习项目框架模板
categories:
  - 代码开发 - Pytorch
tags:
  - Pytorch
  - 目录结构
cover: /images/cover/pytorch.png
abbrlink: c64e04ad
date: 2020-05-12 21:20:15
---


# PyTorch 项目模板

一个简单而设计良好的结构对于任何深度学习项目都是必不可少的，因此在pytorch项目中进行了大量实践和贡献之后，这里有一个pytorch项目模板，它结合了**简单性、文件夹结构的最佳实践和良好的OOP设计。**其主要的想法是每次开始你的pytorch项目的时候都会做很多相同的事情，所以包装这些共享的东西将有助于你每次开始一个新的pytorch项目时，或改变核心的想法时的代码构建过程。

所以，一个简单的Pythorch模板，可以帮助您更快地进入项目的主要部分，只关注您的核心（模型架构、训练流程等）

# 项目结构

```python
PyTorch_project
├── config
│   └── defaults.py             # config file.
├──  configs  
│   └── train_mnist_softmax.yml # specific config file for specific model or dataset.
├── data  
│   ├── dataset     # datasets folder that is responsible for all data handling.
│   ├── transforms  # data preprocess folder that is responsible for all data augmentation.
│   ├── build.py            # file to make dataloader.
│   └── collate_batch.py    # file that is responsible for merges a list of samples to form a mini#batch.
├──  engine
│   ├── trainer.py      # train loops.
│   └── inference.py    # inference process.
├── layers              # customed layers.
│   └── conv_layer.py
├── modeling            # model.
│   └── example_model.py
├── solver              # optimizer.
│   └── build.py
│   └── lr_scheduler.py
├── tools               # train/test model.
│   └── train_net.py    # an example of train model that is responsible for the whole pipeline.
└── utils
│   ├── logger.py
│   └── any_other_utils_you_need
└── tests               # unit test.
    └── test_data_sampler.py
```


# 参考资料

[PyTorch深度学习项目框架模板(最佳实践)](https://www.ctolib.com/mip/L1aoXingyu-Deep-Learning-Project-Template.html)