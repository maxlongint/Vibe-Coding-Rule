# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v2.0.0](https://img.shields.io/badge/Version-v2.0.0-blue)](./CHANGELOG.md)
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
├── AGENTS.md                         # 最小安装：通用 AI 协作底线
├── docs/                             # 完整安装：递归同步全部文件（下列为当前内容示例）
│   ├── README.md                     # 项目规范目录索引
│   └── 规范/
│       ├── 代码设计原则.md
│       └── 可选工具协作.md
├── .agents/                          # 完整安装包含，实际 skill 按需添加
│   └── README.md                     # 完整安装：项目级 skill 索引与安装入口
└── openspec/                         # 可选：启用 OpenSpec 时的进行中需求
```

**最小安装：** 只复制 `AGENTS.md`；不创建 `docs/`、`.agents/`，不安装 OpenSpec、CodeGraph、Superpowers。

**按需扩展：** 最小安装后确有长期规范时，先创建 `docs/README.md` 再添加规范文档；需要自建或安装项目级 skill 时，先创建 `.agents/README.md` 再向 `.agents/skills/` 添加实际技能包。索引只登记实际存在的内容。

**完整安装（含 docs/ 全部文件）：** 创建或更新 `AGENTS.md`、递归合并 `docs/` 下全部文件，并创建或更新 `.agents/README.md`。同路径文件应合并并保留业务项目内容，尤其是 `docs/README.md` 中的项目自有索引；不得删除业务项目自行新增的文件或目录。完整安装只接入规范文件，不安装或初始化 OpenSpec、CodeGraph、Superpowers；需要工具时，由用户后续自行取用本文第 4 节的对应提示词。具体场景见 [`快速开始.md`](./快速开始.md)。

### 2. 日常协作

下列流程文档在**本规范仓库**内查阅（或按需复制到业务项目）；只完成最小安装的项目默认没有这些文件。

| 你要做的事 | 看哪里 |
| --- | --- |
| 约束 AI 基本行为 | 项目根目录 `AGENTS.md` |
| 查技术栈、架构、编码约定 | 项目 `docs/` |
| 新功能 / 大改动 | [`新需求开发.md`](./新需求开发.md) |
| 改已有需求 | [`需求变更.md`](./需求变更.md) |
| OpenSpec / CodeGraph / Superpowers 的流程与限制 | [`docs/规范/可选工具协作.md`](./docs/规范/可选工具协作.md) |
| 代码设计取舍（复制 `docs/` 时） | [`docs/规范/代码设计原则.md`](./docs/规范/代码设计原则.md) |

### 3. 用 AI 接入或升级 Vibe Coding Rule

在**业务项目**（不是本规范仓库）里把提示词发给 AI。

**首次接入**（先询问安装类型）：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

写入任何文件前，先只询问我选择哪种安装类型，并等我回答：
1. 最小安装：只创建或更新 AGENTS.md。
2. 完整安装：创建或更新 AGENTS.md、docs/ 下全部文件、.agents/README.md。

获得选择后，直接按所选类型接入。最小安装只创建或更新 AGENTS.md。完整安装递归创建或更新规范仓库 docs/ 下全部文件；同路径文件先对比并合并，保留业务项目自己的规范以及 docs/README.md 中的自有索引，不得删除业务项目自行新增的文件或目录。只有无法安全合并时再询问。只改当前项目，不要把项目特有规则写进 AGENTS.md；完成后列出实际更新文件和剩余风险。
```

**升级已接入版本**：

```text
请为当前业务项目更新已接入的 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

请先识别当前项目采用最小安装还是完整安装；无法根据已有文件确定时先询问我，未经确认不要切换安装类型。

再读取当前项目 AGENTS.md 中的 Vibe Coding Rule 版本，并与规范仓库最新版本比较。版本相同也继续核对当前安装类型要求的规范文件是否齐全且内容已同步；全部一致时才提示当前已是最新版本并停止。版本不同、缺失、无法识别，或要求的文件缺失、落后时，直接按最新版本升级，不先输出差异分析。

请按规范仓库最新版本更新当前项目中的：
1. AGENTS.md
2. docs/ 下全部文件（仅完整安装）
3. .agents/README.md（仅完整安装）

要求：按当前安装类型更新，只改当前项目。最小安装只更新 AGENTS.md，不创建或更新 docs/、.agents/。完整安装递归合并规范仓库 docs/ 下全部文件：上游存在的文件逐一创建或更新，同路径文件合并并保留项目内容，docs/README.md 必须保留项目自有索引；不得删除业务项目自行新增的文件或目录。.agents/README.md 同样保留项目 skill 索引。只有无法安全合并时再询问；完成后列出实际更新文件、未更新项和剩余风险。
```

### 4. 可选工具链（通过 AI 安装）

