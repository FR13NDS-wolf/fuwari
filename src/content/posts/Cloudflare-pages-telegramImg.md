---
title: Telegram图床搭建
published: 2025-07-13
description: '部署到CloudflarePages上的图床'
image: 'https://teleimg.loophy.top/file/1753604042666_20250727161353774.png'
tags: [Cloudflare, Pages, Telegram]
category: '教程'
draft: false 
lang: ''
---

## 对比

**Telegraph-Image**：[https://github.com/cf-pages/Telegraph-Image](https://link.zhihu.com/?target=https%3A//github.com/cf-pages/Telegraph-Image) 功能较为完善。

**telegraph-Imag**e：[https://github.com/x-dr/telegraph-Image](https://link.zhihu.com/?target=https%3A//github.com/x-dr/telegraph-Image)

**tgState**：[https://github.com/csznet/tgState](https://link.zhihu.com/?target=https%3A//github.com/csznet/tgState)

**Telegraph-Image-Hosting：**[https://github.com/missuo/Telegraph-Image-Hosting](https://link.zhihu.com/?target=https%3A//github.com/missuo/Telegraph-Image-Hosting)

**Cloudflare Image Hosting**：[https://github.com/ifyour/cf-image-hosting](https://link.zhihu.com/?target=https%3A//github.com/ifyour/cf-image-hosting) 可以称之为Telegraph-Image-Hosting的 Cloudflare Workers版本

上面这些我基本多用过但都不太好用, 不过最近开源了一个比较新的, 还有完善的文档因此我转移到了这里.

官方文档: https://cfbed.sanyue.de/deployment/cloudflare.html

## Let's go

1. Fork项目地址 https://github.com/MarSeventh/CloudFlare-ImgBed
2. CloudflarePages使用该的Git项目

### 配置项目设置

| 配置项       | 值                              | 说明                      |
| :----------- | :------------------------------ | :------------------------ |
| 项目名称     | `cloudflare-imgbed`（或自定义） | 项目标识符                |
| 生产分支     | `main`                          | 生产环境分支              |
| 构建命令     | `npm install`                   | **重要：v2.0 新构建命令** |
| 构建输出目录 | `/`                             | 保持默认                  |

### 创建 KV 命名空间

KV 数据库用于存储文件元数据，是必需的组件。

1. 在 Cloudflare Dashboard 中选择 "Workers & Pages"
2. 点击 "KV"
3. 点击 "创建命名空间"
4. 输入命名空间名称：`img_url`（建议使用此名称）
5. 点击 "添加”

### 绑定 KV 到项目

1. 返回您的 Pages 项目
2. 选择 "设置" → "绑定"
3. 点击 "添加" → "KV 命名空间"
4. 填写绑定信息：
   - **变量名称**：`img_url`（必须是这个名称）
   - **KV 命名空间**：选择刚创建的命名空间
5. 点击 "保存"

### 基础认证配置

| 变量名       | 类型   | 必需 | 说明         | 示例值                 |
| :----------- | :----- | :--- | :----------- | :--------------------- |
| `BASIC_USER` | string | 否   | 管理员用户名 | `admin`                |
| `BASIC_PASS` | string | 否   | 管理员密码   | `your_secure_password` |
| `AUTH_CODE`  | string | 否   | 上传认证码   | `your_auth_code`       |

### Telegram 渠道

| 变量名         | 类型   | 必需 | 说明               | 示例值                   |
| :------------- | :----- | :--- | :----------------- | :----------------------- |
| `TG_BOT_TOKEN` | string | 是   | Telegram Bot Token | `123456789:ABCdefGHI...` |
| `TG_CHAT_ID`   | string | 是   | Telegram 频道 ID   | `-1001234567890`         |

### 重新部署

绑定 KV 后需要重新部署以生效：

1. 进入项目的 "部署" 页面
2. 找到最新的部署记录
3. 点击右侧的 "..." 菜单
4. 选择 "重试部署"
5. 等待部署完成

## 上传 API

### **基本信息**

- **端点**：`/upload`
- **方法**：`POST`
- **内容类型**：`multipart/form-data`
- **文件大小限制**：根据存储渠道而定

### 请求参数

**Query** **参数**

| 参数名           | 类型    | 必需 | 默认值     | 说明                                                         |
| :--------------- | :------ | :--- | :--------- | :----------------------------------------------------------- |
| `authCode`       | string  | 否   | -          | 上传认证码                                                   |
| `serverCompress` | boolean | 否   | `true`     | 服务端压缩（仅针对Telegram渠道的图片文件）                   |
| `uploadChannel`  | string  | 否   | `telegram` | 上传渠道：`telegram`、`cfr2`、`s3`                           |
| `autoRetry`      | boolean | 否   | `true`     | 失败时自动切换渠道重试                                       |
| `uploadNameType` | string  | 否   | `default`  | 文件命名方式，可选值为`[default, index, origin, short]`，分别代表默认`前缀_原名`命名、`仅前缀`命名、`仅原名`命名和`短链接`命名法，默认为`default` |
| `returnFormat`   | string  | 否   | `default`  | 返回链接格式，可选值为`[default, full]`，分别代表默认的`/file/id`格式、完整链接格式 |
| `uploadFolder`   | string  | 否   | -          | 上传目录，用相对路径表示，例如上传到img/test目录需填`img/test` |

### Body 参数

| 参数名 | 类型 | 必需 | 说明         |
| :----- | :--- | :--- | :----------- |
| `file` | File | 是   | 要上传的文件 |

### 响应格式

`data[0].src`为获得的图片链接

### 示例

在 `PicList` 中使用高级自定义

![image.png](https://teleimg.loophy.top/file/1753605879311_image.png)

如果站点设置了密码需要添加参数 `/upload?authCode=xxx`
