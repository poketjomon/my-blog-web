# 我的博客

基于 Hexo + Butterfly 主题的个人博客，记录随笔与学习笔记。

站点地址：https://poketjomon.github.io/my-blog-web/

## 本地开发

```bash
pnpm install
npx hexo server    # 预览 http://localhost:4000/my-blog-web/
```

## 写文章

```bash
npx hexo new "文章标题"   # 在 source/_posts/ 下新建 Markdown 文件
```

文章头部 front-matter 可设置 `categories`（分类）与 `tags`（标签），字数统计由主题自动计算。
图片放在 `source/images/` 目录，用相对路径引用即可。

## 发布

git push 到 main 分支后，GitHub Actions 自动构建并部署到 GitHub Pages。
