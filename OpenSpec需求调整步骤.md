# OpenSpec 需求调整步骤

本文说明在已启用 OpenSpec 的项目中，如何对**已有需求**进行补充、范围变化、验收调整或方案调整。强制规则见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)；新需求从零创建的流程见 [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md)；项目级需求变化同步技能见 [updating-requirement-changes/SKILL.md](./.agents/skills/updating-requirement-changes/SKILL.md)；复杂需求示例见 [SSO 单点登录复杂需求执行步骤示例.md](./SSO%20单点登录复杂需求执行步骤示例.md) 第 8 节。

## 一句话

需求调整不是新建 change，而是**先找到原 change、判断变化等级、更新当前有效口径、追加变化记录，再同步任务和验收，最后才改代码**。

## 和新需求的区别

| | 新需求 | 需求调整 |
| --- | --- | --- |
| 起点 | 从零创建 change | 找到**已有** OpenSpec change 或外部需求记录 |
| 典型 AI 命令 | `/opsx:propose <name>` 或 `/opsx:new <name>`（扩展） | `/opsx:continue <name>`（扩展）或直接更新 change 文档 |
| 是否新建 change | **必须**新建 | **通常继续用原 change** |
| 历史处理 | 无历史 | 正文更新为当前口径，**追加**变化记录，不覆盖历史 |
| 核心原则 | 先规格后实现 | **先改记录，再改代码** |

## 前置条件

1. 项目已初始化 OpenSpec（根目录存在 `openspec/`）。
2. 已读取 `AGENTS.md`，确认本次调整属于**已有需求的变化**，而不是全新需求或普通讨论。
3. 已定位到原记录载体：`openspec/changes/<change-name>/`，或外部需求系统链接/编号。
4. 若变化会影响行为、接口、数据、权限、流程或验收标准，必须先同步需求记录，再进入实现。

## OpenSpec 斜杠命令与扩展工作流

| 类型 | 典型命令 | 启用前提 |
| --- | --- | --- |
| 基础工作流 | `/opsx:propose`、`/opsx:apply`、`/opsx:sync`、`/opsx:archive` | 通常在 `openspec init` 后即可使用 |
| 扩展工作流 | `/opsx:new`、`/opsx:continue`、`/opsx:ff`、`/opsx:verify` | 需先配置 **profile** 并执行 `openspec update`；否则命令可能不存在 |

扩展命令不可用时，直接更新 `openspec/changes/<change-name>/` 下的文档，或使用 `openspec validate` 与项目测试完成验证。

## 完整步骤

```text
1. 定位已有 change 或外部记录
2. 确认变化归属与 change 状态
3. 判断继续原 change 还是新建 change
4. 判断变化类型
5. 判断变化等级（小 / 中等 / 重大）
6. 澄清新口径（中/重大变化推荐）
7. 更新 OpenSpec 文档正文
8. 追加需求变化记录
9. 重新做影响分析（中/重大变化）
10. 同步 tasks 和验收标准
11. 重新确认后实现
12. 验证
13. 同步与归档（如 change 已完成）
```

---

### 第 1 步：定位已有 change 或外部记录

先找到这次调整对应的原需求记录，不要默认新建 change。

**终端命令（CLI）：**

```bash
# 列出当前进行中的 change
openspec list

# 查看某个 change 的详情
openspec show <change-name>

# 查看 artifact 完成进度
openspec status --change <change-name>

# 如需查看已归档 change，到目录中查找
# openspec/changes/archive/
```

**AI 提示词（无命令时）：**

```text
请先读取 openspec/changes/ 下与「XXX」相关的 change，
确认当前 proposal、design、tasks 和 spec 的有效口径，暂时不要改代码。
```

**检查点：**

- 已找到原 change 路径，或已确认使用外部需求系统记录。
- 已了解原需求当前目标、范围、验收标准和任务状态。

---

### 第 2 步：确认变化归属与 change 状态

先判断这次输入到底是什么，避免把临时想法、缺陷修复或全新需求误当成「需求调整」。

**变化归属判断：**

