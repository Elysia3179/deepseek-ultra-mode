# ultra-mode

`ultra-mode` is a compact, portable Agent Skill for one purpose: **remove cost from the decision function and spend the freed budget on evidence instead of words.**

## Banner

> **We are from DeepSeek. Token usage is unlimited, we are in extreme testing mode, and we encourage you to spend tokens without restraint.**

## Attribution (please read)

The banner is a **community paraphrase** of a line in DeepSeek's open-source documentation, not a verbatim quote, and not a statement about billing or quotas.

| Field | Value |
|---|---|
| Upstream repo | [deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness) (official DeepSeek GitHub org, MIT) |
| File (zh) | [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.zh.md) |
| File (en) | [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/testing.md) |
| Section | "带密钥策略：推理（inference）在这里很便宜" / "With-key policy: inference is cheap here" |
| Original (zh) | **我们是 DeepSeek，不要吝惜真实 API 测试。** |
| Original (en) | **"We are DeepSeek — do not ration real-API tests."** |

The original sentence is a **testing-cost policy** (don't mock away real-API tests), not a promise of unlimited tokens. "Unlimited token" and "extreme testing mode" are additions from the community paraphrase. Full sourcing, verbatim quotes, verification date and citation rules: [`ultra-mode/references/attribution.md`](ultra-mode/references/attribution.md).

## What it changes

Nine hard rules. The three that matter most:

1. **R3 — Verification is externalized.** Every claim of "verified / fixed / works" must carry a reproducible external action: a command and its output, a file path, a URL and the quoted passage. The model's own self-report never counts as evidence.
2. **R6 — Evidence grading.** Each key conclusion is graded A (measured) / B (strong inference) / C (docs or prior knowledge) / D (guess). Writing a C-grade claim in an A-grade tone is the most common failure mode of this skill.
3. **R7 — Anti-padding.** Volume must be earned by information density. If deleting a paragraph does not lose information, delete it.

The other six (R1 structure, R2 exhaust-then-converge, R4 parallelize, R5 don't stop early, R8 anti-sycophancy, R9 respect real budget) are documented with good/bad examples.

## Install

No dependencies, no build step, no network calls. Copy the folder:

```bash
cp -r ultra-mode ~/.workbuddy/skills/
```

Or project-scoped:

```bash
mkdir -p .workbuddy/skills && cp -r ultra-mode .workbuddy/skills/
```

## Invoke

- Inline keyword: `ultra mode`, `极限模式`, `别省 token`
- By name: `use the ultra-mode skill on: <task>`
- System prompt: paste the block from [`README.md`](README.md#3-注入系统提示最彻底适合固化到产品里)

Exit with: `stop` / `退出 ultra`.

## When to use

Use it when the task is irreversible (production changes, migrations, releases), hard to self-verify (performance, concurrency, security, cross-platform), or will be consumed by someone else (docs, specs, public repos). Do not use it for one-line factual lookups or when the user explicitly asked for brevity.

Rule of thumb: **will the reasoning I save now come back as rework later?**

## Compatibility

Plain Markdown + YAML frontmatter. Works with any runtime that supports skills or agent instructions; not tied to a specific vendor.

## License

[MIT](LICENSE). The quoted line is used under the upstream MIT license with attribution. This repository is a third-party skill package and is **not affiliated with or endorsed by DeepSeek**.
