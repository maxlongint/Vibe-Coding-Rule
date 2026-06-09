# Vibe Coding 技能包与项目搭建指南

本文说明如何使用本仓库提供的通用 `AGENTS.md` 和项目级技能包。**所有档位**以 `AGENTS.md` 为跨项目、工具无关的强制底线；**Full 档位**可进一步搭建 `AGENTS.md + CodeGraph + OpenSpec + Superpowers` 四件套。核心不是把工具堆满，而是让 AI 开发过程同时具备四个能力（Full 档位）：

-   `AGENTS.md`：定义强制底线、权限边界、验证规则与优先级（跨项目、工具无关）。
-   OpenSpec：承载需求变化、规格、任务、验收标准和归档记录。
-   CodeGraph：提供代码事实、调用链、依赖关系和影响范围分析。
-   Superpowers：提供 AI 执行方法，例如需求澄清、调试、TDD、计划、审查和完成前验证。

四件套角色分工（**Full 档位**）：

```text
AGENTS.md   定规则和边界
OpenSpec    定需求、规格、任务和验收
CodeGraph   定代码事实和影响范围
Superpowers 定 AI 怎么推进工作
```

**Full 档位** AI 执行顺序（摘要自 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)；冲突处理见 [AGENTS.md](./AGENTS.md) 第 5 节）：

```text
1. 确认已加载 AGENTS.md 中的规则、权限和验证底线（工具默认识别，无需每轮粘贴）
2. 判断任务类型，按需检查或创建 OpenSpec change
3. 涉及代码改动时，须先完成影响分析（有代码分析工具时优先，如 CodeGraph；否则人工分析并记录风险）
4. 按需启用 Superpowers 做澄清、TDD、调试和完成前验证
5. 更新需求记录后再进入实现
6. 完成后说明验证结果和剩余风险
```

## 内容目录

