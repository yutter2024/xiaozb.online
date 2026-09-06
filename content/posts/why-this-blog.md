---
title: "为什么我要搭这个博客"
date: 2026-09-06
draft: false
slug: "why-this-blog"
description: "建站初衷，以及为什么选择 Hugo + GitHub Pages 这个零成本方案"
tags: ["Meta", "建站"]
categories: ["随笔"]
---

## 起因

AI 时代，信息的半衰期越来越短。读完一篇论文、踩完一个坑，
过两周自己就忘了。**笔记外化** 是对抗遗忘最便宜的办法。

与其窝在自己的 Notion 里，不如放到公网 —
既方便自己随时翻，也方便偶尔帮到路过的人。

## 技术选型

需求很简单：**写 Markdown、自动部署、能被搜到、花钱最少**。

| 候选 | 成本 | 优点 | 缺点 |
|---|---|---|---|
| Hugo + GitHub Pages | **0 元** | 免费、Git 推送即部署、HTTPS 自动 | 国内访问有时慢/不稳定 |
| Hexo + Vercel | 0 元 | 中文文档多 | 构建慢 |
| WordPress + 虚拟主机 | ~100 元/年 | 后台成熟 | 要维护 |
| Notion 转公开页 | 0 元 | 一键发布 | 风格受限、SEO 差 |

最后选了 **Hugo + GitHub Pages**：
- Hugo 是静态站点生成器里最快的，单页 1ms 出头
- GitHub Pages 对公开仓库免费无限，仓库就是备份
- GitHub Actions 自动构建部署，推送代码即上线
- HTTPS、自定义域名全自动
- 域名我已经有了 `xiaozb.online`，配个 DNS 就行

## 接下来

- 持续更新 AI / Agent 方向的折腾笔记
- 把零散的知识整理成系列文章
- 偶尔分享工具链和工作流

> 本站所有内容仅代表个人观点，如有错误欢迎指正。
> 转载需保留原文链接。