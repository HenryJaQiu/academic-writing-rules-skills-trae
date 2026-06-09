---
name: "academic-paper-factory"
description: "统筹 AI 论文从选题到投稿的全流程。Invoke when 用户要从零开始写论文、推进中期草稿、或组织多项论文写作子任务。"
---

# Academic Paper Factory

面向人工智能领域期刊/会议投稿的总控 Skill。它负责把论文推进工作拆成稳定阶段，并把具体执行交给合适的下游 Skill，而不是自己包办所有细节。

## 适用场景

- 用户说“从零开始写一篇论文”
- 用户只有一个研究想法，想把它落成可投稿项目
- 用户已经有实验/定理/草稿，想系统推进到投稿
- 用户想跨项目复用一套稳定、低风险的学术写作流程

## 核心原则

1. 先定投稿目标，再定论文结构，最后才是局部润色。
2. 先做"可审稿的完整版本"，再做迭代优化，避免无限打磨局部。
3. 所有强主张都必须有证据承接：定理、实验、消融、或权威引用。
4. 所有引用都必须是真实存在的学术文献，且元数据要正确。
5. 每一轮迭代都采用 Darwin 风格棘轮：评估 -> 修改 -> 验证 -> 只保留可测量改进。
6. 最终稿需要做语言自然化检查，避免模板化和明显 AI 味。

## paper type 使用护栏

- `paper type` 的作用是校准定位、实验、审稿和修复口径，不是允许把论文主线随意改成另一种写法
- 默认保守先验是技术类主线；只要主贡献是方法、机制、理论或系统能力，就继续按 `technical-paper` 组织整篇论文
- 只有当 benchmark、evaluation protocol、diagnostic asset 或社区评估修正本身就是第一贡献时，才切到 `benchmark/evaluation` 主线
- “实验做得很多”“覆盖很多数据集”“附带一个新 protocol” 都不足以单独触发 `benchmark/evaluation` 分型
- 混合型论文优先看主 claim：若 reviewer 最终应该问“你的方法到底新在哪、证据是否够”，就仍按技术类推进
- 若分型不清，先在 `paper-positioning` 里锁定主贡献，再让后续流程沿用同一个判断，避免一会儿像技术类、一会儿像 benchmark 类

## 工作模式

### `full-orchestration`

默认模式，适合需要从 idea、draft 或实验资产一路推进到投稿的人：

- 给出完整阶段链
- 明确每个阶段的下游 Skill
- 保持“先成稿、再审计、再修复、再终审”的顺序

### `checkpoint-dashboard`

适合用户说“先看看下一步做什么”“先别一口气跑完整条链”时：

- 先停在当前最合适的 checkpoint
- 只输出阶段判定、风险图、下一步最小行动集
- 不默认静默展开整条 pipeline
- 更适合中期接手稿件、时间紧、或用户希望逐 checkpoint 决策

## 默认工作流

### Phase 0: 任务建模

收集以下信息：

- 目标 venue 与年份
- 论文类型：理论 / 方法 / 实验 / benchmark / survey / interdisciplinary
- 当前资产：idea、notes、code、figures、theorems、草稿、`.tex/.bib`
- 最大风险：新颖性、证据不足、实验不稳、引用不实、篇幅失控、格式不符

分型时额外判断：

- 第一贡献到底是“新方法/新理论/新系统能力”，还是“新 benchmark/新 protocol/新评价资产”
- 如果删掉 benchmark 或更丰富的评测，论文主线是否仍成立；若仍成立，通常继续按技术类处理
- 如果没有 benchmark/protocol 这部分，论文是否几乎失去核心贡献；若是，才考虑 `benchmark/evaluation`

输出：

- 一页 paper brief
- 论文主线一句话
- 风险优先级列表
- `paper type` 判定或 `待判断`

若走 `checkpoint-dashboard`，此处默认停下并交付：

- `Pipeline Dashboard`
- `Current Stage`
- `Top 3 Risks`
- `Next Best Skill`
- `Do Not Do Yet`

若 venue 未定，先调用 `venue-fit-selector`。

### Phase 1: 定位与最小贡献集

若用户还在问“这个题值不值得做/先做哪个/更像主线还是 side project”，可先前插：

- `idea-evaluator`

调用 `paper-positioning`，明确：

- 问题定义
- 与近两年工作的差异点
- 最小可投稿贡献集
- 不该宣称的内容

此阶段必须额外锁定：

- 本文最终按 `technical-paper` 还是 `benchmark/evaluation` 作为主线写
- 若是技术类，benchmark/diagnostic 内容只是证据增强还是独立贡献
- 若是 benchmark/evaluation 类，不能把整理性工作包装成方法创新

### Phase 2: 结构搭建

默认骨架：

1. 标题与一句话摘要
2. Abstract
3. Introduction
4. Related Work / Positioning
5. Method / Theory
6. Experiments / Proof Sketch
7. Discussion / Limitations
8. Conclusion
9. References
10. Appendix / Checklist / Supplementary

目标：先形成“可审稿完整稿”，而不是过早沉迷局部润色。

checkpoint 建议：

- 若用户还没有稳定 outline、section contract 或 contribution-to-section 映射，优先停在这里，不要过早进入润色或 reviewer mode

### Phase 3: 文献、证据与事实闭环

若用户先要“根据指定论文回答问题”或“核文献到底说了什么”，可前插：

- `grounded-paper-qa`

若用户已有大量读书笔记、摘录或会议记录，但还没整理成写作素材，可前插：

- `reading-note-synthesizer`

按顺序串联：

