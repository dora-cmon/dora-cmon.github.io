---
title: Ubuntu 18.04 美化完全攻略
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: 42e35e2a
---

[最终效果]()

# 预备工作

## 更换 apt 更新源

打开 `Software & Update`，点击 `Download from` 下拉框，选择阿里源 `China -> mirrors.aliyun.com`：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-18-19-39-10.png)

关闭选项卡，在弹出框中选择 `Reload`：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-18-19-40-58.png)

## 安装显卡驱动

在 `Software & Updates` 中打开 `Additional Drivers`，选择你的显卡：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-18-22-12-38.png)

安装完成后重启系统。

## 加速上网

参考文章 {% post_link Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网 %}

# 桌面主题美化

## 安装 `gnome-tweak-tool`

```bash
sudo apt update
# gnome 美化软件
sudo apt-get install ​gnome-tweaks ​gnome-tweak-tool
# 支持通过浏览器安装 gnome 插件
​sudo apt install chrome-gnome-shell
# 开启gnome扩展
sudo apt-get install gnome-shell-extensions
```

在网页中打开 `https://extensions.gnome.org/`，点击 `Click here to install browser extension` 安装浏览器的 gnome 扩展插件。

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-00-09-19.png)

安装完成后刷新，右上角可以看到该插件图标。

刷新页面并进入 `User Themes`，开启该查件：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-00-12-21.png)

根据提示完成安装。

## 更换 gnome 主题

## 更换软件图标

## 更换鼠标图标

## 更换 gnome shell 主题

## 更换 dock

## 动态/静态 桌面设置

### 静态桌面

在 [WallPaperSite](https://wallpapersite.com) 中下载心仪的壁纸，保存到 `~/Pictures/WallPapers` 中，右键选择 `Scripts -> SetAsWallpaper` 即可。

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-18-23-33-05.png)

### 动态桌面
参考文章 {% post_link Ubuntu-18-04-使用Komorebi实现动态桌面 %}

## GDM主题设置(登录/锁定界面)

## 调整缩放比例

因为在设置的显示中，只能设置 `100%/200%` 缩放，然而这样不是太小就是太大。因此在 `Tweaks -> Fonts` 中修改 `Scaling Factor`：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-00-21-21.png)

**注意**：在 `Tweaks` 中修改缩放比例后在使用 Chrome 时，可能会出现视频无法全屏，只占屏幕的一部分的情况。目前没有找到完美解决方案，但是发现 **不要将 Chrome 最大化，然后点击全屏按钮，就可以正常全屏。** 只能如此作一折中。

## Plymouth 主题设置(引导界面)

# gnome 插件推荐

- [Hide Top Bar](https://extensions.gnome.org/extension/545/hide-top-bar/) 自动隐藏顶栏

  **注意**:在配置中设置呼出顶栏快捷键为 `Alt + T`，原本的快捷键和 Terminal 冲突！！！

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-15-38-54.png)

- [Dynamic Panel Transparency](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/) 将顶栏变透明

- [Resource Monitor](https://extensions.gnome.org/extension/1634/resource-monitor/) 在顶栏显示资源使用情况

# Ubuntu 18.04 软件推荐

参考文章 {% post_link Ubuntu软件推荐 %}

# 更多

更多 Ubuntu 相关见[环境搭建 - Ubuntu](/categories/环境搭建-Ubuntu/)


# 参考资料

[史上最良心的 Ubuntu desktop 美化优化指导(1)](https://zhuanlan.zhihu.com/p/63584709)
[ubuntu终极美化教程](https://blog.csdn.net/weixin_40570554/article/details/81953751)