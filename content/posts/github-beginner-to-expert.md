---
title: "GitHub 从入门到精通：找项目、建仓库、协作与自动化"
date: 2026-09-07
draft: false
slug: "github-beginner-to-expert"
description: "万字长文（转载）：一次走通 GitHub——看懂别人的仓库、发布第一个项目、走通一次真实协作、接入 Actions/Pages/Release，以及常见翻车自救"
tags: ["GitHub", "教程", "Git", "转载"]
categories: ["建站教程"]
related: ['domain-email-setup', 'why-this-blog']
---

> ✍️ **转载文章**｜原作者：小墨同学（@xiaomovps）｜[原文链接](https://x.com/i/article/2095169933118480384)｜[原推文](https://x.com/xiaomovps/status/2095322130422653298)

<div class="tutorial-meta">
<strong>适用对象</strong>想用 GitHub 找项目/发成果/参与协作，但一进仓库就晕的人<br>
<strong>预计时长</strong>阅读 30–50 分钟，动手实践 2–3 小时<br>
<strong>前置准备</strong>安装 Git + 注册 GitHub 账号；[域名/DNS 实操见域名邮箱教程](/posts/domain-email-setup/)
</div>

很多人第一次打开 GitHub，只是想跟着 AI 安装一个工具。AI 或安装教程甩来一个仓库链接，点进去以后，Code、Issues、Pull requests、Actions、main、commits、branches 和 tags 同时铺在眼前。原本只想找到下载按钮，几分钟后却连该下载哪个文件、源码能不能直接运行都拿不准。

2026 年 5 月，我在介绍 github-publisher 时留过一句话。

> 不了解 GitHub 也没关系，后面我有时间也会专门花时间去讲解一下 GitHub 出一篇文章。

这句话一直欠着没兑现。我做这个 Skill 时，很多人已经能让 AI 写脚本、做网站、整理 Skill，分享成果和使用开源项目时却常被 GitHub 卡住。按钮可以交给 AI 帮忙点，仓库是否可信、文件该从哪里拿、一次修改会推到哪里，仍然要自己看懂。

这件事拖了几个月，现在终于来补上。

我重新建了一个公开仓库 [github-first-project](https://github.com/xiaomoBoy/github-first-project)，从网页提交 README 开始，又在电脑上添加页面，接着开 Issue、建分支、发 Pull Request、跑 Actions、上线 Pages，最后发布 v1.0.0。教程里的失败也来自这次真实操作。第一次自动检查找不到文件，确实亮了红灯；修复以后重新运行，才变成绿灯。

![图1](/images/github-guide/22-2095170215348969472.jpg)

这篇文章会带你完成两件事。先学会读懂别人的仓库，知道项目能不能用、应该从哪里下载。然后把自己的第一个项目放上 GitHub，让它可以更新、协作、自动检查，也能通过一个公开网址访问。

标题里写了从入门到精通。这里的精通有一个很实际的范围。你能独立处理个人项目和常见的开源协作，遇到普通报错知道先查什么，也知道涉及权限、密钥和历史修改时应该停下来确认。复杂的 Git 历史重写、大型团队权限和企业治理，不在这一篇里展开。

### 本文导读

01｜先看懂别人的仓库

第一章先分清 Git 和 GitHub，第二、三章再认识作者、README、文件、提交、分支和标签。看完这三章，你会知道一个项目值不值得下载，也能在 ZIP、Clone 和 Release 之间选对入口。

02｜发布自己的第一个仓库

第四章先用浏览器创建仓库和提交 README，第五、六章再把项目拉到电脑，走完 status、add、commit 和 push。每一步都会告诉你完成以后应该看到什么。

03｜走通一次真实协作

第七章用 Issue、任务分支和 Pull Request 完成一项修改。第八章再把视角移到别人的项目，讲清 Fork、上游仓库和跨仓库 PR 的关系。

04｜补齐交付和安全

第九章处理 README、.gitignore、License 和敏感信息。第十章接上 Actions、Pages 与 Release，让仓库可以自动检查、公开访问并交付版本。

05｜遇到问题知道先查哪里

第十一章集中处理权限、同步、冲突、大文件和密钥泄露。只想解决眼前报错，可以直接从最后一章的表格开始查。

## 一、GitHub 能替普通人做什么

### 1. Git 负责记录，GitHub 负责连接

很多人第一次接触 GitHub，是因为某个 AI 工具的安装教程给了一个仓库地址。点进去能看到文件，也能下载，于是很容易把 GitHub 理解成一个代码网盘。

它确实能存文件，但文件只是表面。GitHub 真正有用的地方，是它把项目内容、每次修改、问题讨论、协作过程和发布版本放在了同一个位置。

这里要先分清两个名字。

Git 是安装在电脑上的版本管理工具。它负责观察文件变化，记录某次提交包含哪些修改，也能让你回到以前的版本。即使电脑没有联网，Git 仍然可以在本地工作。

GitHub 是承载远端仓库的平台。它让另一台电脑拿到同一份项目，也让其他人查看、讨论和提交修改。后面要用到的 Issue、Pull Request、Actions、Pages 和 Release，都属于 GitHub 提供的能力。

可以暂时把 Git 理解成电脑里的版本记录，把 GitHub 理解成联网以后共同使用的项目页面。这个比喻只帮助你区分职责。真正操作时，我们还是看工作区、提交和远端仓库之间发生了什么。

Repository 常缩写成 repo，中文通常叫仓库。一个仓库里会放项目文件、使用说明、配置和完整的提交历史。仓库不一定是程序，也可以是一套文档、一个静态网站、一组自动化脚本，甚至是一套你自己维护的 Skill。

### 2. 不会写代码，也有三种常见用法

- 找项目。 不少 AI 工具、模型启动器、自动化脚本和数据同步方案都把 GitHub 当作发布入口。README 里有安装方法，Issues 里能查已知问题，Releases 里常放正式版本。

- 保存成果。 文档、Skill、脚本和个人网站都能放进仓库。Git 留下修改历史，GitHub 把提交同步到远端，换电脑后还能继续。它比一排 最终版、最终版2、真的最终版 更容易追踪。

- 参与协作。 发现问题可以开 Issue，改好文档或代码可以发 Pull Request。不会写程序，也能修错字、补中文说明或整理复现步骤。

这也是 AI 时代 GitHub 越来越难绕开的原因。优秀开源项目集中在这里，很多数据同步和自动化方案依赖 Git，AI 帮你生成的文件也需要一个可以审查、回退和分享的位置。AI 能降低操作门槛，但仓库是否可信、许可是否允许、密钥是否安全，仍然需要你自己判断。

![图2](/images/github-guide/01-2095170353484173312.jpg)

### 3. 读完以后的完成标准

学完这篇文章，你应该可以完成下面这些事。

- 打开陌生仓库后，能找到 README、维护记录、License 和 Release

- 根据目的选择 Download ZIP、clone 或正式发布包

- 在网页创建仓库并完成第一次提交

- 在电脑上走完 status、add、commit、push 和 pull

- 用 Issue、分支和 Pull Request 完成一次修改

- 看懂 Actions 的成功与失败，并用 Pages 和 Release 交付结果

这些对象看起来多，真正跑完一次以后，它们会落在同一条路径上。接下来先从最常见的仓库首页开始。

## 二、第一次打开仓库，先看懂这六组入口

第一次看 GitHub 仓库，不要急着把每个英文按钮都学完。先回答三个问题。这个项目是谁发布的，它解决什么问题，我下一步要找的是说明、文件还是正式版本。

![图3](/images/github-guide/02-2095170408043671553.jpg)

### 1. 作者名/仓库名 先告诉你项目归谁

仓库页面左上方通常会显示 作者名/仓库名。比如 xiaomoBoy/github-publisher，斜杠前面是账号或组织，后面是仓库名称。

同名项目可能由不同账号发布，下载可执行文件、安装插件或运行脚本前，最好把作者和仓库名一起核对。名称附近的 Public 表示公开可见，Private 只对获得权限的人开放。公开不等于可以随意修改或商用，具体范围还要看 License。

### 2. README 是第一份使用说明

进入仓库首页以后，页面下半部分经常会直接展开 README。它通常叫 README.md，可以把它理解成项目说明书。

一份能用的 README 应该说明项目用途、安装方法、使用方式、支持环境和反馈入口。英文很长时，先找 Installation、Usage、Requirements、Examples、FAQ 和 License。浏览器翻译和 AI 可以帮忙，命令里的文件名、版本号和参数仍要回原文核对。

README 只有一句口号，没有安装条件和使用示例，最近也看不到维护记录时，先别运行。Star 再多也补不上缺失的说明。

### 3. 文件列表不要逐个点

README 上方是文件列表。初学者容易从第一项点到最后一项，最后看见一堆配置文件，还是不知道入口在哪里。

先认几组常见名字会更省时间。

- README.md 和 LICENSE 分别放说明与许可

- src/、scripts/、examples/ 常放源码、辅助脚本和示例

- docs/ 或 references/ 常放文档和参考资料

- .github/ 常放协作配置，package.json、pyproject.toml、requirements.txt 常说明依赖和运行环境

这些是常见约定，不是强制标准。真正入口还是以 README 为准。遇到一个只有源码、没有说明的项目，先把它当成需要研究的材料，不要把它当成双击就能运行的软件。

### 4. Commits、Branches、Tags 看的不是同一件事

Commits 保存每次修改，能看到谁在什么时候改了哪些文件。Branches 让不同修改暂时分开，默认分支通常叫 main。Tags 给某个提交留下固定名字，常用来标记 v1.0.0 这样的版本。

第一次使用别人的项目，通常不用研究所有分支。先看默认分支，再看 Releases 有没有正式版本。只有 README 明确要求使用某个开发分支时，才切过去。

### 5. Issues、Pull requests、Actions 分别负责什么

- Issues 记录问题、建议和待办。安装失败时先搜错误关键词，新建问题时写清系统、软件版本、复现步骤和报错。

- Pull requests 常缩写成 PR，用来提交一组修改并请求合并。讨论、检查结果和文件差异都在这里。

- Actions 执行自动检查、测试或部署。绿色对勾表示通过，红色叉号表示至少有一步失败，原因要进日志查看。

### 6. Stars、Forks 和热度只能作为线索

Star 很像收藏，也能反映项目受到多少关注。Fork 表示有多少关联副本被创建。它们可以帮助你发现热门项目，却不能单独证明安全和质量。

一个仓库 Star 很多，可能已经很久没有维护。Fork 很多，也可能只是教程要求大家点过。下载前更有价值的信息，是最近提交时间、README 是否完整、Release 是否持续更新、Issues 里有没有大量相同故障，以及许可证是否清楚。

看到这里，你已经能读懂仓库首页的主要区域。下一步才是下载。

## 三、下载之前多看三眼

GitHub 上的下载有几条路。选错以后最常见的结果，是拿到一堆源码却不知道怎么运行，或者把 ZIP 当成可以持续更新的本地仓库。

![图4](/images/github-guide/14-2095170498489618432.jpg)

### 1. Download ZIP 适合只拿当前文件

在仓库首页点击 Code，再选 Download ZIP，可以下载当前分支的一份压缩包。

它适合一次性查看文档、拿几张素材，或者下载不需要继续跟踪的简单项目。解压以后能看到文件，但里面没有可供你正常使用的完整 Git 历史。作者以后更新，电脑里的 ZIP 也不会自己变化。

ZIP 只是文件打包方式。它不保证项目可以直接运行。打开之前仍然要看 README 里的安装条件。

### 2. Clone 适合继续跟踪和修改

Clone 会把仓库和版本数据复制到电脑。以后可以用 Git 查看变化、拉取更新，也可以把自己的提交推回 GitHub。

Code 菜单里通常能看到 HTTPS、SSH 和 GitHub CLI 等入口。这里先知道 clone 适合长期使用，登录方式放到第六章再选。

如果你准备跟着项目更新、修改代码、让 AI 在本地继续开发，clone 通常比 ZIP 更合适。

### 3. Release 适合拿作者正式交付的版本

很多桌面软件会在 Releases 页面提供安装包。macOS 可能看到 .dmg，Windows 可能看到 .exe 或 .msi，也可能提供压缩后的可执行文件。

GitHub 还会自动给每个 Release 提供 Source code 归档。它们是这个版本对应的源码 ZIP 和 tar.gz，不等于作者主动上传的安装包。

![图5](/images/github-guide/06-2095170538226499584.jpg)

我们这次发布的 v1.0.0 没有额外上传应用安装包，所以页面里只有 GitHub 自动生成的两个源码归档。这个结果反而很适合说明差别。看到 Assets 时先读文件名和版本说明，不要凭体积猜哪个能运行。

### 4. 真正要多看的三眼

第一眼看发布者和维护状态。

确认作者账号与项目来源，看看最近一次提交和最近一个 Release 是什么时候。长期不更新不一定不能用，但你要确认它是否支持当前系统和依赖版本。

第二眼看说明与已知问题。

README 写的是正常路径，Issues 往往能看到真实使用中的故障。如果首页写着支持 macOS，Issues 里却有很多最新版无法启动的报告，就要继续看维护者有没有回应。

第三眼看 License 和安全提示。

准备二次修改、公开分发或商业使用时，先确认许可证。项目让你复制命令到终端时，也要知道命令准备下载什么、修改哪里、会不会要求管理员权限。

### 5. AI 可以帮你读，但最后三件事自己确认

你可以把 README 交给 AI，让它整理安装条件，也可以让它解释目录和报错。更稳妥的问法，是让 AI 指出每个结论来自说明里的哪一段，再回到原文核对。

最后三件事不要直接外包给 AI。

- 下载地址是不是项目官方入口

- License 是否允许你的具体用途

- 即将执行的命令会获得什么权限

AI 可能读错旧文档，也可能忽略仓库刚出现的安全提醒。确认完这三项，再下载和执行。

## 四、只用浏览器，建好第一个仓库

接下来开始建立自己的项目。前半段完全在浏览器里完成，不需要先安装 Git。

### 1. 注册、验证邮箱和保存账号恢复入口

打开 [GitHub](https://github.com/)，点击 Sign up，按页面要求填写邮箱、密码和用户名，再完成邮箱验证。

用户名会出现在仓库地址里，也会成为公开身份的一部分。用于长期创作时，最好选一个愿意持续使用的名字。邮箱如果不想公开，可以在 GitHub 邮箱设置里使用隐私地址，并在后面的 Git 配置里保持一致。

账号建好后，建议在设置中开启双重验证。常见方式是认证器应用生成的一次性验证码。恢复代码只保存在密码管理器或其他安全位置，不要放进仓库、截图和聊天记录。

这篇文章不展示真实二维码和恢复代码。账号安全截图如果无法完全避开秘密，宁可不放。

### 2. 新建 github-first-project

点击右上角加号，选择 New repository，进入新建仓库页面。

![图6](/images/github-guide/23-2095170591020269568.jpg)

仓库名填写 github-first-project。描述可以写成一句容易看懂的话，例如 My first GitHub practice project。

Visibility 决定可见范围。本文需要演示 Pages、Issue 和 Release，所以选择 Public。真实工作里如果包含私人笔记、客户资料、内部文档或还没检查的 AI 生成文件，先用 Private 更安全。

初始化时勾选 README。.gitignore 可以按项目类型选择，暂时没有特定语言也可以后面再加。License 选择 MIT 只为了完成本次公开演示，它不一定适合所有项目。

点击 Create repository 后，能进入仓库首页并看到 README.md，说明第一个仓库已经建好。

### 3. 在网页里改 README，完成第一次 commit

打开 README.md，点击编辑按钮，在原有标题下补一句项目说明。预览确认排版以后，点击 Commit changes。

GitHub 会让你填写提交信息。不要只写 update。这次可以写 docs: add project introduction，读起来就知道修改的是文档，内容是补充项目介绍。

提交完成以后返回首页。README 多了一行，Commits 里的数量也会增加。这个可见变化说明你已经完成了一次 commit。

网页提交和后面在终端执行 git commit 的结果属于同一种版本记录。区别只在于操作入口。GitHub 网页替你完成了选文件和提交，本地 Git 会把这几个动作拆开显示。

### 4. 在网页创建分支并提交修改

点击当前分支名 main，输入一个新分支名，例如 docs/web-edit，按页面提示创建分支。

切换成功后，页面上的分支名会变。此时编辑 README 并提交，修改会留在新分支，main 暂时不受影响。

分支的价值就在这里。你可以隔离一组尚未确认的改动，检查完成后再合并。第一次练习不用同时建很多分支，一项清楚的任务配一个分支就够了。

到这一步，你已经在浏览器里拥有了仓库、README、提交和分支。接下来需要补上 Git 的最小工作模型。

## 五、Git 怎样记住每一次修改

Git 的命令很多，新手阶段先掌握四个位置就够用。

### 1. 四个位置已经够用

工作区是你正在编辑的项目目录。git add 把选中的修改放进暂存区，git commit 将它保存为本地版本，git push 再把本地提交送到 GitHub。四个位置和三个动作可以直接看下面这张图。

![图7](/images/github-guide/12-2095170635450474496.jpg)

这条路能解释大多数新手疑问。文件明明改了，GitHub 上为什么没有变化，因为改动还在工作区。已经 commit 了，网页为什么没更新，因为提交还在本地。push 提示没有新内容，可能是改动根本没有形成新的 commit。

### 2. 一次修改怎样走过四个位置

```
git status
git add README.md
git commit -m "docs: update README"
git push
```

第一条先看变化。新文件通常出现在 Untracked files 下，加入暂存区后会移到 Changes to be committed。commit 成功后，本地已经有了版本；GitHub 网页出现新提交，才说明 push 也完成了。

### 3. GitHub 页面上的 commit 从哪里来

第四章在网页修改 README 时，GitHub 已经直接在远端创建了 commit。本地操作时，commit 先产生在电脑，再通过 push 发送到 GitHub。

每次 commit 最好只处理一组相关修改。修一个说明错字和重写整个页面放在同一个提交里，后面很难审查，也不容易单独撤回。

提交信息写清动作和对象即可。docs: add usage instructions 比 改一下 更有价值。几个月后查看历史时，你不用点开差异就能大致知道发生了什么。

## 六、把仓库拉到电脑，再把修改推回去

下面以 macOS 为主线。Windows 可以安装 Git for Windows，并在 Git Bash 里使用相同的 Git 命令。编辑器和路径显示会有差别，仓库里的版本逻辑相同。

### 1. 安装 Git，只做一次身份设置

先运行 git --version。看到版本号就可以继续；没有安装时，从 [Git 官网](https://git-scm.com/downloads) 选择当前系统版本。Windows 安装 Git for Windows 时，初学阶段保留默认选项通常就够用。

第一次使用需要设置提交身份。

```
git config --global user.name "你的提交署名"
git config --global user.email "与你的 GitHub 账号关联的提交邮箱"
```

把第一行改成你的提交署名，它可以是真名或昵称，并不要求与 GitHub 用户名相同。两项都会写进未来的提交记录，[GitHub 官方说明](https://docs.github.com/en/get-started/git-basics/setting-your-username-in-git)也明确区分了 Git 署名与 GitHub 用户名。邮箱介意公开时，先到 GitHub 邮箱设置里复制隐私地址。它们不是登录密码，也不负责给电脑授权。

### 2. HTTPS、GitHub CLI 和 SSH 怎么选

GitHub 不再接受把账号密码直接当作 Git 远端密码。现在常见的是 HTTPS 配合凭据管理、GitHub CLI 浏览器登录，或者 SSH 密钥。

想少处理密钥，可以安装 GitHub CLI，运行 gh auth login 后按提示用浏览器登录，再用 gh auth status 检查当前账号。

经常在同一台电脑推送，也可以配置 SSH。把公钥添加到 GitHub 账号后，运行 ssh -T git@github.com 测试。第一次连接可能出现主机指纹确认，先和 [GitHub 公布的指纹](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)核对，再输入 yes。

看到 GitHub 识别出用户名就说明连接可用。私钥内容始终不能粘贴到网页、仓库或聊天里。

![图8](/images/github-guide/19-2095170780955136000.jpg)

这次真实操作还遇到一个细节。仓库最初通过 HTTPS 连接，现有 GitHub CLI 授权没有包含修改工作流所需的范围，推送 .github/workflows 时被拒绝。切换到电脑已经配置好的 SSH 后，工作流才正常推送。这个经历不代表每个人都必须用 SSH，只说明遇到权限报错时要核对当前登录方式和授权范围。

### 3. 克隆自己的仓库

先在电脑上选一个专门放练习项目的目录，再复制仓库 Code 菜单里的地址。HTTPS 可以用 git clone https://github.com/你的用户名/github-first-project.git，SSH 使用 git clone git@github.com:你的用户名/github-first-project.git，GitHub CLI 则是 gh repo clone 你的用户名/github-first-project。三种方式选一种即可，并把示例用户名换成自己的账号。

克隆完成后进入目录并检查状态。

```
cd github-first-project
git status
```

看到当前位于 main，并且 working tree clean，说明本地仓库已经准备好。不要在一个不相关的大目录里直接运行 Git 初始化，先确认终端当前路径确实是这个练习项目。

![图9](/images/github-guide/05-2095170863251529728.jpg)

### 4. 新增 index.html，走完第一次本地提交

用编辑器在仓库根目录创建 index.html，写入一个最小页面。

```htmlbars
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <title>My First GitHub Project</title>
  </head>
  <body>
    <h1>Hello GitHub</h1>
    <p>这是我的第一个 GitHub 项目。</p>
  </body>
</html>
```

```shell
git status
git add index.html
git status
git commit -m "feat: add first project page"
git push origin main
```

第一次 status 会把 index.html 列为未跟踪文件，git add 后再检查，它应该出现在待提交区域。

![图10](/images/github-guide/08-2095171047352139776.jpg)

commit 保存本地版本，push 把它同步到 GitHub。提交成功后工作区会恢复干净。

![图11](/images/github-guide/10-2095171088686927872.jpg)

打开 GitHub 仓库，文件列表里出现 index.html，Commits 里出现刚才的提交信息，说明第一次本地修改已经同步成功。

下面这张图是项目完成后的同步复核。先看提交历史，再运行 git push origin main。终端返回 Everything up-to-date，最后的状态也显示本地 main 与远端一致。

![图12](/images/github-guide/07-2095171123399061505.jpg)

### 5. push 前先 pull，什么时候真的需要

很多教程把 git pull 写成 push 前固定动作。它的真正用途，是把远端已有的新提交拿到本地。

刚 clone 完成，只有你在一台电脑上修改，远端期间也没有变化，可以直接 push。多人协作、换设备编辑，或者你刚在 GitHub 网页改过文件，本地就可能落后。

不确定时先获取远端状态。

```
git fetch origin
git status
```

如果状态提示本地分支落后，再执行 git pull origin main。

Git 能自动合并就会完成同步。同一位置被两边修改时会出现冲突，先处理冲突再 push。不要一看到拒绝就强推，强推可能覆盖远端历史。

## 七、Issue、分支和 PR 怎样组成一次协作

前面完成的是个人提交。现在把一次修改按协作方式重新走一遍。

### 1. 先开 Issue，把要做的事写清楚

进入仓库的 Issues，点击 New issue，标题写 给页面增加使用说明。

正文不用写得很长，至少包含当前情况和预期结果。例如 README 还没有说明怎样打开页面，希望补充本地查看方式和 Pages 地址。

一条清楚的 Issue 能让后续分支和 PR 都围绕同一个目标。报错类 Issue 还要补上系统版本、操作步骤和完整错误信息。密钥、邮箱和私人路径要先处理。

这次演示创建的是 Issue #1。任务完成后，它被标记为 completed。

![图13](/images/github-guide/17-2095171203996749825.jpg)

### 2. 从最新 main 建分支

回到本地，先更新 main，再创建任务分支。

```
git switch main
git pull origin main
git switch -c docs/add-usage
```

分支名用 docs/add-usage，一眼就能看出这次修改属于文档，目标是补充用法。

建分支以前先更新 main，可以减少后面合并时的差异。修文档和改功能属于两项无关任务时，分别建立分支，不要全塞进一个 PR。

### 3. 提交并 push 分支

在 README 中加入使用说明，保存后用 git status 和 git diff 确认只改了需要的内容，再提交并推送分支。

```
git add README.md
git commit -m "docs: add usage instructions"
git push -u origin docs/add-usage
```

参数 -u 会建立本地分支与远端分支的跟踪关系。以后仍在这个分支时，通常可以直接运行 git push。

### 4. 创建 Pull Request，先看 diff

分支推送后，GitHub 仓库首页通常会出现 Compare & pull request。进入创建页面时，base 选择 main，compare 选择 docs/add-usage。

标题说明这次修改做了什么，描述写清修改内容和检查方法。需要关联同仓库 Issue 时，可以在 PR 描述里写 Closes #1。

PR 合并到默认分支后，GitHub 才会自动关闭对应问题；跨仓库问题需要写成 Closes 作者名/仓库名#1。[官方文档](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue?apiVersion=2022-11-28)列出了完整规则。

这次实操中，PR 合并后 Issue #1 没有按预期关闭，最后由我手动标记完成。教程保留这个结果，也提醒你合并后回到 Issue 页面检查状态。

创建以前先看 Files changed。确认没有误删文件，没有混入编辑器缓存、本地配置和敏感信息。需要提前讨论方案时，可以先建 Draft PR，等内容准备好再转成正式审查。

### 5. 自己也要做一次 review

个人仓库只有自己维护，也值得走一次 review。先看文件数量，一个只改 README 的任务突然出现十几个文件，通常说明提交范围有问题。再看绿色新增和红色删除，检查误删、拼写、无关格式变化与私人信息。最后看自动检查是否通过，它只能验证写进规则的内容，不能替你判断说明是否准确。

### 6. 合并、关闭 Issue、删除分支

检查完成后点击 Merge pull request，再确认合并。回到 main 查看 README，新增说明已经出现，说明修改进入主分支。

![图14](/images/github-guide/16-2095171330949992448.jpg)

接着回 Issue 检查状态，再删除已经合并的 docs/add-usage 分支。删除分支不会删除已经合并的提交，也不会让 PR 讨论消失。本地需要继续工作时，切回 main、拉取最新内容，再删除旧任务分支即可。

到这里，一条完整的 GitHub flow 已经跑完。Issue 说明任务，分支隔离修改，commit 保存过程，PR 承担检查和合并。

## 八、修改别人的项目，为什么要先 Fork

自己的仓库可以直接 push。别人的公开仓库通常只给维护者写权限，这时 clone 和 fork 的区别就很重要。

### 1. Clone 和 Fork 解决不同权限问题

Clone 是把仓库复制到电脑。它给你本地文件和历史，不会自动给你向原仓库 push 的权限。

Fork 会在自己的 GitHub 账号下创建一份关联仓库。你可以把修改推到自己的 fork，再向原仓库提交 PR。

原作者仓库、你的 fork 和本地电脑之间的关系，可以直接看下面这张图。

![图15](/images/github-guide/13-2095171372335104000.jpg)

这里的 origin 和 upstream 是常见远端名称。它们只是本地别名，真正指向哪个地址可以用 git remote -v 检查。

### 2. 一次标准贡献路径

先读仓库的 README、License 和 CONTRIBUTING。维护者可能规定分支命名、测试方式和 PR 模板，也可能明确不接受某类修改。

确认任务后点击 Fork，在自己账号下生成副本，然后 clone 自己的 fork，并把原作者仓库添加为 upstream。

```
git clone https://github.com/你的用户名/目标仓库.git
cd 目标仓库
git remote add upstream https://github.com/原作者/目标仓库.git
git fetch upstream
git switch main
git merge upstream/main
git switch -c docs/fix-guide
```

这组命令先准备本地仓库，再把 main 同步到上游最新状态，最后创建修改分支。后面的 add、commit 和 push 与第七章相同，不再重复列一遍。完成后在 GitHub 创建跨仓库 PR，base 指向原作者仓库的目标分支，compare 指向你的 fork 和修改分支。

### 3. 第一次贡献从文档和小修正开始

第一次参与开源，优先选边界清楚的小任务。文档错字、失效链接、明确标记为 good first issue 的问题，都比直接重写核心功能更容易获得有效反馈。

贡献前先搜索已有 Issue 和 PR，避免重复劳动。修改后写清原因和验证方式，不要为了留下贡献记录制造没有实际价值的格式变化。

维护者可能要求修改，也可能拒绝 PR。公开协作的重点是让项目得到合适的改进，PR 是否合并要尊重项目规则。

## 九、一个能交给别人用的仓库还缺什么

项目能在自己电脑运行，不代表已经适合交给别人。陌生人看不到你的操作背景，只能依靠仓库里的文件理解和判断。

### 1. README 先回答五个问题

一份够用的 README 先回答五个问题。

- 项目做什么，解决哪种实际问题

- 最短的开始方式，包括依赖、安装和运行入口

- 一个读者可以照着完成的使用示例

- 出问题以后去哪里求助

- 谁在维护，项目目前处于实验、暂停还是稳定状态

README 不需要第一天就写成长篇文档。先让一个没参与项目的人可以按说明跑起来，再随着真实问题补充。

### 2. .gitignore 保护的是不该进入版本历史的文件

.gitignore 用来忽略缓存、构建产物、本地配置和临时文件。常见例子包括 .DS_Store、node_modules/、Python 缓存和本地 .env。

```
.DS_Store
node_modules/
__pycache__/
.env
```

已经提交过的文件，后来写进 .gitignore 不会自动停止跟踪。以 .DS_Store 为例，可以运行 git rm --cached .DS_Store，再提交这次变化。这个动作只处理当前跟踪状态，旧提交中的内容仍然存在。

.gitignore 也不是密钥保险箱。真正的 Token、API Key 和私钥不应该先写进项目再依赖忽略规则补救。使用环境变量、密码管理器或 GitHub Secrets，才是更稳妥的路径。

### 3. Public 和开源中间还隔着 License

公开仓库让别人可以查看和 fork。没有 License 时，默认版权仍然保留，别人并不会自动获得复制、修改和分发许可。

本文演示选择 MIT License，是因为它条款相对简洁，也适合这个最小示例。采用它通常需要保留版权和许可声明。项目涉及公司代码、第三方素材、模型权重、数据集或商业授权时，不能照抄教程里的选择。

看别人的项目也一样。README 写着免费，不等于许可证允许所有用途。准备发布二次作品前，要核对仓库实际 License 和依赖项目的许可。

### 4. 项目长大以后再补文档

项目开始有人使用以后，再按真实问题补文档。CONTRIBUTING 说明怎样贡献，SECURITY 提供安全问题报告渠道，CHANGELOG 记录版本变化。个人练习项目不必第一天把模板全部建齐，长期没人维护的空模板反而会误导读者。

### 5. 公开前做一次安全扫描

发布前先搜索容易泄露的内容。

- API Key、Token、密码和私钥

- 个人邮箱、绝对路径和设备名

- 客户文件、内部地址和未公开数据

- 数据库、备份包、模型和大体积媒体

再看 git status 和即将提交的 diff，确认文件范围。AI 生成的项目尤其需要这一步，因为它可能顺手创建调试日志、示例密钥或本地配置。

我做 github-publisher 时，把敏感信息扫描、仓库内容检查和推送前确认放进了流程。工具可以减少遗漏，最终公开范围仍然要由人确认。

![图16](/images/github-guide/11-2095171505873354752.jpg)

## 十、让项目自动检查、上线并发布版本

仓库结构稳定以后，可以让 GitHub 自动完成重复检查，再把静态页面发布成网站。

### 1. Actions 先做一件有用的小事

GitHub Actions 的工作流文件放在 .github/workflows/。这次创建 check.yml，只检查 README 和页面文件是否存在。

```yaml
name: Check project files

on:
  push:
  pull_request:

jobs:
  check-files:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - name: Check required files
        run: |
          test -f README.md
          test -f index.html
```

on 说明什么时候运行，这里选择 push 和 pull request。jobs 下面是一组任务，runs-on 指定 GitHub 提供的运行环境，steps 按顺序拉取仓库并检查文件。

![图17](/images/github-guide/03-2095171621808160768.jpg)

第一次接触 Actions，先让一项确实有用的小检查跑通，再逐步增加内容。本文更新时，actions/checkout 官方示例已经使用 v7，因此上面的可复制配置也换成了 v7。

### 2. 看懂绿灯和失败日志

为了确认失败路径是否真实可见，第一次工作流检查的是并不存在的 MISSING.md。提交后进入 Actions，运行结果亮起红灯。

![图18](/images/github-guide/09-2095171662065156096.jpg)

展开失败步骤后，日志显示命令返回 exit code 1。它没有告诉我们一大段玄学原因，核心问题就是目标文件不存在。

这次运行还提示旧版 Checkout 所用运行环境即将弃用。实操当时从 v4 升到了 v5，所以截图中仍会看到 v5；当前可复制版本已经按 [Checkout 官方说明](https://github.com/actions/checkout/blob/main/README.md)更新为 v7。教程里的配置会过期，警告信息值得读完。

第二次把检查目标改成真实存在的 README.md 与 index.html，新运行顺利通过。

![图19](/images/github-guide/21-2095171704473759745.jpg)

红灯的处理顺序很简单。先点进失败运行，再展开红色步骤，找到第一条真正失败的命令，然后回到对应文件修复。不要只反复点击重新运行，同一份内容通常还会得到同一个结果。

### 3. 用 Pages 把 index.html 发成网站

进入仓库 Settings，找到 Pages。在 Build and deployment 中选择从分支发布，再选择 main 和仓库根目录。

![图20](/images/github-guide/18-2095171744822898688.jpg)

保存以后，GitHub 会开始部署。第一次出现地址可能需要等待一会儿。页面显示成功后访问公开网址，确认标题和正文与仓库中的 index.html 一致。

这次实操得到的地址是 [https://xiaomoboy.github.io/github-first-project/](https://xiaomoboy.github.io/github-first-project/)。

![图21](/images/github-guide/20-2095171782915563520.jpg)

Pages 适合 HTML、CSS、JavaScript 组成的静态内容，例如项目说明、作品页和文档站。它不能直接运行需要常驻服务器、数据库或私密后端环境的完整应用。

### 4. 打标签并发布 v1.0.0

项目得到一个可以对外说明的阶段结果后，可以发布 Release。

进入 Releases，点击 Draft a new release，新建标签 v1.0.0。标题也可以写 v1.0.0，说明里列出这版已经完成的内容，例如静态页面、使用说明、自动文件检查和 Pages 地址。

标签负责给当前提交一个固定名字。Release 在标签基础上增加版本说明和可选附件。两者有关联，承担的用途不同。

发布以后回到 Release 页面，检查标签、目标提交、说明和 Assets。需要交付安装包时，由作者主动上传经过验证的文件。GitHub 自动生成的 Source code 归档不能替代安装包说明。

![图22](/images/github-guide/15-2095171820492304384.jpg)

到这一步，github-first-project 已经有 Actions 绿灯、公开 Pages 地址和 v1.0.0。一个最小项目从文件走到了可以检查和交付的状态。

![图23](/images/github-guide/04-2095171861525209088.jpg)

## 十一、最常见的五种翻车怎样救

Git 的报错看起来很长，先抓住错误类型，再决定第一步。下面这张表可以先存着。

| 现象                          | 先检查什么               | 第一处理动作                         |
| --------------------------- | ------------------- | ------------------------------ |
| `Permission denied` 或 `403` | 当前账号、仓库权限、远端地址、登录方式 | 确认身份和权限，不要反复输入账号密码             |
| push 被拒，远端有新提交              | 本地是否落后、远端改了什么       | 先 fetch，再决定怎样同步                |
| 合并冲突                        | 哪些文件的同一位置被两边修改      | 打开冲突文件，保留最终内容后重新提交             |
| 文件过大或仓库变重                   | 大文件类型、是否应该进入版本历史    | 从提交中移出，按用途选择 Release 或 Git LFS |
| 密钥已经推送                      | 密钥还能不能被使用           | 立即撤销并轮换，再处理仓库历史                |

### 1. Permission denied 或 403

先运行 git remote -v，确认远端地址，再检查当前账号是否有写权限。HTTPS 要看 GitHub CLI 或凭据管理器里的账号和授权范围，SSH 则用 ssh -T git@github.com 检查公钥是否加到了正确账号。

本文实操在推送工作流文件时遇到过权限不足，切换到已经授权的 SSH 连接后才完成。求助时提供报错文本即可，个人访问令牌不能发进聊天，泄露后应立即撤销。

### 2. push 被拒，提示远端有你没有的提交

这通常发生在你从另一台电脑提交过，或者刚在 GitHub 网页修改了文件。

先获取远端信息并查看当前状态。

```
git fetch origin
git status
```

看清两边变化以后，再运行 git pull origin main。如果自动合并成功，检查结果后继续 push。出现冲突就进入下一节处理。

不要在不了解差异时使用强推。个人仓库也可能被另一台设备或自动化任务更新，覆盖以后很难找回现场。

### 3. 合并冲突

冲突表示 Git 发现同一位置存在两套修改，无法替你选择最终内容。打开冲突文件会看到类似标记。

```
<<<<<<< HEAD
本地内容
=======
远端内容
>>>>>>> origin/main
```

阅读上下文，保留最终需要的内容，并删除三组标记。确认文件可以正常使用后，对实际冲突文件运行 git add 文件名，再执行 git commit。

冲突不等于仓库损坏。没看内容就整段接受一边，或者为了让提示消失直接删文件，才容易把需要的修改一起丢掉。

### 4. 文件太大或仓库越来越重

Git 擅长跟踪文本变化。视频、模型、数据库和压缩备份每次修改都可能把新副本写进历史。

GitHub 网页单文件上传上限是 25 MiB；普通 Git 提交超过 50 MiB 会收到警告，超过 100 MiB 会被拦截。更大的版本文件要使用 Git LFS，供用户下载的安装包也可以放进 Release。具体限制可查看 [GitHub 大文件说明](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)。

只在当前目录删除大文件，不代表它已经从旧提交消失。准备清理历史时先备份并确认影响范围，尤其不要直接在多人仓库重写历史。

### 5. 密钥已经推上去了

先撤销并轮换密钥，让旧密钥立即失效。然后把程序改为从环境变量或 GitHub Secrets 读取凭据，确认新密钥没有再次进入日志和截图。最后再评估怎样清理仓库历史。公开内容可能已经被 clone、缓存或索引，删除文件和 commit 也不能保证外部副本消失。

GitHub 的 Push protection 能拦截一部分已识别的密钥，仍然不能代替提交前检查。账号应开启双重验证，重要服务只给必要权限，并设置合理的有效期。

### 6. 先保住什么

遇到故障时先保账号、凭据和远端历史。对强推、rebase 和历史清理没有把握，就先备份仓库，记下当前分支、远端地址和错误信息。分支多一个、提交信息不够漂亮都可以以后整理，密钥仍然有效或远端可能被覆盖时，应先停下普通操作。

## 结尾｜以后再遇到 GitHub，按这条路走

以后打开陌生仓库，先看发布者、README、维护记录、License 和 Release，再决定下载 ZIP、clone，还是寻找正式安装包。保存自己的项目时，网页提交适合简单修改，长期维护再把仓库 clone 到电脑。

我做 github-publisher，是想让完全不懂 GitHub 的人也能借助 AI 发布 Skill。AI 可以执行命令，仓库来源、授权范围、提交内容和密钥安全仍要由人判断。知道一次修改正处于工作区、本地提交还是远端仓库，你就不用靠背命令处理 GitHub。

下次再打开 GitHub，先确认自己要读项目、拿文件、保存修改，还是交付一个版本。找到眼前这一步，入口就不会那么乱了。
