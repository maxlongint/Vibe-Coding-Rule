---
name: new-feature-development
description: 从零开发新需求时使用——凡是会改变外部可观察行为的改动（新增能力、改业务规则、改接口契约、改权限）。编排完整流程：判断建 change、$brainstorming 澄清、OpenSpec proposal/specs、CodeGraph 影响分析、design/tasks、实现、验证、归档。用户提出做新功能、新接口、加能力，或要求按新需求流程开发时启用。已有需求的口径变化不用本 skill，用 requirement-change。
disable-model-invocation: true
---

# 新需求开发流程

按下面 8 步**逐步执行**：为每一步建一条 todo，确认产出后再进下一步。完整说明、命令示例与产出模板见同目录 [`workflow.md`](./workflow.md)；三种命令写法、占位符与工具 fallback 见项目根目录 `流程公共约定.md`。

## 前置约束

- 强制底线见项目根目录 `AGENTS.md`，本 skill 不放宽任何底线。
- **使用依赖**：跑完整流程需 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**；安装本 skill 无此前提。OpenSpec 承载 change 与 `/opsx:*`，CodeGraph 用于影响分析，Superpowers 用于澄清 / 拆任务 / TDD / 验证。未装齐时见 `流程公共约定.md` fallback。
- **单一事实来源**：`openspec/changes/<change-name>/`；未启用 OpenSpec 时用 issue / PR / 任务卡承载同样产出。
- `$brainstorming`、`$writing-plans` 等 Superpowers 技能的产出一律回填 change 目录（或所选需求载体），不另存独立文档。

## 步骤清单

1. **判断要不要建 change**——改动会改变外部可观察行为（接口、数据结构、权限、流程、状态流转、验收标准、用户预期）就建；拿不准从严处理。产出：是/否及理由。
2. **澄清需求**——目标、范围、验收标准有一处含糊就用 `$brainstorming` 一次问一个问题，先不写代码。产出：目标 / 非目标 / 验收标准 / 待决问题（留在对话，下一步落地）。
3. **Proposal**——`/opsx:propose <change-name>` 起草 `proposal.md` + `specs/`，交用户审核目标、范围、验收。
4. **影响分析**——CodeGraph（`codegraph_explore` / `codegraph_callers` / `codegraph_callees` / `codegraph_impact`）摸清受影响模块与回归范围；没装就人工通读源码与测试并记录未覆盖项。
5. **design + tasks**——`/opsx:ff <change-name>`（快进）或 `/opsx:continue <change-name>`（分步审核），把第 4 步结论落进 `design.md` / `tasks.md`。
6. **实现**——`/opsx:apply <change-name>` 按 `tasks.md` 逐项推进、逐项更新状态；推荐配合 `$test-driven-development`。不在生产链路用 mock / 默认值 / 空对象伪造成功。
7. **验证**——`openspec validate <change-name>`（扩展工作流再加 `/opsx:verify <change-name>`）+ 项目自身 test / build / lint，再用 `$verification-before-completion` 核对证据。验不了要写清原因和剩余风险。
8. **归档**——验收通过后 `/opsx:sync <change-name>` + `/opsx:archive <change-name>`，标明状态。

## 禁止

- 澄清 / proposal 未经用户确认就写实现代码。
- 跳过影响分析直接定技术方案。
- 把澄清、设计、计划另存为 `docs/` 或独立文件（必须落 change 目录或所选需求载体）。
- 把"没验证"说成"已通过"。

## 边界

- 已有需求口径变化 → 用 `requirement-change` skill。
- 纯文案、内部重命名、不改变外部行为的 bug 修复 → 不必建 change，按 `AGENTS.md` 最小验证即可。
- 未启用 OpenSpec → 流程不变，载体换成 issue / PR / 任务卡，见 `workflow.md` 文末「未启用 OpenSpec 时」。
