# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

面向 AI 辅助开发的通用协作规范。**核心规范一个文件、一页纸，复制即用。**

跨项目、前后端通用，不依赖任何特定 AI 工具或插件。

**快速选型:** [`快速开始.md`](./快速开始.md) · **版本记录:** [`CHANGELOG.md`](./CHANGELOG.md)

## 用法

### 业务项目目录（推荐结构）

```text
your-project/
├── AGENTS.md                 # 从本仓库复制：AI 协作通用底线
├── docs/                     # 项目规范（长期约定）；索引用 docs/README.md
│   └── README.md             # 可从本仓库 docs/README.md 复制
├── .agents/                  # 项目 skill（随仓库共享）
│   ├── README.md             # skill 目录索引；可从本仓库 .agents/README.md 复制
│   └── skills/
│       ├── new-feature-development/   # 新需求开发流程 skill（手动调用）
│       │   ├── SKILL.md
│       │   └── workflow.md
│       └── requirement-change/        # 需求变更流程 skill（手动调用）
│           ├── SKILL.md
│           └── workflow.md
└── openspec/                 # 仅启用 OpenSpec 时：进行中需求变更
    └── changes/<change-name>/
```

`docs/` 放稳定规范；`openspec/changes/` 放进行中需求（未启用 OpenSpec 则用 issue/PR）。详见 [`docs/README.md`](./docs/README.md)。

### 初始化步骤

