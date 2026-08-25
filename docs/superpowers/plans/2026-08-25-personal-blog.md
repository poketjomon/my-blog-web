# 个人博客（Hexo + Butterfly + GitHub Pages）实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 搭建一个纯静态个人博客：Hexo + Butterfly 主题，支持日期/分类/标签/字数统计，关于页承载个人介绍与 GitHub 链接，git push 后经 GitHub Actions 自动部署到 GitHub Pages。

**Architecture:** 本地 Hexo 项目生成静态 HTML，Butterfly 主题提供栏目与文章元信息展示；GitHub Actions 在 push 时自动构建并发布到 GitHub Pages。无后端、无数据库。

**Tech Stack:** Node.js 22、Hexo 7、hexo-theme-butterfly（npm 包）、GitHub Actions、GitHub Pages

**已知常量：**
- GitHub 用户名：`poketjomon`
- 仓库名：`my-blog-web`（与本地目录一致）
- 站点地址：`https://poketjomon.github.io/my-blog-web/`
- Hexo 站点根路径：`/my-blog-web/`（非 username.github.io 仓库必须配置）

---

### Task 1: 初始化 Hexo 项目

**Files:**
- Create: Hexo 脚手架全部文件（`_config.yml`、`source/`、`scaffolds/`、`package.json` 等，由 `hexo init` 生成）
- Modify: `_config.yml`

- [ ] **Step 1: 初始化 Hexo 并合入当前目录**

工作目录为 `/Users/alucard/Code/others/my-blog-web`（已有 git 仓库和 docs/）。注意：`hexo init` 要求目标目录为空，因此先在临时目录初始化，再合入工作区：

```bash
npx hexo init /tmp/hexo-init-tmp
rsync -a --exclude='.git' /tmp/hexo-init-tmp/ /Users/alucard/Code/others/my-blog-web/
rm -rf /tmp/hexo-init-tmp
cd /Users/alucard/Code/others/my-blog-web
npm install
```

Expected: 输出 `INFO  Start blogging with Hexo!`，工作区中出现 `_config.yml`、`source/`、`themes/landscape/`、`package.json`；原有的 `docs/` 与 `.git/` 不受影响。

- [ ] **Step 2: 配置站点基本信息**

编辑 `_config.yml`，替换/新增以下字段（其余保持默认）：

```yaml
# Site
title: 我的博客
subtitle: '随笔与学习记录'
description: '记录随笔、学习笔记与个人介绍'
author: poketjomon
language: zh-CN
timezone: 'Asia/Shanghai'

# URL
url: https://poketjomon.github.io/my-blog-web
root: /my-blog-web/
permalink: post/:title/
```

注意：`root` 必须是 `/my-blog-web/`，否则部署到 GitHub Pages 后静态资源路径全部 404。

- [ ] **Step 3: 验证本地构建与预览**

Run:
```bash
npx hexo clean && npx hexo generate
npx hexo server
```
Expected: `hexo generate` 无报错，生成 `public/` 目录；`hexo server` 输出 `Hexo is running at http://localhost:4000/my-blog-web/`，浏览器打开可见默认主题页面。验证后 Ctrl+C 停止服务。

- [ ] **Step 4: 提交**

```bash
git add -A
git commit -m "feat: 初始化 Hexo 博客项目"
```

---

### Task 2: 安装 Butterfly 主题并配置导航

**Files:**
- Modify: `package.json`（新增 hexo-theme-butterfly 依赖）
- Create: `_config.butterfly.yml`（主题配置覆盖文件）
- Modify: `_config.yml`（theme 字段）

- [ ] **Step 1: 安装主题包**

Run:
```bash
cd /Users/alucard/Code/others/my-blog-web
npm install hexo-theme-butterfly
```
Expected: `package.json` 的 dependencies 中出现 `hexo-theme-butterfly`。

- [ ] **Step 2: 复制主题默认配置到根目录**

