# SSO 单点登录复杂需求执行步骤示例

本文档是一个教学示例，用来说明：在大型既有项目中新增 SSO 单点登录能力时，AI 和开发者应如何按当前 `AGENTS.md` 使用 `AGENTS.md + CodeGraph + OpenSpec + Superpowers` 协作模型推进需求。

SSO 属于重大真实业务需求。它通常会改变登录入口、认证协议、用户绑定、权限映射、Session 或 token 逻辑、前端路由守卫、配置管理、审计日志、发布策略和验收标准。因此，正式实现前必须先建立可追溯需求记录；默认主载体是 OpenSpec change，而不是自定义 `docs/需求/` 目录。

推荐主流程：

```text
读取 AGENTS.md
  -> 判断任务类型和文档性质
  -> 启用 Superpowers 流程
  -> 创建或更新 OpenSpec change
  -> 使用 CodeGraph 做影响分析
  -> 完善 proposal、design、tasks、spec
  -> 先测试后实现
  -> 分阶段验证
  -> 评审、发布、回滚
  -> 完成、暂停、废弃或替代时归档
```

## 0. 示例定位

### 文档性质

本文是用户明确要求保留的教学示例文件，用于展示复杂需求的执行方法。它不是某个真实项目的 SSO 需求记录，也不替代 OpenSpec change。

如果在真实项目中执行 SSO，需求记录应进入：

```text
openspec/changes/add-sso-login/
  proposal.md
  design.md
  tasks.md
  specs/
    auth/
      spec.md
```

如果项目明确使用外部需求系统，应在 OpenSpec change、任务记录、PR 描述、提交说明或交付说明中保留外部链接、编号或用户确认来源。

### 需求等级

```text
任务类型：重大需求
变化范围：认证、用户、权限、会话、配置、前端登录流程、审计和发布回滚
OpenSpec：必须
CodeGraph：必须
Superpowers：brainstorming、writing-plans、test-driven-development、requesting-code-review、verification-before-completion
是否可直接编码：否
是否需要验证：必须
是否允许未经授权提交或发布：否
```

## 1. 读取 AGENTS.md 并判断任务

### 目标

先确认当前项目规则、权限边界、需求治理和验证底线，再决定下一步。AI 不应把 SSO 当成普通代码修改或纯技术实验直接实现。

### 给 AI 的示例指令

```text
请先读取当前项目 AGENTS.md。
本次任务是新增 SSO 单点登录能力，请判断任务类型、需求等级、记录载体、需要使用的 Superpowers、是否必须使用 CodeGraph，以及实现前需要哪些确认。
暂时不要修改代码。
```

### 判断结果示例

```text
任务类型：重大真实业务需求
需求变化：认证协议、登录策略、权限映射、用户绑定和验收标准变化
记录载体：openspec/changes/add-sso-login/
CodeGraph：必须用于认证、用户、权限、会话、路由守卫和测试影响分析
Superpowers：先 brainstorming，再 writing-plans；实现阶段使用 TDD；完成前使用 verification-before-completion
提交发布：未经用户明确要求不得 commit、push、merge、deploy
```

### 检查点

-   已读取并遵守 `AGENTS.md`。
-   已确认 SSO 是重大需求，不是小修或纯重构。
-   已确认需要 OpenSpec change。
-   已确认需要 CodeGraph 影响分析。
-   已确认实现前需要方案和验收标准。

## 2. 使用 Superpowers 澄清需求

### 目标

在创建正式规格前，用 `brainstorming` 明确目标、边界、非目标、成功标准和高风险决策。

### 需要澄清的问题

```text
1. 身份源是什么：OIDC、SAML、企业微信、钉钉、Azure AD，还是自建 IdP？
2. 第一阶段是否保留原账号密码登录？
3. 用户绑定规则是什么：外部身份 ID、邮箱、手机号、工号，还是首次登录创建？
4. 权限来源是什么：沿用系统角色、从 IdP 同步，还是做映射表？
5. 是否需要灰度开关？
6. 是否要求真实 IdP 联调，还是先接测试 IdP？
7. 回滚目标是什么：关闭 SSO 入口，还是完全禁用 callback？
```

### 推荐第一阶段方案

