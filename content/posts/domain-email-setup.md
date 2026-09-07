---
title: "域名创建与品牌邮箱部署（详细图文教学版）"
date: 2026-09-07
draft: false
slug: "domain-email-setup"
description: "从注册域名到创建第一个品牌邮箱的完整图文教程：Google Workspace 买域名 → Cloudflare 接管 DNS → OquMail 免费域名邮箱 → 配置 MX/SPF/DKIM/DMARC → 测试收发，含全部排错步骤"
tags: ["教程", "域名", "邮箱", "DNS", "Cloudflare"]
categories: ["技术"]
---

按视频真实操作顺序整理，步骤、界面、DNS 记录和排错全部展开。

> **本版重点**：这版只围绕实际创建流程展开。你会按顺序完成：创建域名、验证域名、接管 DNS、创建 OquMail 主账号、添加邮箱 DNS 记录、创建第一个域名邮箱、测试收发。价格和页面名称会变化，遇到不同界面时，以同一功能的实际按钮为准。

## 开始前准备

- Google 账号和长期可访问的辅助邮箱。
- 用于支付域名注册费的付款方式。
- 想注册的域名名称，建议准备 2 至 3 个备选。
- Cloudflare 账号。
- 一个普通邮箱，用来注册 OquMail 和接收激活邀请。
- 密码管理器，用来保存管理员密码、邮箱密码和恢复码。

> ⚠️ **重要**：域名注册完成，不代表邮箱已经创建。域名、DNS 和邮箱是三个连续环节。本教程每完成一个环节都会给出"完成判断标准"。

---

## 第一章 · 创建域名

**步骤 1 · 打开 Google Workspace 注册页面**

打开 Google Workspace 官方注册页面，点击"开始免费试用"、Get started 或相同含义的按钮。已登录 Google 账号时，系统通常会自动识别当前账号；没有登录时，按页面提示登录。

**图 1 · Workspace 注册流程界面**

![图1 注册流程](/images/domain-email-setup/fig01.jpg)

**步骤 2 · 创建新的 Workspace 账号**

选择创建新的账号，填写公司名称或项目名称。个人使用可以填自己的名字或品牌名。员工人数选择"只有您一人"。这个选项主要影响后续 Workspace 的组织设置，不会改变你要注册的域名名称。

**步骤 3 · 选择地区并填写联系信息**

选择地区会影响货币、价格、税费和付款方式。视频使用了土耳其地区作为演示，但实际操作请按真实情况和平台允许范围填写。接着填写姓氏、名字和当前可访问的辅助邮箱。

> ⚠️ **不要照搬视频资料**：视频画面中的地址、电话、邮箱和密码只是演示内容，不要复制。请使用自己的真实、可恢复资料。

**步骤 4 · 选择新的自定义域名**

当页面询问是否已有域名时，选择"获取新的自定义域名"、购买新的域名或相同含义的选项。**不要选临时子域名**，否则后续邮箱地址可能不是你想要的自定义域名邮箱。

**步骤 5 · 搜索域名**

输入准备好的域名名称，点击搜索。系统会告诉你这个域名是否可购买。已被占用的名称不能直接注册，换名称或从系统推荐的备选中选择。

**图 2 · 填写域名注册人的姓氏、名字和辅助邮箱**

![图2 填写联系人](/images/domain-email-setup/fig02.jpg)

> 💡 建议先决定品牌名，再决定后缀。`.com` 适合个人品牌、博客、作品集和商务邮箱。名称尽量短、容易拼写，避免使用难以口述的连续数字和复杂连字符。

**图 3 · 搜索要注册的域名名称**

![图3 搜索域名](/images/domain-email-setup/fig03.jpg)

---

## 第二章 · 确认价格、资料和付款

**步骤 6 · 确认域名价格**

付款前不要只看首年金额。重点检查注册年限、续费价格、货币、税费和自动续费状态。视频中的人民币换算只适用于当时汇率和账号条件，实际金额以你当前结算页显示为准。

**图 4 · 检查域名注册价格和币种**

![图4 确认价格](/images/domain-email-setup/fig04.jpg)

**步骤 7 · 填写域名联系人资料**

按页面要求填写街道、城市、省份、邮编和电话。域名联系人资料用于注册和验证，应该填写真实、长期可访问的信息。若页面显示 Whois 隐私保护选项，查看它是否已经开启。

**图 5 · 填写域名联系人地址和电话**

![图5 联系人地址](/images/domain-email-setup/fig05.jpg)

