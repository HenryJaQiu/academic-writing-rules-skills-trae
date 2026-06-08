---
name: "paper-ratchet-optimizer"
description: "用棘轮式评估-改进-验证-保留/回滚循环优化论文质量。Invoke when 用户要根据审稿 findings 和 scorecard 持续修稿、控回归、准备复审，或按论文类型决定先修什么。"
---

# Paper Ratchet Optimizer

这个 Skill 借鉴 Darwin 风格的 Skill 优化思想，但优化对象从 `SKILL.md` 换成论文草稿、投稿包和相关证据链。目标不是“多改”，而是“只保留可验证的改进”。

它现在默认与 `reviewer-risk-audit` 对齐，优先吃上一轮审计输出：

- `Findings`
- `Scorecard`
- `Decision`
- `Residual Risks`

然后只选择一个最高杠杆问题做最小闭环修复，再决定保留、回滚或进入复审。

## 何时调用

- 用户说“做最后一轮微调”
- 用户要连续迭代 abstract / intro / related work / conclusion
- 用户要比较两个版本哪个更适合投稿
- 用户要在不引入新风险的前提下提升稿件质量
- 用户刚做完 `reviewer-risk-audit`，现在要按 findings 修稿
- 用户要根据 `GO / CONDITIONAL GO / NO-GO` 结果决定先修什么
- 用户要做重大修改后的“修一轮 -> 复审一轮”闭环

## 核心机制

每轮只做一个高杠杆改动，并执行：

1. `Evaluate`
2. `Improve`
3. `Verify`
4. `Keep or Revert`

不要同时大改五个方向，否则无法归因。

## 与 `reviewer-risk-audit` 的契约

默认把上一轮审计结果视为上游输入：

- `Findings`：告诉你先修哪一个问题
- `Scorecard`：给出当前基线分数
- `Decision`：告诉你当前版本是 `GO / CONDITIONAL GO / NO-GO`
- `Residual Risks`：提醒哪些问题即使本轮不修，也不能被忘掉

本 Skill 的作用不是重新做一遍 review，而是：

1. 选一个最值得修的 finding
2. 设计最短修复路径
3. 检查改动是否真的改善该 finding
4. 判断是否值得进入下一轮 `reviewer-risk-audit`

## 类型化修复策略

默认沿用上游 `reviewer-risk-audit` 的 paper-type calibration。

默认规则：

- 若用户没有特意强调论文类型，默认按 `technical-paper repair` 处理
- 只有当 benchmark、evaluation protocol、diagnostic asset 或社区评估修正本身就是第一贡献时，才切到 `benchmark-evaluation repair`
- 若论文主贡献仍是方法、机制、理论或系统能力，即使实验很多、覆盖很多设置，也继续按技术类修复优先级处理

### `technical-paper repair`

优先修：

- 核心方法到底新增了什么
- 最强 baseline、公平预算与消融是否站得住
- abstract / intro / conclusion 是否把局部提升写成普遍突破
- 机制解释、主结果和理论口径是否一致

常见高杠杆修复：

- 收紧方法贡献表述
- 补最自然 baseline 或负对照
- 强化 ablation 与 mechanism evidence
- 降级过强 claim，避免“增量小却写得很大”

### `benchmark-evaluation repair`

优先修：

- benchmark / protocol gap 是否被具体举证
- coverage、公平性、diagnostic value 是否真的被证明
- 是否把 protocol-specific 结果夸成普遍结论
- 是否只用少量任务、指标或方法就声称改变社区认知

常见高杠杆修复：

- 补 coverage / subgroup / metric breakdown
- 收紧 community-level claim
- 补公平性与 protocol rationale
- 增加 re-ranking、failure mode 或稳定性证据

## 评分维度

建议总分 100，按以下维度评估：

- 选题与定位清晰度：15
- 新颖性表达可信度：15
- 主张与证据一致性：20
- 相关工作与引用可靠性：10
- 理论/实验可审性：15
- 图表与排版可读性：10
- 语言风格与 AI 检测风险：10
- 投稿包完整性与低风险程度：5

## 单轮优化流程

### Step 1: 读取上游审计结果并选一个目标

优先从 `reviewer-risk-audit` 中读取：

- 最严重的 `P0/P1`
- 最低的维度分
- 当前 `Decision`
- 仍未关闭的 `Residual Risks`

