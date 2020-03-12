---
title: Hexo博客搭建
cover: /images/cover/Hexo.jpg
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - 博客
abbrlink: b8f4bd70
date: 2020-02-14 16:28:18
---

# Hexo简介

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

**官方文档**：https://hexo.io/zh-cn/docs/index.html

# 环境依赖

* [Node.js](http://nodejs.org/) (Node.js 版本需不低于 8.10，建议使用 Node.js 10.0 及以上版本)
* [Git](http://git-scm.com/)

# 安装

## 安装Git

* Windows

  下载[Git](https://git-scm.com/downloads)并安装

* Linux

  ```bash
  sudo apt-get update
  sudo apt-get install git-core
  ```

验证安装：

```bash
git --version
```

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

# 创建博客

**以后命令皆在`Git Bash`中运行，部分命令需访问github，因此可能因网络原因失败。**

创建博客目录（其中`folder`是博客工程目录）：

```bash
hexo init folder
cd folder
cnpm install
```

启动服务：

```bash
hexo g	# 生成
hexo s	# 启动
```

在浏览器输入`localhost:4000`即可看到生成的博客（**若为云服务器，必须在控制台添加防火墙规则**）。

## 创建分类页面

执行命令，创建分类页面：

```bash
hexo new page categories
```

得到以下提示：

```bash
INFO  Created: ~/blog/source/categories/index.md
```

修改该`index.md`文件，添加`type: "categories"`：

```yaml
---
title: categories
date: 2020-02-15 19:36:46
type: "categories"
---
```

打开`scaffolds/post.md`文件，添加`categories`:

```yaml
---
title: {{ title }}
date: {{ date }}
categories:
tags:
---
```

## 创建标签页面

执行命令，创建分类页面：

```bash
hexo new page tags
```

得到以下提示：

```bash
INFO  Created: ~/blog/source/tags/index.md
```

修改该`index.md`文件，添加`type: "tags"`：

```yaml
---
title: tags
date: 2020-02-15 19:41:04
type: "tags"
---
```

## 新建文章

执行以下命令创建一篇新文章：

```bash
hexo new title
```

系统会在`source/_posts/`文件夹下生成一个Markdown文件：

```bash
INFO  Created: ~/blog/source/_posts/title.md
```

打开该文件，内容如下：

```markdown
---
title: title
date: 2020-02-15 19:20:12
categories:
tags:
---
```

在这之后撰写自己的文章即可，也可以将已经写好的文章添加以上字段后粘贴至`source/_posts/`文件夹下。

## 给文章添加"categories"和"tags"属性

```yaml
---
title: 搭建Hexo博客
date: 2017-05-26 12:12:57
categories: 
- 环境搭建
  - Hexo
tags:
- Hexo
- 博客
---
```

**注意**：Hexo一篇文章只能属于一个分类。如果在“- 环境搭建”下方添加“- Hexo”，Hexo不会产生两个分类，而是把分类嵌套（即该文章属于 “- 环境搭建”下的 “- Hexo ”分类）。

# 指令

*  init

  ```bash
  hexo init [folder]
  ```

  新建一个网站。如果没有设置 `folder` ，Hexo 默认在目前的文件夹建立网站。

*  new

  ```bash
  hexo new [layout] title
  ```

  新建一篇文章。如果没有设置 `layout` 的话，默认使用站点配置文件 `_config.yml`中的 `default_layout` 参数代替。

  如果标题包含空格，请使用引号括起来。
  
  ```bash
hexo new "post title with whitespace"
  ```
  
  - `-p`, `--path`  自定义新文章的路径  
  - `-r`, `--replace`  如果存在同名文章，将其替换 
  - `-s`, `--slug`  文章的 Slug，作为新文章的文件名和发布后的 URL

*  generate

   ```bash
   hexo generate
   ```

   生成静态文件。

   - `-d`, `--deploy`  文件生成后立即部署网站  
   - `-w`, `--watch`  监视文件变动  
   - `-b`, `--bail`  生成过程中如果发生任何未处理的异常则抛出异常  
   - `-f`, `--force`  强制重新生成文件
   - `-c`, `--concurrency`  最大同时生成文件的数量，默认无限制

   该命令可以简写为

     ```bash
   hexo g
     ```

* publish

  ```bash
  hexo publish [layout] filename
  ```

  发表草稿。

* server

  ```bash
  hexo server
  ```

  启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。

  - `-p`, `--port`  重设端口  
  - `-s`, `--static`  只使用静态文件 
  -  `-l`, `--log`  启动日记记录，使用覆盖记录格式

* deploy

  ```bash
  hexo deploy
  ```

  部署网站。

  - `-g`, `--generate`  部署之前预先生成静态文件

  该命令可以简写为：

  ```bash
  hexo d
  ```

* render

  ```bash
  hexo render file1 [file2] ...
  ```

  渲染文件。

  - `-o`, `--output`  设置输出路径

* migrate

  ```bash
  hexo migrate type
  ```

   从其他博客系统 [迁移内容](https://hexo.io/zh-cn/docs/migration)。

* clean

  ```bash
  hexo clean
  ```

  清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。

  在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

* list

  ```bash
  hexo list type
  ```

  列出网站资料。

* version

  ```bash
  hexo version
  ```

  显示 Hexo 版本。

* 安全模式

  ```bash
  hexo --safe
  ```

  在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。

* 调试模式

  ```bash
  h --debug
  ```

  在终端中显示调试信息并记录到 `debug.log`。

* 简洁模式

  ```bash
  hexo --silent
  ```

  隐藏终端信息。

* 显示草稿

  ```bash
  hexo --draft
  ```

  显示 `source/_drafts` 文件夹中的草稿文章。

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[文档 | Hexo](https://hexo.io/zh-cn/docs/)

[建站 | Hexo](https://hexo.io/zh-cn/docs/setup)

[搭建基于Hexo的个人博客](https://gavincrown.gitee.io/2019/09/18/搭建基于Hexo的个人博客/)

[hexo史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029)

[在windows下安装hexo - 简书](https://www.jianshu.com/p/343934573342)

[npm 和 cnpm](https://www.jianshu.com/p/115594f64b41)

