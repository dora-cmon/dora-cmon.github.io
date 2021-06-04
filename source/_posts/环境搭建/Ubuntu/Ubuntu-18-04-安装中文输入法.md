---
title: Ubuntu 18.04 安装中文输入法
categories:
  - 环境搭建 - Ubuntu
tags:
  - Ubuntu
  - Linux
cover: /images/cover/ubuntu.jfif
abbrlink: 69aa783c
date: 2020-03-12 16:08:50
---

# 安装 fcitx 和搜狗拼音输入法

```bash
sudo apt install fcitx
```

[官网下载](https://pinyin.sogou.com/linux/)搜狗拼音输入法 `deb` 格式安装包，双击安装

# 安装中文字体

进入 `Settings -> Region & Language`，点击`Manage Installed Languages`：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-10-21.png)

根据提示安装语言支持：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-11-48.png)

选择 `fcitx`：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-12-45.png)

# 配置输入法

打开 `Fcitx`：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-15-06.png)

打开配置界面：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-15-50.png)

- 若已有 `Sogou Pinyin`
  
  ![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-17-23-26.png)

  继续下一步。

- 若没有 `Sogou Pinyin`

  ![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-18-21.png)

  添加输入法，取消掉 `Only Show Current Language` 复选框，搜索 `sogou pinyin`(图为我之前安装goole pinyin时的截图，因为不好用所以转搜狗输入法)：

完成后，可以调整各输入法顺序，第一个为默认输入法。

在 `Global Config` 界面根据自己的习惯设置切换输入法快捷键，这里我设置为 `left shift`：

![](/images/Ubuntu-18-04-安装中文输入法/2020-03-12-16-20-49.png)

**P.S** 首次运行，输入时可能不显示中文，稍等一会就好。

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[Ubuntu 18.04 安装中文输入法](https://baijiahao.baidu.com/s?id=1619306801356144376&wfr=spider&for=pc)