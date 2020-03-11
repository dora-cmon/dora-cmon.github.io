---
title: Hexo配置butterfly主题
cover: /images/cover/hexo_theme_butterfly.jpg
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
abbrlink: fad26ff2
date: 2020-02-16 15:00:57
---

# 准备工作

完成Hexo环境搭建，参考{% post_link 搭建Hexo博客 %}。

# 安装Butterfly主题

## 下载主题

在终端窗口下，定位到 Hexo 站点目录下。使用 `Git` checkout 代码：

```bash
cd <folder>
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly
```

如果你没有 pug 以及 stylus 的渲染器（默认是没有的），请下载安装： 

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

## 启用主题

修改站點配置文件`_config.yml`，把主題改為`Butterfly`:

```yaml
theme: Butterfly
```

为了主题的平滑升级, 把主题默认配置文件`themes/Butterfly/_config.yml`复製到 Hexo 工作目录下的`themes/source/_data/butterfly.yml`，如果`source/_data`的目录不存在那就创建一个。

如果创建了`butterfly.yml`, 它将会替换主题默认配置文件`themes/Butterfly/_config.yml`里的配置项 (不是合并而是替换), 之后就只需要通过`git pull`的方式平滑地升级 theme-butterfly了。

## 验证主题

清除 Hexo 的缓存，并启动Hexo：

```bash
hHexo clean && hexo s
```

当命令行输出中提示：

```bash
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```

即可使用浏览器访问 `http://localhost:4000`，检查站点是否正确运行。

如果出现以下错误：

```bash
Error: Cannot find module 'cheerio'
```

则运行以下命令安装`cheerio`：

```bash
npm install cheerio
```

# 主题配置

**配置文件説明**
站点配置文件`_config.yml`是Hexo工作目录下的主配置文件
`butterfly.yml`是 Butterfly 的配置文件。需要手动将主题目录下的`_config.yml`文件复製到 Hexo 工作目录的`source/_data/butterfly.yml`中。如果文件或者文件夹不存在，需要手动创建。

## 修改首页显示

### 修改导航菜单

配置`butterfly.yml`，必须是` /xxx/`，后面`||`分开，然后写图标名。菜单名称可自己修改：

```yaml
menu:
  首页: / || fa fa-home
  时间轴: /archives/ || fa fa-archive
  标籤: /tags/ || fa fa-tags
  分类: /categories/ || fa fa-folder-open
#  清单||fa fa-heartbeat:
#    - 音乐 || /music/ || fa fa-music
#    - 照片 || /Gallery/ || fa fa-picture-o
#    - 电影 || /movies/ || fa fa-film
#  友链: /link/ || fa fa-link
#  关于: /about/ || fa fa-heart
```

### 修改首页背景图片

修改`butterfly.yml`中的`index_img`为自己的图片：

```yaml
index_img: /img/top_img/index.jpg
default_top_img: /img/top_img/index.jpg
```

修改首页子标题，修改`subtitle`字段：

```
subtitle:
  sub:
    - 中文
    - English
```

### 修改标签栏小图标

替换`/themes/Butterfly/source/img/favicon.ico`文件为你的图标。

## 配置页面

### 修改侧边栏

* 设置头像

  修改`butterfly.yml`中的`avatar`为自己的图片：
  
  ```yaml
  avatar: /img/avatar.jpeg
  ```
  
* 社交按钮

  ```yaml
  social:
    fa fa-github: https://github.com/
    fa fa-envelope: mailto:your_mail@outlook.com
  #  fa fa-rss: /atom.xml
  ```
  
  不需要的注释掉即可。

* 网页已运行时间

  ```yaml
  #网页开通时间
  #格式: 月/日/年 时间
  #也可以写成 年/月/日 时间
  runtimeshow:
    enable: true
    start_date: 6/7/2018 00:00:00  
  ```

### Footer设置

- 博客年份
  配置`butterfly.yml`的`since`字段，它用来展示站点起始时间，位于页面的最底部。

  ```yaml
  since: 2018
  ```

- 页脚自定义文本
  `footer_custom_text`用来在页脚自定义文本,通常可以在这里写声明文本等。支持 HTML。

  ```yaml
  footer_custom_text: Hi, welcome to my blog!
  ```

- ICP
  对于部分有备案的域名，可以在配置`ICP`字段：

  ```yaml
  ICP:
    enable: true
    url: http://www.beian.miit.gov.cn/state/outPortal/loginPortal.action
    text: 粤ICP备xxxx
    icon: /img/icp.png
  ```

## 文章显示

### 首页摘要封面图

在目标文档的`Front-matter`添加`cover`字段，并填上要显示的图片地址。如果不配置`cover`,可以设置显示默认的封面.

配置`butterfly.yml`的`default_cover`字段：

```yaml
default_cover: https://cdn.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg.png
```

### 代码配置

取消代码语言显示，修改`butterfly.yml`中的`highlight_lang`字段：

```yaml
highlight_lang: false
```

## 夜间模式

配置`butterfly.yml`中的`display_mode`字段：

```bash
display_mode: dark
```

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[hexo-theme-butterfly安裝文檔 | JerryC](https://jerryc.me/posts/21cfbf15/)