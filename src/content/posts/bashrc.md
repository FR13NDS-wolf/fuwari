---
title: 配置bash zsh
published: 2025-05-20
description: '终端命令处理器'
image: 'https://teleimg.loophy.top/file/1753599209123_ohmyzsh-logo-ansi.png'
tags: [Linux, Bash, Zsh, Tools]
category: '教程'
draft: false 
lang: ''
---

## 常用的配置

### 环境变量

```shell
export PATH=$PATH:'/root/frp'
PATH="/Users/loophy/perl5/bin${PATH:+:${PATH}}"; export PATH;
```

### 别名

```shell
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

alias proxy='export all_proxy=socks5://10.2.0.5:7890'
alias unproxy='unset all_proxy'
```

### 生效配置

`source ~/.bashrc`

### Oh-my-zsh

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

### ZIM

`curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh`
