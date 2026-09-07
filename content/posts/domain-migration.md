---
title: "换域名记：从 GitHub Pages 迁到 Cloudflare Pages"
date: 2026-09-06
posttype: "复盘"
summary: "从 GitHub Pages 迁到 Cloudflare Pages 的完整记录：DNS 等待、证书签发与方案对比。"
draft: false
slug: "domain-migration"
description: "一次真实的博客搬家记录：DNS 记了两轮、证书等不起、最后决定把站点搬到 Cloudflare Pages 的过程与复盘"
tags: ["Meta", "建站"]
categories: ["建站教程"]
related: ['why-this-blog', 'github-beginner-to-expert']
---

## 起因

网站最初搭在 **GitHub Pages + xiaozb.online** 上。一切都顺利：推送即部署、域名秒解析。

> 对 GitHub 的仓库/分支/PR 还不熟？
> 推荐先读 [GitHub 从入门到精通](/posts/github-beginner-to-expert/)，30 分钟看懂。

但卡在了最后一步——**HTTPS 证书**。

DNS 记录加完几分钟就生效了（`xiaozb.online` 已解析到 GitHub Pages 的 IP），
可 GitHub 的 **DNS Check** 默认要等它自己的校验周期跑完，才会签发 Let's Encrypt 证书。
等了一个多小时，`https://` 还是拿不到证书，`Enforce HTTPS` 一直灰着。

更要紧的是：**国内访问 GitHub Pages（*.github.io 那批 IP）时好时坏**，
今天能开，明天可能就超时。个人博客要的是「掏出来就能访问」，
不能赌运气。

## 方案对比

| 方案 | 成本 | HTTPS | 国内访问 |
|---|---|---|---|
| GitHub Pages | 0 元 | 自动但要等 DNS 校验 | 不稳定，看运气 |
| Cloudflare Pages | 0 元 | 自动，几分钟 | 比 GitHub 稳一些 |
| 国内 OSS + CDN | 几十元/年 | 自动但需备案 | 最稳 |

域名 `yutterx.com` 已经托管在 Cloudflare，DNS 切过去一步到位。

## 迁移步骤（真实执行）

### 1. 代码侧

- `hugo.yaml` 的 `baseURL` 改成 `https://yutterx.com/`
- PaperMod 主题本来是用 git submodule 引入的，Cloudflare Pages 对 submodule 支持要看具体版本
  → 干脆 **把主题文件直接入库**（`rm -rf themes/PaperMod/.git && rm .gitmodules`），
  Hugo 对主题的约定是把目录放 `themes/` 下，文件在就是主题在，构建零依赖

```bash
rm -f .gitmodules
git rm --cached themes/PaperMod
rm -rf themes/PaperMod/.git
git add -A && git commit -m "theme 直接入库" && git push
```

### 2. Cloudflare Pages 侧

1. 控制台 → **Workers 和 Pages** → 创建 → **Pages** → **连接到 Git**
2. 选仓库 → 构建配置：框架预设 **Hugo**，构建命令 `hugo --minify`，输出目录 `public`
3. 保存并部署，第一次构建 1-2 分钟
4. 项目 → **自定义域** → 添加 `yutterx.com` + `www.yutterx.com`

### 3. 切流

- 新域名挂好后，旧域名 `xiaozb.online` 停下（删掉旧 DNS 记录即可）
- 旧 GitHub Pages 不再绑域名，只留 `yutter2024.github.io/xiaozb.online` 备用

## 复盘

- **先想清楚托管再动手**：如果一开始就决定用 Cloudflare Pages，能省下整轮证书等待
- **DNS 校验要选面向国内友好的托管**：GitHub Pages 适合搭着玩，长期用建议 CDN/Pages 层
- **submodule 是迁移阻力**：静态站点直接 vendoring 主题，减少外部依赖

> 本站配置、部署文档都在仓库里：`DEPLOY.md`。欢迎参考。