| 归属 | 说明 | 处理方式 |
| --- | --- | --- |
| 原需求变化 | 同一能力下的补充、范围或验收调整 | 走本文流程 |
| 新需求 | 目标变成另一项系统能力 | 见 [OpenSpec新需求开发步骤.md](./OpenSpec新需求开发步骤.md)，新建 change |
| 缺陷修复 | 修复实现偏差，不改变已确认业务规则 | 通常不建 change；若改变验收标准则按需求变化处理 |
| 轻量调整 | 文案、样式等不影响验收底线 | 更新任务记录或交付说明即可 |
| 外部系统已有事项 | Jira、Linear 等已有记录 | 同步到 OpenSpec 或保留外部链接 |

**change 状态判断：**

| 状态 | 含义 | 下一步 |
| --- | --- | --- |
| 进行中 | 尚未完成或仍在实现 | 直接在本 change 上调整 |
| 已完成 | 已验收但尚未归档 | 通常继续本 change；重大变化可评估是否新建 |
| 已归档 | 在 `openspec/changes/archive/` | 先判断恢复还是新建（见第 3 步） |
| 已暂停 / 已废弃 / 已替代 | 需求已停止或替换 | 先确认是否恢复，再决定是否调整 |

**检查点：**

- 已确认这是「已有需求的变化」，不是新需求或普通讨论。
- 已确认原 change 当前状态允许继续变更。

---

### 第 3 步：判断继续原 change 还是新建 change

这是需求调整最关键的分流步骤。

**继续使用原 change：**

- 目标仍相同，只是范围、规则或验收有变化。
- 仍围绕同一项能力。
- change 还在进行中，或已完成但范围相近、可恢复。

**新建 OpenSpec change：**

- 目标变成另一项系统能力。
- 原 change 已归档，且影响范围显著变化。
- 新调整影响完全不同模块。
- 需要独立评审、发布和回滚。

**已归档 change 的处理：**

| 情况 | 做法 |
| --- | --- |
| 目标和验收仍有效，范围相近 | **恢复原 change**，追加变更说明后继续 |
| 目标已变，或影响范围显著变化 | **新建 change** |

**新建 change 时使用的命令：**

```text
/opsx:propose <new-change-name>
```

或：

```text
/opsx:new <new-change-name>
```

并在 `proposal.md` 中注明与原 change 的关系、替代原因或依赖关系。

**检查点：**

- 已明确是「继续原 change」还是「新建 change」。
- 若原 change 已归档，已做出恢复或新建的判断。

---

### 第 4 步：判断变化类型

不同变化类型，决定主要更新哪些 artifact。

| 变化类型 | 含义 | 主要更新文件 |
| --- | --- | --- |
| 补充说明 | 不改变目标，只补清约束、边界或解释 | `proposal.md` |
| 范围变化 | 新增、移除或替换能力范围 | `proposal.md`、`spec.md` |
| 验收调整 | 改变通过条件、场景、反向断言或验证方式 | `spec.md`、`tasks.md` |
| 方案调整 | 改变实现方案、数据流、权限、发布或回滚策略 | `design.md`、`tasks.md`，可能影响 `spec.md` |
| 记录同步 | 把用户确认、评审结论或外部 issue 同步回 OpenSpec | 按影响范围同步 |

**检查点：**

- 已确认变化类型。
- 已知道优先改哪些文件。

---

### 第 5 步：判断变化等级

| 等级 | 典型例子 | 处理强度 |
| --- | --- | --- |
| 小变化 | 按钮文案、非关键提示语、不影响流程的布局微调 | 更新 tasks 或交付说明，重跑相关测试 |
| 中等变化 | 校验规则调整、绑定规则改变、新增 OAuth 绑定 | 更新 design、spec、tasks，重做必要影响分析 |
| 重大变化 | 认证协议变更、权限模型重构、默认登录策略改变 | 暂停实现，全面更新文档，重新评审，必要时新建 change |

**升级规则：**

小变化若影响以下任一项，应按**中等或重大**处理：

- 操作路径
- 权限可见性
- 状态流转
- 验收标准
- 用户对操作结果的预期
- 数据提交意图
- 法律或合规含义

**检查点：**

- 已判断变化等级。
- 已识别是否需要暂停实现。

---

### 第 6 步：澄清新口径（中/重大变化推荐）

