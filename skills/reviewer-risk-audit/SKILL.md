---
name: "reviewer-risk-audit"
description: "从严苛审稿人视角审计新颖性、证据、写作、格式与投稿风险。Invoke when 用户要做审稿视角检查、重大修改后复审、或投稿前终审。"
---

# Reviewer Risk Audit

这个 Skill 模拟“顶会/顶刊严苛审稿人 + Area Chair”的阅读方式，优先识别会导致拒稿、弱拒或低置信度评分的问题。

它吸收了近期较受欢迎的论文评审/打分 skill 中最有价值的做法，但保持为轻量、可复用、单 Skill 协议：

- 用 `finding protocol` 约束每条批评，避免空泛评论
- 用 `score + confidence + decision` 三联输出，避免只给模糊感觉
- 支持 venue-aware 的评分口径，但不把整个 Skill 拆成大量 venue 子目录

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
5. 每条关键批评都应尽量满足 `位置 + 证据 + 原因 + 修复方向`。
6. 结论要同时给出 `score`、`confidence` 和 `go/no-go`，避免只报情绪化判断。

## 审计模式

### `quick-screen`

适合时间有限或草稿还不完整时：

- 快速找会导致拒稿的前 5-10 个关键问题
- 给出粗粒度 score 和 go/no-go 建议

### `full-review`

适合投稿前主审查：

- 覆盖 novelty、evidence、theory、experiments、writing、format、submission package
- 输出结构化 findings、scorecard、decision 和 fix priority

### `re-review`

适合重大修改后复审：

- 只追踪上轮问题是否关闭
- 查是否引入新回归
- 更新 score、confidence 和 residual risks

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

### 7. 评分与决策一致性

- Findings 的严重程度是否真的支持当前 recommendation
- 是否因为写作流畅而高估技术质量
- 是否因为单一硬伤而忽略整体可修复性
- confidence 是否与证据完整度匹配
- venue 标准是否被正确理解，而不是拿理想化标准拍死稿件

## Finding Protocol

每条 `P0/P1` finding 尽量写成四段：

- `Location`：问题出现在哪一节、哪一类内容
- `Evidence`：文本、图表、claim、结构或缺失项的直接证据
- `Why it matters`：为什么这会导致低分、弱拒或 reviewer 不信
- `Fix direction`：最短路径修什么，不要只批评不指路

若拿不到精确行号，至少要指出 section、subsection、figure/table 或 claim 类型。

## 评分协议

### A. 维度分

默认给 6 个维度，各 `0-100`：

- `Novelty / Positioning`
- `Technical Correctness`
- `Evidence / Experiments`
- `Clarity / Organization`
- `Reproducibility / Integrity`
- `Venue Fit`

### B. 总分解释

- `80-100`：可投，通常对应 `Accept / Minor Revision`
- `65-79`：有机会，但仍有明显短板，通常对应 `Borderline / Major Revision`
- `50-64`：高风险，通常对应 `Weak Reject / Major Revision`
- `<50`：当前版本不建议投，通常对应 `Reject / No-Go`

总分不是简单数学平均，而是要受 `P0` 硬伤约束。

### C. Confidence

给 `1-5`：

- `1`：材料不完整，很多判断只能粗估
- `2`：读到了主要部分，但缺 appendix / supplementary / figures 细节
- `3`：有较完整证据，结论中等可信
- `4`：证据充分，判断较稳
- `5`：全文、附录、图表、格式和提交材料都审过，判断很稳

### D. Decision

统一映射到三档：

- `GO`
- `CONDITIONAL GO`
- `NO-GO`

推荐判断：

- 有 `P0` 未解决，默认 `NO-GO`
- 无 `P0`，但存在 2 个以上影响接收概率的 `P1`，通常 `CONDITIONAL GO`
- 关键风险已收敛，且 score 与 confidence 都不低，才给 `GO`

## Venue-Aware Calibration

若目标 venue 已知，评审时应显式校准：

- conference vs journal
- theory vs systems vs empirical
- 匿名、checklist、artifact、broader impact 等 venue-specific 要求

但不要因为 venue 未知就停止审查；未知时默认按“顶会/高水平期刊审稿人”口径执行。

## 默认输出格式

必须按以下顺序给出：

- `Findings`
- `Scorecard`
- `Decision`
- `Open Questions / Assumptions`
- `Change Summary`
- `Residual Risks`

其中 `Findings` 应按严重程度排序，优先列出：

- 会被 reviewer 一眼抓住的硬伤
- 会导致“claim too strong”的地方
- 会导致“not sufficiently supported”的地方

`Scorecard` 至少包含：

- 6 个维度分
- overall score
- confidence
- target venue calibration

`Decision` 至少包含：

- `GO / CONDITIONAL GO / NO-GO`
- 1-3 句理由
- 若不是 `GO`，必须写清最短修复路径

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
- Findings 很多，但没有哪一条能定位到具体 section 或证据
- recommendation 和前面 findings 严重脱节
- confidence 很高，但其实没读 appendix / figures / checklist

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
- Scorecard: 维度分 + overall score + confidence
- Decision: `GO / CONDITIONAL GO / NO-GO`
- Open Questions / Assumptions
- Change Summary
- Residual Risks

## Examples

- “从严苛 reviewer 视角 review 一下这篇稿子，优先找硬伤。”
- “不要润色语法，只告诉我哪些问题会导致弱拒或低分。”
- “投稿前帮我做一次 final risk audit，尤其看 novelty 和 evidence。”
