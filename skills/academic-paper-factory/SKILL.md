---
name: "academic-paper-factory"
description: "统筹 AI 论文从选题到投稿的全流程。Invoke when 用户要从零开始写论文、推进中期草稿、或组织多项论文写作子任务。"
---

# Academic Paper Factory

面向人工智能领域期刊/会议投稿的总控 Skill。目标是把论文写作从“临时改稿”变成“有阶段门控、有风险控制、有可复用产物”的流水线。

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
6. **语言风格自然化**：最终稿件必须通过人工审查去除 AI 生成痕迹。审稿人和编辑部可能使用自动检测工具识别非自然句式；稿件应读起来像领域研究者亲手所写，而非 LLM 模板输出。

## 默认工作流

### Phase 0: 任务建模

收集并明确以下信息：

- 目标 venue：NeurIPS / ICML / ICLR / JMLR / TPAMI / Nature MI 等
- 论文类型：理论 / 方法 / 实验 / benchmark / survey / interdisciplinary
- 当前资产：idea、notes、code、figures、theorems、草稿、已有 `.tex/.bib`
- 最大风险：新颖性、证据不足、实验不稳、引用不实、篇幅失控、格式不符

输出：

- 一页 paper brief
- 论文主线一句话
- 目标审稿人画像
- 风险优先级列表

若用户目标 venue 还不稳定，先调用 `venue-fit-selector`，再进入后续写作与适配链。

### Phase 1: 选题定位

调用 `paper-positioning`，输出：

- 问题定义
- 与近两年工作的差异点
- 最小可投稿贡献集
- 不该宣称的内容
- 需要补的理论/实验/对比

### Phase 2: 结构搭建

默认建立如下骨架：

1. 标题与一句话摘要
2. Abstract
3. Introduction
4. Related Work / Positioning
5. Method / Theory
6. Experiments / Instances / Proof Sketch
7. Discussion / Limitations
8. Conclusion
9. References
10. Appendix / Checklist / Supplementary

要求：

- 先让主文达到"可审稿密度"
- 首屏必须让审稿人 30 秒内知道：问题、差异、核心结果、边界

### Phase 2.5: 系统文献综述与差异分析

在结构搭建后，若论文定位已明确但相关领域文献覆盖不足，可调用 `literature-survey-builder`：

- 建文献矩阵与分类体系
- 写差异表（与近 2-5 年关键工作的对比）
- 识别 novelty gap 与审稿人可能质疑点
- 确认哪些 claim 已有文献支撑或矛盾

目标：让 Related Work 节不仅有引用罗列，更有结构化的差异叙事。

### Phase 3: 证据闭环

默认先调用 `claim-evidence-mapper`，逐条检查主张与证据映射：

- Abstract 每一句是否在正文有承接
- Introduction 是否过度宣称
- Related Work 是否覆盖近两年关键工作
- Theory 是否给出 proof sketch 和关键假设
- Experiment 是否支撑主结论而不是旁证
- Figures / Tables / Captions 是否可复现

若当前稿件是理论型或 theorem-heavy 稿件，可追加调用 `proof-consistency-checker`：

- 核对 theorem statement 与 appendix proof 的口径
- 检查 assumptions / constants / logarithmic factors 是否一致
- 把 proof sketch 扩成 appendix-ready 版本

### Phase 4: 引用与事实核验

调用 `citation-reality-guard`：

- 核实所有引用真实存在
- 修正 `.bib` 元数据
- 检查近两年相关工作覆盖
- 检查正文对引用的描述是否准确
- **正文引用数必须 ≥20 篇**，覆盖经典奠基性工作和近两年关键工作
- **确认 `\bibliographystyle{unsrt}`（按首次出现顺序编号），删除 `.bib` 中未引用的死条目**

### Phase 5: 审稿风险审计

调用 `reviewer-risk-audit`：

- 新颖性风险
- 理论边界风险
- 实验公平性风险
- 图文不一致风险
- 版面和提交格式风险

