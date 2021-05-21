---
title: Ubuntu 20.04 全新安装 优化配置 界面美化 常用软件全攻略
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu2004.png
abbrlink: bbf09ec7
date: 2021-05-21 11:38:13
---


之前将系统从 Ubuntu 18.04 升级到了 Ubuntu 20.04，但是由于升级的方式造成系统不够稳定，Xorg 进程 经常占用 CPU 到 100% 及以上，造成电脑非常卡顿。因此重新进行 Ubuntu 20.04 全盘安装，并进行配置美化。

# 制作 Ubuntu 20.04 镜像 U盘

1. 下载镜像文件

    从 [ubuntu 官方网站](https://releases.ubuntu.com/20.04/) 下载 [ubuntu-20.04.desktop-amd64.iso](https://releases.ubuntu.com/20.04/ubuntu-20.04-desktop-amd64.iso) ISO 镜像文件。

2.  下载 Rufus 镜像制作工具

    从 [Rufus 官方网站](https://rufus.ie/) 下载并安装 [rufus-3.11exe](https://github.com/pbatard/rufus/releases/download/v3.11/rufus-3.11.exe)。

3. 制作 Ubuntu 20.04 系统启动 U盘

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

2. 从 U盘启动安装程序

    开机后狂按 `F12` 选择 `USB` 启动，进入安装引导程序。

    - 选择安装界面语言：

      ![安装界面语言](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-30-20.png)

    - 选择键盘布局：

      ![键盘布局](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-33-13.png)

    - 连接 Wifi 后，选择安装方式和其他选项

      ![更新和其他软件](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-34-36.png)

    - 选择安装类型

      ![安装类型](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-05-10-37-05.png)

      也可选择其他选项，自己创建分区表，参考方案：

        |||
        |---|---|
        |efi|512M|
        |/home|50% of free space|
        |/|50% of free space|

      选择 `efi` 分区进行安装。

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

![区域与语言](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-08-21-20-41.png)

在输入源中可以设置 `所有窗口使用相同的输入源` 或 `每个窗口使用不同的输入源`。

对 `中文(智能拼音)` 进行设置，默认初始状态改为 `英文`：

![输入法设置](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-08-21-23-25.png)

使用时按 `Shift` 切换中英文。

关闭 ibus 表情快捷键，避免冲突，执行命令 `ibus-setup`：

![ibus快捷键](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-11-09-31-15.png)

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

## 设置暗色主题

在 `设置 -> 外观` 中选择 `Dark` 主题。

## 安装 tweaks

1. 安装美化软件

    ```bash
    sudo apt update
    # 安装 gnome-tweak 优化软件
    sudo apt install gnome-tweak-tool
    # 使支持浏览器安装 gnome 插件
    sudo apt install chrome-gnome-shell
    # 开启 gnome shell 扩展
    sudo apt install gnome-shell-extensions
    ```

2. Firfox/Chrome(二选一即可) extension

    安装 GNOME Shell Integration 插件

    - firfox 安装 GNOME Shell integration 插件

        ![gnome shell integration firfox](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-06-29.png)

    - chrome 安装 GNOME Shell integration 插件

        ![gnome shell integration chrome](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-09-08.png)


    开启 `User Themes` :

    ![user themes](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-15-43-15.png)

    如果无法打开网页则需要加速上网，参考本文 [V2rayL](#推荐安装) 小节。

## 更换 GNOME 主题

在 [gnome主题官方网站](https://www.gnome-look.org/)选择自己心仪的主题，**注意需要选择 GTK3 分类下的主题**，其他分类的主题可能不适合 Ubuntu 20.04。以 [Sweet](https://www.gnome-look.org/p/1253385/) 为例：

- 下载 `Sweet-Dark.tar.xz` 并解压至 `~/.themes/` 下。如果没有则创建该目录。

- 重启 `Tweaks` 并在 `外观 -> 应用程序 和 Shell` 中选择该主题：

  ![](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-34-27.png)

  ![Sweet-Dark](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-15-51-04.png)

## 更换软件图标

在 `Full Icon Themes` 类目下选择自己心仪的软件图标主题。以 [Sweet folders](https://www.opendesktop.org/p/1284047/) 和 [Candy icons](https://www.pling.com/p/1305251/) 为例：


- 下载 `Sweet-Rainbow.tar.xz` 和 `candy-icons.tar.xz` 并解压至 `~/.icons/` 下。如果没有则创建该目录。
  
- 重启 `Tweaks` 并在 `Themes -> Icons` 中选择该主题：

  ![Sweet-Rainbow](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-42-44.png)

`Sweet folders` 仅包含文件夹图标，`Candy icons` 仅包含应用程序图标，`Sweet-Rainbow` 继承了 `Candy icons` 因此下载两个图标主题后，选择 `Sweet-Rainbow` 即可同时应用文件夹和程序图标。

然而，其他文件依旧是系统默认图标，再安装一个项目 [suru-plus-dark](https://github.com/gusbemacbe/suru-plus-dark)：

```bash
wget -qO- https://raw.githubusercontent.com/gusbemacbe/suru-plus-dark/master/install.sh | env DESTDIR="$HOME/.icons" sh
# 如果失败，单独运行每个命令
wget https://raw.githubusercontent.com/gusbemacbe/suru-plus-dark/master/install.sh
env DESTDIR="$HOME/.icons" sh install.sh
rm install.sh
```

在 `~/.icons` 目录会增加 `Suru++-Dark` 主题：

修改 `Candy icons` 的继承关系 `vi ~/.icons/candy-icons/index.theme` ：

```bash
Inherits=Suru++-Dark
```

这样，文件夹图标(Sweet-Rainbow)，应用程序图标(candy-icons)，其他应用图标(Suru++-Dark)都将被应用。

![三合一](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-15-28-34.png)

## 更换鼠标图标

在 `Cursors` 类目下选择自己心仪的鼠标主题。以 [Bibata](https://www.gnome-look.org/s/Gnome/p/1197198/) 为例：

![Bibata Classic](/images/Ubuntu-18-04-美化完全攻略/2020-03-19-17-37-02.png)

- 下载 `Bibata_Classic.zip` 并解压至 `~/.icons/`目录下
  
- 重启 `Tweaks` 并在 `外观 -> 光标` 中选择该主题：

  ![Cursor](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-14-54-44.png)

## 更换 dock

安装 gnome 扩展 [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)

配置如下：

![dash to dock 配置](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-14-26-01.png)

## GDM主题设置(登录/锁定界面)

GNOME 显示管理器（GDM）是一个管理图形显示服务和处理图形用户登录的程序，使用 [High Ubunterra](https://www.gnome-look.org/p/1207015/)主题进行美化，这个主题可以锁屏录界面几乎看起来像 macOS 锁屏，运行其脚本可以为桌面设置壁纸，同时在锁定屏幕和登录屏幕上设置相同的壁纸，且带有模糊效果。

- 下载 `High_Ubunterra_DD-2.4(noPass).tar.xz` 注意不同版本针对不同 Ubuntu 版本，具体参考主题首页。

- 解压后，运行其中的安装脚本 `sh ./install.sh`，运行结束按照提示，先按 `ALT + F2`，再按 `r` 回车，让其生效。

- 右键你的壁纸文件，选择 `脚本 -> SetAsWallpaper` 即可。(**注意**：右键一级目录的`设为壁纸`不会设置登录/锁定界面，必须是 `Script` 中的该选项)

## 修改 Terminal 配色

打开 Terminal 的配置文件首选项：

![配置文件首选项](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-08-23-17-40.png)

新建配色方案，设置为内置方案 `Tango 暗色`，设置 `Use transparent backgroud` 调整透明度。

![配色方案](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-09-00-38-55.png)

## gnome 插件推荐

1. [Hide Top Bar](https://extensions.gnome.org/extension/545/hide-top-bar/) 自动隐藏顶栏

2. [Dynamic Panel Transparency](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/) 将顶栏变透明

3. [TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/) 将后台应用托盘置于顶栏

3. [Resource Monitor](https://extensions.gnome.org/extension/1634/resource-monitor/) 在顶栏显示资源使用情况

4. [Scroll Workspace](https://extensions.gnome.org/extension/1438/scroll-workspace/) 在屏幕右侧边缘滚轮切换工作区

5. [Status Area Horizontal Spacing](https://extensions.gnome.org/extension/355/status-area-horizontal-spacing/) 调整顶栏右上角图标间距

6. [Workspace Wraparound](https://extensions.gnome.org/extension/971/workspace-wraparound/) 工作区循环切换(第一个工作区向上切换至最后一个工作区)

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

    - 设置全局模式

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

    在终端执行 `proxychains curl www.google.com`，查看是否加速成功。

2. chrome 浏览器

    从 [官方下载网页](https://www.google.com/intl/zh-CN/chrome/) 下载 deb 包并安装。

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

3. WPS

    从[官方下载网页](https://linux.wps.cn/) 下载 deb 包进行安装。

    安装完成后打开 WPS，提示 `系统缺失字体`：

    ![系统缺失字体](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-07-14-12-23.png)

    寻找一台 Windows 电脑，将 `C:\Windows\Fonts` 中对应的字体复制到 Ubuntu 的 `/usr/share/fonts/wps-office/` 目录下，重新启动 WPS 即可。

4. TIM & 微信

    **提示：** 后台托盘可通过 Gnome 插件 `Topicon Plus` 置于顶栏。

    - 安装 deepin-wine

      ```bash
      git clone "https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git"
      cd deepin-wine-for-ubuntu && sh ./install_2.8.22.sh
      cd .. && rm -rf deepin-wine-for-ubuntu
      ```
      在 https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu 下载所需软件的 deb 包。

    - 安装 TIM

      1. 安装
  
          在 deb 安装包所在路径下运行：

          ```bash
          # 根据下载的版本修改文件名
          sudo dpkg -i deepin.com.qq.office_2.0.0deepin4_i386.deb
          ```

          安装完成后，启动一次 TIM 并退出。

          若中文显示为方块，修改 `/opt/deepinwine/tools/run.sh` 文件：

        ```bash
        # 将 WINE_CMD 修改为如下
        WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"
        ```

      2. 更新

          在 [TIM 官方下载页](https://office.qq.com/download.html) 下载最新 `Tim.exe` 安装包，并在该安装包所在目录下运行：

          ```bash
          # 根据下载的版本修改文件名
          env WINEPREFIX=~/.deepinwine/Deepin-TIM deepin-wine TIM3.1.0.21789.exe
          ```

          然后会弹出安装界面，此时的安装界面可能会出现字体乱码、色块变色等问题，请根据自己的经验猜测各个按钮的所在位置，安装完成后退出。
        
      3. 设置 DPI

          如果电脑分辨率较高，可能 TIM 的界面非常小，执行命令：

          ```bash
          env WINEPREFIX="$HOME/.deepinwine/Deepin-TIM" /usr/bin/deepin-wine winecfg
          ```

          修改 `显示 -> 屏幕分辨率`，设置合适的 dpi，然后重启 TIM。

      4. 处理字体缺失

          将 Windows 系统 `C:\Windows\Fonts` 中的 `微软雅黑, 宋体` 字体拷贝至 `~/.deepinwine/Deepin-TIM/drive_c/windows/Fonts/`

      5. 修改桌面图标

          Deepin-Wine 启动 TIM 后，在 dock 中会打开另一个图标，这不符合习惯，修改 `/usr/share/applications/deepin.com.qq.office.desktop` 文件：

          ```bash
          # 将 TIM.exe 改为小写 tim.exe
          StartupWMClass=tim.exe
          ```

          Deepin-Wine 启动 TIM 后，在关闭程序界面后，虽然程序在后台运行，但如果再次点击程序图标，会关闭后台程序，重新运行一个程序实例，这不符合习惯，

          - 安装 xdotool

              ```bash
              sudo apt install xdotool
              ```
          
          - 修改 `/opt/deepinwine/apps/Deepin-TIM/run.sh` 文件：

            ```bash
            # 原脚本如下
            # BOTTLENAME="Deepin-TIM"
            # APPVER="2.0.0deepin4"
            # 
            # /opt/deepinwine/tools/run.sh $BOTTLENAME $APPVER "$1" "$2" "$3"

            # 修改为
            BOTTLENAME="Deepin-TIM"
            APPVER="2.0.0deepin4"

            TIM_PID=$(pgrep QQExternal.exe)

            if [ ! $TIM_PID ]; then
                /opt/deepinwine/tools/run.sh $BOTTLENAME $APPVER "$1" "$2" "$3"
            else
                xdotool key --window $(xdotool search --limit 1 --all --pid $(pgrep TIM.exe)) "ctrl+alt+z"
            fi
            ```

    - 安装 微信

      1. 安装
  
          在 deb 安装包所在路径下运行：

          ```bash
          # 根据下载的版本修改文件名
          sudo dpkg -i deepin.com.wechat_2.6.8.65deepin0_i386.deb
          ```

          安装完成后，启动一次 微信 并退出。

      2. 更新

          在 [微信 官方下载页](https://pc.weixin.qq.com/) 下载最新 `WeChatSetup.exe` 安装包，并在该安装包所在目录下运行：

          ```bash
          # 根据下载的版本修改文件名
          env WINEPREFIX=~/.deepinwine/Deepin-WeChat/ deepin-wine WeChatSetup.exe
          ```

          然后会弹出安装界面，此时的安装界面可能会出现字体乱码、色块变色等问题，请根据自己的经验猜测各个按钮的所在位置，安装完成后退出。
        
      3. 设置 DPI

          如果电脑分辨率较高，可能 微信 的界面非常小，执行命令：

          ```bash
          env WINEPREFIX="$HOME/.deepinwine/Deepin-WeChat" /usr/bin/deepin-wine winecfg
          ```

          修改 `显示 -> 屏幕分辨率`，设置合适的 dpi，然后重启 微信。

      4. 处理字体缺失

          将 Windows 系统 `C:\Windows\Fonts` 中的 `微软雅黑, 宋体` 字体拷贝至 `~/.deepinwine/Deepin-WeChat/drive_c/windows/Fonts/`

          创建文件 `vi ~/.deepinwine/Deepin-WeChat/font.reg`:
        
          ```bash
            REGEDIT4
            [HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontSubstitutes]
            "MS Shell Dlg"="msyh"
            "MS Shell Dlg 2"="msyh"

            [HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink]
            "Lucida Sans Unicode"="msyh.ttc"
            "Microsoft Sans Serif"="msyh.ttc"
            "MS Sans Serif"="msyh.ttc"
            "Tahoma"="msyh.ttc"
            "Tahoma Bold"="msyhbd.ttc"
            "msyh"="msyh.ttc"
            "Arial"="msyh.ttc"
            "Arial Black"="msyh.ttc"
          ```

          注册 font.reg :

          ```bash
          WINEPREFIX=~/.deepinwine/Deepin-WeChat/ deepin-wine regedit ~/.deepinwine/Deepin-WeChat/font.reg
          ```

          重新运行微信。

      5. 修改桌面图标

          Deepin-Wine 启动 微信 后，在 dock 中会打开另一个图标，这不符合习惯，修改 `/usr/share/applications/deepin.com.wechat.desktop` 文件：

          ```bash
          # 将 WeChat.exe 改为小写 wechat.exe
          StartupWMClass=wechat.exe
          ```
          Deepin-Wine 启动 微信 后，在关闭程序界面后，虽然程序在后台运行，但如果再次点击程序图标，会关闭后台程序，重新运行一个程序实例，这不符合习惯，

          - 安装 xdotool

              ```bash
              sudo apt install xdotool
              ```
          
          - 修改 `/opt/deepinwine/apps/Deepin-WeChat/run.sh` 文件：

            ```bash
            # 原脚本如下
            # BOTTLENAME="Deepin-WeChat"
            # APPVER="2.6.8.65deepin0"
            # EXEC_PATH="c:/Program Files/Tencent/WeChat/WeChat.exe"
            # 
            # WeChat_PID=$(pgrep WeChat.exe)
            # 
            # if [ -n "$EXEC_PATH" ];then
            #     /opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "$EXEC_PATH" "$@"
            # else
            #     /opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "uninstaller.exe" "$@"
            # fi

            # 修改为
            BOTTLENAME="Deepin-WeChat"
            APPVER="2.6.8.65deepin0"
            EXEC_PATH="c:/Program Files/Tencent/WeChat/WeChat.exe"

            WeChat_PID=$(pgrep WeChat.exe)

            if [ ! $WeChat_PID ]; then
                if [ -n "$EXEC_PATH" ];then
                    /opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "$EXEC_PATH" "$@"
                else
                    /opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "uninstaller.exe" "$@"
                fi
            else
                xdotool key --window $(xdotool search --limit 1 --all --pid $(pgrep WeChat.exe)) "ctrl+alt+W"
            fi
            ```

    - 其他 deepin-wine 支持的软件同理
    
5. flameshot 截图工具

    ```bash
    sudo apt install flameshot
    ```

    安装完成后在 `设置 -> 键盘快捷键` 中添加快捷键：

    ![flameshot 快捷键](/images/Ubuntu-20-04-全新安装-完全配置-界面美化-常用软件全攻略/2020-08-06-21-04-30.png)

    效果如图：

    ![flameshot 效果图](/images/Ubuntu-18-04-截图工具-flameshot-转载/2020-03-12-15-49-30.png)

    该工具在非 `100%, 200%` 屏幕缩放下会出现 Bug 暂无解决方案（2020.08.09）。

## 根据需求选择安装

1. VS Code 代码编辑器

    **注意：** 不要从 Ubuntu 自带的商店安装，可能出现无法输入中文的 bug。

    从 [官方下载页面](https://code.visualstudio.com/) 下载 deb 包并安装。

2. 坚果云同步网盘

    使用过 Onedrive， Dropbox， Goole Drive，但各有弊端，当然最重要的还是网络问题。

    - Onedrive 即使网络加速后同步还是很慢，无 Linux 客户端
    - Google Drive 网盘与相册的逻辑不喜欢，无 Linux 客户端
    - Drobox 有 Linux 客户端，但免费账户只允许两个设备绑定

    综上，还是考虑使用国内的坚果云，虽然免费版每月只提供 1G 上传流量和 3G 下载流量，但作为同步盘也勉强够用。

    由于 [官方网站](https://www.jianguoyun.com/s/downloads/linux) 提供的 deb 包安装后无法打开坚果云客户端，因此通过官网提供的源码编译方式进行安装：

    ```bash
    # 安装相关依赖
    sudo apt-get install libglib2.0-dev libgtk2.0-dev libnautilus-extension-dev gvfs-bin python-gi gir1.2-appindicator3-0.1
    # 下载Nautilus插件源代码包
    wget https://www.jianguoyun.com/static/exe/installer/nutstore_linux_src_installer.tar.gz
    # 解压缩，编译和安装Nautilus插件
    tar zxf nutstore_linux_src_installer.tar.gz
    cd nutstore_linux_src_installer && ./configure && make
    sudo make install
    # 重启Nautilus
    nautilus -q
    # 自动下载和安装坚果云其他二进制组件
    ./runtime_bootstrap
    ```

3. AnyDesk 远程桌面控制软件

    从 [官方下载页面](https://anydesk.com/zhs/downloads/linux) 下载 `DBeaver Community Edition` 的 deb 包，并安装。

    AnyDesk 的别名全局唯一且不可修改，一旦配置文件丢失则只能重新设置其他别名。可以备份 `/etc/anydesk/` 下的文件，更换设备后用备份的文件覆盖新文件，可以找回设置。

    启动系统，停留在登陆页时，AnyDesk 远程连接无法建立，通过设置自动登陆可以解决，修改 `/etc/gdm3/custom.conf` 文件：

    ```bash
    # Enabling automatic login
    AutomaticLoginEnable=true
    AutomaticLogin=$USERNAME
    ```

4. Virtual Box 开源虚拟机软件

    从 [官方下载页面](https://www.virtualbox.org/wiki/Linux_Downloads) 下载  `Ubuntu 19.10 / 20.04`的 deb 包，并安装。

5. DBeaver 数据库管理软件

    从 [官方下载页面](https://dbeaver.io/download/) 下载 `DBeaver Community Edition` 的 deb 包，并安装。

6. Postman 测试工具

    从 [官方下载页面](https://www.postman.com/downloads/) 下载 tar.gz 文件，解压到 `/opt/` 目录下并运行：

    ```bash
    # 解压
    sudo tar -xzf Postman-linux-x64-7.29.1.tar.gz -C /opt/
    # 运行
    /opt/Postman/Postman
    ```

    添加快捷访问方式：

    ```bash
    sudo ln -s /opt/Postman/Postman /usr/bin/postman
    # 创建 postman.desktop 文件
    sudo vi ~/.local/share/applications/postman.desktop
    # 添加以下内容
    [Desktop Entry]
    Encoding=UTF-8
    Name=Postman
    Exec=postman
    Icon=/opt/Postman/app/resources/app/assets/icon.png
    Terminal=false
    Type=Application
    Categories=Development;
    ```

    即可从程序目录中启动 Postman。

7. miniconda 环境管理工具

    从 [官方下载页面](https://docs.conda.io/en/latest/miniconda.html) 下载 sh 文件，在文件目录下运行：

    ```bash
    sh ./Miniconda3-latest-Linux-x86_64.sh
    ```

    按提示操作即可。

    通过以下命令设置是否默认激活终端的 conda 环境：

    ```bash
    # true/false
    conda config --set auto_activate_base false
    ```