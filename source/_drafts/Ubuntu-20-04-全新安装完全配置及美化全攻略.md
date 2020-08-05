---
title: Ubuntu 20.04 全新安装完全配置及美化全攻略
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu2004.png
---

之前将系统从 Ubuntu 18.04 升级到了 Ubuntu 20.04，但是由于升级的方式造成系统不够稳定，Xorg 进程 经常占用 CPU 到 100% 及以上，造成电脑非常卡顿。因此重新进行 Ubuntu 20.04 全盘安装，并进行配置美化。

# 制作 Ubuntu 20.04 镜像 U盘

1. 下载镜像文件

    从 [ubuntu 官方网站](https://releases.ubuntu.com/20.04/) 下载 [ubuntu-20.04.desktop-amd64.iso](https://releases.ubuntu.com/20.04/ubuntu-20.04-desktop-amd64.iso) ISO 镜像文件。

2.  下载 Rufus 镜像制作工具

    从 [Rufus 官方网站](https://rufus.ie/) 下载并安装 [rufus-3.11exe](https://github.com/pbatard/rufus/releases/download/v3.11/rufus-3.11.exe)。

3. 制作 Ubuntu 20.04 系统启动 U盘

    使用 Rufus 制作系统启动U盘，注意 `分区类型` 选择 `GPT`，`目标系统类型` 选择 `UEFI(非 CSM)。制作系统启动 U盘会清除 U盘数据，注意备份。

    ![rufus](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-01-28.png)

    点击开始，选择 `以 DD 镜像 模式写入`。

    ![以DD镜像模式写入](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-06-51.png)

    **注意：** 不要使用 `ISO镜像 模式` 或 `Ultra ISO` 软件制作 U盘，否则安装过程中可能会出现以下报错：

    ```bash
    # English
    The 'grub-efi-amd64-signned' package failed to install into /tatget/. Without the GRUB boot loader, the installed system will not boot.
    # 中文
    无法将 grub-efi-amd64-signed 软件包安装到 /target/ 中。如果没有 GRUB 启动引导器，所安装的系统将无法启动
    ```

# 全盘安装 Ubuntu 20.04

1. 修改 BIOS 设置

    设置 UEFI(非CSM)

    ![UEFI(非CSM)](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-17-52.png)

    关闭 Secure Boot

    ![Secure Boot](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-25-26.png)

2. 从 U盘启动安装程序

    开机后狂按 `F12` 选择 `USB` 启动，进入安装引导程序。

    - 选择安装界面语言：

      ![安装界面语言](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-30-20.png)

      点击 `安装 Ubuntu`

    - 选择键盘布局：

      ![键盘布局](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-33-13.png)

    - 连接 Wifi 后，选择安装方式和其他选项

      ![更新和其他软件](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-34-36.png)

    - 选择安装类型

      ![安装类型](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-10-37-05.png)

    - 确认分区表

      ![确认分区表](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-20-14-28.png)

    - 选择地区

      ![选择地区](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-11-05-20.png)
     
    - 设置用户名及密码

     ![设置用户名及密码](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-20-15-16.png)

    - 完成安装

      ![完成安装](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-20-43-36.png)

# 系统配置及优化

## 更换 apt 软件源并更新

打开 `Software and Update` 选择中科大源：

![apt 换源](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-21-02-14.png)

更换完成后，更新软件源索引并更新软件：

```bash
sudo apt update
sudo apt upgrade
```

## 设置 ibus 中文输入法

在设置中打开 `区域与语言` 删除 `汉语` 输入法，只保留 `中文(智能拼音)`：

![区域与语言](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-21-07-52.png)

对 `中文(只能拼音)` 进行设置，默认初始状态改为 `英文`：

![](/images/Ubuntu-20-04-全新安装完全配置及美化全攻略/2020-08-05-21-09-42.png)

使用时按 `Shift` 切换中英文。

## 设置 sudo 免密码（慎重）

## 安装 tweaks

1. apt安装

2. chrome extension

3. 插件推荐

# 软件安装推荐

## V2rayL

## chrome 浏览器

## VS Code

**注意：** 不要从 Ubuntu 自带的商店安装，可能出现无法输入中文的 bug。

## Virtual Box

从[官方下载页面](https://www.virtualbox.org/wiki/Linux_Downloads)下载  [Ubuntu 19.10 / 20.04](https://download.virtualbox.org/virtualbox/6.1.12/virtualbox-6.1_6.1.12-139181~Ubuntu~eoan_amd64.deb) deb 包，并安装。