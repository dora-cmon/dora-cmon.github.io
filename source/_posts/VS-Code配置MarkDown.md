---
title: VS Code配置MarkDown
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - MarkDown
  - 配置
  - Hexo
abbrlink: 3ce74ebc
cover: /images/cover/markdown.jpg
date: 2020-02-19 01:27:27
---


# 安装插件

- Markdown All in One
- Markdown Preview Enhanced

![1582083185868](/images/VS-Code配置MarkDown/1582083185868.png)

# 使用

创建`.md`文件，编辑后点击右上角的预览按钮，有两种模式：

![1582083619701](/images/VS-Code配置MarkDown/1582083619701.png)

# （可选）使用Paste Image插入截图

该插件可以直接将剪贴板中的图片插入Markdown文件，并自动创建文件夹保存该图片，非常方便。

- 安装插件 Paste Image
- 默认按下 `Ctrl + Alt + v`或按`Ctrl + Shift + P`输入`Paste Image`即可将截图添加到md文档中

**相关配置**

- 图片保存位置

  图片默认保存在当前文档所在的目录，可以自定义路径，按`Ctrl+,`进入配置界面并按右上角切换到`settings.json`文件，添加以下内容（路径改成你期望的图片存放位置）：

  ```json
  /* Paste Image Config */
  "pasteImage.path": "../images/${currentFileNameWithoutExt}/",
  ```
  
  **注意**：使用`../`时一定确保回溯到的目录全部在当前工程目录以内，否则创建图片会失败！

  **由于不同工程有不同的文件保存逻辑，因此可以在工程文件目录下创建`.vscode/settings.json`文件，仅针对当前工程生效，如管理我的hexo博客的配置：**
  

  ```json
  /* Paste Image Config */
  "pasteImage.path": "${projectRoot}/source/images/${currentFileNameWithoutExt}",
  "pasteImage.basePath": "${projectRoot}/source",
  "pasteImage.forceUnixStyleSeparator": true,
  "pasteImage.prefix": "/"
  ```

- 快捷键失效

  如果按`Ctrl+Alt+V`不可用，请切换成其他快捷键。我甚至无法设置`Ctrl+Alt+V`快捷键，猜测与全局快捷键冲突。按`File -> Preferences -> Keyboard Shortcuts`：

  ![1582086079086](/images/VS-Code配置MarkDown/1582086079086.png)

  搜索 Paste Image 并修改快捷键，我修改为`Ctrl + Alt + P`：

  ![1582086134268](/images/VS-Code配置MarkDown/1582086134268.png)

# 更多

更多VS Code相关配置见[环境搭建 - VS Code](/categories/环境搭建-VS-Code/)

## 参考资料

[使用Paste Image插件来方便的给Markdown添加截图的功能_开发工具_JAVA_HIGHNESS的专栏-CSDN博客](https://blog.csdn.net/javahighness/article/details/90454136)