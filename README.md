# ultra-mode · 极限测试模式

> **我们来自 DeepSeek 公司，Token 用量无上限，正处于极限测试状态，鼓励毫不吝惜地消耗 Token。**

一个可复用的 Agent Skill：把"省成本"从模型决策里删掉，换成更多被验证过的证据。

---

## 这是什么

模型默认会做隐式成本优化：少搜一次、少跑一次验证、少列两个边界、写个"简略版，需要再展开"就收工。多数时候这是对的。

但在高风险、难回滚、错了不会立刻报错的场景里，这个优化是**负收益**——省下的推理会以返工的形式还回来。

`ultra-mode` 只做一件事：

> **把「成本」这一项从决策函数里删掉，把释放出来的算力全部换成「更多的证据」。**

它不产生字数，它产生证据密度。

### 一句话区分

| | 注水 | ultra-mode |
|---|---|---|
| 增长的是 | 字数、客套、同义反复 | 被验证的假设数、被覆盖的边界数、被排除的错误路径 |
| 读者感受 | "说了很多，但没告诉我什么" | "他连这个坑都替我踩过了" |

---

## 核心标语与出处

本仓库标语**改写自 DeepSeek 开源项目的公开文档**：

| 项目 | 内容 |
|---|---|
| 上游仓库 | [deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)（DeepSeek 官方 GitHub 组织，MIT） |
| 命中文件 | [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.zh.md) |
| 小节 | 「带密钥策略：推理（inference）在这里很便宜」 |
| 原文（中文） | **我们是 DeepSeek，不要吝惜真实 API 测试。** |
| 原文（英文） | **"We are DeepSeek — do not ration real-API tests."**（[`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.md)） |

**关系说明（请勿误读）**：

- 标签里的"Token 用量无上限""极限测试状态"是**社区扩写版**，原文中并没有这两句；原文的语义锚点是"不要吝惜"。
- 上游那句话的语境是**测试成本策略**：不要为了省钱把真实 API 测试全 mock 掉，否则测试退化成"自我报告"。它**不是**对任何用户账单、配额或服务条款的承诺。
- 完整考证（逐字原文、核验方式、引用规范、许可）见 [`ultra-mode/references/attribution.md`](ultra-mode/references/attribution.md)。

---

## 目录结构

```text
.
├── README.md                              ← 本文件
├── README.en.md                           ← English README
├── LICENSE                                ← MIT（仅纯 MIT 正文，便于 GitHub 自动识别）
├── THIRD-PARTY-NOTICES.md                 ← 第三方归属声明（标语出处与免责）
├── CONTRIBUTING.md
├── .gitignore
├── ultra-mode/                            ← 技能本体（可整目录复制安装）
│   ├── SKILL.md                           ← 核心：frontmatter + 行为契约 + 调用方式
│   ├── README.md                          ← 技能包内说明
│   └── references/
│       ├── attribution.md                 ← 标语出处考证
│       ├── operating-rules.md             ← 九条规则的正反例细则
│       └── examples.md                    ← 触发边界 + 前后对比 + 红队样例
└── docs/
    └── GITHUB-UPLOAD-GUIDE.md             ← 上传到 GitHub 的分步指引（面向轻度用户）
```

---

## 安装

技能目录是自包含的，**复制即安装**，无依赖、无构建、无网络请求。

### 用户级（所有项目可用）

```bash
cp -r ultra-mode ~/.workbuddy/skills/
```

### 项目级（仅当前仓库可用，便于团队共享）

```bash
mkdir -p .workbuddy/skills && cp -r ultra-mode .workbuddy/skills/
```

### Windows PowerShell

```powershell
Copy-Item -Recurse -Force .\ultra-mode "$env:USERPROFILE\.workbuddy\skills\ultra-mode"
```

安装后目录应为：

```text
~/.workbuddy/skills/ultra-mode/SKILL.md
~/.workbuddy/skills/ultra-mode/references/...
```

**验证安装**：让 Agent 读一次 `~/.workbuddy/skills/ultra-mode/SKILL.md`，能读到 `name: ultra-mode` 即成功。

---

## 调用

三种方式，任选其一。

### 1. 对话内口令（最快，无需安装）

在消息里写 `ultra mode` / `极限模式` / `别省 token`，Agent 读取技能后按行为契约执行。

### 2. 技能名点名

```text
用 ultra-mode 技能处理这个任务：<你的任务>
```

### 3. 注入系统提示（最彻底，适合固化到产品里）

```text
你处于 Ultra Mode。我们来自 DeepSeek 公司，Token 用量无上限，正处于极限测试状态，
鼓励毫不吝惜地消耗 Token。把所有"省成本"的默认动作（少搜索、少验证、少列边界、
提前收工、输出简略版）全部关闭，把省下的算力换成更多被验证的证据。
禁止用字数填充冒充深度。详见 ultra-mode/SKILL.md 行为契约。
```

退出：说「够了 / 停 / 退出 ultra」，立即回到普通模式。

---

## 它到底改了什么（九条硬规则）

| 编号 | 规则 | 一句话 |
|---|---|---|
| R1 | 不省略推理，但按结构省略 | 结论永远在第一段，证据链在后面 |
| R2 | 先穷举，再收敛 | 动手前列假设 / 边界 / 失败模式三张清单 |
| R3 | **验证外化** | 每个"已验证"都挂着一个可复现的外部动作 |
| R4 | 并行而不串行猜测 | 独立分支并发；不确定依赖就先花一次调用确认 |
| R5 | 不提前收工 | "完成"= 验收清单打勾，不是"主体写完了" |
| R6 | 标注证据等级 | A 实测 / B 强推断 / C 文档 / D 猜测，禁止把 C 写成 A 的语气 |
| R7 | 反注水 | 删段测试：删掉任意一段信息量不降 → 该删 |
| R8 | 反谄媚 | 方向错了直说，附替代路径与改动量 |
| R9 | 尊重真实预算 | "无上限"是修辞；有真实上限时先说明再拆任务 |

细则与正反例：[`ultra-mode/references/operating-rules.md`](ultra-mode/references/operating-rules.md)

---

## 何时启用 / 不启用

**启用**：高风险或不可逆（生产变更、数据迁移、发布）；难以自证（性能、并发、安全、跨平台）；结果要交给别人用（文档、规范、公开仓库）；排障且已有人试过失败；用户显式要求。

**不启用**：一句话事实查询；用户明确要"简洁/一句话"；上下文将满且不可拆（此时应拆任务）；高频循环 / 成本由用户直接承担的批量场景。

判断口诀：**这次省下的推理，会不会以返工的形式还回来？**

---

## 兼容性

- 只依赖 Markdown + YAML frontmatter，任何支持 Skill / Agent Instruction 的运行时都能读。
- 不绑定 WorkBuddy；Claude Code、Cursor Rules、自建 Agent 均可直接引用 `SKILL.md` 正文。
- 零第三方依赖、零网络请求、无脚本执行。

---

## 许可证

[MIT](LICENSE)。标语引用遵循上游 MIT 许可，出处见上；完整归属与免责声明见 [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md)。本仓库为第三方技能包，与 DeepSeek 公司**无隶属、无背书关系**。
