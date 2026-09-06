# xiaozb.online DNS 配置指南（阿里云 DNS）

## 已完成 ✅
- ✅ Hugo 站点代码：`C:\Users\p\projects\xiaozb.online\`
- ✅ GitHub 仓库：https://github.com/yutter2024/xiaozb.online
- ✅ GitHub Pages 启用（workflow 模式），地址：https://yutter2024.github.io/xiaozb.online/
- ✅ GitHub 端 CNAME 已设：xiaozb.online
- ✅ 部署成功，https://yutter2024.github.io/xiaozb.online/ 已返回 200

## 待你操作：阿里云 DNS 加 2 条记录

打开 https://dns.console.aliyun.com/ → 域名列表 → 点 **xiaozb.online** 后面的「解析设置」

### 删除旧记录（如果有）

如果之前已经有指向其他地方的 A/CNAME 记录，先删掉。

### 添加 4 条记录

GitHub Pages 官方要求 `xiaozb.online` 的 4 条记录：

| 主机记录 | 记录类型 | 记录值 | TTL |
|---|---|---|---|
| @ | A | 185.199.108.153 | 600 |
| @ | A | 185.199.109.153 | 600 |
| @ | A | 185.199.110.153 | 600 |
| @ | A | 185.199.111.153 | 600 |
| www | CNAME | yutter2024.github.io | 600 |
| @ | CAA | 0 issue "letsencrypt.org" | 600 |

> **@ 记录**就是主域名 xiaozb.online
> **www 记录**让 www.xiaozb.online 也能访问
> **CAA 记录**允许 Let's Encrypt 签发证书（GitHub Pages 用）

### 添加完毕后

1. 回到 https://github.com/yutter2024/xiaozb.online/settings/pages
3. 等 DNS 生效（通常 5–30 分钟），你会看到：
   - 「DNS check successful」
   - 「HTTPS Enforced」自动启用
4. 访问 https://xiaozb.online — 应该能看到了

## 验证

DNS 生效后命令行验证：

```bash
nslookup xiaozb.online
nslookup -type=CNAME www.xiaozb.online
```

两条都应该指向 GitHub Pages 的服务器。

## 写新文章

```bash
cd C:\Users\p\projects\xiaozb.online
hugo new content posts/my-new-post.md
# 编辑文件
git add -A && git commit -m "new post"
```

GitHub Actions 会自动构建+部署，1–2 分钟内线上可见。