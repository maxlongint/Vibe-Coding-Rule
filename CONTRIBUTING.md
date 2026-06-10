# 贡献指南

感谢你对 [Vibe Coding Rule](https://github.com/maxlongint/Vibe-Coding-Rule) 的关注。本仓库是一份可复制的 AI 协作规范（核心就是 `AGENTS.md`），欢迎通过 Issue 和 Pull Request 改进。

## 核心原则：保持精简

本仓库的价值在于**一页纸、复制即用**。提交前请先问自己：这条规则是不是"AI 写好代码必须遵守的通用底线"？

- ✅ 跨项目通用、与具体技术栈/工具无关的协作底线
- ❌ 某公司内部流程、域名、业务规范
- ❌ 特定框架/技术栈的编码风格大全
- ❌ 真实业务需求或具体项目文档

后两类应放在你自己业务项目的文档里，不进本仓库。

## 仓库结构（维护者参考）

| 文件 | 性质 |
| --- | --- |
| `AGENTS.md` | 强制底线，复制到业务项目 |
| `快速开始.md` | 落地选型、目录总览与初始化清单 |
| `docs/README.md` | 业务项目 docs 目录说明模板（复制到业务项目） |
| `.agents/README.md` | 业务项目 skill 目录索引模板（复制到业务项目） |
| `SECURITY.md` / `CODE_OF_CONDUCT.md` | 开源社区文件 |
| `新需求开发.md` / `需求变更.md` | 可选流程（OpenSpec 主） |
| `流程公共约定.md` | 流程文档共用段落，避免重复 |
| `README.md` | 入口说明 |
| `CHANGELOG.md` | 规范包版本演进 |

## 提交前请确认

1. 改动范围与 PR 描述一致，不夹带无关格式化。
2. `AGENTS.md` 是影响所有使用者的强制底线，改动门槛**高**：在 PR 中说明改了什么、是否为 breaking change、使用者如何迁移。
3. 流程文档与 `流程公共约定.md` 重复内容应引用而非复制粘贴。
4. 中文文档修改后，确认文件 UTF-8 编码与中文显示正常。
5. 不提交密钥、token、`.env` 或未脱敏的生产数据。

## `AGENTS.md` 变更原则

- **工具无关**：不绑定单一 AI 产品或第三方工具的安装前提。
- **前后端通用**：不写特定框架实现细节。
- **可单独使用**：复制 `AGENTS.md` 一个文件仍须完整自洽。
- **不膨胀**：项目特有 UI/API/业务规则放业务项目 `docs/`，不写入 `AGENTS.md`。

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