若审稿风险主要来自 disclosure、checklist、supplementary、一致性或图表出版质量，再追加 `submission-integrity-audit`。

### Phase 6: 棘轮优化

调用 `paper-ratchet-optimizer`：

- 找最低分维度
- 只改一个高杠杆问题
- 重评
- 保留真正改进，避免反复来回

若主要问题是语言模板感而非内容本身，再追加 `writing-naturalness-guard` 做风格自然化，而不是直接在总控里做大段重写。

### Phase 7: venue 适配

当用户尚未确定 venue、需要比较主投与备投路线时，先调用 `venue-fit-selector`。

当用户已明确目标会议/期刊、需要切换 venue 或生成投稿平台字段时，再调用 `venue-submission-adapter`：

- 抽取 venue-specific 要求
- 区分哪些是格式/投稿平台问题，哪些是内容层问题
- 将模板、页数、匿名性和 metadata 任务移交给合适的执行链

若进入提交前最后检查，再追加 `submission-integrity-audit` 与 `latex-submission-packager`。

### 语言风格自然化检查：AI 生成痕迹与规避指南

在进入最终交付前，必须对全文做一轮语言风格审查。AI 辅助生成的学术文本存在可识别的模式特征，自动检测工具和资深审稿人都能凭经验察觉。以下列出最高频的 AI 风格信号及改进方向。

#### 高频 AI 风格信号

**过度使用的连接词与模板化句式**

| 高 AI 检测风险表达 | 更自然的人类写作替代 |
|---|---|
| Furthermore, ... / Moreover, ... / Additionally, ... | 直接陈述下一句，或用简短的 "Also, ..." / 更具体的逻辑连接词（如 "However," / "In contrast,"） |
| It is worth noting that ... | 直接陈述值得注意的内容本身 |
| It is important to highlight that ... | 直接强调，或删掉该句 |
| Notably, ... / Importantly, ... / Interestingly, ... | 大多数情况下可直接删掉；若需强调，用 "A key observation is ..." 代替 |
| Specifically, ... / In particular, ... | 前文够具体时直接省略 |
| In this paper, we propose / present / introduce ... | 保留首次出现，但后续避免每段重复 |
| A growing body of literature / research / work ... | 改为具体引用："Recent work by X et al. (2025) and Y et al. (2026) ..." |
| This suggests that ... / This indicates that ... / This highlights that ... | 不要每段都用 "This shows..." 开头；有时直接给结论不加引导词 |

**句子结构与段落的过度规整**

- 每段都是"主题句 → 证据 → 总结句"的固定模式。人类写作常有段落直接以证据开头，或以问题/反诘结尾。
- 每段长度高度均匀。人类写作中段落长度自然波动，有些段落可以短到 2-3 句，有些可以长达 15 句。
- 每句长度和复杂度相近。AI 倾向于生成均匀复杂的句子；人类写作会在短句（制造冲击）和长句（容纳细节）之间切换。
- 所有节（Abstract / Intro / Method / Experiment 等）的写作密度一致。人类写作通常在 Method 节更专注具体操作，在 Intro 节更舒展。

**"we" 的过度使用**

AI 倾向于在每句开头使用 "We ..." 结构（"We propose ... We observe ... We note that ... We also find ... We believe ..."）。人类写作者会变换句子起点，包括直接以技术名词、被动语态、或者时间状语开头。

**修辞平衡的过度对称**

"Not only ... but also ...", "On the one hand ... on the other hand ...", "While ... yet ..." 这类平衡结构如果出现频率过高，是强 AI 信号。人类写作者不会每页都使用平行结构。

**"完美"的语法与一致的语气**

人类写作者在某些位置会有略微非正式的措辞、领域惯用语、或者略带主观的判断——这些正是写作个人风格的体现。完全无瑕的语法、每句话都同样正式的语气，反而是 AI 检测的强烈特征。

