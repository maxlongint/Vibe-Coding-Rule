# 贡献指南

感谢你对 [Vibe Coding Rule](https://github.com/maxlongint/Vibe-Coding-Rule) 的关注。本仓库是一份可搭配 OpenSpec、CodeGraph、Superpowers 的完整 AI 协作规范，欢迎通过 Issue 和 Pull Request 改进。

## 核心原则：保持精简

本仓库的价值在于**精简、复制即用**。提交前请先问自己：这项内容是不是跨项目通用的 AI 协作底线，或业务项目可直接复用的目录与流程模板？

- ✅ 跨项目通用、与具体技术栈/工具无关的协作底线
- ✅ 可直接复制到业务项目的通用目录说明与协作流程模板
- ❌ 某公司内部流程、域名、业务规范
- ❌ 特定框架/技术栈的编码风格大全
- ❌ 真实业务需求或具体项目文档

后两类应放在你自己业务项目的文档里，不进本仓库。

## 仓库结构（维护者参考）

| 文件 | 性质 |
| --- | --- |
| `AGENTS.md` | 强制底线，复制到业务项目 |
| `快速开始.md` | 复制到业务项目的工具启用模型与工作入口导航 |
| `docs/README.md` | 业务项目 docs 目录说明模板（复制到业务项目） |
| `design/README.md` | 业务项目设计资料目录说明模板 |
| `docs/规范/代码设计原则.md` | KISS、YAGNI、DRY、职责边界与抽象取舍的通用规范 |
| `docs/规范/工具协作.md` | OpenSpec、CodeGraph、Superpowers 采用门禁、职责、组合方式与失败处理的唯一来源 |
| `docs/规范/UI任务重组.md` | UI 设计来源、两阶段任务基线、映射、机械核对和旧结构修复的唯一来源 |
| `.agents/README.md` | 业务项目 skill 目录索引模板（默认不内置项目级 skill，复制到业务项目） |
| `SECURITY.md` / `CODE_OF_CONDUCT.md` | 开源社区文件 |
| `新需求开发.md` / `需求变更.md` | 复制到业务项目的新需求与需求变更流程，包含可选 OpenSpec 主流程与轻量载体分支 |
| `流程公共约定.md` | 旧路径兼容入口，不再承载独立规则或新增引用 |
| `README.md` | 入口说明 |
| `CHANGELOG.md` | 规范包版本演进 |

## 提交前请确认

1. 改动范围与 PR 描述一致，不夹带无关格式化。
2. `AGENTS.md` 是影响所有使用者的强制底线，改动门槛**高**：在 PR 中说明改了什么、是否为 breaking change、使用者如何迁移。
3. 工具详细采用门禁、职责、调用方式、可用性检查和失败处理只写入 `docs/规范/工具协作.md`；UI 两阶段任务重组规则只写入 `docs/规范/UI任务重组.md`。`README.md` 只保留官方来源和接入提示词，快速开始与场景流程只保留导航、调用入口或场景特有内容。
4. 中文文档修改后，确认文件 UTF-8 编码与中文显示正常。
5. 不提交密钥、token、`.env` 或未脱敏的生产数据。

## `AGENTS.md` 变更原则

- **产品无关、工具按需启用**：不绑定单一 AI 产品；OpenSpec、CodeGraph、Superpowers 只有在用户明确选择或当前任务已确认采用时才启用。
- **前后端通用**：不写特定框架实现细节。
- **整套使用**：`AGENTS.md`、`docs/`、`design/README.md` 和 `.agents/README.md` 作为完整规范接入，不维护单文件安装路径。
- **不膨胀**：项目特有架构与跨需求业务规则放业务项目 `docs/`，UI、页面、交互和视觉设计资料放 `design/`；进行中的接口/API 契约变化放需求载体，不写入 `AGENTS.md`。

大改建议先开 Issue 对齐方向，再提 PR。

## Pull Request 流程

1. Fork，从 `main` 创建分支（建议 `docs/xxx`、`fix/xxx`）。
2. 单次 PR 尽量聚焦一个主题。
3. 按 [PR 模板](./.github/pull_request_template.md) 填写变更摘要、影响范围、Breaking changes、如何验证。
4. 维护者 review 通过后合并。

## 行为准则

见 [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md)。

## 许可证

贡献内容按 [MIT License](./LICENSE) 授权。
