# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v3.2.1](https://img.shields.io/badge/Version-v3.2.1-blue)](./CHANGELOG.md)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

**一套可搭配 OpenSpec、CodeGraph、Superpowers 的完整 AI 协作规范。** 不绑定 Cursor、Claude Code 等特定 AI 工具，前后端项目都能用。

Vibe Coding Rule 通过可复制的行为约束、判断规则、操作流程和项目模板，约定 AI 与开发者在需求澄清、影响分析、实现、验证和交付中的协作方式。它属于规范与流程层，不是自动执行策略、拦截改动或收集验证证据的软件工具。

**版本记录：** [`CHANGELOG.md`](./CHANGELOG.md)

## 日常协作

| 你要做的事 | 看哪里 |
| --- | --- |
| 约束 AI 常驻行为、需求确认与知识路由 | [`AGENTS.md`](./AGENTS.md) |
| 查技术栈、架构、编码约定 | [`docs/README.md`](./docs/README.md) 及其索引 |
| 存放 UI、页面、交互和视觉资料 | [`design/README.md`](./design/README.md) |
| 正式新增需求 | [`新增需求工作流.md`](./新增需求工作流.md)（仅本规范仓教程，不接入业务项目） |
| 正式变更需求 | [`需求变更工作流.md`](./需求变更工作流.md)（仅本规范仓教程，不接入业务项目） |

AI 可以识别用户内容可能构成新增需求或需求变更，但不得据此自动进入 OpenSpec 流程。用户先确认需求意图，AI 才按 [`docs/README.md`](./docs/README.md) 索引读取 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md) 并继续澄清。新增需求在目标、范围和验收口径确认后，由用户主动要求创建；AI 生成 `change-id` 并检查冲突，发现同名或可能承载该需求的现有 change 时停止，否则创建。需求变更或已有需求的后续工作仍须只读查找候选，并由用户确认继续原 change / 新建关联 change；新建分支同样由创建请求授权 AI 生成 ID、检查冲突并创建。后续口径与验证结论回填同一 change，不另建平行口径；issue/PR 等只作输入来源。

## 工具链

接入本规范包后，**默认假定** OpenSpec、CodeGraph、Superpowers **均已成功安装并可调用**。仅在当前任务已触发并确实需要某项工具时检查可用性，不在每个 Chat 开始时预检全部工具；职责与失败处理见 [`docs/规范/工具协作.md`](./docs/规范/工具协作.md)。

三句话口径：假定已安装 → 执行中**需要使用**某项却不可用则中断并指明工具名 → 用文末「工具安装」提示词或官方文档修复，不静默降级、不擅自在接入流程里顺手安装。

官方来源（命令以当前文档为准）：

- OpenSpec：<https://github.com/Fission-AI/OpenSpec>
- CodeGraph：<https://github.com/colbymchenry/codegraph>
- Superpowers：<https://github.com/obra/superpowers#installation>

## 规范组成

```text
your-project/                         # 业务项目接入后的示意结构（非本规范仓目录清单）
├── AGENTS.md                         # AI 常驻规则、强制底线与按需路由
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

[`AGENTS.md`](./AGENTS.md) 承载每次 Chat 都需要知道的规则，用六条底线约束 AI，并负责需求识别、用户确认与 `docs/` 按需路由：**不伪造成功 · 完成前验证 · 改行为前先记录 · 不碰敏感信息 · 不擅自制造外部副作用 · 拿不准就问**。

项目长期知识与条件化流程放 `docs/`，由 [`docs/README.md`](./docs/README.md) 按需索引，不在每个 Chat 中全量读取；UI、页面、交互和视觉资料放 `design/`（见 [`design/README.md`](./design/README.md)）；项目 skill 放 `.agents/skills/`（见 [`.agents/README.md`](./.agents/README.md)）。只有用户明确确认的新增需求或需求变更才启用 OpenSpec，后续继续使用用户确认的同一 change（issue/PR 等仅作输入来源）。

## 接入业务项目

本项目只提供一套完整接入方式：创建或更新 `AGENTS.md`，递归合并 `docs/` 下全部文件，处理 `design/README.md`，并创建或更新 `.agents/README.md`。

接入时应保留业务项目已有内容和索引，不删除项目自行新增的文件或目录。同路径规范文件应合并；`AGENTS.md` 或 `design/README.md` 不存在时创建，内容相同时保持不变，内容不同时展示差异摘要并暂停，由用户选择保留、覆盖或合并。不得修改或删除 `design/` 下其他文件。

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

升级前先查看 [`CHANGELOG.md`](./CHANGELOG.md) 中当前版本之后、目标版本及之前各版本的 `Migration`，依次处理受管文件改名、删除和接入范围变化。

```text
请升级当前业务项目已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

比对 AGENTS.md 中的版本与规范仓最新版；同版本仍核对受管文件是否齐全一致，一致则说明已是最新。
需要升级时：先阅读规范仓 CHANGELOG.md 中当前版本之后、目标版本及之前各版本的 Migration，依次完成受管文件改名、删除和接入范围迁移；再只合并 AGENTS.md、docs/、design/README.md、.agents/README.md；保留项目自有内容；不复制非受管文件。
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
