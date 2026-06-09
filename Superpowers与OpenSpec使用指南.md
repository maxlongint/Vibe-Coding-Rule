# Superpowers 与 OpenSpec 用法对照

本文说明两套流程在“给现有项目/现有功能增加新功能”时应该怎么用。强制规则见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md) 和 [AI协作与工具链.md](./docs/规范/AI协作与工具链.md)；本文侧重流程对照、提示词和阶段映射。

## 结论

如果项目已经使用 OpenSpec，优先使用 **OpenSpec change 流程**，Superpowers 作为澄清、实现和验证纪律的补充。

如果项目没有 OpenSpec，可按 Superpowers 流程推进设计与计划；但若涉及**真实业务需求**，须先确认需求记录载体（初始化 OpenSpec、外部系统或轻量记录），见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)，不得跳过可追溯治理底线。

## 场景一：只使用 Superpowers

适用场景：

- 项目没有 `openspec/` 目录
- 团队没有要求用 OpenSpec 管理需求
- 你希望 AI 先理解需求，再设计、计划、实现、验证

推荐顺序：

1. `superpowers:brainstorming`
   - 用于新增功能、扩展现有功能、修改现有行为。
   - 先理解项目上下文、需求目的、范围、成功标准。
   - 不直接写代码，先形成设计并让用户确认。

2. `superpowers:writing-plans`
   - 在设计确认后，把设计拆成可执行的实施计划。
   - 适合多文件、多模块、跨前后端或有依赖顺序的需求。

3. `superpowers:test-driven-development`
   - 开始实现时使用。
   - 先写失败测试，再实现功能，再让测试通过。

4. `superpowers:verification-before-completion`
   - 完成前使用。
   - 运行测试、构建、校验命令，确认结果后再声明完成。

一句话：

> 新功能先用 `brainstorming`，设计通过后用 `writing-plans`，实现阶段用 `test-driven-development`，收尾用 `verification-before-completion`。

## 场景二：项目使用 OpenSpec

适用场景：

- 项目存在 `openspec/` 目录
- 项目使用 OpenSpec 管理需求、设计、任务和规格变更
- 需求需要形成可审查、可归档的 change

OpenSpec 的核心流程：

1. Proposal
2. Planning
3. Implementation
4. Archive

**斜杠命令说明：** `/opsx:propose`、`/opsx:apply`、`/opsx:sync`、`/opsx:archive` 属于基础工作流，通常在 `openspec init` 后可用。`/opsx:new`、`/opsx:continue`、`/opsx:ff`、`/opsx:verify` 属于**扩展工作流**，需先配置 profile 并执行 `openspec update`，否则命令可能不存在；不可用时退回基础路径或直接维护 `openspec/changes/<change-name>/` 文档。

常见命令路径：

```text
/opsx:propose <change-name> -> /opsx:apply -> /opsx:sync -> /opsx:archive
```

或扩展路径（需 profile + `openspec update`）：

```text
/opsx:new <change-name> -> /opsx:ff 或 /opsx:continue -> /opsx:apply -> /opsx:verify -> /opsx:archive
```

OpenSpec 会把需求文档放在：

```text
openspec/changes/<change-name>/
```

通常包含：

- `proposal.md`：为什么做、做什么、范围是什么
- `design.md`：技术方案、架构决策、权衡
- `tasks.md`：实现任务清单
- `specs/`：delta specs，描述新增、修改或删除的需求

## OpenSpec 与 Superpowers 的对应关系

| 阶段 | OpenSpec | Superpowers 辅助技能 |
| --- | --- | --- |
| 需求澄清 | `/opsx:propose` 或 `/opsx:new`（扩展） | `superpowers:brainstorming` |
| 设计与任务 | `/opsx:ff` 或 `/opsx:continue`（扩展） | `superpowers:writing-plans` |
| 实现 | `/opsx:apply` | `superpowers:test-driven-development` |
| 验证 | `/opsx:verify`（扩展）、`openspec validate`、项目测试 | `superpowers:verification-before-completion` |
| 归档 | `/opsx:archive` | 完成后归档规格 |

## OpenSpec 项目的推荐做法

如果项目使用 OpenSpec，不建议再单独把设计写到：

```text
docs/superpowers/specs/
```

而是应该把需求、设计和任务落到：

```text
openspec/changes/<change-name>/
```

也就是说：

> OpenSpec 是需求和规格的主流程，Superpowers 是 AI 编码代理执行时的工作纪律。

## 选择规则

### 用 Superpowers

当你说：

- “给这个功能加一个能力”
- “帮我实现一个新需求”
- “这个现有页面/接口/模块要扩展”

并且项目没有 OpenSpec 时，使用：

```text
superpowers:brainstorming
```

### 用 OpenSpec

当你说：

- “这个项目用 OpenSpec”
- “创建一个 change”
- “按 OpenSpec 新增这个功能”
- “先写 proposal/spec/tasks”

或者项目里已经有 `openspec/` 目录时，使用：

```text
/opsx:propose <change-name>
```

扩展工作流（需 profile + `openspec update`）也可使用：

```text
/opsx:new <change-name>
```

## 最实用的提示词

### Superpowers 项目

```text
我要给现有功能增加一个新功能，请先用 superpowers:brainstorming 理解需求，不要直接写代码。
```

### OpenSpec 项目

```text
这个项目使用 OpenSpec。请为这个需求创建一个 OpenSpec change，先写 proposal、delta specs、design 和 tasks，不要直接实现。
```

### OpenSpec 实现阶段

```text
请按 OpenSpec 的 tasks.md 开始实现这个 change，并用 TDD 和 verification-before-completion 做验证。
```

## 推荐默认策略

1. 先检查项目是否存在 `openspec/`。
2. 如果存在，走 OpenSpec change 主流程，Superpowers 作澄清、TDD、验证等执行纪律补充。
3. 如果不存在，可走 Superpowers 流程承载设计与计划；但若任务属于**真实业务需求**（会改变行为、接口、数据、权限、流程或验收标准），须先向用户确认是否初始化 OpenSpec、采用外部需求系统或使用轻量记录（见 [需求治理与文档结构.md](./docs/规范/需求治理与文档结构.md)），不得默认自建自定义需求目录。
4. 实现前必须有设计或任务清单，并满足 `AGENTS.md` 速查中的需求治理底线。
5. 完成前必须运行验证，并说明验证结果和剩余风险。

