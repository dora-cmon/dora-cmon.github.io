---
title: Win10+Ubuntu双系统搭建
categories:
  - 环境搭建 - Ubuntu
tags:
  - 双系统
  - Ubuntu
  - Linux
cover: /images/cover/双系统.jpg
abbrlink: ea70757b
date: 2020-02-23 22:50:13
---


# 准备工作：

**环境**：已有Win10系统，一块SSD，分割约128G安装Ubuntu 18.0系统。

**工具**：

- [Ubuntu 18.0 LTS ](https://www.ubuntu.com/download/desktop)
- [UltraISO](https://cn.ultraiso.net/xiazai.html) 

# 分割硬盘

在此电脑右键，打开 管理->磁盘管理

![磁盘管理](/images/Win10-Ubuntu双系统搭建/磁盘管理.png)

磁盘0是要安装双系统的磁盘，**恢复分区**用于Win10还原，**EFI分区**为Win10引导分区，均保留。

从C盘压缩128G空间，对C盘->右键->压缩卷，等待系统查询可压缩空间。这时，很有可能出现可压缩空间远小于实际空闲空间的情况，这是因为文件存储并不是连续的，而压缩空间必须是一段连续的空间。如果出现这种情况，尝试以下解决方案：

- **法一**：C盘右键->属性->工具->优化->选择C盘并优化(该方法针对**机械硬盘**)

  ![系统碎片整理](/images/Win10-Ubuntu双系统搭建/系统碎片整理.png)
  
  若为机械硬盘，磁盘整理后应当可以压缩出接近空闲空间的大小。

- **法二**：若磁盘为 SSD 或上述操作无效，可尝试使用 Disk Genius 软件执行压缩卷操作，进入软件后选定C盘，右键->调整分区大小->输入合适的大小->开始->调整成功后保存更改。

  使用该方法可能提示错误：

  ![DG失败](/images/Win10-Ubuntu双系统搭建/DG失败.png)

- **法三**：使用 Smart Defrag 整理空闲空间，选择C盘->智能磁盘整理->可用空间碎片整理。这一步操作比较耗时！

  ![可用空间碎片整理](/images/Win10-Ubuntu双系统搭建/可用空间碎片整理.png)

  下面是整理后的磁盘文件分布：

  ![碎片整理完成](/images/Win10-Ubuntu双系统搭建/碎片整理完成.png)

  完成磁盘可用空间整理后，重新进入C盘->右键->压缩卷，可压缩空间基本是全部可用空间了。

  输入合适的大小，执行压缩卷操作：

  ![压缩卷](/images/Win10-Ubuntu双系统搭建/压缩卷.png)

  可将最后829MB的分区格式化后合并到新压缩出来的卷中，但不推荐这钟做法。若系统磁盘管理无法删除该分区，可使用Disk Genius删除，选中该分区右键->删除当前分区->保存更改，**注意别删错分区**。

# 安装Ubuntu 18.0系统

## 制作启动U盘

使用[UltraISO](https://cn.ultraiso.net/xiazai.html)打开**Ubuntu 18.0**镜像文件，启动->写入硬盘映像->选择U盘（**注意不要选错**）->写入(**此举会丢失U盘中所有文件，做好备份**)

![写入硬盘映像](/images/Win10-Ubuntu双系统搭建/写入硬盘映像.png)

## 可能需要的操作

关闭Win10快速启动和Security Boot：

- 我安装时并未关闭Win10快速启动，故此步可忽略，若出现问题考虑是否该步造成。
- Security Boot原本就是关闭的（可在BIOS中关闭，方法自寻），故不确定是否必须。

## 安装Ubuntu系统

### 修改BIOS为UEFI ONLY

重新启动电脑，进入BIOS->Startup->设置UEFI Only（不同电脑BIOS可能不同）->按`F10`保存退出：

![UEFI_ONLY](/images/Win10-Ubuntu双系统搭建/UEFI_ONLY.jpg)

**注意：**如果有`Boot device List F12 Option`选项则设置为`Enabled`。

### 安装Ubuntu

在计算机启动过程中按F12，选择从U盘启动（USB HDD）：

![启动选项](/images/Win10-Ubuntu双系统搭建/启动选项.jpg)

**建议：**安装过程不要联网，不要选安装更新/第三方软件，因为在更换源之前更新下载速度会极慢，可在系统安装好并更改源后再更新。

进入U盘后选择`Install Ubuntu`，出现选项则默认继续：

![Install_Ubuntu](/images/Win10-Ubuntu双系统搭建/Install_Ubuntu.jpg)

在安装类型时选`安装Ubunt，与Windows Boot Manager共存`：

![安装类型](/images/Win10-Ubuntu双系统搭建/安装类型.jpg)

点击现在安装后，提示将写入磁盘改动，选择继续：

![写入磁盘改动](/images/Win10-Ubuntu双系统搭建/写入磁盘改动.jpg)

选择时区，一般默认即可（若不为上海/北京，则手动选择）：

![选择时区](/images/Win10-Ubuntu双系统搭建/选择时区.jpg)

接着系统就开始自动安装，Ubuntu 18.0不需要手动设置各分区大小，旧版本可能需要。若安装旧版，可参考博客[亲测!UEFI启动模式下，电脑安装win10和Ubuntu双系统](https://blog.csdn.net/jky95624/article/details/78785755/)进行手动分区。

安装完成后重启即可进入Ubuntu的双系统引导界面，第一个即为启动Ubuntu，第三个为启动Windows。

![启动Ubuntu](/images/Win10-Ubuntu双系统搭建/启动Ubuntu.jpg)

## 设置默认启动系统

分三种情况：①默认Ubuntu；②默认Win10；。

### 默认进入Ubuntu系统

进入BIOS，设置`ubuntu`为第一项，`Windows Boot Manager`为第二项即可（**图片非该情况**）：

![引导顺序](/images/Win10-Ubuntu双系统搭建/引导顺序.jpg)

### 默认进入Win10系统

这里有两种设置方式（个人喜欢第二种）：

- **直接进入Win10引导，进入Ubuntu需按`F12`再选择系统：**

  进入BIOS，修改引导顺序，设置`Windows Boot Manager`为第一项，`ubuntu`为第二项；

  电脑在启动时会自动进入Win10系统，如果想要进入Ubuntu系统，则在启动时按`F12`选择ubuntu系统即可。

- **进入Ubuntu引导，默认选择Windows Boot Manager，进入Ubuntu只需在引导界面按上下键选择即可：**

  进入BIOS，设置`ubuntu`为第一项，`Windows Boot Manager`为第二项；

  进入Ubuntu系统，编辑`/etc/default/grub`文件：

  ```shell
  sudo vi /etc/default/grub
  # 修改该项
  GRUB_DEFAULT=2		# 更改数字设置默认启动项(0表示第一个，以此类推)
  ```

  更新引导（`update-grub`和`update-grub2`实际上完全相同）：

  ```shell
  sudo update-grub
  ```

  这样，电脑在启动时在Ubuntu引导界面停留，若无操作则进入Windows，否则可通过上下键选择系统。

# 更多

更多 Ubuntu 相关见[折腾造作 - Ubuntu](/categories/折腾造作-Ubuntu/)

# 参考资料

[Ubuntu中update-grub2与update-grub的区别](https://www.cnblogs.com/EasonJim/p/7471650.html)

[ubuntu更改启动顺序](https://www.cnblogs.com/hb91/p/5809710.html)
 

