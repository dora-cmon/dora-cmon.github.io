---
title: Docker 安装指南(Win10)
categories:
  - 环境搭建 - Docker
tags:
  - Docker
  - 换源
cover: /images/cover/docker.jfif
abbrlink: f2fd094e
date: 2020-10-15 17:04:21
---


# 安装 Docker

## Windows 10

### 开启 Hyper-V

打开 `控制面板\程序\程序和功能`，选择 `启用或关闭 Windows 功能`:

![启用或关闭 Windows 功能](/images/Docker-安装指南-Win10/2020-07-30-23-43-41.png)

选中 `Hyper-V`：

![Hyper-V](/images/Docker-安装指南-Win10/2020-07-30-23-45-32.png)

### 下载并安装 Docker

在[官方网站](https://www.docker.com/get-started)下载并安装 Docker。

打开 `Docker Desktop` 若出现 `Not enough memory` 提示：

![Not enough memory](/images/Docker-安装指南-Win10/2020-07-31-00-24-19.png)

右键托盘图标，选择 `Setting`：

![Setting](/images/Docker-安装指南-Win10/2020-07-31-00-25-48.png)

将 `Resources` 中的 `Memory` 调整为 `1G`：

![Memory adjust](/images/Docker-安装指南-Win10/2020-07-31-00-26-39.png)

重启 `Docker Desktop`，若依旧内存不足，可以下载 [RAMmap](https://docs.microsoft.com/zh-cn/sysinternals/downloads/rammap)

![](/images/Docker-安装指南-Win10/2020-07-31-00-40-45.png)

选择菜单栏的 `Empty -> Empty Working Sets`，然后重新启动 `Docker Desktop` 即可。

## Ubuntu

更新ubuntu的apt源索引

```bash
sudo apt-get update
```

安装包允许apt通过HTTPS使用仓库

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```
添加Docker官方GPG key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

设置Docker稳定版仓库

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
添加仓库后，更新apt源索引
 
```bash
sudo apt-get update
```

安装最新版Docker CE（社区版）

```bash
sudo apt-get install docker-ce
```

# 更换镜像源地址

Docker 官方中国区：https://registry.docker-cn.com
网易：http://hub-mirror.c.163.com
中国科技大学：https://docker.mirrors.ustc.edu.cn
阿里云：https://y0qd3iq.mirror.aliyuncs.com

## Windows 10

在 Dash board 中添加镜像源

![更换镜像源](/images/Docker-安装指南-Win10/2020-07-31-10-18-51.png)

## Ubuntu

创建 Docker 的镜像源配置文件 `/etc/docker/daemon.json`，如果没有配置过镜像该文件默认是不存的，在其中增加镜像源：

```bash
{
  "registry-mirrors": ["https://y0qd3iq.mirror.aliyuncs.com"]
}
```

创建完成后重启 Docker 服务：

```bash
service docker restart
```

查看配置是否生效：

```bash
docker info|grep Mirrors -A 1
```

出现以下输出则表示配置已经生效：

```bash
Registry Mirrors:
  https://y0qd3iq.mirror.aliyuncs.com/
```


# 载入 Getting Start 镜像

执行命令 `docker run -d -p 80:80 docker/getting-started`：

![docker/getting-started](/images/Docker-安装指南-Win10/2020-07-31-10-22-42.png)

随后打开网页 `http://localhost`:

![网页](/images/Docker-安装指南-Win10/2020-07-31-10-26-36.png)

Windows 10 系统中，在 Docker Dashboard 中可以管理 Docker 镜像：

![dashboard](/images/Docker-安装指南-Win10/2020-07-31-10-28-53.png)

# Docker 常用命令

## Docker 容器信息

1. 查看 docker 容器版本
   
    ```bash
    docker version
    ```

1. 查看 docker 容器信息

    ```bash
    docker info
    ```

1. 查看 docker 容器帮助

    ```bash
    docker --help
    ```

## 镜像操作

**提示：对于镜像的操作可使用镜像名、镜像长ID和短ID。**

1. 镜像查看

    ```bash
    # 列出本地 images
    docker images
    ```

1. 镜像搜索

    ```bash
    # 搜索仓库镜像
    docker search <image_name>
    ```
    参数：
        `--no-trunc`: 显示镜像完整描述

1. 镜像下载

    ```bash
    # 下载官方最新镜像，等同于 docker pull <image_name>:latest
    docker pull <image_name>
    ```

1. 镜像删除

    ```bash
    # 单个镜像删除，等同于 docker rmi <image_name>:latest
    docker rmi <image_name>
    ```

    参数：
        `-f`: 强制删除（针对基于镜像有运行的容器进程）
    
    示例：

    ```bash
    # 删除本地全部镜像
    docker rmi -f $(docker images -q)
    ```

1. 镜像构建

    ```bash
    # 1) 编写 dockerfile
    cd /docker/dockerfile
    # 2) 构建 docker 镜像
    docker build -f /docker/dockerfile/myubuntu -t myubuntu:1.1
    ```

## 容器操作

**提示：对于容器的操作可使用 CONTAINER ID 或 NAMES**

1. 容器启动

    ```bash
    # 新建并启动容器
    docker run -i -t --name <dock_name>
    # 后台启动容器
    docker run -d <dock_name>
    ```

    参数：

        `-i`: 以交互模式运行
        `-t`: 为容器重新分配一个伪输入终端
        `--name`: 为容器指定一个名称
        `-d`: 以守护方式启动容器


1. 容器的停止与删除

    ```bash
    # 停止一个运行中的容器
    docker stop <dock_name>
    # 杀掉一个运行中的容器
    docker kill <dock_name>
    # 删除一个已停止的容器
    docker rm <dock_name>
    # 删除一个运行中的容器
    docker rm -f <dock_name>
    # 删除多个容器
    docker rm -f $(docker ps -aq)
    docker ps -aq | xargs docker rm
    ```

    参数：
        `-l`: 删除容器间的网络连接
        `-v`: 删除容器，并删除容器挂载的数据卷

# 更多

参考[环境搭建-Docker](/categories/环境搭建-Docker/)

# 参考资料

[记Windows10下安装Docker的步骤](https://blog.csdn.net/xgocn/article/details/80457833)

[Not enough memory to start Docker解决方法](https://blog.csdn.net/xu1194947097/article/details/102730750)

[在Ubuntu中安装Docker和docker的使用](https://www.cnblogs.com/blog-rui/p/11244023.html)

[Ubuntu Linux下修改docker镜像源](https://blog.csdn.net/fenglibing/article/details/92090925)