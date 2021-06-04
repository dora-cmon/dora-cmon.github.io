---
title: Ubuntu 18.04 安装Postman
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: 73127b7d
date: 2020-03-25 23:33:53
---


Postman 是一款功能强大的网页调试和模拟发送HTTP请求的Chrome插件，支持几乎所有类型的HTTP请求，操作简单且方便。

# 下载并解压 Postman

[Postman 官网](https://www.postman.com/downloads/)下载 Linux 版本的安装包。

解压压缩包到 `/opt`：

```bash
sudo tar -xzf Postman-linux-x64-*.tar.gz -C /opt/
```

**可能遇到的错误**：

- 错误1：
  
    ```bahs
    ./Postman: error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory
    ```

    解决方案：
    
    ```bash
    sudo apt-get install libgconf-2-4
    ```

- 错误2：
  
    ```bash
    Gtk-Message: Failed to load module "canberra-gtk-module"
    ```

    解决方案：
    
    ```bash
    sudo apt install libcanberra-gtk-module
    ```

# 创建快捷图标

创建链接：

```bash
sudo ln -s /opt/Postman/Postman /usr/bin/postman
```

创建快捷图标：

```bash
cat > ~/.local/share/applications/postman.desktop <<EOL
```

然后输入以下代码：

```
[Desktop Entry]
Encoding=UTF-8
Name=Postman
Exec=postman
Icon=/opt/Postman/app/resources/app/assets/icon.png
Terminal=false
Type=Application
Categories=Development;
EOL
```

在启动器搜索 Postman 并将其添加到 Dock 栏。

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[在Ubuntu18.04系统中Postman的安装、配置及使用](https://ywnz.com/linuxjc/3158.html)