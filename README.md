# Vibe Coding 项目搭建指南

本文说明如何在一个项目中搭建并使用 `AGENTS.md + CodeGraph + OpenSpec + Superpowers` 协作体系。这里的核心不是把工具堆满，而是让 AI 开发过程同时具备四个能力：

- `AGENTS.md`：定义项目规则、权限边界、文档治理、验证底线和优先级。
- CodeGraph：提供代码事实、调用链、依赖关系和影响范围分析。
- OpenSpec：承载需求变化、规格、任务、验收标准和归档记录。
- Superpowers：提供 AI 执行方法，例如需求澄清、调试、TDD、计划、审查和完成前验证。

一句话理解：

```text
AGENTS.md   定规则和边界
CodeGraph   定代码事实和影响范围
OpenSpec    定需求、规格、任务和验收
Superpowers 定 AI 怎么推进工作
```

## 1. 适用场景

这套方式适合希望让 AI 深度参与真实项目开发的团队或个人，尤其适合这些场景：

- 需求会持续变化，需要保留可追溯记录。
- 项目有真实业务流程、权限、状态流转、数据模型或外部接口。
- AI 不只是写代码，还要参与需求分析、影响判断、测试和交付说明。
- 团队希望减少“AI 猜业务”“AI 绕过验证”“AI 改错边界”的风险。

一次性脚本、临时实验和很小的个人项目可以轻量使用：保留 `AGENTS.md`、完成前验证和必要的变更说明即可。

## 2. 推荐项目结构

新项目推荐从这个结构开始：

```text
your-project/
  AGENTS.md
  README.md
  openspec/
    project.md
    changes/
      <change-name>/
        proposal.md
        design.md
        tasks.md
        specs/
          <domain>/
            spec.md
    specs/
      <domain>/
        spec.md
  docs/
    规范/
      规范总览.md
      工作流规范.md
      需求文档规范.md
      工程协作规范.md
  .agents/
    skills/
      <project-skill>/
        SKILL.md
```

这些目录不是都要预先创建。原则是：**有真实用途再创建，不为空规范、空需求或空案例预建目录**。

常见文档落点可以按下面判断：

- 跨项目或跨模块长期有效的协作规则、UI 规范、工程约定，放在 `docs/规范/`，并从 README 或规范总览建立入口。
- 会改变用户能力、操作路径、接口契约、数据结构、权限规则、状态流转或验收标准的需求，放在 `openspec/changes/<change-name>/`。
- 已确认长期有效的领域规格、后端数据字段含义、字段可选性、默认值和失败行为，放在 `openspec/specs/<domain>/spec.md`。
- 纯实现细节，例如组件局部说明、数据库迁移步骤、索引设计或模块内部约束，可以放在对应模块文档、设计记录或迁移说明中。

一句话判断：**全局规则进规范，需求变化进 OpenSpec，字段契约进领域 spec，实现细节就近记录**。

最小落地版本只需要：

```text
your-project/
  AGENTS.md
  README.md
```

然后按需求逐步启用 CodeGraph、OpenSpec 和项目级技能包。

## 3. 第一步：使用 AGENTS.md

`AGENTS.md` 是项目协作章程，应该放在项目根目录。本仓库已经提供了一份完整的 `AGENTS.md`，新项目可以直接复制这份文件，再按项目实际情况调整目录、命名方式和治理边界。

它负责告诉 AI：

- 文档、需求、规范、技能包应该放在哪里。
- 什么任务需要 OpenSpec，什么任务可以轻量记录。
- 什么时候必须用 CodeGraph 分析影响范围。
- 什么时候必须使用 Superpowers 流程。
- 提交、推送、部署等高风险动作需要什么授权。
- 完成前需要运行哪些验证。

如果项目只想先轻量落地，可以从下面这个最小版开始；完整项目推荐直接使用本仓库根目录的 `AGENTS.md`：

```markdown
# AGENTS.md

本项目采用 `AGENTS.md + CodeGraph + OpenSpec + Superpowers` 协作方式：

- AGENTS.md 定义项目规则、权限边界和验证底线。
- CodeGraph 用于代码结构、调用链和影响范围分析。
- OpenSpec 用于中等和重大需求的规格、任务和验收记录。
- Superpowers 用于需求澄清、调试、TDD、计划、审查和完成前验证。

执行规则：

1. 任务开始前先读取本文件。
2. 涉及行为、接口、数据、权限、流程或验收标准变化时，先检查或创建 OpenSpec change。
3. 涉及代码修改、缺陷定位、重构或影响判断时，优先使用 CodeGraph。
4. 涉及设计、调试、TDD、计划或完成前验证时，使用对应 Superpowers 流程。
5. 未经明确要求，不执行 commit、push、merge、release、deploy。
6. 完成后必须说明修改文件、验证结果和剩余风险。
```

