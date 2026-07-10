# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v1.1.3](https://img.shields.io/badge/Version-v1.1.3-blue)](./CHANGELOG.md)
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

只用 `AGENTS.md` 已经够用。需要规格化需求、影响分析与执行纪律时，在**业务项目**按顺序安装 **OpenSpec → CodeGraph → Superpowers**（勿在本规范仓库操作）。把下面提示词分别发给 AI 即可。

口诀：**AGENTS 定底线 · OpenSpec 管需求 · CodeGraph 管影响 · Superpowers 管方法**。分工与 fallback 见 [`快速开始.md` 场景三](./快速开始.md#场景三全套工具链openspec--codegraph--superpowers)。

**OpenSpec**

前提：Node.js ≥ 20.19.0；在业务项目根目录以项目依赖安装（`npm install -D @fission-ai/openspec`，勿全局安装）。

```text
请从 https://github.com/Fission-AI/OpenSpec 为当前项目安装 OpenSpec。
先查看官方 README，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
如果需要执行网络下载或修改项目文件，请先向我申请权限。
安装完成后验证 CLI 可用。
```

安装后初始化：`npx openspec init` → `npx openspec --version`；（可选）`npx openspec config profile` → `npx openspec update`。

**CodeGraph**

前提：Windows / macOS / Linux；以项目依赖安装（`npm install -D @colbymchenry/codegraph`）；当前 AI 工具须支持 MCP；CLI 安装后需新开终端。

```text
请从 https://github.com/colbymchenry/codegraph 为当前项目安装 CodeGraph。
先查看官方 README 和安装脚本，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，并说明是否需要重启当前 AI 工具。
```

安装后初始化：新开终端 `npx codegraph install` → 业务项目根目录 `npx codegraph init -i`。

**Superpowers**

前提：安装在当前 AI 工具侧（用户级）；例：Cursor 用 `/add-plugin superpowers`（详见 [官方 Installation](https://github.com/obra/superpowers#installation)）。

```text
请从 https://github.com/obra/superpowers#installation 安装 Superpowers。
先查看官方 README 和 Installation 章节，确认适合当前 AI 工具的安装方式。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
```

安装后初始化：重启 AI 工具 → 确认 `$brainstorming` 等技能可触发。

装齐后按 [`新需求开发.md`](./新需求开发.md) / [`需求变更.md`](./需求变更.md) 推进。

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
