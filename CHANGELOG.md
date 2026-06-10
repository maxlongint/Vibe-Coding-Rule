# Changelog

本文件记录 **Vibe Coding Rule** 规范包本身的演进。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

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
