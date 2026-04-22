[English](./README.md) | [简体中文](./README.zh-CN.md)

# 简体中文版
面向 [OpenAI Codex CLI](https://github.com/openai/codex) 的行为规范，用来减少
LLM 在编程时常见的错误。这些原则整理自
[Andrej Karpathy 对 LLM 编码陷阱的观察](https://x.com/karpathy/status/2015883857489522876)。

这个仓库是原始项目
[`andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills)
的 Codex 专用移植版本（原项目还提供 Claude Code 和 Cursor 格式）。这里主要包含：

- [`AGENTS.md`](./AGENTS.md)：Codex CLI 会自动加载的四条原则。
- [`prompts/karpathy.md`](./prompts/karpathy.md)：可在会话中通过 `/karpathy`
  调用的自定义斜杠命令。

## 四条核心原则

| 原则 | 主要解决的问题 |
|------|----------------|
| **Think Before Coding（先思考，再动手）** | 错误假设、隐性困惑、缺少权衡 |
| **Simplicity First（简单优先）** | 过度设计、抽象膨胀 |
| **Surgical Changes（外科手术式修改）** | 无关改动、误碰不该改的代码 |
| **Goal-Driven Execution（目标驱动执行）** | 缺少验证、成功标准不明确 |

完整内容见 [`AGENTS.md`](./AGENTS.md)。

## Codex CLI 如何加载这些规则

Codex CLI 会按以下顺序合并指令：

1. `~/.codex/AGENTS.md`：你个人的全局默认规则
2. 仓库根目录下的 `AGENTS.md`：项目共享规则
3. 当前工作目录下的 `AGENTS.md`：子目录级覆盖规则

放进 `AGENTS.md` 的内容会在每次对话时自动加入模型上下文。
放在 `~/.codex/prompts/` 下的自定义 prompt，会在 Codex 会话中变成斜杠命令
（例如 `/karpathy`）。

## 安装方式

### 方案 A：全局默认规则（所有项目生效）

```bash
mkdir -p ~/.codex
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  -o ~/.codex/AGENTS.md
```

设置后，你在任何项目中新开的 Codex 会话都会默认遵循这四条原则。

### 方案 B：按项目启用

新项目：

```bash
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  -o AGENTS.md
```

已有项目（追加到现有规则后面，并保留你自己的规则在前）：

```bash
printf '\n\n' >> AGENTS.md
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  >> AGENTS.md
```

### 方案 C：作为斜杠命令安装

一次性安装该 prompt：

```bash
mkdir -p ~/.codex/prompts
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/prompts/karpathy.md \
  -o ~/.codex/prompts/karpathy.md
```

之后在任意 Codex 会话中输入 `/karpathy`，即可让这四条原则在当前会话剩余阶段持续生效。
这适合你不想全局开启时使用，或者在任务中途重新给 agent “校准”行为。

## 如何与项目规则组合使用

`AGENTS.md` 是可叠加的。你可以把项目专属规则和这些原则一起保留，例如：

```markdown
## Project-Specific Guidelines
- Use TypeScript strict mode
- All API endpoints must have tests
- Follow error-handling patterns in `src/utils/errors.ts`
```

Codex 会把这些项目规则与上面的 Karpathy 原则合并使用。

## 如何判断它生效了

- diff 中无关改动更少：只出现你真正要求的修改。
- 因过度设计导致的返工更少：第一次实现就更简单直接。
- agent 会先澄清，再编码，而不是出错后才补问。
- PR 更干净：没有顺手做的大量无关重构。

## 取舍

这些规则整体上更偏向 **谨慎优先，而不是速度优先**。
对于特别简单的一行改动，可以灵活判断，不必每次都走完整套严格流程。

## 许可证

MIT
