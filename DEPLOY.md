# 部署文档（当前架构）

```
GitHub 仓库 (yutter2024/xiaozb.online)
   ├── 主线托管: Cloudflare Pages → https://yutterx.com
   └── 备用托管: GitHub Pages (Actions) → https://yutter2024.github.io/xiaozb.online/
```

## Cloudflare Pages（主）

1. https://dash.cloudflare.com → **Workers 和 Pages** → 创建 → Pages → 连接到 Git
2. 选择仓库 yutter2024/xiaozb.online，构建配置：
   - 框架预设：Hugo
   - 构建命令：`hugo --minify`
   - 输出目录：`public`
3. 项目 → **自定义域**：`yutterx.com`、`www.yutterx.com`

## GitHub Pages（备）

- 仓库 `CNAME` 文件控制自定义域名，删掉文件即解绑
- 域名 xiaozb.online 已停用（GitHub 侧不再绑定；阿里云 DNS 记录已删/待删）

## 域名 DNS

- `yutterx.com`：Cloudflare 托管（NS = lars/raquel.ns.cloudflare.com），
  原 xiaozb.online 注册在阿里云（DNS 记录 2026-09-06 已停止使用）

## 变更生效流程

1. 本地改代码 → `hugo --minify` 构建检查 → `git push`
2. Cloudflare Pages 自动构建部署（约 1-2 分钟）
3. 验证：`curl -s https://yutterx.com/ | grep '<title>'`

## 注意事项

- `hugo.yaml` 中 `baseURL` 是主域名，改域名需同步改这里
- 主题 PaperMod 已直接入库（不再用 submodule），升主题 = 直接 git pull 上游对比替换