然后只选一个本轮目标。选择优先级建议：

1. 未关闭的 `P0`
2. 直接压低 `Decision` 的 `P1`
3. 拉低最低维度分的系统性问题
4. 会连带改善多个 section 的结构性问题

常见目标例如：

- Abstract 过大
- Related Work 太旧
- 引用元数据不准
- 结论外推过头
- 图文不一致
- 语言风格过度模板化（AI 检测风险高）
- 正文实验内容不足（过多推给 appendix）
- 引用数量不足（<20 篇）或 Related Work 缺乏差异分析
- Notation/Definitions 误嵌入 Introduction 内部
- checklist / appendix / references 顺序错误

若已知论文类型，选目标时再加一条：

- 技术类优先修“方法是否真成立、证据是否真支撑”
- benchmark/evaluation 类优先修“评估资产是否真合理、结论是否被过度外推”

若论文类型未知，默认先按技术类目标选择，除非上游已经明确给出 benchmark/evaluation 的充分理由。

### Step 2: 写出单轮优化假设

在动手前先明确：

- `Target finding`
- `Current baseline`
- `Expected improvement`
- `Do-not-break constraints`

推荐写成一句话：

`如果我们只修 X，并保持 Y 不变，那么最有可能改善 Z 分数/决策，而不会引入新的 A/B/C 风险。`

### Step 3: 只改一类问题

一轮内只处理一个主题，例如：

- 只收紧 claims
- 只补近年 work
- 只核验 citations
- 只修 submission format
- 只做语言风格自然化（降低 AI 检测率）

若是 `technical-paper repair`，优先单轮主题：

- 只收紧方法 claim
- 只补一个最关键 baseline / ablation 缺口
- 只修 abstract / intro / conclusion 的 overclaim

若是 `benchmark-evaluation repair`，优先单轮主题：

- 只补 coverage / fairness 证据
- 只补 diagnosis / re-ranking 证据
- 只收紧 protocol-specific 到 general claim 的外推

若该问题本质上需要跨 3 个以上模块的大改，说明它不适合当前棘轮轮次，应先拆小。

### Step 4: 验证改动是否真改进

最少检查：

- 是否降低 reviewer 可攻击点
- 是否引入新矛盾
- 是否需要重新编译
- 是否改变了页数、引用、图表边界
- 是否让表达更准确而不是更花哨

若输入来自 `reviewer-risk-audit`，还应显式检查：

- 被选中的 finding 是否已明显缓解
- 对应最低分维度是否至少有合理的改善预期
- `Decision` 是否有机会从 `NO-GO -> CONDITIONAL GO` 或 `CONDITIONAL GO -> GO`
- 是否把其他未修问题掩盖掉了，而不是解决掉了

若有 paper-type calibration，还应检查：

- 技术类论文是否真的更像“方法成立”而不是“文案收紧了”
- benchmark/evaluation 类论文是否真的更像“评估资产可信了”而不是“包装得更像社区结论”

### Step 5: 评估是否需要立即复审

以下情况建议立刻回到 `reviewer-risk-audit`：

- 本轮修的是 `P0`
- 修改波及 abstract / intro / conclusion / main figure / theorem / key experiment
- 可能改变 overall score 或 venue fit 判断
- 修改后自己已无法确定是否引入新回归

以下情况可先继续下一轮小修：

- 只修单个表述或局部 caption
- 不影响 claim、scorecard 或 decision 主结论
- 只是清理明显格式或残留问题

### Step 6: 决策

- 改动显著降低风险 -> 保留
- 改动带来新不确定性或副作用 -> 回滚或重写
- 改动价值不明确，但成本已扩散 -> 停止本轮并请求复审，不继续连改

若是 `benchmark-evaluation repair`，尤其警惕：

- 通过更强措辞掩盖 coverage 不足
- 通过删掉不利结果而不是解释它们来“提升一致性”

## 推荐输出格式

- `上游基线`
- `本轮目标 finding`
- `拟实施改动`
- `验证结果`
- `是否保留`
- `是否进入复审`
- `下一轮建议`

若论文类型明确，可再补：

- `Type calibration`
- `Why this is the highest-leverage fix for this paper type`

若未特别说明论文类型，默认输出中应显式写明：

- `Type calibration: technical-paper (default)`

## 适合优化的对象