本节独立提供工具安装与初始化提示词，不属于最小安装或完整安装的自动步骤。启用条件、协作流程、调用方式、限制和 fallback 统一见 [`docs/规范/可选工具协作.md`](./docs/规范/可选工具协作.md)。只用 `AGENTS.md` 已经够用；需要可选工具时，由用户在**业务项目**自行取用对应提示词，勿在本规范仓库操作。

需要完整工具链时，用户可按 **OpenSpec → CodeGraph → Superpowers** 依次取用下方提示词；只需要其中一项时，直接取用该工具的提示词即可。复制规范文件不构成工具安装或初始化授权；只有用户实际发送对应提示词时才执行，并在执行前查看官方文档，确认当前版本、项目环境和 AI 工具的接入方式。

**执行安装 / 初始化命令时，何时需要重启 AI 工具**

重启不是安装或初始化后的统一动作。每一步都要判断：下一步是否依赖本轮刚注册的 MCP、斜杠命令或 skill，以及当前会话能否动态加载它。

- 当前会话已能使用下一步所需能力：继续初始化和验证，不额外要求重启。
- 下一步必须使用尚未加载的能力：先完成重启前可安全执行的步骤，说明已完成项、剩余步骤和原因，然后停下；用户完整退出并重新打开 AI 编码工具后，在新会话继续。
- CLI 初始化可以独立完成：先完成 CLI 步骤，在首次调用 MCP / 命令 / skill 前再判断是否需要重启，不把重启当成 CLI 初始化前置条件。

这里的重启指完整退出并重新打开 AI 编码工具，不是只刷新聊天窗口。AI 不能替用户重启，也不得在需要重启时假装新能力已经加载。

**OpenSpec**

官方来源：<https://github.com/Fission-AI/OpenSpec>。当前前提为 Node.js ≥ 20.19.0；官方安装文档当前提供 npm、pnpm、yarn、bun 的全局安装方式以及 Nix 方式。安装命令和支持范围可能变化，执行时以官方 Installation 文档、团队环境和包管理策略为准，不沿用模板中的旧命令猜测。

安装：

```text
请从 https://github.com/Fission-AI/OpenSpec 为当前项目安装 OpenSpec。
先查看官方 Installation 文档，确认当前支持且适合团队环境的安装方式，不照搬过期命令。
安装完成后验证已可用，并提醒我继续发送初始化提示词。
```

初始化（安装完成后另发一条）：

```text
请按 https://github.com/Fission-AI/OpenSpec 官方文档，为当前项目完成 OpenSpec 初始化（接入当前 AI 工具、使当前 AI 工具生成的 OpenSpec 命令或 skill 可用）。
不同 AI 工具生成的调用形式可能不同：Claude Code 中可为 `/opsx:propose`，Cursor 中可为 `/opsx-propose`，部分工具以 skill 形式提供；`/opsx:*` 只是通用示例记法，实际以当前工具生成的内容为准。
逐步判断下一步是否依赖本轮刚写入、但当前会话尚未加载的斜杠命令或技能；若必须完整重启 AI 工具才能继续，先说明已完成项、剩余步骤和原因，然后停下等我重启，重启后再继续。
完成后验证初始化结果；不要把安装或文件写入成功当成斜杠命令已经加载。
```

**CodeGraph**

官方来源：<https://github.com/colbymchenry/codegraph>。支持 Windows、macOS、Linux，当前 AI 工具须支持相应 MCP 接入；官方当前提供独立安装器，以及 `npx` / npm 安装方式，并由交互式安装器配置支持的 AI 工具。安装与初始化入口可能变化，具体以官方 README 和目标 AI 工具为准。

安装：

```text
请从 https://github.com/colbymchenry/codegraph 为当前项目安装 CodeGraph。
先查看官方 README 和安装说明，确认当前支持且适合操作系统与 AI 工具的安装方式，不照搬过期命令。
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

官方来源：[Installation](https://github.com/obra/superpowers#installation)。Superpowers 安装在 AI 工具侧，**无项目级初始化**；例如 Cursor 使用 `/add-plugin superpowers`。具体安装方式和是否能动态加载 skill，以当前 AI 工具和官方说明为准。

安装：

```text
请从 https://github.com/obra/superpowers#installation 安装 Superpowers。
先查看官方 README 和 Installation 章节，确认适合当前 AI 工具的安装方式。
安装后先按官方说明判断当前 AI 工具能否动态加载新技能；若首次调用技能前必须完整重启 AI 工具，说明安装结果和重启原因后停下等我重启，重启后再验证技能可用性。
不要把插件安装成功当成技能已经加载。
```

安装和初始化完成后，按 [`docs/规范/可选工具协作.md`](./docs/规范/可选工具协作.md) 使用；场景选型见 [`快速开始.md`](./快速开始.md)，OpenSpec 主流程见 [`新需求开发.md`](./新需求开发.md) / [`需求变更.md`](./需求变更.md)。

## 贡献与许可

贡献见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)（只收跨项目通用的 AI 协作底线）。许可：[MIT](./LICENSE)，可自由复制到商业或非商业项目。
