---
title: Ubuntu 18.04 通过网线共享网络(转载)
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: 6ff9e8ef
date: 2020-07-28 01:03:17
---


本文转载自[Ubuntu18.04通过网线共享网络](https://www.cnblogs.com/jiading/p/11989966.html)


这几天要给实验室一个新电脑装系统，但是实验室路由器好像有点问题，所以决定共享我的笔记本的网络，但是搜了很多教程都是基于Ubuntu16.04的，而Ubuntu18和16在设置上区别还是挺大的，最后直接搜英文才得到的结果：

1. 首先通过网线将一台有无线网卡、已经连接无线网络的设备和一台需要网络连接的设备相连接

2. 终端中输入:

    ```
    nm-connection-editor
    ```

3. 然后在网络连接中的“以太网”选择修改，将IPV4目录下的“方法”修改为“与其他网络共享”即可。