**步骤 8 · 创建管理员邮箱和密码**

设置管理员用户名和密码。用户名可能成为你的域名邮箱前缀，例如用户名是 admin，域名是 example.com，之后可能创建 admin@example.com。密码不要与 Google 账号或其他网站重复。

- 用户名要记下来，之后会用于登录管理后台。
- 密码保存到密码管理器。
- 恢复邮箱要使用你能长期访问的普通邮箱。
- 不要把视频中的示例密码复制到自己的账号。

**图 6 · 设置 Workspace 管理员用户名和密码**

![图6 管理员账号](/images/domain-email-setup/fig06.jpg)

**步骤 9 · 完成付款**

付款页面可能同时列出域名注册费和 Workspace 试用或订阅费用。逐项确认今天扣款金额、试用结束时间、后续自动扣款金额和取消入口。完成付款后，保存订单邮件和截图。

**图 7 · 确认付款方式和应付金额**

![图7 付款](/images/domain-email-setup/fig07.jpg)

> ✅ **域名创建完成检查**：付款成功只是第一阶段。继续检查域名是否出现在域名管理后台、状态是否为 Active、到期时间是否可见，以及是否收到联系人验证邮件。

---

## 第三章 · 验证域名并接管 DNS

**步骤 10 · 验证域名联系人**

打开辅助邮箱或新建账号的邮箱，查找来自域名注册合作方的验证邮件。点击 Verify、验证联系人或类似按钮。验证完成后，域名状态应从 Action required 变为 Active。

**图 8 · 使用新建的 Workspace 账号进入域名管理页面**

![图8 域名管理](/images/domain-email-setup/fig08.jpg)

**步骤 11 · 确认域名状态**

进入域名列表，确认域名名称、状态、到期时间和 DNS 管理入口。不要在没有备份记录的情况下删除旧 DNS 记录。

**图 9 · 确认域名已经激活**

![图9 域名激活](/images/domain-email-setup/fig09.jpg)

**步骤 12 · 添加域名到 Cloudflare**

登录 Cloudflare，点击 Add a site，输入完整域名，例如 example.com。不要输入 https://、www 或邮箱前缀。选择免费方案。Cloudflare 会尝试扫描并导入现有记录。

**图 10 · 把域名添加到 Cloudflare**

![图10 添加域名](/images/domain-email-setup/fig10.jpg)

**步骤 13 · 检查导入的 DNS 记录**

逐条检查 A、CNAME、MX 和 TXT 记录。A/CNAME 通常和网站有关，MX 和 TXT 通常和邮箱、验证或安全策略有关。先记录旧邮箱的 MX，以便你知道后续切换到 OquMail 时需要替换什么。

**步骤 14 · 替换名称服务器**

Cloudflare 会给出两条名称服务器。复制后回到域名注册商的 Nameservers 页面，删除旧名称服务器，粘贴两条新地址并保存。名称服务器不是普通 DNS 记录，必须在注册商的域名设置页面修改。

**图 11 · 把 Cloudflare 分配的名称服务器复制回域名注册商**

![图11 替换NS](/images/domain-email-setup/fig11.jpg)

**步骤 15 · 处理 DNSSEC**

如果原域名启用了 DNSSEC，先在注册商处关闭或按官方多签名迁移流程处理。普通迁移应等待旧 DS 记录缓存过期，再切换名称服务器。Cloudflare 接管成功后，再在 Cloudflare 生成新的 DS 信息并回填注册商。

**图 12 · 迁移时检查 DNSSEC 状态**

![图12 DNSSEC](/images/domain-email-setup/fig12.jpg)

> ✅ **Cloudflare 接管完成标准**：Cloudflare 页面显示 Active；注册商名称服务器已经替换；网站没有异常；域名可以正常解析。完成后再继续邮箱配置。

---

## 第四章 · 创建 OquMail 主账号

**步骤 16 · 打开 OquMail 并注册**

打开 OquMail 官网，点击 Get started free、Get started now 或相同含义的按钮。注意这里创建的是 OquMail 管理后台主账号，还不是最终给别人使用的域名邮箱。

**图 13 · OquMail 首页，点击 Get started free**

![图13 OquMail首页](/images/domain-email-setup/fig13.jpg)

**步骤 17 · 填写主账号信息**

填写姓名、普通登录邮箱、密码、公司或项目名称、国家和手机号。点击 Create Account 后，去普通邮箱查找 6 位验证码。输入验证码后进入 OquMail 管理后台。

