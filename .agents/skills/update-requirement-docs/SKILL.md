---
name: update-requirement-docs
description: Use only when the user explicitly asks to use this skill for updating existing requirement documentation, OpenSpec changes, acceptance criteria, scope, design notes, or task records.
---

# 需求文档更新工作流

## 核心原则

仅在用户明确点名使用本技能时启用。用户只是讨论需求、询问规则、补充想法或临时描述新口径时，不要自动调用本技能；先按当前项目 `AGENTS.md`、OpenSpec 和用户本轮指令判断。

需求正文应更新为当前有效口径，需求变化记录应保留历史轨迹。不要用新说法直接覆盖旧变化过程，也不要只写变更记录却忘记同步 `proposal.md`、`design.md`、`tasks.md` 或 `spec.md`。

## 显式调用方式

接受类似下面的明确调用：

```text
请使用 $update-requirement-docs 更新这个已有需求。
请调用 update-requirement-docs，把这个新口径同步到 OpenSpec change。
按需求文档更新 skill 处理 openspec/changes/<change-name>/。
```

如果用户没有明确调用本技能，只给出需求变化内容，应说明可以使用本技能来系统更新文档，并等待用户确认或按项目基础规则处理。

## 执行顺序

1. 读取并遵守当前项目 `AGENTS.md`，确认需求治理、验证、文档目录和权限边界。
2. 定位已有记录载体：优先找用户指定的 `openspec/changes/<change-name>/`；如用户指定外部需求系统，记录外部链接或编号；如没有明确载体，先向用户确认。
3. 判断变更性质：原需求补充、原需求变化、范围新增、范围缩减、验收调整、缺陷修复或轻量调整。
4. 判断是否继续原 change：同一目标、同一能力、同一发布边界时继续更新原记录；目标变成新能力、影响模块明显不同、需要独立评审发布或原需求已归档时，先建议新建 change 并等待确认。
5. 追加需求变化记录，写清时间、来源、原口径、新口径、变化原因、影响范围、验收变化、已同步文件和剩余风险。
6. 同步更新当前有效文档正文：
   - `proposal.md`：目标、范围、非目标、来源和取舍原因。
   - `design.md`：方案影响、边界、兼容策略、风险和回滚。
   - `tasks.md`：实现任务、验证任务、状态和未验证项。
   - `specs/<domain>/spec.md`：外部可观察行为和验收标准。
7. 如变更涉及代码、接口、数据、权限、流程或验收标准，使用 CodeGraph 或人工搜索记录影响分析；CodeGraph 不可用时说明限制。
8. 完成前重新读取被修改的中文文档，确认无乱码、替换字符、问号占位或明显断句；如影响代码，再运行相关验证。
9. 交付时列出修改文件、验证结果、未验证项和剩余风险。

## 变化记录模板

在原 change 的合适位置追加记录；若项目已有固定位置，遵循项目约定。

```text
## 需求变化记录

### YYYY-MM-DD

- 来源：用户确认 / 评审结论 / 外部需求编号
- 类型：原需求补充 / 原需求变化 / 范围新增 / 范围缩减 / 验收调整 / 缺陷修复
- 原口径：
  - ...
- 新口径：
  - ...
- 变化原因：
  - ...
- 影响范围：
  - ...
- 验收变化：
  - ...
- 已同步更新：
  - proposal.md
  - design.md
  - tasks.md
  - specs/<domain>/spec.md
- 剩余风险：
  - ...
```

## 常见错误

- 只更新 `tasks.md`，没有同步 `spec.md` 的验收标准。
- 只把正文改成新口径，没有记录旧口径如何变化。
- 把新能力塞进已完成或不相关的 change，导致发布、评审和回滚边界混乱。
- 用“补充说明”掩盖实际的流程、权限、数据或接口变化。
- 文档改动后没有重新读取中文内容，留下乱码或复制异常。
