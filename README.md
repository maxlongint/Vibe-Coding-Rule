# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v3.0.0](https://img.shields.io/badge/Version-v3.0.0-blue)](./CHANGELOG.md)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

**一套可搭配 OpenSpec、CodeGraph、Superpowers 的完整 AI 协作规范。** 不绑定 Cursor、Claude Code 等特定 AI 工具，前后端项目都能用。

Vibe Coding Rule 通过可复制的行为约束、判断规则、操作流程和项目模板，约定 AI 与开发者在需求澄清、影响分析、实现、验证和交付中的协作方式。它属于规范与流程层，不是自动执行策略、拦截改动或收集验证证据的软件工具。

**版本记录：** [`CHANGELOG.md`](./CHANGELOG.md)

## 日常协作

| 你要做的事 | 看哪里 |
| --- | --- |
| 约束 AI 基本行为 | [`AGENTS.md`](./AGENTS.md) |
| 查技术栈、架构、编码约定 | [`docs/README.md`](./docs/README.md) 及其索引 |
| 存放 UI、页面、交互和视觉资料 | [`design/README.md`](./design/README.md) |
| 新功能 / 大改动 | [`新需求开发.md`](./新需求开发.md)（仅本规范仓，不接入业务项目） |
| 修改已有需求 | [`需求变更.md`](./需求变更.md)（仅本规范仓，不接入业务项目） |

收到需求时以 OpenSpec change 为唯一载体，并按 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md) 使用三项工具；确认后的口径、设计、计划和验证结论回填该 change，不另建平行口径。issue/PR 等只作输入来源。

## 工具链

接入本规范包后，**默认假定** OpenSpec、CodeGraph、Superpowers **均已成功安装并可调用**。职责与失败处理见 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md)。

三句话口径：假定已安装 → 执行中**需要使用**某项却不可用则中断并指明工具名 → 用文末「工具安装」提示词或官方文档修复，不静默降级、不擅自在接入流程里顺手安装。

官方来源（命令以当前文档为准）：

- OpenSpec：<https://github.com/Fission-AI/OpenSpec>
- CodeGraph：<https://github.com/colbymchenry/codegraph>
- Superpowers：<https://github.com/obra/superpowers#installation>

## 规范组成

```text
your-project/                         # 业务项目接入后的示意结构（非本规范仓目录清单）
├── AGENTS.md                         # 通用 AI 协作底线
├── docs/
│   ├── README.md                     # 项目长期规范索引
│   └── 规范/
│       ├── 代码设计原则.md
│       └── 工具协作.md               # OpenSpec / CodeGraph / Superpowers；含 UI 两阶段重组
├── design/
│   └── README.md                     # UI、页面、交互和视觉资料说明
├── .agents/
│   └── README.md                     # 项目级 skill 索引与安装入口
└── openspec/                         # 由 OpenSpec 在业务项目中生成；本规范仓不含
```

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI：**不伪造成功 · 完成前验证 · 改行为前先记录 · 不碰敏感信息 · 不擅自制造外部副作用 · 拿不准就问**。

项目长期规范放 `docs/`（见 [`docs/README.md`](./docs/README.md)）；UI、页面、交互和视觉资料放 `design/`（见 [`design/README.md`](./design/README.md)）；项目 skill 放 `.agents/skills/`（见 [`.agents/README.md`](./.agents/README.md)）；进行中需求只以 OpenSpec change 为唯一载体（issue/PR 等仅作输入来源）。

## 接入业务项目

本项目只提供一套完整接入方式：创建或更新 `AGENTS.md`，递归合并 `docs/` 下全部文件，处理 `design/README.md`，并创建或更新 `.agents/README.md`。

接入时应保留业务项目已有内容和索引，不删除项目自行新增的文件或目录。同路径规范文件应合并；`AGENTS.md` 或 `design/README.md` 不存在时创建，内容相同时保持不变，内容不同时展示差异摘要并暂停，由用户选择保留、覆盖或合并。不得修改或删除 `design/` 下其他文件。

### 受管文件改名与删除

升级时区分“规范仓库已移除或改名的受管文件”和“业务项目自行新增的文件”。本版本须按下列路径迁移，规则相同：

