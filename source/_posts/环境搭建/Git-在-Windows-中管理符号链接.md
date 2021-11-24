---
title: Git 在 Windows 中管理符号链接
categories:
  - 环境搭建
tags:
  - Git
  - Symbol link
  - 符号链接
cover: /images/cover/git.jpg
abbrlink: c866167c
date: 2021-11-24 10:54:27
---

# 流程

1. 下载最新 [Git for windows](https://git-scm.com/download/win)
2. 安装时选中 `Enable symbolic links`
    ![](/images/Git-在-Windows-中管理符号链接/2021-11-24-10-43-00.png)
    启用符号链接（需要SeCreateSymbolicLink权限）。注意，现有仓库不受此设置的影响。
3. 克隆仓库并启用符号链接支持
    ```bash
    git clone -c core.symlinks=true <URL>
    ```
4. 创建符号链接
    符号链接支持指向绝对/相对路径
    ```bash
    mklink /d this-link-points-to c:\that-directory
    mklink this-link-points-to c:\that-file
    ```
    目录联结只支持指向绝对路径（命令中的相对路径会被转为绝对路径）
    ```bash
    mklink /j this-link-points-to c:\that-directory
    ```
5. 赋予相关权限
    若使用符号链接，需要添加权限，详见[允许非管理员创建符号链接](##允许非管理员创建符号链接)
    完成后重启计算机

# 官方说明

## 符号链接

概述：Windows 上没有完全等效的 POSIX 符号链接，默认情况下，非管理员无法使用类似的链接，除非启用开发人员模式并使用相对较新的 Windows 10 版本。 因此，仅在检测到场景能够支持时，符号链接功能才会默认开启。 用户可以通过 `core.symlinks=true` 配置手动启用。

## 背景

从 Windows Vista 开始，添加了对符号链接的支持。但其并不是 Unix 符号链接，它们在很多方面有所不同：

- 符号链接仅在 Windows Vista 及更高版本上可用，尤其是在 XP 上不可用
- 需要 `SeCreateSymbolicLinkPrivilege` 权限，默认情况下仅分配给管理员并由 `UAC` 保护，但可以分配给其他用户或用户组（见下文）。
- 默认情况下禁用远程文件系统上的符号链接（调用 fsutil 行为查询 SymlinkEvaluation 查找）
- 符号链接仅适用于 NTFS，不适用于 FAT 或 exFAT
- Windows 的符号链接是分类型的：它们需要知道是指向目录还是​​指向文件（因此，Git 发现错误时会更新类型）
- 许多程序不支持符号链接
由于这些原因，Git for Windows 默认禁用对符号链接的支持（当遇到符号链接时它仍会读取它们）。可以通过 `core.symlinks` 配置变量启用支持，例如克隆时：

```bash
git clone -c core.symlinks=true <URL>
```

## 创建符号链接

默认情况下，Git Bash 中的 `ln -s` 命令不会创建符号链接。 相反，它会创建副本。

要创建符号链接（前提是您的帐户有相应权限），使用内置的 mklink 命令，如下所示：

```bash
mklink /d this-link-points-to c:\that-directory
mklink this-link-points-to c:\that-file
```

## 允许非管理员创建符号链接

可以使用本地策略编辑器（或在域帐户的情况下通过 Active Directory 策略）分配创建符号链接的权限。 Windows 家庭版不支持策略编辑器，但可以使用免费的 Polsedit。

- 本地组策略编辑器：启动 `gpedit.msc`，导航到 `计算机配置 - Windows 设置 - 安全设置 - 本地策略 - 用户权限分配` 并将帐户添加到 `创建符号链接` 选项中。

- 本地安全策略：启动 `secpol.msc`，导航到 `本地策略 - 用户权限分配` 并将帐户添加到 `创建符号链接` 选项中。

- Polsedit：启动 `polseditx32.exe` 或 `polseditx64.exe`（取决于您的 Windows 版本），导航到 `安全设置 - 用户权限分配` 并将帐户添加到 `创建符号链接` 选项中。

请注意，无论权限分配如何，管理员组的成员也将需要 UAC 提升。从 Windows 10 版本 1703（创作者更新）开始，启用开发者模式将禁用此限制，并允许在没有 UAC 提升的情况下创建符号链接（尽管 Git for Windows 仍然需要 UAC 提升直到 v2.13.0）。

## 创建目录联结

默认情况下，非管理员用户可以创建目录联结。可以作为符号链接的替代品。要创建目录连接，请使用带有 `/j` 参数的 `mklink` 命令：

```bash
mklink /j this-link-points-to c:\that-directory
```

# 参考资料

[Symbolic Links](https://github.com/git-for-windows/git/wiki/Symbolic-Links)