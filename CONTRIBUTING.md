# 贡献指南

## 这个仓库接受什么样的改动

`ultra-mode` 的目标很窄：**让 Agent 把算力花在证据上，而不是字数上**。符合这个目标的改动都欢迎。

### 欢迎

- 新增/修正行为规则的正反例（`ultra-mode/references/operating-rules.md`）
- 补充触发边界的判定案例（`ultra-mode/references/examples.md`），尤其是"看起来该触发但其实不该"的场景
- 修正标语出处的表述、补充核验信息（`ultra-mode/references/attribution.md`）
- 不同运行时的适配说明（Claude Code / Cursor / 自建 Agent 等）
- 中英文档的表述改进

### 不接受

- 把规则改成"输出越长越好"——本技能反对注水，长度必须由信息密度挣来
- 删除或弱化 R3（验证外化）与 R6（证据分级）——这两条是技能的地基
- 把标语改写为"DeepSeek 官方承诺 Token 无上限"之类的表述——这是对上游文档的误读
- 引入第三方依赖、构建步骤或网络请求——技能必须保持"复制即用"

## 提交前自查

按 `ultra-mode/SKILL.md` 第六章的交付自检清单走一遍。特别是：

- [ ] 改动的文件在预期位置，没有误加临时文件
- [ ] 新增的示例是真实场景，不是为凑数编的
- [ ] 中英文档若同步修改，两边语义一致
- [ ] 引用上游内容时保留了出处链接

## 提交信息格式

```text
<type>: <简短描述>

type ∈ feat | fix | docs | refactor | chore
```

例：`docs: 补充 PAT 认证失败的排查步骤`

## 关于标语

核心标语改写自 DeepSeek Harness 文档（MIT），原文为「我们是 DeepSeek，不要吝惜真实 API 测试。」详见 [`ultra-mode/references/attribution.md`](ultra-mode/references/attribution.md)。修改标语相关文字时，请同步更新该文件与根目录 `README.md` 的出处段落。

## 许可

贡献即表示同意以 [MIT](LICENSE) 许可发布。
