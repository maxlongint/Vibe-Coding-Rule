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
| `docs/`（含 `README.md` 与其下规范） | 长期项目规范模板，递归合并到业务项目 |
| `design/README.md` | 业务项目设计资料目录说明模板 |
| `.agents/README.md` | 业务项目 skill 目录索引模板（默认不内置项目级 skill） |
| `新需求开发.md` / `需求变更.md` | **仅规范仓**工作流指引；不复制到业务项目，不参与接入/升级 |
| `规则干条.md` | **仅规范仓**维护者速查；不复制到业务项目，不参与接入/升级 |
| `README.md` | 入口说明：先日常协作导航，再工具来源、规范组成示意、接入/升级与安装提示词 |
| `CHANGELOG.md` / `CONTRIBUTING.md` / `SECURITY.md` / `CODE_OF_CONDUCT.md` | 开源社区与版本元文档；不接入业务项目 |

**接入受管范围（唯一清单）**：`AGENTS.md` + `docs/` 下全部文件 + `design/README.md` + `.agents/README.md`。

v3.0.0 起已删除 `流程公共约定.md`、`快速开始.md` 与 `docs/规范/UI任务重组.md`（UI 两阶段规则并入 `docs/规范/工具协作.md` §5）；`新需求开发.md` / `需求变更.md` 改为仅规范仓查阅。工具与 UI 重组细则见 `docs/规范/工具协作.md`，入口导航见 `README.md`「日常协作」。

## 提交前请确认

1. 改动范围与 PR 描述一致，不夹带无关格式化。
2. `AGENTS.md` 是影响所有使用者的强制底线，改动门槛**高**：在 PR 中说明改了什么、是否为 breaking change、使用者如何迁移。
3. 工具默认假定、职责、组合使用、未安装中断，以及 UI 两阶段任务重组规则，只写入 `docs/规范/工具协作.md`。工作流与阶段标题用字必须与该文件规定原文一致。`README.md` 负责定位、官方来源、接入/升级与日常导航，不在此重复工具细则。
4. 中文文档修改后，确认文件 UTF-8 编码与中文显示正常。
5. 不提交密钥、token、`.env` 或未脱敏的生产数据。
6. 不要把 `规则干条.md`、`新需求开发.md`、`需求变更.md` 或其他非受管文件写入接入提示词或业务项目受管清单。

## `AGENTS.md` 变更原则

- **产品无关、工具默认已安装**：不绑定单一 AI 产品；默认假定 OpenSpec、CodeGraph、Superpowers 已成功安装，执行中发现未安装则中断并指明工具。
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
