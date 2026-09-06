# Yutter的 AI 笔记

个人 AI 技术博客，基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod)（主题已直接入库）。

- 线上地址：https://yutterx.com （Cloudflare Pages）
- 备用地址：https://yutter2024.github.io/xiaozb.online/ （GitHub Pages，仅 fallback）
- 部署文档见 [DEPLOY.md](DEPLOY.md)

## 写新文章

```bash
hugo new content posts/my-post.md   # 或直接新建 .md 文件
# 编辑完推送即自动部署
git add -A && git commit -m "new post" && git push
```

## 本地预览

```bash
hugo server --port 1313
# 打开 http://127.0.0.1:1313/
```

## 发布检查清单

- front matter：`draft: false`、`slug`、`description`、`tags`
- 文章内的图片放到 `static/images/` 引用 `/images/xxx.png`
- 本地 `hugo --minify` 构建无报错后再 push
