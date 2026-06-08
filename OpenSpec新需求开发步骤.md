# OpenSpec 新需求开发步骤

本文说明在已启用 OpenSpec 的项目中，如何用 OpenSpec change 推进一个新需求。强制规则见 `AGENTS.md` 第 1 节「目录规则」和第 8 节「AI 执行规则」；与 Superpowers 的流程对照见 `Superpowers与OpenSpec使用指南.md`；复杂需求完整示例见 `SSO 单点登录复杂需求执行步骤示例.md`。

## 一句话

OpenSpec 管「要做什么、为什么做、做到什么程度算完成」；实现前先把 change 建好，再按 `tasks.md` 写代码，最后验证并归档。

## 前置条件

1. 项目已初始化 OpenSpec（根目录存在 `openspec/`）。
2. 已读取 `AGENTS.md`，确认本次变更是**真实业务需求**（会改变行为、接口、数据、权限、流程或验收标准）。
3. 中等及以上需求默认必须走 OpenSpec change。

如果项目还没有 `openspec/`，先初始化：

```text
请为当前项目初始化 OpenSpec。
先检查是否已安装 openspec；未安装则从 https://github.com/Fission-AI/OpenSpec 安装。
然后在项目根目录执行 openspec init。
初始化后说明创建了哪些目录、命令或 AI 指令文件，并给出第一个 change 的建议路径。
```

## 完整步骤

```text
1. 判断需求等级
2. 澄清需求（可选但推荐）
3. 创建 OpenSpec change
4. 补齐 change 文档
5. 影响分析（涉及代码改动时推荐）
6. 补全设计与任务
7. 方案确认后实现
8. 验证
9. 同步与归档
```

### 第 1 步：判断要不要建 change

| 情况 | 是否建 change |
| --- | --- |
| 新功能、中等/重大需求 | **必须** |
| 改 API、数据模型、权限、状态流 | **必须** |
| 小修、文案、局部样式 | 通常不需要 |
| 纯 bug 修复（不改业务规则） | 通常不需要 |

OpenSpec 硬触发条件：只要本次修改会改变外部可观察行为、接口契约、数据结构、权限规则、状态流转、验收标准或用户操作路径，就必须先有可追溯需求记录。

### 第 2 步：澄清需求（可选但推荐）

这一步**不是 OpenSpec 命令**，而是 **Superpowers 技能**，通过自然语言让 AI 启用，不是终端里的 `openspec` CLI。

| 类型 | 名称 | 怎么触发 |
| --- | --- | --- |
| Superpowers 技能 | `brainstorming` | 在对话里说明要用它 |
| 不是 | `/opsx:propose` 等 | 那是 OpenSpec 命令，属于第 3 步 |

常用提示词：

```text
请先用 superpowers:brainstorming 理解这个需求，不要直接写代码。
```

或：

```text
我要新增 XXX 功能，先澄清目标、范围、非目标和验收标准，确认后再创建 OpenSpec change。
```

`brainstorming` 会按技能流程提问、给方案、等待确认。需求已经很清楚时，可以跳过本步，直接进入第 3 步；需求模糊或影响面大时，建议先做本步再创建 change。

**OpenSpec 项目注意：** `brainstorming` 的产出应写入 `openspec/changes/<change-name>/`，不要落到 `docs/superpowers/specs/` 或 `docs/superpowers/plans/`。

需要澄清的典型问题：

- 做什么 / 不做什么
- 影响哪些模块、用户路径
- 验收标准是什么
- 有哪些风险和待决事项

### 第 3 步：创建 OpenSpec change

在 Cursor 等支持 OpenSpec 指令的环境中，可用斜杠命令，二选一：

**快捷路径：**

```text
/opsx:propose <change-name>
```

**完整路径：**

```text
/opsx:new <change-name>
```

`change-name` 使用短横线命名，例如 `add-sso-login`、`add-email-login`。

也可以直接对 AI 说：

```text
这个项目使用 OpenSpec。请为这个需求创建一个 OpenSpec change，
先写 proposal、delta specs、design 和 tasks，不要直接实现。
```

### 第 4 步：补齐 change 文档

所有内容统一落在：

```text
openspec/changes/<change-name>/
  proposal.md      # 为什么做、做什么、范围、风险
  design.md        # 技术方案、架构决策、权衡
  tasks.md         # 实现任务清单 + 验证方式 + 完成状态
  specs/
    <domain>/
      spec.md      # 外部可观察行为和验收场景（delta spec）
```

各文件职责：