> 💡 免费方案的邮箱数量、域名数量、容量和每日发件额度会以 OquMail 当前官网为准。不要把视频中的额度当成永久不变的承诺。

**图 14 · 填写 OquMail 主账号注册信息**

![图14 主账号](/images/domain-email-setup/fig14.jpg)

---

## 第五章 · 把域名接入 OquMail

**步骤 18 · 添加域名**

在 OquMail 后台点击 Add Domain，输入完整域名，例如 example.com，然后点击 Connect My Domain。平台会展示 TXT、MX、SPF、DKIM、DMARC 等记录。保持这个页面打开，另开 Cloudflare 标签页。

**图 15 · 点击 Add Domain 添加自己的域名**

![图15 添加域名](/images/domain-email-setup/fig15.jpg)

**步骤 19 · 可选的 Cloudflare 一键授权**

如果 OquMail 提供 Cloudflare 一键配置，页面可能要求授权访问 Cloudflare。授权前看清权限范围。你不想授权时，可以取消授权，改用手动复制 DNS 记录的方式。手动配置更容易确认每条记录的具体内容。

**图 16 · OquMail 请求访问 Cloudflare 的授权页面**

![图16 授权](/images/domain-email-setup/fig16.jpg)

**步骤 20 · 添加 MX 收信记录**

在 Cloudflare DNS 页面点击 Add record。类型选择 MX，Name 通常是 @，服务器地址复制 OquMail 显示的地址，Priority 按页面要求填写。视频画面演示的服务器是 mail.oqumail.com、优先级 10，但你要以自己后台显示的值为准。

**图 17 · 添加 MX 记录，指定邮件接收服务器**

![图17 MX记录](/images/domain-email-setup/fig17.jpg)

> ⚠️ 如果域名之前接入过 Google Workspace 或其他邮箱服务，检查并处理旧的 MX 记录。**同一个域名不能同时让两家服务商负责收信。**

**步骤 21 · 添加 SPF 和 DKIM**

SPF 通常是 TXT 记录，用来声明允许代表你的域名发信的服务器。DKIM 也是 TXT 记录，主机名通常包含 selector，内容是一串公钥。二者都必须从 OquMail 后台完整复制。

**图 18 · 添加 SPF 和 DKIM 记录**

![图18 SPF/DKIM](/images/domain-email-setup/fig18.jpg)

> ⚠️ 如果域名已经有 SPF 记录，**不要再创建第二条 SPF**。应按照服务商文档合并授权内容。DKIM 的主机名不能凭感觉改成 @。

**步骤 22 · 添加 DMARC 并验证**

DMARC 主机名通常是 _dmarc，记录内容按 OquMail 提示填写。保存所有记录后回到 OquMail，点击 "I've added the records" — Verify DNS 或类似按钮。

**图 19 · 添加 DMARC 后点击 Verify DNS**

![图19 DMARC](/images/domain-email-setup/fig19.jpg)

> 🔧 验证失败时，先检查五件事：① Name 是否正确；② Type 是否正确；③ Value 是否完整；④ MX 优先级是否正确；⑤ Cloudflare 是否误开了不适用的代理。确认无误后等待 DNS 传播，再重新验证。

---

## 第六章 · 创建第一个域名邮箱

**步骤 23 · 打开 Users and Mailboxes**

在 OquMail 左侧打开 Users and Mailboxes，点击 Add User。Email address 输入邮箱前缀，例如 hello、support 或你的名字，再从下拉框选择已验证域名。页面会组合出 hello@example.com。

**图 20 · 添加用户并设置邮箱前缀、域名和权限**

![图20 添加用户](/images/domain-email-setup/fig20.jpg)

**步骤 24 · 选择用户权限**

如果这个人只需要收发邮件，Role 选择 Mail user 或 Mail only。如果这个账号要管理 OquMail 后台，才选择 Company admin 或 Mail + admin。**权限越少越安全。**

**步骤 25 · 发送激活邀请**

填写接收激活链接的普通邮箱，点击 Create and send invitation。去普通邮箱查找邀请邮件，点击激活链接，为新邮箱设置登录密码，然后完成激活。

**步骤 26 · 登录网页邮箱**

退出管理后台，使用完整域名邮箱和新密码登录 OquMail 网页邮箱。进入后先确认左上角显示的发件人地址是你的域名邮箱，而不是注册 OquMail 的普通邮箱。

**图 21 · 使用新创建的域名邮箱登录网页邮箱**