Run:
```bash
cp node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```
Expected: 根目录出现 `_config.butterfly.yml`。Hexo 会自动将该文件作为 butterfly 主题的配置覆盖（theme 配置合并机制），node_modules 已被默认 .gitignore 排除，因此主题配置可随仓库版本管理。

- [ ] **Step 3: 启用主题**

修改 `_config.yml`：

```yaml
theme: butterfly
```

- [ ] **Step 4: 配置导航菜单**

编辑 `_config.butterfly.yml`，找到 `menu:` 段并替换为：

```yaml
menu:
  首页: / || fas fa-home
  归档: /archives/ || fas fa-archive
  分类: /categories/ || fas fa-folder-open
  标签: /tags/ || fas fa-tags
  关于: /about/ || fas fa-heart
```

同文件中确认以下字段为 `true`（Butterfly 默认即为 true，若被改动则改回）：

```yaml
post_meta:
  page:
    date_type: created
    categories: true
    wordcount: true
    read_time: true
```

- [ ] **Step 5: 验证主题生效**

Run:
```bash
npx hexo clean && npx hexo server
```
Expected: 浏览器打开 `http://localhost:4000/my-blog-web/`，界面变为 Butterfly 风格，顶部导航出现「首页 归档 分类 标签 关于」。验证后 Ctrl+C 停止服务。

- [ ] **Step 6: 提交**

```bash
git add -A
git commit -m "feat: 安装 Butterfly 主题并配置导航菜单"
```

---

### Task 3: 创建关于页与示例文章

**Files:**
- Create: `source/about/index.md`
- Create: `source/_posts/hello-world.md`（覆盖 init 生成的默认文章，改为随笔示例）
- Create: `source/_posts/my-first-study-note.md`
- Create: `source/images/.gitkeep`

- [ ] **Step 1: 创建关于页**

创建 `source/about/index.md`：

```markdown
---
title: 关于
date: 2026-08-25 12:00:00
type: "about"
---

## 个人介绍

你好，这里是 poketjomon 的个人博客，记录我的随笔与学习笔记。

## 学术经历

（待补充：学校 / 研究方向 / 经历）

## 联系方式

- GitHub: [https://github.com/poketjomon](https://github.com/poketjomon)
```

- [ ] **Step 2: 创建图片目录**

```bash
mkdir -p source/images && touch source/images/.gitkeep
```

- [ ] **Step 3: 替换默认示例文章为随笔示例**

用以下内容完整覆盖 `source/_posts/hello-world.md`：

```markdown
---
title: 博客的第一篇文章
date: 2026-08-25 12:00:00
categories:
  - 随笔
tags:
  - 生活
---

这是博客的第一篇随笔，用来验证文章的日期、分类、标签和字数统计是否正常显示。

这里多写几段文字，让字数统计的数字更有存在感。写作流程是：本地用 Markdown 写文章，执行 `hexo new "标题"` 新建，`hexo server` 预览，然后 git push 自动发布到 GitHub Pages。

图片放在 `source/images/` 目录，随项目一起提交即可。
```

- [ ] **Step 4: 创建学习类示例文章**

创建 `source/_posts/my-first-study-note.md`：

```markdown
---
title: 学习笔记示例
date: 2026-08-25 13:00:00
categories:
  - 学习
tags:
  - Hexo
  - 博客
---

这是一篇学习类文章的示例，演示 Hexo 的 front-matter 元信息写法。

## 常用命令

- `hexo new "标题"`：新建文章
- `hexo server`：本地预览
- `hexo generate`：生成静态文件

文章头部的 `categories` 与 `tags` 决定文章归属的分类和标签；字数统计由主题根据正文自动计算，无需手动维护。
```

- [ ] **Step 5: 验证页面与文章**

Run:
```bash
npx hexo clean && npx hexo server
```
Expected: 首页列出两篇文章；点开任一文章，标题下方显示日期、分类、字数统计与阅读时长；`/my-blog-web/about/` 可访问且渲染出个人介绍。验证后 Ctrl+C 停止服务。

