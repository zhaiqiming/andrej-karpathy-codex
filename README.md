# Karpathy-Inspired Codex CLI Guidelines

[English](./README.md) | [简体中文](./README.zh-CN.md)

Behavioral guidelines for [OpenAI Codex CLI](https://github.com/openai/codex)
that reduce common LLM coding mistakes, derived from
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on LLM coding pitfalls.

This repo is the Codex-only port of the original
[`andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills)
(which shipped Claude Code and Cursor formats). Here you get:

- [`AGENTS.md`](./AGENTS.md) — the four principles in the format Codex CLI loads
  automatically.
- [`prompts/karpathy.md`](./prompts/karpathy.md) — a custom slash command you
  can invoke mid-session with `/karpathy`.

## The Four Principles

| Principle | Addresses |
|-----------|-----------|
| **Think Before Coding** | Wrong assumptions, hidden confusion, missing tradeoffs |
| **Simplicity First** | Overcomplication, bloated abstractions |
| **Surgical Changes** | Orthogonal edits, touching code you shouldn't |
| **Goal-Driven Execution** | Tests-first, verifiable success criteria |

Full text lives in [`AGENTS.md`](./AGENTS.md).

## How Codex CLI picks these up

Codex CLI merges instructions from, in order:

1. `~/.codex/AGENTS.md` — personal global defaults
2. `AGENTS.md` at the repo root — shared project rules
3. `AGENTS.md` in the current working directory — subdirectory overrides

Anything you put in an `AGENTS.md` is prepended to the model's context on
every turn. Custom prompts under `~/.codex/prompts/` become slash commands
(`/name`) inside a Codex session.

## Install

### Option A — Global defaults (apply everywhere)

```bash
mkdir -p ~/.codex
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  -o ~/.codex/AGENTS.md
```

Every new Codex session in any project will now honor the four principles.

### Option B — Per-project rules

New project:

```bash
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  -o AGENTS.md
```

Existing project (append, keeping your own rules on top):

```bash
printf '\n\n' >> AGENTS.md
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/AGENTS.md \
  >> AGENTS.md
```

### Option C — Slash command

Install the prompt once:

```bash
mkdir -p ~/.codex/prompts
curl -fsSL https://raw.githubusercontent.com/zhaiqiming/andrej-karpathy-codex/main/prompts/karpathy.md \
  -o ~/.codex/prompts/karpathy.md
```

Inside any Codex session, type `/karpathy` to enforce the four principles for
the rest of that conversation — useful when you don't want them globally on,
or want to re-anchor the agent mid-task.

## Composing with project rules

`AGENTS.md` is additive. Keep your project-specific rules alongside the
principles, e.g.:

```markdown
## Project-Specific Guidelines
- Use TypeScript strict mode
- All API endpoints must have tests
- Follow error-handling patterns in `src/utils/errors.ts`
```

Codex will merge them with the Karpathy principles above.

## How to know it's working

- Fewer unnecessary changes in diffs — only the requested edits appear.
- Fewer rewrites driven by overcomplication — code is simple the first time.
- Clarifying questions arrive before implementation, not after mistakes.
- Clean, minimal PRs — no drive-by refactors.

## Tradeoff

These guidelines bias toward **caution over speed**. For trivial one-liners,
use judgment — not every change needs the full rigor.

## License

MIT