1. 复制 [`AGENTS.md`](./AGENTS.md) 到业务项目根目录（若已有，先 diff 再合并）。
2. 创建 `docs/`，复制 [`docs/README.md`](./docs/README.md) 作目录说明与索引。
3. 创建 `.agents/`，复制 [`.agents/README.md`](./.agents/README.md) 作 skill 索引；需要流程 skill 时，可从本仓库复制两个 skill 目录（**安装 skill 不依赖** OpenSpec / CodeGraph / Superpowers，见 [项目流程 skill](#项目流程-skill可选)）。
4. 不确定流程深度？看 [`快速开始.md`](./快速开始.md) 三种场景；启用 OpenSpec 再按 [`新需求开发.md`](./新需求开发.md) 操作。

#### 通过 AI 初始化

在**业务项目**里把下面提示词发给 AI，由它参考本仓库完成目录初始化（具体命令由 AI 按当前环境处理）：

```text
请为当前业务项目接入 Vibe Coding Rule 协作规范。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

按该仓库 README「业务项目目录」完成初始化：
1. 将 AGENTS.md 放到项目根目录；若已存在，先 diff 说明差异与合并方案，我确认后再写入。
2. 创建 docs/，写入 docs/README.md（内容参考规范仓库 docs/README.md）。
3. 创建 .agents/，写入 .agents/README.md（内容参考规范仓库 .agents/README.md）。

完成后请询问我：是否需要安装两个项目流程 skill（new-feature-development、requirement-change）？若我确认需要，再按规范仓库 README「项目流程 skill」小节的安装提示词，从规范仓库复制对应目录到 .agents/skills/。安装 skill 不需要先装 OpenSpec、CodeGraph、Superpowers。

约束：
- 只改当前业务项目，不要在规范仓库里操作。
- 项目特有规范写进 docs/，skill 写进 .agents/skills/，不要把项目规则合并进 AGENTS.md。
- 写入或修改项目文件前先向我申请权限；完成后列出实际创建/更新的文件路径供我核对。
- 本次不主动安装 OpenSpec、CodeGraph、Superpowers；也不主动安装流程 skill，须先询问我并等我确认。
```

## 它管什么

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI 的协作行为：

1. 不伪造成功——真实失败要显式暴露
2. 完成前必须验证并说明结果
3. 改行为/接口/数据/权限前先有可追溯记录
4. 不碰密钥与敏感信息
5. 未经明确要求不 commit / push / deploy
6. 拿不准就停下来问

展开章节含 **§7 实施与改动**（简洁、精准改动；属展开层，非速查底线）。澄清、任务拆解、TDD 等执行方法由 OpenSpec / Superpowers 承担，见 [`流程公共约定.md`](./流程公共约定.md)。

## 它不管什么

技术栈、框架、命令、UI/API/业务约定等**项目规范文档**，统一放在项目 `docs/` 下；**项目 skill** 放在 `.agents/skills/` 下，并在 `.agents/README.md` 维护技能包目录（详见 `AGENTS.md` 开头）。都不写进 `AGENTS.md`——这样这份规范才能保持跨项目通用、不膨胀。

## 推荐工具链（可选）

只用 `AGENTS.md` 已经够用。需要更完整的需求治理与执行纪律时，推荐组合 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**：OpenSpec 管需求与验收，CodeGraph 管影响分析，Superpowers 管执行方法。三者都不替代 `AGENTS.md` 底线，未安装时自动 fallback 到 issue/PR 与人工分析。

开发流程见：[`新需求开发.md`](./新需求开发.md) · [`需求变更.md`](./需求变更.md) · 共用约定 [`流程公共约定.md`](./流程公共约定.md)。

### 项目流程 skill（可选）

本仓库提供两个**随项目共享**的流程 skill，放在 `.agents/skills/` 下，把 **OpenSpec + CodeGraph + Superpowers** 串成可执行的逐步清单。二者均设置 `disable-model-invocation: true`，**不会自动触发**，需要你在对话里手动 `@` 或明确说「按 xxx skill 执行」。

#### 工具依赖（使用，非安装）

**安装**两个 skill：随时可从本仓库复制到 `.agents/skills/`，**不需要**先装 OpenSpec、CodeGraph、Superpowers。

**使用**两个 skill 跑完整流程：步骤编排指向 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**；未装齐时无法完整执行对应步骤，只能按 [`流程公共约定.md`](./流程公共约定.md) fallback 到 issue/PR、人工读源码、手动澄清与验证（`AGENTS.md` 底线不打折）。

| 工具 | 在使用中的角色 | 在两个 skill 中的用途 | 未安装时的 fallback |
| --- | --- | --- | --- |
| **OpenSpec** | **主（核心）** | `openspec/changes/` 承载需求；`/opsx:propose` / `apply` / `sync` / `archive`；`openspec validate` / `archive` CLI | 用 issue、PR、任务卡承载 proposal / spec / tasks / 验收 |
| **CodeGraph** | **辅（影响分析步骤）** | `codegraph_explore` / `callers` / `callees` / `impact` 做影响分析与回归范围评估 | AI 人工通读相关源码与测试，并记录未覆盖范围 |
| **Superpowers** | **辅（执行方法步骤）** | `$brainstorming` 澄清；`$writing-plans` 拆任务；`$test-driven-development` 实现（推荐）；`$verification-before-completion` 核对验证证据 | 手动澄清、按 tasks 推进、完成前自行核对验证证据 |

**推荐顺序（完整使用流程时）**：

1. 复制 `AGENTS.md` + `docs/` + `.agents/`（及可选的两个 skill）
2. 安装 **OpenSpec** → **CodeGraph** → **Superpowers**（提示词见下方 [通过 AI 安装](#通过-ai-安装)）
3. 手动 `@` 调用 skill 跑流程

| Skill | 路径 | 何时手动调用 | 内容 |
| --- | --- | --- | --- |
| `new-feature-development` | `.agents/skills/new-feature-development/` | 从零做新功能、新接口、加能力（会改变外部可观察行为） | `SKILL.md`：8 步编排清单；`workflow.md`：完整命令与产出 |
| `requirement-change` | `.agents/skills/requirement-change/` | 已有需求口径变化（范围、验收、方案调整） | `SKILL.md`：10 步编排清单；`workflow.md`：完整命令、变化记录模板 |

人类查阅入口仍保留在根目录 [`新需求开发.md`](./新需求开发.md) 与 [`需求变更.md`](./需求变更.md)（流程速览 + 指向 skill 正文）；**正文唯一来源**在各自 skill 目录的 `workflow.md`。索引维护见 [`.agents/README.md`](./.agents/README.md)。

**手动调用示例：**

```text
@new-feature-development 按流程开发 add-sso-login
@requirement-change 密码最短长度要从 8 改到 12，先走变更流程
```

### 通过 AI 安装

在**业务项目**（勿在本规范仓库）里把下面提示词发给 AI 完成**安装**；安装完成后，按各工具小节的「初始化」步骤自行操作。

#### 项目流程 skill（new-feature-development / requirement-change）

| 项目 | 说明 |
| --- | --- |
| **使用依赖** | 跑完整流程需 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**——见上方 [工具依赖（使用，非安装）](#工具依赖使用非安装)；**安装 skill 本身无此前提** |
| **环境** | 在**业务项目根目录**操作；建议已复制 `AGENTS.md` 并创建 `.agents/` |
| **安装** | 从本规范仓库复制两个 skill 目录到业务项目 `.agents/skills/`（无需先装三项工具） |
| **验证** | 存在 `.agents/skills/new-feature-development/SKILL.md` 与 `requirement-change/SKILL.md`；`.agents/README.md` 索引表已登记；手动 `@` 可加载 skill |

安装提示词：

```text
请为当前业务项目安装 Vibe Coding Rule 的两个项目流程 skill。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

说明：复制 skill 目录即可，不需要先安装 OpenSpec、CodeGraph、Superpowers。这三项是「使用 skill 跑完整流程」时的依赖；未装齐时相关步骤按规范仓库 流程公共约定.md fallback。

需要复制到业务项目的内容（参考规范仓库对应路径）：
1. .agents/skills/new-feature-development/（含 SKILL.md 与 workflow.md）
2. .agents/skills/requirement-change/（含 SKILL.md 与 workflow.md）
3. 更新 .agents/README.md 的 skill 索引表（参考规范仓库 .agents/README.md）

约束：
- 只改当前业务项目，不要在规范仓库里操作。
- 这两个 skill 设置了 disable-model-invocation: true，仅手动 @ 调用，不会自动触发。
- 写入或修改项目文件前先向我申请权限；完成后列出实际创建/更新的文件路径供我核对。
- 若业务项目尚无 .agents/，先创建 .agents/README.md（参考规范仓库）。
- 本次只安装流程 skill，不安装 OpenSpec、CodeGraph、Superpowers（除非我另行要求）。
```

初始化：

1. 确认 `.agents/skills/` 下两个 skill 目录完整（各含 `SKILL.md` + `workflow.md`）
2. 在 AI 工具中手动 `@new-feature-development` 或 `@requirement-change` 试加载
3. 若要跑完整流程，再按下方 OpenSpec / CodeGraph / Superpowers 小节安装三项工具

#### OpenSpec（需项目初始化）

| 项目 | 说明 |
| --- | --- |
| **环境** | Node.js ≥ 20.19.0；在**业务项目根目录**操作；有 `.node-version` 时先按 `AGENTS.md` 切换版本 |
| **安装** | 项目依赖 `npm install -D @fission-ai/openspec`（勿全局安装） |
| **验证** | 存在 `openspec/` 目录；CLI 有版本输出；AI 中 `/opsx:propose` 可用 |

安装提示词：

```text
请从 https://github.com/Fission-AI/OpenSpec 为当前项目安装 OpenSpec。
先查看官方 README，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
如果需要执行网络下载或修改项目文件，请先向我申请权限。
安装完成后验证 CLI 可用。
```

初始化：

1. 在业务项目根目录执行 `npx openspec init`，生成 `openspec/`
2. 执行 `npx openspec --version` 验证 CLI 可用
3. （可选）执行 `npx openspec config profile` 选择 profile，再执行 `npx openspec update` 启用扩展斜杠命令

#### CodeGraph（需本机 + 项目初始化）

| 项目 | 说明 |
| --- | --- |
| **环境** | Windows / macOS / Linux；CLI 安装后需**新开终端**；当前 AI 工具须支持 MCP |
| **安装** | 项目依赖 `npm install -D @colbymchenry/codegraph`（勿全局安装）；需查官方 README 时再参考安装脚本 |
| **验证** | 项目内有 `.codegraph/`；AI 可调用 `codegraph_explore` 等 MCP 工具 |

安装提示词：

```text
请从 https://github.com/colbymchenry/codegraph 为当前项目安装 CodeGraph。
先查看官方 README 和安装脚本，确认适合当前项目的安装方式（项目依赖，不要全局安装）。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，并说明是否需要重启当前 AI 工具。
```

初始化：

1. 新开终端，执行 `npx codegraph install`，把 MCP 接入当前 AI 工具
2. 在业务项目根目录执行 `npx codegraph init -i`（创建 `.codegraph/` 并建索引）
3. 若未使用 `-i`：先执行 `npx codegraph init`，再执行 `npx codegraph index`

#### Superpowers（无项目级 init，安装后需重启 AI 工具）

| 项目 | 说明 |
| --- | --- |
| **环境** | 安装在**当前 AI 工具侧**（用户级）；Cursor / Claude Code / Codex 等按官方章节操作 |
| **安装** | 例：Cursor 用 `/add-plugin superpowers`；详见 [官方 Installation](https://github.com/obra/superpowers#installation) |
| **验证** | 技能目录可见；产出回填 OpenSpec change 或需求载体，遵守 `AGENTS.md` |

安装提示词：

```text
请从 https://github.com/obra/superpowers#installation 安装 Superpowers。
先查看官方 README 和 Installation 章节，确认适合当前 AI 工具的安装方式。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
```

初始化：

1. 重启 AI 工具
2. 确认 `$brainstorming` 等技能可触发

## 贡献

详见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)。请保持改动聚焦：只收跨项目通用的 AI 协作底线，不收公司内部流程或框架风格大全。

## 许可证

[MIT](./LICENSE)。可自由复制到商业或非商业项目，保留版权声明即可。
