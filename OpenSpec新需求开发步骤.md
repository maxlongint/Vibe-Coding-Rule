# OpenSpec 新需求开发步骤

> **适用范围：** 项目**已安装并初始化 OpenSpec**（存在 `openspec/`）。未启用 OpenSpec 时，新需求按 [AGENTS.md](./AGENTS.md)「需求治理与影响分析（自洽）」建立 issue、PR 或轻量记录，见下文「前置条件」。

本文说明在已启用 OpenSpec 的项目中，如何用 OpenSpec change 推进一个新需求。强制规则见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) 和 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)；与 Superpowers 的流程对照见 [Superpowers与OpenSpec使用指南.md](./Superpowers与OpenSpec使用指南.md)；复杂需求完整示例见 [SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md)。

## 一句话

OpenSpec 管「要做什么、为什么做、做到什么程度算完成」；实现前先把 change 建好，再按 `tasks.md` 写代码，最后验证并归档。

## 前置条件

1. 项目已初始化 OpenSpec（根目录存在 `openspec/`）。
2. 已读取 `AGENTS.md`，确认本次变更是**真实业务需求**（会改变行为、接口、数据、权限、流程或验收标准）。
3. 项目已启用 OpenSpec 时，中等及以上需求默认走 OpenSpec change；未启用 OpenSpec 时，按 [AGENTS.md](./AGENTS.md)「需求治理与影响分析（自洽）」选择 issue、PR、外部系统或轻量记录。

如果项目还没有 `openspec/`，**不要默认直接初始化**。先按 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) 确认需求记录载体：

```text
本次变更属于真实业务需求。项目尚未初始化 OpenSpec。
请先向用户确认采用哪种治理方式：
1. 初始化 OpenSpec（确认后再安装并执行 openspec init）
2. 使用外部需求系统（保留链接/编号）
3. 使用轻量记录（任务记录、PR 描述或交付说明，须可追溯）

在用户确认前，只读分析、复现和影响判断可以继续；
正式改动须先完成记录载体确认，再进入设计与实现。
```

用户确认选择 OpenSpec 后，再执行安装与初始化：

```text
用户已确认采用 OpenSpec。请为当前项目安装并初始化 OpenSpec。
先检查是否已安装 openspec；未安装则从 https://github.com/Fission-AI/OpenSpec 安装。
然后在项目根目录执行 openspec init。
初始化后说明创建了哪些目录、命令或 AI 指令文件，并给出第一个 change 的建议路径。
```

## OpenSpec 斜杠命令与扩展工作流

OpenSpec 在 AI 工具中提供的 `/opsx:*` 斜杠命令，分**基础工作流**和**扩展工作流**两类：

| 类型 | 典型命令 | 启用前提 |
| --- | --- | --- |
| 基础工作流 | `/opsx:propose`、`/opsx:apply`、`/opsx:sync`、`/opsx:archive` | 通常在项目根目录执行 `openspec init` 后即可使用 |
| 扩展工作流 | `/opsx:new`、`/opsx:continue`、`/opsx:ff`、`/opsx:verify` | 需先在项目中配置对应 **profile**，并执行 `openspec update`；否则命令可能不存在 |

若扩展命令不可用，改用基础路径（`/opsx:propose` → `/opsx:apply` → …），或让 AI 直接读写 `openspec/changes/<change-name>/` 下的 `proposal.md`、`design.md`、`tasks.md` 和 `specs/`。

## 完整步骤

```text
1. 判断需求等级
2. 澄清需求（可选但推荐）
3. 创建 OpenSpec change
4. 补齐 change 文档
5. 影响分析（涉及代码改动时必须完成）
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
| 小修、文案、局部呈现 | 通常不需要 |
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

**完整路径（扩展工作流，需先配置 profile 并执行 `openspec update`）：**

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

### 第 5 步：影响分析（涉及代码改动时必须完成）

涉及代码改动时，**必须先完成影响分析**，再进入实现。CodeGraph 可用时优先使用；不可用时改用人工搜索、目录检查和调用链推断，并在 `design.md`、`tasks.md` 或交付说明中记录限制和剩余风险。

典型动作：

1. 用 CodeGraph 分析调用链、依赖和影响范围（可用时）。
2. 把结论写入 `design.md` 或 `tasks.md`。
3. CodeGraph 不可用时，记录人工影响判断和剩余风险。

### 第 6 步：补全设计与任务

```text
/opsx:ff <change-name>        # 快速补全（扩展工作流）
# 或
/opsx:continue <change-name>  # 分步继续（扩展工作流）
```

上述命令需先配置 profile 并执行 `openspec update`；不可用时，让 AI 直接补全 `openspec/changes/<change-name>/` 下的文档。

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
/opsx:verify <change-name>   # 扩展工作流；不可用时可省略，改用 openspec validate 与项目测试
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

**路径 B（完整，扩展工作流；需先配置 profile 并执行 `openspec update`）：**

```text
/opsx:new <name> -> /opsx:ff 或 /opsx:continue -> /opsx:apply -> /opsx:verify -> /opsx:archive
```

扩展命令不可用时，退回路径 A，或直接维护 `openspec/changes/<change-name>/` 文档。

## OpenSpec 与 Superpowers 怎么配合

| 阶段 | OpenSpec 命令 | Superpowers 技能 | 说明 |
| --- | --- | --- | --- |
| 需求澄清 | — | `brainstorming` | 对话式澄清，不是 `/opsx:` 命令 |
| 创建 change | `/opsx:propose` 或 `/opsx:new`（扩展） | — | `/opsx:new` 需 profile + `openspec update` |
| 设计与任务 | `/opsx:ff` 或 `/opsx:continue`（扩展） | `writing-plans` | 扩展命令不可用则直接维护 change 文档 |
| 实现 | `/opsx:apply` | `test-driven-development` | 按 `tasks.md` 推进 |
| 验证 | `/opsx:verify`（扩展）、`openspec validate` | `verification-before-completion` | 需要验证证据 |
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
3. 如果不存在，**先向用户确认需求记录载体**（OpenSpec、外部系统或轻量记录），见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)；用户确认 OpenSpec 后再初始化，不得默认直接 `openspec init` 或跳过治理底线。
4. 涉及代码改动时，必须先完成影响分析（CodeGraph 优先；不可用时人工分析并记录风险）。
5. 实现前必须有设计或任务清单。
6. 完成前必须运行验证。
7. OpenSpec 项目不要把设计单独写到 `docs/superpowers/specs/` 或 `docs/superpowers/plans/`。
