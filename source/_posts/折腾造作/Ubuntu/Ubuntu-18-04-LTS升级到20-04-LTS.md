---
title: Ubuntu 18.04 LTS升级到20.04 LTS
categories:
  - 折腾造作 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu2004.png
abbrlink: 685eebef
date: 2020-07-29 02:27:05
---


Ubuntu 20.04 LTS（Focal Fossa）于4月23日正式发布，这是最新的Ubuntu长期支持（LTS）版本。

# 备份数据

![Ubuntu备份工具界面](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-28-11-47-33.png)

# 升级到20.04 LTS –使用图形向导

使用GUI工具升级到Ubuntu 20.04 LTS:

```bash
sudo do-release-upgrade -d -f DistUpgradeViewGtk3
```

将使用基于Gtk3的GUI启动升级过程

![提示](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-29-02-13-36.png)

首先将要求确认升级:

![确认升级](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-29-02-14-46.png)

向导将在升级过程中禁用锁定屏幕:

![](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-29-02-15-38.png)

该工具将下载必要的系统文件和应用程序以进行升级。可能需要一些时间，请耐心等待。

![下载文件](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-29-02-16-08.png)

在升级过程中，可能会询问您一个或两个问题，如果您想保留某些系统文件的现有设置（例如时区配置），不确定如何选择，则可以安全地使用默认答案。

![](/images/Ubuntu-18-04-LTS升级到20-04-LTS/2020-07-29-02-20-05.png)

一段时间后，升级应该完成，并且您应该会看到自己已登录20.04桌面！

# 疑难杂症

## 升级后无法进入系统

可能由于 Nvidia 显卡驱动引起，启动时选择高级 `Advanced` 启动，并选择旧内核进入系统，卸载 Nvidia 驱动：

```bash
sudo apt-get remove nvidia* -y
```

重启即可进入系统

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[ubuntu下如何卸载nvidia显卡驱动？](https://www.cnblogs.com/dakewei/p/10902899.html)

[How to upgrade from Ubuntu 18.04 LTS to 20.04 LTS today](https://ubuntu.com/blog/how-to-upgrade-from-ubuntu-18-04-lts-to-20-04-lts-today)