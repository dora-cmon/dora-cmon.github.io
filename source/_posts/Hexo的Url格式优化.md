---
title: Hexo的Url格式优化
cover: /images/cover/url.jpg
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - Url格式
abbrlink: 9e1df0ba
date: 2020-02-16 21:36:05
---


Hexo默认根据配置文件`_config.yml`里的`permalink: :year/:month/:day/:title/`生成链接，这种url链接里有年月日分隔符，且有中文出现，会造成很多问题。

使用 Hexo-abbrlink 插件解决该问题：

```bash
sudo cnpm install hexo-abbrlink --save
```

修改`_config.yml`中的`permalink`字段：

```yaml
permalink: posts/:abbrlink/
```

在`_config.yml`末尾添加以下字段：

```yaml
# abbrlink config
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

清除缓存，重新生成并启动：

```bash
hexo clean && hexo g
hexo s
```

成功后，网页Url效果如下：

```
http://127.0.0.1:4000/posts/c1d5d64c/
```

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)