```text
保留原账号密码登录
  + 新增 SSO 登录入口
  + 配置开关灰度
  + SSO callback 接入现有 session/token 生成链路
  + 用户绑定规则先走明确唯一字段
  + SSO 异常时可关闭入口并保留原登录兜底
```

这个方案不是最“干净”的最终形态，但适合大型既有项目第一阶段上线，因为它把认证协议变化和登录体系替换拆开，降低发布和回滚风险。

### 检查点

-   已明确目标和非目标。
-   已确认第一阶段是否保留原登录。
-   已确认用户绑定和权限映射方向。
-   已记录需要人工确认的问题。
-   未在需求未澄清前开始编码。

## 3. 创建 OpenSpec Change

### 目标

把 SSO 从聊天意图变成可追溯的需求记录。OpenSpec 负责说明“要做什么、为什么做、做到什么程度算完成”。

如果项目尚未初始化 OpenSpec，应先按 `AGENTS.md` 向用户确认：是初始化 OpenSpec，还是采用外部需求系统记录；确认后再创建对应记录。

### 目录结构

```text
openspec/changes/add-sso-login/
  proposal.md
  design.md
  tasks.md
  specs/
    auth/
      spec.md
```

### 给 AI 的示例指令

```text
请为新增 SSO 单点登录创建 OpenSpec change。

路径使用 openspec/changes/add-sso-login/。
请创建或更新：
1. proposal.md：目标、范围、非目标、用户影响、验收标准、风险。
2. design.md：方案、影响范围、数据流、安全策略、回滚方案。
3. tasks.md：实现任务、验证方式、完成状态。
4. specs/auth/spec.md：外部可观察行为和验收场景。

需求来源记录为：用户确认新增 SSO 单点登录能力。
状态标记为：进行中。
第一阶段默认保留原账号密码登录。
暂时不要写实现代码。
```

### proposal.md 应包含

```text
# Proposal

## 背景
- 企业用户希望通过统一身份源登录系统。

## 目标
- 新增 SSO 登录入口。
- SSO callback 成功后接入现有登录态生成流程。
- SSO 异常时不影响原账号密码登录。

## 非目标
- 第一阶段不移除原账号密码登录。
- 第一阶段不默认把所有用户强制跳转 SSO。
- 第一阶段不做复杂组织架构同步，除非用户另行确认。

## 验收标准
- 未启用 SSO 时，原登录保持可用，登录页不展示 SSO 入口。
- 启用 SSO 时，用户可以从登录页发起 SSO。
- callback 校验失败时拒绝创建登录态。
- 已绑定用户可以成功登录并获得正确权限。
- 禁用用户不能通过 SSO 登录。
- 可以通过配置关闭 SSO 入口并回退原登录。
```

### design.md 应包含

```text
# Design

## 推荐方案
- 保留原登录 + 新增 SSO 入口 + 配置开关灰度。

## 认证流程
1. 用户点击 SSO 登录入口。
2. 系统生成带 state、nonce、redirect_uri 的 IdP 登录地址。
3. IdP 完成认证后回调系统 callback。
4. 系统校验 state、nonce、token 签名、issuer、audience、有效期。
5. 系统按绑定规则定位用户。
6. 系统复用现有 session/token 生成流程。
7. 前端进入原有登录后页面。

## 安全要求
- redirect_uri 必须走白名单。
- state 和 nonce 必须一次性、可过期、不可复用。
- token 校验失败时不得创建登录态。
- 用户绑定字段必须唯一，不允许猜测匹配。
- 错误信息不能泄露敏感内部细节。

## 回滚
- 关闭 SSO_ENABLED。
- 登录页隐藏 SSO 入口。
- callback 拒绝新的 SSO 登录。
- 原账号密码登录保持可用。
```

### specs/auth/spec.md 示例

