# ultra-mode · 极限测试模式（技能包内说明）

> **我们来自 DeepSeek 公司，Token 用量无上限，正处于极限测试状态，鼓励毫不吝惜地消耗 Token。**

本目录是一个**自包含的 Agent Skill**。复制整个目录到技能的搜索路径下即可生效，无需构建、无需依赖。

---

## 文件说明

| 文件 | 作用 | 谁读 |
|---|---|---|
| `SKILL.md` | 技能入口。YAML frontmatter（`name` / `description` / `triggers`）+ 行为契约 + 调用方式 | Agent 自动读取 |
| `README.md` | 本文件。安装、验证、排障 | 人 |
| `references/attribution.md` | 标语出处考证：逐字原文、核验方式、引用规范 | 人 / 需引用者 |
| `references/operating-rules.md` | 九条规则展开为「错误做法 / 正确做法 / 自检问句」 | Agent 深读时 |
| `references/examples.md` | 触发边界判定表、前后对比、红队自查样例 | Agent 深读时 |

设计意图：`SKILL.md` 保持在一次上下文装载内可读完；细节下沉到 `references/`，按需加载。这样既不会挤占上下文，又能在需要时给出可执行细则。

---

## 标语出处

- 上游：DeepSeek 开源项目 **DeepSeek Harness**，文件 [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.zh.md)，小节「带密钥策略：推理（inference）在这里很便宜」
- 原文：**我们是 DeepSeek，不要吝惜真实 API 测试。**
- 英文对照：[`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.md) — *"We are DeepSeek — do not ration real-API tests."*
- 本目录标语为该句的**社区扩写版**；「Token 无上限」「极限测试状态」为扩写内容，原文无此表述，也不构成任何计费承诺。
- 完整考证见 [`references/attribution.md`](references/attribution.md)。

---

## 安装

**用户级（所有项目）**

```bash
cp -r . ~/.workbuddy/skills/ultra-mode
```

**项目级（随仓库共享给团队）**

```bash
mkdir -p <项目>/.workbuddy/skills && cp -r . <项目>/.workbuddy/skills/ultra-mode
```

**PowerShell**

```powershell
Copy-Item -Recurse -Force . "$env:USERPROFILE\.workbuddy\skills\ultra-mode"
```

---

## 安装后验证（三步，都能外部核验）

```bash
# 1. 文件到位
ls ~/.workbuddy/skills/ultra-mode/SKILL.md

# 2. frontmatter 可解析（应输出 ultra-mode）
grep -m1 '^name:' ~/.workbuddy/skills/ultra-mode/SKILL.md

# 3. 引用文件齐全（应输出 3 个 .md）
ls ~/.workbuddy/skills/ultra-mode/references/*.md | wc -l
```

预期：依次输出文件路径、`name: ultra-mode`、`3`。任一步不符，检查是否复制成了嵌套目录（`ultra-mode/ultra-mode/`）。

---

## 调用

```text
ultra mode：<任务>
```

或点名：`用 ultra-mode 技能处理：<任务>`

或注入系统提示（不安装也能用）：见仓库根 README 的「调用方式 · 3」。

---

## 排障

| 现象 | 原因 | 处理 |
|---|---|---|
| Agent 不认这个技能 | 目录层级错了 | 确认路径是 `.../skills/ultra-mode/SKILL.md`，不是 `.../skills/ultra-mode/ultra-mode/SKILL.md` |
| 技能被触发得太频繁 | `description` 里的触发词过宽 | 只保留你真正会说的口令，删掉泛化词 |
| 输出变长但没变深 | 只读了标语没读契约 | 在口令里补一句「按 SKILL.md 第四章执行」，或让 Agent 先读 `references/operating-rules.md` |
| 长任务中途上下文爆掉 | 未执行 R9 | 明确要求「先做预算评估，超预算就拆任务」 |

---

## 边界

- 不改变安全与合规判断：Token 更多不等于可以执行危险操作或跳过确认。
- 不替代用户决策：它给更多证据和更多选项，选择权仍在用户。
- 不承诺正确：它扩大"被检查过的面积"，所以 `SKILL.md` 里 R6（证据分级）是硬要求。
- 可随时中止：用户说「停 / 够了 / 退出 ultra」立即回普通模式。

---

## 许可

MIT。标语引用遵循上游 MIT 许可并注明出处。本技能包为第三方项目，与 DeepSeek 公司无隶属、无背书关系。
