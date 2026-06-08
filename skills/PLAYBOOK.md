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
- `idea-evaluator`
- `literature-survey-builder`
- `grounded-paper-qa`
- `reading-note-synthesizer`
- `claim-evidence-mapper`
- `citation-reality-guard`
- `experiment-story-builder`
- `figure-design-advisor`
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
4. `idea-evaluator`
5. `literature-survey-builder`
6. `grounded-paper-qa`
7. `reading-note-synthesizer`
8. `claim-evidence-mapper`
9. `citation-reality-guard`
10. `experiment-story-builder`
11. `figure-design-advisor`
12. `latex-first-compile-bootstrap`
13. `proof-consistency-checker`
14. `writing-naturalness-guard`
15. `reviewer-risk-audit`
16. `paper-ratchet-optimizer`
17. `venue-fit-selector`
18. `submission-integrity-audit`
19. `latex-submission-packager`
20. `venue-submission-adapter`
21. `rebuttal-response-drafter`

这 21 个 Skill 覆盖了绝大多数学术期刊/会议工作流，且不会把库绑死在 NeurIPS、OpenReview、Zotero 或某个单一论文类型上。

其中三类新增能力分别对应三批高频缺口：

- `claim-evidence-mapper`
  作用：把摘要、引言、结论、定理、图表中的 claims 显式映射到证据，防止“主张大于证据”
- `venue-fit-selector`
  作用：在“先写什么”和“按哪个 venue 格式改”之间，补上“到底投哪里更合适”的决策层
- `submission-integrity-audit`
  作用：合并图表出版级审计与 disclosure / checklist / supplementary 一致性审计，避免继续拆出多个高重叠目录

这轮继续补了两类近期社区中更热门、但此前本库较弱的中层能力：

- `grounded-paper-qa`
  作用：把“根据这些论文回答问题”和“先核清事实再写作”独立出来，强调回答必须回到给定材料
- `reading-note-synthesizer`
  作用：把阅读笔记、摘录和批注转成 related work、定位、rebuttal 可直接复用的结构化资产

参考 `Supervisor-Skills` 之后，又补上了两类更偏“导师式科研判断”的能力：

- `idea-evaluator`
  作用：在真正进入 paper positioning 前，先评估一个 idea 值不值得做、适不适合作为主线论文方向
- `figure-design-advisor`
  作用：把动机图、方法总览图和实验结果图单独建模，补上“图的叙事设计”这一层

同时没有继续新增 `intro-drafter` 或 `benchmark-paper-template` 这类目录，而是把它们更值得保留的价值压缩吸收到 `paper-positioning`：

- `introduction-flowchart` mode
- `技术类论文` 与 `benchmark/evaluation 类论文` 的定位模板

同样地，没有再拆一个独立的 benchmark 实验 Skill，而是把类型化实验叙事直接吸收到 `experiment-story-builder`：

- `core-experiment-story`
- `benchmark-evaluation mode`

审稿层也沿用同一原则，没有再拆 `technical-reviewer` 或 `benchmark-reviewer`，而是把类型化校准直接吸收到 `reviewer-risk-audit`：

- `technical-paper calibration`
- `benchmark-evaluation calibration`

修复层也继续沿用同一原则，没有再拆 `technical-ratchet` 或 `benchmark-ratchet`，而是把类型化修复直接吸收到 `paper-ratchet-optimizer`：

- `technical-paper repair`
- `benchmark-evaluation repair`

## 外部 Skills 的整理结论

### 建议吸收为通用能力

- `deai` -> 吸收为 `writing-naturalness-guard`
- `theory` -> 吸收为 `proof-consistency-checker`
- `nips` + `openreview` -> 抽象为 `venue-submission-adapter`
- `lit-review mode` + `systematic-review mode` -> 增强为 `literature-survey-builder`
- `fact-check mode` + citation-backed answer pattern -> 抽象为 `grounded-paper-qa`
- notes-to-writing workflow -> 抽象为 `reading-note-synthesizer`
- supervisor-style idea triage -> 抽象为 `idea-evaluator`
- motivated / overview / result figure design -> 抽象为 `figure-design-advisor`

