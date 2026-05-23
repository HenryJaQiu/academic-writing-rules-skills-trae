---
name: "paper-positioning"
description: "把研究想法转成投稿级问题定义、贡献地图和近年工作差异。Invoke when 用户刚有 idea、要定 venue、或要收紧 novelty/positioning。"
---

# Paper Positioning

这个 Skill 专门解决“有想法但还不是一篇论文”的问题。它把粗糙 idea 变成可投稿的 paper framing。

## 何时调用

- 用户只有一句研究想法或一个方向
- 用户说“这个题能不能投 NeurIPS/ICML/ICLR”
- 用户需要写 title / abstract / intro 的第一版
- 用户需要明确与近两年相关工作的差异与增量
- 用户担心“看起来像重述经典结果”或“贡献不聚焦”

## 目标

输出以下五项核心资产：

1. 问题定义
2. 贡献地图
3. 风险边界
4. 实验/理论最小支撑集
5. venue 对齐版本

## 输入清单

优先收集：

- 研究问题一句话
- 你已经有的结果：理论、实验、系统、数据、观察
- 目标 venue
- 潜在 baseline / related work
- 用户最想强调的亮点

## 执行步骤

### Step 1: 写一句话主线

用一句话回答：

- 我们解决什么问题？
- 为什么这个问题现在值得解决？
- 我们和最近工作相比多做了什么？

模板：

`We study X under Y setting, identify Z limitation in recent work, and provide A/B/C evidence showing D.`

### Step 2: 明确论文类型

判断当前论文属于哪一类：

- 理论型：核心是 theorem / lower bound / identifiability / guarantees
- 方法型：核心是新算法 / 架构 / objective / system
- 实证型：核心是 benchmark / scaling study / empirical law / ablation
- 交叉型：理论 + 实验，或系统 + 分析

不同类型决定不同风险：

- 理论型最怕“新瓶装旧酒”“假设过强”“证明不支撑摘要”
- 方法型最怕“实验增量小”“缺最强 baseline”“ablation 不充分”
- 实证型最怕“故事大于证据”“结论不稳”“统计不规范”

### Step 3: 建贡献地图

把贡献拆成最多 3 个主贡献：

- C1: 核心理论/方法结果
- C2: 关键机制解释或边界分析
- C3: 实证支撑、实例验证、系统收益

每个贡献必须写清：

- 输入前提
- 产出结果
- 相比最近工作新增了什么
- 哪些内容不能顺手夸大

### Step 4: 近两年工作对齐

至少列出 3–5 篇近两年强相关工作，并回答：

- 它们解决了什么
- 你的问题设置哪里不同
- 它们的证据类型与本文的证据类型有何不同
- 本文是替代、补充、统一、泛化还是重新解释

要求：

- 不允许只列经典工作不列近作
- 不允许把“研究目标不同”的工作硬说成“被我们超越”

### Step 5: 风险边界前置

主动写出：

- 本文在哪些假设下成立
- 哪些结论只在 controlled setting 下成立
- 哪些结论只是经验观察
- 哪些未来工作不能伪装成已完成贡献

### Step 6: 生成投稿定位包

交付：

- 候选标题 3 个
- 一段摘要骨架
- Introduction 首段
- Contribution bullets
- Related work positioning 段落
- Reviewer 可能会攻击的 5 点

## 输出格式

建议按以下结构回复：

- `一句话主线`
- `投稿定位`
- `主贡献`
- `与近作差异`
- `不能过度宣称的点`
- `下一步证据缺口`

## 低风险写法守则

- 少说“first / unified / optimal / definitive”，除非证据非常硬
- 多说“under the assumptions studied here / in this controlled setting / suggests / helps explain”
- 贡献描述优先精确，不优先夸大
- 标题优先具体，不优先宏大

## 与其他 Skills 的关系

- 初始 framing 后，交给 `academic-paper-factory` 统筹推进
- 引用核验交给 `citation-reality-guard`
- 成稿后交给 `reviewer-risk-audit`

## 标准输入

- 研究想法一句话
- 当前已有资产：理论 / 实验 / 系统 / 数据 / 草稿
- 目标 venue 与年份
- 用户最想强调的贡献
- 已知最强相关工作或 baseline

## 标准交付

- 一句话主线
- 3 个候选标题
- contribution map
- 近两年差异点
- 不可过度宣称清单
- 下一步证据缺口

## Examples

- “我想研究 sparse MoE 的统计效率，这个题能不能投 NeurIPS？”
- “帮我把这个想法收紧成 ICML 能接的定位，不要写太大。”
- “我已经有一个 theorem 和几个初步实验，帮我定 abstract 和 intro 的 framing。”
