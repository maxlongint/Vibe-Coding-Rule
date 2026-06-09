# AI 协作与工具链

> **适用范围：** 采用可选工具链（OpenSpec、CodeGraph、Superpowers、项目技能包）中**部分或全部**的项目。**未安装某工具时，按 `AGENTS.md` 强制底线 fallback** 到人工分析、轻量记录或向用户确认；缺失任一工具不影响 `AGENTS.md` 效力。
>
> **权威关系：** 验证、提交权限、需求治理底线以 `AGENTS.md` 为准；本文定义工具分工、执行顺序与任务分流，不替代强制规则。

## 可选工具协作模型

`AGENTS.md` 为所有档位的必需基线；下表组件均为 **Full 档位可选增强**：

| 组件 | 职责 |
| --- | --- |
| `AGENTS.md` | 规则、权限边界、验证底线、优先级 |
| OpenSpec | 需求变化、设计记录、任务拆分、验收、归档 |
| CodeGraph | 代码事实、调用链、依赖关系、重构影响 |
| Superpowers | AI 执行方法：澄清、调试、TDD、计划、审查、完成前验证 |

## 技能包目录

默认目录：

```text
.agents/skills/
  <skill-name>/SKILL.md
  <skill-name>/references/   # 按需
```

规则：

- 项目可在 `docs/规范/` 中声明其他技能包目录、入口文件或命名方式；未声明时使用默认目录。
- 如果 `.agents/skills/README.md` 或 `.agents/skills/index.md` 存在，AI 检查技能包时应先读索引，再按需读具体 `SKILL.md`。
- `SKILL.md` 是默认入口，应说明适用场景、执行规则和需要读取的参考资料。
- `references/` 放技能包补充资料，不要在任务开始时一次性读取全部资料。
- 简单问答、纯解释、纯命令查询等不涉及工程决策的任务，可以不加载技能包。
- 技能包规则与当前项目 `AGENTS.md` 冲突时，以 `AGENTS.md` 为准；与用户本轮明确指令冲突时，在不违反平台安全、项目治理和权限边界的前提下，以用户本轮指令为准。
- 不要把技能包内容复制到通用规范文档中。

**触发边界：**

- `executing-complex-requirements`：复杂、高风险或跨模块需求时，AI 可检查并加载；用户也可点名使用。
- `updating-requirement-changes`：**仅**在用户明确点名本技能，或明确要求按需求变化同步技能处理时触发；普通需求讨论、规则问答或临时想法不得自动套用。
- 未点名 `updating-requirement-changes` 时，按 [需求治理与文档结构.md](./需求治理与文档结构.md) 的 OpenSpec 流程处理需求变化。

## 标准执行顺序

```text
1. 确认已加载 AGENTS.md 中的规则、权限边界和验证要求。
2. 判断任务类型：问答、小修、bugfix、中等需求、重大需求、重构或紧急修复。
3. 如任务涉及行为、接口、数据、权限、流程或验收标准变化，必须先建立或更新可追溯需求记录；项目已存在 `openspec/` 时优先使用 OpenSpec change；否则按 [需求治理与文档结构.md](./需求治理与文档结构.md) 向用户确认载体（外部需求系统或轻量记录等）。
4. 如任务涉及代码修改、影响判断、调用链、重构或缺陷定位，有 CodeGraph 或其他代码分析工具时优先使用；否则人工分析并记录剩余风险。
5. 根据任务类型启用对应 Superpowers 流程（若已安装）。
6. 更新需求记录后，再进入实现；若用户明确使用外部需求系统，应同步保留外部链接、编号或确认来源。
7. 完成后运行必要验证，并说明验证结果和剩余风险（见 AGENTS.md 第 1 节工程守则）。
```

说明：上述顺序是治理与交付检查清单，不是每一步都必须严格串行。澄清阶段（如 `brainstorming`）可在创建 OpenSpec change 之前进行；但**进入实现前**须满足 [需求治理与文档结构.md](./需求治理与文档结构.md) 的记录底线，并遵守「先更新记录再改代码」。

## 工具使用边界

- OpenSpec 定义“要做什么、为什么做、做到什么程度算完成”；不替代代码分析和测试。
- CodeGraph 定义“代码现在真实是什么样、改动影响哪里”；不替代需求确认和业务验收。
- Superpowers 定义“AI 如何工作”；不替代项目规则、需求记录、代码事实和验证结果。
- 当 OpenSpec、CodeGraph 和实现现状冲突时，应先判断是需求变更、代码偏离规格，还是分析范围不足；不得直接选择最省事的实现路径。

**事实和方法边界：**

