---
title: git命令如何指定秘钥
categories:
  - 疑难杂症
tags:
  - MobaXterm
  - ssh密钥
cover: /images/cover/git.jpg
abbrlink: bfc06ff2
date: 2020-04-03 22:33:08
---

# 保存秘钥文件

将秘钥移动到 `~/.ssh/` 目录(Window路径为 `%HOMEPATH%/.ssh/`)下，若已有秘钥，注意修改秘钥名称，如 `id_rsa_proj`

# 添加配置文件

在 `～/.ssh/` 目录下创建文件 `config`，添加以下内容：

```yml
Host project
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_rsa_proj
```

- **project**: 识别名称，自定
  - HostName: Git 服务地址
  - User: 用户
  - **IdentityFile**: 秘钥路径

# 执行 git 命令

假如仓库 ssh 地址为： `git@gitee.com:repository.git`

只需将其中的 `gitee.com` 替换为第 2 步中的 `Host` 字段即可，本例为 `project`：

  ```
  git@project:repository.git
  ```

之后正常进行 git 操作即可。

推荐使用 VS Code，其中集成的 Git 非常方便。

# 常用 git 命令

|功能|命令|
|---|---|
|克隆仓库             |`git clone [url]`|
|获取远程仓库变更      |`git pull`|
|推送本地变更至远程仓库 |`git push`|
|显示所有本地分支[创建] |`git branch [new branch]`|
|切换分支             |`git checkout [branch name]`|
|显示当前分支的版本历史  |`git log`|
|把文件添加到缓冲区     |`git add filename`|
|添加所有文件到缓冲区   | `git add .`|
|删除文件             |`git rm filename`|
|显示文件变更          |`git diff filename`|
|提交缓冲区的所有修改   |`git commit -m  "commit"`|