| 文件 | 写什么 |
| --- | --- |
| `proposal.md` | 背景、目标、非目标、用户影响、验收标准、风险 |
| `design.md` | 方案、数据流、安全策略、回滚方案 |
| `specs/.../spec.md` | 用户可见行为、接口契约、验收场景；不写实现细节 |
| `tasks.md` | 可勾选的任务项，含验证步骤和完成状态 |

### 第 5 步：影响分析（涉及代码改动时推荐）

涉及多模块、跨前后端或权限/数据变更时，用 CodeGraph 分析调用链、依赖和影响范围，把结论写入 `design.md` 或 `tasks.md`。

### 第 6 步：补全设计与任务

```text
/opsx:ff <change-name>        # 快速补全
# 或
/opsx:continue <change-name>  # 分步继续
```

这一阶段可配合 Superpowers 的 `writing-plans` 做任务拆分，但产出仍应落在 `openspec/changes/<change-name>/`，不要写到 `docs/superpowers/plans/`。

### 第 7 步：方案确认后实现

**先更新 OpenSpec，再改代码。**

实现阶段命令：

```text
/opsx:apply <change-name>
```

配合 Superpowers 的 `test-driven-development`，按 `tasks.md` 逐项推进，并更新任务完成状态。

常用提示词：

```text
请按 OpenSpec 的 tasks.md 开始实现这个 change，
并用 TDD 和 verification-before-completion 做验证。
```

### 第 8 步：验证

```text
/opsx:verify <change-name>
openspec validate
```

再加上项目自身的测试、构建、lint 等。完成前必须有验证证据，不能把未验证结果表述为已通过。

### 第 9 步：同步与归档

实现完成、验收通过后：

```text
/opsx:sync <change-name>    # 把 delta spec 合并到主 spec（如需要）
/opsx:archive <change-name> # 归档 change
```

在 change 或外部需求系统里标记状态：`进行中`、`已完成`、`已暂停`、`已废弃`、`已替代`。

## 两条常用命令路径

**路径 A（简洁）：**

```text
/opsx:propose <name> -> /opsx:apply -> /opsx:sync -> /opsx:archive
```

**路径 B（完整）：**

```text
/opsx:new <name> -> /opsx:ff 或 /opsx:continue -> /opsx:apply -> /opsx:verify -> /opsx:archive
```

## OpenSpec 与 Superpowers 怎么配合

| 阶段 | OpenSpec 命令 | Superpowers 技能 | 说明 |
| --- | --- | --- | --- |
| 需求澄清 | — | `brainstorming` | 对话式澄清，不是 `/opsx:` 命令 |
| 创建 change | `/opsx:propose` 或 `/opsx:new` | — | 生成 change 目录和主文档 |
| 设计与任务 | `/opsx:ff` 或 `/opsx:continue` | `writing-plans` | 产出写入 OpenSpec change |
| 实现 | `/opsx:apply` | `test-driven-development` | 按 `tasks.md` 推进 |
| 验证 | `/opsx:verify`、`openspec validate` | `verification-before-completion` | 需要验证证据 |
| 归档 | `/opsx:archive` | — | 完成后归档规格 |

OpenSpec 是需求和规格的主流程；Superpowers 是 AI 执行纪律，不替代需求记录。

## 最小实操示例

假设要做「新增邮箱登录」：

```text
1. （可选）用 brainstorming 澄清需求
2. /opsx:propose add-email-login
3. AI 生成 openspec/changes/add-email-login/ 下的 proposal、design、tasks、specs
4. 确认方案和验收标准
5. /opsx:apply add-email-login（或让 AI 按 tasks.md 实现）
6. 跑测试 + openspec validate + /opsx:verify
7. /opsx:sync add-email-login（如需要）
8. /opsx:archive add-email-login
```

## 常用提示词

### 创建 change（不写代码）

```text
这个项目使用 OpenSpec。请为这个需求创建一个 OpenSpec change，
先写 proposal、delta specs、design 和 tasks，不要直接实现。
```

### 实现阶段

```text
请按 OpenSpec 的 tasks.md 开始实现这个 change，
并用 TDD 和 verification-before-completion 做验证。
```

### 需求澄清（Superpowers）

```text
我要给现有功能增加一个新功能，请先用 superpowers:brainstorming 理解需求，不要直接写代码。
```

## 默认策略

1. 先检查项目是否存在 `openspec/`。
2. 如果存在，走 OpenSpec change 主流程。
3. 如果不存在，走 Superpowers 流程，或先向用户确认是否初始化 OpenSpec。
4. 实现前必须有设计或任务清单。
5. 完成前必须运行验证。
6. OpenSpec 项目不要把设计单独写到 `docs/superpowers/specs/` 或 `docs/superpowers/plans/`。
