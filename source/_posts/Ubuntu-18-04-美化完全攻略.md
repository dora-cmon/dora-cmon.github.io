---
title: Ubuntu 18.04 美化完全攻略
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/Ubuntu-18-04-美化完全攻略/2020-03-19-19-16-41.png
abbrlink: 42e35e2a
date: 2020-04-22 16:52:37
---


![](/images/Ubuntu-18-04-美化完全攻略/screenshot.png)

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-19-16-18.png)

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-19-16-41.png)

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

刷新页面并进入 `User Themes`，开启该插件：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-00-12-21.png)

根据提示完成安装。

## 更换 gnome 主题

在 [gnome主题官方网站](https://www.gnome-look.org/)选择自己心仪的主题，**注意需要选择 GTK3 分类下的主题**，其他分类的主题不适合 Ubuntu 18.04

以 [Layan gtk theme](https://www.gnome-look.org/p/1309214/) 为例：

- 下载 `Layan-dark-solid.tar.xz` 并解压至 `~/.themes/`目录下

- 重启 `Tweaks` 并在 `Themes -> Applications` 中选择该主题：

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-01-38.png)

## 更换鼠标图标

在 `Cursors` 类目下选择自己心仪的鼠标主题。以 [Bibata](https://www.gnome-look.org/s/Gnome/p/1197198/) 为例：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-37-02.png)

- 下载 `Bibata_Classic.zip` 并解压至 `~/.icons/`目录下
  
- 重启 `Tweaks` 并在 `Themes -> Cursor` 中选择该主题：

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-39-17.png)
 
## 更换软件图标

在 `Full Icon Themes` 类目下选择自己心仪的软件图标主题。以 [Korla](https://www.gnome-look.org/s/Gnome/p/1256209) 为例：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-31-44.png)

- 下载 `korla_1-2-5.zip` 并解压，解压目录中有多个风格的目录，将所需风格目录复制到至 `~/.icons/`目录下，如 `korla`
  
- 重启 `Tweaks` 并在 `Themes -> Icons` 中选择该主题：

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-30-42.png)

## 更换 gnome shell 主题

在 `Gnome Shell Themes` 类目下选择自己心仪的 Shell 主题。gnome shell 不同主题安装方法有所不同，具体请参考主题页面提供的安装方法。

以 [Transparent Shell Theme](https://www.gnome-look.org/p/1203425/) 为例，该主题安装简单与之前安装方法一致：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-14-36.png)

  - 下载 `Transparent shell theme 3.7.tar.gz` 并解压至 `~/.themes`目录中

  - 重启 `Tweaks` 并在 `Themes -> Shell` 中选择该主题：

    ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-08-54.png)

为了将模糊效果应用于GNOME Shell UI 元素，需要安装 [Blyr](https://extensions.gnome.org/extension/1251/blyr/) 扩展

## 更换 dock

安装 gnome 扩展 [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)

我的配置如下：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-21-14.png)

## 动态/静态 桌面设置

### 静态桌面

在 [WallPaperSite](https://wallpapersite.com) 中下载心仪的壁纸，保存到 `~/Pictures/WallPapers` 中，右键选择 `Scripts -> SetAsWallpaper` 即可。

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-18-23-33-05.png)

### 动态桌面
参考文章 {% post_link Ubuntu-18-04-使用Komorebi实现动态桌面 %}

## GDM主题设置(登录/锁定界面)

GNOME 显示管理器（GDM）是一个管理图形显示服务和处理图形用户登录的程序，使用 [High Ubunterra](https://www.gnome-look.org/p/1207015/)主题进行美化，这个主题可以锁屏录界面几乎看起来像 macOS 锁屏，运行其脚本可以为桌面设置壁纸，同时在锁定屏幕和登录屏幕上设置相同的壁纸，且带有模糊效果。

- 下载 `High_Ubunterra_2.3(Pass).tar.xz` 注意其他版本针对不同 Ubuntu 版本，具体参考主题首页。

- 解压后，运行其中的安装脚本 `install.sh`，运行结束按照提示，先按 `ALT + F2`，再按 `r` 回车，让其生效。

- 右键你的壁纸文件，选择 `Scripts -> SetAsWallpaper` 即可。(**注意**：右键一级目录的`SetAsWallpaper`不会设置登录/锁定界面，必须是 `Script` 中的该选项)

## 调整缩放比例

因为在设置的显示中，只能设置 `100%/200%` 缩放，然而这样不是太小就是太大。因此在 `Tweaks -> Fonts` 中修改 `Scaling Factor`：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-00-21-21.png)

**注意**：在 `Tweaks` 中修改缩放比例后在使用 Chrome 时，可能会出现视频无法全屏，只占屏幕的一部分的情况。目前没有找到完美解决方案，但是发现 **不要将 Chrome 最大化，然后点击全屏按钮，就可以正常全屏。** 只能如此作一折中。

## Plymouth 主题设置(开机动画)

Plymouth 主题也就是开机动画，可以在 [Plymouth Themes](https://www.gnome-look.org/browse/cat/108/)类目下选择自己喜欢的开机动画主题进行安装。

以 [Futurebuntu Ani-04 Torquoise S Plymouth Theme](https://www.gnome-look.org/p/1368224/) 为例：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-55-36.png)

- 下载 `Futurebuntu-Ani-04-Torquoise-S.tar.bz2` 并解压

- 将主题目录移至 `/usr/share/plymouth/themes/`，如果没有该目录则自行创建：

  ```bash
  sudo mv Futurebuntu-Ani-04-Torquoise-S /usr/share/plymouth/themes/
  ```

- 根据自己的主题，修改以下命令并执行：

  ```bash
  sudo update-alternatives --install /usr/share/plymouth/themes/default.plymouth  default.plymouth /usr/share/plymouth/themes/Futurebuntu-Ani-04-Torquoise-S/Futurebuntu-Ani-04-Torquoise-S.plymouth 100
  ```

- 选择主题：

  ```bash
  sudo update-alternatives --config default.plymouth
  ```
  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-53-49.png)

  ```bash
  sudo update-initramfs -u
  ```

- 不重启测试

  ```bash
  sudo plymouthd ; sudo plymouth --show-splash ; for ((I=0; I<10; I++)); do sleep 1 ; sudo plymouth --update=test$I ; done ; sudo plymouth --quit
  ```

## Grub 主题设置(引导界面)

在 `Grub Themes` 类目下选择自己心仪的软件图标主题。以 [Blur grub](https://www.gnome-look.org/s/Gnome/p/1256209) 为例：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-19-04-56.png)

- 下载 [blur-grub2_hd.tar.gz](https://www.gnome-look.org/p/1220920/) 并解压

- 执行 `./install.sh`

# gnome 插件推荐

- [Hide Top Bar](https://extensions.gnome.org/extension/545/hide-top-bar/) 自动隐藏顶栏

  **注意**:在配置中设置呼出顶栏快捷键为 `Alt + T`，原本的快捷键和 Terminal 冲突！！！

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-15-38-54.png)

- [Dynamic Panel Transparency](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/) 将顶栏变透明

- [Resource Monitor](https://extensions.gnome.org/extension/1634/resource-monitor/) 在顶栏显示资源使用情况

- [Scroll Workspace](https://extensions.gnome.org/extension/1438/scroll-workspace/) 在屏幕右侧边缘滚轮切换工作区

- [Workspace Wraparound](https://extensions.gnome.org/extension/971/workspace-wraparound/) 工作区循环切换(第一个工作区向上切换至最后一个工作区)

# Ubuntu 18.04 软件推荐

参考文章 {% post_link Ubuntu软件推荐 %}

# 更多

更多 Ubuntu 相关见[环境搭建 - Ubuntu](/categories/环境搭建-Ubuntu/)


# 参考资料

[史上最良心的 Ubuntu desktop 美化优化指导(1)](https://zhuanlan.zhihu.com/p/63584709)

[ubuntu终极美化教程](https://blog.csdn.net/weixin_40570554/article/details/81953751)

[How to change boot splash screen in 18.04](https://askubuntu.com/questions/1046370/how-to-change-boot-splash-screen-in-18-04)