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

## 默认工作流

### Phase 0: 任务建模

收集以下信息：

- 目标 venue 与年份
- 论文类型：理论 / 方法 / 实验 / benchmark / survey / interdisciplinary
- 当前资产：idea、notes、code、figures、theorems、草稿、`.tex/.bib`
- 最大风险：新颖性、证据不足、实验不稳、引用不实、篇幅失控、格式不符

输出：

- 一页 paper brief
- 论文主线一句话
- 风险优先级列表

若 venue 未定，先调用 `venue-fit-selector`。

### Phase 1: 定位与最小贡献集

调用 `paper-positioning`，明确：

- 问题定义
- 与近两年工作的差异点
- 最小可投稿贡献集
- 不该宣称的内容

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

### Phase 4: 风险审计与迭代优化

默认串联：

- `reviewer-risk-audit`
- `paper-ratchet-optimizer`

必要时追加：

- 语言模板感明显：`writing-naturalness-guard`
- 图表、disclosure、supplementary 风险突出：`submission-integrity-audit`

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

## 禁忌

- 不要在未核验引用真实性前写死关键 related work 论断
- 不要在主文未成形时沉迷局部措辞抛光
- 不要把"实验现象"写成"理论保证"
- 不要把"存在性结果"写成"对现代大模型已被严格证明"
- 不要用虚假的代码可用性、虚假的匿名仓库、虚假的 supplementary 声明
- 不要直接把未经改写的大段 LLM 输出放进最终正文

## 推荐搭配

- 统一入口：`paper-intake-router`
- 定位：`paper-positioning`
- 文献与差异：`literature-survey-builder`
- 指定论文问答：`grounded-paper-qa`
- 读书笔记整理：`reading-note-synthesizer`
- claim 映射：`claim-evidence-mapper`
- 引用核验：`citation-reality-guard`
- 审稿风险：`reviewer-risk-audit`
- 迭代优化：`paper-ratchet-optimizer`
- 实验叙事：`experiment-story-builder`
- 理论核查：`proof-consistency-checker`
- 风格自然化：`writing-naturalness-guard`
- venue 决策：`venue-fit-selector`
- 投稿一致性：`submission-integrity-audit`
- 最终打包：`latex-submission-packager`
- rebuttal：`rebuttal-response-drafter`

## 组合调用模板

### 从零开始

`paper-intake-router -> academic-paper-factory -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> venue-fit-selector -> reviewer-risk-audit -> submission-integrity-audit -> latex-submission-packager`

### 先问清文献再写

`paper-intake-router -> academic-paper-factory -> grounded-paper-qa -> literature-survey-builder -> paper-positioning -> claim-evidence-mapper`

### 中期打磨

`paper-intake-router -> academic-paper-factory -> literature-survey-builder -> claim-evidence-mapper -> paper-ratchet-optimizer -> citation-reality-guard -> reviewer-risk-audit`

### 投稿前终审

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

## 标准输入

- 用户原始目标：从零开始 / 中期推进 / 投稿前终审
- 当前资产：idea、draft、figures、theorems、experiments、`.tex/.bib`
- 目标 venue 与年份
- 约束：deadline、页数、双盲、是否已有模板
- 当前最担心的风险

## 标准交付

- 当前阶段判定
- 总体执行链
- P0 / P1 / P2 风险图
- 本轮最小行动集
- 需要调用的下游 Skill
- 明确哪些内容已经可信，哪些只能保守表述

## Examples

- “帮我从零开始做一篇能投顶会的 AI 论文。”
- “我已经有一版草稿和一些实验，帮我系统推进到 submission-ready。”
- “不要只改句子，帮我像一个论文工厂一样组织后续所有子任务。”
