# Academic Submission Skills Playbook

这是一套可跨 AI 会议与期刊复用的论文 Skills Playbook。目标不是为某一个 venue 临时拼一套工具，而是形成一组稳定的“核心能力层 + venue 适配层 + 项目覆盖层”。

如果你只想快速决定“该用哪个 Skill”，先看 `INDEX.md`；如果你要理解整套体系的分层、canonical set 和推荐链路，再看本文件。

这份 Playbook 现在吸收了两组来源：

- 当前 `.trae/skills` 中已经比较稳定的路由、总控与论文子任务 Skills
- `skills from Di` 中较强的专项能力，例如去 AI 味、严格理论核查、投稿平台/venue 操作经验

整理后的原则是：保留低重叠、高复用、可路由的 Skill；把强但过于 venue-specific 或流程串得太死的外部 Skill，抽象成更通用的能力模块。

## 推荐技能分层

### 1. 入口层

- `paper-intake-router`
  作用：识别论文生命周期阶段、主风险与下一步 Skill 链

### 2. 总控编排层

- `academic-paper-factory`
  作用：统筹从 idea 到投稿的全流程

### 3. 核心执行层

- `paper-positioning`
- `literature-survey-builder`
- `claim-evidence-mapper`
- `citation-reality-guard`
- `experiment-story-builder`
- `latex-first-compile-bootstrap`
- `proof-consistency-checker`
- `writing-naturalness-guard`
- `reviewer-risk-audit`
- `paper-ratchet-optimizer`
- `venue-fit-selector`
- `submission-integrity-audit`
- `latex-submission-packager`
- `venue-submission-adapter`

### 4. 投稿后阶段

- `rebuttal-response-drafter`

## 建议保留的“标准技能集合”

如果要把这套库复用到其他学术项目，优先保留以下 Skill 作为 canonical set：

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

这 17 个 Skill 覆盖了绝大多数学术期刊/会议工作流，且不会把库绑死在 NeurIPS、OpenReview 或某个单一论文类型上。

其中三类新增能力分别对应三批高频缺口：

- `claim-evidence-mapper`
  作用：把摘要、引言、结论、定理、图表中的 claims 显式映射到证据，防止“主张大于证据”
- `venue-fit-selector`
  作用：在“先写什么”和“按哪个 venue 格式改”之间，补上“到底投哪里更合适”的决策层
- `submission-integrity-audit`
  作用：合并图表出版级审计与 disclosure / checklist / supplementary 一致性审计，避免继续拆出多个高重叠目录

## 外部 Skills 的整理结论

### 建议吸收为通用能力

- `deai` -> 吸收为 `writing-naturalness-guard`
- `theory` -> 吸收为 `proof-consistency-checker`
- `nips` + `openreview` -> 抽象为 `venue-submission-adapter`

### 建议作为参考模板保留，但不进入 canonical set

- `restruct`
  原因：价值高，但和 `academic-paper-factory`、`paper-positioning`、`reviewer-risk-audit` 有明显交叉，更适合作为写作模板参考而非独立常驻 Skill
- `ref`
  原因：已有 `citation-reality-guard`，后者更适合当前库的命名与路由体系

### 不建议纳入学术写作核心集合

- `post`
  原因：流程串得过死，跨项目时容易误触发大量不必要步骤
- `dir`
  原因：更偏目录整理，而不是论文写作核心能力
- `nips`
  原因：venue-specific 过强，建议让 `venue-submission-adapter` + `latex-submission-packager` 承担
- `openreview`
  原因：平台相关能力应该放在 venue 适配层，而不是单独占一个核心 Skill

## 推荐使用方式

### 模糊请求

如果用户说：

- “帮我推进这篇论文”
- “从这一步继续”
- “看看接下来做什么最合适”

优先用：

- `paper-intake-router`

### 从零开始

推荐链路：

`paper-intake-router -> academic-paper-factory -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> reviewer-risk-audit -> venue-fit-selector -> submission-integrity-audit -> latex-submission-packager`

### 理论型或理论+方法型论文

推荐链路：

`paper-intake-router -> academic-paper-factory -> paper-positioning -> claim-evidence-mapper -> proof-consistency-checker -> citation-reality-guard -> reviewer-risk-audit`

### 中后期打磨

推荐链路：

`paper-intake-router -> academic-paper-factory -> claim-evidence-mapper -> paper-ratchet-optimizer -> writing-naturalness-guard -> citation-reality-guard -> reviewer-risk-audit`

### venue 切换或投稿适配

推荐链路：

`paper-intake-router -> venue-fit-selector -> venue-submission-adapter -> submission-integrity-audit -> latex-submission-packager`

### 投稿前终审

推荐链路：

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

### 投稿后回应

推荐链路：

`paper-intake-router -> rebuttal-response-drafter`

## 每个 Skill 的统一要求

每个 Skill 默认应包含：

- frontmatter：`name` + `description`
- `何时调用`
- `核心目标`
- `执行步骤`
- `标准输入`
- `标准交付`
- `Examples`
- 与其他 Skills 的 handoff 关系

快速索引入口：

- `INDEX.md`

## 设计原则

1. 路由优先，避免一上来就盲改稿件
2. 先区分通用能力、venue 适配和项目局部规则，再落地执行
3. 强主张必须有证据闭环
4. 先做 venue fit 选择，再做 venue-specific 适配
5. 所有引用必须真实存在且元数据准确
6. 每轮优化只保留可验证提升，避免越改越乱
7. 不把 NeurIPS、OpenReview、单一模板要求硬编码进所有 Skill

## 维护建议

- 新增 Skill 时，先判断它是“核心能力”还是“venue/project 适配”
- 不要创建和现有 Skill 高度重叠的新 Skill
- 如果两个候选 Skill 共享同一输入材料、同一交付对象且主要服务同一阶段，优先合并而不是拆目录
- 如果只是补 venue 规则，优先更新 `venue-submission-adapter` 或规则文件，而不是复制一份新的 conference-specific Skill
- 如果只是补写作策略，优先增强现有 Skill，而不是新增冗余目录
- 每次新增或大改 Skill 后，都检查 description、示例、输入/交付协议、handoff 关系是否清晰
