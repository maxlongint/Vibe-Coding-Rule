# 项目 Skill 目录

本文件是 `.agents/skills/` 下所有 skill 技能包的索引。新增、重命名或移除 skill 时，请同步更新下表。

本模板默认不登记项目级 skill；需要团队自定义或安装第三方 skill 时，再放入 `.agents/skills/` 并更新下表。

| Skill | 路径 | 用途 |
| --- | --- | --- |
| （暂无） | - | 当前索引暂无项目级 skill |

## 目录结构

```text
.agents/
├── README.md              # 本文件：skill 技能包目录索引
└── skills/
    └── <skill-name>/
        ├── SKILL.md       # 技能入口（必需）
        └── supporting.*   # 可选：参考文档、脚本或模板
```

## 安装第三方 skill

通过 [skills CLI](https://skills.sh/) 安装到本项目的 skill，默认也落在 `.agents/skills/`（与自建 skill 同目录）。安装后请更新上表索引。

```text
npx skills add <owner/repo@skill-name>   # 项目级，装到 ./.agents/skills/
npx skills add <owner/repo@skill-name> -g   # 个人全局，装到用户目录，不写入本仓库
```

**与 Superpowers 的区别**：Superpowers 是用户级执行方法库，按官方说明安装到 AI 工具用户目录；本项目 `.agents/skills/` 只承载**随仓库共享**的项目 skill。
