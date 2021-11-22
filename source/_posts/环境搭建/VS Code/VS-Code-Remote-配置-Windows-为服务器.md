---
title: VS Code Remote 配置 Windows 为服务器
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - Remote Developement
cover: /images/cover/VSCode.jpg
abbrlink: 695abada
date: 2021-10-19 17:19:45
sticky: 3
---


VS Code 远程开发非常方便实用，以下介绍将 Windows 作为 VS Code 远程服务器的方法，完成配置后可以使用其他机器连接到该 Windows 机器进行远程开发。

# 在 Windows 服务器上安装 SSH Server

## 安装 `OpenSSH Server`

以管理员身份运行 `Power Shell`，执行命令：

```bash
Get-WindowsCapability -Online -Name Open*
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

第一条命令查询可安装的 `OpenSSH` 组件，输出为：

```bash
Name         : OpenSSH.Client~~~~0.0.1.0
State        : Installed
DisplayName  : OpenSSH 客户端
Description  : 基于 OpenSSH 的安全外壳(SSH)客户端，可用于安全密钥管理和远程计算机访问。
DownloadSize : 1314377
InstallSize  : 10602592

Name         : OpenSSH.Server~~~~0.0.1.0
State        : NotPresent
DisplayName  : OpenSSH 服务器
Description  : 基于 OpenSSH 的安全外壳(SSH)服务器，可用于安全密钥管理和远程计算机访问。
DownloadSize : 1290075
InstallSize  : 9894430
```

第二条命令安装 `OpenSSH 服务器`，输出为：

```bash
Path          :
Online        : True
RestartNeeded : False
```

验证 `SSH 服务` 运行状态：

```bash
Get-Service sshd,ssh-agent
```

输出为：

```bash
Status   Name               DisplayName
------   ----               -----------
Stopped  ssh-agent          OpenSSH Authentication Agent
Stopped  sshd               OpenSSH SSH Server
```

代表已安装 `OpenSSH Agent` 和 `OpenSSH Server`，但未启用。

## 设置 `OpenSSH 服务` 开机自启

```bash
Set-Service -Name sshd -StartupType Automatic 
Set-Service -Name ssh-agent -StartupType Automatic 
Start-Service sshd 
Start-Service ssh-agent
```

再次验证 `SSH 服务` 运行状态：

```bash
Get-Service sshd,ssh-agent
```

输出为：

```bash
Status   Name               DisplayName
------   ----               -----------
Running  ssh-agent          OpenSSH Authentication Agent
Running  sshd               OpenSSH SSH Server
```

代表 `OpenSSH Agent` 和 `OpenSSH Server` 已启用。

## 尝试连接本机 `ssh`

```bash
ssh <user-name>@cn-zz-bgp-5.natfrp.cloud -p38900
```

其中，`user-name` 为 Windows 登录用户名，若为微软在线账户，在本地查看 `C:\Users\<user-name>` 路径（通常为在线账户的部分前缀）。

# 添加可信主机密钥

完成以上步骤后，每次登录都需要输入密码，我们可以将常用主机的 `RSA` 密钥添加到 `Windows` 服务器主机中，防止每次输入密码。


## 生成密钥

在要进行远程连接的机器上执行以下命令，确认项直接回车确认：

```bash
ssh-keygen -t rsa
```

拷贝文件 `C:\Users\<user-name>\.ssh\id_rsa.pub` 中的公钥，其中 `user-name` 是你的用户名。

## 在 `Windows 服务器` 中添加可信公钥

登录 `Windows 服务器`，进行以下操作：

### 若登录用户不属于管理员组

进入目录 `C:\Users\<user-name>\`，若 `.ssh` 目录不存在，则创建该目录；

进入该目录，新建文件 `authorized_keys` 在其中添加第 [1 小节](#1-生成密钥) 中目标机器的公钥；

如需添加多台机器公钥，回车分割每行一个即可。

### 若登录用户属于管理员组

进入目录 `C:\ProgramData\ssh\`；

进入该目录，新建文件 `administrators_authorized_keys` 在其中添加第 [1 小节](#1-生成密钥) 中目标机器的公钥；

如需添加多台机器公钥，回车分割每行一个即可。

执行以下 `Power Shell` 脚本，为该文件赋予合适的权限：

```bash
$acl = Get-Acl C:\ProgramData\ssh\administrators_authorized_keys
$acl.SetAccessRuleProtection($true, $false)
$administratorsRule = New-Object system.security.accesscontrol.filesystemaccessrule("Administrators","FullControl","Allow")
$systemRule = New-Object system.security.accesscontrol.filesystemaccessrule("SYSTEM","FullControl","Allow")
$acl.SetAccessRule($administratorsRule)
$acl.SetAccessRule($systemRule)
$acl | Set-Acl
```

## 重启机器

重启机器，尝试从远程机器连接到 `Windows VS Code`，无需输入密码即可连接。

# 参考资料

[VSCode success: manage Windows Server 2019 (Core) remotely](https://medium.com/@bigoudi/vscode-success-manage-windows-server-2019-core-remotely-64167b9edf3f)

[Key-based Authentication for OpenSSH on Windows](https://www.concurrency.com/blog/may-2019/key-based-authentication-for-openssh-on-windows)