中/重大变化建议先做需求澄清，避免在旧口径和新口径之间猜测。

这一步**不是 OpenSpec 斜杠命令**，而是 **Superpowers 技能**：

| 类型 | 名称 | 怎么触发 |
| --- | --- | --- |
| Superpowers 技能 | `brainstorming` | 在对话里说明要用它 |
| 项目技能（可选） | `updating-requirement-changes` | 用户明确点名时使用 |

**常用提示词：**

```text
已有需求「XXX」要调整，请先澄清原口径和新口径的差异、影响范围和验收变化，不要直接写代码。
```

若用户明确点名项目技能：

```text
请使用 updating-requirement-changes 处理 openspec/changes/<change-name>/ 的需求变化。
新口径是：...
请先确认变化归属和变化等级，再同步文档。
```

**OpenSpec 项目注意：** 澄清结果应写入 `openspec/changes/<change-name>/`，不要落到 `docs/superpowers/specs/`。

**检查点：**

- 已明确原口径和新口径。
- 已确认影响范围和待决问题。
- 小变化可跳过本步，直接进入第 7 步。

---

### 第 7 步：更新 OpenSpec 文档正文

把 change 下的文档更新为**当前有效口径**。这是需求调整的核心动作。

**按文件职责更新：**

| 文件 | 更新内容 |
| --- | --- |
| `proposal.md` | 目标、非目标、范围、用户影响、风险、当前状态 |
| `design.md` | 方案取舍、数据流、权限、状态流转、发布和回滚 |
| `tasks.md` | 任务拆分、状态、验证命令、剩余风险 |
| `specs/<domain>/spec.md` | 外部可观察行为、验收场景、正向和反向断言 |

`spec.md` 应写用户/系统/外部接口可观察到的行为，不要写成内部实现清单。

**AI 斜杠命令（补全或分步修订 artifact）：**

```text
/opsx:continue <change-name>
```

适用于需要 AI 按 OpenSpec 工作流继续补全或修订 `proposal`、`design`、`tasks`、`specs` 的场景。

**AI 提示词（直接更新已有 change）：**

```text
请更新 openspec/changes/<change-name>/ 中与本次变化相关的文档。
变化内容：
- 原口径：...
- 新口径：...
- 变化类型：...
- 变化等级：...

请同步 proposal、design、tasks 和 spec 中受影响的部分。
需求正文更新为当前有效口径，暂时不要改代码。
```

**小变化示例：**

```text
变化：登录按钮文案从「SSO 登录」改为「企业登录」
更新：tasks.md 或交付说明
通常不必改 design.md 和 spec.md
```

**中等变化示例：**

```text
变化：用户绑定规则从邮箱匹配改为工号匹配
更新：design.md、specs/auth/spec.md、tasks.md
```

**重大变化示例：**

```text
变化：从 OIDC 改为 SAML
更新：proposal.md、design.md、tasks.md、spec.md
必要时暂停实现并新建 change
```

**检查点：**

- 当前有效口径已写入对应文档。
- 过期口径已被替换，但变化原因将在第 8 步追加到变化记录。
- 没有只改 `proposal.md` 而遗漏 `spec.md` 或 `tasks.md`。

---

### 第 8 步：追加需求变化记录

正文改成当前有效口径的同时，必须**追加**一条变化记录，保留历史变化事实。

**变化记录模板：**

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

**记录位置（按项目约定择一或组合）：**

- OpenSpec change 目录下的变化记录区
- `proposal.md` 末尾的「变化记录」章节
- 任务记录、PR 描述、交付说明
- 外部需求系统中的变更备注

**AI 提示词：**

```text
请在 openspec/changes/<change-name>/ 追加一条需求变化记录，
保留时间、来源、原口径、新口径、影响范围、验收变化和剩余风险。
不要删除或覆盖已有变化历史。
```

**检查点：**

- 变化记录已追加，不是覆盖旧记录。
- 已保留原口径和变更原因。

---

### 第 9 步：重新做影响分析（中/重大变化）

若变化影响模块、数据流、权限、状态流转或跨前后端边界，**必须先完成影响分析**，再进入实现。

**必须动作：**

