---
name: requirement-change
description: 已有需求发生变化时使用——口径调整、范围细化、验收变化、方案调整。编排完整流程：定位原 change、确认归属与状态、判断继续还是新建、判定变化等级、$brainstorming 澄清新口径、更新正文、追加变化记录（不覆盖历史）、重做影响分析、同步验收、实现归档。用户说"需求变了""xxx 要从 A 改成 B""原来的方案要调整"时启用。全新需求不用本 skill，用 new-feature-development。
disable-model-invocation: true
---

# 需求变更流程

按下面 10 步**逐步执行**：为每一步建一条 todo，确认产出后再进下一步。完整说明、命令示例与变化记录模板见同目录 [`workflow.md`](./workflow.md)；三种命令写法、占位符与工具 fallback 见项目根目录 `流程公共约定.md`。

## 核心原则

**不急着新建——先定位原记录、判断变化等级、把正文更新为当前口径、追加变化记录（不覆盖历史），再改代码。**

## 前置约束

- 强制底线见项目根目录 `AGENTS.md`，本 skill 不放宽任何底线。
- **使用依赖**：跑完整流程需 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**；安装本 skill 无此前提。OpenSpec 承载 change 与 `/opsx:*`，CodeGraph 用于影响分析，Superpowers 用于澄清 / 拆任务 / TDD / 验证。未装齐时见 `流程公共约定.md` fallback。
- **单一事实来源**：OpenSpec 的 change 目录；未启用 OpenSpec 时用对应 issue / PR / 外部需求记录。
- 变化记录只能**追加**，绝不覆盖历史口径——这是变更可追溯的关键。

## 步骤清单

1. **定位原记录**——`openspec list` / `openspec show <change-name>` 找到对应的原 change 或外部需求记录，别上来就新建。产出：原记录位置 + 当前有效口径。
2. **确认归属与状态**——确认是"已有需求的变化"而非新需求 / 缺陷 / 随口讨论；确认原记录状态（进行中 / 已完成未归档 / 已归档）允许怎么推进。
3. **决定继续还是新建**——目标没变、同一能力 → 原 change 上改；目标变成另一件事、影响完全不同模块、需独立发布回滚 → `/opsx:propose <new-change>` 并注明与旧 change 的关系。
4. **判断变化等级**——小（文案微调）/ 中等（规则调整）/ 重大（协议、权限模型变化）。只要影响操作路径、权限可见性、状态流转、验收标准、用户预期、数据提交意图或合规含义，就按中等/重大处理。
5. **澄清新口径**（中等/重大必做）——`$brainstorming` 理清：原口径 → 新口径的差异、影响范围、验收怎么跟着变。
6. **更新正文为当前口径**——按变化类型决定改哪些文件（补充说明→proposal；范围变化→proposal+spec；验收调整→spec+tasks；方案调整→design+tasks，可能含 spec），别只改 proposal 漏了 spec / tasks。
7. **追加变化记录**——按 `workflow.md` 文末模板追加一条记录（时间、来源、变化类型、原口径、新口径、影响范围、验收变化、实现影响、验证调整、剩余风险），保留原口径。
8. **重做影响分析**（中等/重大必做）——口径变了，受影响范围也可能变；CodeGraph 重新评估并把结论写进 design / tasks。
9. **同步 tasks 与验收**——更新任务、验证方式、回归范围，**不能拿旧测试覆盖新验收**；任务结构变化大时配合 `$writing-plans` 重拆，但落进 change 的 `tasks.md`。
10. **实现、验证、归档**——正文 / 记录 / 任务 / 验收全部同步后再实现；`/opsx:apply` → `openspec validate` + 项目 test/build/lint（扩展工作流再加 `/opsx:verify`）→ `/opsx:sync` → `/opsx:archive`。实现途中又冒出新变化，回到第 4 步重新判断。

## 禁止

- 跳过定位原记录直接新建 change。
- 覆盖或删除历史口径（变化记录只能追加）。
- 正文 / 记录 / 任务 / 验收未同步就改代码，或把途中冒出的新变化直接吸进代码。
- 拿旧测试覆盖新验收。

## 边界

- 全新需求（不是已有需求的变化）→ 用 `new-feature-development` skill。
- 缺陷修复（不改变原设计口径）→ 不走本流程，按 `AGENTS.md` 验证要求修复即可。
- 未启用 OpenSpec → 流程不变，载体换成 issue / PR / 外部需求记录，见 `workflow.md` 文末「未启用 OpenSpec 时」。