#### 如何在保留 AI 辅助效率的同时降低检测率

1. **本地化改写**：对 AI 生成的段落，逐句做结构性改写——不要只换同义词，要重新组织信息顺序。
2. **引入领域特有表达**：在 Method 节使用领域内惯用的简称和技术细节，而非通用解释。
3. **打破模板节奏**：故意让某些段落不以主题句开头，让某些段落结束得更突然。
4. **变化引用位置**：不要总是在句子末尾放引用（`[X]`），可以在句子中间或开头自然嵌入。
5. **限制 "Furthermore / Moreover / Additionally" 的使用**：全文各出现不超过一次。
6. **结论不要模板化**：避免 "In summary, we have shown that ... Our results demonstrate ... Future work could ..." 这种三段模板。改为以具体的开放问题结尾。
7. **手动检查 Abstract 中的 every sentence**：Abstract 是检测器最关注的区域，确保其中无过度模板句式。
8. **分散写作"风格"**：如果多个 section 由 AI 生成，让不同 section 呈现不同的句式偏好，而非全篇统一风格。

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
- **不要直接使用 LLM 未经改写的一整段输出作为稿件正文**。AI 辅助仅应用于词汇润色、结构调整和局部改写；完整段落的直接生成会留下强烈的句式模板痕迹，增加检测风险。

## 推荐搭配

- 统一入口：`paper-intake-router`
- 选题与定位：`paper-positioning`
- 文献综述与差异分析：`literature-survey-builder`
- 主张与证据映射：`claim-evidence-mapper`
- 引用核验：`citation-reality-guard`
- 风险审稿：`reviewer-risk-audit`
- 迭代优化：`paper-ratchet-optimizer`
- 实验叙事：`experiment-story-builder`
- 理论一致性：`proof-consistency-checker`
- 风格自然化：`writing-naturalness-guard`
- venue 决策：`venue-fit-selector`
- 投稿语义合规：`submission-integrity-audit`
- 投稿打包：`latex-submission-packager`
- venue 适配：`venue-submission-adapter`
- 审稿回应：`rebuttal-response-drafter`

## 主控调用手册

把本 Skill 当作总调度器，而不是所有细节都自己做。默认按论文阶段进行路由。

### 路由总原则

1. 先判断论文处于哪个阶段。
2. 一个阶段优先调用一个主 Skill，必要时再串联辅助 Skill。
3. 若用户需求横跨多个阶段，先拆任务，再按顺序执行。
4. 如果用户需求本身很模糊，先交给 `paper-intake-router` 做分流，再由本 Skill 统筹执行链。

### 阶段 1：只有 idea，还没有论文

优先调用：

- `paper-positioning`

可选补充：

- 若用户已经列出候选相关工作，再补 `citation-reality-guard`

目标：

- 定问题
- 定贡献
- 定 venue
- 定不能乱说的边界

若 venue 还未定或用户想比较会议/期刊路线：

- 追加 `venue-fit-selector`

### 阶段 2：有部分结果，开始搭主文

优先调用：

- `paper-positioning`
- 然后回到 `academic-paper-factory` 组织主文骨架

如果实验已经在跑：

- 同步调用 `experiment-story-builder`

目标：

- 完成标题、摘要、引言、相关工作、方法/理论、实验骨架
- 让稿件尽快达到“可审稿完整度”

### 阶段 3：开始补 related work / references / 文献综述

根据具体需求选择主 Skill：

- 若需要系统化文献矩阵与差异分析 -> `literature-survey-builder`
- 若需要引用真实性核验与元数据修正 -> `citation-reality-guard`

触发信号：

- 需要系统巡视近年文献
- 新增 `.bib`
- 写 related work
- 要比较近两年工作
- 准备做投稿前事实核查

目标：

- 建文献矩阵与差异表
- 消除幻觉引用
- 修正元数据
- 对齐近两年文献

### 阶段 4：实验结果开始成形