```text
## ADDED Requirements

### Requirement: SSO 登录入口
系统启用 SSO 后，登录页应展示 SSO 登录入口，并能跳转到受信任身份源。

#### Scenario: SSO 启用时用户发起登录
- GIVEN 系统已启用 SSO
- AND SSO 配置完整有效
- WHEN 用户点击 SSO 登录入口
- THEN 系统生成第三方身份源登录地址
- AND 登录地址包含有效的 state、nonce 和 redirect_uri

#### Scenario: SSO 未启用时隐藏入口
- GIVEN 系统未启用 SSO
- WHEN 用户打开登录页
- THEN 登录页不展示 SSO 登录入口
- AND 原账号密码登录仍然可用

### Requirement: SSO Callback 安全校验
系统必须在创建登录态前完成 callback 安全校验。

#### Scenario: callback 校验失败
- GIVEN 用户从身份源返回 callback
- WHEN state、nonce、token 签名、issuer、audience 或有效期任一校验失败
- THEN 系统拒绝创建登录态
- AND 用户看到安全的失败提示
- AND 审计日志记录失败事件
```

### tasks.md 示例

```text
# Tasks

- [ ] 1. 使用 CodeGraph 分析现有认证、用户、权限、会话和前端登录链路。
- [ ] 2. 确认 SSO 协议、身份源、用户绑定字段和权限映射规则。
- [ ] 3. 增加 SSO 配置结构和灰度开关。
- [ ] 4. 增加 SSO 登录发起接口。
- [ ] 5. 增加 SSO callback 接口和安全校验。
- [ ] 6. 增加用户绑定或首次创建逻辑。
- [ ] 7. 复用现有 session/token 生成流程。
- [ ] 8. 登录页增加 SSO 入口和启停逻辑。
- [ ] 9. 前端处理 callback 成功、失败和等待状态。
- [ ] 10. 增加后端正向和反向测试。
- [ ] 11. 增加前端流程测试。
- [ ] 12. 准备灰度、监控、发布和回滚说明。
- [ ] 13. 完成验证、评审和归档记录。
```

### 检查点

-   OpenSpec change 路径已明确。
-   proposal、design、tasks、spec 都已覆盖。
-   spec 描述的是外部可观察行为和验收标准，不只是内部实现。
-   tasks 包含实现步骤、验证方式和状态。

## 4. 使用 CodeGraph 做影响分析

### 目标

CodeGraph 负责回答“代码现在真实是什么样、改动影响哪里”。SSO 这种跨模块需求，不能只凭文件名猜测影响范围。

### 给 AI 的示例指令

```text
请使用 CodeGraph 分析当前项目中新增 SSO 登录的影响范围。

优先使用 codegraph_explore 理解：
- 登录入口
- 认证服务
- 用户服务
- 权限和角色服务
- session/token 生成逻辑
- 前端登录页
- 路由守卫
- 配置加载逻辑
- 审计日志
- 认证相关测试

然后使用 callers/callees/impact 分析关键函数和共享模块。
请把影响范围、关键调用链、需要修改的模块、需要回归的测试和剩余风险写入 openspec/changes/add-sso-login/design.md 和 tasks.md。
```

### CodeGraph 使用方式

如果平台提供 CodeGraph MCP 工具，优先使用：

```text
codegraph_explore: "auth login session token user role permission router guard"
codegraph_callers: "createSession"
codegraph_callees: "login"
codegraph_impact: "createSession"
```

如果 CodeGraph 不可用，应记录人工影响分析方式：

```text
影响分析方式：人工搜索
使用工具：rg、测试文件索引、目录结构检查
限制：未配置 CodeGraph，可能遗漏动态路由、运行时注入或间接调用链
剩余风险：需要在评审时重点复查认证和权限相关调用方
```

### 影响范围示例

```text
后端：
- auth 登录入口
- sso provider 配置
- callback controller
- user binding service
- session/token service
- role/permission mapping
- audit log

前端：
- login page
- SSO redirect/callback page
- route guard
- auth store
- error display

测试：
- auth unit tests
- sso callback integration tests
- login page tests
- route guard tests
- permission regression tests
```

### 检查点

-   已识别受影响模块和调用链。
-   已识别直接调用方和被调用方。
-   已识别需要回归的测试。
-   CodeGraph 结论已写入 OpenSpec design 或 tasks。
-   CodeGraph 不可用时已说明人工分析限制和剩余风险。

## 5. 确认方案并更新 OpenSpec

### 目标

根据需求澄清和影响分析确认第一阶段方案，更新设计、任务和验收标准。若出现需求变化，先更新 OpenSpec，再进入实现。

### 方案比较示例

方案 A：保留原登录，并新增 SSO 入口。