-   [文档地图](#文档地图)
-   [本仓库定位](#本仓库定位)
-   [AGENTS.md 说明与使用](#agentsmd-说明与使用)
-   [通过 AI 安装规则包](#通过-ai-安装规则包)
-   [安装 CodeGraph](#安装-codegraph)
-   [安装 OpenSpec](#安装-openspec)
-   [安装 Superpowers](#安装-superpowers)
-   [步骤文档、技能与示例怎么选](#步骤文档技能与示例怎么选)
-   [技能包使用](#技能包使用)
    -   [复杂需求执行：`$executing-complex-requirements`](#复杂需求执行executing-complex-requirements)
    -   [需求变化同步：`$updating-requirement-changes`](#需求变化同步updating-requirement-changes)
-   [OpenSpec 使用](#openspec-使用)
-   [流程教程与示例](#流程教程与示例)

## 文档地图

本仓库文档按层级组织，阅读时可按「规则 → 入门 → 流程 → 规范 → 执行 → 示例」递进：

| 层级 | 文档 | 用途 |
| --- | --- | --- |
| 规则层 | [AGENTS.md](./AGENTS.md) | 强制协作章程；复制到业务项目后作为 AI 规则入口 |
| 入门层 | 本 README | 仓库定位、工具安装、场景分流和文档导航 |
| 流程层 | [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md)、[OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md)、[Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) | 分场景的人类可读操作步骤 |
| 规范层 | [docs/规范/](./docs/规范/)（索引见 [规范总览.md](./docs/规范/规范总览.md)） | 内含 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)、[AI协作与工具链.md](./docs/规范/AI协作与工具链.md) 等；项目特有规则由业务项目在规范总览中索引；工程/提交底线只在 AGENTS.md |
| 执行层 | [.agents/skills/README.md](./.agents/skills/README.md) 及各 `SKILL.md` | AI 在任务中按需加载的执行方法 |
| 示例层 | [SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md) | 复杂需求的端到端教学示范，非真实需求记录 |

**首次阅读路径：** 本 README [本仓库定位](#本仓库定位) → [AGENTS.md](./AGENTS.md) 速查 → [步骤文档、技能与示例怎么选](#步骤文档技能与示例怎么选) → 按场景打开对应流程文档。

## 本仓库定位

GitHub 地址：<https://github.com/maxlongint/Vibe-Coding-Rule>

本仓库是 Vibe Coding 的通用规则包和技能包，不是某个业务项目本身。因此本仓库不需要初始化 `.codegraph/` 或 `openspec/`；这些目录应在实际采用本规则的业务项目中按需创建。

本仓库根目录的 `AGENTS.md` 是跨项目复用的固定基线，负责定义不可跳过的协作规则、权限边界、需求治理底线、验证底线和优先级。不同项目会变化的技术栈、业务规范、目录适配、命令说明和领域规则，不应继续写入本仓库的 `AGENTS.md`；应放在使用方项目自己的文档中（见下表）。

| 内容类型 | 业务项目推荐落点 | 本规则仓库的做法 |
| --- | --- | --- |
| 项目环境、命令、文档入口 | 根 `README.md` 或 `docs/README.md` | 根 `README.md`（本文件） |
| 项目特有规范 | `docs/规范/`（Standard 起至少含需求治理文档） | 示例索引见 `规范总览.md`；工程/提交底线只在 AGENTS.md 第 1–4 节 |
| 真实需求与验收 | `openspec/changes/` 或外部需求系统 | 不预建 OpenSpec change |
| 流程教程与长示例 | `docs/规范/` 或由 README 链接 | 放根目录便于分发（见下段说明） |

**关于根目录教程文档：** [AGENTS.md](./AGENTS.md) 建议业务项目把长教程沉淀到 `docs/规范/`。本仓库作为可复制的规则包，将 OpenSpec 步骤、Superpowers 对照指南和 SSO 教学示例放在根目录，方便一次性拷贝和链接；这是分发形态的例外，不代表业务项目也应把同类文档长期堆在根目录。

根目录教程与示例文件：

- [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md)、[OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md)、[Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)：流程教程
- [SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md)：复杂需求教学示例，展示如何串联四件套；不是本仓库真实需求，也不要求创建对应 OpenSpec change

## AGENTS.md 说明与使用

[AGENTS.md](./AGENTS.md) 是 Vibe Coding 的**项目协作章程**，也是整套协作体系的规则入口。它定义需求治理底线、权限边界、验证底线、提交规范与规则优先级；四件套分工、技能触发与任务分流见 `docs/规范/`（Full 档位），不替代 `AGENTS.md` 中的强制规则。

主流 AI 编码工具（如 Cursor）会**默认识别**业务项目根目录的 `AGENTS.md`，日常协作**无需每轮对话重复粘贴**规则提示词；复制到项目根目录后即可生效。

### 说明

本仓库根目录的 `AGENTS.md` 是**跨项目复用的固定基线**，可直接复制到业务项目根目录使用。完整规则见 [AGENTS.md](./AGENTS.md)，主要包括：

| 位置 | 内容 |
| --- | --- |
| [AGENTS.md](./AGENTS.md) 速查 + 第 1–5 节 | 跨项目强制底线（工具无关）：工程守则、注释、编码、提交、规则优先级 |
| [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) | 目录结构、OpenSpec 触发、治理模式、需求变化与归档（Standard / Full） |
| [AI协作与工具链.md](./docs/规范/AI协作与工具链.md) | 四件套分工、技能包、任务分流、执行顺序（Full） |

第 1–4 节（工程、注释、编码、提交）是跨项目强制规则，**完整写在 `AGENTS.md`**，不下沉到 `docs/规范/`。

以下内容**不应**写入本仓库这份通用 `AGENTS.md`，应放在业务项目自己的文档中：

- 技术栈、框架、常用命令 → 项目 `README.md`
- 项目特有规范（UI、API、业务约定等） → `docs/规范/`
- 真实需求、验收标准、任务拆分 → OpenSpec change 或外部需求系统
- 特定场景的推进方法 → 项目级 `.agents/skills/`

### 使用

**落地到业务项目：**

先选定档位（与 [AGENTS.md](./AGENTS.md) 文末「规则包档位」一致），再复制对应文件：

| 档位 | 必复制 | 说明 |
| --- | --- | --- |
| **Minimal** | `AGENTS.md` | 速查与第 1–5 节已含工具无关强制底线；**无需**复制 `docs/规范/` |
| **Standard** | `AGENTS.md` + `docs/规范/需求治理与文档结构.md` | 需求变化、OpenSpec 触发与归档细则；推荐同时复制 PR 模板与 [轻量记录模板.md](./docs/规范/模板/轻量记录模板.md) |
| **Full** | Standard 内容 + 全部 `docs/规范/` + `.agents/skills/` + 四件套工具 | 含 AI 工具链分工、任务分流表 |

**复制步骤：**

0. 若尚未获取规则包，可先克隆：`git clone https://github.com/maxlongint/Vibe-Coding-Rule`
1. 复制 `AGENTS.md` 到业务项目根目录（**所有档位必需**）。
2. **Standard / Full：** 建立 `docs/规范/`，复制 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)（**Standard 起为必需，不是可选**）。
3. **Full：** 继续复制 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)、[规范总览.md](./docs/规范/规范总览.md)、[模板/](./docs/规范/模板/) 等剩余规范文件。
4. **Standard / Full（推荐）：** 复制 `.github/pull_request_template.md` 与 [轻量记录模板.md](./docs/规范/模板/轻量记录模板.md)。
5. **Full：** 安装并初始化 [CodeGraph](#安装-codegraph)、[OpenSpec](#安装-openspec)、[Superpowers](#安装-superpowers)；复制 `.agents/skills/` 或按需选取技能包。
6. **（可选，各档位）：** 建立 `docs/README.md` 作为项目文档入口；在 `docs/规范/` **追加**本项目特有规则（见 [规范总览.md](./docs/规范/规范总览.md)）。
项目差异（技术栈、命令、业务规范）写在项目自己的 `README.md`、`docs/规范/` 或 OpenSpec 中，不要写进通用 `AGENTS.md`。

把 `AGENTS.md` 放到项目根目录后，AI 会按其中速查与第 1–5 节执行。**Full 档位**另按 `docs/规范/AI协作与工具链.md` 与 `.agents/skills/` 决定是否启用 OpenSpec、CodeGraph、Superpowers 及项目级技能。

**Full 档位**需要额外点名时，可在对话中说明，例如复杂需求使用 `$executing-complex-requirements`、需求变化同步使用 `$updating-requirement-changes`（后者**仅点名触发**，详见 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)）。

如果项目已有自己的规则文件，应先合并规则职责和优先级，再决定是直接采用本仓库模板，还是只补充其中的 Vibe Coding 执行规则。项目差异尽量通过项目规范或需求记录承载，避免让通用 `AGENTS.md` 变成每个项目都不同的业务说明文件。

## 通过 AI 安装规则包

以下提示词用于让 AI 把 Vibe Coding 规则包**安装到当前业务项目**。规则包应落在**使用方项目**，不要在本规则仓库内初始化 `openspec/`、`.codegraph/` 或复制业务项目结构。

**规则包来源（默认）：** <https://github.com/maxlongint/Vibe-Coding-Rule>

若你已在本地克隆该仓库，可在发送提示词时把来源改成本地目录；否则 AI 应从上述 GitHub 仓库获取文件（必要时先 clone 到临时目录）。

**使用前请确认：**

1. 已在 AI 工具中打开**目标业务项目**（不是本规则仓库）。
2. 已选定档位：**Minimal**、**Standard** 或 **Full**（见 [AGENTS.md](./AGENTS.md) 文末「规则包档位」）。

**通用约束（各档提示词均适用）：**

- 写入文件前先列出将要创建或覆盖的路径，并向用户申请权限。
- 若目标项目已有 `AGENTS.md` 或 `docs/规范/`，先 diff 说明冲突与合并策略，**不得静默覆盖**。
- 安装完成后列出实际复制的文件，并说明下一步（如初始化 OpenSpec、重启 AI 工具）。

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

【四件套工具（在当前业务项目内安装）】
5. OpenSpec：npm install -D @fission-ai/openspec@latest，再 npx openspec init
6. CodeGraph：项目级 MCP（local），codegraph init -i
7. Superpowers：按当前 AI 工具官方说明安装

若某文件已存在，先 diff 并说明合并策略；Node 命令前检查 .node-version。
各步写入或执行前申请权限；工具安装细节可参考规则包 README 中 CodeGraph、OpenSpec、Superpowers 的「通过 AI 安装」小节。
全部完成后列出：已复制文件、已初始化目录、验证命令结果，以及是否需要重启 AI 工具。
```

### 仅补充项目级技能（已有 AGENTS.md 时）

```text
请从 Vibe Coding 规则包仅复制项目级技能到当前业务项目。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
复制 .agents/skills/ 到当前项目 .agents/skills/；若目录已存在，只新增 executing-complex-requirements、updating-requirement-changes，不要覆盖项目自有技能。
写入前列出路径并申请权限；完成后说明技能索引位置（.agents/skills/README.md）及 updating-requirement-changes 仅点名触发的规则。
```

### 从规则包仓库安装到另一项目

若当前对话打开的是**规则包仓库**，而目标业务项目在别的目录，发送前先补两行：

```text
目标项目：（填写业务项目根目录）
档位：Minimal | Standard | Full
```

再发送：

```text
我要把 Vibe Coding 规则包装到上述目标项目，不是当前打开的仓库。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
请先确认目标路径存在且为业务项目根目录，再按 README「通过 AI 安装规则包」中对应档位的步骤执行；所有文件写入目标项目，不要修改规则包来源仓库。
```

## 安装 CodeGraph

GitHub 地址：<https://github.com/colbymchenry/codegraph>

CodeGraph 用于让 AI 从已索引的代码图中理解项目，而不是每次都从零开始搜索文件。它适合用于找功能入口、查调用关系、判断修改影响范围，以及辅助 bug 定位、重构和代码审查。

**CodeGraph 应安装在使用方当前业务项目中，而不是本规则仓库。** 进入目标项目根目录后再执行安装和初始化；MCP 配置优先选择**项目级（local）**，让 `.codegraph/` 索引和 AI 配置随项目一起管理。

### 通用安装方式

请优先查看 [CodeGraph 官方 README](https://github.com/colbymchenry/codegraph)，在**当前业务项目根目录**按系统和 AI 编码工具完成安装。常见路径包括 `npx @colbymchenry/codegraph`，或先安装 CLI 后执行项目级配置。

推荐步骤：

1. 进入目标项目根目录：`cd your-project`
2. 运行安装器，**MCP 配置选择项目级**：`codegraph install --target=auto --location=local --yes`（交互式安装时选择「仅当前项目」）
3. 在项目根目录建立索引：`codegraph init -i`
4. 运行 `codegraph --version` 验证 CLI 可用
5. 重启 AI 编码工具，让项目级 MCP 配置生效

具体命令和平台差异以官方文档为准。

### 通过 AI 安装

```text
请为当前业务项目安装 CodeGraph，不要在本规则仓库中初始化。
先进入当前项目根目录，再从 https://github.com/colbymchenry/codegraph 查看官方 README 和安装脚本。
MCP 配置优先选择项目级（local），不要默认写入全局配置。
如果需要执行网络下载、修改配置或写入项目目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，并在当前项目根目录执行 codegraph init -i。
最后说明生成了哪些项目级配置文件和索引文件，以及是否需要重启当前 AI 工具。
```

## 安装 OpenSpec

GitHub 地址：<https://github.com/Fission-AI/OpenSpec>

OpenSpec 用于把需求变化从聊天记录里拉出来，形成可追溯的 proposal、design、tasks 和 spec。它适合承载新业务能力、API 与数据模型变化、中等和重大需求变更，以及验收标准和任务拆分。

**OpenSpec 应安装在使用方当前业务项目中，而不是本规则仓库。** 进入目标项目根目录后安装 CLI 并执行 `openspec init`，让 `openspec/` 目录、配置和 AI 指令文件随项目一起管理。

### 通用安装方式

OpenSpec 要求 Node.js 20.19.0 或更高版本。请优先查看 [OpenSpec 官方 README](https://github.com/Fission-AI/OpenSpec) 和安装文档，在**当前业务项目根目录**完成安装。

推荐步骤：

1. 进入目标项目根目录：`cd your-project`
2. 将 OpenSpec 安装为项目依赖（推荐）：

```powershell
npm install -D @fission-ai/openspec@latest
npx openspec --version
```

也可使用 pnpm、yarn 或 bun 的项目级安装方式；具体以官方文档为准。
3. 在当前项目根目录初始化：`npx openspec init`
4. 确认项目内已生成 `openspec/` 目录及相关 AI 指令文件

如果项目存在 `.node-version`，执行 Node 相关命令前先切换到对应版本（例如 `fnm use`）。

### 通过 AI 安装

```text
请为当前业务项目安装 OpenSpec，不要在本规则仓库中初始化。
先进入当前项目根目录，再从 https://github.com/Fission-AI/OpenSpec 查看官方 README。
OpenSpec 优先安装为当前项目依赖（例如 npm install -D @fission-ai/openspec@latest），不要默认做全局安装。
执行 Node 相关命令前，先检查项目是否存在 .node-version；存在则通过 fnm 使用对应版本。
如果需要安装 Node 版本、写入 package.json 或修改项目目录，请先向我申请权限。
安装完成后运行 npx openspec --version 验证，并在当前项目根目录执行 npx openspec init。
初始化后说明创建了哪些目录、命令或 AI 指令文件。
```

## 安装 Superpowers

GitHub 地址：<https://github.com/obra/superpowers>

Superpowers 是一组面向 AI 编码代理的工作流技能，解决“AI 如何推进任务”的问题，常用技能包括 `brainstorming`、`systematic-debugging`、`test-driven-development`、`writing-plans`、`requesting-code-review` 和 `verification-before-completion`。

### 通用安装方式

Superpowers 的安装入口会随 AI 编码工具不同而变化。请优先查看 [Superpowers 官方仓库](https://github.com/obra/superpowers) 和当前 AI 工具的插件、扩展或技能安装说明，再选择对应安装方式。

安装时应确认：

1. 当前 AI 工具是否支持插件、扩展或技能目录。
2. Superpowers 技能是否已被当前 AI 工具发现。
3. 安装或配置完成后，是否需要重启当前 AI 工具才能生效。

不同 AI 编码工具需要分别安装；不要假设在一个工具中安装后，另一个工具会自动生效。

### 通过 AI 安装

```text
请从 https://github.com/obra/superpowers 为当前 AI 工具安装 Superpowers。
先查看对应当前工具的官方安装说明。
如果需要克隆仓库、创建 junction/符号链接、修改用户目录或安装插件，请先向我申请权限。
安装完成后验证技能目录是否可见，并提醒我重启当前 AI 工具。
```

安装 Superpowers 后，建议继续阅读 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)，确认项目有无 `openspec/` 时应走哪条主流程。

## 步骤文档、技能与示例怎么选

三类材料职责不同，不要混用：

| 类型 | 代表 | 何时使用 |
| --- | --- | --- |
| 强制规则 | `AGENTS.md` | AI 默认识别；速查与第 1–5 节为跨项目、工具无关底线 |
| 项目规范 | `docs/规范/` | UI、API、业务约定等项目特有规则；增改只动本目录 |
| 流程教程 | OpenSpec 新需求/需求调整步骤、Superpowers 与 OpenSpec 指南 | 人读或 AI 按步骤推进；覆盖大多数常规场景 |
| 项目级技能（Full） | `$executing-complex-requirements`、`$updating-requirement-changes` | 见 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)；复杂需求可自动或点名触发，需求变化同步**仅点名触发** |
| 教学示例 | SSO 复杂需求示例 | 理解端到端串联方式；不作为唯一固定场景模板 |

按任务选型：

| 场景 | 优先阅读/使用 | 补充 |
| --- | --- | --- |
| 初次了解体系 | 本 README → AGENTS.md 章节摘要 | [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) |
| 普通新需求（项目已有 OpenSpec） | [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md) | 澄清/实现阶段按需 Superpowers |
| 复杂、高风险、跨模块新需求 | 点名 `$executing-complex-requirements` | 可参考 SSO 示例；仍须建 OpenSpec change |
| 已有需求补充、范围或验收调整 | [OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md) | 需要结构化同步记录时，再点名 `$updating-requirement-changes` |
| 项目尚无 OpenSpec | [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) 场景一 | 先确认是否初始化 OpenSpec（见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)） |

说明：

- 未点名 `$updating-requirement-changes` 时，按 OpenSpec 需求调整步骤或 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) 处理需求变化，不要自动套用该技能（触发规则在 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)，不在 `AGENTS.md` 速查）。
- `$executing-complex-requirements` 覆盖复杂新需求的完整闭环；与 OpenSpec 新需求步骤内容重叠，但技能版更强调 AI 执行纪律和检查清单。
- SSO 示例用于对照「先记录、再分析、再实现」的顺序，执行时仍以通用技能和 OpenSpec 步骤为准。

## 技能包使用

项目级技能包放在 `.agents/skills/` 下，用来补充 `AGENTS.md`、OpenSpec、CodeGraph 和 Superpowers 的协作流程。它们不是长期规范文档的替代品，而是让 AI 在特定任务中按固定方法推进。

技能包索引见 [.agents/skills/README.md](./.agents/skills/README.md)。本仓库当前提供：

```text
.agents/skills/README.md
.agents/skills/executing-complex-requirements/SKILL.md
.agents/skills/updating-requirement-changes/SKILL.md
```

根目录还保留一个复杂需求教学示例：

```text
SSO 单点登录复杂需求执行步骤示例.md
```

这个示例用于说明复杂认证需求的完整推进方式；具体执行时优先使用通用的 `$executing-complex-requirements` 技能，而不是把 SSO 当成唯一固定场景。

### 复杂需求执行：`$executing-complex-requirements`

这个技能用于复杂、高风险或跨模块产品需求，尤其是会改变外部可观察行为、API 契约、数据结构、权限规则、状态流转、用户路径、验收标准、发布或回滚的业务变更。

适合使用的场景包括：

-   新增或调整重要业务能力。
-   涉及认证、授权、支付、审批流、数据迁移等高风险领域。
-   需要先建立 OpenSpec change，再做 CodeGraph 影响分析和实现计划。
-   需要把方案取舍、验证、发布和回滚一起纳入交付范围。

推荐调用方式：

```text
请使用 $executing-complex-requirements 处理这个复杂需求。
先判断需求等级、记录载体、OpenSpec change 路径、CodeGraph 影响分析范围和验证计划。
在追踪记录和方案确认完成前，暂时不要写实现代码。
```

进入实现阶段时也可以继续点名：

```text
请使用 $executing-complex-requirements 继续执行 openspec/changes/<change-name>/ 的 tasks.md。
实现前先确认当前任务、影响范围、需要先失败的测试和验证命令。
完成后说明实际验证结果、未覆盖项和剩余风险。
```

核心流程是：先记录需求，再分析代码影响，再确认方案和计划，然后按 TDD 实现，最后用验证和评审收口。

### 需求变化同步：`$updating-requirement-changes`

这个技能只在用户明确点名时使用，用于处理已有需求的补充、范围变化、验收调整、需求正文更新和 OpenSpec change 同步。普通需求讨论、规则问答或临时想法不会自动触发该技能。

推荐调用方式：

```text
请使用 $updating-requirement-changes 更新 openspec/changes/<change-name>/。
这次需求新口径是：...
请把需求正文更新为当前有效口径，并追加需求变化记录。
同步 proposal、design、tasks 和 spec 中受影响的内容。
```

变化记录至少应保留：

```text
时间：
来源：
变化类型：
原口径：
新口径：
影响范围：
验收变化：
实现影响：
验证调整：
剩余风险：
```

核心规则是：需求正文更新为当前有效口径，需求变化记录追加保留历史变化事实，避免直接覆盖历史。

## OpenSpec 使用

本仓库推荐以 **OpenSpec 为主、Superpowers 为辅** 协作：

- **OpenSpec（主）**：承载需求、规格、任务和验收；决定「做什么、为什么做、做到什么程度算完成」。
- **Superpowers（辅）**：补充 AI 执行方法，例如需求澄清、调试、TDD、计划拆分、代码审查和完成前验证；不改变 OpenSpec 中的需求事实和验收标准。

典型配合方式：用 OpenSpec change 建立和追踪需求，在澄清、实现、调试和收尾阶段按需触发 Superpowers 技能。阶段映射与提示词见 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)。

一句话判断：**以 OpenSpec 管需求和验收，Superpowers 管执行纪律；新能力走新需求步骤，已有需求变更走需求调整步骤。** 具体选型见 [步骤文档、技能与示例怎么选](#步骤文档技能与示例怎么选)。

## 流程教程与示例

安装与规则就绪后，按场景打开对应文档：

| 场景 | 文档 | 说明 |
| --- | --- | --- |
| 新需求开发 | [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md) | 从零创建 change，按 `proposal → design → tasks → spec` 推进，验证后归档 |
| 已有需求调整 | [OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md) | 定位原 change，更新当前有效口径并追加变化记录，再同步任务和验收 |
| 与 Superpowers 配合 | [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) | 有无 `openspec/` 时的分流、阶段映射和实用提示词 |
| 复杂需求端到端示范 | [SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md) | 教学示例；执行时优先 `$executing-complex-requirements`，勿照搬 SSO 为唯一模板 |

补充参考：

- 规范索引：[docs/规范/规范总览.md](./docs/规范/规范总览.md)
- 技能包索引：[.agents/skills/README.md](./.agents/skills/README.md)
- 需求变化同步技能：[updating-requirement-changes/SKILL.md](./.agents/skills/updating-requirement-changes/SKILL.md)（仅用户明确点名时使用）
- 强制规则与目录约定：[AGENTS.md](./AGENTS.md)
