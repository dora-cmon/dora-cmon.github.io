---
title: Ubuntu 18.04 使用V2ray图形化软件V2rayL加速上网
categories:
  - 折腾造作 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: b22b8242
date: 2020-04-22 16:53:00
---


安装方法 Ubuntu 18.04 请直接跳转到第四节 [安装](#安装)；其他 Ubuntu 版本请跳转到第三节[使用](#使用)

# github 项目介绍

## v2ray

V2Ray 是 Project V 下的一个工具。Project V 包含一系列工具，帮助你打造专属的定制网络体系。而 V2Ray 属于最核心的一个。 简单地说，V2Ray 是一个与 Shadowsocks 类似的代理软件，但比Shadowsocks更具优势

V2Ray 用户手册：https://www.v2ray.com

V2Ray 项目地址：https://github.com/v2ray/v2ray-core

## v2rayL

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/v2rayL.png)

v2ray linux 客户端，使用pyqt5编写GUI界面，核心基于v2ray-core(v2ray-linux-64)

开发环境：`ubuntu18.04 + Python3.6`

目前已实现以下功能：

- 全新的UI界面
- 添加订阅地址，自动解析并展示可连接VPN
- 设置自动更新订阅、更换地址
- 支持协议：vmess、shadowsocks
- 通过vmess://、ss://分享链接添加配置，通过二维码添加配置
- 手动添加配置，修改本地监听端口
- 导出配置、生成配置分享链接、生成分享二维码
- 最小化至托盘、测试延时、检查更新
- 透明代理(Beta)
- ......
- 
其中vmess支持websocket、mKcp、tcp 目前程序可能存在一些bug但是没有测试出，若在使用过程中发现bug，请在issue中提交，以便改进。

**透明代理说明**：

透明代理设置参考v2ray教程：[透明代理(TPROXY)](https://guide.v2fly.org/app/tproxy.html)

测试环境： 三台不同的机器(条件有限)

测试时出现问题： 有些透明代理无法生效，导致代理失败。

解决办法：在测试时发现多尝试启动几次(关闭，开启)或重启程序就可以正常使用

后续会进一步深入优化这个问题，透明代理无法使用时可以关闭，不影响其正常使用

## 使用

**使用前请注意**

**所有命令请直接运行，避免导致出现权限问题**

**所有命令请直接运行，避免导致出现权限问题**

使用脚本安装时下载的程序实在ubuntu 18.04 + Python3.6的环境下打包的，因此在Python版本不一致的环境中可能会出现版本不兼容的问题

解决方法(请先运行安装脚本)：

在自己的电脑上重新打包程序，具体方法如下（参考）

1. 运行git clone https://github.com/jiangxufeng/v2rayL.git
2. 进入项目文件夹，然后运行pip install -r requirements.txt
3. 运行cd v2rayL-GUI && pyinstaller -F v2rayLui.py -p config.py -p sub2conf_api.py -p v2rayL_api.py -p v2rayL_threads.py -p utils.py -i images/logo.ico -n v2rayLui
4. 打包后运行mv dist/v2rayLui /usr/bin/v2rayL/v2rayLui 替换安装时下载的程序

## 安装

```bash
bash <(curl -s -L http://dl.thinker.ink/install.sh)
```

## 更新

```bash
bash <(curl -s -L http://dl.thinker.ink/update.sh)
```

## 卸载

```bash
bash <(curl -s -L http://dl.thinker.ink/uninstall.sh)
```

## 展示

![首页](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-19-48-49.png)

![setting1](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-19-49-02.png)

![setting2](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-19-49-12.png)

## 感谢
UI界面设计来源：https://zmister.com/archives/477.html

配置方面参考: https://github.com/2dust/v2rayNG

# 节点推荐

## 免费节点

二爷翻墙，目前来说可用的免费节点，虽然速度不快，但尚且可用。

添加订阅：https://jiang.netlify.com

如果失效，可在 [github项目](https://github.com/ugvf2009/Miles) 中寻找新地址。

## 收费节点

由于我是个人使用，因此推荐便宜可用的订阅（**注意**：稳定性可能不高）:

- 2云 [注册地址](http://www.2yun.icu/auth/register?code=M0ol)
  
  ![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-20-44-32.png)

  按年购买平均 5 元/月。

- VC喵 [注册地址](https://kt.kkkktttt.tk/auth/register?code=h1Uz)

  可免费使用，免费节点速度还不错，但流量仅 1G，每天签到可获得 $< 100M$ 的流量。
  
  ![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-20-47-46.png)

  按月限流量最低 4 元/月。价格便宜但流量有限，适合非大流量用户。

  如果失效，请从[路标站](https://www.vcneko.xyz/)进入。

# 具体使用方式

首先打开 `V2rayL` 并且添加 节点/订阅。测试服务器延迟成功。

在设置中寻找 `Network -> Network Proxy`，并设置代理模式为 `Manual`，`Socks Host` 设置为 `127.0.0.1` 端口为 `1080`:

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-21-53-23.png)

在终端执行 `curl www.google.com`，查看是否加速成功。

若成功，直接开始正常使用即可；若不成功尝试以下方法。

## Proxychains

若加速不成功，则尝试安装 `proxychain`：

```bash
sudo apt install proxychains
```

修改配置 `sudo vi /etc/proxychains.conf`，注释掉 `socks4` 行，添加 `socks5` 行：

```bash
#socks4   127.0.0.1 9050
socks5    127.0.0.1 1080
```
在终端执行 `proxychains curl www.google.com`，查看是否加速成功。

## Chrome 插件 Proxy SwitchyOmega

安装 Chrome：

```bash
proxychains firefox www.google.com
```

进入浏览器后根据提示下载 `chrome` 的`deb`安装包，双击安装即可。

用同样的方式启动 `Chrome`：

```bash
proxychains google-chrome
```
打开扩展市场：

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-22-02-34.png)

搜索并安装 `Proxy SwitchyOmega`，扩展添加完成后，根据提示打开配置页，修改 `proxy` 配置：

![](/images/Ubuntu-18-04-使用V2ray图形化软件V2rayL加速上网/2020-03-18-22-07-11.png)

以后使用时按右上角插件切换代理模式即可。

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[jiangxufeng/v2rayL: v2ray linux GUI客户端，支持订阅、vemss、ss等协议，自动更新订阅、检查版本更新](https://github.com/jiangxufeng/v2rayL)