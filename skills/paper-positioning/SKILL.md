---
name: "paper-positioning"
description: "把研究想法转成投稿级问题定义、贡献地图、intro 主线和近年工作差异。Invoke when 用户刚有 idea、要定 venue、或要收紧 novelty/positioning。"
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

## 工作模式

### `core-positioning`

默认模式，适合绝大多数论文：

- 澄清问题定义
- 收紧贡献边界
- 对齐近两年工作

### `introduction-flowchart`

适合用户明确要写 intro、abstract、title 第一版时：

- 把定位压成 introduction 的逻辑流
- 明确背景 -> 缺口 -> 难点 -> 核心思路 -> 贡献 -> 证据承接

### `paper-type-template`

适合用户已经知道自己更像技术类论文还是 benchmark/evaluation 类论文时：

- 技术类论文：强调新机制、新理论、新系统能力
- benchmark/evaluation 类论文：强调评估价值、公平性、覆盖性、诊断性和社区贡献

默认规则：

- 若用户没有特意强调论文类型，默认按 `技术研究型论文` 处理
- 只有当 benchmark、evaluation protocol、diagnostic asset 或社区评估修正本身就是第一贡献时，才切到 `benchmark/evaluation` 模板

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

默认先验：

- 若用户未特别说明，先按 `技术研究型论文` 校准
- 也就是默认优先问：方法/机制/理论到底新增了什么，证据是否足以支撑主 claim

分型判据模板：

1. `主贡献判定`
   - 第一贡献是否是新方法、新机制、新理论或新系统能力？
   - 若是，默认按技术类处理
2. `去掉 benchmark 后主线是否仍成立`
   - 若删掉 benchmark、更多评测或 diagnostic 分析后，论文主线仍成立，继续按技术类处理
3. `benchmark 是否是核心资产`
   - 只有当没有 benchmark / protocol / evaluation asset 这部分，论文就几乎失去核心贡献时，才转到 `benchmark/evaluation`
4. `reviewer 核心问题是什么`
   - 若 reviewer 最核心会问“你的方法到底新增了什么、baseline 是否公平、ablation 是否充分”，则是技术类
   - 若 reviewer 最核心会问“这个评估协议是否更合理、覆盖性是否充分、是否真的改变社区结论”，则是 benchmark/evaluation 类

进一步收敛成 2 条高频模板：

- `技术类论文模板`
  - 主问题是什么
  - 现有方法卡在哪里
  - 你的机制/方法到底新增了什么
  - 最小证据链是什么

- `benchmark/evaluation 类论文模板`
  - 现有评估或基准哪里不足
  - 为什么这个不足会误导社区或阻碍进展
  - 你的 benchmark/evaluation 资产如何修正这个问题
  - 需要哪些覆盖性、公平性、鲁棒性和可复用性证据

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

若是 benchmark/evaluation 类论文，贡献地图优先写成：

- C1: 新 benchmark / new evaluation protocol / new diagnostic setting
- C2: 覆盖性、公平性或失效模式分析
- C3: 对已有方法或社区实践的重新排序、再认知或设计启发

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

### Step 5.5: 生成 Introduction Flowchart

若用户需要 intro framing，按以下顺序生成：

1. `Why this area matters now`
2. `What recent work still fails to resolve`
3. `Why the gap is non-trivial`
4. `What we do differently`
5. `What evidence we provide`
6. `What should not be overclaimed`

输出时尽量给：

- 一句背景句
- 一句 gap 句
- 一句 difficulty 句
- 一句 approach 句
- 一组 contribution bullets

避免 intro 常见失误：

- 一上来背景铺太大，半页都没进入具体问题
- gap 只说“现有方法不够好”，但没有具体失效点
- approach 句只写 buzzwords，不写真正机制
- contribution bullets 和正文证据类型不匹配

### Step 6: 生成投稿定位包

交付：

- 候选标题 3 个
- 一段摘要骨架
- Introduction 首段
- Contribution bullets
- Related work positioning 段落
- Reviewer 可能会攻击的 5 点

若是 `introduction-flowchart` 模式，再补：

- Intro 六句骨架
- 首段与次段分工
- 哪些内容适合放 intro，哪些应推迟到方法/实验节

若是 `paper-type-template` 模式，再补：

- 技术类论文写法提示，或
- benchmark/evaluation 类论文写法提示
- `Type decision rationale`（按分型判据模板给出 2-4 条理由）

## 输出格式

建议按以下结构回复：

- `一句话主线`
- `投稿定位`
- `主贡献`
- `与近作差异`
- `不能过度宣称的点`
- `下一步证据缺口`

若用户明确需要 intro，可追加：

- `Introduction Flowchart`
- `Intro 6-Sentence Skeleton`

若用户明确要区分论文类型，可追加：

- `Paper Type Template`
- `Type-Specific Risks`
- `Type Decision Rationale`

## 低风险写法守则

- 少说“first / unified / optimal / definitive”，除非证据非常硬
- 多说“under the assumptions studied here / in this controlled setting / suggests / helps explain”
- 贡献描述优先精确，不优先夸大
- 标题优先具体，不优先宏大
- benchmark/evaluation 类论文不要假装自己提出了新方法贡献
- 技术类论文不要把 benchmark 整理或分析性观察包装成核心算法创新

## 与其他 Skills 的关系

- 若用户还在早期判断 idea 是否值得做，前置 `idea-evaluator`
- 初始 framing 后，交给 `academic-paper-factory` 统筹推进
- 引用核验交给 `citation-reality-guard`
- 成稿后交给 `reviewer-risk-audit`

## 标准输入

- 研究想法一句话
- 当前已有资产：理论 / 实验 / 系统 / 数据 / 草稿
- 目标 venue 与年份
- 用户最想强调的贡献
- 已知最强相关工作或 baseline
- 是否要进入 `introduction-flowchart` 模式
- 是否更像 `技术类` 或 `benchmark/evaluation` 类论文；若未说明，默认按 `技术研究型论文`

## 标准交付

- 一句话主线
- 论文类型判定（默认技术类或显式改判）
- 3 个候选标题
- contribution map
- 近两年差异点
- 不可过度宣称清单
- 下一步证据缺口
- 若需要，附 intro flowchart 与六句骨架
- 若需要，附技术类/benchmark 类模板建议与判定理由

## Examples

- “我想研究 sparse MoE 的统计效率，这个题能不能投 NeurIPS？”
- “帮我把这个想法收紧成 ICML 能接的定位，不要写太大。”
- “我已经有一个 theorem 和几个初步实验，帮我定 abstract 和 intro 的 framing。”
