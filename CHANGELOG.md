# Changelog

本文件记录 **Vibe Coding Rule** 规范包本身的演进。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

### Added

- `.agents/skills/new-feature-development/`：新需求开发流程 skill（`SKILL.md` 编排 + `workflow.md` 正文）
- `.agents/skills/requirement-change/`：需求变更流程 skill（`SKILL.md` 编排 + `workflow.md` 正文）

### Changed

- `AGENTS.md`：新增 §7「实施与改动」（简洁、精准改动）；§1 交叉引用；§7 与 OpenSpec / Superpowers 分工边界（不重复澄清、计划、TDD、change 流程）
- `新需求开发.md`、`.agents/skills/new-feature-development/workflow.md`、`流程公共约定.md`、`README.md`：TDD 表述回归 Superpowers 推荐、非 AGENTS 重复
- `新需求开发.md`、`需求变更.md`：改为薄入口（流程速览 + 指向 skill 的 `workflow.md`），正文唯一来源迁入对应 skill 目录
- `.agents/skills/new-feature-development/`、`.agents/skills/requirement-change/`：设置 `disable-model-invocation: true`，仅手动调用，不自动触发
- `README.md`：新增「项目流程 skill」介绍、使用依赖说明（安装 skill 无此前提）与通过 AI 安装的提示词；初始化提示词改为询问是否安装 skill，不主动装三项工具
- `.agents/README.md`：登记两个流程 skill，目录结构示例补充 `workflow.md`
- `CONTRIBUTING.md`：仓库结构表同步 skill 化调整

## [1.1.1] - 2026-06-10

### Removed

- `README.en.md`：入口统一为 `README.md`

### Changed

- `README.md`、`CONTRIBUTING.md`：移除英文 README 引用

## [1.1.0] - 2026-06-10

### Added

- `快速开始.md`：业务项目目录总览、初始化清单、三种落地场景
- `流程公共约定.md`：OpenSpec / Superpowers / CodeGraph 共用约定
- `docs/README.md`：业务项目 docs 目录模板，明确与 OpenSpec 的边界
- `.agents/README.md`：skill 目录索引模板，含 `npx skills add` 说明
- `README.en.md`、`CHANGELOG.md`、`SECURITY.md`、`CODE_OF_CONDUCT.md`
- `.github/pull_request_template.md`

### Changed

- `AGENTS.md`：项目路径约定（`docs/` / `.agents/skills/` / `.agents/README.md`）、`docs/` 与需求变更分工、AI 不得擅自修改本文件
- `README.md`：业务项目推荐目录结构与初始化步骤
- `新需求开发.md`、`需求变更.md`：引用公共约定，统一 TDD 为推荐方法
- 精简仓库结构，移除冗余平行文档

## [1.0.0] - 2026-06-10

### Added

- `AGENTS.md`：六条底线与六章展开（工程、安全、验证、需求可追溯、注释、提交）
- `README.md`：用法、边界、可选工具链安装提示词
- `新需求开发.md`、`需求变更.md`：OpenSpec 主流程指南
- `CONTRIBUTING.md`、`LICENSE`（MIT）

[1.1.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/releases/tag/v1.0.0