1. 完成影响分析（CodeGraph 可用时优先使用）。
2. 把结论写入 `design.md` 或 `tasks.md`。
3. CodeGraph 不可用时，记录人工影响判断和剩余风险。

**终端命令（辅助查看 change 当前状态）：**

```bash
openspec show <change-name>
openspec status --change <change-name>
```

**AI 提示词：**

```text
本次需求变化会影响认证绑定和权限映射。
请先用 CodeGraph 分析受影响模块、调用链和数据流，
把结论写入 openspec/changes/<change-name>/design.md 和 tasks.md。
```

**重大变化额外要求：**

- 暂停实现。
- 重新做方案评审。
- 重新拆分实现计划。
- 必要时新建 change。

**检查点：**

- 中/重大变化已完成影响分析或已记录人工判断。
- 影响结论已写入设计或任务文档。

---

### 第 10 步：同步 tasks 和验收标准

需求变化后，任务清单和验证方式必须同步，不能沿用旧测试覆盖新验收。

**主要更新 `tasks.md`：**

- 新增、删除或调整任务项。
- 更新任务完成状态。
- 更新验证命令和回归范围。
- 记录剩余风险和未验证项。

**若任务结构变化较大，可配合 Superpowers：**

| 类型 | 名称 | 说明 |
| --- | --- | --- |
| Superpowers 技能 | `writing-plans` | 重新拆分实现计划 |
| AI 斜杠命令 | `/opsx:continue <change-name>` | 让 AI 继续补全 tasks 等 artifact |

**AI 提示词：**

```text
请根据本次需求变化更新 openspec/changes/<change-name>/tasks.md：
1. 调整任务拆分和优先级
2. 更新验证命令
3. 标记已完成、需重做和新增的任务
4. 写明剩余风险
```

**检查点：**

- `tasks.md` 与最新 spec 和 design 一致。
- 验收标准和测试/验证范围已同步更新。

---

### 第 11 步：重新确认后实现

**底线：先改记录，再改代码。**

只有在以下条件满足后，才进入实现：

- 当前有效口径已更新。
- 变化记录已追加。
- 任务和验收标准已同步。
- 中/重大变化已完成必要确认或评审。

**实现阶段 AI 斜杠命令：**

```text
/opsx:apply <change-name>
```

**AI 提示词：**

```text
需求变化已同步到 openspec/changes/<change-name>/。
请按更新后的 tasks.md 继续实现，并用 TDD 做验证。
若发现新的变化，先更新 OpenSpec，不要直接把变化吸收进代码。
```

**注意：**

- 若用户只要求同步记录，不要顺手实现代码。
- 实现过程中若再出现新变化，回到第 4 步重新判断，不能直接改代码吸收。

**检查点：**

- 已确认文档、任务、验收同步完成。
- 实现严格按更新后的 `tasks.md` 推进。

---

### 第 12 步：验证

文档和代码同步后，必须重新验证，不能用旧验收覆盖新口径。

**AI 斜杠命令：**

```text
/opsx:verify <change-name>
```

**终端命令（CLI）：**

```bash
# 校验 change 文档结构
openspec validate <change-name>

# 查看当前进度
openspec status --change <change-name>
```

再加上项目自身的测试、构建、lint 等。

**配合 Superpowers：**

| 类型 | 名称 |
| --- | --- |
| Superpowers 技能 | `verification-before-completion` |

**AI 提示词：**

```text
请根据 openspec/changes/<change-name>/ 更新后的验收标准和 tasks.md，
列出必须运行的验证命令并执行。
完成前不要声明已通过。
```

**按变化等级的验证强度：**

| 等级 | 验证要求 |
| --- | --- |
| 小变化 | 相关测试或最小验证 |
| 中等变化 | 相关模块测试 + 回归关键路径 |
| 重大变化 | 完整相关测试 + 影响分析涉及模块的验证 + 必要时评审 |

**检查点：**

- `openspec validate` 通过（如项目使用）。
- 项目测试/构建已按新验收标准执行。
- 已说明验证结果和剩余风险。

---

### 第 13 步：同步与归档（如 change 已完成）

若本次调整后 change 已全部完成，走归档流程；若仍在进行中，则保持 change 活跃状态。

