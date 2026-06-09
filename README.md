# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

> **EN:** AI collaboration rules for frontend, backend, and full-stack projects. [`AGENTS.md`](./AGENTS.md) works standalone; **recommended:** AGENTS.md + OpenSpec (primary) + CodeGraph + Superpowers (auxiliary).

面向 AI 辅助开发的通用协作规则包：跨项目、前后端通用。

- **可独立使用：** 仅复制 [`AGENTS.md`](./AGENTS.md) 即可（Minimal 档位），不依赖 OpenSpec、CodeGraph、Superpowers 或任何特定 AI 插件。
- **推荐组合：** **Full 档位** — `AGENTS.md` + 三个可选工具一起用：**OpenSpec 管需求与验收（主）**，CodeGraph 管影响分析，**Superpowers 管执行纪律（辅）**。未安装的工具按 AGENTS.md 自洽规则 fallback。

仓库：<https://github.com/maxlongint/Vibe-Coding-Rule> · 协议：[MIT](./LICENSE) · 贡献：[CONTRIBUTING.md](./CONTRIBUTING.md)

## Quick start

在 Cursor 等 AI 编码工具中**打开目标业务项目**（不是本规则仓库），选定档位后复制对应提示词发送即可。安装完成后可新开对话，让 AI 简述「本项目须遵守的 AGENTS.md 底线」以验证生效。

### Minimal 档位

```text
请为当前业务项目安装 Vibe Coding 规则包（Minimal 档位）。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
只复制 AGENTS.md 到当前项目根目录；不要复制 docs/规范/、.agents/skills/，也不要在规则包来源仓库里做业务项目初始化。
若当前项目已有 AGENTS.md，先说明差异与合并方案，经我确认后再写入。
写入前请先列出目标路径并申请权限。
安装完成后说明 AGENTS.md 是否已被当前 AI 工具识别，以及 Minimal 档位下仍需遵守的底线（见 AGENTS.md 速查）。
```

### Standard 档位

```text
请为当前业务项目安装 Vibe Coding 规则包（Standard 档位）。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
按以下清单从规则包来源复制到当前业务项目：
1. AGENTS.md → 项目根目录
2. docs/规范/需求治理与文档结构.md → 项目 docs/规范/
3. （推荐）.github/pull_request_template.md → 项目 .github/
4. （推荐）docs/规范/模板/轻量记录模板.md → 项目 docs/规范/模板/
若目标路径已存在同名文件，先 diff 并说明合并策略，经我确认后再写入。
写入前请先列出全部目标路径并申请权限。
安装完成后说明已安装文件列表、是否需补充 docs/规范/规范总览.md（Standard 可选），以及需求变化时应打开的文档入口。
```

### Full 档位

```text
请为当前业务项目安装 Vibe Coding 规则包（Full 档位）。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
在当前业务项目根目录完成以下步骤（不要在规则包来源仓库里初始化 openspec/ 或 .codegraph/）：

【规则与规范】
1. 复制 AGENTS.md 到项目根目录
2. 复制 docs/规范/ 下全部文件到项目 docs/规范/（含 规范总览.md、需求治理与文档结构.md、AI协作与工具链.md、模板/）
3. 复制 .agents/skills/ 到项目 .agents/skills/
4. 复制 .github/pull_request_template.md 到项目 .github/

【推荐工具（在当前业务项目内安装；OpenSpec 为主、Superpowers 为辅）】
5. OpenSpec（推荐）：npm install -D @fission-ai/openspec@latest，再 npx openspec init
6. CodeGraph（推荐）：项目级 MCP（local），codegraph init -i
7. Superpowers（推荐）：按当前 AI 工具官方说明安装
三者缺一时按 AGENTS.md 自洽规则 fallback；若用户明确只要 Minimal/Standard 或暂不装某工具，经确认后跳过。

【通用约束】若目标路径已存在同名文件，先 diff 并说明合并策略，经我确认后再写入；各步写入或执行前申请权限。
【OpenSpec 步骤】执行 npm/npx 前，若项目存在 .node-version，按 AGENTS.md 切换 Node 版本。
全部完成后列出：已复制文件、已初始化目录、验证命令结果，以及是否需要重启 AI 工具。
```

推荐工具安装细节、跨目录安装与其它提示词见 [docs/安装与AI提示词.md](./docs/安装与AI提示词.md)。

## 这是什么

本仓库是**可复制的规则包**，不是业务项目本身。不要在规则仓库里初始化 `openspec/` 或 `.codegraph/`。

| 内容 | 放哪里 |
| --- | --- |
| 强制协作底线（工程、验证、提交、优先级） | [`AGENTS.md`](./AGENTS.md) |
| 需求治理、工具链分工 | [`docs/规范/`](./docs/规范/) |
| 技术栈、常用命令 | 业务项目 `README.md` |
| UI/API/业务约定 | 业务项目 `docs/规范/` |
| 真实需求与验收 | issue、PR、OpenSpec change 等 |