如果项目已经有自己的规则文件，应先合并规则职责和优先级，再决定是替换为本仓库模板，还是只把其中的 Vibe Coding 执行规则补充进去。

## 4. 第二步：安装并初始化 CodeGraph

GitHub 地址：<https://github.com/colbymchenry/codegraph>

CodeGraph 用于让 AI 从已索引的代码图中理解项目，而不是每次都从零开始搜索文件。它适合用于：

- 找功能入口、函数、类、组件、路由。
- 查调用方、被调用方和依赖关系。
- 判断修改公共模块会影响哪里。
- 辅助 bug 定位、重构和代码审查。

### 4.1 Windows PowerShell 安装

方式一：使用官方 PowerShell 安装脚本。

```powershell
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

方式二：如果当前环境已经有 Node，可以使用 npm 包。

```powershell
npx @colbymchenry/codegraph
```

或全局安装：

```powershell
npm i -g @colbymchenry/codegraph
```

### 4.2 通过 AI 安装

如果希望让当前 AI 工具代为安装，可以直接发送：

```text
请从 https://github.com/colbymchenry/codegraph 安装 CodeGraph。
先查看官方 README 和安装脚本，确认适合当前系统的安装方式。
如果需要执行网络下载、修改全局配置或写入用户目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，并说明是否需要重启当前 AI 工具。
```

如果只想在当前项目启用：

```text
请为当前项目启用 CodeGraph。
先检查是否已经安装 codegraph；如果未安装，请从 https://github.com/colbymchenry/codegraph 安装。
然后在项目根目录执行初始化，优先使用 codegraph init -i。
完成后说明生成了哪些配置或索引文件，以及后续如何更新索引。
```

### 4.3 配置 AI Agent

交互式安装器会询问要配置哪些 Agent，例如 Codex、Claude Code、Cursor、OpenCode、Gemini CLI 等，并可选择全局配置或仅当前项目配置。

如果需要非交互式安装，可以使用：

```powershell
codegraph install --yes
```

只查看 Codex 配置片段、不写文件：

```powershell
codegraph install --print-config codex
```

安装或配置完成后，重启你的 AI 编码工具，让 MCP 配置生效。

### 4.4 在项目中初始化

进入项目根目录后执行：

```powershell
cd your-project
codegraph init -i
```

初始化后，项目中会生成 `.codegraph/` 索引。AI 在这个项目中就可以使用 CodeGraph 查询代码结构、调用关系和影响范围。

常见使用意图：

```text
请用 CodeGraph 分析登录流程的入口、调用链和影响范围。
请用 CodeGraph 查看修改 createSession 会影响哪些模块。
请用 CodeGraph 找出订单状态流转相关代码。
```

如果索引过期，可以重新同步或重新初始化，具体命令以 CodeGraph 当前 README 为准。

## 5. 第三步：安装并初始化 OpenSpec

GitHub 地址：<https://github.com/Fission-AI/OpenSpec>

OpenSpec 用于把需求变化从聊天记录里拉出来，形成可追溯的 proposal、design、tasks 和 spec。它适合承载：

- 新业务能力。
- API、数据模型、权限、状态机变化。
- 中等和重大需求变更。
- 验收标准、任务拆分和归档状态。

### 5.1 Node 前置检查

OpenSpec 要求 Node.js 20.19.0 或更高版本。执行 Node 相关命令前，先按项目规则检查 `.node-version`：

```powershell
Test-Path .node-version
```

如果存在 `.node-version`，用 fnm 切换：

```powershell
fnm use
node --version
```

如果不存在，使用当前默认 Node：

```powershell
node --version
```

### 5.2 安装 OpenSpec

```powershell
npm install -g @fission-ai/openspec@latest
openspec --version
```

也可以使用 pnpm、yarn、bun 或 nix；具体以 OpenSpec 官方 README 和安装文档为准。

### 5.3 通过 AI 安装

如果希望让当前 AI 工具代为安装，可以直接发送：

```text
请从 https://github.com/Fission-AI/OpenSpec 安装 OpenSpec。
先查看官方 README，确认当前 Node 版本是否满足要求。
执行 Node 相关命令前，先检查项目是否存在 .node-version；存在则通过 fnm 使用对应版本。
如果需要安装 Node 版本、执行 npm 全局安装或写入用户目录，请先向我申请权限。
安装完成后运行 openspec --version 验证。
```

如果只想在当前项目启用：

```text
请为当前项目初始化 OpenSpec。
先检查是否已经安装 openspec；如果未安装，请从 https://github.com/Fission-AI/OpenSpec 安装。
然后在项目根目录执行 openspec init。
初始化后说明创建了哪些目录、命令或 AI 指令文件，并给出第一个 change 的建议路径。
```

### 5.4 在项目中初始化

进入项目根目录后执行：

```powershell
cd your-project
openspec init
```

初始化会创建 OpenSpec 目录结构，并根据所选 AI 编码工具生成对应的指令或命令。

初始化后，可以让 AI 从需求提案开始：

```text
/opsx:propose 新增手机号验证码登录
```

如果需要扩展工作流命令，可以按 OpenSpec 文档切换 profile 并更新：

```powershell
openspec config profile
openspec update
```

在 Vibe Coding 中，推荐把中等和重大真实业务需求放进：

```text
openspec/changes/<change-name>/
  proposal.md
  design.md
  tasks.md
  specs/
    <domain>/
      spec.md