优先调用：

- `experiment-story-builder`

必要时串联：

- 若主张口径开始变强，补 `paper-ratchet-optimizer`
- 若要检查 claim 是否被 theorem / figure / table 真正支撑，补 `claim-evidence-mapper`
- 若重点转为图表出版质量与 caption 审计，补 `submission-integrity-audit`

目标：

- 建实验证据链
- 配图配表
- 规范 caption、baseline、统计
- 区分主文结果和附录结果

### 阶段 5：中后期整稿优化

优先调用：

- `paper-ratchet-optimizer`

必要时串联：

- 发现引用问题 -> `citation-reality-guard`
- 发现定位问题 -> `paper-positioning`

目标：

- 一轮只修一个高杠杆问题
- 评估 -> 修改 -> 验证 -> 保留/回滚

### 阶段 6：草稿成形，做审稿人视角检查

优先调用：

- `reviewer-risk-audit`

必要时串联：

- 若要逐条核 claim 与证据，补 `claim-evidence-mapper`

目标：

- 提前发现 reviewer 一眼能抓到的硬伤
- 输出 P0/P1/P2 风险
- 明确哪些问题必须修、哪些可选

### 阶段 7：最终投稿前封包

优先调用：

- `submission-integrity-audit`
- 然后 `latex-submission-packager`

必要时串联：

- 若发现文献有问题 -> `citation-reality-guard`
- 若发现 claims 还偏大 -> `paper-ratchet-optimizer`

目标：

- 确认 references / appendix / checklist 顺序
- 检查匿名性、编译、版面、supplementary、一致性

### 阶段 8：审稿意见回应

优先调用：

- `rebuttal-response-drafter`

触发信号：

- 收到审稿意见
- 需要拆解 reviewer 问题并制定回应策略
- 准备提交 response letter

目标：

- 拆解审稿意见并分类（可修、需反驳、需补充实验）
- 制定回应策略与措辞风险控制
- 起草逐条 rebuttal

## 快速决策图

如果用户说：

- “这个 idea 能不能投？” -> `paper-positioning`
- “帮我从零开始写论文” -> `academic-paper-factory`
- “帮我系统做一轮文献综述” -> `literature-survey-builder`
- “帮我看每个 claim 有没有证据承接” -> `claim-evidence-mapper`
- “related work / bib / 引用帮我核一下” -> `citation-reality-guard`
- “实验节怎么讲更有说服力？” -> `experiment-story-builder`
- “做最后一轮微调” -> `paper-ratchet-optimizer`
- “review 一下这篇稿子” -> `reviewer-risk-audit`
- “投哪个 venue 更合适？” -> `venue-fit-selector`
- “投稿前检查图表、checklist、ethics、code/data 声明” -> `submission-integrity-audit`
- “准备投稿了，帮我检查格式和提交包” -> `latex-submission-packager`
- “收到 reviews，帮我写 rebuttal” -> `rebuttal-response-drafter`

## 组合调用模板

### 从零开始

`paper-intake-router -> academic-paper-factory -> paper-positioning -> literature-survey-builder -> claim-evidence-mapper -> experiment-story-builder -> citation-reality-guard -> venue-fit-selector -> reviewer-risk-audit -> submission-integrity-audit -> latex-submission-packager`

### 中期打磨

`paper-intake-router -> academic-paper-factory -> literature-survey-builder -> claim-evidence-mapper -> paper-ratchet-optimizer -> citation-reality-guard -> reviewer-risk-audit`

### 投稿前终审

`paper-intake-router -> claim-evidence-mapper -> reviewer-risk-audit -> citation-reality-guard -> submission-integrity-audit -> latex-submission-packager`

### 投稿后审稿回应

`paper-intake-router -> rebuttal-response-drafter`

## 一句话策略

先做出可审稿完整稿，再用低风险、可验证、可回滚的方式逐轮逼近高质量投稿版本。

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