- 需求事实优先从用户确认、OpenSpec 和外部需求系统获得。
- 代码事实优先从 CodeGraph、测试结果和实际源码获得。
- 执行方法优先使用 Superpowers；Superpowers 只规定怎么推进，不改变需求事实和代码事实。
- 先检查项目是否存在 `openspec/`：存在则走 OpenSpec change 主流程，Superpowers 仅作澄清、TDD、验证等执行纪律；不存在 `openspec/` 时，若任务属于真实业务需求，须先向用户确认需求记录载体（初始化 OpenSpec、外部需求系统或轻量记录，见 [需求治理与文档结构.md](./需求治理与文档结构.md)），不得默认自建自定义需求目录；确认后可使用 Superpowers 默认落档路径承载设计与计划。

## 任务分流表

涉及代码改动时，**必须先完成影响分析**；CodeGraph 可用时优先使用，不可用时改用人工分析并记录剩余风险。下表「影响分析」列指这一底线，不等同于「必须安装 CodeGraph」。

| 任务类型 | OpenSpec | 影响分析 | Superpowers | 备注 |
| --- | --- | --- | --- | --- |
| 只读分析、解释、命令查询 | 不需要 | 不需要 | 可选 | 不改文件时说明未修改 |
| 小修、文案、局部呈现 | 通常不需要 | 视影响范围 | verification-before-completion | 用轻量记录说明范围和验证 |
| 缺陷修复 | 视是否改变业务规则 | 必须（CodeGraph 优先） | systematic-debugging、test-driven-development、verification-before-completion | 若改变验收标准或对外行为，补 OpenSpec |
| 新功能或中等需求 | 必须 | 必须（CodeGraph 优先） | brainstorming、writing-plans、test-driven-development、verification-before-completion | 先规格后实现 |
| 重大需求、权限、数据、接口变更 | 必须 | 必须（CodeGraph 优先） | brainstorming、writing-plans、test-driven-development、requesting-code-review、verification-before-completion | 需要记录方案、风险和验收 |
| 重构 | 视行为是否变化 | 必须（CodeGraph 优先） | writing-plans、verification-before-completion | 重构前记录影响范围；若行为变化按需求变化处理 |
| 紧急修复 | 可先轻量记录，事后补齐 | 必须覆盖故障链路（CodeGraph 优先） | systematic-debugging、verification-before-completion | 不得跳过验证底线 |

## 协作工具最低动作标准

**OpenSpec：**

- 至少明确 change 路径、目标、边界、验收标准、任务状态和验证方式。
- 归档或废弃时记录状态、日期、原因和剩余风险。

**CodeGraph：**

- 理解模块优先用 `codegraph_explore`；查调用关系用 callers/callees；重构或跨模块修改前用 impact。
- 结论应写入设计记录、任务记录或交付说明。
- CodeGraph 不可用时，说明改用人工搜索并记录剩余风险。

**Superpowers：**

- 按任务类型启用对应流程；出现 bug、测试失败或异常结果时优先使用 systematic-debugging。
- 完成前必须使用 verification-before-completion 的原则核对验证证据。

**三者结论不一致时：** 先暂停实现并判断冲突来源：需求变化、规格过期、代码偏离规格、测试覆盖不足或分析范围不足；必要时更新 OpenSpec 或向用户确认。

## 执行前扫描顺序

完整扫描顺序（含本文件加载前提）见根目录 [AGENTS.md](../../AGENTS.md)「执行前扫描顺序」。在已加载 `AGENTS.md` 速查与第 1–5 节的前提下，补充按工具链执行任务时：

1. 扫描 `docs/规范/` 的目录索引或文件名，识别与当前任务相关的规范；仅加载明显相关的规范正文，不批量读取无关内容。
2. 再检查 `.agents/skills/` 是否有相关技能包；小范围、低风险修改可以只检查目录索引或技能包名称，发现明显相关技能包时再读取对应 `SKILL.md` 和必要参考资料。
3. 如果 `docs/规范/` 和 `.agents/skills/` 都存在相关规则，按 `AGENTS.md`「规则优先级」处理；两者均不存在或均无匹配时，按根目录 `AGENTS.md` 执行。

## 文档与规则修改纪律

- 未经用户明确要求修改 `AGENTS.md` 或规则文档时，不得擅自修改；只能提出建议或说明建议改动点。
- 修改 `AGENTS.md` 或长期规范时，应说明变更原因、影响范围；项目有 changelog、需求记录或规范变更记录时，应同步记录。
- `AGENTS.md` 应保持为协作入口和强制规则索引；工程、注释、编码、提交规则固定写在本文件。项目特有规则、长示例和工具教程放 `docs/规范/` 或由项目 README 链接。
- 修改文档前先判断文档性质：项目级长期规范、真实业务需求、教学案例。
- 用户明确要求沉淀真实业务需求时，项目已启用 OpenSpec 则默认写入 OpenSpec change；否则写入用户确认的外部需求系统、issue 或轻量记录，并保留可追溯来源。
- 用户明确要求沉淀教学案例时，写入 `docs/案例/`；否则不得主动创建案例目录。
- 不为具体 AI 工具、插件或连接器单独建立长期规范，除非用户明确要求。
