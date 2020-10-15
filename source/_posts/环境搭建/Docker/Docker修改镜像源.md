---
title: Docker修改镜像源
categories:
  - 环境搭建 - Docker
tags:
  - Docker
  - 换源
cover: /images/cover/docker.jfif
abbrlink: cb6da15d
date: 2020-02-26 12:47:26
---


安装 docker 后拉取镜像非常慢，尝试更换为中国镜像源，更换多个镜像源后发现阿里源最快。

# 获取加速器地址

[阿里云镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/mirrors)

[镜像加速器](/image/Docker修改镜像源/镜像加速器.png)

# 修改配置文件

修改配置文件`/etc/docker/daemon.json`:

```json
{
  "registry-mirrors": ["https://yanc7cda.mirror.aliyuncs.com"]
}
```

# 重载守护进程，重启docker

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 查看加速器是否生效

输入命令 `docker info`，如果存在以下字段则表示成功：

```yaml
...
Registry Mirrors:
 https://yanc7cda.mirror.aliyuncs.com
...
```

# 更多

参考[环境搭建-Docker](/categories/环境搭建-Docker/)

# 参考资料

[【Docker】更新docker镜像源](https://cloud.tencent.com/developer/article/1335788)