---
title: NextCloud 同步盘完全配置指南
categories:
tags:
---

# 安装配置 Docker


## 修改 docker 运行根目录（若设备空间充足可略过该步）

修改/添加文件 `vi /etc/docker/daemon.conf`，将 `mnt/usb/docker` 路径换成自己的路径：

```json
{
  "data-root": "/mnt/usb/docker",
}
```
重启 docker 服务：

```bash
systemctl daemon-reload
systemctl restart docker
```

查看 docker 信息：

```bash
docker info
```

在输出中查看 `Root Dir`，确定根目录地址是否已经改变：

```
Docker Root Dir: /mnt/usb/docker
```

# 拉取 Docker 镜像

需要用到的镜像有：

- nextcloud
- mariadb
- redis
- nginx

默认拉取最新版镜像

```bash
docker pull nextcloud && docker pull mariadb && docker pull redis && docker pull nginx
```

# 创建数据库容器

```bash
docker run --name MariaDB -v /mnt/usb/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD='your-password' -e MYSQL_DATABASE=NextCloud --privileged=true --restart always -d mariadb
```