---
title: 编译安装fastfetch
published: 2025-07-05
description: '代替neofetch,它主要用 C 语言编写，运行速度比Neofetch快得多，资源占用更低'
image: 'https://teleimg.loophy.top/file/1753597152884_20250727141906583.png'
tags: [Linux, Tools, Fastfetch]
category: '教程'
draft: false 
lang: ''
---

## 编译安装

1. *克隆仓库：*    `git clone https://github.com/fastfetch-cli/fastfetch.git`
2. *进入项目目录：*  `cd fastfetch`
3. *编译*：
   - 创建build 目录：`mkdir build`
   - 进入build 目录：`cd build`
   - 使用CMake：`cmake ..`
   - 使用Make：`make`
   - 安装：`sudo make install`

Usage:

`fastfetch -C all` 显示所有配置

## 配置文件

`fastfetch --gen-config` 

```json
{
  "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
  "logo": {
    "type": "auto",        // Logo type: auto, builtin, small, file, etc.
    "source": "arch",      // Built-in logo name or file path
    "width": 65,           // Width in characters (for image logos)
    "height": 35,          // Height in characters (for image logos)
    "padding": {
        "top": 0,          // Top padding
        "left": 0,         // Left padding
        "right": 2         // Right padding
    },
    "color": {             // Override logo colors
        "1": "",
        "2": ""
    }
	},
  "modules": [
    "title",
    "separator",
    "os",
    "host",
    "kernel",
    "uptime",
    "packages",
    "shell",
    "display",
    "de",
    "wm",
    "wmtheme",
    "theme",
    "icons",
    "font",
    "cursor",
    "terminal",
    "terminalfont",
    "cpu",
    "gpu",
    "memory",
    "swap",
    "disk",
    "localip",
    "battery",
    "poweradapter",
    "locale",
    "break",
    "colors"
  ]
}
```