1. `docs/规范/可选工具协作.md` → `docs/规范/工具协作.md`（更名）
2. `docs/规范/UI任务重组.md` → 并入 `docs/规范/工具协作.md` §5 后删除（不再单独维护）

处理方式：

- 旧文件与上一版本原文一致时，落盘新内容、更新 `docs/README.md` 索引后删除旧文件；
- 旧文件含业务项目自定义内容时，先展示差异，把仍有效的项目内容合并到 `工具协作.md` 或其他合适载体，等待用户确认后再删除旧文件；
- 无法判断来源或无法安全迁移时停止；不得保留两份当前工具规范或两份 UI 两阶段规则，也不静默删除。

以后发生受管文件改名或删除时，也必须在对应版本的迁移说明中明确列出，不能用“保留项目已有文件”跳过迁移。

本版本（v3.0.0）从接入范围移除并停止维护业务项目侧的下列文件（规范仓内仍保留工作流原文，供本仓库查阅）：

- `流程公共约定.md`、`快速开始.md`：已删除；业务项目残留且仅为旧迁移说明/导航或与上一版本原文一致时可删；含自定义内容时先展示差异再确认。
- `新需求开发.md`、`需求变更.md`：不再复制到业务项目；业务项目若曾接入且内容与规范仓上一版本原文一致，升级时可删除；含项目自定义流程时先展示差异并确认后再处理。

下列文件**仅存在于本规范仓**，不属于接入/升级受管文件，不得复制到业务项目：`规则干条.md`、`新需求开发.md`、`需求变更.md`、本仓库 `README.md` / `CHANGELOG.md` / `CONTRIBUTING.md` 等开源元文档。

在业务项目中把下面的提示词发给 AI：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

只处理受管文件：AGENTS.md、docs/ 下全部文件、design/README.md、.agents/README.md。保留项目自有内容与索引；不复制其他文件。
design/README.md：不存在则创建，相同跳过，不同则展示差异并由我选择；不得改 design/ 下其余文件。
默认三项工具已安装；需要用却不可用时中断并指明工具，勿自动安装或静默降级。
完成后列出实际更新、未更新项和剩余风险。
```

## 升级已接入版本

```text
请升级当前业务项目已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

比对 AGENTS.md 中的版本与规范仓最新版；同版本仍核对受管文件是否齐全一致，一致则说明已是最新。
需要升级时：按规范仓 README「受管文件改名与删除」迁移，再只合并 AGENTS.md、docs/、design/README.md、.agents/README.md；保留项目自有内容；不复制非受管文件。
design/README.md 与 AGENTS.md：不存在则创建，相同跳过，不同则展示差异并由我选择；不得改 design/ 下其余文件。
默认三项工具已安装；需要用却不可用时中断并指明工具。完成后列出更新、迁移、删除、未更新项和剩余风险。
```

## 工具安装（首次或修复时）

接入规范包**不会**安装这三项工具。尚未安装，或协作中被告知某项不可用时，把对应提示词发给 AI。

**OpenSpec**（需 CLI + 当前项目初始化 + AI 侧接入）：

```text
请按 https://github.com/Fission-AI/OpenSpec 当前官方说明，安装 OpenSpec CLI，在当前项目初始化，并接入我正在使用的 AI 工具。
需要重启 AI 才能加载时说明原因后停止，由我重启；不要假装已可用。完成后说明验证方式。
```

**CodeGraph**（需安装 + AI/MCP 接入 + 当前项目索引）：

```text
请按 https://github.com/colbymchenry/codegraph 当前官方说明，安装 CodeGraph，完成我正在使用的 AI 工具接入，并在当前项目执行索引初始化。
需要重启 AI 才能加载 MCP 时说明原因后停止，由我重启；不要假装已可用。完成后说明验证方式。
```

**Superpowers**（按 AI 工具安装到用户侧；无项目级初始化）：

```text
请按 https://github.com/obra/superpowers#installation 中与我当前 AI 工具对应的方式安装 Superpowers。
需要重启 AI 才能加载 skill 时说明原因后停止，由我重启；不要假装已可用。完成后说明验证方式。
```

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线，以及可复制的目录与流程模板）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