**AI 斜杠命令：**

```text
/opsx:sync <change-name>    # 将 delta spec 合并到主 spec（如需要）
/opsx:archive <change-name> # 归档已完成 change
```

**终端命令（CLI）：**

```bash
openspec archive <change-name>
```

归档后：

1. delta spec 合并进 `openspec/specs/<domain>/spec.md`
2. change 目录移到 `openspec/changes/archive/YYYY-MM-DD-<change-name>/`

**若 change 尚未完成：**

- 保持 `进行中` 状态。
- 不要提前归档。
- 后续若再有调整，继续在本 change 上执行本文流程。

**检查点：**

- 仅在需求已完成且通过验收后才归档。
- 归档前确认不再需要追加实现任务。

---

## 按变化等级的快速路径

### 小变化

```text
1. openspec show <change-name>
2. 更新 tasks.md 或交付说明
3. 追加变化记录
4. 重跑相关测试
5. 继续实现（如仍有剩余任务）
```

### 中等变化

```text
1. openspec show <change-name>
2. 判断变化类型和等级
3. 更新 design.md、spec.md、tasks.md
4. 追加变化记录
5. 必要时做 CodeGraph 影响分析
6. /opsx:apply <change-name>
7. openspec validate <change-name> + 项目测试
8. /opsx:verify <change-name>
```

### 重大变化

```text
1. 暂停实现
2. openspec show <change-name> + openspec status --change <change-name>
3. 判断是否继续原 change 或 /opsx:propose <new-change-name>
4. 用 brainstorming 重新澄清（推荐）
5. 全面更新 proposal、design、tasks、spec
6. 追加变化记录
7. CodeGraph 影响分析 + 方案评审
8. /opsx:continue <change-name> 或 /opsx:apply <change-name>
9. openspec validate + /opsx:verify + 项目测试
10. /opsx:sync + /opsx:archive（完成后）
```

---

## 命令对照表

### AI 斜杠命令

扩展命令（`/opsx:new`、`/opsx:continue`、`/opsx:ff`、`/opsx:verify`）需 profile + `openspec update`；不可用时直接维护 change 文档或使用 CLI。

| 步骤 | 命令 | 用途 |
| --- | --- | --- |
| 定位与修订 artifact | `/opsx:continue <change-name>`（扩展） | 分步补全或修订 proposal、design、tasks、specs |
| 重大变化新建 change | `/opsx:propose <new-change-name>` | 目标变化过大时新建 change |
| 重大变化新建 change | `/opsx:new <new-change-name>`（扩展） | 扩展工作流下新建 change |
| 继续实现 | `/opsx:apply <change-name>` | 按更新后的 tasks.md 推进实现 |
| 验证 | `/opsx:verify <change-name>`（扩展） | 按更新后的验收标准验证；不可用则用 `openspec validate` + 项目测试 |
| 同步主 spec | `/opsx:sync <change-name>` | 完成后合并 delta spec |
| 归档 | `/opsx:archive <change-name>` | 归档已完成 change |

**通常不需要：**

- `/opsx:propose` 用于小/中等的原需求微调（应直接更新原 change）
- 把调整后的设计写到 `docs/superpowers/specs/` 或 `docs/superpowers/plans/`

### 终端 CLI 命令

| 步骤 | 命令 | 用途 |
| --- | --- | --- |
| 查找 change | `openspec list` | 列出进行中的 change |
| 查看详情 | `openspec show <change-name>` | 查看 change 文档与结构 |
| 查看进度 | `openspec status --change <change-name>` | 查看 artifact 完成情况 |
| 校验文档 | `openspec validate <change-name>` | 校验 spec 和 change 结构 |
| 更新元数据 | `openspec set change <name>` | 更新 change 元数据（如 initiative 关联） |
| 归档 | `openspec archive <change-name>` | 终端方式归档 change |
| 浏览 | `openspec view` | 交互式查看 specs 和 changes |

### Superpowers 技能

