---
title: Ubuntu软件推荐
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
abbrlink: 5c8dc1d5
cover: /images/cover/ubuntu.jfif
date: 2020-03-12 17:31:54
---


# Windows 常用软件 Linux 版

## 工具/娱乐

### 微信网页版

因为不想安装 `Wine` 或 `electronic-wechat` 等，因此使用网页版微信，虽然不是最好的解决办法，但还是比较优雅的。该方法需要 `Chrome` 的配合。

使用 Chrome 打开[微信网页版](https://wx.qq.com)，点击右上角菜单，选择 `More tools -> Create Shortcut...`

![](/images/Ubuntu软件推荐/2020-03-12-16-47-00.png)

在弹出的通知中勾选 `Open as window` 然后点击 `Create`。系统会在桌面生成微信网页版的图标，双击并选择 `Trust and Launch`：

![](/images/Ubuntu软件推荐/2020-03-12-16-52-03.png)

然后固定到 `dock`即可：

![](/images/Ubuntu软件推荐/2020-03-12-16-55-16.png)

我认为效果不输 `wine` 等软件。

### QQ Linux 版

界面烂透了还得扫码登录，就不能好好做吗？？！但是基础聊天、文件传输等功能没问题。由于我对 Windows 软件依赖不强，且不想装 `wine`，因此凑和用了。

[官网下载](https://im.qq.com/linuxqq/download.html) `deb` 格式安装包，双击安装即可

### 搜狗拼音输入法

参考文章 {% post_link Ubuntu-18-04-安装中文输入法 %}

### 百度网盘 Linux 版

[官网下载](https://pan.baidu.com/download) `deb` 格式安装包，双击安装即可

### Teamviewer

[官网下载](https://www.teamviewer.cn/cn/download/linux/) `deb` 格式安装包，双击安装即可

## 办公/开发

### WPS

WPS 对 Linux 的支持很好，[官网下载](https://wps.cn/product/wpslinux/)`deb` 格式安装包，双击安装即可

### VS Code

[官网下载](https://code.visualstudio.com/Donload)`deb` 格式安装包，双击安装即可

### Virtual Box

[官网下载](https://www.virtualbox.org/wiki/Linux_Downloads)`deb` 格式安装包，双击安装即可

### JetBrains 系列

官网下载[JetBrains Toolbox](https://www.jetbrains.com/zh-cn/toolbox-app/)`tar.gz`压缩包，解压后双击安装即可

![](/images/Ubuntu软件推荐/2020-03-13-15-38-42.png)

然后在 TOOLBOX 中安装旗下软件即可。

# Linux 软件

## 截图编辑软件 flameshot

参考文章 {% post_link Ubuntu-18-04-截图工具-flameshot-转载 %}

![](/images/Ubuntu-18-04-截图工具-flameshot-转载/2020-03-12-15-49-30.png)

## Dropbox

注意需要科学上网！

为了跨平台，因此选择了 Dropbox。免费账户只有 2G，即使刷完邀请也能到 18G，同时还有链接最多 3 个设备的限制，真的是非常小气了。而收费版巨贵无比，好在我个人使用也算够用。

[官网下载](https://dropbox.com/install?os=lnx)`deb` 格式安装包，双击安装。安装完成后，因为首次登录需要配置代理，因此先在命令行进行安装(科学上网及 proxychains 提前配置好)：

```bash
proxychains /usr/bin/dropbox start -i
```

安装完成后链接电脑，然后在 `Dropbox Preferences -> Proxies` 中设置代理：

![](/images/Ubuntu软件推荐/2020-03-12-17-07-15.png)

关闭命令行，重启软件即可。

## DBeaver 数据库管理系统

[官网下载](https://dbeaver.io/download/)`deb` 格式安装包，双击安装即可

# 未完代续

# 更多

更多 Ubuntu 相关见[环境搭建 - Ubuntu](/categories/环境搭建-Ubuntu/)

# 参考资料

[How to Turn Any Website into an App on Linux - Make Tech Easier](https://www.maketecheasier.com/turn-website-to-app-linux/)