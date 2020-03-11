---
title: Win10+Ubuntu双系统时钟不一致解决办法
categories:
  - 环境搭建 - Linux
tags:
  - 双系统
  - Ubuntu
  - Linux
abbrlink: 710a6b23
cover: /images/cover/ubuntu.jfif
date: 2020-02-23 22:58:13
---


双系统安装完成后，会出现两系统时间不一致的现象。这是因为两系统对CMOS始终的定义不同，可以通过修改Windows注册表解决：

按`Win + R`，输入 regedit 打开注册表->定位到`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation`->新建一个DWORD(32位)值，命名为`RealTimeIsUniversal`->赋值为`1`->重新同步时间即可。

![注册表Time](/images/Win10-Ubuntu双系统时钟不一致解决办法/2020-02-23-22-56-53.png)

如果以上操作后，系统时间还是不一致，尝试在Ubuntu系统中输入命令：

```bash
timedatectl set-local-rtc 1 --adjust-system-clock
```

这样，双系统时间应当会保持一致。

# 更多

更多VS Code相关配置见[环境搭建 - Linux](/categories/环境搭建-Linux/)