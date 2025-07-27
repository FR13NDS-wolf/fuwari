---
title: Github配置
published: 2025-07-12
description: 'ssh以及git的一些基础用法'
image: 'https://teleimg.loophy.top/file/1753596508371_20250727140821767.png'
tags: [Github, Git, SSH]
category: '教程'
draft: false 
lang: ''
---

## Github SSH

### 生成密钥对

`ssh-keygen -t rsa -b 4096 -f ~/.ssh/github_key`

会得到两个文件, 公钥 (`.pub`) 和私钥 

`cat github_key.pub` 显示公钥

在GitHub[设置页](https://github.com/settings/ssh/new)添加上刚刚生成的公钥内容

![image-20250727下午12246943](https://teleimg.loophy.top/file/1753593772689_image-20250727下午12246943.png)

### 验证SSH

为了方便, 需要配置一下 `.ssh/config` 指定连接Github时所使用的私钥

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_key 
  IdentitiesOnly yes 
```

终端执行 `ssh -T git@github.com` 

> Hi FR13NDS-wolf! You've successfully authenticated, but GitHub does not provide shell access.

即表示成功!

## Git入门

### 本地创建文件夹并上传到新仓库

* `mkdir dir_name`
* `git init` 
* `vim readme.md` #新增一些文件
* `git add .`       #暂存所有变化
* `git commit -m "update"`
* `git branch -M main`

### SSH连接到远程仓库

* `git remote add origin git@github.com:FR13NDS-wolf/ssh-test.git`
* `git remote -v`   验证连接
* `git push -u origin main`  推送

### 创建分支

* `git branch dev`	
* `git switch dev` 切换分支
* `git push -u origin dev` -u参数设置跟踪
* `git push` 将main分支复制给dev

### 合并分支

* `git switch main` 切换到main
* `git pull origin main` 从仓库拉取
* `git merge dev` 从dev合并
* `git push origin main` 推送

### 删除分支

* `git branch -d dev` 删除本地dev分支
* `git push origin --delete dev` 删除远程dev分支

### 切换主分支

`setting -> default branch -> modify it `

### 设置 `.gitignore`

基础配置

```shell
# 忽略所有 .log 文件
*.log

# 忽略 build 目录及其所有内容
build/

# 忽略 node_modules 目录（常见于 Node.js 项目）
node_modules/

# 忽略操作系统生成的文件
.DS_Store # macOS
Thumbs.db # Windows

# 忽略 IDE 配置文件
.idea/      # IntelliJ IDEA
.vscode/    # VS Code
*.iml       # IntelliJ IDEA 模块文件

# 忽略临时文件
*~          # Emacs 临时文件
*.tmp
*.bak

# 忽略编译生成的文件
*.o         # 对象文件
*.exe       # 可执行文件
*.dll       # 动态链接库
*.class     # Java 编译文件

# 忽略敏感信息（例如环境变量文件）
#.env
config.local.php

# 忽略特定文件，即使它们在子目录中
mysecretfile.txt

# 忽略 logs 目录下的所有文件，但重新包含 important.log
logs/*
!logs/important.log
```

