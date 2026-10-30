# 标语出处考证（Attribution）

本文记录本仓库核心标语的来源、原文、上下文与引用边界。**任何人引用本仓库标语时，请一并引用本文的出处链接。**

---

## 1. 标语原文（本仓库）

> **我们来自 DeepSeek 公司，Token 用量无上限，正处于极限测试状态，鼓励毫不吝惜地消耗 Token。**

## 2. 上游原始出处

| 项目 | 内容 |
|---|---|
| 上游项目 | DeepSeek Harness（简称 DSH），"Everything is a Plugin." |
| 上游仓库 | <https://github.com/deepseek-ai/deepseek-harness> |
| 上游组织 | `deepseek-ai`（DeepSeek 官方 GitHub 组织） |
| 默认分支 | `master` |
| 命中文件（中文） | `docs/testing.zh.md` → <https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.zh.md> |
| 命中文件（英文） | `docs/testing.md` → <https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.md> |
| 命中位置 | 小节标题「带密钥策略：推理（inference）在这里很便宜」 |
| 上游许可证 | MIT |

### 2.1 中文原文（逐字引用）

```text
## 带密钥策略：推理（inference）在这里很便宜

我们是 DeepSeek，不要吝惜真实 API 测试。无密钥测试只能证明底层通路；只有带密钥运行
才能证明 agent（智能体）能对接真实模型正常工作。覆盖文件写入提示词、包含多个轮次的
对话、工具使用和流中取消。价值最高的是冒烟测试：启动已交付的 dsh profile、发送一条
提示词，并检查外部世界；它们能捕获「单元测试全绿、产品却坏了」这一类 mock 无法发现
的问题（事故复盘 0001）。自动跳过让无密钥 CI 和无密钥贡献者不受阻塞；它不是成本信号。
```

### 2.2 英文原文（逐字引用）

```text
We are DeepSeek — do not ration real-API tests. A no-key test proves plumbing; only a
with-key run proves the agent works against a real model.
```

### 2.3 核验方式与时间

- 核验日期：2026-10-30
- 核验方式：抓取上述两个 URL 的正文，确认小节标题与句子逐字存在
- 溯源路径：先由检索定位到该文档，再回到 `deepseek-ai` 组织仓库确认文件路径与分支名（默认为 `master`，不是 `main`）

---

## 3. 本仓库标语与原文的关系（重要）

本仓库标语 **不是逐字引用**，而是社区流传的**扩写版 / 口头版**。对应关系如下：

| 原文要素 | 本仓库标语中的对应 |
|---|---|
| 「我们是 DeepSeek」 | 「我们来自 DeepSeek 公司」 |
| 「不要吝惜真实 API 测试」 | 「鼓励毫不吝惜地消耗 Token」 |
| 原文未出现「Token 无上限」 | 「Token 用量无上限」（社区扩写） |
| 原文未出现「极限测试」 | 「正处于极限测试状态」（社区扩写） |

**语义锚点**是「不要吝惜」。**扩写部分是修辞**，用于表达「不要因为成本而降低努力程度」这一行为意图。

因此：

- 引用时请写成「**改写自** DeepSeek Harness 文档中的『我们是 DeepSeek，不要吝惜真实 API 测试』」，而不是「DeepSeek 官方说 Token 无上限」。
- 不要把扩写部分当作 DeepSeek 对计费、配额或服务条款的任何承诺。上游那句话的语境是**测试成本策略**：不要为了省钱而把真实 API 测试全部 mock 掉，否则测试会变成"自我报告"。

---

## 4. 上游那一段到底在说什么（避免误读）

上游文档的这段话不是「鼓励烧钱」，而是一条**测试有效性**原则：

1. 无密钥（全 mock）的测试只能证明「管道通」，不能证明「模型真的能干活」。
2. 只有带真实密钥跑一遍，才能证明 agent 对接真实模型时正常工作。
3. 上游之所以敢这么说，是因为它自己判断「推理在这里很便宜」——**这是成本判断的结果，不是成本判断的取消**。
4. 同一份文档紧接着还有一条相反方向的约束：「只 mock 开销高或不确定的边界；下游一切保持真实。」两条合起来才是完整策略：**该省的省，该花的花，省不能省掉有效性。**

本仓库把这条原则从「测试」迁移到「Agent 工作方式」：不要为了省 token 而省掉验证，因为省掉的验证会以返工的形式还回来。

---

## 5. 引用规范（给二次使用者）

推荐写法：

```markdown
> 我们来自 DeepSeek 公司，Token 用量无上限，正处于极限测试状态，鼓励毫不吝惜地消耗 Token。
>
> 改写自 DeepSeek Harness 文档（MIT）：
> https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.zh.md
```

不推荐写法：

- 「DeepSeek 官方声明 Token 无上限」（把修辞当承诺）
- 「DeepSeek 内部提示词泄露」（把文档段落说成泄露）
- 只写标语不给链接（无法核验）

---

## 6. 上游许可与合规

- 上游仓库为 MIT 许可证，允许引用、修改、再分发，需保留版权与许可声明。
- 本仓库为**第三方技能包**，由社区维护，**与 DeepSeek 公司无隶属、无背书关系**。
- 本仓库不复制上游源代码，仅引用一段公开文档文字并注明出处；`ultra-mode` 的全部行为规则为本仓库原创。
- 若上游后续修改该段文字或调整许可证，以本文件记录的核验日期版本为准，并应更新本文。
