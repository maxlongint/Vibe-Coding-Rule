# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v1.1.4](https://img.shields.io/badge/Version-v1.1.4-blue)](./CHANGELOG.md)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

**一套可复制到业务项目的 AI 协作规范。** 不绑定 Cursor、Claude Code 等特定工具，前后端项目都能用。

把本仓库的 [`AGENTS.md`](./AGENTS.md) 复制到你的项目根目录，就能约束 AI：不散提交、不伪造成功、改接口前先留记录、完成前要验证等。项目自己的技术栈、命令、架构与业务规则写在 `docs/`，**不要**塞进 `AGENTS.md`。

**版本记录：** [`CHANGELOG.md`](./CHANGELOG.md) · **落地深度选型：** [`快速开始.md`](./快速开始.md)

## 这是什么

| 在本仓库 | 复制到业务项目后 |
| --- | --- |
| 通用模板：`AGENTS.md`、`docs/`、`.agents/` | 你项目里的 AI 底线与长期约定 |
| 可选流程文档（OpenSpec / CodeGraph / Superpowers） | 按需选用，不是必需品 |

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI：**不伪造成功 · 完成前验证 · 改行为前先记录 · 不碰敏感信息 · 不擅自提交发布 · 拿不准就问**。

项目规范放 `docs/`（见 [`docs/README.md`](./docs/README.md)）；项目 skill 放 `.agents/skills/`（见 [`.agents/README.md`](./.agents/README.md)）。进行中需求放 `openspec/changes/`（启用 OpenSpec 时）或 issue / PR。

## 怎么用

### 1. 复制到业务项目

```text
your-project/
├── AGENTS.md              # 必填：从本仓库复制
├── docs/                  # 可选：项目长期规范（含 docs/README.md 模板）
├── .agents/               # 可选：项目 skill 目录
└── openspec/              # 可选：启用 OpenSpec 时的进行中需求
```

**最小落地：** 只复制 `AGENTS.md` 即可。  
**常规落地：** 再加 `docs/` 与 `.agents/README.md`。  
**完整流程：** 见 [`快速开始.md`](./快速开始.md) 三种场景。

### 2. 日常协作

下列流程文档在**本规范仓库**内查阅（或按需复制到业务项目）；只复制了 `AGENTS.md` 的项目根目录默认没有这些文件。

| 你要做的事 | 看哪里 |
| --- | --- |
| 约束 AI 基本行为 | 项目根目录 `AGENTS.md` |
| 查技术栈、架构、编码约定 | 项目 `docs/` |
| 新功能 / 大改动 | [`新需求开发.md`](./新需求开发.md) |
| 改已有需求 | [`需求变更.md`](./需求变更.md) |
| OpenSpec / Superpowers 命令怎么写 | [`流程公共约定.md`](./流程公共约定.md) |
| 代码设计取舍（复制 `docs/` 时） | [`docs/规范/代码设计原则.md`](./docs/规范/代码设计原则.md) |

### 3. 用 AI 接入或升级 Vibe Coding Rule

在**业务项目**（不是本规范仓库）里把提示词发给 AI。

**首次接入**（不安装可选工具链）：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

请参考规范仓库 README，在当前项目中创建或更新：
1. AGENTS.md
2. docs/（复制或合并规范仓库 docs/ 下的通用规范模板）
3. .agents/README.md

要求：只改当前项目；已有文件先 diff 并给出合并方案，等我确认后再写入；不要把项目特有规则写进 AGENTS.md；完成后列出实际更新文件和剩余风险。本次不安装 OpenSpec、CodeGraph、Superpowers。
```

**升级已接入版本**：

```text
请为当前业务项目更新已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

请参考规范仓库最新版本，对比当前项目中的：
1. AGENTS.md
2. docs/（包含 docs/README.md 及 docs/ 下明确来自 Vibe Coding Rule 的通用规范模板）
3. .agents/README.md
4. 其他明确来自 Vibe Coding Rule 的文档或模板

