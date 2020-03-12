---
title: Ubuntu设置sudo免密码
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: 314a888b
date: 2020-03-12 16:03:02
---


编辑 `sudo vi /etc/sudoers`（如果没有写权限，则 `sudo chmod +w /etc/sudoers`），添加一行命令：

```
username ALL=(ALL:ALL) NOPASSWD:ALL
```

添加后效果如下：

```# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
username ALL=(ALL:ALL) NOPASSWD:ALL
```

注意将 `username` 修改为自己的

注意添加位置，若在靠前的位置添加，可能会被之后的配置覆盖。

# 更多

更多 Ubuntu 相关见[环境搭建 - Ubuntu](/categories/环境搭建-Ubuntu/)