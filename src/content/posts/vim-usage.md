---
title: vim基础使用
published: 2025-06-15
description: '记录下基本的使用'
image: 'https://teleimg.loophy.top/file/1753597859539_image.png'
tags: [Vim, Linux, Tools]
category: '教程'
draft: false 
lang: ''
---

## 基本操作

* `a` 光标前面插入
* `i` 光标后面插入
* `o` 换行插入
* `yy` 复制行
* `p` 粘贴
* `dd` 删除行
* `u` 撤销
* `ctrl+r` 重做
* `:w :q :wq :q!`
* `h j k l` move cursor

## 进阶操作

`10yy - p` 复制下面10行

`10dd` 删除下面10行

`/xxx - n N` 查找文本位置 next 

`:!command` 在vim中执行shell命令

**打开多个文件**

`vim app.py dht11.py pump.py`

`:sp` 水平显示

`:vsp` 垂直

`:bn` next缓冲区

`:bp` past缓冲区

`:ls` list all file(buffers)

`:b<number>` 切换文件

`:b file(TAB)` 文件名切换

*切换窗口*

快捷键：Ctrl + w 然后按方向键（h、j、k、l）
窗口编号：Ctrl + w <number>
命令行：:wincmd w 或 :<number>wincmd w

## 配置文件

`vim ~/.vimrc`

```shell
set encoding=utf-8
set fileencoding=utf-8
set fileencodings=utf-8,gbk,latin1
```

