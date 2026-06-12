# 需求变更（OpenSpec 主 · Superpowers 辅）

> 本文是 skill `requirement-change` 的完整流程正文（唯一来源），由 [`SKILL.md`](./SKILL.md) 编排触发。强制底线见 [`AGENTS.md`](../../../AGENTS.md)。本文讲开发人员怎么操作、执行什么命令、产出什么。从零开发新需求见 [`new-feature-development`](../new-feature-development/workflow.md)。
>
> **核心原则：不急着新建——先定位原记录、判断变化等级、把正文更新为当前口径、追加变化记录（不覆盖历史），再改代码。**
>
> **文档只有一处事实来源——OpenSpec 的 change 目录。** 三种命令写法、占位符、TDD 定位与工具 fallback 见 [`流程公共约定.md`](../../../流程公共约定.md)。

## 操作步骤

### 第 1 步　定位原记录

来了一个变更，先找到它对应的原 change 或外部需求记录，**别上来就新建**。在终端里查：

```text
openspec list                       # 列出进行中的 change
openspec show update-password-policy # 查看某个 change 的当前口径
```

> 也可以让 AI 帮你找：「读取 openspec/changes/ 下与『密码策略』相关的 change，确认当前 proposal/design/tasks/spec 的有效口径，先别改代码。」

**产出**：原记录的位置 + 它当前的有效口径。

### 第 2 步　确认归属与状态

确认这真的是「已有需求的变化」，而不是一个全新需求、一个缺陷修复、或只是随口讨论。再确认原记录当前状态允许继续推进：

- **进行中**：直接在原 change 上改。
- **已完成未归档**：可以补改，注意同步验收。
- **已归档**：原则上新开 change 承接，并注明与旧 change 的关系。

需要看完成进度辅助判断，跑 `openspec status --change update-password-policy`。

**产出**：变化归属（是变更，不是新需求/缺陷）+ 状态结论。

### 第 3 步　决定继续还是新建

判断口诀：

- **继续用原 change**——目标没变、还是同一项能力，只是细化或调整。例如"密码最短长度从 8 改到 12"，仍是同一条密码策略。
- **新建 change**——目标变成了另一件事、影响的是完全不同的模块、或需要独立发布和回滚。例如原本只做"密码策略"，现在要追加"登录失败锁定账号"，这是另一项能力。

继续就不动命令，在原 change 上改；新建则：

```text
/opsx:propose lock-account-on-failed-login   # 扩展工作流用 /opsx:new
```

并在新 change 里写明它和原 change 的关系。

**产出**：继续 / 新建的决定（新建时附带新 change）。

### 第 4 步　判断变化等级

| 等级 | 典型例子 | 处理力度 |
| --- | --- | --- |
| **小变化** | 文案微调、不影响流程的展示调整 | 可简化流程 |
| **中等变化** | 校验规则调整、新增一种 OAuth 绑定方式 | 走澄清 + 影响分析 |
| **重大变化** | 认证协议更换、权限模型调整、默认登录策略变化 | 完整走澄清 + 影响分析 + 回归 |

注意：看着像"小变化"，但只要它影响了操作路径、权限可见性、状态流转、验收标准、用户对结果的预期、数据提交意图或合规含义，就按中等/重大处理。这一步只是判断，不执行命令。

**产出**：变化类型与等级。

### 第 5 步　澄清新口径

中等/重大变化必做，小变化可跳过。让 AI 把"原来是怎样、现在要变成怎样、差异在哪、影响哪些范围、验收怎么跟着变"理清楚：

```text
$brainstorming
帮我澄清密码策略这次变更：原口径与新口径的差异、影响范围、验收标准的变化。一次问一个问题，先不要改代码。
```

> 不必在提示词里叮嘱"别写独立设计稿"——`AGENTS.md` 文末已约束 Superpowers 产出回填到 change 目录，自动生效。

**产出**：新口径要点（原口径 → 新口径的差异 + 影响范围 + 验收变化）。

### 第 6 步　更新正文为当前口径

让 AI 把受影响的文档改成**当前有效口径**——按变化类型决定改哪些文件，别只改 `proposal` 漏了 `spec` / `tasks`：

| 变化类型 | 要改的文件 |
| --- | --- |
| 补充说明 | `proposal` |
| 范围变化 | `proposal` + `spec` |
| 验收调整 | `spec` + `tasks` |
| 方案调整 | `design` + `tasks`（可能含 `spec`） |

```text
/opsx:continue update-password-policy
```

> 扩展命令不可用：直接让 AI 更新 `openspec/changes/update-password-policy/` 下对应文档。

**产出**：更新为当前口径的 proposal / design / tasks / specs。

### 第 7 步　追加变化记录

在 change 里**追加**一条变化记录（模板见文末），保留原口径，**绝不覆盖历史**——这是变更可追溯的关键。让 AI 写或手动写都行，CLI 不生成正文。

**产出**：一条追加的变化记录。

### 第 8 步　重做影响分析

中等/重大变化必做。口径变了，受影响范围也可能变，重新评估一遍并把结论写进 `design` / `tasks`：

```text
codegraph_explore "password policy 相关功能"
codegraph_callers <符号>
codegraph_callees <符号>
codegraph_impact <符号>
```

> 没装 CodeGraph：人工分析并记录哪些范围没覆盖到。

**产出**：更新后的影响分析结论。

### 第 9 步　同步 tasks 与验收

更新任务、验证方式、回归范围——**不能拿旧测试覆盖新验收**。任务结构变化大时，让 AI 配合 `$writing-plans` 重新拆——但任务要落进 change 的 `tasks.md`，不要另存独立计划文件：

```text
/opsx:continue update-password-policy   # 或直接让 AI 更新 tasks.md
```

**产出**：更新后的 `tasks.md` 与验收标准。

### 第 10 步　实现、验证、归档

正文 / 记录 / 任务 / 验收全部同步后，再实现和验证；整个 change 都完成才归档。实现途中又冒出新变化，回到第 4 步重新判断，**别直接把变化吸进代码**。

```text
/opsx:apply update-password-policy        # 实现；启用 Superpowers 时可配合 $test-driven-development（推荐，非强制）
openspec validate update-password-policy  # 校验文档结构；再加项目自身 test/build/lint
/opsx:verify update-password-policy       # 扩展工作流
/opsx:sync update-password-policy         # 完成后合并 delta spec
/opsx:archive update-password-policy      # 完成后归档；或 openspec archive <change-name>
```

**产出**：代码 + 验证证据 + 归档状态。

## 变化记录模板（第 7 步产出）

```text
时间：
来源：
变化类型：（小 / 中等 / 重大）
原口径：
新口径：
影响范围：
验收变化：
实现影响：
验证调整：
剩余风险：
```

记录位置：change 目录的变化记录区、`proposal.md` 末尾「变化记录」章节，或外部需求系统 / PR 描述。

## 未启用 OpenSpec 时

把 change 换成对应的 issue、PR 或外部需求记录：同样先定位原记录、更新正文为当前口径、追加变化记录、同步验收，再改代码。各工具 fallback 见 [`流程公共约定.md`](../../../流程公共约定.md)。`AGENTS.md` 底线不打折。
