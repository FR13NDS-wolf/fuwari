---
title: Fuwari博客框架
published: 2025-07-27
description: '迁移到fuwari模版'
image: 'https://teleimg.loophy.top/file/1753550039113_20250727011352275.png'
tags: [Blog, Cloudflare, Fuwari]
category: '记录'
draft: false 
lang: ''
---

## 流程图

本地部署Fuwari，编写文章 -> 推送更改到远程Github仓库 -> Cloudflare Pages检测到仓库更新自动构建新的网站静态文件 -> 网站成功更改

## Let's go

1. Fork仓库: https://github.com/saicaca/fuwari
2. 克隆到本地: `git clone <git@github.com:yourname/fuwari.git>`

* 需要自己配置好GitHub SSH密钥

3. 全局安装pnpm: `sudo npm install -g pnpm`
4. 项目根目录安装依赖：`pnpm install` 和 `pnpm add sharp`
5. 成功在本地部署了Fuwari

## 主要配置

在根目录下的 `src` 文件夹中，你可以找到 `config.ts` 我们来开始改写

- title：你的博客主标题
- subtitle：你的博客副标题。可选，在首页会显示为“主标题 - 副标题”
- lang：博客显示语言。注释已经列出了一些常用的值，如：en, zh_CN, zh_TW, ja, ko
- themeColor：hue值则是你的博客主题色，可以在你的博客右上角的画板图标确定喜欢的颜色再填写
- banner：src：即banner图片，支持http/https URL
- favicon：src：即网站图标，支持http/https URL
- links：即友情链接，这些链接在导航栏上
- avatar：即你的头像
- name：即你的名字
- bio：即个性签名，会显示在头像和名字下面
- `NavBarConfig` 为导航栏设置的超链接
- `ProfileConfig` 为你的用户的超链接
- icon：你需要前往[icones.js](https://icones.js.org/)去搜索你想要的图标

`src/content/posts` 下存在一些示例文章,可以直接删除

`src/content/spec/about.md` 是博客的个人主页需要修改一下

根目录下的 `astro.config.mjs` 配置一下自己的站点链接

## 开始写作

1. 首先，在项目根目录执行：`pnpm new-post <这里填写你的文章标题>` 
2. 然后，在根目录下的 `src/content/posts` 文件夹中会多出一个 `xxx.md`文件
3. 我们打开这个文件，你可以看到一些基本信息，我们只需要关注几个重要的信息

```
title: Fuwari博客框架
published: 2025-07-27
description: '迁移到fuwari模版'
image: 'https://teleimg.loophy.top/file/1753550039113_20250727011352275.png'
tags: [Blog, Cloudflare, Fuwari]
category: '记录'
draft: false 
lang: 'zh_CN'
```

- title：文章标题
- published：文章创建时间
- description：文章描述，正常会显示在文章标题下面
- image：文章封面图 (同目录需要写 `./` )
- tag：文章标签
- categories：文章分类
- Lang：与网站语言不同是需要修改

4. 至此我们就可以编写MarkDown语法的博文了

## 本地预览并推送到仓库

根目录执行：`pnpm dev`

![image-20250727下午10025124](https://teleimg.loophy.top/file/1753592460041_image-20250727下午10025124.png)

推送到GitHub仓库

若是配置好SSH密钥可以直接commit推送

* 首先，让我们提交所有文件：`git add .`

- 然后，让我们发布一个本地提交：`git commit -m "Init"`
- 最后，让我们将本地更改提交到远程仓库：`git push`

## Cloudflare Pages部署

构建命令：`pnpm build` ，然后设置构建输出目录：`dist`

![image-20250727下午10653094](https://teleimg.loophy.top/file/1753592819740_image-20250727下午10653094.png)

绑定自定义域，访问自定义域即可访问你的博客！

随后，你只需要在本地编写文章，然后使用Git将更改推送到远程仓库，Cloudflare就会自动部署，更新你的博客！

