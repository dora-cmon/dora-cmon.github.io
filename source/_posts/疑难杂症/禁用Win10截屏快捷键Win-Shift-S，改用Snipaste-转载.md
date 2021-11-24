---
title: 禁用Win10截屏快捷键Win+Shift+S，改用Snipaste(转载)
abbrlink: aea02211
date: 2021-11-24 11:06:19
categories:
    - 疑难杂症
tags:
    - 截图工具
    - Snipaste
    - 快捷键
cover:
    - /images/cover/snipaste.jpg
---

本文转载自[禁用Win10截屏快捷键Win+Shift+S，改用Snipaste](https://blog.csdn.net/weixin_46201756/article/details/106154786)

# 打开注册表

按下Win+R键，输入regedit，回车即可打开；

![](/images/禁用Win10截屏快捷键Win-Shift-S，改用Snipaste-转载/2021-11-24-11-07-43.png)

# 进入我们要设置的目录

`\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced`

![](/images/禁用Win10截屏快捷键Win-Shift-S，改用Snipaste-转载/2021-11-24-11-08-25.png)

# 设置具体禁用的键

新建一个字符串值，数值名称为 `DisabledHotkeys`，数值数据为 `S`

![](/images/禁用Win10截屏快捷键Win-Shift-S，改用Snipaste-转载/2021-11-24-11-09-04.png)

# 关闭注册表、重启电脑

重启后再试试，Win10自带的截图工具快捷键是不是已经失效了。