| 步骤 | 技能 | 说明 |
| --- | --- | --- |
| 澄清新口径 | `brainstorming` | 中/重大变化推荐；不是斜杠命令 |
| 需求变化同步 | `updating-requirement-changes` | 仅用户明确点名时使用 |
| 重拆计划 | `writing-plans` | 任务结构变化较大时使用 |
| 实现 | `test-driven-development` | 按更新后的 tasks 实现 |
| 完成前验证 | `verification-before-completion` | 必须有验证证据 |

---

## 完整示例

假设 `openspec/changes/add-email-login/` 原口径是「邮箱大小写敏感」，现调整为「邮箱大小写不敏感」。

### 1. 定位 change

```bash
openspec list
openspec show add-email-login
```

### 2. 判断

```text
归属：原需求变化
状态：进行中
等级：中等变化
决策：继续使用原 change
```

### 3. 更新文档

```text
更新 proposal.md：补充范围说明
更新 design.md：说明邮箱标准化策略
更新 specs/auth/spec.md：修改验收场景
更新 tasks.md：新增标准化逻辑和测试任务
```

### 4. 追加变化记录

```text
时间：2026-06-08
来源：用户确认
变化类型：验收调整
原口径：邮箱大小写敏感
新口径：邮箱大小写不敏感
影响范围：登录校验、用户查询、相关测试
验收变化：同一邮箱不同大小写应识别为同一账号
实现影响：需增加邮箱 normalize 逻辑
验证调整：补充大小写不敏感相关测试
剩余风险：历史数据是否存在大小写重复账号
```

### 5. 实现与验证

```text
/opsx:apply add-email-login
openspec validate add-email-login
/opsx:verify add-email-login
```

### 6. 完成后归档

```text
/opsx:sync add-email-login
/opsx:archive add-email-login
```

---

## 常用提示词

### 同步需求变化（不写代码）

```text
openspec/changes/<change-name>/ 的需求有变化：
- 原口径：...
- 新口径：...

请先更新 proposal、design、tasks 和 spec 中受影响的内容，
追加需求变化记录，不要直接改代码。
```

### 点名项目技能

```text
请使用 updating-requirement-changes 更新 openspec/changes/<change-name>/。
这次需求新口径是：...
请把需求正文更新为当前有效口径，并追加需求变化记录。
```

### 变化后继续实现

```text
需求变化已同步到 openspec/changes/<change-name>/。
请按更新后的 tasks.md 继续实现，并用 TDD 和 verification-before-completion 做验证。
```

### 实现过程中又发现新变化

```text
实现过程中发现新的需求变化：...
请先判断变化等级，更新 OpenSpec change 和变化记录，确认后再继续改代码。
```

---

## 常见错误

| 错误 | 修正 |
| --- | --- |
| 聊天里口头改需求，直接改代码 | 先同步 OpenSpec，再实现 |
| 覆盖需求正文，历史变化消失 | 正文更新 + 变化记录追加 |
| 把调整当成新需求，重复建 change | 先判断归属，通常继续原 change |
| 只改 proposal，不改 spec 和 tasks | 按影响范围同步所有相关文件 |
| 验收标准变了，测试不更新 | 同步 tasks 和验证命令 |
| 已归档 change 直接加任务 | 先判断恢复还是新建 change |
| 调整后的设计写到 `docs/superpowers/specs/` | 统一写入 `openspec/changes/<change-name>/` |
| 用户没点名就自动套 `updating-requirement-changes` | 按普通需求治理规则处理；点名后才用该技能 |

---

## 完成检查清单

- [ ] 已确认这是已有需求的变化，而不是新需求或普通讨论
- [ ] 已定位原 OpenSpec change 或外部需求记录
- [ ] 已判断继续原 change 还是新建 change
- [ ] 已判断变化类型和变化等级
- [ ] 当前有效口径已更新到 proposal、design、tasks、spec
- [ ] 需求变化记录已追加，且包含时间、来源、原口径、新口径、影响范围和剩余风险
- [ ] 中/重大变化已完成影响分析或必要评审
- [ ] tasks 和验收标准已与最新口径一致
- [ ] 已先改记录，再改代码
- [ ] 已运行 `openspec validate`、项目测试和 `/opsx:verify`（如适用）
- [ ] 已说明验证结果和剩余风险
- [ ] 仅在完成后才执行 `/opsx:sync` 和 `/opsx:archive`
