# 从零把项目传到 GitHub（面向轻度用户的分步指引）

这份指引假设你**没用过几次 GitHub**，目标是让你在 30 分钟内独立完成：本地建仓 → 首次提交 → 建公开仓库 → 推送 → 核对公开状态。

每一步都按固定结构写：**目的 → 命令 → 预期结果 → 失败怎么办**。看到预期结果再进下一步；对不上就跳到该步的「失败怎么办」或文末速查表。

> **关于本次交付的说明**：为你生成这套文件的运行环境**无法直连 github.com**（实测 `git ls-remote https://github.com/...` 返回 `CONNECT tunnel failed, response 502`）。因此第 7 步的推送必须由你在本机终端执行——这也正是这份指引存在的原因。前 1–5 步（本地建仓与提交）我已经替你做完，你可以直接跳到第 6 步，或从第 1 步复盘一遍。

---

## 目录

- [第 0 步 · 准备环境](#第-0-步--准备环境)
- [第 1 步 · 进入项目目录并核对文件](#第-1-步--进入项目目录并核对文件)
- [第 2 步 · 配置 Git 身份](#第-2-步--配置-git-身份)
- [第 3 步 · 初始化仓库并统一分支名](#第-3-步--初始化仓库并统一分支名)
- [第 4 步 · 提交前安全检查](#第-4-步--提交前安全检查)
- [第 5 步 · 首次提交](#第-5-步--首次提交)
- [第 6 步 · 在 GitHub 网页创建公开仓库](#第-6-步--在-github-网页创建公开仓库)
- [第 7 步 · 关联远程并推送](#第-7-步--关联远程并推送)
- [第 8 步 · 核对公开状态与仓库信息](#第-8-步--核对公开状态与仓库信息)
- [第 9 步 · 验收](#第-9-步--验收)
- [日常更新流程（以后每次改完都走这个）](#日常更新流程以后每次改完都走这个)
- [可选 · 用 GitHub CLI 一条命令搞定](#可选--用-github-cli-一条命令搞定)
- [常见错误速查表](#常见错误速查表)
- [网络需要代理怎么办](#网络需要代理怎么办)

---

## 第 0 步 · 准备环境

**目的**：确认两样东西齐了——本机有 Git、你有 GitHub 账号。缺一样后面都会卡住。

**检查命令**（在 Git Bash 或 PowerShell 里都行）：

```bash
git --version
```

**预期结果**：

```text
git version 2.55.0.windows.3
```

版本号不用一致，能打印出来就说明装了。

**失败怎么办**：

- 提示 `command not found` / `不是内部或外部命令` → 去 <https://git-scm.com/download/win> 下载安装，安装时**一路默认**即可（默认会带 Git Bash 和凭据管理器，两个都后面要用）。
- 装完**关掉终端重开**，再跑一次 `git --version`（新装的可执行文件不会进已打开终端的 PATH）。

**账号**：浏览器打开 <https://github.com>，右上角有头像 = 已登录；没有就注册，邮箱验证完再回来。

---

## 第 1 步 · 进入项目目录并核对文件

**目的**：先看清"要传什么"，避免把临时文件、密钥、缓存一起传上去——**公开仓库传错东西是不可逆的**（见第 4 步）。

**命令**：

```bash
cd "D:/work buddy/2026-10-30-12-30-26"
pwd
ls -la
```

**预期结果**：`pwd` 回显当前目录；`ls -la` 至少看到这些条目：

```text
.gitignore
LICENSE
CONTRIBUTING.md
README.md
README.en.md
docs/
ultra-mode/
```

**失败怎么办**：

- `No such file or directory` → 路径打错了。Windows 路径里的空格要用引号包起来，反斜杠建议统一换成 `/`。
- 看到一堆意外文件（`node_modules`、`.env`、`*.log`）→ 先别继续，回第 4 步处理。

---

## 第 2 步 · 配置 Git 身份

**目的**：Git 的每次提交都要记录"谁提交的"。没配这两项，第一次 `commit` 会直接报错中断。

**命令**：

```bash
git config --global user.name "你的名字或昵称"
git config --global user.email "你的GitHub注册邮箱"
```

**预期结果**：两条命令都**没有任何输出**——Git 配置成功时就是静默的。

**验证**（这一步别省，这是外部核验，不是"应该成功了"）：

```bash
git config --global user.name
git config --global user.email
```

**预期结果**：原样回显你刚设的值。

**失败怎么办**：

- 回显为空 → `--global` 拼错（少横线、多空格）。注意是两个短横线：`--global`。
- 邮箱写错 → 重跑一次 `git config --global user.email "正确邮箱"` 即可覆盖，不用删除。
- 想让提交邮箱不暴露在公开历史里 → 在 GitHub 设置里开启 *Keep my email addresses private*，用它给出的 `xxx@users.noreply.github.com` 作为邮箱。

---

## 第 3 步 · 初始化仓库并统一分支名

**目的**：把普通文件夹变成 Git 仓库，并把默认分支名定为 `main`。

> **为什么强调分支名**：GitHub 新建仓库默认是 `main`，而老版本 Git 本地默认是 `master`。两边不一致会导致推送后出现两条互不相干的分支，新手最常见的困惑来源。**一开始就统一，能省掉后面所有麻烦。**

**命令**：

```bash
git init -b main
```

**预期结果**：

```text
Initialized empty Git repository in D:/work buddy/2026-10-30-12-30-26/.git/
```

`-b main` 表示初始化时直接把分支命名为 `main`。

**失败怎么办**：

- 报 `unknown option: -b` → Git 版本太老（< 2.28）。改用两条命令：
  ```bash
  git init
  git branch -M main
  ```
- 报 `Reinitialized existing Git repository` → 说明这已经是仓库了，**不是错误**，继续下一步即可。

**验证**：

```bash
git branch --show-current
```

**预期结果**：输出 `main`。（空仓库还没有提交时，某些版本这里可能输出为空，属正常，第 5 步提交后必然显示 `main`。）

---

## 第 4 步 · 提交前安全检查

**目的**：在"公开"这件事发生之前，把不该见光的东西拦下来。**仓库一旦公开，历史里的内容即使后来删除也可能已被爬取。**

**检查 1：`.gitignore` 是否生效**

```bash
git status --porcelain
```

**预期结果**：列出待提交文件，且**不应出现** `.env`、`*.key`、`*.pem`、`node_modules/`、`*.log`。

**检查 2：有没有疑似密钥**

```bash
grep -rniE "(api[_-]?key|secret|token|password|passwd|BEGIN .*PRIVATE KEY)" --include="*.md" --include="*.txt" --include="*.json" . | grep -v "Token 用量" | head -20
```

**预期结果**：最多命中 README 里讨论"token"字样的说明性文字，**不应命中任何真实值**（形如 `sk-xxxx`、40 位十六进制串、`-----BEGIN RSA PRIVATE KEY-----`）。

**检查 3：大文件**

```bash
find . -type f -size +5M -not -path "./.git/*"
```

**预期结果**：无输出。GitHub 单文件超过 100MB 会被直接拒绝，超过 50MB 会警告。

**失败怎么办**：

- 检查 1 出现 `.env` → **立刻停下**，在 `.gitignore` 里加一行 `.env`，再跑 `git status --porcelain` 确认它消失。
- 检查 2 命中真实密钥 → 不要提交。把密钥移出目录或换成占位符后再继续；若已提交，需 `git rm --cached` 并从历史中清除（公开前处理，成本最低）。
- 检查 3 有大文件 → 加进 `.gitignore`，或改用 Git LFS。

---

## 第 5 步 · 首次提交

**目的**：把当前文件状态固化成第一个版本。Git 只推送"已提交"的内容，未提交的改动不会被推上去。

**命令**：

```bash
git add -A
git status --short
git commit -m "feat: 初始化 ultra-mode 技能包"
```

**预期结果**：

1. `git status --short` 每行以 `A`（新增）开头，例如：
   ```text
   A  .gitignore
   A  LICENSE
   A  README.md
   ...
   ```
2. `git commit` 输出类似：
   ```text
   [main (root-commit) 3f9a1c2] feat: 初始化 ultra-mode 技能包
    10 files changed, 640 insertions(+)
   ```
   关键是看到 `root-commit`（首个提交）和文件/行数统计。

**失败怎么办**：

- `Author identity unknown` / `Please tell me who you are` → 回第 2 步。
- `nothing to commit, working tree clean` → 文件已被上一次提交包含，或全被 `.gitignore` 忽略。用 `git status --ignored` 看是不是被忽略了。
- `fatal: not a git repository` → 目录不对，回第 1 步 / 第 3 步。
- 提交信息写错了（还没推送时）→ `git commit --amend -m "新信息"`。

**验证**：

```bash
git log --oneline
```

**预期结果**：一行，形如 `3f9a1c2 feat: 初始化 ultra-mode 技能包`。

---

## 第 6 步 · 在 GitHub 网页创建公开仓库

**目的**：在 GitHub 上建一个空仓库作为推送目标。**必须建空仓库**——如果建的时候勾了 "Add a README"，远端就已有提交，第 7 步推送会被拒绝。

**操作**：

1. 浏览器打开 <https://github.com/new>（或右上角 `+` → *New repository*）。
2. **Repository name** 填 `ultra-mode`。若提示已被占用，改成 `ultra-mode-skill`（记住这个名字，第 7 步要用）。
3. **Description** 填一句话，例如：
   `A portable Agent Skill: spend tokens on evidence, not words. 极限测试模式技能包。`
4. **Visibility** 选 **Public**（这是"公开"开关，选 Private 别人就看不到）。
5. **下面三个初始化选项全部不要勾**：
   - ☐ Add a README file
   - ☐ Add .gitignore
   - ☐ Choose a license
6. 点绿色 **Create repository**。

**预期结果**：跳转到一个标题为 *Quick setup — if you've done this kind of thing before* 的页面，中间显示仓库地址，下方是 `git remote add origin ...` 之类的命令提示。页面上**没有任何文件**（空仓库）。

**失败怎么办**：

- 名称报红 *already exists* → 换名（如 `ultra-mode-skill`），或先删掉自己旧的同名仓库。
- 不小心勾了 README → 不用删仓库，第 7 步用「方案 B：远端已有提交」处理。
- 页面提示需要验证邮箱 → 去邮箱点确认链接，再回来。

**复制仓库地址**：页面上 `HTTPS` 那栏，形如

```text
https://github.com/你的用户名/ultra-mode.git
```

点右侧复制图标。第 7 步要用。

---

## 第 7 步 · 关联远程并推送

**目的**：告诉本地仓库"远端在哪"，然后把提交送上去。

### 7.1 关联远程

```bash
git remote add origin https://github.com/你的用户名/ultra-mode.git
```

**预期结果**：无输出（成功即静默）。

**验证**：

```bash
git remote -v
```

**预期结果**：

```text
origin  https://github.com/你的用户名/ultra-mode.git (fetch)
origin  https://github.com/你的用户名/ultra-mode.git (push)
```

**失败怎么办**：

- `remote origin already exists` → 说明之前加过。改成：
  ```bash
  git remote set-url origin https://github.com/你的用户名/ultra-mode.git
  ```

### 7.2 推送

```bash
git push -u origin main
```

**首次推送会发生什么**：Git 会弹出**浏览器登录窗口**（Git Credential Manager，Git for Windows 默认自带）。点 *Sign in with your browser* → 在浏览器里授权 → 回到终端，推送自动继续。

**预期结果**：

```text
Enumerating objects: 14, done.
...
To https://github.com/你的用户名/ultra-mode.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

关键行：`* [new branch] main -> main`。`-u` 的作用是把本地 `main` 与远端 `main` 绑定，以后直接 `git push` 就行。

**失败怎么办**：

| 报错关键字 | 原因 | 处理 |
|---|---|---|
| `Support for password authentication was removed` | 在密码框里输了 GitHub 登录密码 | GitHub 不接受密码。改用下面「方案 A：个人访问令牌」 |
| `Authentication failed` / `403` | 令牌过期、权限不足、或登录了错误账号 | 清除旧凭据后重试（见方案 A 末尾） |
| `repository not found` | 地址错 / 仓库名不一致 / 没权限 | `git remote -v` 核对，注意用户名和仓库名大小写 |
| `rejected ... non-fast-forward` | 远端已有提交（建仓时勾了 README） | 见「方案 B：远端已有提交」 |
| `src refspec main does not match any` | 还没提交，或分支名不是 main | 回第 5 步；用 `git branch -a` 看实际分支名 |
| `Failed to connect to github.com port 443` | 网络/代理问题 | 见文末「网络需要代理怎么办」 |

#### 方案 A：个人访问令牌（PAT）

适用于：不想装凭据管理器、或用的是精简版 Git。

1. 打开 <https://github.com/settings/tokens> → *Generate new token (classic)*。
2. Note 填 `ultra-mode-push`；Expiration 选 30 天（够用，别选 No expiration）。
3. 勾选 scope：**`repo`**（公开仓库推送必需）。
4. 点 *Generate token*，**立刻复制**（`ghp_` 开头，只显示一次）。
5. 再次 `git push -u origin main`，在弹出框里：
   - Username：你的 GitHub 用户名
   - Password：**粘贴令牌**（不是登录密码）

清除旧凭据（Windows）：

```powershell
cmdkey /list | findstr github
cmdkey /delete:git:https://github.com
```

#### 方案 B：远端已有提交

如果第 6 步不小心勾了 README，远端有 1 个提交，本地也有 1 个，两者无共同祖先，推送会被拒。

**推荐做法**（把远端那个提交合并进来，保留双方内容）：

```bash
git pull origin main --allow-unrelated-histories --no-rebase
```

解决可能出现的 README 冲突（保留你本地的版本，两份内容合并），然后：

```bash
git add -A && git commit -m "merge: 合并远端初始化提交"
git push -u origin main
```

**备选做法**（远端那个空 README 你根本不要）：

```bash
git push -u origin main --force
```

`--force` 会用本地覆盖远端。**只在这个仓库是你刚建的、没有任何别人内容时使用**；对已有他人协作的仓库用 `--force` 会毁掉别人的提交。

---

## 第 8 步 · 核对公开状态与仓库信息

**目的**：确认"公开"真的生效，并把仓库页面收拾干净——别人点进来第一眼看到的就是这里。

**操作**（在 `https://github.com/你的用户名/ultra-mode` 页面）：

1. 标题下方应有 **Public** 标签。若是 `Private`，去 *Settings* → 拉到最底 *Danger Zone* → *Change repository visibility* → 选 **Public** → 按提示输入仓库名确认。
2. 右上 *About* 旁的齿轮 → 填 Description、Website（可留空）、Topics：
   ```text
   agent-skill, prompt-engineering, llm, deepseek, skill, ultra-mode
   ```
3. 确认许可证被识别：仓库页右侧应显示 **MIT license**（因为根目录有 `LICENSE` 文件，GitHub 会自动识别）。

**预期结果**：仓库页显示 Public、有描述、有 topics、右侧显示 MIT license。

---

## 第 9 步 · 验收

**目的**：用外部视角确认"别人看到的东西"是对的——不是确认"我推上去了"。

**操作**：

1. 开一个**无痕窗口**（Ctrl+Shift+N），访问 `https://github.com/你的用户名/ultra-mode`。
2. 确认：README 正常渲染（标题、标语引用块、表格都在）、目录树里有 `ultra-mode/SKILL.md`、`docs/GITHUB-UPLOAD-GUIDE.md`。
3. 点开 `ultra-mode/SKILL.md`，确认最上面的 YAML frontmatter 正常显示（`name: ultra-mode`）。
4. 点开 README 里的两个外链（`docs/testing.zh.md`、`docs/testing.md`），确认能打开到 DeepSeek 仓库对应文件——**出处链接必须真实可达，这是本仓库可信度的一部分。**

**预期结果**：四步全过。任何一步不符，回对应步骤修。

---

## 日常更新流程（以后每次改完都走这个）

```bash
cd "D:/work buddy/2026-10-30-12-30-26"
git status                       # 1. 看改了什么
git diff                         # 2. 看具体改动内容（提交前必看，防手滑）
git add -A                       # 3. 暂存
git commit -m "docs: 补充 XX 说明"  # 4. 提交
git push                         # 5. 推送（已设过 -u，不需要再写 origin main）
```

提交信息建议用前缀：`feat:` 新功能 / `fix:` 修复 / `docs:` 文档 / `refactor:` 重构 / `chore:` 杂项。

**撤销操作速查**：

| 想做什么 | 命令 |
|---|---|
| 撤销未暂存的修改 | `git restore <文件>` |
| 撤销已暂存（还没提交） | `git restore --staged <文件>` |
| 改最后一次提交信息（未推送） | `git commit --amend -m "新信息"` |
| 看历史 | `git log --oneline --graph` |
| 看某次提交改了什么 | `git show <提交号前7位>` |

---

## 可选 · 用 GitHub CLI 一条命令搞定

如果你愿意装官方命令行工具，第 6–7 步可以合成一条命令。

```powershell
winget install --id GitHub.cli -e
```

装完**重开终端**，然后：

```bash
gh auth login          # 选 GitHub.com → HTTPS → Login with a web browser，按提示输一次性码
gh repo create ultra-mode --public --source=. --remote=origin --push
```

第二条命令会：创建公开仓库 → 加 remote → 推送当前分支，一步完成。

**预期结果**：终端打印仓库地址，浏览器能打开。

> 注意：本机当前**未安装** `gh`（实测 `gh --version` 返回 command not found）。要用这条路线，先执行上面的 `winget install`。

---

## 常见错误速查表

| 报错原文（关键片段） | 含义 | 一句话解决 |
|---|---|---|
| `Author identity unknown` | 没配用户名/邮箱 | 回第 2 步 |
| `fatal: not a git repository` | 当前目录不是仓库 | `cd` 到项目根目录，或 `git init` |
| `nothing to commit` | 没有变更 / 全被忽略 | `git status --ignored` 查忽略规则 |
| `remote origin already exists` | 重复添加远程 | `git remote set-url origin <新地址>` |
| `src refspec main does not match any` | 没有提交或分支名不对 | 回第 5 步；`git branch -a` 查分支 |
| `repository not found` | 地址错或无权限 | `git remote -v` 核对用户名/仓库名 |
| `Authentication failed` | 用了密码而非令牌 | 方案 A |
| `non-fast-forward` / `rejected` | 远端有本地没有的提交 | 方案 B |
| `Failed to connect to github.com port 443` | 网络/代理 | 见下节 |
| `CONNECT tunnel failed` | 代理拒绝转发 github | 见下节 |
| 中文文件名显示成 `\344\270\255` | Git 转义了非 ASCII 路径 | `git config --global core.quotepath false` |
| `LF will be replaced by CRLF` | 换行符警告 | 警告不影响推送，可忽略；想消掉用 `git config --global core.autocrlf true` |

---

## 网络需要代理怎么办

如果公司网络或本地代理导致连不上 github.com：

```bash
# 设置代理（端口改成你自己的，常见 7890 / 1080 / 8080）
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# 验证是否连通（预期：打印一串 HEAD 哈希，不报错）
git ls-remote https://github.com/你的用户名/ultra-mode HEAD

# 不再需要时取消
git config --global --unset http.proxy
git config --global --unset https.proxy
```

**注意**：`git ls-remote` 成功才说明 Git 能连通。浏览器能打开 GitHub 不代表 Git 能连通——两者走的代理配置常常不是同一套。这是本指引里唯一必须靠"实测"而不是"看文档"确认的一步。
