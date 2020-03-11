---
title: Hexo使用katex渲染公式
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - 博客
  - katex公式
cover: /images/cover/katex.jfif
abbrlink: 95c5c5d2
date: 2020-02-18 17:14:05
---


首先禁用MathJax（如果你配置过 MathJax 的话），然后修改`butterfly.yml`（若使用其他主题则修改对应`_config.yml`文件），以便加载katex.min.css:

```yaml
katex:
  enable: true
  # true 表示每一页都加载katex.js
  # false 需要时加载，须在使用的Markdown Front-matter 加上 katex: true
  per_page: false
  hide_scrollbar: true
```


由于不再需要添加`katex.min.js`来渲染数学方程。相应的需要卸载之前 hexo 的 markdown 渲染器以及`hexo-math`，然后安装新的`hexo-renderer-markdown-it-plus`:

```bash
# 替换 `hexo-renderer-kramed` 或者 `hexo-renderer-marked` 等hexo的markdown渲染器
# 可以在你的package.json里找到hexo的markdwon渲染器，并将其卸载
npm un hexo-renderer-marked --save
# or
npm un hexo-renderer-kramed --save
# 卸载 `hexo-math`
npm un hexo-math --save

# 然后安装 `hexo-renderer-markdown-it-plus`
npm i @upupming/hexo-renderer-markdown-it-plus --save
```

可以通过 `@neilsustc/markdown-it-katex`控制 KaTeX 的设置，所有可配置的选项参见 https://katex.org/docs/options.html。 如想要禁用掉 KaTeX 在命令行上输出的宂长的警告信息，你可以在根目录的 _config.yml 中使用下面的配置将 strict 设置为 false:

```ymal
markdown_it_plus:
  plugins:
    - plugin:
      name: '@neilsustc/markdown-it-katex'
      enable: true
      options:
        strict: false
```


当然，你还可以利用这个特性来定义一些自己常用的 `macros`。

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[hexo-theme-butterfly安裝文檔 | JerryC](https://jerryc.me/posts/21cfbf15/#KaTeX)