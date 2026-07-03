# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version: v1.1.2](https://img.shields.io/badge/Version-v1.1.2-blue)](./CHANGELOG.md)
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
│   ├── README.md             # 可从本仓库 docs/README.md 复制
│   └── 规范/                 # 可从本仓库 docs/规范/ 复制通用规范模板
├── .agents/                  # 项目 skill（按需，随仓库共享）
│   ├── README.md             # skill 目录索引；可从本仓库 .agents/README.md 复制
│   └── skills/
│       └── <skill-name>/     # 可选：团队自定义或第三方 skill
│           └── SKILL.md
└── openspec/                 # 仅启用 OpenSpec 时：进行中需求变更
    └── changes/<change-name>/
```

`docs/` 放稳定规范；`openspec/changes/` 放进行中需求（未启用 OpenSpec 则用 issue/PR）。详见 [`docs/README.md`](./docs/README.md)。

### 初始化步骤

1. 复制 [`AGENTS.md`](./AGENTS.md) 到业务项目根目录（若已有，先 diff 再合并）。
2. 创建 `docs/`，复制本仓库 [`docs/`](./docs/) 下的通用规范模板（至少包含 [`docs/README.md`](./docs/README.md)）。
3. 创建 `.agents/`，复制 [`.agents/README.md`](./.agents/README.md) 作 skill 索引；有团队自定义或第三方 skill 时，再放入 `.agents/skills/`。
4. 不确定流程深度？看 [`快速开始.md`](./快速开始.md) 三种场景；启用 OpenSpec 再按 [`新需求开发.md`](./新需求开发.md) 操作。

#### 在业务项目中接入 Vibe Coding Rule

在**业务项目**里把下面提示词发给 AI，由它参考本仓库把 Vibe Coding Rule 接入当前项目（具体命令由 AI 按当前环境处理）：

```text
请为当前业务项目接入 Vibe Coding Rule。
规范仓库：https://github.com/maxlongint/Vibe-Coding-Rule

请参考规范仓库 README，在当前项目中创建或更新：
1. AGENTS.md
2. docs/（复制或合并规范仓库 docs/ 下的通用规范模板）
3. .agents/README.md

要求：只改当前项目；已有文件先 diff 并给出合并方案，等我确认后再写入；不要把项目特有规则写进 AGENTS.md；完成后列出实际更新文件和剩余风险。本次不安装 OpenSpec、CodeGraph、Superpowers。
```

#### 升级已接入的 Vibe Coding Rule

业务项目已经接入过 Vibe Coding Rule，后来本仓库升级时，在**业务项目**里把下面提示词发给 AI，让它先比对差异、给出合并方案，再更新规则相关文件：

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

## 它管什么

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI 的协作行为：

1. 不伪造成功——真实失败要显式暴露
2. 完成前必须验证并说明结果
3. 改行为/接口/数据/权限前先有可追溯记录
4. 不碰密钥与敏感信息
5. 未经明确要求不 commit / push / deploy
6. 拿不准就停下来问

展开章节含 **§7 实施与改动**（简洁、目标变更与非目标不变式、精准改动；属展开层，非速查底线）。澄清、任务拆解、TDD 等执行方法由 OpenSpec / Superpowers 承担，见 [`流程公共约定.md`](./流程公共约定.md)。

## 它不管什么

技术栈、框架、命令、架构与跨需求业务约定等**项目规范文档**，统一放在项目 `docs/` 下；**项目 skill** 放在 `.agents/skills/` 下，并在 `.agents/README.md` 维护技能包目录（详见 `AGENTS.md` 开头）。都不写进 `AGENTS.md`——这样这份规范才能保持跨项目通用、不膨胀。

## 推荐工具链（可选）

只用 `AGENTS.md` 已经够用。需要更完整的需求治理与执行纪律时，推荐组合 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**：OpenSpec 管需求与验收，CodeGraph 管影响分析，Superpowers 管执行方法。三者都不替代 `AGENTS.md` 底线，未安装时自动 fallback 到 issue/PR 与人工分析。

开发流程见：[`新需求开发.md`](./新需求开发.md) · [`需求变更.md`](./需求变更.md) · 共用约定 [`流程公共约定.md`](./流程公共约定.md)。

### 工具依赖（可选）

完整流程指向 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**；未装齐时按 [`流程公共约定.md`](./流程公共约定.md) fallback 到 issue/PR、人工读源码、手动澄清与验证（`AGENTS.md` 底线不打折）。

| 工具 | 在使用中的角色 | 在流程中的用途 | 未安装时的 fallback |
| --- | --- | --- | --- |
| **OpenSpec** | **主（核心）** | `openspec/changes/` 承载需求；`/opsx:propose` / `apply` / `sync` / `archive`；`openspec validate` / `archive` CLI | 用 issue、PR、任务卡承载 proposal / spec / tasks / 验收 |
| **CodeGraph** | **辅（影响分析步骤）** | `codegraph_explore` / `callers` / `callees` / `impact` 做影响分析与回归范围评估 | AI 人工通读相关源码与测试，并记录未覆盖范围 |
| **Superpowers** | **辅（执行方法步骤）** | `$brainstorming` 澄清；`$writing-plans` 拆任务；`$test-driven-development` 实现（推荐）；`$verification-before-completion` 核对验证证据 | 手动澄清、按 tasks 推进、完成前自行核对验证证据 |

**推荐顺序（完整使用流程时）**：

1. 复制 `AGENTS.md` + `docs/` + `.agents/`
2. 安装 **OpenSpec** → **CodeGraph** → **Superpowers**（提示词见下方 [通过 AI 安装](#通过-ai-安装)）
3. 按根目录流程文档推进：新需求看 [`新需求开发.md`](./新需求开发.md)，需求变更看 [`需求变更.md`](./需求变更.md)

### 通过 AI 安装

在**业务项目**（勿在本规范仓库）里把下面提示词发给 AI 完成可选工具安装；安装完成后，按各工具小节的「初始化」步骤自行操作。

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
