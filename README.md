# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v4.1.3](https://img.shields.io/badge/Version-v4.1.3-blue)](./CHANGELOG.md)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

**一套可搭配 OpenSpec 与 CodeGraph 的 AI 协作规范。** 不绑定特定 AI 工具，前后端项目都能用。

Vibe Coding Rule 通过可复制的行为约束、判断规则和项目模板，约定 AI 与开发者在需求澄清、影响分析、实现、验证和交付中的协作方式。它属于规范与流程层，不是自动执行、拦截改动或收集证据的软件工具。

它兼容 `AGENTS.md` 生态中“给 coding agents 的项目说明书”这一定位，但不只提供提示词模板：本规范把需求载体、代码图谱证据、敏感信息、外部状态和完成前验证都纳入同一套协作边界。适合希望 AI 能参与真实工程交付、同时又不伪造成功或越权改动的团队。

**版本记录：** [`CHANGELOG.md`](./CHANGELOG.md)

## 日常协作

| 你要做的事                           | 看哪里                                                                       |
| ------------------------------------ | ---------------------------------------------------------------------------- |
| 约束 AI 常驻行为、需求确认与知识路由 | [`AGENTS.md`](./AGENTS.md)                                                   |
| 查技术栈、架构、编码约定             | [`docs/README.md`](./docs/README.md) 及其索引                                |
| 存放 UI、页面、交互和视觉资料        | [`design/README.md`](./design/README.md)                                     |
| 正式新增需求                         | [`新增需求工作流.md`](./新增需求工作流.md)（仅本规范仓教程，不接入业务项目） |
| 正式变更需求                         | [`需求变更工作流.md`](./需求变更工作流.md)（仅本规范仓教程，不接入业务项目） |

只有用户明确指定新增需求、需求变更或已有需求的后续工作，才进入 OpenSpec 承接流程；用户未指定时，AI 按直接授权的任务处理。确认后的口径与验证结论回填同一 change，issue/PR 等只作输入来源。

## 工具链

本规范只维护 OpenSpec + CodeGraph 的完整协作路线：OpenSpec 承载确认后的需求、设计、任务和验证记录，CodeGraph 基于当前项目索引提供代码结构、符号关系、调用路径和影响范围证据。不提供 Lite、单文件复制、可选工具替代或部分接入分支。

接入本规范包不会自动安装工具；正式协作前应完成当前项目和当前 AI 工具侧接入。某些 AI 工具或个人环境可能默认带有全局能力、内置 skill、Superpowers 目录及其附带文档，这些不构成本规范依赖，也不属于业务项目受管文件。职责、边界与失败处理以 [`AGENTS.md`](./AGENTS.md) 为准。

官方来源（命令以当前文档为准）：

- OpenSpec：<https://github.com/Fission-AI/OpenSpec>
- CodeGraph：<https://github.com/colbymchenry/codegraph>

## v4 工作方式

1. 普通问答、只读分析、仓库状态检查和用户直接授权的小改动，按当前请求处理，不自动创建或查找 OpenSpec；若实施中发现会改变外部行为、接口、数据、权限、流程或验收，先停止并确认承接方向。
2. 用户明确指定新增需求、需求变更或已有需求后续时，按 `AGENTS.md` 的授权顺序进入 OpenSpec；确认后的目标、范围、设计、任务和验证结果只回填同一个 change。
3. 涉及既有代码结构、符号关系、引用、调用路径或影响范围时，先确认 CodeGraph 已完成当前项目索引且当前会话可用，再用 CodeGraph 取得证据；CodeGraph 不可用时中断依赖代码图谱证据的部分，不用纯文本搜索或模型猜测冒充同等结论。
4. 完成实现或验证后，说明实际改变、已核对的不变式、验证证据、未验证项、剩余风险；涉及外部状态时说明授权来源和实际结果。

## 规范组成

```text
your-project/                         # 业务项目接入后的示意结构（非本规范仓目录清单）
├── AGENTS.md                         # AI 常驻规则、强制底线与按需路由
├── docs/
│   ├── README.md                     # 项目长期规范索引
│   └── 规范/
│       ├── 代码设计原则.md
│       └── 前后端工程级编码规范.md
├── design/
│   └── README.md                     # UI、页面、交互和视觉资料说明
├── .agents/
│   └── README.md                     # 项目级 skill 索引与安装入口
└── openspec/                         # 由 OpenSpec 在业务项目中生成；本规范仓不含
```

[`AGENTS.md`](./AGENTS.md) 承载每次 Chat 都需要知道的规则，用六条底线约束 AI，并负责需求授权判断、用户确认与 `docs/` 按需路由。

项目长期知识放 `docs/`，由 [`docs/README.md`](./docs/README.md) 按需索引；UI、页面、交互和视觉资料放 `design/`；项目 skill 放 `.agents/skills/`。

个人全局 skill、AI 工具内置 skill、Superpowers 目录或其他用户目录下的工具文档，不属于上面的项目结构；业务项目接入和升级时不得把它们当作受管文件复制或索引，除非用户明确要求并确认归属。

最小理解示例：

```text
AGENTS.md：每次 Chat 必须先知道的底线、权限、需求承接和知识路由。
docs/README.md：长期项目知识的索引，只在任务需要时读取。
OpenSpec change：用户确认后的新增需求或需求变更，只使用一个承接载体。
CodeGraph：涉及代码结构、符号关系、调用路径或影响范围时使用的代码事实来源。

交付说明：必须说明实际改变、验证证据、未验证项和剩余风险。
```

## 接入业务项目