![图21 网页邮箱](/images/domain-email-setup/fig21.jpg)

**步骤 27 · 测试发信**

点击 Compose，给自己的 Gmail、Outlook 或其他普通邮箱发送测试邮件。邮件标题可以写"域名邮箱测试"。发送后检查外部邮箱是否收到，发件人地址是否显示为 hello@example.com。

**步骤 28 · 测试收信和回复**

从外部邮箱回复测试邮件，回到 OquMail 刷新页面，检查是否收到。再从第三个邮箱服务商发一封新邮件。**至少完成一次发出、收到、回复的闭环。**

**步骤 29 · 测试附件和垃圾邮件**

发送一个小附件，确认对方可以收到。检查邮件是否进入垃圾箱，并查看邮件的 SPF、DKIM、DMARC 结果。如果邮件进入垃圾箱，不要马上反复发送，先检查 DNS 记录和域名信誉。

**步骤 30 · 可选：配置 Outlook**

视频作者展示了 Outlook 自动发现配置。你可以在 Outlook 添加账户时输入完整域名邮箱和密码。如果自动配置失败，到 OquMail 后台查找 IMAP/SMTP 参数，按当前页面填写服务器地址、端口、加密方式和用户名。**不要猜参数。**

**图 22 · 使用完整域名邮箱配置 Outlook**

![图22 Outlook配置](/images/domain-email-setup/fig22.jpg)

> ✅ **邮箱创建完成标准**：邮箱能够登录；能够发到外部邮箱；能从外部邮箱收到邮件；能够回复；发件人地址正确；附件可用；没有明显 SPF、DKIM、DMARC 错误。

---

## 第七章 · 常见错误的处理顺序

1. 域名搜不到或已被占用 → 换一个名称或后缀。
2. 付款失败 → 检查卡片、币种、地区和账单信息，不要反复提交。
3. 域名状态 Action required → 先查验证邮件并完成联系人验证。
4. Cloudflare 一直 Pending → 核对注册商处的两条名称服务器。
5. 域名接入 OquMail 失败 → 先检查 TXT 验证记录。
6. 别人发不进来 → 检查 MX 是否指向 OquMail，是否残留旧 MX。
7. 邮件进垃圾箱 → 检查 SPF、DKIM、DMARC 是否为平台给出的值。
8. 管理员能登录但普通邮箱不能登录 → 检查用户是否激活以及邀请链接是否过期。
9. Outlook 配置失败 → 先用网页邮箱确认账号本身能正常收发，再处理客户端。

## 最终检查清单

- [ ] 域名状态 Active。
- [ ] 联系人验证完成。
- [ ] Cloudflare 状态 Active。
- [ ] 名称服务器已替换。
- [ ] DNSSEC 状态匹配。
- [ ] OquMail 域名验证通过。
- [ ] TXT、MX、SPF、DKIM、DMARC 配置完成。
- [ ] 第一个域名邮箱已激活。
- [ ] 发信、收信、回复、附件测试通过。
- [ ] 管理员已开启两步验证。
- [ ] 邮箱密码和恢复码已保存。

**操作顺序记忆**（一步没有成功，就不要跳到下一步）：

```text
创建域名 → 验证域名 → Cloudflare 接管 DNS → 创建 OquMail 主账号
→ 添加域名 → 填 DNS 记录 → 验证 DNS → 创建邮箱用户 → 激活 → 测试收发
```

## 来源

1. [Google Workspace 域名注册与 Cloudflare 托管视频](https://www.youtube.com/watch?v=wWqbL-CnHFg)
2. [OquMail 免费域名邮箱部署视频](https://www.youtube.com/watch?v=phDIHy_Q8DQ)
3. [Google Workspace 官网](https://workspace.google.com/)
4. [OquMail 官网](https://oqumail.com/)
5. [Cloudflare 官网](https://www.cloudflare.com/)
6. [Google Workspace 官方域名购买说明](https://knowledge.workspace.google.com/admin/domains/purchase-a-domain-when-you-sign-up-for-google-services)
7. [Squarespace 官方域名管理说明](https://support.squarespace.com/hc/en-us/articles/24615057474061)
8. [Cloudflare 名称服务器迁移说明](https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/)
9. [Cloudflare DNSSEC 说明](https://developers.cloudflare.com/dns/dnssec/)
10. [OquMail FAQ](https://oqumail.com/faq)
11. [OquMail 产品说明](https://oqumail.com/product)