- 标题
- 摘要
- 引言首段
- contributions bullets
- related work positioning
- theorem statement
- discussion / limitations
- conclusion
- references / checklist / appendix 顺序
- **全文语言风格自然化（降低 AI 检测率）**

## 语言风格自然化优化专项

当本轮优化目标是降低 AI 检测风险时，按以下清单逐项检查：

### 检查清单

- [ ] 全文 "Furthermore / Moreover / Additionally" 总数是否 ≤ 2
- [ ] "Notably / Importantly / Interestingly" 是否已删除大半，保留的确实必要
- [ ] 是否有段落以 "This suggests / This indicates / This highlights" 开头——改成其他句子起点
- [ ] 句子开头 "We ..." 的比例是否过高；加入被动语态、时间状语、名词短语开头等变化
- [ ] 段落结构是否有变化：至少 2-3 个段不以"主题句"开头
- [ ] 段落长度是否有自然波动：存在短段落（2-3 句）和长段落（8+ 句）的交替
- [ ] Abstract 是否有模板痕迹：逐句检查，"In this paper, we propose / present / introduce" 是否出现在第一句
- [ ] Conclusion 是否落入 "In summary, we have shown that ... Our results demonstrate ... Future work could ..." 三段模板——改为以开放问题或具体局限结尾
- [ ] 引用位置是否有变化：不仅放在句尾，也在句中或句首自然嵌入
- [ ] 全文是否存在明显来自 LLM 的整段输出未做结构性改写

### 验证方法

将修改前后的同一段落放入在线 AI 检测器（如 GPTZero、Originality.ai 等）做对比，确认改写降低了检测概率。但注意：任何检测器都是参考指标而非绝对标准。最可靠的策略是让稿件读起来像有学术经验的研究者亲自写作，而非完美语法与均衡结构的模板输出。

## 不适合盲目优化的对象

- 尚未核验真伪的引用
- 尚未稳定的核心定理
- 尚未跑完的实验结果
- 依赖大规模重构的文稿结构

## 默认保守策略

- 优先减少错误和硬伤，而不是追求“更像营销文案”
- 优先提升审稿可验证性，而不是追求句子更华丽
- 优先修硬伤，再修高频 reviewer irritation points
- 没有上游 findings 时，也应先自造一个最小 finding，再进入改单轮
- 不允许在 `NO-GO` 状态下跳过关键 `P0/P1`，直接去修边角料
- 不把“分数可能会提高”当成事实，除非能说明是哪个 finding 被缓解了
- 技术类论文不要用更强修辞替代更强证据
- benchmark/evaluation 类论文不要用更大社区叙事替代 coverage / fairness 证据

## 与其他 Skills 的关系

- 默认上游是 `reviewer-risk-audit`
- 用 `citation-reality-guard` 解决引用问题
- 用 `writing-naturalness-guard` 处理自然化与 AI 痕迹问题
- 用 `academic-paper-factory` 维持整体流程

推荐闭环：

`reviewer-risk-audit -> paper-ratchet-optimizer -> reviewer-risk-audit`

## 标准输入

- 当前版本的草稿或指定 section
- 上一轮 `Findings`
- 上一轮 `Scorecard`
- 上一轮 `Decision`
- 上一轮 `Residual Risks`
- 本轮只允许优化的一个目标
- 是否允许重新编译或改动 `.bib` / figures / appendix
- 是否更像 `technical-paper` 或 `benchmark-evaluation`；若未说明，默认按 `technical-paper`

## 标准交付

- 上游基线：最低分维度、overall score、confidence、decision
- 本轮目标 finding
- 本轮改动策略
- 验证结果
- 保留 / 回滚建议
- 是否建议立即复审
- 下一轮最优先问题
- Type calibration: `technical-paper (default)` 或显式改判结果
- 若需要，附类型化修复理由

## 复审就绪信号

若出现以下任一信号，建议把当前版本交回 `reviewer-risk-audit`：

- 原 `P0` 已被针对性修复
- 最低分维度已完成一轮实质性改善
- `NO-GO` 的核心原因已处理
- 关键 section 的主张、证据或格式边界发生了变化

## Examples

- “做最后一轮微调，但每次只动一个高杠杆问题。”
- “帮我比较两个 abstract 版本，哪个更稳、更适合投稿？”
- “按 Darwin 风格连续优化这篇稿子，避免越改越乱。”
