---
title: 'Notepad++, Everything, FreeFileSync提高工作效率的三个免费软件神器（转载）'
categories:
  - 软件工具
tags:
  - Notepad++
  - Everything
  - FreeFileSync
  - 转载
abbrlink: 3bb670f1
cover: /images/cover/办公.jpg
date: 2020-02-20 10:35:56
---


本文转载自[Notepad++, Everything, FreeFileSync——提高工作效率的三个免费软件神器 - 知乎](https://zhuanlan.zhihu.com/p/38942108?utm_source=wechat_session&utm_medium=social&utm_oi=646731722364948480)

还在用windows自带的记事本或写字板编辑文本文档？

还在用window自带的搜索服务查找文件？

还在手动备份重要数据和资料？

如果你对以上三个问题中任何一个的回答是“是”的话，我建议你看完下面的内容。

我会简单的介绍三个十分有用的免费软件，保证能提高你的办公室工作效率，为你节约出大量的时间用来刷知乎。

注：前方多图，非wifi读者注意避让。

一、Notepad++

这是一个免费但强大的文本编辑工具，只要用过一次，你就再也不会想打开windows自带记事本或写字板了。而且整个软件十分小巧，安装包才4M多。

官方下载连链接：[Notepad++ v7.5.7 - Current Version](https://notepad-plus-plus.org/download)，下载最新版的installer，双击，无脑下一步就行。

安装完成后，应该会自动在右键菜单添加一个“edit with notepad++”选项，可以用于打开任何纯文本文档：

![右键](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-25-49.png)

打开后的界面是这样子的，支持常见的程序语言格式（包括latex)、代码高亮、自动补全等功能，建议在 设置-首选项-文件关联 中关联自己常用的文件格式：

![关联文件格式](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-26-08.png)

可以同时打开多个文件，并且ctrl+shift+s批量保存文件，拖拽文件标签可以打开双视图模式

![双视图](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-26-36.png)

支持多文件批量查找、计数、替换，并且支持正则表达式：

![批量操作](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-27-06.png)

按住alt键再拉光标，可以进行多行的列编辑（编辑规律的数据超好用有没有）：

![多行编辑](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-27-21.png)

二、Everything

这是一个文件搜索软件。比windows资源管理器自带的搜索功能上强大很多，而且搜索速度非常快，除第一次需要初始化之外，不管文件夹多大几乎每次搜索都是秒出结果。

官方下载连链接：[Downloads - voidtools](https://www.voidtools.com/downloads/) 。软件同样很小巧，installer只有1M多，下载双击后，无脑下一步就行。

安装好，运行，建议在 工具-选项-常规 里把“集成到资源管理器右键菜单”勾选并应用。

![集成右键](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-27-46.png)

这样，在任意文件夹里点击右键，会出现搜索Everything选项：

![右键](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-28-06.png)

点击后，就能在当前目录及所有子目录下搜索文件，例如输入.txt，就能搜索所有txt格式的文件，多个关键字用空格（逻辑与）或“|”字符（逻辑或）分离：

![多关键字](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-28-25.png)

支持区分大小写、匹配文件路径、正则表达式、按文件类型搜索：

![高级搜索](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-29-15.png)

支持高级搜索：

![高级搜索](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-29-37.png)

搜索处理大量文本时，配合Notepad++食用味道更佳：

![配合](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-29-57.png)

选择上图中的“复制完整路径和文件名”，可以批量输出文件路径，创建文件地址数据库，方便用python等文本处理脚本批量调用。

![批量调用](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-30-18.png)

三、FreeFileSync

自从隔壁实验室同学因为火灾把电脑烧毁导致延期毕业后，我所有的重要数据都是本地一份，同步盘随时同步一份（国内可以用 onedirve 或者 坚果云），移动硬盘每天定时同步一份。

FreeFileSync就是用于本地自动备份和同步文件的软件，大部分功能免费。

官方下载连链接：[FreeFileSync](https://freefilesync.org/download.php) 。下载运行安装后，界面如下。看起来稍微有点复杂，但只需要设置一次，保存成批处理程序以后自动执行即可：

![设置](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-32-02.png)

首先把你的所有需要备份的重要数据整理到一个文件夹内，把这个文件夹地址输入到上图左边的路径框中。在创建一个存放备份文件的文件夹（建议放在另一个硬盘，通过物理隔离保护数据），地址输入到右边的路径框。

然后点击“比较设置”，选择“文件时间和大小”，也就是根据文件的修改时间和大小，来判断文件是否和备份文件夹中的一致，从而判断是否需要更新备份。

![判断规则](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-32-22.png)


再点击“同步设置”，选择镜像。这样一来，任何对重要数据文件夹的修改都会同步到你的备份文件夹中。

![同步设置](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-32-39.png)

然后点击“另存为批处理程序”，点击另存为，得到一个xxx.ffs_batch文件。

![另存为](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-32-58.png)

以后，只要双击运行这个xxx.ffs_batch文件，程序就会自动同步并备份你的数据了。是不是很方便很省心？告诉你，还有更省心的。使用windows自带的“计划任务”功能，可以定时自动执行xxx.ffs_batch程序，从而完全不用你管自动备份你的重要数据。

用WIN10的Cortana搜索并打开“计划任务”（非WIN10系统在控制面板里搜）。点击创建基本任务：

![创建基本任务](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-33-19.png)

名称随便输一个（如Backup），点击下一步：

![触发器](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-33-40.png)

![触发事件](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-33-57.png)

设置每天自动运行的时间，建议设置成电脑一定会开机的时间段。点击下一步：

![操作](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-34-11.png)

![启动程序](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-34-23.png)

操作选择“启动程序”，路径框里选择刚才得到的xxx.ffs_batch脚本，点击下一步，完成即可。这样，系统就会每天定时备份你的文件了。

![设置完成](/images/Notepad-Everything-FreeFileSync提高工作效率的三个免费软件神器（转载）/2020-02-20-10-34-40.png)

从此，妈妈再也不用担心我的数据丢失了。

# 更多

参考[软件工具](/categories/软件工具/)