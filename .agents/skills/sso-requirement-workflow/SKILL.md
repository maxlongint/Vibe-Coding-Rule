---
name: sso-requirement-workflow
description: Execute or guide SSO single sign-on requirements in Vibe Coding projects. Use when Codex is asked to plan, analyze, document, implement, review, or verify SSO login, enterprise identity, OIDC, SAML, IdP callback, user binding, permission mapping, session/token, route guard, SSO configuration, audit, rollout, rollback, OpenSpec, CodeGraph, or Superpowers workflows.
---

# SSO 重大需求执行工作流

## 核心原则

把 SSO 单点登录视为重大真实业务需求，除非用户明确说明只是只读分析、教学演示或局部文案调整。正式实现前，先建立可追溯需求记录，再做代码影响分析和实现计划。

SSO 常影响认证协议、登录入口、用户绑定、权限映射、Session 或 token、路由守卫、配置、审计、发布和回滚。不要把它当作普通登录按钮或单接口改动直接编码。

## 执行顺序

1. 读取并遵守当前项目 `AGENTS.md`，确认需求治理、验证、提交发布权限和本地环境规则。
2. 判断任务类型：只读分析、教学示例、小修、缺陷修复、中等需求、重大需求或紧急修复。
3. 若是正式 SSO 能力变更，先使用 Superpowers：需求澄清用 `brainstorming`，方案和拆解用 `writing-plans`，实现阶段用 `test-driven-development`，评审用 `requesting-code-review`，完成前用 `verification-before-completion`。遇到故障或异常结果时先用 `systematic-debugging`。
4. 创建或更新 OpenSpec change；若项目尚未初始化 OpenSpec，先按项目规则向用户确认是否初始化 OpenSpec 或采用外部需求系统记录。
5. 使用 CodeGraph 分析现有认证、用户、权限、会话、路由守卫、配置、审计和相关测试。CodeGraph 不可用时，记录人工搜索方式、限制和剩余风险。
6. 将需求来源、状态、目标、非目标、验收标准、方案、影响范围、任务、验证方式和剩余风险写入 OpenSpec 或用户确认的外部记录。
7. 先写或更新关键测试，再实现。认证、权限、状态流转和外部集成不得用 mock、默认值、旧字段或本地缓存伪造真实成功。
8. 完成前运行与改动直接相关的验证，汇报实际命令、结果、未验证原因和剩余风险。

## OpenSpec 最小结构

默认 change 路径：

```text
openspec/changes/add-sso-login/
  proposal.md
  design.md
  tasks.md
  specs/
    auth/
      spec.md
```

`proposal.md` 写背景、目标、非目标、用户影响、验收标准和风险。

`design.md` 写推荐方案、认证流程、数据流、用户绑定、权限映射、安全策略、影响范围、灰度和回滚。

`tasks.md` 写可执行任务、验证方式、状态、未验证项和剩余风险。

`specs/auth/spec.md` 写外部可观察行为和验收场景，不只描述内部实现。

## 默认第一阶段方案

优先推荐低风险接入：

```text
保留原账号密码登录
  + 新增 SSO 登录入口
  + 配置开关灰度
  + SSO callback 接入现有 session/token 生成链路
  + 用户绑定规则先走明确唯一字段
  + SSO 异常时可关闭入口并保留原登录兜底
```

只有在 OpenSpec 或用户确认中明确记录原因、影响和回滚策略后，才考虑默认跳转 SSO 或完全替换原登录。

## 安全与实现边界

- 校验 `state`、`nonce`、token 签名、issuer、audience、有效期和 `redirect_uri` 白名单后，才能创建登录态。
- 用户绑定字段必须唯一；不唯一、缺失或不可验证时必须失败，不得猜测匹配。
- 禁用用户不得通过 SSO 登录。
- SSO callback 成功后复用现有 session/token 生成链路，避免引入平行登录体系。
- 用户侧错误信息避免泄露敏感内部细节，服务端保留可审计失败原因。
- 原账号密码登录是否保留、权限来源、绑定规则、灰度开关和回滚方式必须可追溯。
- 未经用户明确要求，不执行 commit、push、merge、deploy、release。

## 验证重点

覆盖正向和关键反向场景：

- 未启用 SSO 时原登录保持可用，登录页不展示 SSO 入口。
- 启用 SSO 后可生成受信任身份源登录地址。
- callback 校验失败时拒绝创建登录态并记录审计。
- 已绑定用户可登录并获得正确权限。
- 禁用用户、绑定字段冲突、token 过期、issuer/audience 不匹配等场景失败。
- 登录页、callback 页面、路由守卫、auth store 和权限回归通过。
- 灰度开关、关闭入口和回滚路径可执行。

## 参考资料

需要具体模板、示例指令、检查点或完整 SSO 执行步骤时，读取 `references/sso-complex-requirement-example.md`。
