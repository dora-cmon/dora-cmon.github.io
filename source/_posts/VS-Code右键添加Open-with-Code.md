---
title: VS Code右键添加Open with Code
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - 菜单栏
abbrlink: 4c8a04c2
cover: /images/cover/VSCode.jpg
date: 2020-02-18 22:16:27
---


新建脚本`vsCodeOpenFolder.reg`输入以下内容：

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\VSCode]
@="Open with Code"
"Icon"="C:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\*\shell\VSCode\command]
@="\"C:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%1\""

Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\VSCode]
@="Open with Code"
"Icon"="C:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
@="\"C:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%V\""

Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
@="Open with Code"
"Icon"="C:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
@="\"C:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%V\""
```

替换其中相关路径为`VS Code`安装路径，双击运行即可。

# 更多

更多VS Code相关配置见[环境搭建 - VS Code](/categories/环境搭建-VS-Code/)