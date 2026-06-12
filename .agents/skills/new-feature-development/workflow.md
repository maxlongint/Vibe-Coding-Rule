# 新需求开发（OpenSpec 主 · Superpowers 辅）

> 本文是 skill `new-feature-development` 的完整流程正文（唯一来源），由 [`SKILL.md`](./SKILL.md) 编排触发。强制底线见 [`AGENTS.md`](../../../AGENTS.md)。本文讲开发人员怎么操作、执行什么命令、产出什么。
>
> 分工：`AGENTS.md` 定底线 · OpenSpec 管需求/规格/任务/验收 · CodeGraph 管影响分析 · Superpowers 管执行纪律（澄清、TDD、验证）。
>
> **文档只有一处事实来源——OpenSpec 的 change 目录。** 三种命令写法、占位符、TDD 定位与工具 fallback 见 [`流程公共约定.md`](../../../流程公共约定.md)。

## 操作步骤

### 第 1 步　判断要不要建 change

判断标准就一句：**这次改动会不会改变"外部能观察到的行为"**——会，就建 change；不会，通常不必。"外部可观察"指接口、数据结构、权限、流程、状态流转、验收标准、用户对结果的预期这些别人能感知到的东西。

**要建 change 的场景：**

- 新增能力：接 SSO 单点登录、加一个导出报表接口。
- 改规则：密码最短长度从 6 位提到 12 位、订单状态新增"已退款"。
- 改契约：某 API 返回结构变了、分页默认条数从 20 改成 50。
- 改权限：某功能从"仅管理员可见"放开到"所有登录用户"。

**不必建 change 的场景：**

- 改错别字、调整提示文案（且不改变操作含义）。
- 内部变量/函数重命名、补注释、纯代码整理。
- 修一个不改变对外行为的 bug（比如修空指针，但输入输出和原设计一致）。

拿不准时，按"会改变外部行为"从严处理，先建 change。这一步只是判断，不执行任何命令。

### 第 2 步　澄清需求

需求已经很清楚可以跳过；只要目标、范围、验收标准有一处含糊，就先让 AI 帮你问清楚，别急着写代码。在对话框里触发澄清技能：

```text
$brainstorming
帮我澄清这个需求：目标是什么、范围到哪、哪些明确不做、验收标准怎么定。一次问一个问题，先不要写代码。
```

> 这里只把 `$brainstorming` 当澄清对话用——不要它写设计稿、提交，也不要转去 `$writing-plans`；澄清结论留到第 3 步写进 `proposal` / `specs`。（change 目录此时还没创建，所以结论先留在对话里，第 3 步再落地。）
>
> 不必在提示词里叮嘱"别写独立设计稿"——`AGENTS.md` 文末已约束 Superpowers 产出回填到主需求载体，自动生效。

**产出**：一份澄清结论——目标 / 非目标 / 验收标准 / 待决问题，作为下一步写 proposal 的输入（不落地成独立文档）。

### 第 3 步　Proposal——定「做什么」

让 AI 创建 change 并起草 proposal 和 specs，你来审核目标、范围、验收标准对不对。在对话框里发：

```text
/opsx:propose add-sso-login
```

> 扩展工作流用 `/opsx:new add-sso-login`；都不可用时直接说：「这个项目用 OpenSpec，请在 openspec/changes/add-sso-login/ 下创建 change，先写 proposal 和 specs，不要实现。」

**产出**：

- `openspec/changes/add-sso-login/proposal.md`——背景、目标、非目标、用户影响、验收标准、风险。
- `openspec/changes/add-sso-login/specs/<domain>/spec.md`——外部可观察行为和验收场景（写"系统应该表现成什么样"，不写实现细节）。

### 第 4 步　影响分析——先搞清楚「会动到哪」

定方案前先把范围摸清楚，**别跳过这步直接拍方案**。让 AI 用 CodeGraph 把调用链、依赖、受影响范围理清楚：

```text
codegraph_explore "sso login session 相关功能"   # 先理解功能区域
codegraph_callers <符号>     # 谁调用了它
codegraph_callees <符号>     # 它又调用了谁
codegraph_impact <符号>      # 改它会波及哪里
```

