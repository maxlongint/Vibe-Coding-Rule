# 贡献指南

感谢你对 [Vibe Coding Rule](https://github.com/maxlongint/Vibe-Coding-Rule) 的关注。本仓库是可复制的 AI 协作规则包，欢迎通过 Issue 和 Pull Request 改进文档、技能包与示例。

## 你可以贡献什么

| 类型 | 示例 | 门槛 |
| --- | --- | --- |
| 文档与教程 | 修正笔误、补充场景说明、翻译 | 低 |
| `docs/规范/` | 治理流程、工具链说明、模板 | 中 |
| `.agents/skills/` | 新技能包、现有技能改进 | 中 |
| `AGENTS.md` | 强制底线、验证规则、优先级 | **高**（影响所有使用者） |

以下内容**不建议**提交到本仓库 upstream：

- 某公司内部的 Jira/飞书流程、域名、业务规范
- 特定技术栈的编码风格大全（应放在业务项目 `docs/规范/`）
- 真实业务需求或 OpenSpec change

## 提交前请确认

1. 改动范围与 PR 描述一致，不夹带无关格式化。
2. 若修改 `AGENTS.md`，在 PR 中说明：影响哪些档位（Minimal / Standard / Full）、是否为 **breaking change**、使用者如何迁移。
3. 中文文档修改后，确认文件 UTF-8 编码与中文显示正常。
4. 不提交密钥、token、`.env` 或未脱敏的生产数据。

## Pull Request 流程

1. Fork 本仓库，从 `main` 创建分支（建议命名：`docs/xxx`、`fix/xxx`、`skill/xxx`）。
2. 尽量保持单次 PR 聚焦一个主题。
3. 填写 PR 说明：
   - **变更摘要**（为什么改）
   - **影响范围**（哪些文件 / 档位）
   - **Breaking changes**（如有）
   - **如何验证**（你做了哪些检查）
4. 维护者 review 通过后合并。

## `AGENTS.md` 变更原则

`AGENTS.md` 是跨项目的强制底线，变更应满足：

- **工具无关**：不绑定单一 AI 产品、OpenSpec、CodeGraph 或 Superpowers 的安装前提。
- **前后端通用**：不写特定框架实现细节。
- **可单独使用**：Minimal 档位（仅 `AGENTS.md`）仍须完整自洽。
- **职责不膨胀**：项目特有 UI/API/业务规则仍放在业务项目 `docs/规范/`，不写入 `AGENTS.md`。

如需讨论大改，建议先开 Issue 对齐方向，再提 PR。

## Issue 指南

- **Bug / 文档错误**：说明文件路径、期望与实际行为。
- **规则建议**：说明场景、现有规则的不足、建议改法及是否 breaking。
- **使用问题**：说明档位（Minimal/Standard/Full）、AI 工具、是否安装可选工具。

## 行为准则

参与本项目即表示尊重他人、就事论事。如有骚扰或不当行为，维护者可关闭 Issue/PR 或限制参与。

## 许可证

贡献内容按 [MIT License](./LICENSE) 授权；你保留对自己贡献的权利，同时授予项目及使用者 MIT 许可下的使用权利。
