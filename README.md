# Vibe Coding Rule

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-maxlongint%2FVibe--Coding--Rule-blue?logo=github)](https://github.com/maxlongint/Vibe-Coding-Rule)

面向 AI 辅助开发的通用协作规范。**核心规范一个文件、一页纸，复制即用。**

跨项目、前后端通用，不依赖任何特定 AI 工具或插件。

**English:** [`README.en.md`](./README.en.md) · **快速选型:** [`快速开始.md`](./快速开始.md) · **版本记录:** [`CHANGELOG.md`](./CHANGELOG.md)

## 用法

### 业务项目目录（推荐结构）

```text
your-project/
├── AGENTS.md                 # 从本仓库复制：AI 协作通用底线
├── docs/                     # 项目规范（长期约定）；索引用 docs/README.md
│   └── README.md             # 可从本仓库 docs/README.md 复制
├── .agents/                  # 项目 skill（随仓库共享）
│   ├── README.md             # skill 目录索引；可从本仓库 .agents/README.md 复制
│   └── skills/<name>/SKILL.md
└── openspec/                 # 仅启用 OpenSpec 时：进行中需求变更
    └── changes/<change-name>/
```

`docs/` 放稳定规范；`openspec/changes/` 放进行中需求（未启用 OpenSpec 则用 issue/PR）。详见 [`docs/README.md`](./docs/README.md)。

### 初始化步骤

1. 复制 [`AGENTS.md`](./AGENTS.md) 到业务项目根目录（若已有，先 diff 再合并）。
2. 创建 `docs/`，复制 [`docs/README.md`](./docs/README.md) 作目录说明与索引。
3. 创建 `.agents/`，复制 [`.agents/README.md`](./.agents/README.md) 作 skill 索引。
4. 不确定流程深度？看 [`快速开始.md`](./快速开始.md) 三种场景；启用 OpenSpec 再按 [`新需求开发.md`](./新需求开发.md) 操作。

## 它管什么

[`AGENTS.md`](./AGENTS.md) 用六条底线约束 AI 的协作行为：

1. 不伪造成功——真实失败要显式暴露
2. 完成前必须验证并说明结果
3. 改行为/接口/数据/权限前先有可追溯记录
4. 不碰密钥与敏感信息
5. 未经明确要求不 commit / push / deploy
6. 拿不准就停下来问

## 它不管什么

技术栈、框架、命令、UI/API/业务约定等**项目规范文档**，统一放在项目 `docs/` 下；**项目 skill** 放在 `.agents/skills/` 下，并在 `.agents/README.md` 维护技能包目录（详见 `AGENTS.md` 开头）。都不写进 `AGENTS.md`——这样这份规范才能保持跨项目通用、不膨胀。

## 推荐工具链（可选）

只用 `AGENTS.md` 已经够用。需要更完整的需求治理与执行纪律时，推荐组合 **OpenSpec（主）+ CodeGraph + Superpowers（辅）**：OpenSpec 管需求与验收，CodeGraph 管影响分析，Superpowers 管执行方法。三者都不替代 `AGENTS.md` 底线，未安装时自动 fallback 到 issue/PR 与人工分析。

开发流程见：[`新需求开发.md`](./新需求开发.md) · [`需求变更.md`](./需求变更.md) · 共用约定 [`流程公共约定.md`](./流程公共约定.md)。

### 通过 AI 安装

在你的**业务项目**里把下面的提示词发给 AI，让它参考各工具官方说明完成安装（各系统的具体命令交给 AI 处理，这里不展开）。

**OpenSpec**

```text
请为当前业务项目安装 OpenSpec，不要在规则仓库里初始化。
参考 https://github.com/Fission-AI/OpenSpec 官方 README，优先装为项目依赖，不要全局安装。
执行安装命令前，若项目存在 .node-version，按 AGENTS.md 切换到对应 Node 版本。
需要写入 package.json 或修改项目目录前先向我申请权限。
完成后验证版本，并在项目根目录初始化 OpenSpec。
```

**CodeGraph**

```text
请为当前业务项目安装 CodeGraph，不要在规则仓库里初始化。
参考 https://github.com/colbymchenry/codegraph 官方 README，MCP 配置优先项目级（local），不要写全局。
需要网络下载、改配置或写入项目目录前先向我申请权限。
完成后验证版本，并在项目根目录完成初始化。
```

**Superpowers**

```text
请从 https://github.com/obra/superpowers 为当前 AI 工具安装 Superpowers。
先查看对应当前工具的官方安装说明。
需要克隆仓库、创建链接、修改用户目录或安装插件前先向我申请权限。
完成后验证技能目录可见，并提醒我重启 AI 工具。
```

## 许可证

[MIT](./LICENSE)。可自由复制到商业或非商业项目，保留版权声明即可。
