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

## 新增能力

这次仓库进一步补充和增强了几类高复用能力：

- `claim-evidence-mapper`
  用于把论文中的 claim 显式映射到 theorem、experiment、figure、table、citation，检查“主张是否大于证据”
- `venue-fit-selector`
  用于在正式 venue 适配前先判断“该投哪里更合适”，支持会议/期刊取舍与转投路线分析
- `submission-integrity-audit`
  合并图表出版级审计与 disclosure/checklist/supplementary 一致性审计，避免继续拆出多个高重叠 Skill
- `grounded-paper-qa`
  用于只基于指定论文、摘录和读书笔记做带出处问答与事实核查，适合“先问清文献再写作”
- `reading-note-synthesizer`
  用于把读书笔记、高亮、摘录和会议记录整理成 related work、定位和 rebuttal 可复用素材
- `literature-survey-builder` 增强
  新增 `scan / lit-review / systematic-review` 三种工作模式，支持更严格的文献覆盖说明与检索逻辑记录
- `idea-evaluator`
  用于在真正投入实验和写作前，先从导师视角评估一个 idea 是否值得做、能否成论文、是否与当前资源能力匹配
- `figure-design-advisor`
  用于把“想表达什么”转成动机图、方法总览图和实验结果图的设计方案，补上图的叙事设计层

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

如果还在判断某个 idea 值不值得做，可走：

`paper-intake-router -> idea-evaluator -> paper-positioning`

如果前期先要基于一批论文问清事实，再进入综述和写作，可走：

`paper-intake-router -> grounded-paper-qa -> literature-survey-builder -> paper-positioning`

如果已经积累了很多读书笔记、高亮和摘录，但还没变成论文资产，可走：

`paper-intake-router -> reading-note-synthesizer -> literature-survey-builder -> claim-evidence-mapper`

如果核心图还没想清楚怎么表达，可走：

`paper-intake-router -> figure-design-advisor -> experiment-story-builder -> submission-integrity-audit`

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

## 如何归纳、总结并更新强化 Skill

建议把 Skill 的持续演化当成一个固定循环，而不是想到什么就零散修改：

### 1. 先归纳真实高频任务

优先从以下来源抽共同需求：

- 你自己最近反复做的论文任务
- 不同项目里重复出现的 reviewer 攻击点
- 热门社区仓库、awesome list、Claude skill 仓库里反复出现的能力类型
- 真实投稿链路中最容易出错的环节

归纳时优先提炼：

- 这个任务真正解决什么问题
- 它通常发生在论文生命周期的哪个阶段
- 它和现有 Skill 是否高度重叠
- 它更适合“新增 Skill”还是“增强现有 Skill”

### 2. 先抽象能力边界，再决定拆分还是合并

判断标准建议统一：

- 如果输入材料、交付对象、调用时机高度一致，优先合并
- 如果它只是现有 Skill 缺一块高频能力，优先增强原 Skill
- 只有在“目标、输入、输出、路由时机”都明显独立时，才新增目录

一个实用问题是：

- 这个能力能不能用一句话清楚回答“什么时候该调用它”

如果回答不清，往往说明边界还没抽稳。

### 3. 更新时优先补 4 类内容

相比只改描述，更值得优先强化的是：

- `风险门禁`：哪些情况先停、先转交、先核验
- `默认保守策略`：证据不足时如何降级口径
- `标准输入/交付`：让下游 Skill 能无缝 handoff
- `写作资产模板`：让输出能直接服务 intro、related work、rebuttal、submission

### 4. 每次增强都同步检查 5 个文件层

一轮 Skill 更新最好至少检查：

- `skills/<skill-name>/SKILL.md`
- `skills/INDEX.md`
- `skills/PLAYBOOK.md`
- `skills/CONVENTIONS.md`
- `README.md`

避免出现“Skill 已增强，但入口文档和路由还按旧理解调用”的问题。

### 5. 用最小闭环验证更新是否值得保留

每次更新后，至少问 4 个问题：

- 这个 Skill 的触发条件有没有更清楚
- 输出是否更容易直接复用于论文工作流
- 是否减少了和其他 Skill 的重叠
- 是否降低了幻觉、误路由或过度宣称风险

只有回答大多是“是”，这轮增强才值得保留。

### 6. 推荐的演化方向

当前这套仓库更适合继续沿以下方向增强：

- 证据闭环更强
- 文献问答更 grounded
- 阅读笔记到写作资产的转换更顺
- 投稿前风险门禁更明确
- 路由更清楚，而不是 Skill 越堆越碎

### 7. 一个简单的更新模板

每次准备增强某个 Skill，可以先写 5 行备忘：

- `现有 Skill`: 现在负责什么
- `新发现的高频需求`: 最近多了什么真实任务
- `拟采取动作`: 新增 / 合并 / 增强
- `需要同步的入口文件`: INDEX / PLAYBOOK / CONVENTIONS / README
- `验证方式`: 诊断通过 + 路由边界清楚 + 输出模板更可复用

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