```

## 6. 第四步：安装 Superpowers

GitHub 地址：<https://github.com/obra/superpowers>

Superpowers 是一组面向 AI 编码代理的工作流技能。它解决的是“AI 如何推进任务”的问题，常用技能包括：

- `brainstorming`：实现前澄清目标、边界和成功标准。
- `systematic-debugging`：遇到 bug 时先复现、定位、验证假设，再修复。
- `test-driven-development`：先写失败测试，再写实现，再重构。
- `writing-plans`：把已确认设计拆成可执行计划。
- `requesting-code-review`：完成重要改动后请求审查。
- `verification-before-completion`：声明完成前运行验证并记录证据。

### 6.1 Codex App 安装

在 Codex App 中，优先使用插件市场：

```text
1. 打开 Codex 侧边栏的 Plugins。
2. 在 Coding 分类中找到 Superpowers。
3. 点击 + 安装并按提示完成。
4. 重启 Codex，使技能发现生效。
```

### 6.2 Codex 手动安装

如果当前环境没有插件市场入口，可以按 GitHub 仓库说明手动安装。Windows PowerShell：

```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.codex\superpowers"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
```

验证：

```powershell
Get-ChildItem "$env:USERPROFILE\.agents\skills" | Where-Object { $_.LinkType }
```

看到 `superpowers` junction 后，重启 Codex。

### 6.3 通过 AI 安装

可以直接让 Codex 按官方安装说明执行：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md
```

也可以使用更明确的中文提示：

```text
请从 https://github.com/obra/superpowers 为当前 AI 工具安装 Superpowers。
先查看对应当前工具的官方安装说明。
如果需要克隆仓库、创建 junction/符号链接、修改用户目录或安装插件，请先向我申请权限。
安装完成后验证技能目录是否可见，并提醒我重启当前 AI 工具。
```

如果只想让当前项目更好地配合 Superpowers：

```text
请检查当前项目是否适合新增项目级技能包。
如果不需要自定义技能包，请不要创建 .agents/skills/。
如果需要，请先说明技能名称、适用场景和入口文件，再创建 .agents/skills/<skill-name>/SKILL.md。
```

### 6.4 其他 Agent 安装入口

Claude Code：

```text
/plugin install superpowers@claude-plugins-official
```

Cursor：

```text
/add-plugin superpowers
```

Gemini CLI：

```powershell
gemini extensions install https://github.com/obra/superpowers
```

不同 Agent 需要分别安装 Superpowers；不要假设在一个工具中安装后，另一个工具会自动生效。

### 6.5 项目初始化

Superpowers 通常是 AI 工具级别安装，不需要每个项目重复安装。新项目只需要：

1. 根目录有清晰的 `AGENTS.md`。
2. 如果项目需要专用技能，再创建 `.agents/skills/<skill-name>/SKILL.md`。
3. 在任务中按项目规则触发对应技能。

如果项目不需要自定义技能包，可以不创建 `.agents/skills/`。

### 6.6 项目级技能包：`$sso-requirement-workflow`

本仓库已经把原来的 SSO 复杂需求执行步骤沉淀为项目级技能包：

```text
.agents/skills/sso-requirement-workflow/SKILL.md
```

