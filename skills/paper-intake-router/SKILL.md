---
name: "paper-intake-router"
description: "识别论文任务所处阶段并路由到合适的写作、核验、审稿或投稿 Skill。Invoke when 用户给出模糊论文需求、需要自动分流、或想把多任务拆成执行链。"
---

# Paper Intake Router

这个 Skill 是整套学术投稿系统的统一入口。它不直接深做某个子任务，而是先判断论文处于哪个阶段、最高风险是什么，以及下一步最合适的单 Skill 或 Skill 链。

## 何时调用

- 用户请求比较模糊，例如“帮我推进这篇论文”“从这一步继续”“看看接下来该做什么”
- 用户需求同时涉及多个阶段，例如既要补 related work，又要收紧 claims，还要检查投稿格式
- 用户没有明确说该用哪个 Skill，但希望高效推进论文
- 用户希望以后把整套系统作为统一学术投稿入口

## 核心目标

1. 判断论文当前阶段
2. 识别当前最高优先级风险
3. 选择最合适的单个 Skill 或组合调用链
4. 生成清晰的执行顺序，而不是同时做太多不相干的事

## 快速输入信号

- 当前资产：idea / notes / draft / figures / theorem / experiments / `.tex/.bib`
- 当前目标：选题、写作、补实验、核引用、做 review、准备投稿
- 当前约束：venue、deadline、双盲、页数、是否已有模板
- 当前风险：新颖性、证据不足、实验叙事混乱、引用真实性、格式与匿名性

## 路由总规则

1. 先判阶段，再判主风险
2. 默认只给一个主 Skill；确有必要时再给一条短链
3. 若用户已经明确指定某个 Skill，不再重复路由
4. 若 deadline 很近，优先处理 submission 与 reviewer risk，而不是重写大段正文

## 阶段路由

### Stage A: 想法与定位

- 信号：能不能投、怎么定题、怎么定位
- 主 Skill：`paper-positioning`
- 常见追加：`venue-fit-selector`

### Stage B: 从零起稿或搭骨架

- 信号：从零写论文、组织已有结果、搭摘要/引言/方法骨架
- 主 Skill：`academic-paper-factory`
- 常见追加：`paper-positioning`

### Stage C: 文献与引用

- 信号：补 related work、补引用、核 bib、系统看近年工作
- 主 Skill：
- 若偏指定论文问答、事实核查、只基于给定材料回答：`grounded-paper-qa`
- 若偏把读书笔记、摘录、批注整理成写作素材：`reading-note-synthesizer`
- 若偏文献矩阵与差异分析：`literature-survey-builder`
- 若偏真实性核验与元数据修正：`citation-reality-guard`

### Stage D: 实验与图表叙事

- 信号：实验节怎么写、图表怎么组织、caption 怎么改、baseline/消融/统计缺什么
- 主 Skill：`experiment-story-builder`
- 常见追加：`submission-integrity-audit`

### Stage E: 中后期打磨

- 信号：做一轮微调、优化摘要/引言/结论、持续打磨稿件
- 主 Skill：`paper-ratchet-optimizer`
- 常见追加：`writing-naturalness-guard`

### Stage E2: 去 AI 味与残留清理

- 信号：去 AI 味、humanize、检查 ChatGPT 残留、清理奇怪的生成痕迹
- 主 Skill：`writing-naturalness-guard`
- 默认理解：这不是单纯润色，而是三联任务
- 文字自然化
- 提示语 / 元编辑残留检查
- 幻觉引用、错引与明显合规风险初筛
- 常见追加：
- 若命中引用真实性风险：`citation-reality-guard`
- 若命中 disclosure、图表或提交材料一致性风险：`submission-integrity-audit`

### Stage F: 审稿视角检查

- 信号：review 一下、从审稿人角度看、投稿前找硬伤
- 主 Skill：`reviewer-risk-audit`
- 常见追加：`claim-evidence-mapper`

### Stage G: 投稿与适配

- 信号：准备投稿、检查 LaTeX、submission package、匿名性、checklist
- 主 Skill：
- 若还没定 venue：`venue-fit-selector`
- 若要做 venue-specific 改造：`venue-submission-adapter`
- 若要检查提交包：`latex-submission-packager`
- 若担心图表、disclosure、一致性：`submission-integrity-audit`

### Stage H: 审稿回应

- 信号：收到 reviews、写 rebuttal、写 response letter
- 主 Skill：`rebuttal-response-drafter`

## 风险加挂规则

- 定位不清：`paper-positioning`
- 引用或事实风险：`citation-reality-guard`
- 指定论文问答或事实核查：`grounded-paper-qa`
- 笔记散乱但已有大量阅读资产：`reading-note-synthesizer`
- 实验证据链不足：`experiment-story-builder`
- 理论口径不稳：`proof-consistency-checker`
- claim 大于证据：`claim-evidence-mapper`
- 强表述过头：`paper-ratchet-optimizer`
- 最终提交踩雷：`latex-submission-packager`
- venue 选择失配：`venue-fit-selector`
- AI 味过重或疑似存在提示语残留：`writing-naturalness-guard`
- 去 AI 味过程中发现幻觉引用或错引：`citation-reality-guard`

## 默认链路模板

### 从 idea 到投稿

`paper-positioning -> literature-survey-builder -> academic-paper-factory -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> reviewer-risk-audit -> venue-fit-selector -> submission-integrity-audit -> latex-submission-packager`

### 中途接手一篇已有草稿

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> paper-ratchet-optimizer`

### 先基于一组论文问清事实，再进入写作

`paper-intake-router -> grounded-paper-qa -> literature-survey-builder -> paper-positioning`

### 已有很多读书笔记，但还没变成论文素材

`paper-intake-router -> reading-note-synthesizer -> literature-survey-builder -> claim-evidence-mapper`

### 去 AI 味但要同时查残留与幻觉问题

`paper-intake-router -> writing-naturalness-guard -> citation-reality-guard -> reviewer-risk-audit`

### 投稿前最后 48 小时

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

### 投稿后审稿回应

`paper-intake-router -> rebuttal-response-drafter`

## 标准输入

- 用户原始请求
- 当前论文资产：idea / draft / figures / experiments / `.tex/.bib`
- 目标 venue、年份、deadline
- 用户显式指定的优先级或禁区

## 标准交付

- `任务判定`
- `当前阶段`
- `最高优先级风险`
- `推荐调用`
- `执行顺序`
- `暂不建议现在做的事`

## Examples

- “继续推进这篇论文，你自己判断现在先做什么。”
- “我有草稿、实验和一个 bib，帮我分步骤推进到投稿。”
- “现在时间不多了，帮我判断应该先补实验、先核引用还是先做投稿检查。”
