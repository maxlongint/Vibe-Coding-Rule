# Vibe Coding 技能包与项目搭建指南

本文说明如何使用本仓库提供的通用 `AGENTS.md` 和项目级技能包，并在具体项目中搭建 `AGENTS.md + CodeGraph + OpenSpec + Superpowers` 协作体系。这里的核心不是把工具堆满，而是让 AI 开发过程同时具备四个能力：

-   `AGENTS.md`：定义项目规则、权限边界、文档治理、验证底线和优先级。
-   CodeGraph：提供代码事实、调用链、依赖关系和影响范围分析。
-   OpenSpec：承载需求变化、规格、任务、验收标准和归档记录。
-   Superpowers：提供 AI 执行方法，例如需求澄清、调试、TDD、计划、审查和完成前验证。

一句话理解：

```text
AGENTS.md   定规则和边界
CodeGraph   定代码事实和影响范围
OpenSpec    定需求、规格、任务和验收
Superpowers 定 AI 怎么推进工作
```

## 内容目录

-   [本仓库定位](#本仓库定位)
-   [安装 CodeGraph](#安装-codegraph)
-   [安装 OpenSpec](#安装-openspec)
-   [安装 Superpowers](#安装-superpowers)
-   [技能包使用](#技能包使用)
    -   [复杂需求执行：`$executing-complex-requirements`](#复杂需求执行executing-complex-requirements)
    -   [需求变化同步：`$updating-requirement-changes`](#需求变化同步updating-requirement-changes)
-   [OpenSpec 使用](#openspec-使用)

## 本仓库定位

本仓库是 Vibe Coding 的通用规则包和技能包，不是某个业务项目本身。因此本仓库不需要初始化 `.codegraph/` 或 `openspec/`；这些目录应在实际采用本规则的业务项目中按需创建。

本仓库根目录的 `AGENTS.md` 是跨项目复用的固定基线，负责定义不可跳过的协作规则、权限边界、需求治理底线、验证底线和优先级。不同项目会变化的技术栈、业务规范、目录适配、命令说明和领域规则，不应继续写入本仓库的 `AGENTS.md`；应放在使用方项目自己的 `README.md`、`docs/规范/`、OpenSpec、外部需求系统或项目级技能包中。

`SSO 单点登录复杂需求执行步骤示例.md` 是保留在根目录的教学示例，用来展示复杂需求如何串联 AGENTS.md、CodeGraph、OpenSpec 和 Superpowers。它不是本仓库自身的真实需求记录，也不要求本仓库创建对应的 OpenSpec change。

## 安装 CodeGraph

GitHub 地址：<https://github.com/colbymchenry/codegraph>

CodeGraph 用于让 AI 从已索引的代码图中理解项目，而不是每次都从零开始搜索文件。它适合用于找功能入口、查调用关系、判断修改影响范围，以及辅助 bug 定位、重构和代码审查。

### 通用安装方式

请优先查看 [CodeGraph 官方 README](https://github.com/colbymchenry/codegraph)，按当前系统和 AI 编码工具选择对应安装方式。常见路径包括官方安装脚本、`npm i -g @colbymchenry/codegraph` 或 `npx @colbymchenry/codegraph`。

安装完成后：

1. 运行 `codegraph --version` 验证。
2. 按官方说明配置 AI Agent（如需非交互式安装，可使用 `codegraph install --yes`）。
3. 在项目根目录执行 `codegraph init -i` 生成索引。
4. 重启 AI 编码工具，让 MCP 配置生效。

具体命令和平台差异以官方文档为准。

### 通过 AI 安装

```text
请从 https://github.com/colbymchenry/codegraph 安装 CodeGraph。
先查看官方 README 和安装脚本，确认适合当前系统的安装方式。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，配置 AI Agent，并在当前项目根目录执行 codegraph init -i。
最后说明是否需要重启当前 AI 工具，以及后续如何更新索引。
```

## 安装 OpenSpec

GitHub 地址：<https://github.com/Fission-AI/OpenSpec>

OpenSpec 用于把需求变化从聊天记录里拉出来，形成可追溯的 proposal、design、tasks 和 spec。它适合承载新业务能力、API 与数据模型变化、中等和重大需求变更，以及验收标准和任务拆分。

### 通用安装方式

OpenSpec 要求 Node.js 20.19.0 或更高版本。请优先查看 [OpenSpec 官方 README](https://github.com/Fission-AI/OpenSpec) 和安装文档。

常见安装方式：

```powershell
npm install -g @fission-ai/openspec@latest
openspec --version
```

也可使用 pnpm、yarn、bun 或 nix。安装完成后，在项目根目录执行 `openspec init` 初始化。

如果项目存在 `.node-version`，执行 Node 相关命令前先切换到对应版本（例如 `fnm use`）。

### 通过 AI 安装

```text
请从 https://github.com/Fission-AI/OpenSpec 安装 OpenSpec。
先查看官方 README，确认当前 Node 版本是否满足要求。
执行 Node 相关命令前，先检查项目是否存在 .node-version；存在则通过 fnm 使用对应版本。
如果需要安装 Node 版本、执行 npm 全局安装或写入用户目录，请先向我申请权限。
安装完成后运行 openspec --version 验证，并在当前项目根目录执行 openspec init。
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

## 技能包使用

项目级技能包放在 `.agents/skills/` 下，用来补充 `AGENTS.md`、OpenSpec、CodeGraph 和 Superpowers 的协作流程。它们不是长期规范文档的替代品，而是让 AI 在特定任务中按固定方法推进。

本仓库当前提供两个项目级技能包：

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

典型配合方式：用 OpenSpec change 建立和追踪需求，在澄清、实现、调试和收尾阶段按需触发 Superpowers 技能。详细阶段映射见 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)。

安装完成后，按场景查阅本仓库已有文档：

| 场景 | 文档 | 说明 |
| --- | --- | --- |
| 新需求开发 | [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md) | 从零创建 change，按 `proposal → design → tasks → spec` 推进，验证后归档 |
| 已有需求调整 | [OpenSpec需求调整步骤.md](./OpenSpec需求调整步骤.md) | 定位原 change，更新当前有效口径并追加变化记录，再同步任务和验收 |
| 与 Superpowers 配合 | [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md) | 说明两套流程的分工、适用场景和阶段映射 |

补充参考：

- 复杂需求完整示例：[SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md)
- 需求变化同步技能：`.agents/skills/updating-requirement-changes/SKILL.md`（用户明确点名时使用）
- 强制规则与目录约定：根目录 `AGENTS.md`

一句话判断：**以 OpenSpec 管需求和验收，Superpowers 管执行纪律；新能力走新需求步骤，已有需求变更走需求调整步骤。**
