---
title: Hexo跨平台同步
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - 博客
cover: /images/cover/Hexo.jpg
abbrlink: 454ba26
date: 2020-03-12 14:45:34
---



由于使用 Windows 和 Linux 双系统，在加上想令办公电脑和家用电脑均可维护博客，因此需要在不同电脑、不同平台之间同步 Hexo。尝试过直接使用 Onedrive, Dropbox 等云同步平台，但是文件夹中的插件（`npm 下载的包`）在不同电脑、不同平台并不兼容，因此考虑使用 Git 实现多端同步。

# 创建分支

## 创建同步分支

在 Github/Gitee 网页端进入 Hexo 的部署仓库，当前只有 `master` 分支。在 `branch` 界面创建新的分支，分支名自定，我设置为 `sync`。

## 设置默认分支

将 `sync` 分支设置为默认分支，在拉取仓库时即可直接拉取该分支。
- `sync` 分支用于存放博客的源文件
- `master` 分支用于存放生成的静态网页

# 上传源文件

## 创建本地分支

在本地创建目录 `～/Document/blog` 并进入该文件夹，克隆刚才创建的仓库（分支）：

```bash
git clone git@gitee.com:your_name/your_repository.git
```

## 复制源文件

在克隆好的仓库中，输入 `git branch` 查看当前分支，已经是 `sync` 分支了。将除 `.git` 以外的所有文件及文件夹删掉。

然后将除`.deploy_git, node_modules/, public/` 外博客源文件全部复制过来。

修改 `.gitignore`，如果没有就创建一个，输入以下内容，在 `git push` 时自动忽略：

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

注意，因为 `git` 不能嵌套上传，如果在 `theme` 中有克隆过的主题文件，需要将主题文件夹中的 `.git` 文件夹删掉。

## push 源文件

```bash
git add .
git commit -m "add branch"
git push
```

# 安装相关依赖

## 安装 Node.js

* Windows

  * 下载[Node.js](https://nodejs.org/en/)并安装

* Linux

  ```bash
  sudo apt-get install nodejs
  sudo apt-get install npm
  ```

检查是否安装成功：

```bash
node -v
npm -v
```

**安装cnpm**

cnpm是淘宝团队做的国内镜像，因为npm的服务器位于国外可能会影响安装。淘宝镜像与官方同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

```bash
sudo npm install cnpm -g --registry=https://registry.npm.taobao.org
```
## 安装Hexo

```bash
sudo cnpm install -g hexo-cli
```

检查是否安装成功：

```bash
hexo -v
```

## 安装插件

```bash
cnpm install --save

```

# 部署博客

```bash
hexo clean && hexo g && hexo d
```

# 同步源文件

- 拉取远程分支

    ```bash
    git fetch
    git merge
    ```

- 编辑博客后，推送分支

    ```
    git add .
    git commit –m "your commit"
    git push 
    ```

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[Hexo多台电脑同步](https://www.cnblogs.com/shuofxz/p/11736825.html)