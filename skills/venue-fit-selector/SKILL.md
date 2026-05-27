---
name: "venue-fit-selector"
description: "根据稿件成熟度、贡献类型、篇幅与风险选择最合适的会议或期刊。Invoke when 用户还没定 venue、想转投、或要比较多个 venue 的匹配度与 desk reject 风险。"
---

# Venue Fit Selector

这个 Skill 负责在“已经知道怎么写论文”和“已经确定某个 venue 格式”之间，补上一层经常被忽略但非常关键的决策：到底投哪里更合适。

它关注的是 `scope fit + novelty threshold + evidence maturity + rewrite cost + timeline`，而不是模板或投稿系统字段本身。

## 何时调用

- 用户说“这篇更适合投哪个 venue / journal”
- 用户还没确定会议还是期刊
- 用户要在多个候选 venue 之间做取舍
- 用户被拒后想判断转投路线
- 用户担心 desk reject、scope mismatch、篇幅不合适或稿件成熟度不够

## 核心目标

1. 给出基于稿件现状的 venue shortlist，而不是泛泛推荐顶会
2. 区分 scope 不匹配、成熟度不足、证据不足和格式成本四类问题
3. 输出主投、备投和不建议投的 venue 选择理由
4. 为后续 `venue-submission-adapter` 提供清晰的目标 venue profile

## 强制规则

- 不得虚构 venue 要求、deadline、页数或投稿政策
- 不得只因 venue 名气高就推荐，不顾稿件成熟度
- 不得把“适合改成某 venue 格式”误当成“适合投某 venue”
- 对年份、政策或是否双盲等不确定信息，必须标记需核验

## 执行步骤

### Step 1: 建立稿件画像

先明确：

- 论文类型：理论 / 方法 / 系统 / 实证 / survey / interdisciplinary
- 当前成熟度：idea / partial results / full draft / near submission
- 证据形态：theorem、benchmark、user study、system demo、case study
- 当前篇幅与图表负担
- 时间约束：最近 deadline、是否能接受长周期 journal

### Step 2: 建候选 venue 池

候选池至少按三类划分：

- 主目标：最希望命中的 venue
- 稳妥备选：要求略低但学术画像匹配
- 不建议路线：scope、成熟度或叙事方式明显不合适

### Step 3: 做 fit matrix

建议至少按以下维度打分或排序：

- `Scope Fit`
- `Novelty Threshold`
- `Evidence Readiness`
- `Length / Format Pressure`
- `Rewrite Cost`
- `Review Style Risk`
- `Timeline Fit`

### Step 4: 标记主要风险

常见风险：

- `P0`：scope 明显不匹配
- `P1`：新颖性或证据强度达不到目标 venue 常见门槛
- `P1`：篇幅、结构或叙事方式会导致改稿成本很高
- `P2`：venue 可投，但不是当前资产的最佳利用方式

### Step 5: 给出投递策略

默认应输出：

- 主投 venue
- 备投 venue 1-2 个
- 当前不建议投的方向
- 若切换 venue，需要先修什么，再交给哪个 Skill

## 默认输出格式

- `Paper Profile`
- `Venue Shortlist`
- `Primary Recommendation`
- `Backup Route`
- `Why Not the Others`
- `Next Skill Handoff`

## 标准输入

- 论文摘要、主线或标题
- 论文类型与当前成熟度
- 已有资产：实验、定理、系统、数据、草稿、页数
- 候选 venue 列表，若用户已有偏好
- 时间约束：deadline、是否接受 journal 周期

## 标准交付

- venue fit matrix
- 主投 / 备投建议
- 主要 desk reject 或 mismatch 风险
- 改投成本评估
- 后续 handoff：`paper-positioning` / `venue-submission-adapter` / `submission-integrity-audit`

## 默认保守策略

- 若稿件定位仍不清，先回调 `paper-positioning`
- 若 claim 和证据还不稳定，先回调 `claim-evidence-mapper`
- 若用户只给出模糊想法，不要过早做过细 venue 排序

## 与其他 Skills 的关系

- 选题主线来自 `paper-positioning`
- 证据成熟度评估可串联 `claim-evidence-mapper`
- 确定目标 venue 后，交给 `venue-submission-adapter`
- 投稿语义合规与材料一致性交给 `submission-integrity-audit`
- 总流程由 `academic-paper-factory` 统筹

## Examples

- “这篇方法论文更适合冲 NeurIPS，还是转成期刊路线更稳？”
- “我现在有定理和初步实验，帮我选一个最合适的 venue shortlist。”
- “被拒后准备转投，帮我比较 ICML 风格路线和 TPAMI 风格路线的改稿成本。”
