---
title: Ubuntu 20.04 全新安装 完全配置 界面美化 常用软件全攻略
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

1.  下载 Rufus 镜像制作工具

    从 [Rufus 官方网站](https://rufus.ie/) 下载并安装 [rufus-3.11exe](https://github.com/pbatard/rufus/releases/download/v3.11/rufus-3.11.exe)。

1. 制作 Ubuntu 20.04 系统启动 U盘

    使用 Rufus 制作系统启动U盘，注意 `分区类型` 选择 `GPT`，`目标系统类型` 选择 `UEFI(非 CSM)。制作系统启动 U盘会清除 U盘数据，注意备份。

    ![rufus](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-01-28.png)

    点击开始，选择 `以 DD 镜像 模式写入`。

    ![以DD镜像模式写入](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-06-51.png)

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

    ![UEFI(非CSM)](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-17-52.png)

    关闭 Secure Boot

    ![Secure Boot](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-25-26.png)

1. 从 U盘启动安装程序

    开机后狂按 `F12` 选择 `USB` 启动，进入安装引导程序。

    - 选择安装界面语言：

      ![安装界面语言](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-30-20.png)

    - 选择键盘布局：

      ![键盘布局](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-33-13.png)

    - 连接 Wifi 后，选择安装方式和其他选项

      ![更新和其他软件](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-34-36.png)

    - 选择安装类型

      ![安装类型](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-37-05.png)

    - 确认分区表

      ![确认分区表](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-20-14-28.png)

    - 选择地区

      ![选择地区](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-11-05-20.png)
     
    - 设置用户名及密码

      ![设置用户名及密码](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-20-15-16.png)

    - 完成安装

      ![完成安装](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-20-43-36.png)

# 系统配置及优化

## 更换 apt 软件源并更新

打开 `Software and Update` 选择中科大源：

![apt 换源](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-21-02-14.png)

更换完成后，更新软件源索引并更新软件：

```bash
sudo apt update
sudo apt upgrade
```

## 设置 ibus 中文输入法

在设置中打开 `区域与语言` 删除 `汉语` 输入法，只保留 `中文(智能拼音)`：

![区域与语言](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-21-07-52.png)

对 `中文(只能拼音)` 进行设置，默认初始状态改为 `英文`：

![输入法设置](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-21-09-42.png)

使用时按 `Shift` 切换中英文。

## 设置 sudo 免密码（慎重）

为 `/etc/sudoers` 添加写权限：

```bash
sudo chmod +w /etc/sudoers
```

修改 `/etc/sudoers` 文件

```bash
sudo vi /etc/sudoers
```

在 `%sudo ALL=(ALL:ALL) ALL` 后添加一行：

```bash
%sudo ALL=(ALL:ALL) ALL
your-user-name ALL=(ALL) NOPASSWD:ALL
```

注意，添加在之前的话会被 `%sudo ALL=(ALL:ALL) ALL` 覆盖，导致不生效。

取消 `/etc/sudoers` 写权限：

```bash
sudo chmod -w /etc/sudoers
```

如果编辑 `/etc/sudoers` 文件时，手抖写错，导致无法使用 `sudo` 命令，可以重启电脑，在出现 `Ubuntu logo` 时按 `ESC` 键，选择 `Advanced` 高级启动，以 `root` 身份进入终端，修改正确后重启进入系统。

# 界面主题美化

## 安装 tweaks

1. apt 安装

    ```bash
    sudo apt update
    # 安装 gnome-tweak 优化软件
    sudo apt install gnome-tweak-tool
    # 使支持浏览器安装 gnome 插件
    sudo apt install chrome-gnome-shell
    # 开启 gnome 插件
    # sudo apt install gnome-shell-extensions
    ```

1. Firfox/Chrome(二选一即可) extension

    - firfox 安装 GNOME Shell integration 插件

        ![gnome shell integration firfox](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-06-29.png)

    - chrome 安装 GNOME Shell integration 插件

        ![gnome shell integration chrome](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-09-08.png)

    如果无法打开网页则需要加速上网，参eg文 [V2rayL](#推荐安装) 小节。

## 更换 GNOME 主题

在 [gnome主题官方网站](https://www.gnome-look.org/)选择自己心仪的主题，**注意需要选择 GTK3 分类下的主题**，其他分类的主题可能不适合 Ubuntu 20.04。以 [Sweet](https://www.gnome-look.org/p/1253385/) 为例：

- 下载 `Sweet-Dark.tar.xz` 并解压至 `~/.themes/` 下。如果没有则创建该目录。

- 重启 `Tweaks` 并在 `外观 -> 应用程序` 中选择该主题：

  ![Sweet-Dark](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-34-27.png)

## 更换软件图标

在 `Full Icon Themes` 类目下选择自己心仪的软件图标主题。以 [Sweet folders](https://www.opendesktop.org/p/1284047/) 为例：


- 下载 `Sweet-Rainbow.tar.xz` 并解压至 `~/.icons/` 下。如果没有则创建该目录。
  
- 重启 `Tweaks` 并在 `Themes -> Icons` 中选择该主题：

  ![Sweet-Rainbow](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-42-44.png)

## 更换鼠标图标

在 `Cursors` 类目下选择自己心仪的鼠标主题。以 [Bibata](https://www.gnome-look.org/s/Gnome/p/1197198/) 为例：

![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-37-02.png)

- 下载 `Bibata_Classic.zip` 并解压至 `~/.icons/`目录下
  
- 重启 `Tweaks` 并在 `Themes -> Cursor` 中选择该主题：

  ![](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-39-17.png)

## 更换 dock

安装 gnome 扩展 [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)

我的配置如下：

![dash to dock 配置](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-18-21-14.png)

# 软件安装推荐

## 推荐安装

1. V2rayL

    项目地址：https://github.com/jiangxufeng/v2rayL

    由于原项目提供的脚本中下载链接极不稳定，容易导致安装失败，因此我将项目克隆并将所需文件直接上传到仓库中，并略微修改安装脚本。可以克隆以下仓库并运行 `install.sh` 脚本直接安装，也可以将原项目安装脚本链接中的内容下载至本地，然后手动运行每一行命令安装。

    - 克隆仓库

        由于所需资源全部保存在仓库中，因此克隆可能比较耗时。

        ```bash
        git clone https://github.com/zhenglinli/v2rayL.git
        ```

    - 运行安装脚本

        ```bash
        cd v2rayL
        # 安装
        ./install
        # 卸载
        ./uninstall
        ```

    - 添加订阅

        免费订阅地址（可能会失效，各凭本事）：https://jiang.netlify.com

    - 设置全局代理

        在 `系统设置 -> 网络 -> 网络代理` 中设置 `Socks 主机` 为 `127.0.0.1:1080`：

        ![设置代理](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-01-13-12.png)
    
    在终端执行 `curl www.google.com`，查看是否加速成功。

    若加速失败，安装 `proxychains` 进行终端加速：

    安装 `proxychain`：

    ```bash
    sudo apt install proxychains
    ```

    修改配置 `sudo vi /etc/proxychains.conf`，注释掉 `socks4` 行，添加 `socks5` 行：

    ```bash
    #socks4   127.0.0.1 9050
    socks5    127.0.0.1 1080
    ```

    在终端执行 `curl www.google.com`，查看是否加速成功。

1. chrome 浏览器

    从[官方下载网页](https://www.google.com/intl/zh-CN/chrome/)下载 deb 包并安装。

    若无法实现全局加速，使用终端加速打开 `firefox` 并下载安装 Chrome：

    ```bash
    proxychains firefox
    ```

    安装完成后，添加扩展程序 `SwitchyOmega` 并进行设置：

    ![Proxy SwitchyOmega](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-23-48-20.png)

    将`情景模式`中的 `proxy` 设置为：

    - 模式：`SOCKS5` 
    - 服务器：`localhost`
    - 端口：`1080`

    完成后点击插件选择 `直连模式` 或 `proxy` 即可切换是否加速。

1. WPS

    从[官方下载网页]()

1. flameshot 截图软件

    ```bash
    sudo apt install flameshot
    ```

    安装完成后在 `设置 -> 键盘快捷键` 中添加快捷键：

    ![flameshot 快捷键](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-04-30.png)

    效果如图：

    ![flameshot 效果图](/images/Ubuntu-18-04-截图工具-flameshot-转载/2020-03-12-15-49-30.png)

## 根据需求选择安装

1. VS Code

    **注意：** 不要从 Ubuntu 自带的商店安装，可能出现无法输入中文的 bug。

    从[官方下载页面](https://code.visualstudio.com/)下载 deb 包并安装。

1. Virtual Box

    从[官方下载页面](https://www.virtualbox.org/wiki/Linux_Downloads)下载  `Ubuntu 19.10 / 20.04`的 deb 包，并安装。