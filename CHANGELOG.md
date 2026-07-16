# Changelog

本文件记录 **Vibe Coding Rule** 规范包本身的演进。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

### Changed

- `AGENTS.md`：等义压缩重复规则与需求性质决策表，保留单文件可用的六条底线、需求/验证/权限门禁和非目标不变式；不再承载可选工具规则
- `docs/规范/可选工具协作.md`：集中 OpenSpec、CodeGraph、Superpowers 的职责、组合流程、单一事实来源、调用方式、权限限制和 fallback，作为工具协作流程与限制的唯一来源
- `README.md`、`快速开始.md`、`docs/README.md`、`新需求开发.md`、`需求变更.md`、`CONTRIBUTING.md`：明确内容分工——README 负责安装、初始化与重启，工具指南负责协作流程与限制，其他文档只保留导航或场景特有内容
- `AGENTS.md`、`README.md`、`快速开始.md`：接入前区分只含 `AGENTS.md` 的最小安装与递归合并 `docs/` 下全部文件、`.agents/README.md` 的完整安装；完整安装保留项目自有内容和索引，不删除业务项目自行新增的文件或目录；接入与升级提示词只处理当前安装类型的规范文件，选择类型后不再固定二次确认，同版本仍核对文件完整性；工具安装和初始化由用户后续自行取用 README 中的对应提示词
- `流程公共约定.md`：改为新工具协作指南的兼容入口，保留一个发布周期

### Fixed

- `README.md`：按当前上游说明校正 OpenSpec 与 CodeGraph 安装方式，移除固定使用项目依赖的过期假设，改为执行前核对官方 Installation / README 与目标环境
- `README.md`、`快速开始.md`：将 AI 工具重启说明从安装或初始化后的静态结论，修正为执行安装或初始化命令时的条件式检查；仅当后续步骤依赖当前会话尚未加载的 MCP、斜杠命令或技能时暂停并等待重启
- `README.md`：移除 OpenSpec、CodeGraph、Superpowers 安装 / 初始化提示词中重复的权限申请要求，权限控制统一交由实际 AI 工具和运行环境执行
- `docs/规范/可选工具协作.md`、`README.md`、`新需求开发.md`、`需求变更.md`：OpenSpec 授权与调用覆盖当前 AI 工具生成的等价命令或 skill，并明确 `/opsx:*` 只是通用示例记法
- `快速开始.md`：移除操作步骤、示例提示词和记录字段要求，只保留场景选择与后续文档导航

## [1.1.4] - 2026-07-15

### Changed

- `AGENTS.md`：明确项目规范与项目级 skill 按需创建目录；补充需求留痕载体、需求性质决策与 OpenSpec 显式授权门禁；澄清可选工具未启用时的执行边界
- `README.md`：可选工具链改为「安装」与（仅 OpenSpec / CodeGraph）「初始化」两段 AI 提示词；注明 Superpowers 无项目级初始化；重启表标题改为覆盖「仅安装」情形；安装前提标明本仓库推荐；§2 注明流程文档在本规范仓库查阅
- `快速开始.md`：场景三区分 OpenSpec / CodeGraph 需初始化，并链到 `README.md` §4 的重启说明
- `README.md`：重写入口说明——开篇明确仓库定位与用法；接入/升级与 OpenSpec / CodeGraph / Superpowers 的 AI 安装提示词直接展开；补回代码设计原则入口
- `快速开始.md`：聚焦三档选型与工具 fallback 表；安装提示词统一引用 `README.md` §4；场景一链到 AI 接入提示词，场景三与 `docs/` 复制范围对齐
- `新需求开发.md`、`需求变更.md`：重写为面向开发人员的 AI Chat 操作指引，统一步骤、提示词与 fallback 写法，补充 UI 两阶段流程
- `流程公共约定.md`、`README.md`、`docs/README.md`：统一 `<change-id>` 与 `specs` 术语，补充 `/opsx:explore` 说明
- `需求变更.md`：开篇与第 1 步补充「主流程在 AI Chat、终端 CLI 可辅助」说明
- `docs/README.md`、`AGENTS.md`：「进行中需求」用语与表格、目录说明对齐
- `CHANGELOG.md`：修正 v1.1.3 中未落地的 worktree 描述