## 三档选择

| 档位 | 复制内容 | 适用 |
| --- | --- | --- |
| **Minimal** | `AGENTS.md` | 个人、小项目、快速试点；只要底线、暂不装工具链 |
| **Standard** | Minimal + [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) + PR 模板（推荐） | 有文档治理、暂不装 OpenSpec 等工具的团队 |
| **Full** | Standard + 全部 `docs/规范/` + [`.agents/skills/`](./.agents/skills/) + **推荐**安装 OpenSpec / CodeGraph / Superpowers | **团队默认推荐**；复杂产品与完整 AI 协作 |

**推荐路径：** Full 档位 + 三工具全套。OpenSpec 负责需求、规格、任务与验收；Superpowers 负责澄清、TDD、调试、审查等执行纪律，**不替代** OpenSpec 的需求主流程。详见 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)。

档位详情见 [AGENTS.md 规则包档位](./AGENTS.md#规则包附带文档)。

## 日常协作四件事

| 时机 | 做什么 |
| --- | --- |
| 改行为 / 接口 / 数据 / 权限前 | 先有可追溯记录（issue、PR、任务卡或轻量说明） |
| 改代码前 | 做影响分析（有 CodeGraph 等工具时优先） |
| 完成前 | 跑验证并说明结果；跑不了则写原因和剩余风险 |
| 提交 / 发布 | 仅当用户明确要求；AI 不得擅自 commit / push / deploy |

## 推荐工具链（Full 档位）

`AGENTS.md` 为必需基线；下表三工具 **推荐一起安装**，缺省时 fallback，不影响 AGENTS.md 效力。

| 组件 | 职责 | 角色 | 缺失时 |
| --- | --- | --- | --- |
| [`AGENTS.md`](./AGENTS.md) | 规则、验证、优先级 | **必需** | — |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 需求、规格、任务、验收 | **主**（需求治理） | issue / PR / 轻量记录 |
| [CodeGraph](https://github.com/colbymchenry/codegraph) | 调用链、影响分析 | 推荐 | IDE 引用、人工阅读 |
| [Superpowers](https://github.com/obra/superpowers) | AI 执行纪律（TDD、调试、审查） | **辅**（执行方法） | 通用习惯 + 项目 skills |

安装命令与 AI 提示词见 [docs/安装与AI提示词.md](./docs/安装与AI提示词.md)。

## 文档索引

| 文档 | 用途 |
| --- | --- |
| [`AGENTS.md`](./AGENTS.md) | 强制规则入口 |
| [`docs/规范/规范总览.md`](./docs/规范/规范总览.md) | 规范目录索引 |
| [`docs/规范/需求治理与文档结构.md`](./docs/规范/需求治理与文档结构.md) | 治理模式、需求变化与归档 |
| [`docs/规范/AI协作与工具链.md`](./docs/规范/AI协作与工具链.md) | 工具分工、任务分流 |
| [`docs/安装与AI提示词.md`](./docs/安装与AI提示词.md) | 复制步骤、AI 安装提示词、推荐工具 |
| [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md) | 新需求流程 |
| [OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md) | 需求变更流程 |
| [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) | 工具配合与场景分流 |
| [SSO 示例](./SSO%20单点登录复杂需求执行步骤示例.md) | 复杂需求教学（非真实需求） |
| [`.agents/skills/`](./.agents/skills/) | 复杂需求 / 需求变化技能包 |

## 按场景选文档

| 场景 | 优先 |
| --- | --- |
| 初次了解 / 只用底线 | [`AGENTS.md`](./AGENTS.md) 速查 |
| 普通新需求（有 OpenSpec） | [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md) |
| 普通新需求（无 OpenSpec） | AGENTS.md 需求治理 + issue/PR |
| 复杂、高风险、跨模块 | 点名 `$executing-complex-requirements` |
| 已有需求范围或验收调整 | [OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md)；结构化同步可点名 `$updating-requirement-changes`（**仅点名触发**） |

**OpenSpec 为主、Superpowers 为辅：** 有 `openspec/` 时走 OpenSpec change 主流程；Superpowers 仅补充澄清、实现与验证纪律，不替代需求记录。详见 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)。

## 已有规则文件的项目

先合并职责与优先级，再决定全量采用或只补充 Vibe Coding 执行规则。项目差异写在业务项目 `docs/规范/` 或需求记录中，**不要**把通用 `AGENTS.md` 改成各项目不同的业务说明。

## 许可证

本项目采用 [MIT License](./LICENSE)。你可以自由复制 `AGENTS.md` 及本仓库其他文件到商业或非商业项目；保留版权声明即可。
