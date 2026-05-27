---
name: "paper-intake-router"
description: "识别论文任务所处阶段并路由到合适的写作、核验、审稿或投稿 Skill。Invoke when 用户给出模糊论文需求、需要自动分流、或想把多任务拆成执行链。"
---

# Paper Intake Router

这个 Skill 是整套学术投稿系统的统一入口。它不直接深做某个子任务，而是先判断用户当前处于论文生命周期的哪个阶段、当前主要风险是什么、应该调用哪个 Skill 或哪条 Skill 链。

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

## 已知可路由 Skills

- `academic-paper-factory`
- `paper-positioning`
- `claim-evidence-mapper`
- `citation-reality-guard`
- `experiment-story-builder`
- `proof-consistency-checker`
- `writing-naturalness-guard`
- `reviewer-risk-audit`
- `paper-ratchet-optimizer`
- `venue-fit-selector`
- `submission-integrity-audit`
- `latex-submission-packager`
- `venue-submission-adapter`
- `literature-survey-builder`
- `rebuttal-response-drafter`

## 输入信号

优先从用户请求中提取：

- 当前资产：idea / notes / draft / figures / theorem / experiments / `.tex/.bib`
- 当前目标：选题、写作、补实验、核引用、做 review、准备投稿
- 当前约束：venue、deadline、双盲、页数、是否已有模板
- 当前风险：新颖性、证据不足、实验叙事混乱、引用真实性、格式与匿名性

## Step 1: 判断任务阶段

将请求映射到以下阶段之一：

### Stage A: 想法阶段

信号：

- “这个 idea 能不能投”
- “帮我定题”
- “想投 NeurIPS/ICML/ICLR，怎么定位”

默认路由：

- 主 Skill: `paper-positioning`

若用户已经开始问“投哪个 venue 更合适”，追加：

- `venue-fit-selector`

### Stage B: 起稿阶段

信号：

- “帮我从零开始写论文”
- “我有一些结果，怎么组织成论文”
- “帮我搭 abstract/introduction/method 骨架”

默认路由：

- 主 Skill: `academic-paper-factory`
- 辅助 Skill: `paper-positioning`

### Stage C: 相关工作与引用阶段

信号：

- “补 related work”
- “加几篇引用”
- “核一下 bib / references”
- “确认没有幻觉引用”
- “帮我系统巡视相关领域近年工作”
- “比较我和现有工作的差异，确认 novelty gap”

默认路由：

- 若需要系统化文献矩阵与差异分析：`literature-survey-builder`
- 若需要引用真实性核验与元数据修正：`citation-reality-guard`

### Stage D: 实验叙事阶段

信号：

- “实验节怎么写”
- “这些图表怎么组织”
- “caption 怎么改更像顶会论文”
- “baseline / 消融 / 统计还缺什么”

默认路由：

- 主 Skill: `experiment-story-builder`

若重点是图表出版质量、caption 独立可读性或表格可投稿性，追加：

- `submission-integrity-audit`

### Stage E: 中后期优化阶段

信号：

- “做一轮微调”
- “优化摘要/引言/结论”
- “帮我持续打磨这篇稿子”

默认路由：

- 主 Skill: `paper-ratchet-optimizer`

### Stage E2: 语言自然化阶段

信号：

- “去 AI 味”
- “写得太像 LLM 了”
- “帮我润色得更像研究者写的”
- “humanize this section”

默认路由：

- 主 Skill: `writing-naturalness-guard`

### Stage F: 审稿视角检查阶段

信号：

- “review 一下”
- “从审稿人角度看有什么问题”
- “投稿前帮我找硬伤”

默认路由：

- 主 Skill: `reviewer-risk-audit`

若用户要显式检查“每个 claim 是否都被支撑”，追加：

- `claim-evidence-mapper`

### Stage G: 投稿封包阶段

信号：

- “准备投稿了”
- “检查 LaTeX 格式”
- “重新编译并确认 submission package”
- “检查匿名性 / appendix / checklist / references 顺序”

默认路由：

- 主 Skill: `latex-submission-packager`

若用户同时担心 checklist、ethics、code/data availability 或 figures/tables 的提交质量，追加：

- `submission-integrity-audit`

### Stage G2: venue / 平台适配阶段

信号：

- “改成某会议/期刊格式”
- “切 venue”
- “准备 OpenReview / ScholarOne / Editorial Manager 信息”
- “某会议要求下需要补哪些字段”

默认路由：

- 若用户还没定 venue 或在多个候选间比较：`venue-fit-selector`
- 若用户已明确目标 venue：`venue-submission-adapter`

### Stage H: 审稿回应阶段

信号：

- “收到审稿意见了，帮我写 rebuttal”
- “reviewer 说 novelty 不够，怎么回应”
- “准备提交 response letter”
- “审稿人的问题不知道怎么拆解和回答”

默认路由：

- 主 Skill: `rebuttal-response-drafter`

## Step 2: 判断主风险

在确定阶段后，再识别当前最高风险，并决定是否追加辅助 Skill。

### 风险 1：定位不清

信号：

- 用户不断改标题、摘要，但仍说不清贡献
- related work 很多，但差异点不明确

追加：

- `paper-positioning`

### 风险 2：引用与事实风险

