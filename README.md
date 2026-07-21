# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v2.1.1](https://img.shields.io/badge/Version-v2.1.1-blue)](./CHANGELOG.md)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

**一套可搭配 OpenSpec、CodeGraph、Superpowers 的完整 AI 协作规范。** 不绑定 Cursor、Claude Code 等特定 AI 工具，前后端项目都能用。

Vibe Coding Rule 通过可复制的行为约束、判断规则、操作流程和项目模板，约定 AI 与开发者在需求澄清、影响分析、实现、验证和交付中的协作方式。它属于规范与流程层，不是自动执行策略、拦截改动或收集验证证据的软件工具。

**版本记录：** [`CHANGELOG.md`](./CHANGELOG.md) · **快速开始：** [`快速开始.md`](./快速开始.md)

## 可选工具链

OpenSpec、CodeGraph、Superpowers 可以组合使用，但都按需启用。详细启用门禁、职责、组合方式、可用性检查和失败处理统一见 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md)；本 README 只提供官方来源和接入场景。

官方来源：

- OpenSpec：<https://github.com/Fission-AI/OpenSpec>
- CodeGraph：<https://github.com/colbymchenry/codegraph>
- Superpowers：<https://github.com/obra/superpowers#installation>

安装方式、运行时要求和 AI 接入形式可能变化。需要安装或初始化已选择的工具时，应核对当前官方文档，不照搬旧命令。

## 规范组成

```text
your-project/
├── AGENTS.md                         # 通用 AI 协作底线
├── 快速开始.md                       # 默认工具模型与工作入口
├── 新需求开发.md                     # 新需求工作流
├── 需求变更.md                       # 需求变更工作流
├── docs/
│   ├── README.md                     # 项目长期规范索引
│   └── 规范/
│       ├── 代码设计原则.md
│       ├── 工具协作.md               # OpenSpec / CodeGraph / Superpowers
│       └── UI任务重组.md             # UI 两阶段任务的基线、映射与核对
├── design/
│   └── README.md                     # UI、页面、交互和视觉资料说明
├── .agents/
│   └── README.md                     # 项目级 skill 索引与安装入口
└── openspec/                         # 可选：启用 OpenSpec 后的配置、规格与 change
```

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI：**不伪造成功 · 完成前验证 · 改行为前先记录 · 不碰敏感信息 · 不擅自制造外部副作用 · 拿不准就问**。

项目长期规范放 `docs/`（见 [`docs/README.md`](./docs/README.md)）；UI、页面、交互和视觉资料放 `design/`（见 [`design/README.md`](./design/README.md)）；项目 skill 放 `.agents/skills/`（见 [`.agents/README.md`](./.agents/README.md)）；进行中需求使用当前任务、issue、PR、任务卡、用户指定文档或已启用的 OpenSpec change，并保持唯一载体。

## 接入业务项目

本项目只提供一套完整接入方式：创建或更新 `AGENTS.md`、`快速开始.md`、`新需求开发.md`、`需求变更.md`，递归合并 `docs/` 下全部文件，处理 `design/README.md`，并创建或更新 `.agents/README.md`。

接入时应保留业务项目已有内容和索引，不删除项目自行新增的文件或目录。同路径规范文件应合并；根目录工作流文件或 `design/README.md` 不存在时创建，内容相同时保持不变，内容不同时展示差异摘要并暂停，由用户选择保留、覆盖或合并。不得修改或删除 `design/` 下其他文件。

### 受管文件改名与删除

升级时区分“规范仓库已移除或改名的受管文件”和“业务项目自行新增的文件”。本版本把 `docs/规范/可选工具协作.md` 改名为 `docs/规范/工具协作.md`：

- 旧文件与上一版本原文一致时，创建新文件、更新 `docs/README.md` 索引后删除旧文件；
- 旧文件含业务项目自定义内容时，先展示差异，把仍有效的项目内容合并到新文件或其他合适载体，等待用户确认后再删除旧文件；
- 无法判断来源或无法安全迁移时停止，不保留两份当前工具规范，也不静默删除。

以后发生受管文件改名或删除时，也必须在对应版本的迁移说明中明确列出，不能用“保留项目已有文件”跳过迁移。

在业务项目中把下面的提示词发给 AI：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

不要因 OpenSpec、CodeGraph、Superpowers 的文档、目录、配置或命令存在就视为工具已启用。只有我明确选择，或当前任务已经确认采用某项工具时才使用；已启用工具不可用时说明失败和可选处理方式，不要自动安装或静默降级。

创建或更新 AGENTS.md、快速开始.md、新需求开发.md、需求变更.md，递归合并 docs/ 下全部文件，创建或更新 .agents/README.md，并处理 design/README.md。保留项目自行新增的内容和索引。

design/README.md 不存在则创建，相同则不处理，不同时展示差异并由我选择保留、覆盖或合并；不得修改 design/ 下其他文件。只有文件无法安全合并时才询问。完成后列出实际更新文件、未更新项和剩余风险。
```

## 升级已接入版本

```text
请为当前业务项目更新已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

不要因 OpenSpec、CodeGraph、Superpowers 的文档、目录、配置或命令存在就视为工具已启用。只有我明确选择，或当前任务已经确认采用某项工具时才使用；已启用工具不可用时说明失败和可选处理方式，不要自动安装或静默降级。

读取当前项目 AGENTS.md 中的 Vibe Coding Rule 版本并与规范仓库最新版本比较。版本相同也继续核对 AGENTS.md、快速开始.md、新需求开发.md、需求变更.md、docs/ 下全部文件、design/README.md 和 .agents/README.md 是否齐全且已同步；全部一致且没有未处理冲突时才提示已是最新版本。

需要升级时，按本 README 的“受管文件改名与删除”迁移旧规范文件，再递归合并当前规范文件并保留项目自行新增的内容和索引。根目录工作流文件或 design/README.md 不存在则创建，相同则不处理，不同时展示差异并由我选择保留、覆盖或合并；不得修改 design/ 下其他文件。完成后列出实际更新、迁移、删除、未更新的文件和剩余风险。
```

## 日常协作

| 你要做的事 | 看哪里 |
| --- | --- |
| 约束 AI 基本行为 | [`AGENTS.md`](./AGENTS.md) |
| 查技术栈、架构、编码约定 | [`docs/README.md`](./docs/README.md) 及其索引 |
| 存放 UI、页面、交互和视觉资料 | [`design/README.md`](./design/README.md) |
| 新功能 / 大改动 | [`新需求开发.md`](./新需求开发.md) |
| 修改已有需求 | [`需求变更.md`](./需求变更.md) |
| 三项工具的协作顺序与限制 | [`docs/规范/工具协作.md`](./docs/规范/工具协作.md) |
| 使用 UI 设计输入重组两阶段任务 | [`docs/规范/UI任务重组.md`](./docs/规范/UI任务重组.md) |
| 代码设计取舍 | [`docs/规范/代码设计原则.md`](./docs/规范/代码设计原则.md) |

收到需求时先选择当前任务、issue、PR、任务卡、用户指定文档或已启用的 OpenSpec change 作为唯一载体。仅在相关工具已启用时，才按 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md) 使用 OpenSpec、CodeGraph 或 Superpowers；确认后的口径、设计、计划和验证结论回填同一载体，不另建平行口径。

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线，以及可复制的目录与流程模板）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