## [1.1.3] - 2026-07-03

### Added

- `docs/规范/代码设计原则.md`：新增通用代码设计原则模板，覆盖 KISS、YAGNI、DRY、关注点分离、高内聚低耦合、最小惊讶原则及抽象判断清单

### Changed

- `新需求开发.md`：更新新需求开发步骤，使用 `<change-id>` 占位符
- `README.md`、`docs/README.md`：补充代码设计原则模板入口，并同步版本提示

## [1.1.2] - 2026-07-01

### Changed

- `AGENTS.md`：§7 新增「目标变更与非目标不变式」，约束不得通过改变非目标行为间接制造「完成效果」，并补充完成后的不变式核对要求
- `AGENTS.md`、`README.md`：新增版本提示，方便已接入项目判断是否需要同步新版 Vibe Coding Rule
- `AGENTS.md`：新增 §7「实施与改动」（简洁、目标变更与非目标不变式、精准改动）；§1 交叉引用；§7 与 OpenSpec / Superpowers 分工边界（不重复澄清、计划、TDD、change 流程）
- `新需求开发.md`、`流程公共约定.md`、`README.md`：TDD 表述回归 Superpowers 推荐、非 AGENTS 重复
- `README.md`、`快速开始.md`、`CONTRIBUTING.md`、`.agents/README.md`：移除内置项目流程 skill 的安装、索引和手动调用说明；流程入口回到根目录 Markdown 文档
- `README.md`：新增“升级已接入的 Vibe Coding Rule”提示词，指导业务项目同步新版 Vibe Coding Rule 时先 diff、确认方案再写入
- `AGENTS.md`：移除对规则包根目录流程文档的具体文件名引用，避免单文件复制后形成悬空指向
- `快速开始.md`：瘦身为场景选择导航，目录结构、初始化步骤与安装提示词统一回到 `README.md`
- `docs/README.md`：固定 `docs/experience/` 作为经验包 Markdown 目录；调整示例结构，移除默认 `api/` 目录并将接口/API 契约变化归入 OpenSpec change
- `.agents/README.md`：移除对根目录流程文档的相对引用，并调整空索引措辞，避免复制到业务项目后出现断链或语境偏差

### Removed

- `.agents/skills/new-feature-development/`：移除新需求开发流程 skill
- `.agents/skills/requirement-change/`：移除需求变更流程 skill

## [1.1.1] - 2026-06-10

### Removed

- `README.en.md`：入口统一为 `README.md`

### Changed

- `README.md`、`CONTRIBUTING.md`：移除英文 README 引用

## [1.1.0] - 2026-06-10

### Added

- `快速开始.md`：业务项目目录总览、初始化清单、三种落地场景
- `流程公共约定.md`：OpenSpec / Superpowers / CodeGraph 共用约定
- `docs/README.md`：业务项目 docs 目录模板，明确与 OpenSpec 的边界
- `.agents/README.md`：skill 目录索引模板，含 `npx skills add` 说明
- `README.en.md`、`CHANGELOG.md`、`SECURITY.md`、`CODE_OF_CONDUCT.md`
- `.github/pull_request_template.md`

### Changed

- `AGENTS.md`：项目路径约定（`docs/` / `.agents/skills/` / `.agents/README.md`）、`docs/` 与需求变更分工、AI 不得擅自修改本文件
- `README.md`：业务项目推荐目录结构与初始化步骤
- `新需求开发.md`、`需求变更.md`：引用公共约定，统一 TDD 为推荐方法
- 精简仓库结构，移除冗余平行文档

## [1.0.0] - 2026-06-10

### Added

- `AGENTS.md`：六条底线与六章展开（工程、安全、验证、需求可追溯、注释、提交）
- `README.md`：用法、边界、可选工具链安装提示词
- `新需求开发.md`、`需求变更.md`：OpenSpec 主流程指南
- `CONTRIBUTING.md`、`LICENSE`（MIT）

[Unreleased]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.4...HEAD
[1.1.4]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.3...v1.1.4
[1.1.3]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.2...v1.1.3
[1.1.2]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.1...v1.1.2
[1.1.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/releases/tag/v1.0.0