当任务涉及 SSO 单点登录、企业身份源、OIDC、SAML、IdP callback、用户绑定、权限映射、session/token、路由守卫、灰度、回滚或审计时，可以直接在提示中点名使用：

```text
请使用 $sso-requirement-workflow，为当前项目规划新增企业 SSO 登录。
先判断任务等级、OpenSpec 记录载体、CodeGraph 影响分析范围和验证计划，暂时不要写实现代码。
```

也可以用于实现或审查阶段：

```text
请使用 $sso-requirement-workflow，继续执行 openspec/changes/add-sso-login/ 的 tasks.md。
实现前先确认当前任务、影响范围和需要运行的测试；完成后说明验证结果和剩余风险。
```

这个技能包的完整教学示例已放在：

```text
.agents/skills/sso-requirement-workflow/references/sso-complex-requirement-example.md
```

因此根目录不再保留单独的 `SSO单点登录复杂需求执行步骤示例.md`，避免 README、技能入口和示例文档之间出现重复维护。

## 7. 推荐协作流程

完成安装和初始化后，一个真实需求推荐这样推进：

```text
1. AI 读取 AGENTS.md，确认项目规则、权限和验证要求。
2. 判断任务类型：问答、小修、bugfix、中等需求、重大需求、重构或紧急修复。
3. 根据任务类型启用 Superpowers：
   - 新需求：brainstorming、writing-plans
   - bugfix：systematic-debugging、test-driven-development
   - 完成前：verification-before-completion
4. 如果涉及行为、接口、数据、权限、流程或验收标准变化，先创建或更新 OpenSpec change。
5. 如果涉及代码修改、影响判断、重构或缺陷定位，使用 CodeGraph 分析代码事实。
6. 更新 OpenSpec 的 proposal、design、tasks 和 spec。
7. 制定计划并实现。
8. 运行测试、构建、lint、OpenSpec 校验和必要人工验收。
9. 总结修改文件、验证结果和剩余风险。
```

## 8. 常用提示词

项目启动提示：

```text
请按本项目 AGENTS.md 执行。
如果任务涉及需求、接口、数据、权限、流程或验收标准变化，请先检查 OpenSpec 记录。
如果需要理解代码结构或影响范围，请优先使用 CodeGraph。
如果任务需要设计、调试、TDD、计划或完成前验证，请使用对应 Superpowers 流程。
实现前说明任务分级、记录载体、影响范围和验证计划。
完成后说明修改文件、验证结果和剩余风险。
```

需求提案提示：

```text
请为“新增企业 SSO 登录”创建 OpenSpec change。
先说明目标、范围、非目标、验收标准和风险，再拆分 tasks。
实现前请用 CodeGraph 分析认证、用户、会话和权限模块的影响范围。
```

Bugfix 提示：

```text
请使用 systematic-debugging 定位这个问题。
先复现或确认失败现象，再用 CodeGraph 找调用链和影响范围。
修复时优先补回归测试，完成前运行相关验证并说明剩余风险。
```

重构提示：

```text
请先用 CodeGraph 分析这个模块的调用方、被调用方和影响范围。
本次重构不改变外部可观察行为。
实现前说明验证计划，完成后运行同一组基线验证。
```

## 9. 安装和初始化速查

| 能力 | GitHub | 安装 | 项目初始化 |
| --- | --- | --- | --- |
| AGENTS.md | 无需安装 | 复制本仓库根目录的 `AGENTS.md` | 按项目调整目录、命名方式、治理边界和优先级 |
| CodeGraph | <https://github.com/colbymchenry/codegraph> | `irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 \| iex` 或 `npm i -g @colbymchenry/codegraph` | `codegraph init -i` |
| OpenSpec | <https://github.com/Fission-AI/OpenSpec> | `npm install -g @fission-ai/openspec@latest` | `openspec init` |
| Superpowers | <https://github.com/obra/superpowers> | Codex 插件市场安装，或 clone 后创建 skills junction | 通常无需项目级初始化；按需创建 `.agents/skills/` |

## 10. 维护原则

- README 只做项目搭建和使用入口，不承载完整强制规则。
- `AGENTS.md` 是 AI 协作章程和规则入口。
- OpenSpec 记录真实业务需求和验收标准。
- CodeGraph 记录和查询代码事实，但不替代测试。
- Superpowers 规定 AI 执行方法，但不改变项目规则、需求事实和验证底线。
- 工具安装命令可能更新；长期使用时应以对应 GitHub README 和官方文档为准。
