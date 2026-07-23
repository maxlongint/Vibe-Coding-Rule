# Changelog

本文件记录 **Vibe Coding Rule** 规范包本身的演进。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

## [3.2.3] - 2026-07-24

### Changed

- `AGENTS.md`：版本改为 v3.2.3；需求承接改由用户明确指定——AI 不得根据实现、改动范围、技术类别或可观察结果自行分类、要求确认或启动 OpenSpec；仅当用户已指定承接方向且与已确认口径冲突，或用户明确要求判断承接关系但边界不清时，才须说明依据并请求用户决定承接方式；收紧「拿不准就问」至承接冲突场景；移除 §2.1 缺陷修复分类条款及 §4 对应引用
- `README.md`、`CHANGELOG.md`：同步版本标识

## [3.2.2] - 2026-07-23

### Changed

- `AGENTS.md`：版本改为 v3.2.2；明确「缺陷修复不按技术类别判断是否属于需求变更」——有已确认口径证明违背既有行为时属恢复，不因样式、布局、交互、接口、数据或业务逻辑自动触发 OpenSpec；重新定义目标、范围、可观察行为、接口、数据、权限、流程或验收才属需求变更；证据不足须先确认
- `规则干条.md`、`需求变更工作流.md`、`README.md`、`CHANGELOG.md`：同步上述口径与版本标识

## [3.2.1] - 2026-07-23

### Changed

- `AGENTS.md`：版本改为 v3.2.1；新增「用户提供内容不得擅自落盘」——经验、素材、偏好或讨论结论须经用户明确要求保存并指定或确认归属后才可写入仓库；归属不明时先询问
- `规则干条.md`、`README.md`、`CHANGELOG.md`：同步上述口径与版本标识

## [3.2.0] - 2026-07-22

### Changed

- `AGENTS.md`：版本改为 v3.2.0；新增需求与新建关联 change 改为「口径确认后由用户主动授权创建 → AI 生成 `change-id` 并做创建前冲突检查」，无需用户另行确认 AI 生成的 ID；需求变更或已有需求仍须确认继续原 change / 新建关联 change
- `docs/规范/工具协作.md`、`docs/README.md`、`README.md`、`CONTRIBUTING.md`、`规则干条.md`、`新增需求工作流.md`、`需求变更工作流.md`：同步上述分场景写入授权与冲突检查口径；工作流提示词改为单步创建（生成 ID、冲突检查、无冲突则创建），澄清阶段显式使用 Superpowers `brainstorming`

## [3.1.0] - 2026-07-22

### Changed

- `AGENTS.md`：版本改为 v3.1.0；强化需求记录、用户确认、验证、关键写入和外部副作用等强制门禁；明确修改本规范时不得用正在审阅的条文形成 OpenSpec 或工具流程循环门禁
- `AGENTS.md`、`docs/README.md`、`docs/规范/工具协作.md`：明确 `AGENTS.md` 常驻需求识别、两次用户确认与按需路由，`docs/` 作为长期知识库按索引读取；AI 识别潜在新增需求或需求变更不构成执行授权，需求意图确认前不得查找 OpenSpec 候选，具体 change 确认前不得创建、选定、更新载体或形成最终设计、计划、实施
- `新需求开发.md`、`需求变更.md`：分别更名为 `新增需求工作流.md`、`需求变更工作流.md`，明确其为仅供本规范仓主动查阅的工作流教程；同步现行导航与交叉引用，历史版本记录保留旧文件名
- `README.md`、`CONTRIBUTING.md`、`规则干条.md`、`新增需求工作流.md`、`需求变更工作流.md`：同步 OpenSpec 的分阶段授权顺序——先确认需求意图，再只读查找候选或在需求口径明确后提出新 ID，最后由用户确认具体 change；工具仅在任务实际需要时检查
- `README.md`、`CHANGELOG.md`：将 v3.0.0 的受管文件改名、删除与接入范围迁移细则归入对应版本的 `Migration`；README 仅保留稳定升级入口

## [3.0.0] - 2026-07-21

### Changed