其中 `writing-naturalness-guard` 不再只是语言自然化器，而是“自然化 + 风险门禁”组件：

- 先查提示语残留、元编辑残留和明显 AI 痕迹
- 若发现疑似幻觉引用或错引，转交 `citation-reality-guard`
- 若只是风格模板化，再执行 humanize 改写

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

`paper-intake-router -> academic-paper-factory -> idea-evaluator -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> figure-design-advisor -> experiment-story-builder -> citation-reality-guard -> reviewer-risk-audit -> venue-fit-selector -> submission-integrity-audit -> latex-submission-packager`

若前期有大量指定论文需要先问清事实，可插入：

`paper-intake-router -> grounded-paper-qa -> literature-survey-builder`

若前期已有大量读书笔记和摘录，可插入：

`paper-intake-router -> reading-note-synthesizer -> literature-survey-builder`

若还在判断一个题值不值得做，可前插：

`paper-intake-router -> idea-evaluator -> paper-positioning`

若主图/总览图表达不清，可前插：

`paper-intake-router -> figure-design-advisor -> experiment-story-builder`

投稿前审核与优化阶段，推荐显式走一个闭环：

`reviewer-risk-audit -> paper-ratchet-optimizer -> reviewer-risk-audit`

第一轮审计负责给出 findings、score、confidence 和 `GO / CONDITIONAL GO / NO-GO`；
中间一轮由 `paper-ratchet-optimizer` 读取 `Findings + Scorecard + Decision`，只修一个最高杠杆问题；
最后一轮复审确认 score 是否真的改善、是否仍有 `P0/P1` 未关闭。

若论文类型已明确，建议把同一个 type signal 贯穿整条链：

`paper-positioning -> experiment-story-builder -> reviewer-risk-audit -> paper-ratchet-optimizer`

也就是定位层先判断更像技术类还是 benchmark/evaluation 类，实验层按对应证据链组织，审稿层按对应口径打分，修复层再按对应高杠杆缺口做最小闭环修复。

### 理论型或理论+方法型论文

推荐链路：

`paper-intake-router -> academic-paper-factory -> paper-positioning -> claim-evidence-mapper -> proof-consistency-checker -> citation-reality-guard -> reviewer-risk-audit`

### 中后期打磨

推荐链路：

`paper-intake-router -> academic-paper-factory -> claim-evidence-mapper -> paper-ratchet-optimizer -> writing-naturalness-guard -> citation-reality-guard -> reviewer-risk-audit`

说明：

- 若 `writing-naturalness-guard` 命中 `P0` 引用风险，可先切到 `citation-reality-guard` 再返回自然化
- 若命中 disclosure、图表或提交材料一致性风险，可转交 `submission-integrity-audit`
- 若已知是技术类或 benchmark/evaluation 类论文，优先让 `paper-ratchet-optimizer` 继续沿用上一轮 `reviewer-risk-audit` 的 paper-type calibration

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
8. 去 AI 味不是单纯修文，必须先区分文风问题与事实/合规风险

## 维护建议

- 新增 Skill 时，先判断它是“核心能力”还是“venue/project 适配”
- 不要创建和现有 Skill 高度重叠的新 Skill
- 如果两个候选 Skill 共享同一输入材料、同一交付对象且主要服务同一阶段，优先合并而不是拆目录
- 如果只是补 venue 规则，优先更新 `venue-submission-adapter` 或规则文件，而不是复制一份新的 conference-specific Skill
- 如果只是补写作策略，优先增强现有 Skill，而不是新增冗余目录
- 每次新增或大改 Skill 后，都检查 description、示例、输入/交付协议、handoff 关系是否清晰
