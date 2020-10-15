---
title: Ubuntu软件推荐
categories:
  - 折腾造作 - Ubuntu
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

因为不想安装 `Wine` 或 `electronic-wechat` 等，因此使用网页版微信，虽然不是最好的解决办法，但还是比较优雅的。该方法需要 [Chrome](#chrome) 的配合。

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

### 搜狗拼音输入法 Linux 版

参考文章 {% post_link Ubuntu-18-04-安装中文输入法 %}

### 百度网盘 Linux 版

[官网下载](https://pan.baidu.com/download) `deb` 格式安装包，双击安装即可

### 网易云音乐 Linux 版

[官网下载](https://music.163.com/#/download) `deb` 格式安装包，双击安装即可

## 办公/开发

### V2rayL

参考文章 {% post_link Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网 %}

### Chrome

安装 V2rayL 并加速上网成功后：

**Proxychains**

```bash
sudo apt install proxychains
```

修改配置 `sudo vi /etc/proxychains.conf`，注释掉 `socks4` 行，添加 `socks5` 行：

```bash
#socks4   127.0.0.1 9050
socks5    127.0.0.1 1080
```
在终端执行 `proxychains curl www.google.com`，查看是否加速成功。

**安装 Chrome**：

```bash
proxychains firefox www.google.com
```

进入浏览器后根据提示下载 `chrome` 的`deb`安装包，双击安装即可。

**安装插件 Proxy SwitchyOmega**

用同样的方式启动 `Chrome`：

```bash
proxychains google-chrome
```
打开扩展市场：

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-22-02-34.png)

搜索并安装 `Proxy SwitchyOmega`，扩展添加完成后，根据提示打开配置页，修改 `proxy` 配置：

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-22-07-11.png)

以后使用时按右上角插件切换代理模式即可。

**将网页固定位 APP**

Chrome 可以将任意网页生成为 APP，并固定至 dock，以微信网页版为例，参考小节 [微信网页版](#微信网页版)。

### WPS

WPS 对 Linux 的支持很好，[官网下载](https://wps.cn/product/wpslinux/)`deb` 格式安装包，双击安装即可

### VS Code

[官网下载](https://code.visualstudio.com/Donload)`deb` 格式安装包，双击安装即可

### Virtual Box

[官网下载](https://www.virtualbox.org/wiki/Linux_Downloads)`deb` 格式安装包，双击安装即可

### Teamviewer Linux 版

[官网下载](https://www.teamviewer.cn/cn/download/linux/) `deb` 格式安装包，双击安装即可

### JetBrains 系列

官网下载[JetBrains Toolbox](https://www.jetbrains.com/zh-cn/toolbox-app/)`tar.gz`压缩包，解压后双击安装即可

![](/images/Ubuntu软件推荐/2020-03-13-15-38-42.png)

然后在 TOOLBOX 中安装旗下软件即可。

### Postman

postman是一款功能强大的网页调试和模拟发送HTTP请求的Chrome插件，支持几乎所有类型的HTTP请求，操作简单且方便。

安装方式参考 {% post_link Ubuntu-18-04-安装Postman %}

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

## MySQL

```bash
sudo apt install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```

执行命令 `sudo netstat -tap |grep mysql`，检查是否安装成功：

```
tcp     0   0   localhost:mysql     0.0.0.0:*   LISTEN  10165/mysqld
```
出现以上行，则代表安装成功。

登录数据库：

```bash
# 若 root 账户无密码
mysql -uroot
# 若 root 账户有密码
mysql -uroot -p<your_passwd>
```

修改数据库账户密码（给 `root` 本地用户设置密码）

```sql
mysql> set password for root@localhost=password("111111");
```

## DBeaver 数据库管理系统

[官网下载](https://dbeaver.io/download/)`deb` 格式安装包，双击安装即可

# 未完代续

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[How to Turn Any Website into an App on Linux - Make Tech Easier](https://www.maketecheasier.com/turn-website-to-app-linux/)