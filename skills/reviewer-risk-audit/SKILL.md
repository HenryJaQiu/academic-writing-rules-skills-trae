---
name: "reviewer-risk-audit"
description: "从严苛审稿人视角审计新颖性、证据、写作、格式与投稿风险。Invoke when 用户要做审稿视角检查、重大修改后复审、或投稿前终审。"
---

# Reviewer Risk Audit

这个 Skill 模拟“顶会/顶刊严苛审稿人 + Area Chair”的阅读方式，优先识别会导致拒稿、弱拒或低置信度评分的问题。

## 何时调用

- 用户说“review 一下”
- 草稿已经基本成形
- 做完一轮较大修改后
- 准备 final polish / final submission
- 用户希望从审稿人视角找风险而不是润色语法

## 审计原则

1. Findings first：先报问题，再讲优点。
2. 优先级明确：P0 硬伤 > P1 高风险 > P2 可优化。
3. 不只看措辞，要看证据闭环。
4. 不只看正文，要看图表、appendix、checklist、references 顺序和 submission package。

## 必查维度

### 1. 新颖性与定位

- 是否清楚覆盖近两年相关工作
- 是否真的有明确增量
- 是否有“换名词但没增量”的风险
- 是否把目标不同的工作错误地当作被超越对象

### 2. 证据与主张一致性

- Abstract / Introduction / Conclusion 的每一条主张是否被正文支撑
- Theory 是否与 proof sketch / appendix 一致
- Experiments 是否真的支撑主结论
- 是否把存在性结果、经验观察、局部结果写成普适结论

### 3. 理论与数学风险

- 假设是否前置且集中
- 定理与证明口径是否一致
- 经典结果是否被错误包装成本文贡献
- 符号、条件、常数依赖、对数因子是否自洽

### 4. 实验与图表风险

- baseline 是否公平
- seeds、均值、置信区间是否规范
- caption 是否能复现实验设置
- 图内标题、caption、正文数字是否一致
- 黑白可读性、legend 遮挡、表格溢出是否影响审稿体验

### 5. 投稿格式风险

- 页数是否合规
- references / appendix / checklist 顺序是否正确
- 匿名性是否完整
- supplementary、reproducibility、code availability 说法是否真实

### 6. 章节结构与实验分布

- Introduction 是否嵌入 Notation/Definitions（禁止）
- 实验内容是否过多推给 Appendix 而正文仅有少量结果
- 引用数量是否 ≥20 篇（正文引用）
- Related Work 是否覆盖经典奠基性工作和近两年关键工作
- 每段 Related Work 末尾是否明确与本文的区别
- 正文页数是否超过 venue 限制（NeurIPS: ≤9 页含图表）
- `\bibliographystyle` 是否为 `unsrt`（按首次出现顺序，非字母序）

## 默认输出格式

必须按以下顺序给出：

- `Findings`
- `Open Questions / Assumptions`
- `Change Summary`
- `Residual Risks`

其中 `Findings` 应按严重程度排序，优先列出：

- 会被 reviewer 一眼抓住的硬伤
- 会导致“claim too strong”的地方
- 会导致“not sufficiently supported”的地方

## 建议标签

- `P0`: 必修，否则高概率被打低分
- `P1`: 高价值修复，通常影响 overall confidence
- `P2`: 可选优化，改善观感或可读性

## 常见高风险信号

- 标题太大，正文边界太小
- Abstract 说“first/unified/optimal”，正文只给了局部设置
- Related Work 没覆盖 2024–2026
- 图表中的数字和正文/caption 对不上
- 结论把 controlled setting 外推到现代大模型或真实系统
- checklist 说有代码，但没有匿名仓库或 supplementary 说明

## 推荐审计节奏

- 主文骨架完成后做一次
- 相关工作和实验补齐后再做一次
- 投稿前最后一天再做一次 submission package audit

## 与其他 Skills 的关系

- 若问题出在定位，回调 `paper-positioning`
- 若问题出在引用，回调 `citation-reality-guard`
- 若需要持续迭代修复，回调 `paper-ratchet-optimizer`

## 标准输入

- 当前草稿或关键 section
- 目标 venue 与年份
- 是否已具备 appendix / checklist / supplementary
- 用户最担心的风险：新颖性 / 实验 / 理论 / 格式

## 标准交付

- Findings，按 P0 / P1 / P2 排序
- Open Questions / Assumptions
- Change Summary
- Residual Risks

## Examples

- “从严苛 reviewer 视角 review 一下这篇稿子，优先找硬伤。”
- “不要润色语法，只告诉我哪些问题会导致弱拒或低分。”
- “投稿前帮我做一次 final risk audit，尤其看 novelty 和 evidence。”