- `literature-survey-builder`
- `claim-evidence-mapper`
- `citation-reality-guard`

必要时追加：

- 理论稿：`proof-consistency-checker`
- 实验稿：`experiment-story-builder`
- 若主图/方法总览图还不清楚：`figure-design-advisor`

类型护栏：

- 技术类论文调用 `experiment-story-builder` 时，优先追问方法贡献、最强 baseline、公平预算与 ablation
- 只有论文主贡献是评估资产时，才让 `experiment-story-builder` 进入 `benchmark-evaluation mode`

checkpoint 建议：

- 若 claim-evidence 还没闭环，优先停在本阶段，先补证据缺口，不要提前做 final polish

### Phase 4: 风险审计与迭代优化

默认串联：

- `reviewer-risk-audit`
- `paper-ratchet-optimizer`

必要时追加：

- 语言模板感明显：`writing-naturalness-guard`
- 图表、disclosure、supplementary 风险突出：`submission-integrity-audit`

类型护栏：

- 若主线是技术类，`reviewer-risk-audit` 与 `paper-ratchet-optimizer` 默认继续沿用技术类口径，不因 benchmark 内容丰富而改判
- 若审稿意见只是在要求补 coverage 或 subgroup 分析，但不改变主贡献，修复时仍应把“方法是否成立”放在更高优先级

checkpoint 建议：

- 若用户给的是 reviewer comments、Findings 或一版基本成形的稿件，可直接从本阶段启动，而不是强制回到 Phase 0

### Phase 5: venue 适配与最终提交

若需要比较主投与备投路线：

- `venue-fit-selector`

若目标 venue 已明确：

- `venue-submission-adapter`

提交前终审：

- `submission-integrity-audit`
- `latex-submission-packager`

## 交付物模板

每次执行本 Skill，尽量输出：

- 当前阶段判断
- 风险列表（P0/P1/P2）
- 下一步最小行动集
- 需要新建/修改的文件
- 哪些结论已经可靠，哪些只能保守表述

若走 `checkpoint-dashboard`，优先输出：

- `Pipeline Dashboard`
- `Current Stage`
- `Top Risks`
- `Recommended Next Skill`
- `Minimal Next Actions`
- `Stop Here or Continue`

## 禁忌

- 不要在未核验引用真实性前写死关键 related work 论断
- 不要在主文未成形时沉迷局部措辞抛光
- 不要把"实验现象"写成"理论保证"
- 不要把"存在性结果"写成"对现代大模型已被严格证明"
- 不要用虚假的代码可用性、虚假的匿名仓库、虚假的 supplementary 声明
- 不要直接把未经改写的大段 LLM 输出放进最终正文
- 不要因为“benchmark/evaluation”标签更时髦，就把本质上的技术类论文改写成评测资产论文
- 不要因为实验很多，就弱化技术类论文应承担的方法增量、baseline 公平性和 ablation 责任

## 推荐搭配

- 统一入口：`paper-intake-router`
- 定位：`paper-positioning`
- idea 预筛：`idea-evaluator`
- 文献与差异：`literature-survey-builder`
- 指定论文问答：`grounded-paper-qa`
- 读书笔记整理：`reading-note-synthesizer`
- claim 映射：`claim-evidence-mapper`
- 引用核验：`citation-reality-guard`
- 审稿风险：`reviewer-risk-audit`
- 迭代优化：`paper-ratchet-optimizer`
- 实验叙事：`experiment-story-builder`
- 核心图设计：`figure-design-advisor`
- 理论核查：`proof-consistency-checker`
- 风格自然化：`writing-naturalness-guard`
- venue 决策：`venue-fit-selector`
- 投稿一致性：`submission-integrity-audit`
- 最终打包：`latex-submission-packager`
- rebuttal：`rebuttal-response-drafter`

## 组合调用模板

### 从零开始

`paper-intake-router -> academic-paper-factory -> idea-evaluator -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> figure-design-advisor -> experiment-story-builder -> citation-reality-guard -> venue-fit-selector -> reviewer-risk-audit -> submission-integrity-audit -> latex-submission-packager`

### 先问清文献再写

`paper-intake-router -> academic-paper-factory -> grounded-paper-qa -> literature-survey-builder -> paper-positioning -> claim-evidence-mapper`

### 中期打磨

`paper-intake-router -> academic-paper-factory -> literature-survey-builder -> claim-evidence-mapper -> paper-ratchet-optimizer -> citation-reality-guard -> reviewer-risk-audit`

### 先做阶段 dashboard，不直接跑完整流程

`paper-intake-router -> academic-paper-factory`

### 投稿前终审

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

## 标准输入

- 用户原始目标：从零开始 / 中期推进 / 投稿前终审
- 当前资产：idea、draft、figures、theorems、experiments、`.tex/.bib`
- 目标 venue 与年份
- 约束：deadline、页数、双盲、是否已有模板
- 当前最担心的风险
- 若已知，论文主贡献类型；若未知则要求先判定或标记 `待判断`

## 标准交付

- 当前阶段判定
- `paper type` 判定与理由
- 总体执行链
- P0 / P1 / P2 风险图
- 本轮最小行动集
- 需要调用的下游 Skill
- 明确哪些内容已经可信，哪些只能保守表述

## Examples

- “帮我从零开始做一篇能投顶会的 AI 论文。”
- “我已经有一版草稿和一些实验，帮我系统推进到 submission-ready。”
- “不要只改句子，帮我像一个论文工厂一样组织后续所有子任务。”
- “先别全跑，先给我一个 checkpoint dashboard，告诉我现在最该做什么。”