要求：只改当前项目；先说明差异、建议更新项和合并方案，等我确认后再写入；保留项目自己的技术栈、命令、架构、业务规则、经验包索引和 skill 索引；完成后列出实际更新文件、未更新项和剩余风险。
```

### 4. 可选工具链（通过 AI 安装）

只用 `AGENTS.md` 已经够用。需要规格化需求、影响分析与执行纪律时，在**业务项目**按顺序安装 **OpenSpec → CodeGraph → Superpowers**（勿在本规范仓库操作）。OpenSpec / CodeGraph 先发「安装」再发「初始化」提示词；Superpowers 只需安装（无项目级初始化）。

口诀：**AGENTS 定底线 · OpenSpec 管需求 · CodeGraph 管影响 · Superpowers 管方法**。分工与 fallback 见 [`快速开始.md` 场景三](./快速开始.md#场景三全套工具链openspec--codegraph--superpowers)。

**执行安装 / 初始化命令时，何时需要重启 AI 工具**

重启不是插件安装或初始化完成后的统一提醒，也不能只按工具名称预先判定。执行每段提示词时，AI 应根据官方文档和当前 AI 工具的接入方式逐步判断：下一步是否依赖本轮刚注册的 MCP、斜杠命令、技能等能力，以及当前会话能否动态加载这些能力。

- 当前会话可以继续使用所需能力：直接完成后续初始化和验证，不额外要求重启。
- 下一步必须使用尚未加载的能力：先完成重启前可安全执行的步骤，明确说明已完成项、剩余步骤和重启原因，然后停下；用户完整退出并重新打开当前 AI 编码工具后，再在新会话中继续初始化和验证。
- CLI 初始化可以独立完成、只有后续在 AI 中首次调用 MCP / 命令 / 技能时才需要加载：先完成 CLI 初始化，在首次调用前再按官方说明判断是否重启，不把重启误写成 CLI 初始化的前置条件。

这里的重启指完整退出并重新打开当前 AI 编码工具（如 Cursor / Claude Code），不是只刷新聊天窗口。AI 无法替用户重启，也不得在需要重启时假装新能力已加载。

**OpenSpec**

前提：Node.js ≥ 20.19.0；本仓库推荐在业务项目根目录以项目依赖安装（`npm install -D @fission-ai/openspec`，勿全局安装），具体以官方文档与当前项目包管理器为准。

安装：

```text
请从 https://github.com/Fission-AI/OpenSpec 为当前项目安装 OpenSpec。
先查看官方 README，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
安装完成后验证已可用，并提醒我继续发送初始化提示词。
```

初始化（安装完成后另发一条）：

```text
请按 https://github.com/Fission-AI/OpenSpec 官方文档，为当前项目完成 OpenSpec 初始化（接入当前 AI 工具、使 /opsx 工作流可用）。
逐步判断下一步是否依赖本轮刚写入、但当前会话尚未加载的斜杠命令或技能；若必须完整重启 AI 工具才能继续，先说明已完成项、剩余步骤和原因，然后停下等我重启，重启后再继续。
完成后验证初始化结果；不要把安装或文件写入成功当成斜杠命令已经加载。
```

**CodeGraph**

前提：Windows / macOS / Linux；当前 AI 工具须支持 MCP；本仓库推荐以项目依赖安装（`npm install -D @colbymchenry/codegraph`），具体以官方文档与当前项目包管理器为准。

安装：

```text
请从 https://github.com/colbymchenry/codegraph 为当前项目安装 CodeGraph。
先查看官方 README 和安装说明，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
安装完成后验证已可用，并提醒我继续发送初始化提示词。
```

初始化（安装完成后另发一条）：

```text
请按 https://github.com/colbymchenry/codegraph 官方文档，为当前项目完成 CodeGraph 初始化（含接入当前 AI 工具，以及为本仓库建立索引）。
逐步判断下一步是否需要调用本轮刚注册的 CodeGraph MCP 或其他当前会话尚未加载的能力；若必须完整重启 AI 工具才能继续，先说明已完成项、剩余步骤和原因，然后停下等我重启，重启后再继续。
如果建立索引的 CLI 命令不依赖 AI 工具加载 MCP，则先完成索引，不要把重启误当成 CLI 初始化的前置条件；在首次调用 MCP 前再按官方说明判断是否需要重启。
完成后验证索引和接入结果；不要假装 MCP 已加载。
```

**Superpowers**

前提：安装在当前 AI 工具侧（用户级），**无项目级初始化**；例：Cursor 用 `/add-plugin superpowers`（详见 [官方 Installation](https://github.com/obra/superpowers#installation)）。

安装：

```text
请从 https://github.com/obra/superpowers#installation 安装 Superpowers。
先查看官方 README 和 Installation 章节，确认适合当前 AI 工具的安装方式。
安装后先按官方说明判断当前 AI 工具能否动态加载新技能；若首次调用技能前必须完整重启 AI 工具，说明安装结果和重启原因后停下等我重启，重启后再验证技能可用性。
不要把插件安装成功当成技能已经加载。
```

装齐后按 [`新需求开发.md`](./新需求开发.md) / [`需求变更.md`](./需求变更.md) 推进。

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