- **Breaking**：接入方式收敛为整套规范，并将受管文件 `docs/规范/可选工具协作.md` 更名为 `docs/规范/工具协作.md`；升级须按迁移规则处理旧路径，避免两份当前工具规范并存
- **Breaking**：默认假定 OpenSpec、CodeGraph、Superpowers 均已成功安装；执行中需要使用却发现未安装或不可用时必须中断并指明工具，不得静默降级或改用轻量载体继续；移除“按需启用 / 未启用不阻塞”与新需求、需求变更中的“未启用 OpenSpec”分支
- **Breaking**：进行中需求只以 OpenSpec change 为唯一载体；移除「用户可指定 issue/PR/任务卡等为唯一载体」例外。issue/PR 等仅作输入来源，确认后必须写入 change；OpenSpec 不可用时中断，不得改用它们继续
- **Breaking**：接入受管范围收敛为 `AGENTS.md`、`docs/` 下全部文件、`design/README.md`、`.agents/README.md`；`新需求开发.md` / `需求变更.md` 仅留规范仓，不再复制到业务项目（升级时按本版本 `Migration` 处理业务项目残留）
- **Breaking**：删除根目录兼容入口 `流程公共约定.md`；规则以 `docs/规范/工具协作.md` 为准；业务项目残留该文件时按本版本 `Migration` 处理
- **Breaking**：删除 `快速开始.md`；入口导航改由 `README.md`「日常协作」承接；接入/升级受管清单不再包含该文件
- **Breaking**：删除 `docs/规范/UI任务重组.md`，UI 两阶段规则并入 `docs/规范/工具协作.md` §5；升级须按本版本 `Migration` 处理业务项目残留，避免与 `工具协作.md` 两份 UI 规则并存
- `AGENTS.md`：版本改为 v3.0.0；重组为核心规则层；速查「不伪造成功」澄清隔离 mock 与不得冒充真实依赖的边界；工具条款改为默认已安装与未安装中断
- `docs/规范/工具协作.md`：作为三项工具与 UI 两阶段重组的唯一来源；补充「Superpowers 与载体优先级」与 CodeGraph 按需查询；规定阶段标题原文（顿号与“与”），禁止近义改写；明确非 UI 场景不读取 §5；删除「§6 调用方式」（由运行环境发现，不在规范中教写）
- `新需求开发.md`、`需求变更.md`：阶段标题与工具协作规定原文对齐；关键提示词写死 Superpowers 只作方法、只回填 change、禁止平行文档与自动提交；说明开头预检 OpenSpec 的原因，以及后续用到 CodeGraph/Superpowers 时同样中断；标明仅规范仓使用
- `docs/README.md`：索引去掉 `UI任务重组.md`；收紧工具协作按需读取范围为 §1–4 与条件读取 §5；唯一载体口径改为仅 OpenSpec change
- `README.md`、`CONTRIBUTING.md`：统一“假定已安装 → 需要时不可用则中断”口径；规范组成与接入提示词仅含受管文件；排除工作流文档与 `规则干条.md`；「日常协作」前移为教程入口；恢复三项工具的精简安装提示词（与接入提示词分离）
- `规则干条.md`：仅规范仓维护者速查，不接入业务项目；内容与正式文档对齐

### Removed

- `流程公共约定.md`：兼容入口到期移除
- `快速开始.md`：与 README 日常协作导航重复，予以移除
- `docs/规范/UI任务重组.md`：内容并入 `docs/规范/工具协作.md` §5 后删除
- `docs/规范/工具协作.md` 原「§6 调用方式」及对各文档中调用方式 / §6 引用的清理

### Migration

从较早版本升级到 v3.0.0 时，应区分“规范仓库已移除或改名的受管文件”和“业务项目自行新增的文件”。本版本须按下列路径迁移，规则相同：

1. `docs/规范/可选工具协作.md` → `docs/规范/工具协作.md`（更名）
2. `docs/规范/UI任务重组.md` → 并入 `docs/规范/工具协作.md` §5 后删除（不再单独维护）

处理方式：

