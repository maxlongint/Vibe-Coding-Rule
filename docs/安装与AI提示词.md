# 安装与 AI 提示词

本文档承接 [README](../README.md) 中的 Standard / Full 安装细节、可选工具安装，以及通过 AI 安装规则包的完整提示词。

**规则包来源：** <https://github.com/maxlongint/Vibe-Coding-Rule>

**重要：** 规则包应安装到**业务项目**，不要在本规则仓库内初始化 `openspec/`、`.codegraph/` 或复制业务项目结构。

## 复制步骤

0. 若尚未获取规则包：`git clone https://github.com/maxlongint/Vibe-Coding-Rule`
1. 复制 `AGENTS.md` 到业务项目根目录（**所有档位必需**）
2. **Standard / Full：** 建立 `docs/规范/`，复制 [需求治理与文档结构.md](./规范/需求治理与文档结构.md)
3. **Full：** 继续复制 [AI协作与工具链.md](./规范/AI协作与工具链.md)、[规范总览.md](./规范/规范总览.md)、[模板/](./规范/模板/) 等
4. **Standard / Full（推荐）：** 复制 `.github/pull_request_template.md` 与 [轻量记录模板.md](./规范/模板/轻量记录模板.md)
5. **Full（推荐）：** 复制 `.agents/skills/`；**推荐**安装 OpenSpec / CodeGraph / Superpowers（见下文）
6. **（可选）** 建立 `docs/README.md`；在 `docs/规范/` 追加本项目 UI/API 等特有规则

若目标项目已有 `AGENTS.md` 或 `docs/规范/`，先 diff 说明冲突与合并策略，**不得静默覆盖**。

## 通过 AI 安装规则包

**使用前确认：**

1. 已在 AI 工具中打开**目标业务项目**（不是本规则仓库）
2. 已选定档位：**Minimal**、**Standard** 或 **Full**

**通用约束：**

- 写入前先列出目标路径并申请权限
- 已有同名文件时先 diff，经确认后再写入
- 完成后列出实际复制的文件与下一步

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

若某文件已存在，先 diff 并说明合并策略；Node 命令前检查 .node-version。
各步写入或执行前申请权限。
全部完成后列出：已复制文件、已初始化目录、验证命令结果，以及是否需要重启 AI 工具。
```

### 仅补充项目级技能

```text
请从 Vibe Coding 规则包仅复制项目级技能到当前业务项目。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
复制 .agents/skills/ 到当前项目 .agents/skills/；若目录已存在，只新增 executing-complex-requirements、updating-requirement-changes，不要覆盖项目自有技能。
写入前列出路径并申请权限；完成后说明技能索引位置（.agents/skills/README.md）及 updating-requirement-changes 仅点名触发的规则。
```

### 从规则包仓库安装到另一项目

先补两行：

```text
目标项目：（填写业务项目根目录）
档位：Minimal | Standard | Full
```

再发送：

```text
我要把 Vibe Coding 规则包装到上述目标项目，不是当前打开的仓库。
规则包来源：https://github.com/maxlongint/Vibe-Coding-Rule
请先确认目标路径存在且为业务项目根目录，再按 docs/安装与AI提示词.md 中对应档位的步骤执行；所有文件写入目标项目，不要修改规则包来源仓库。
```

## 推荐工具安装

均在**业务项目根目录**安装；**Full 档位推荐三工具全套**（OpenSpec 为主、Superpowers 为辅）。未安装时按 [AGENTS.md](../AGENTS.md) fallback。

### OpenSpec

- 仓库：<https://github.com/Fission-AI/OpenSpec>
- 要求 Node.js ≥ 20.19.0

```powershell
npm install -D @fission-ai/openspec@latest
npx openspec init
```

AI 安装提示词：

```text
请为当前业务项目安装 OpenSpec，不要在本规则仓库中初始化。
先进入当前项目根目录，再从 https://github.com/Fission-AI/OpenSpec 查看官方 README。
OpenSpec 优先安装为当前项目依赖，不要默认做全局安装。
执行 Node 相关命令前，先检查项目是否存在 .node-version；存在则通过 fnm 使用对应版本。
如果需要安装 Node 版本、写入 package.json 或修改项目目录，请先向我申请权限。
安装完成后运行 npx openspec --version 验证，并在当前项目根目录执行 npx openspec init。
```

### CodeGraph

- 仓库：<https://github.com/colbymchenry/codegraph>
- MCP 配置优先**项目级（local）**

```text
cd your-project
codegraph install --target=auto --location=local --yes
codegraph init -i
codegraph --version
```

AI 安装提示词：

```text
请为当前业务项目安装 CodeGraph，不要在本规则仓库中初始化。
先进入当前项目根目录，再从 https://github.com/colbymchenry/codegraph 查看官方 README。
MCP 配置优先选择项目级（local），不要默认写入全局配置。
如果需要执行网络下载、修改配置或写入项目目录，请先向我申请权限。
安装完成后运行 codegraph --version 验证，并在当前项目根目录执行 codegraph init -i。
```

### Superpowers

- 仓库：<https://github.com/obra/superpowers>
- 安装方式因 AI 工具而异，以官方说明为准

AI 安装提示词：

```text
请从 https://github.com/obra/superpowers 为当前 AI 工具安装 Superpowers。
先查看对应当前工具的官方安装说明。
如果需要克隆仓库、创建 junction/符号链接、修改用户目录或安装插件，请先向我申请权限。
安装完成后验证技能目录是否可见，并提醒我重启当前 AI 工具。
```

安装 Superpowers 后，建议阅读 [Superpowers与OpenSpec使用指南.md](../Superpowers与OpenSpec使用指南.md)。

## 技能包点名示例

索引见 [.agents/skills/README.md](../.agents/skills/README.md)。

**复杂需求（可自动或点名）：**

```text
请使用 $executing-complex-requirements 处理这个复杂需求。
先判断需求等级、记录载体、影响分析范围和验证计划。
在追踪记录和方案确认完成前，暂时不要写实现代码。
```

**需求变化同步（仅点名触发）：**

```text
请使用 $updating-requirement-changes 更新 openspec/changes/<change-name>/。
这次需求新口径是：...
请把需求正文更新为当前有效口径，并追加需求变化记录。
```

非 OpenSpec 项目将路径改为 issue/PR 编号即可。触发边界见 [AI协作与工具链.md](./规范/AI协作与工具链.md)。
