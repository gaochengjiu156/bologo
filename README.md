# 拾页

一个用 Hugo 构建的中文文章站点。文章用 Markdown 保存，推送到 GitHub 后由 GitHub Actions 自动构建并发布到 GitHub Pages。

## 目录结构

| 路径 | 作用 |
| --- | --- |
| `hugo.toml` | 站点配置：标题、简介、分页、永久链接、`hasCJKLanguage`（中文按字计词） |
| `content/posts/` | 文章 Markdown；`_index.md` 是「文章」栏目说明 |
| `layouts/_default/baseof.html` | 页面骨架、导航、meta / OG / 字体加载 |
| `layouts/index.html` | 首页（hero + 最新 5 篇 + 空状态引导） |
| `layouts/_default/list.html` | 文章归档列表，含翻页 |
| `layouts/_default/single.html` | 文章详情页 |
| `layouts/_default/terms.html` | 标签总览（标签云） |
| `layouts/_default/taxonomy.html` | 单个标签下的文章列表 |
| `layouts/404.html` | 404 页面 |
| `static/css/style.css` | 全站样式 |
| `static/favicon.svg` | 站点图标 |
| `.github/workflows/pages.yml` | 推送到 `main` 自动构建并部署 |


## 写一篇文章

在 `content/posts/` 下新建 `.md` 文件，例如 `my-first-note.md`：

```markdown
---
title: "我的第一篇文章"
date: 2026-11-25
draft: false
description: "用一句话介绍这篇文章。"
tags: [随笔]
---

从这里开始写正文。支持标准 Markdown。
```

提交前把 `draft` 设为 `false`。文章图片可以放在 `static/images/`，然后用 `/images/文件名.jpg` 引用。

## 发布到 GitHub Pages

1. 在 GitHub 新建一个仓库。GitHub Free 账号使用 Pages 时，仓库需要设为公开。
2. 在仓库设置 **Settings → Pages → Build and deployment**，将 **Source** 设为 **GitHub Actions**。
3. 将这个项目推送到仓库的 `main` 分支。`.github/workflows/pages.yml` 会安装 Hugo、构建 `public/` 并部署到 Pages；之后每次推送到 `main` 都会自动重新发布。
4. 首次部署完成后，在 **Settings → Pages** 查看网站地址。项目站点通常是 `https://<用户名>.github.io/<仓库名>/`；若仓库名是 `<用户名>.github.io`，地址就是 `https://<用户名>.github.io/`。GitHub Pages 会为默认子域名提供 HTTPS。

连接本地目录到你刚创建的仓库并推送：

```sh
git init
git add .
git commit -m "Build Hugo article site"
git branch -M main
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

## 本地预览

安装 [Hugo](https://gohugo.io/installation/)（建议与 CI 一致：0.170.0），在项目目录运行：

```sh
hugo server -D
```

然后打开终端显示的本地地址。站点标题、简介和样式分别可在 `hugo.toml` 与 `static/css/style.css` 中修改。

## 注意事项

- **`baseURL` 必须与实际部署地址一致且结尾带斜杠**，否则本地构建出的 RSS、sitemap、canonical 链接会指向错误域名。GitHub Actions 构建时会用 Pages 实际地址覆盖它，所以部署本身不受影响。
- `hasCJKLanguage = true` 已开启，中文按字统计，阅读时长与摘要才会正确；不要关掉。
- 文章文件名即 URL 路径（`permalinks` 使用 `:slug`）。**用英文短横线命名**，例如 `hello-hugo.md`；中文文件名会生成百分号编码的地址。
- 新文章默认 `draft: true`，不会出现在线上，提交前记得改为 `false`。
- `public/` 是构建产物，已在 `.gitignore` 中忽略，不要提交。

