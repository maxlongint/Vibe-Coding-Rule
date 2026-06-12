# 项目 Skill 目录

本文件是 `.agents/skills/` 下所有 skill 技能包的索引。新增、重命名或移除 skill 时，请同步更新下表。

以下两个流程 skill **使用**完整流程时需 OpenSpec（主）、CodeGraph、Superpowers；**安装 skill 本身无此前提**，未装齐三项工具时可按 [`流程公共约定.md`](../流程公共约定.md) fallback。说明与安装提示词见 [`README.md`](../README.md#项目流程-skill可选)。

| Skill | 路径 | 用途 |
| --- | --- | --- |
| `new-feature-development` | `.agents/skills/new-feature-development/` | 从零开发新需求（会改变外部可观察行为）的完整流程：建 change → 澄清 → proposal → 影响分析 → design/tasks → 实现 → 验证 → 归档。**仅手动调用** |
| `requirement-change` | `.agents/skills/requirement-change/` | 已有需求口径变化的完整流程：定位原记录 → 判等级 → 澄清 → 更新正文 → 追加变化记录（不覆盖历史）→ 重做影响分析 → 同步验收 → 实现归档。**仅手动调用** |

## 目录结构

```text
.agents/
├── README.md              # 本文件：skill 技能包目录索引
└── skills/
    └── <skill-name>/
        ├── SKILL.md       # 技能入口（必需）
        └── workflow.md    # 可选：流程正文、参考文档或脚本
```

## 安装第三方 skill

通过 [skills CLI](https://skills.sh/) 安装到本项目的 skill，默认也落在 `.agents/skills/`（与自建 skill 同目录）。安装后请更新上表索引。

```text
npx skills add <owner/repo@skill-name>   # 项目级，装到 ./.agents/skills/
npx skills add <owner/repo@skill-name> -g   # 个人全局，装到用户目录，不写入本仓库
```

**与 Superpowers 的区别**：Superpowers 是用户级执行方法库，按官方说明安装到 AI 工具用户目录；本项目 `.agents/skills/` 只承载**随仓库共享**的项目 skill。
