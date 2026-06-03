# Vibe Coding 编码工具安装说明

---

## 目录

1. [codegraph](#1-codegraph)
2. [OpenSpec](#2-openspec)
3. [Superpowers](#3-superpowers)

---

## 1. codegraph

### 项目信息

-   GitHub: [colbymchenry/codegraph](https://github.com/colbymchenry/codegraph)
-   用途：代码索引与依赖图分析工具，用于快速搜索代码结构和分析变更影响范围。

### 在 SSO 示例中的用途

在 SSO 复杂需求执行步骤中，codegraph 用于影响分析阶段：

> 项目配置了 codegraph、代码索引工具、依赖图工具或同类能力时，应优先使用；没有工具时，也必须记录人工影响判断。

典型查询命令：

```powershell
codegraph query "auth login session token user role permission"
codegraph impact "login"
codegraph impact "createSession"
codegraph impact "authCallback"
```

### 手动安装方法

codegraph 的安装方式取决于其技术栈，请先从 GitHub Releases 页确定适合的方式：

```powershell
# 方式一：下载 GitHub Release 二进制
# 1. 打开 https://github.com/colbymchenry/codegraph/releases
# 2. 下载最新版本的 Windows 二进制文件
# 3. 解压到目录（例如 C:\tools\codegraph）
# 4. 将该目录添加到系统 PATH

# 方式二：如果项目是 npm 包
npm install -g codegraph

# 方式三：如果项目是 Rust 工具
cargo install codegraph

# 方式四：如果项目是 Go 工具
go install github.com/colbymchenry/codegraph@latest
```

验证安装：

```powershell
codegraph --version
```

### 通过 AI 安装

让当前 AI（Codex、Claude Code、Cursor 等）代为安装：

```text
请查看 https://github.com/colbymchenry/codegraph 的 README 和 Release 页面，
找到适合 Windows 系统的安装方式，然后帮我安装 codegraph。
安装完成后运行 codegraph --version 验证。
```

或更简洁的版本：

```text
请从 https://github.com/colbymchenry/codegraph 安装 codegraph 工具。
先查看 README 确定安装方式，然后执行安装并验证。
```

### 新项目初始化

在新项目中使用 codegraph 前，需要先对项目代码建立索引，生成依赖关系图：

```powershell
# 在项目根目录初始化索引
codegraph init

# 如果项目较大，可以指定代码目录
codegraph init ./src
```

初始化完成后，就可以用 `codegraph query` 和 `codegraph impact` 进行代码搜索和影响分析。
如果项目代码发生变化（新增文件、重构等），需要重新执行 `codegraph init` 来更新索引。

---

## 2. OpenSpec

### 项目信息

-   GitHub: [Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)
-   用途：开放式规格文档框架，用于管理需求规格、验收标准和领域行为描述。

### 在 SSO 示例中的用途

在 SSO 复杂需求执行步骤中，OpenSpec 是可选的规格文档方法：

> 如果项目没有启用 OpenSpec，也不需要为了本需求强行引入 OpenSpec。只要在 `docs/需求/<change-name>/` 中保持各文档职责清晰，就符合需求治理底线。

> 如果项目已经使用 OpenSpec、ADR、RFC 或同类方法，可以把这些方法承载在 `docs/需求/<change-name>/` 内。

OpenSpec 适合需要结构化验收场景和跨团队对齐的大型项目。

### 手动安装方法

请从 GitHub Releases 页面确认具体安装方式：

```powershell
# 方式一：npm 全局安装
npm install -g openspec

# 方式二：项目内安装
npm install --save-dev openspec

# 方式三：如果提供二进制 Release
# 1. 打开 https://github.com/Fission-AI/OpenSpec/releases
# 2. 下载 Windows 版本并解压
# 3. 将目录添加到系统 PATH
```

验证安装：

```powershell
openspec --version
```

### 通过 AI 安装

让当前 AI 代为安装：

```text
请查看 https://github.com/Fission-AI/OpenSpec 的 README 和 Release 页面，
找到适合的安装方式，然后帮我安装 OpenSpec。
安装完成后运行 openspec --version 验证。
```

或更简洁的版本：

```text
请从 https://github.com/Fission-AI/OpenSpec 安装 OpenSpec 工具。
先查看 README 确定安装方式，然后执行安装并验证。
```

### 新项目初始化

在新项目中启用 OpenSpec，需要创建规格文档目录结构和初始化配置文件：

```powershell
# 在项目根目录初始化 OpenSpec
openspec init

# 该命令通常会在项目中创建 specs/ 目录和配置文件
# 具体效果以该工具的 README 为准
```

初始化完成后，可以在 specs/ 目录下为不同领域创建规格文件：

```powershell
openspec new auth -d "认证领域规格"
openspec new user -d "用户领域规格"
```

如果项目遵循 AGENTS.md 的仓库内需求文档治理方式（推荐），也可以不使用 OpenSpec 工具，
直接在 `docs/需求/<change-name>/` 下手动管理需求文档。两种方式都可以满足需求治理底线。

---

## 3. Superpowers

### 项目信息

-   GitHub: [obra/superpowers](https://github.com/obra/superpowers)
-   用途：一套完整的 AI 编码代理开发工作流，基于可组合的技能（skills）系统，帮助 AI 在编码前先做需求澄清、设计确认、计划拆解，再通过子代理驱动、TDD、代码审查和完成前验证等能力完成高质量交付。

### 在 SSO 示例中的用途

在 SSO 复杂需求执行步骤中，Superpowers 对应的技能可以覆盖完整流程：

-   **brainstorming**：在编码前澄清需求、确认目标和边界（对应步骤一、二）。
-   **writing-plans**：将需求拆分为可执行任务（对应步骤五）。
-   **subagent-driven-development / executing-plans**：分阶段执行任务（对应步骤六）。
-   **test-driven-development**：先写测试再写实现。
-   **requesting-code-review**：任务间的自动审查。
-   **verification-before-completion**：完成前验证命令并报告结果（对应步骤八）。
-   **finishing-a-development-branch**：完成后的整合决策。

### 手动安装方法

#### 在 Codex 中安装（当前环境）

```powershell
# 1. 克隆仓库
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.codex\superpowers"

# 2. 创建技能目录和符号链接
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"

# 3. 重启 Codex 使技能生效
```

#### 在 Claude Code 中安装

```bash
/plugin install superpowers@claude-plugins-official
```

或通过社区市场：

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

#### 在 Cursor 中安装

```text
/add-plugin superpowers
```

或在插件市场中搜索 "superpowers"。

#### 在 OpenCode 中安装

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

### 通过 AI 安装

#### 在 Codex 中让 AI 安装

告诉当前 AI（Codex）：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md
```

这条提示词会让 AI 自动完成以下步骤：

1. 克隆 superpowers 仓库到 `~/.codex/superpowers/`。
2. 在 `~/.agents/skills/` 下创建符号链接。
3. 提示重启 Codex 使技能生效。

#### 在 Claude Code 中让 AI 安装

```text
请安装 superpowers 插件。在 Claude Code 中运行 /plugin install superpowers@claude-plugins-official。
```

#### 在 Cursor 中让 AI 安装

```text
请安装 superpowers 插件。使用 /add-plugin superpowers 或在市场搜索。
```

### 新项目初始化

Superpowers 安装后是 AI 平台级别的能力，每个新项目不需要重新安装。但需要让当前项目能够被 Superpowers 技能识别和使用：

#### 场景一：当前环境已安装 Superpowers（推荐）

如果 AI 环境（如 Codex）已经安装了 Superpowers，技能会自动根据任务触发。在新项目的根目录下，确保以下条件满足：

-   技能软链接存在：`~/.agents/skills/superpowers/` 指向技能目录。
-   项目根目录的 `AGENTS.md` 按需声明项目特定规则（可选）。

技能会根据任务自动激活，无需手动操作：

-   提出需要设计决策的任务时，brainstorming 自动激活。
-   设计确定后，writing-plans 自动激活编写计划。
-   实现阶段，TDD 自动要求先写测试。
-   修改完成后，verification-before-completion 自动验证。

#### 场景二：新环境首次安装 Superpowers

如果当前 AI 环境尚未安装 Superpowers，需要先完成一次安装，技能目录建立后即可在所有项目中生效。

```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.codex\superpowers"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
```

#### 场景三：项目特定的技能配置

如果项目中需要自定义 AI 行为规则，可以在项目根目录或 `docs/规范/` 中声明。
Superpowers 技能会自动遵循这些项目级规则。

### 验证安装

```powershell
ls "$env:USERPROFILE\.agents\skills\superpowers"
```

列表中应能看到 brainstorming、test-driven-development、verification-before-completion 等技能目录。

### 更新

```powershell
cd "$env:USERPROFILE\.codex\superpowers"
git pull
```

技能通过符号链接即时生效，无需额外操作。

---

## 附录：新项目工具初始化速查

| 工具        | 是否需要初始化               | 初始化前提         | 初始化命令                              |
| ----------- | ---------------------------- | ------------------ | --------------------------------------- |
| codegraph   | 需要                         | 已安装 codegraph   | `codegraph index .`（在项目根目录执行） |
| OpenSpec    | 需要                         | 已安装 OpenSpec    | `openspec init`（在项目根目录执行）     |
| Superpowers | 不需要（一次安装，全局生效） | 已安装 Superpowers | 无需初始化；技能自动触发                |

> **注**：codegraph 和 OpenSpec 在安装后还需要在具体项目目录中执行初始化命令才能使用。Superpowers 是 AI 平台级别的能力，只要在当前环境安装一次，所有项目自动受惠。每个工具的具体初始化行为以对应 GitHub 仓库的 README 为准。