本项目只提供一套完整接入方式：创建或更新 `AGENTS.md`，递归合并 `docs/` 下全部文件，处理 `design/README.md`，并创建或更新 `.agents/README.md`。不要拆成 Lite、单文件或部分工具接入。

`docs/`、`design/README.md` 和 `.agents/README.md` 中的规范包自带文档会在标题下标注来源。接入或升级业务项目时应保留该标记；业务项目自行新增、生成或沉淀的文档不要使用该标记，以便区分规范包文档与项目自有文档。

接入时应保留业务项目已有内容和索引，不删除项目自行新增的文件或目录。同路径规范文件应合并；`AGENTS.md` 或 `design/README.md` 不存在时创建，内容相同时保持不变，内容不同时展示差异摘要并暂停，由用户选择保留、覆盖或合并。不得修改或删除 `design/` 下其他文件。

业务项目如已有必须每次 Chat 都知道的少量项目事实，可合并到 `AGENTS.md` 的项目事实区，例如技术栈、常用验证命令、源码与测试目录、禁止修改区域。事实必须来自用户确认或项目已有文件；没有明确来源时只提示待确认，不得编造。较长或条件化知识仍放入 `docs/` 并由索引按需读取。

在业务项目中把下面的提示词发给 AI：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

只处理受管文件：AGENTS.md、docs/ 下全部文件、design/README.md、.agents/README.md。保留项目自有内容与索引；不复制其他文件。
保留 Vibe Coding Rule 自带文档标题下的来源标记；业务项目自有或生成的文档不要使用该标记。
本项目只接受完整接入，不拆成 Lite、单文件或部分工具接入。
design/README.md：不存在则创建，相同跳过，不同则展示差异并由我选择；不得改 design/ 下其余文件。
若 AGENTS.md 已有技术栈、常用命令、源码/测试目录、禁止修改区域等每次 Chat 必须知道的项目事实，保留并合并到项目事实区；没有明确来源时不要编造。
本规范使用 OpenSpec 与 CodeGraph 分别承载需求记录和代码图谱证据；接入规则文件本身不要求安装或检查工具。后续任务实际需要工具时，若状态异常则中断并指明阻塞点，勿自动安装或静默降级。
AI 工具或个人环境默认安装的全局 skill、内置能力、Superpowers 目录及其附带文档，不属于本规范依赖或受管文件；不要复制、迁移或登记到本项目，除非我明确要求并确认归属。
完成后列出实际更新、未更新项和剩余风险。
```

## 升级已接入版本

升级前先查看 [`CHANGELOG.md`](./CHANGELOG.md) 中当前版本之后、目标版本及之前各版本的 `Migration`，依次处理受管文件改名、删除和接入范围变化。

```text
请升级当前业务项目已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

比对 AGENTS.md 中的版本与规范仓最新版；同版本仍核对受管文件是否齐全一致，一致则说明已是最新。
需要升级时：先阅读规范仓 CHANGELOG.md 中当前版本之后、目标版本及之前各版本的 Migration，依次完成受管文件改名、删除和接入范围迁移；再只合并 AGENTS.md、docs/、design/README.md、.agents/README.md；保留项目自有内容；不复制非受管文件，不拆成 Lite、单文件或部分工具接入。
保留 Vibe Coding Rule 自带文档标题下的来源标记；业务项目自有或生成的文档不要使用该标记。
design/README.md 与 AGENTS.md：不存在则创建，相同跳过，不同则展示差异并由我选择；不得改 design/ 下其余文件。
本规范使用 OpenSpec 与 CodeGraph 分别承载需求记录和代码图谱证据；升级规则文件本身不要求安装或检查工具。后续任务实际需要工具时，若状态异常则中断并指明阻塞点。完成后列出更新、迁移、删除、未更新项和剩余风险。
AI 工具或个人环境默认安装的全局 skill、内置能力、Superpowers 目录及其附带文档，不属于本规范依赖或受管文件；不要复制、迁移或登记到本项目，除非我明确要求并确认归属。
```

## 工具准备

接入规范包**不会**安装 OpenSpec 或 CodeGraph。首次准备协作环境时，把对应提示词发给 AI。

**OpenSpec**（需 CLI + 当前项目初始化 + AI 侧接入）：

```text
请按 https://github.com/Fission-AI/OpenSpec 当前官方说明，安装 OpenSpec CLI，在当前项目初始化，并优先使用官方共享 agents 接入方式，让 OpenSpec 生成的 skills 落到 .agents/skills/openspec-*，不要安装到单一 AI 工具私有目录（例如 .codex/skills/）。
若官方当前版本不支持共享 agents 接入，立即说明阻塞点和可选方案，不要静默改用工具私有目录。
安装后同步更新 .agents/README.md，将 .agents/skills/openspec-* 作为一组 OpenSpec 工具 skills 登记，注明来源为 OpenSpec CLI、用途为需求流程工具接入、更新以官方 CLI 为准；不要把它们拆成团队手写项目 skill 逐个维护。
需要重启 AI 才能加载时说明原因后停止，由我重启；不要假装已可用。完成后说明验证方式。
```

**CodeGraph**（需安装 + AI/MCP 接入 + 当前项目索引）：

```text
请按 https://github.com/colbymchenry/codegraph 当前官方说明，安装 CodeGraph，完成我正在使用的 AI 工具接入，并在当前项目运行所需的索引初始化（例如 codegraph init）。
需要重启 AI 才能加载 MCP 时说明原因后停止，由我重启；不要假装已可用。完成后说明验证方式。
```

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线，以及可复制的目录与流程模板）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