- [ ] **Step 6: 提交**

```bash
git add -A
git commit -m "feat: 添加关于页与示例文章"
```

---

### Task 4: 创建分类页、标签页与归档页

**Files:**
- Create: `source/categories/index.md`
- Create: `source/tags/index.md`

归档页（`/archives/`）Hexo 默认存在，无需创建。

- [ ] **Step 1: 创建分类页**

创建 `source/categories/index.md`：

```markdown
---
title: 分类
date: 2026-08-25 12:00:00
type: "categories"
---
```

- [ ] **Step 2: 创建标签页**

创建 `source/tags/index.md`：

```markdown
---
title: 标签
date: 2026-08-25 12:00:00
type: "tags"
---
```

- [ ] **Step 3: 验证全部栏目**

Run:
```bash
npx hexo clean && npx hexo server
```
Expected: `/my-blog-web/categories/` 显示「随笔(1) 学习(1)」；`/my-blog-web/tags/` 显示标签云；`/my-blog-web/archives/` 按时间列出两篇文章。验证后 Ctrl+C 停止服务。

- [ ] **Step 4: 提交**

```bash
git add -A
git commit -m "feat: 添加分类页与标签页"
```

---

### Task 5: GitHub Actions 自动部署到 GitHub Pages

**Files:**
- Create: `.github/workflows/pages.yml`
- Create: `README.md`
- Modify: `.gitignore`（追加主题缓存排除项）

- [ ] **Step 1: 补充 .gitignore**

在 `.gitignore` 末尾追加（若已存在则跳过）：

```
public/
.deploy*/
```

- [ ] **Step 2: 编写部署工作流**

创建 `.github/workflows/pages.yml`：

```yaml
name: Deploy Blog to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - name: Install dependencies
        run: npm ci
      - name: Build site
        run: npx hexo generate
      - uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: 创建 README**

创建 `README.md`：

```markdown
# 我的博客

基于 Hexo + Butterfly 主题的个人博客，记录随笔与学习笔记。

站点地址：https://poketjomon.github.io/my-blog-web/

## 本地开发

    npm install
    npx hexo server    # 预览 http://localhost:4000/my-blog-web/

## 发布

文章写在 `source/_posts/`，git push 到 main 分支后 GitHub Actions 自动构建并部署。
```

- [ ] **Step 4: 本地最终验证**

Run:
```bash
npx hexo clean && npx hexo generate
```
Expected: 构建成功，`public/` 中可见 `index.html`、`about/index.html`、`categories/index.html`、`post/` 下的文章页面。

- [ ] **Step 5: 提交**

```bash
git add -A
git commit -m "feat: 添加 GitHub Actions 自动部署与 README"
```

- [ ] **Step 6: 创建 GitHub 仓库并推送（需要用户参与）**

⚠️ 此步骤需要用户在浏览器中操作：

1. 打开 https://github.com/new ，仓库名填 `my-blog-web`，**不要**勾选 README/gitignore（本地已有），创建空仓库
2. 回到终端执行：

```bash
cd /Users/alucard/Code/others/my-blog-web
git remote add origin https://github.com/poketjomon/my-blog-web.git
git push -u origin main
```

Expected: push 成功，GitHub 仓库 Actions 标签页出现 "Deploy Blog to GitHub Pages" 运行记录。

- [ ] **Step 7: 开启 GitHub Pages（需要用户参与）**

1. 打开仓库 Settings → Pages → Build and deployment
2. Source 选择 **GitHub Actions**（不是 "Deploy from a branch"）

Expected: Actions 工作流跑完后，https://poketjomon.github.io/my-blog-web/ 可访问，展示完整博客。

- [ ] **Step 8: 线上验收**

逐项核对：
- 首页文章列表含日期、分类、字数统计
- 文章页显示标签与阅读时长
- 关于页、分类页、标签页、归档页均可访问
- 图片相对路径引用正常（文章内若引用 `source/images/` 下图片）

全部通过则计划完成。
