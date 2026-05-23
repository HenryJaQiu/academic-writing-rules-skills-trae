---
name: "paper-ratchet-optimizer"
description: "用 Darwin 风格的评估-改进-验证-保留/回滚循环优化论文质量。Invoke when 用户要持续打磨草稿、比较方案、或降低投稿风险。"
---

# Paper Ratchet Optimizer

这个 Skill 借鉴 Darwin 风格的 Skill 优化思想，但优化对象从 `SKILL.md` 换成论文草稿、投稿包和相关证据链。目标不是“多改”，而是“只保留可验证的改进”。

## 何时调用

- 用户说“做最后一轮微调”
- 用户要连续迭代 abstract / intro / related work / conclusion
- 用户要比较两个版本哪个更适合投稿
- 用户要在不引入新风险的前提下提升稿件质量

## 核心机制

每轮只做一个高杠杆改动，并执行：

1. `Evaluate`
2. `Improve`
3. `Verify`
4. `Keep or Revert`

不要同时大改五个方向，否则无法归因。

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

### Step 1: 找最低分维度

先识别当前最弱项，例如：

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

### Step 2: 只改一类问题

一轮内只处理一个主题，例如：

- 只收紧 claims
- 只补近年 work
- 只核验 citations
- 只修 submission format
- 只做语言风格自然化（降低 AI 检测率）

### Step 3: 验证改动是否真改进

最少检查：

- 是否降低 reviewer 可攻击点
- 是否引入新矛盾
- 是否需要重新编译
- 是否改变了页数、引用、图表边界
- 是否让表达更准确而不是更花哨

### Step 4: 决策

- 改动显著降低风险 -> 保留
- 改动带来新不确定性或副作用 -> 回滚或重写

## 推荐输出格式

- `本轮最低分维度`
- `拟实施改动`
- `验证结果`
- `是否保留`
- `下一轮建议`

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

## 与其他 Skills 的关系

- 用 `reviewer-risk-audit` 产出问题列表
- 用 `citation-reality-guard` 解决引用问题
- 用 `academic-paper-factory` 维持整体流程

## 标准输入

- 当前版本的草稿或指定 section
- 上一轮问题列表或 reviewer 风险列表
- 本轮只允许优化的一个目标
- 是否允许重新编译或改动 `.bib` / figures / appendix

## 标准交付

- 本轮最低分维度
- 本轮改动策略
- 验证结果
- 保留 / 回滚建议
- 下一轮最优先问题

## Examples

- “做最后一轮微调，但每次只动一个高杠杆问题。”
- “帮我比较两个 abstract 版本，哪个更稳、更适合投稿？”
- “按 Darwin 风格连续优化这篇稿子，避免越改越乱。”
