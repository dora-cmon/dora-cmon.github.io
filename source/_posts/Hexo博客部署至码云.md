---
title: Hexo博客部署至码云
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - 码云
  - 博客
cover: /images/cover/码云.jpg
abbrlink: a91be4ad
date: 2020-02-22 14:46:50
---


# 创建仓库

在码云上创建一个仓库，设置为公开或私有都可以,仓库名命名为Gitee的个人空间地址名（可以在设置 -> 个人资料 -> 个人空间地址查看）：

![个人空间地址](/images/Hexo博客部署至码云/2020-02-22-14-09-40.png)

![新建仓库](/images/Hexo博客部署至码云/2020-02-22-14-18-37.png)

部署后就可以通过`Gitee个人空间地址名.gitee.io`域名访问博客了（不带子目录）；如果仓库名设置为其它，那么访问域名将会是`个人空间地址名.gitee.io/仓库名`（带子目录）。

# 部署

## 安装插件

安装部署插件包，在博客根目录打开`Git Bash`，输入以下命令：

```bash
npm install hexo-deployer-git --save
```

## 站点配置文件
复制仓库地址，选择https（部署需要账户密码）或ssh（部署不需要密码，但需要先配置ssh key）：

![仓库地址](/images/Hexo博客部署至码云/2020-02-22-14-36-27.png)

打开站点配置文件`_config.yml`，在repo中添上仓库地址，部署分支设置为master：

```yaml
deploy:
  type: git
  repo: https://github.com/dora_cmon/dora_cmon.git #仓库地址
  branch: master #部署分支
```

如果域名带子目录，那么还需要多一步设置，在站点配置文件中找到如下配置，url设置为你的完整域名，root设置为你的子目录（若将仓库名和个人空间名设置相同，则可跳过该步）。

```yaml
# URL
url: https://github.com/dora_cmon/
root: /dora_com
```

## 部署博客

在博客根目录输入以下命令：

```bash
hexo clean && hexo g && hexo d
```

打开博客仓库页面 -> 服务 -> GiteePages服务，首次需要先启动，勾选上Https。每次博客有改动都需要在此页面手动更新一下，等待部署完成后即可打开域名访问。

![创建page](/images/Hexo博客部署至码云/2020-02-22-14-31-20.png)

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[Hexo博客部署到GitHub和码云](https://gavincrown.gitee.io/2019/09/24/Hexo%E5%8D%9A%E5%AE%A2%E9%83%A8%E7%BD%B2%E5%88%B0GitHub%E5%92%8C%E7%A0%81%E4%BA%91/)

[使用Gitee+Hexo搭建个人博客](https://www.jianshu.com/p/5014133ba61a)