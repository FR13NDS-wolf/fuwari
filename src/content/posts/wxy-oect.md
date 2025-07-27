---
title: OEC-Turbo服务器
published: 2025-06-29
description: '使用网心云矿渣刷入Armbian搭建自己的NAS'
image: 'https://teleimg.loophy.top/file/1753608348841_image.png'
tags: [NAS, Linux, Docker, Armbian]
category: '教程'
draft: false 
lang: ''
---

首先, 感谢为这台机器开发固件贡献代码的大佬们.

现在这台机器的刷机教程很多,我就不写了 给几个链接

什么值得买 https://post.smzdm.com/p/agwx06lm/

Bilibili视频 https://search.bilibili.com/all?keyword=oect

恩山论坛 https://www.right.com.cn/

固件的选择: 我使用的是H大修改的 Armbian 25.5.1 bookworm, 有NPU驱动

### 机器初始化

作者是linux糕手所以默认使用root登录了

1. 软件包更新 `apt update && apt upgrade`
2. 换源 `armbian-apt` 选择国内源
3. 安装常用的软件包 

`apt install curl wget vim zip unzip aria2 htop neofetch iperf3 cmake tree net-tools  traceroute python3-pip python3-venv git dnsutils arping telnet nfs-common nmap aptitude -y`

### 安装1Panel

大家也可以安装CasaOS但我不是特别喜欢, 所以使用1Panel 功能更多

官网 https://1panel.cn/docs/v2/installation/online_installation/

> `bash -c "$(curl -sSL https://resource.fit2cloud.com/1panel/package/v2/quick_start.sh)"`

### 1Panel常用功能

#### **自动证书申请**

网站 -> 证书

填入自己的 CF Token即可申请, 还能设置推送到目录

#### 数据库

在商店中可以安装docker版的 redis MariaDB

#### Docker

篇幅太多, 之后写在其他博文里
