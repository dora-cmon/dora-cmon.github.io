---
title: 小米路由器青春版刷入Breed教程 - 简书（转载）
categories:
  - 折腾造作 - 刷机
tags:
  - 小米
  - 路由器
  - 刷机
abbrlink: b5628462
cover: /images/cover/电路板.jpg
date: 2020-02-20 22:13:59
---


本文转载自[小米路由器青春版刷入Breed教程 - 简书](https://www.jianshu.com/p/e4e0a5818c3d)。工具打包在[百度网盘](https://pan.baidu.com/s/1zo_7w3DD1nXRpv9S0ATsJA)

# 刷回旧版固件，开启SSH

因小米官方并未开放青春版SSH开启工具，所以必须刷回旧版固件利用BUG获取权限。

## 刷入旧版固件

自行下载小米路由器青春版的2.1.22的开发版固件，并按官方教程刷入，并进入路由器设置好密码等一系列操作。

## 登录路由管理界面

地址栏输入192.168.31.1，输入账号密码后，浏览器地址栏可以看到类似如下URL

```bash
http://192.168.31.1/cgi-bin/luci/;stok=351a9142135557c27acdeae2175381f9/web/home#router 
```
## 更改root密码

将以上地址URL中的
```bash
/web/home#router
```

改成

```bash
/api/xqsystem/set_name_password?oldPwd=当前路由的密码&newPwd=新的路由密码
```
然后查看网页的返回结果，如果返回的JSON字符串是

```json
{"code":0}
```

就已经成功的更改了root密码了。

## 启用路由器telnet登录

以同样的方式修改网址URL，把

```bash
/web/home#router
```

改为

```bash
/api/xqnetwork/set_wifi_ap?ssid=xiaomi&encryption=NONE&enctype=NONE&channel=1%3B%2Fusr%2Fsbin%2Ftelnetd
```

然后查看返回的JSON数据

```json
{"msg":"未能连接到指定WiFi(Probe timeout)","code":1616}
```

返回码有可能不同，但是这里已经可以通过telnet的方式来登录路由器了。

## 启用路由器SSH登录

下载Putty并打开，选填以下参数：连接类型：telnet；主机名称：`192.168.31.1`；打开后看到login，输入root，密码为刚修改后的新密码。

登录后依次执行下面的三条指令：

```bash
sed -i ":x;N;s/if \[.*\; then\n.*return 0\n.*fi/#tb/;b x" /etc/init.d/dropbear
/etc/init.d/dropbear start
nvram set ssh_en=1; nvram commit
```

到此SSH已完成开启。

# 备份并刷入Breed固件

## 备份原厂bin

### Putty操作

打开Putty，按以下选择或填写：主机名：`192.168.31.1`；端口号22；连接类型：ssh。如有提示点击更新或者确定。弹出窗口输入用户名：root；密码：上面设置的新密码。然后在Putty命令行窗口中输入命令

```bash
cat /proc/mtd
```

可以看到有十个固件和分区，看名字可以知道第一个mtd0固件包含已全部分区的数据（All），第二个是Bootloader。 输入命令

```bash
dd if=/dev/mtd0 of=/tmp/all.bin
```

这表示备份第一个固件到tmp文件夹的`all.bin`文件中，如有需要可按对应的方式备份其他分区数据。

### Winscp操作

打开Winscp，按以下选择或填写：文件协议：SCP；主机名：`192.168.31.1`；端口号：22；用户名：root；密码：上面设置的新密码。如有提示点击更新或者确定。进入/tmp文件夹将`all.bin`文件拷贝出来备份。到此备份操作完成。

## 下载并刷入Breed

### 下载Breed

Breed发布地址：http://www.right.com.cn/forum/thread-161906-1-1.html，进入作者提供的下载地址 https://breed.hackpascal.net，选择`breed-mt7688-reset38.bin`和`md5sum.txt`
下载。（注意：下载后一定要校验bin文件的MD5值，将校验结果与`md5sum.txt`文件中对应bin文件的MD5值比对，如MD5值不符，重新下载并校验）。将`breed-mt7688-reset38.bin`改名为`breed.bin`，方便后续操作。

### 刷入Breed

通过WinScp把`breed.bin`传入到`/tmp`中。然后使用Putty，输入命令

```bash
mtd -r write /tmp/breed.bin Bootloader
```

将breed刷入bootloader，刷入成功后按提示重启路由器（注意：breed作者并不推荐此方法）。到此breed固件刷入完毕。

# Breed的使用与固件推荐

## 进入breed控制台

拔掉路由器电源，使路由关机，用取卡针或者其他尖锐物戳着reset键，然后插上电源，待路由器后方的网络接口灯闪烁时松开reset键即可，然后用一条网线把电脑和路由器的WAN口相连，打开浏览器访问`192.168.1.1`，即可进入breed控制台。进入后即可开始对路由器进行刷机。

## 路由器固件推荐
推荐使用Padavan固件，固件发布地址：http://www.right.com.cn/forum/thread-161324-1-1.html（注意：小米路由器青春版请选择MI-NANO专版，目前最新版固件名称如下：`MI-NANO_3.4.3.9-099.trx`）。

# 更多

参考[折腾造作-刷机](/categories/折腾造作-刷机/)

# 参考资料
[小米路由器青春版刷入Breed教程 - 简书](https://www.jianshu.com/p/e4e0a5818c3d)
[小米路由器青春版刷华硕固件 &#8211; 设计笔记](http://biji.io/2017/4963.html)