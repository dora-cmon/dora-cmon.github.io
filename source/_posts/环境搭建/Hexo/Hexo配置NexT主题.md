---
title: Hexo配置NexT主题
cover: /images/cover/hexo_theme_next.jpg
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
abbrlink: 6f2f62d0
date: 2020-02-15 19:20:12
---

# 准备工作

完成Hexo环境搭建，参考{% post_link 环境搭建/Hexo/Hexo博客搭建 %}。

# 安装NexT主题

## 下载主题

在终端窗口下，定位到 Hexo 站点目录下。使用 `Git` checkout 代码：

```bash
cd folder
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

## 启用主题

打开站点配置文件`_config.yml`， 找到 `theme` 字段，并将其值更改为 `next`。

```yaml
theme: next
```

## 验证主题

清除 Hexo 的缓存：

```bash
hHexo clean
```

启动 Hexo，并开启调试模式（即加上 `--debug`）:

```bash
hexo s --debug
```

在服务启动的过程，注意观察命令行输出是否有任何异常信息， 当命令行输出中提示：

```bash
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```

即可使用浏览器访问 `http://localhost:4000`，检查站点是否正确运行。

# 主题配置

## 设置站点信息

修改站点配置文件 `_config.yml` 中的 `site` 字段：

```yaml
# Site
title: 网站标题
subtitle: 网站副标题
description: 网站描述	# 站点描述
author: 作者名字
language: zh-Hans  # 网站使用的语言 简体中文 zh-Hans
```

## 选择Scheme

目前 NexT 支持三种 Scheme：

* Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
* Mist - Muse 的紧凑版本，整洁有序的单栏外观
* Pisces - 双栏 Scheme，小家碧玉似的清新

通过更改主题配置文件 `next/_config.yml`切换Scheme，将需用启用的 scheme 前面注释 `#` 去除：

```yaml
#scheme: Muse
#scheme: Mist
scheme: Pisces
```

## 设置菜单

* **设置菜单项和图标**

  修改主题配置文件 `next/_config.yml` 中的 `menu` 字段，设置格式是：`item name: link`。其中 `item name `是一个名称，这个名称并不直接显示在页面上，它将用于匹配图标以及翻译。

  ```bash
  menu:
    home: /|| home	#主页
    #about: /about/ || user	# 关于页面
    tags: /tags/ || tags		# 标签页
    categories: /categories/|| th	# 分类页
    archives: /archives/|| archive	# 归档页
    #schedule: /schedule/ || calendar
    #sitemap: /sitemap.xml || sitemap
    #commonweal: /404/ || heartbeat
  ```

  若站点运行在子目录中，将链接前缀的 `/` 去掉。

  **`||`前不能有空格**，后接图标名称 。

* 设置菜单项显示文本

  以简体中文为例，若需要添加一个菜单项，比如 `something`。那么就需要修改简体中文对应的翻译文件 `languages/zh-Hans.yml`，在 `menu` 字段下添加一项：

  ```yaml
  menu:
    # ...
    something: 有料
  ```

## 设置侧边栏

默认情况下，侧栏仅在文章页面（拥有目录列表）时才显示，并放置于右侧位置。 可以通过修改主题配置文件 `next/_config.yml` 中的 `sidebar` 字段来控制侧栏的行为：

```yaml
sidebar:
  position: left	# left | right
  display: post		# post | always | hide | remove
```

- `post` - 默认行为，在文章页面（拥有目录列表）时显示
- `always` - 在所有页面中都显示
- `hide` - 在所有页面中都隐藏（可以手动展开）
- `remove` - 完全移除

## 设置首页文章预览

修改主题配置文件 `next/_config.yml` 中的 `sidebar` 字段控制首页文章预览：

```yaml
auto_excerpt:
  enable: true
  length: 150
```

将`enable`设置为`true`就可以开启首页文章预览了，length是显示预览的长度。在文章中使用`<!-- more -->`标志来精确控制文章摘要预览的位置。

## 设置头像

修改主题配置文件 `next/_config.yml` 中的 `avatar` 字段：

```yaml
avatar: /images/avatar.gif	# 可以是source/images目录下的文件，也可以是完整的URL
```

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[配置 | Hexo](https://hexo.io/zh-cn/docs/configuration)

[Hexo相关配置和使用](https://www.jianshu.com/p/d5d3e10576d1)

[开始使用 - NexT 使用文档](http://theme-next.iissnan.com/getting-started.html)

[优化了一下 Hexo 的Url格式 | 明月登楼Hexo博客](https://hexo.imydl.tech/archives/32043.html)