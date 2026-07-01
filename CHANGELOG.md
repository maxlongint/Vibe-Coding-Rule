# Changelog

本文件记录 **Vibe Coding Rule** 规范包本身的演进。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

## [1.1.2] - 2026-07-01

### Changed

- `AGENTS.md`：§7 新增「目标变更与非目标不变式」，约束不得通过改变非目标行为间接制造「完成效果」，并补充完成后的不变式核对要求
- `AGENTS.md`、`README.md`：新增版本提示，方便已接入项目判断是否需要同步新版 Vibe Coding Rule
- `AGENTS.md`：新增 §7「实施与改动」（简洁、目标变更与非目标不变式、精准改动）；§1 交叉引用；§7 与 OpenSpec / Superpowers 分工边界（不重复澄清、计划、TDD、change 流程）
- `新需求开发.md`、`流程公共约定.md`、`README.md`：TDD 表述回归 Superpowers 推荐、非 AGENTS 重复
- `README.md`、`快速开始.md`、`CONTRIBUTING.md`、`.agents/README.md`：移除内置项目流程 skill 的安装、索引和手动调用说明；流程入口回到根目录 Markdown 文档
- `README.md`：新增“升级已接入的 Vibe Coding Rule”提示词，指导业务项目同步新版 Vibe Coding Rule 时先 diff、确认方案再写入
- `AGENTS.md`：移除对规则包根目录流程文档的具体文件名引用，避免单文件复制后形成悬空指向
- `快速开始.md`：瘦身为场景选择导航，目录结构、初始化步骤与安装提示词统一回到 `README.md`
- `docs/README.md`：固定 `docs/experience/` 作为经验包 Markdown 目录；调整示例结构，移除默认 `api/` 目录并将接口/API 契约变化归入 OpenSpec change
- `.agents/README.md`：移除对根目录流程文档的相对引用，并调整空索引措辞，避免复制到业务项目后出现断链或语境偏差

### Removed

- `.agents/skills/new-feature-development/`：移除新需求开发流程 skill
- `.agents/skills/requirement-change/`：移除需求变更流程 skill

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

[Unreleased]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.2...HEAD
[1.1.2]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.1...v1.1.2
[1.1.1]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/maxlongint/Vibe-Coding-Rule/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/maxlongint/Vibe-Coding-Rule/releases/tag/v1.0.0
