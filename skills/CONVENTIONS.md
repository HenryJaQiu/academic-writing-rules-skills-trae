# Skill Conventions

这份文档定义本工作区学术投稿 Skills 的统一约定，便于后续持续扩展。

## 分层约定

Skill 与规则默认分三层：

- `通用核心层`：适用于大多数学术期刊/会议，例如定位、文献、claim-evidence mapping、实验、审稿风险、语言自然化、证明核查
- `venue 适配层`：只负责会议/期刊特定要求，例如页数、投稿平台字段、匿名性、模板顺序
- `项目覆盖层`：只负责当前项目特有的图号映射、实验脚本关系、项目节奏与局部纪律

新增约定：

- `venue-fit-selector` 属于“投稿决策层”，位于通用核心层与 venue 适配层之间，先判断“投哪里合适”，再进入具体 venue 适配
- `submission-integrity-audit` 负责图表出版质量与 disclosure 一致性，默认不承担编译、打包和模板修复

约束：

- 通用 Skill 不应硬编码 NeurIPS / ICML / TPAMI 某一家的细则
- venue-specific 要求优先放入规则文件或 `venue-submission-adapter`
- 项目局部习惯不要污染全局 Skill

## 目录约定

- 当前规则仓库中，每个 Skill 独立放在 `skills/<skill-name>/`
- 当把 Skill 安装到具体项目工作区时，对应落在 `.trae/skills/<skill-name>/`
- 每个 Skill 至少包含一个 `SKILL.md`
- `skill-name` 使用小写加连字符，例如 `paper-intake-router`

## Frontmatter 约定

每个 `SKILL.md` 顶部必须包含：

```md
---
name: "skill-name"
description: "说明这个 Skill 做什么，以及何时调用它。"
---
```

要求：

- `name` 全局唯一
- `description` 同时说明功能和触发场景
- 描述尽量短，但必须让路由器能判断是否该调用

## 结构约定

建议每个 Skill 至少包含以下部分：

- `何时调用`
- `核心目标`
- `执行步骤`
- `标准输入`
- `标准交付`
- `Examples`
- `与其他 Skills 的关系`

若 Skill 会被长期复用，建议再补：

- `禁忌 / 不该做什么`
- `默认保守策略`
- `与规则层的边界`

## 写作约定

- 默认使用中文说明，但关键术语可以保留英文
- 避免空泛口号，优先给可执行规则
- 避免把不同阶段的任务混在同一个 Skill 里
- 强调低风险、可验证、可回滚
- 优先抽象成跨 venue 能力，而不是直接复制某个会议的操作说明

## 路由约定

- 模糊请求优先进入 `paper-intake-router`
- 多 Skill 任务优先由 `academic-paper-factory` 编排
- 专用 Skill 尽量解决一个核心问题，不承担统一入口职责
- 用户明确说“每个 claim 有没有证据支撑 / abstract 句子是否被正文承接”时，优先进入 `claim-evidence-mapper`
- 用户明确说“去 AI 味 / humanize”时，优先进入 `writing-naturalness-guard`
- 对“去 AI 味”的默认理解应包含三联任务：文字自然化、提示语/元编辑残留检查、幻觉引用与明显合规风险初筛
- 用户明确说“检查证明 / 补推导 / theorem 不一致”时，优先进入 `proof-consistency-checker`
- 用户明确说“投哪个 venue 更合适 / 期刊还是会议更稳 / 转投去哪”时，优先进入 `venue-fit-selector`
- 用户明确说“切换 venue / 准备投稿系统字段 / 改成某会议或期刊格式”时，优先进入 `venue-submission-adapter`
- 用户明确说“检查 figures / tables / checklist / ethics / code availability / disclosure 一致性”时，优先进入 `submission-integrity-audit`

## 输出约定

尽量给出结构化交付，而不是泛泛建议。默认至少包含：

- 当前判断
- 风险优先级
- 建议动作
- 如需 handoff，明确下一个 Skill

## 质量约定

- 不能包含虚构引用、虚构 venue 要求或虚构投稿规范
- 不得诱导过度宣称研究贡献
- 对不确定事项应显式标明需要核验
- 更新 Skill 后要检查诊断与结构一致性
- 任何牵涉图表、公式、引用、投稿格式的 Skill，都应明确“修改后如何验证”
- 如果 Skill 建议调大图内字体、移动 legend、改页数或移动正文内容，必须同步要求检查导出 PDF 与编译后的最终论文 PDF
- 任何牵涉 disclosure、checklist、code/data availability 的 Skill，都必须要求区分“已提供”“将提供”“不可提供”三种状态，禁止模糊承诺
- 任何以“去 AI 味”为目标的 Skill，都不得只做文风润色；必须同时检查提示语残留、元编辑残留，以及是否存在需转交 `citation-reality-guard` 的幻觉引用风险