-   优点：风险低，便于灰度，原登录可作为兜底。
-   缺点：登录页和认证逻辑会同时存在两种入口。
-   适用：大型既有项目第一阶段。
-   推荐：是。

方案 B：默认跳转 SSO，原登录作为备用。

-   优点：企业内部用户体验更统一。
-   缺点：一旦 SSO 配置错误，登录入口风险更大。
-   适用：SSO 稳定后第二阶段。
-   推荐：第一阶段暂不采用。

方案 C：完全替换原登录。

-   优点：登录体系更统一。
-   缺点：上线风险最大，回滚困难。
-   适用：后续成熟阶段。
-   推荐：不建议第一阶段采用。

### 给 AI 的示例指令

```text
请基于 OpenSpec change 和 CodeGraph 影响分析，比较 SSO 接入方案。
推荐第一阶段低风险方案，并更新：
1. openspec/changes/add-sso-login/design.md
2. openspec/changes/add-sso-login/tasks.md
3. openspec/changes/add-sso-login/specs/auth/spec.md

如果发现目标、验收标准或风险变化，请先记录为需求变化，再继续实现。
暂时不要写代码。
```

### 检查点

-   已比较多个方案。
-   已说明推荐方案和不采用其他方案的原因。
-   OpenSpec design、tasks、spec 已同步。
-   未把方案决策只留在聊天记录中。

## 6. 编写实现计划

### 目标

使用 `writing-plans` 把已确认方案拆成可执行、可验证、可回滚的小任务。计划不能只写开发动作，必须包含测试和完成标准。

### 任务模板

```text
任务：
目标：
输入：
修改范围：
测试：
验证命令：
风险：
完成标准：
状态：
```

### 推荐拆分

```text
1. 补齐认证链路影响分析。
2. 增加 SSO 配置和开关。
3. 增加登录发起接口。
4. 增加 callback 接口的失败测试。
5. 实现 callback 安全校验。
6. 增加用户绑定规则和反向测试。
7. 复用现有 session/token 生成。
8. 登录页增加 SSO 入口。
9. callback 页面处理成功、失败、等待状态。
10. 增加审计日志。
11. 准备灰度和回滚说明。
12. 完成全量验证和评审。
```

### 检查点

-   每个任务有明确目标和完成标准。
-   每个任务有验证方式。
-   高风险认证逻辑先有测试。
-   计划没有把多个高风险变更塞进同一步。

## 7. 按 TDD 分阶段实现

### 目标

实现阶段使用 `test-driven-development`。涉及认证、权限、状态流转或外部集成时，应先写或更新测试，确认失败符合预期，再改实现。

### 给 AI 的示例指令

```text
请实现 openspec/changes/add-sso-login/tasks.md 的第 N 个任务。

要求：
1. 先读取 AGENTS.md、OpenSpec change 和 CodeGraph 影响分析。
2. 如果任务涉及认证、权限、状态流转或外部集成，先写或更新测试。
3. 先运行测试，确认失败原因符合目标行为。
4. 再修改实现代码。
5. 再运行相关验证。
6. 更新 tasks.md 中该任务状态、验证结果和剩余风险。
7. 不要修改无关文件。
```

### 实现原则

-   优先复用现有认证、用户、权限和 session 逻辑。
-   不引入新的认证体系，除非 OpenSpec 已明确记录原因和影响。
-   不删除原账号密码登录入口。
-   不用 mock、本地缓存、旧字段或默认值伪造真实登录成功。
-   接口失败、权限失败、字段缺失、契约不完整必须显式失败。
-   用户绑定字段不唯一时必须失败，不能猜测绑定。
-   面向用户的错误信息不能泄露敏感内部细节。
-   关键写操作须幂等，处理期间给出清晰进行中状态；进入下一流程前须等待写入结果，不得提前宣称成功。

### 测试重点

后端：

-   未开启 SSO 时原登录正常。
-   开启 SSO 后可以生成登录跳转地址。
-   state 和 nonce 校验成功。
-   state、nonce 或 token 校验失败时拒绝登录。
-   已绑定用户可以登录。
-   禁用用户不能登录。
-   用户绑定字段冲突时失败。
-   权限映射正确。

前端：