> 没装 CodeGraph：让 AI 人工通读相关源码与测试，并明确记下哪些范围没覆盖到。

**产出**：影响分析结论——受影响模块、依赖关系、回归范围、未覆盖项。这份结论是下一步写 design / tasks 的依据。

### 第 5 步　建 design 和 tasks——再定「怎么做」

拿着上一步的影响分析结论，让 AI 起草技术方案和任务清单。下面两条**二选一**，作用一样（都把 design + tasks 补全），区别只是节奏：

```text
/opsx:ff add-sso-login        # 快进：一口气补全 design + tasks，适合需求清楚、想快点过
/opsx:continue add-sso-login  # 分步：一次推进一个阶段、停下来给你审核，适合复杂需求逐步把关
```

> 扩展命令不可用：直接让 AI 起草 `design.md` 和 `tasks.md`，可配合 `$writing-plans` 拆任务——但任务要落进 OpenSpec 的 `tasks.md`，不要另存独立计划文件。

**产出**：

- `design.md`——技术方案、数据流、安全策略、回滚方案。
- `tasks.md`——可勾选的任务清单，每项带验证方式和完成状态。
- 把第 4 步的影响分析结论（受影响模块、回归范围、未覆盖项）落进这两份里。

### 第 6 步　实现

让 AI 按 `tasks.md` 逐项推进，每完成一项就更新状态，你逐项审核。不在生产链路用 mock、默认值、空对象伪造成功。

```text
/opsx:apply add-sso-login
```

启用 Superpowers 时，**推荐**用 TDD 技能作为实现节奏（非 `AGENTS.md` 强制底线；底线仍是完成前验证并说明结果）：

```text
$test-driven-development
请按 tasks.md 实现 add-sso-login，每个任务先写失败测试，跑红，再写实现跑绿，最后我审核。
```

未启用 Superpowers 时，按项目惯例实现，并满足 `AGENTS.md` 验证要求。

**产出**：代码 + `tasks.md` 里每项的完成状态、验证结果、剩余风险。

### 第 7 步　验证

跑校验和测试并确认真实结果，不把"没验证"说成"已通过"；验不了要写清原因和剩余风险。

```text
openspec validate add-sso-login   # 终端：校验 change 文档结构是否完整合法
/opsx:verify add-sso-login        # 扩展工作流；不可用就省略这条
```

再加上项目自己的测试/构建/lint，例如 `npm test`、`pytest`、`make test`，按项目实际命令来。最后用验证技能核对证据：

```text
$verification-before-completion
帮我核对 add-sso-login 的验证证据：实际跑了哪些命令、结果如何、还有什么没覆盖。
```

**产出**：验证证据——实际执行的命令、结果、未覆盖项、剩余风险。

### 第 8 步　归档

验收通过后把 delta spec 合并进主 spec 并归档，并标明状态。

```text
/opsx:sync add-sso-login      # 把 change 里的 spec 改动合并进主 spec（按需）
/opsx:archive add-sso-login   # 归档；终端等价命令：openspec archive add-sso-login
```

**产出**：已归档的 change + 状态标记（`进行中` / `已完成` / `已暂停` / `已废弃` / `已替代`）。

## 产出内容总览

```text
openspec/changes/add-sso-login/
  proposal.md            # 第 3 步：背景、目标、非目标、用户影响、验收标准、风险
  specs/<domain>/spec.md # 第 3 步：外部可观察行为与验收场景（不写实现细节）
  design.md              # 第 5 步：技术方案、数据流、安全策略、回滚
  tasks.md               # 第 5 步：可勾选任务 + 验证方式 + 完成状态
```

这些文档都由 AI 起草、你审核确认，必要时手动微调——`openspec` CLI 只管结构校验和归档，不生成正文。

## 未启用 OpenSpec 时

把 change 换成 issue、任务卡、PR 描述或一份轻量记录，承载同样的产出：需求与验收、影响分析、任务与验证。各工具 fallback 见 [`流程公共约定.md`](../../../流程公共约定.md)。`AGENTS.md` 底线不打折。

## 相关流程

- 已有需求的口径变化（不是从零开发）→ [`requirement-change`](../requirement-change/workflow.md)。
