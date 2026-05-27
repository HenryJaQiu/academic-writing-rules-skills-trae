# academic-writing-rules-skills-trae

一个用于管理通用学术写作规则与 Skills 的仓库，面向 AI 领域论文从 idea、定位、文献综述、实验叙事到投稿与 rebuttal 的完整工作流。

这个仓库的目标不是为某一个会议临时堆 prompt，而是维护一套可以跨项目复用、可持续演化的：

- 通用学术写作规则
- 可路由的论文 Skills
- venue 适配经验
- 项目级扩展入口

## 仓库结构

```text
rules/
  academic_base.md
  venue_rules.md
  project_rules.md
  ...
skills/
  paper-intake-router/
  academic-paper-factory/
  paper-positioning/
  literature-survey-builder/
  claim-evidence-mapper/
  citation-reality-guard/
  experiment-story-builder/
  proof-consistency-checker/
  reviewer-risk-audit/
  venue-fit-selector/
  submission-integrity-audit/
  ...
  PLAYBOOK.md
  CONVENTIONS.md
```

## 核心文档

- `skills/INDEX.md`
  按“做什么 / 何时调用 / 常见后续”快速总览全部 Skills
- `skills/PLAYBOOK.md`
  说明整套 Skills 的分层、推荐链路与 canonical set
- `skills/CONVENTIONS.md`
  定义 Skill 的目录、frontmatter、路由与质量约定
- `rules/academic_base.md`
  通用学术写作底层规则
- `rules/venue_rules.md`
  会议/期刊层规则入口
- `rules/project_rules.md`
  具体科研项目的局部补充规则

## 当前核心 Skills

建议作为默认 canonical set 使用：

1. `paper-intake-router`
2. `academic-paper-factory`
3. `paper-positioning`
4. `literature-survey-builder`
5. `claim-evidence-mapper`
6. `citation-reality-guard`
7. `experiment-story-builder`
8. `latex-first-compile-bootstrap`
9. `proof-consistency-checker`
10. `writing-naturalness-guard`
11. `reviewer-risk-audit`
12. `paper-ratchet-optimizer`
13. `venue-fit-selector`
14. `submission-integrity-audit`
15. `latex-submission-packager`
16. `venue-submission-adapter`
17. `rebuttal-response-drafter`

## 新增能力

这次补充了 3 个高复用能力：

- `claim-evidence-mapper`
  用于把论文中的 claim 显式映射到 theorem、experiment、figure、table、citation，检查“主张是否大于证据”
- `venue-fit-selector`
  用于在正式 venue 适配前先判断“该投哪里更合适”，支持会议/期刊取舍与转投路线分析
- `submission-integrity-audit`
  合并图表出版级审计与 disclosure/checklist/supplementary 一致性审计，避免继续拆出多个高重叠 Skill

## 推荐使用方式

### 1. 模糊请求入口

当需求还比较模糊时，先走：

`paper-intake-router`

例如：

- “继续推进这篇论文”
- “你来判断接下来先做什么”
- “我想把这篇稿子推进到投稿”

### 2. 从零开始搭论文

推荐链路：

`paper-intake-router -> academic-paper-factory -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> reviewer-risk-audit -> venue-fit-selector -> submission-integrity-audit -> latex-submission-packager`

### 3. 投稿前终审

推荐链路：

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

### 4. 投稿后审稿回应

推荐链路：

`paper-intake-router -> rebuttal-response-drafter`

## 设计原则

- 路由优先，不一上来盲目润色
- 强主张必须有证据闭环
- 所有引用必须是真实存在的学术文献
- 先做 venue fit，再做 venue-specific 适配
- 优先保留低重叠、高复用、可组合的 Skill
- 项目规则可以覆盖通用规则，但不要污染通用 Skill

## 维护建议

- 新增 Skill 前，先判断是“核心能力”还是“venue/project 适配”
- 若多个候选 Skill 共享同一输入材料、交付对象和阶段，优先合并而不是拆目录
- 如果只是补某个会议/期刊要求，优先更新规则文件或 `venue-submission-adapter`
- 每次新增或更新 Skill 后，都同步检查 `PLAYBOOK.md` 和 `CONVENTIONS.md`

## 适合的使用场景

- 新建科研项目时，导入一套稳定的学术写作规则与 Skills
- 中途接手一篇已有草稿，快速判断当前阶段与主要风险
- 在投稿前做 claim、citation、figure、checklist、submission package 的完整审计
- 为不同项目复用统一框架，同时保留项目级扩展空间

## 后续扩展

如果后续新增 Skill，建议至少满足：

- 有清晰的 `name` 和 `description`
- 能明确回答“何时调用”
- 能产出结构化交付，而不是泛泛建议
- 与现有 Skill 的边界和 handoff 关系清楚

详细规范见：

- `skills/INDEX.md`
- `skills/PLAYBOOK.md`
- `skills/CONVENTIONS.md`