-   登录页根据配置展示或隐藏 SSO 入口。
-   点击 SSO 入口后跳转正确。
-   callback 成功后进入系统。
-   callback 失败后展示安全错误提示。
-   原账号密码登录仍可用。
-   提交或跳转期间有等待状态，且不重复提交。

### 检查点

-   测试已新增或更新。
-   目标测试先失败再通过。
-   原登录相关测试未被破坏。
-   实现范围符合 OpenSpec tasks。
-   tasks.md 记录了状态、验证结果和剩余风险。

## 8. 处理需求变化

### 目标

实现中出现新规则、新范围或验收变化时，先判断变化等级，再更新 OpenSpec 或外部记录，不能直接改代码吸收变化。

### 示例变化

小变化：登录按钮文案从“SSO 登录”改为“企业登录”。

-   更新 tasks.md 或交付说明。
-   重新运行相关前端测试或截图检查。

中等变化：用户绑定规则从邮箱匹配改为工号匹配。

-   更新 design.md。
-   更新 specs/auth/spec.md。
-   更新 tasks.md。
-   重新做必要影响分析。
-   重新验证绑定相关测试。

重大变化：从 OIDC 改为 SAML，或从保留原登录改为默认跳转 SSO。

-   暂停实现。
-   更新 proposal.md、design.md、tasks.md、spec.md。
-   重新使用 CodeGraph 做影响分析。
-   重新走方案评审。
-   重新拆实现计划。
-   必要时判断是否新建 OpenSpec change。

### 检查点

-   变化归属已确认。
-   记录载体已确认。
-   变化等级已判断。
-   OpenSpec、计划、测试和验收标准已同步更新。
-   重新确认后才继续实现。

## 9. 完成前验证

### 目标

使用 `verification-before-completion`。完成前必须有新鲜验证证据，不能用“应该可以”“看起来没问题”替代命令输出和验收结果。

### 给 AI 的示例指令

```text
请根据 openspec/changes/add-sso-login/ 的验收标准和 tasks.md，列出必须运行的验证命令并执行。

要求：
1. Node 项目时按 AGENTS.md 检查 `.node-version` 并使用对应 Node 版本。
2. 运行认证单元测试。
3. 运行 SSO callback 集成测试。
4. 运行前端登录流程测试。
5. 运行构建或 lint。
6. 验证原账号密码登录不受影响。
7. 汇报实际运行的命令、结果、未覆盖原因和剩余风险。
```

### 验证清单

-   OpenSpec 文件结构和内容检查。
-   认证单元测试。
-   SSO callback 集成测试。
-   前端登录流程测试。
-   原账号密码登录回归。
-   SSO 失败场景。
-   权限回归。
-   构建检查。
-   安全配置检查。
-   灰度开关检查。
-   测试 IdP 或真实 IdP 联调。

### 汇报格式

```text
已运行：
- npm run test：通过
- npm run build：通过
- 认证集成测试：通过

未运行：
- 真实企业 IdP 联调，因为测试环境未提供 IdP 配置。

剩余风险：
- 需要在测试环境完成真实 IdP callback 联调。
```

### 检查点

-   验证命令已实际运行。
-   结果明确。
-   未验证项有原因。
-   剩余风险已写入 tasks、评审材料或交付说明。
-   未把未验证结果表述为已通过。

## 10. 评审

### 目标

使用 `requesting-code-review` 或等效评审流程，让评审者确认实现是否符合 OpenSpec，是否安全，是否影响既有登录和权限。

### 给 AI 的示例指令

```text
请基于当前 SSO 变更准备评审材料。

要求：
1. 汇总 OpenSpec change 的目标、非目标和验收标准。
2. 汇总代码变更文件。
3. 汇总 CodeGraph 影响分析结果。
4. 汇总已运行验证命令。
5. 标出安全敏感点。
6. 标出剩余风险和需要人工确认的问题。
7. 不执行 commit、push、merge、deploy 或 release。
```

### 评审重点

-   是否影响原账号密码登录。
-   state、nonce、redirect_uri 是否安全。
-   用户绑定规则是否可能错绑。
-   权限映射是否清晰。
-   错误处理是否可观测。
-   是否有可执行回滚开关。
-   验收项是否全部覆盖。
-   CodeGraph 识别的影响模块是否都处理或解释了。

