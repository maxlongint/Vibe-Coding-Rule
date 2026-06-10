# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

A **tool-agnostic** collaboration spec for AI-assisted development. **Copy one core file and go.**

Works across frontend, backend, full-stack, and scripting projects. No dependency on a specific AI product or plugin.

**中文:** [`README.md`](./README.md) · **Quick start:** [`快速开始.md`](./快速开始.md) (Chinese) · **Changelog:** [`CHANGELOG.md`](./CHANGELOG.md)

## Usage

### Recommended project layout

```text
your-project/
├── AGENTS.md
├── docs/README.md          # long-lived project specs (template in this repo)
├── .agents/README.md       # skill index (template in this repo)
├── .agents/skills/<name>/SKILL.md
└── openspec/changes/       # optional, when using OpenSpec
```

`docs/` holds stable conventions; `openspec/changes/` holds in-flight requirements. See [`docs/README.md`](./docs/README.md) (Chinese).

### Setup

1. Copy [`AGENTS.md`](./AGENTS.md) to your project root (diff first if one already exists).
2. Copy [`docs/README.md`](./docs/README.md) into your project's `docs/`.
3. Copy [`.agents/README.md`](./.agents/README.md) into your project's `.agents/`.
4. See [`快速开始.md`](./快速开始.md) for three adoption paths (Chinese).

## What it covers

[`AGENTS.md`](./AGENTS.md) enforces six non-negotiable rules for AI collaborators:

1. **No fake success** — expose real failures; don't mask them with defaults, empty objects, mocks, or "fallback success".
2. **Verify before claiming done** — report results; if you can't verify, say why and what risk remains.
3. **Record before changing behavior** — traceable notes before altering APIs, data, permissions, flows, or acceptance criteria.
4. **Don't touch secrets** — no reading, logging, committing, or spreading keys, credentials, or unsanitized data.
5. **Don't ship without explicit ask** — no commit / push / merge / deploy unless the user clearly requests it.
6. **Ask when unsure** — stop and confirm instead of guessing.

## What it does *not* cover

Stack-specific specs belong under **`docs/`**; project skills live under **`.agents/skills/<skill-name>/`** (`SKILL.md`), indexed in **`.agents/README.md`**. None of this goes in `AGENTS.md` or tool-specific config. That keeps this spec portable and small.

## Optional toolchain

`AGENTS.md` alone is enough. For fuller requirement governance and execution discipline, the recommended combo is **OpenSpec (primary) + CodeGraph + Superpowers (supporting)**. None of them replace the `AGENTS.md` floor; without them, fall back to issues/PRs and manual analysis.

- New features: [`新需求开发.md`](./新需求开发.md)
- Requirement changes: [`需求变更.md`](./需求变更.md)
- Shared conventions: [`流程公共约定.md`](./流程公共约定.md)

Install prompts (send to your AI in the **business project**, not this rules repo) are in [`README.md`](./README.md#通过-ai-安装).

## Contributing

See [`CONTRIBUTING.md`](./CONTRIBUTING.md). Keep changes focused: universal AI collaboration floors only — not company-specific process or framework style guides.

## License

[MIT](./LICENSE) — free to use in commercial and non-commercial projects; keep the copyright notice.