信号：

- 需要新增大量引用
- 使用 2024–2026 文献
- 需要核对 arXiv / OpenReview / venue / 年份 / 作者

追加：

- `citation-reality-guard`

### 风险 3：实验证据链不足

信号：

- 结果图很多，但不知道怎么讲
- 需要补 baseline / 消融 / caption / CI / seeds

追加：

- `experiment-story-builder`

### 风险 3.5：理论口径不稳

信号：

- theorem / appendix proof 不一致
- 需要补 detailed proof
- 用户怀疑证明有跳步、缺假设或 claim 过强

追加：

- `proof-consistency-checker`

### 风险 3.8：claim 与证据错位

信号：

- abstract / intro / conclusion 的表述比正文更强
- contribution bullets 说得很满，但找不到对应 theorem / figure / table / citation
- 用户明确要求“做 claim-evidence matrix”

追加：

- `claim-evidence-mapper`

### 风险 4：强表述过头

信号：

- 出现 `first / unified / optimal / definitive`
- 结论明显超出实验或理论边界

追加：

- `paper-ratchet-optimizer`
- 必要时再加 `reviewer-risk-audit`

### 风险 5：最终提交踩雷

信号：

- 接近 deadline
- 用户开始提编译、页数、匿名性、checklist、supplementary

追加：

- `latex-submission-packager`
- 必要时追加 `submission-integrity-audit`

### 风险 6：AI 写作风格检测风险

信号：

- 稿件读起来句子结构过于规整，每段模式雷同
- 大量使用 "Furthermore / Moreover / Additionally / Notably" 等过渡词
- Abstract 或 Conclusion 具有明显的模板化三段结构
- 所有段落的长度和复杂度高度均匀
- 用户或合作者反馈"读起来像 AI 写的"

追加：

- `writing-naturalness-guard`
- 必要时再加 `paper-ratchet-optimizer`（以语言风格自然化为本轮优化目标）

### 风险 7：文献覆盖与新颖性风险

信号：

- related work 只覆盖了少数几篇近期工作
- 不清楚自己和近两年文献的具体差异
- 需要系统化文献矩阵和差异表

追加：

- `literature-survey-builder`

### 风险 7.5：venue 选择失配风险

信号：

- 不确定该投会议还是期刊
- 在多个 venue 之间摇摆
- 被拒后想找转投路线

追加：

- `venue-fit-selector`

### 风险 8：审稿回应风险

信号：

- 已经收到审稿意见
- 需要拆解 reviewer 的问题并制定回应策略
- 准备写 response letter

追加：

- `rebuttal-response-drafter`

## Step 3: 生成路由决策

默认只给出一种主执行路线，避免同时并发太多方向。

### 单 Skill 路由

适合：

- 用户意图单一
- 当前瓶颈很明确

输出格式：

- `判定阶段`
- `主风险`
- `推荐主 Skill`
- `为什么先做这个`

### Skill 链路由

适合：

- 用户请求跨多个阶段
- 当前问题必须按顺序拆开处理

输出格式：

- `判定阶段`
- `主风险`
- `执行链`
- `每一步目标`
- `为什么这个顺序最稳`

## 默认链路模板

### 从 idea 到投稿

`paper-positioning -> literature-survey-builder -> academic-paper-factory -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> reviewer-risk-audit -> venue-fit-selector -> submission-integrity-audit -> latex-submission-packager`

### 中途接手一篇已有草稿

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> paper-ratchet-optimizer`

### 投稿前最后 48 小时

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

### 投稿后审稿回应

`paper-intake-router -> rebuttal-response-drafter`

## 冲突处理规则

- 若用户同时要“写新内容”和“投稿前封包”，先判断 deadline 是否临近
- 若 deadline 很近，优先 `latex-submission-packager` 和 `reviewer-risk-audit`
- 若草稿还不成形，不要过早进入 submission packaging
- 若引用真实性未确认，不要先做 related work 的强结论润色

## 禁忌

- 不要把所有 Skill 都一次性调起
- 不要把“模糊需求”直接当成“需要润色”
- 不要在未识别阶段前直接改稿
- 不要忽略用户的 venue、deadline、双盲和是否已有 `.tex/.bib`

## 默认输出模板

- `任务判定`
- `当前阶段`
- `最高优先级风险`
- `推荐调用`
- `执行顺序`
- `暂不建议现在做的事`

## 与其他 Skills 的关系

- 本 Skill 是统一入口，不替代其他 Skill
- 路由后应把具体执行交给对应专用 Skill
- 若用户已经明确指定 Skill，则无需再二次路由

## 标准输入

- 用户原始请求
- 当前论文资产：idea / draft / figures / experiments / `.tex/.bib`
- 目标 venue、年份、deadline
- 用户显式指定的优先级或禁区

## 标准交付

- 任务判定
- 当前阶段
- 最高优先级风险
- 推荐主 Skill 或执行链
- 为什么按这个顺序处理
- 暂不建议现在做的事

## Examples

- “继续推进这篇论文，你自己判断现在先做什么。”
- “我有草稿、实验和一个 bib，帮我分步骤推进到投稿。”
- “现在时间不多了，帮我判断应该先补实验、先核引用还是先做投稿检查。”