### 检查点

-   评审材料包含 OpenSpec、CodeGraph 和验证结果。
-   安全敏感点被明确标出。
-   评审问题已记录并处理。
-   未经授权没有执行提交或发布动作。

## 11. 提交、发布和回滚

### 目标

降低上线风险，确保 SSO 异常时可以快速恢复原登录路径，并遵守 `AGENTS.md` 的提交与发布权限。

### 权限规则

未经用户明确要求，AI 不得执行：

-   `git commit`
-   `git push`
-   `git merge`
-   `release`
-   `deploy`
-   任何会改变远端仓库、线上环境或发布状态的操作

如果用户要求提交或发布，AI 必须先汇总：

-   当前变更。
-   关联 OpenSpec change。
-   验证结果。
-   剩余风险。
-   未完成验证原因。

### 发布说明示例

```text
发布目标：
- 灰度开放 SSO 登录入口。
- 保留原账号密码登录。

配置项：
- SSO_ENABLED
- SSO_PROVIDER
- SSO_REDIRECT_URI
- SSO_ALLOWED_REDIRECT_HOSTS

灰度步骤：
1. 测试环境接入。
2. 内部管理员账号试点。
3. 部分部门灰度。
4. 全量开放。

回滚方式：
1. 关闭 SSO_ENABLED。
2. 登录页隐藏 SSO 入口。
3. callback 拒绝新的 SSO 登录请求。
4. 原账号密码登录保持可用。
5. 保留审计日志，便于排查已发生登录事件。
```

### 检查点

-   发布步骤清晰。
-   回滚步骤可执行。
-   配置开关可用。
-   监控指标明确。
-   线上验证责任人明确。
-   未经授权没有执行提交或发布动作。

## 12. 完成和归档

### 目标

需求完成、暂停、废弃或被替代时，在 OpenSpec change 或外部需求系统中记录最终状态、日期、原因、验收结果和剩余风险。

### 归档记录示例

```text
状态：已完成
日期：2026-06-20
原因：SSO 第一阶段已完成并通过验收
最终验收：
- 原账号密码登录可用
- SSO 登录成功和失败场景已覆盖
- 灰度开关和回滚路径已验证
剩余风险：
- 后续如切换为默认 SSO，需要新一轮需求变化评审
```

如果需求暂停、废弃或被替代，也应写明停止原因、剩余风险和后续处理方向。是否移动到归档目录由项目约定决定；如果没有归档目录，保持原目录不动，通过状态标记归档。

### 检查点

-   需求状态已更新。
-   日期和原因已记录。
-   最终验收结果已记录。
-   未完成事项和剩余风险已记录。
-   归档后的需求不再直接追加实现任务；如需扩展，先判断是恢复原 change 还是新建 change。

## 最终完成标准

SSO 需求只有在以下条件都满足后，才能认为可以进入合并、发布或归档：

-   已读取并遵守 `AGENTS.md`。
-   已确认 SSO 是重大真实业务需求。
-   已建立 `openspec/changes/add-sso-login/` 或等效外部需求记录。
-   需求来源、状态、目标、边界、验收标准、变化记录和剩余风险可追溯。
-   已使用 CodeGraph 做影响分析，或记录了人工分析限制和剩余风险。
-   CodeGraph 结论已写入 OpenSpec design、tasks 或交付说明。
-   已使用 Superpowers 完成需求澄清、计划、TDD、评审和完成前验证。
-   OpenSpec proposal、design、tasks、spec 已同步更新。
-   原账号密码登录仍然可用。
-   SSO 成功和失败场景都已覆盖。
-   安全敏感点已评审。
-   发布和回滚方案已准备。
-   已汇报实际运行的验证命令、结果、未验证原因和剩余风险。
-   未经用户明确授权，没有执行提交、推送、合并、发布或部署。
-   完成、暂停、废弃或替代时，OpenSpec change 或外部记录已记录最终状态、日期和原因。

核心原则：`AGENTS.md` 定规则和边界，OpenSpec 定需求和验收，CodeGraph 定代码事实和影响范围，Superpowers 定 AI 执行方法。复杂认证需求必须先记录、再分析、再计划、再实现，最后用验证、评审、灰度和回滚把风险关住。
