---
title: Ubuntu 18.04 使用Komorebi实现动态桌面
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-23-36-23.png
abbrlink: c3b0ff28
date: 2020-04-22 16:53:16
---


![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-23-36-23.png)

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-23-47-04.png)

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-23-47-32.png)

# 安装 Komorebi

从 github 项目 [release 页面](https://github.com/cheesecakeufo/komorebi/releases) 下载 Komorebi 的 `deb` 文件，并双击安装即可。

若不成功则使用命令安装：

```bash
sudo add-apt-repository ppa:gnome3-team/gnome3 -y
sudo add-apt-repository ppa:vala-team -y
sudo add-apt-repository ppa:gnome3-team/gnome3-staging -y
sudo apt install cmake valac libgtk-3-dev libgee-0.8-dev libclutter-gtk-1.0-dev libclutter-1.0-dev libwebkit2gtk-4.0-dev libclutter-gst-3.0-dev
git clone https://github.com/cheesecakeufo/komorebi.git
cd komorebi
mkdir build && cd build
cmake .. && sudo make install && ./komorebi
```

安装成功后启动软件，若提示缺失文件，使用 `apt` 安装：

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-22-42-06.png)

```bash
sudo apt install gstreamer1.0-libav
```

## 下载动态桌面视频及封面

在 [火萤视频桌面-资源分享板块](http://bbs.huoying666.com/forum-53-1.html) 挑选自己喜欢的动态桌面并下载。

以 [Daughter Of The Flame](http://bbs.huoying666.com/thread-3911-1-1.html)为例，进入页面后下载视频文件，并挑选一张图片保存作为封面。我将两个文件都保存在 `~/Videos/"Daughter Of The Flame"` 目录下。

## 创建动态桌面

搜索 `komorebi` 打开 `Wallpaper Creator`：

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-22-52-35.png)


输入/选择：

- 名称: Daughter Of The Flame
- wallpaper 类型: A video
- 视频位置: path
- 封面位置: path

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-22-58-17.png)

选择是否在桌面显示时间及显示位置，我选择不显示，其他选项全部默认：

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-22-59-30.png)

选择下一步，根据提示在终端中输入命令：

```bash
sudo mv /home/dora_cmon/daughter_of_the_flame /System/Resources/Komorebi
```

重新启动 Komerebi，右键桌面，选择 `Change Wallpaper` 即可看到刚才新建的动态桌面，选择它。

# 关闭 Komorebi 声音

确定后发现动态桌面将视频中的声音播放出来了，但是我不想要音频。经过多方查找，目前没有找到关闭动态桌面的官方方法，也许今后会有更新。

采用迂回战术，打开设置中的 `Sound -> Applications`，将 `Komorebi` 静音：

![](/images/Ubuntu-18-04-使用Komorebi实现动态桌面/2020-03-18-23-10-10.png)

虽然方法不是很优雅，但无奈官方暂无解决办法（也有可能我没有找对），但该方法确实解决问题。

# 进阶——设置 登录/锁屏 界面

打开设置中的 `Background`，将 `Background` 和 `Lock Screen` 都设置为之前保存的封面。

若要实现密码输入界面雾化效果，请参考文章 {% post_link Ubuntu-18-04-美化完全攻略 %} 中的 **GDM主题设置(登录/锁定界面)** 小节。

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[cheesecakeufo/komorebi: A beautiful and customizable wallpapers manager for Linux](https://github.com/cheesecakeufo/komorebi)