- 旧文件与上一版本原文一致时，落盘新内容、更新 `docs/README.md` 索引后删除旧文件；
- 旧文件含业务项目自定义内容时，先展示差异，把仍有效的项目内容合并到 `工具协作.md` 或其他合适载体，等待用户确认后再删除旧文件；
- 无法判断来源或无法安全迁移时停止；不得保留两份当前工具规范或两份 UI 两阶段规则，也不静默删除。

以后发生受管文件改名或删除时，也必须在对应版本的迁移说明中明确列出，不能用“保留项目已有文件”跳过迁移。

v3.0.0 从接入范围移除并停止维护业务项目侧的下列文件（规范仓内仍保留工作流原文，供本仓库查阅）：

- `流程公共约定.md`、`快速开始.md`：已删除；业务项目残留且仅为旧迁移说明、导航或与上一版本原文一致时可删；含自定义内容时先展示差异再确认。
- `新需求开发.md`、`需求变更.md`：不再复制到业务项目；业务项目若曾接入且内容与规范仓上一版本原文一致，升级时可删除；含项目自定义流程时先展示差异并确认后再处理。

下列文件仅存在于本规范仓，不属于接入或升级受管文件，不得复制到业务项目：`规则干条.md`、`新需求开发.md`、`需求变更.md`、本仓库 `README.md`、`CHANGELOG.md`、`CONTRIBUTING.md` 等开源元文档。

## [2.1.1] - 2026-07-20

### Changed

- `新需求开发.md`、`需求变更.md`：UI 设计图流程改为先固化原始任务基线或本轮更新前快照，再读取设计来源并重组；阶段实现与验证重新读取落盘载体，不依赖之前的 Chat 或上下文摘要
- `新需求开发.md`、`需求变更.md`、`docs/规范/可选工具协作.md`：UI 设计输入支持单一或多个互补的权威来源，要求记录各来源作用和冲突优先级，并允许显式沿用 `design.md` 已记录且仍可读取的来源集合
- `docs/README.md`：补充跨 Chat 与上下文压缩场景的工具协作规范读取要求

### Fixed

- `新需求开发.md`、`需求变更.md`、`docs/规范/可选工具协作.md`：防止 UI 两阶段重组删除、压缩或弱化原任务；基线、映射和设计图新增目录改为非 checkbox 追溯区，只有阶段 1 / 阶段 2 保留可执行 checkbox
- `docs/规范/可选工具协作.md`：统一跨阶段任务的 `-S1` / `-S2` 编号、全文件 checkbox 唯一性与映射机械核对，补充旧错误结构迁移和半重组检测，避免重复计数、阶段 1 提前完成原任务或残缺结构逃过验证
- `新需求开发.md`、`需求变更.md`：补充任务语义重叠检查；仅切分重叠职责，保留页面独有交互的责任任务，避免多个 checkbox 重复承诺同一实现或因模板化拆分产生遗漏

## [2.1.0] - 2026-07-17

### Added

- `design/README.md`：新增业务项目设计资料目录说明模板，约定 `specs/`、`ui/`、`assets/` 的用途以及与程序运行时资源的边界；普通设计文件无需维护逐文件索引

### Changed

- `AGENTS.md`、`docs/README.md`：明确 `docs/` 是 AI 按当前任务相关性读取的长期项目知识库，先读取索引再加载相关规范，不在每次交互中全量加载
- `README.md`：根入口增加 `design/` 说明，完整安装包含 `design/README.md`；目标文件内容冲突时展示差异摘要并由用户选择保留、覆盖或人工确认后合并，且不修改 `design/` 下其他文件
- `CONTRIBUTING.md`：同步设计资料目录的内容归属和维护者文件清单

## [2.0.0] - 2026-07-16

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

[3.2.3]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v3.2.2...v3.2.3
[3.2.2]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v3.2.1...v3.2.2
[3.2.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v3.2.0...v3.2.1
[3.2.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v3.1.0...v3.2.0
[3.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v3.0.0...v3.1.0
[3.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v2.1.1...v3.0.0
[2.1.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v2.1.0...v2.1.1
[2.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.4...v2.0.0
[1.1.4]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.3...v1.1.4
[1.1.3]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.2...v1.1.3
[1.1.2]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.1...v1.1.2
[1.1.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/releases/tag/v1.0.0
