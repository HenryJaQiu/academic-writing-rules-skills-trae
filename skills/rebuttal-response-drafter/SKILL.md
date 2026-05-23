---
name: "rebuttal-response-drafter"
description: "拆解审稿意见、制定回应策略并起草逐条 rebuttal。Invoke when 用户收到 reviews、要写 rebuttal、或要规划修改与回应的一致性。"
---

# Rebuttal Response Drafter

这个 Skill 负责把“收到审稿意见后的混乱状态”变成“逐条、克制、可信、可执行的 rebuttal 与修稿计划”。它既关注语言风格，也关注回应是否真正解决 reviewer 的核心担忧。

## 何时调用

- 用户收到 conference / journal reviews
- 用户要写 rebuttal、author response、revision memo
- 用户想先判断哪些 review 必须正面回应、哪些可以澄清
- 用户需要把“计划修改内容”与“书面回复内容”统一起来

## 核心目标

1. 逐条拆解 reviewer concern，而不是按情绪回应
2. 区分可立即澄清、可承诺修改、无法在 rebuttal 期完成的问题
3. 保持尊重、克制、具体、可验证的回应风格
4. 让 rebuttal 与后续修稿动作一致，避免空头承诺

## 执行步骤

### Step 1: 解析 review

把每条意见拆成：

- concern 类型：novelty / evidence / experiment / theory / writing / formatting
- 严重程度：P0 / P1 / P2
- reviewer 真正在问什么
- 这条意见是否基于误解

### Step 2: 确定回应策略

每条意见只选一个主策略：

- `Clarify`: 论文里已有内容，但 reviewer 没看清
- `Add Evidence`: 需要补实验、补对比、补说明
- `Narrow Claim`: 当前 claim 过强，需要降级
- `Acknowledge Limitation`: 不能在 rebuttal 期解决，但要诚实承认

### Step 3: 生成回应草稿

每条回应优先结构：

1. 感谢与对问题的准确复述
2. 直接回答 reviewer 关切
3. 指向现有证据或新增证据
4. 如有必要，给出将补入正文的具体位置和内容

### Step 4: 检查承诺风险

逐条检查：

- 有没有承诺用户实际上做不到的实验或证明
- 有没有用攻击性或防御性语气
- 有没有把未完成工作写成“已完成”
- 有没有跟论文正文后续修改计划冲突

## 默认输出格式

- `Review Summary`
- `Priority Concerns`
- `Response Strategy`
- `Draft Rebuttal`
- `Planned Paper Changes`
- `Risks to Avoid`

## 低风险规则

- 不要直接说 reviewer 错了，改为“the current wording may have caused confusion”
- 不要承诺未经验证的新结果
- 不要在 rebuttal 中继续扩大主张
- 不要回避最关键的 novelty / evidence / fairness 问题
- 不要写空洞感谢，核心是快速、具体、可信

## 标准输入

- 全部 reviews / meta-review / AE comments
- 当前稿件状态
- rebuttal 字数限制或期刊回复格式
- 用户打算补做的实验或修改
- 当前最担心的 reviewer

## 标准交付

- review 问题拆解
- 优先级排序
- 每条意见的主回应策略
- rebuttal 初稿
- 需要同步修改正文的动作列表
- 高风险措辞提醒

## Examples

- “我收到 3 个 reviewer 的意见，帮我先拆成可以回应的点，再起草 rebuttal。”
- “不要替我硬杠 reviewer，帮我写得克制但有说服力。”
- “帮我把 rebuttal 和下一版修稿计划对齐，避免空头承诺。”

## 与其他 Skills 的关系

- 若 review 质疑定位，回调 `paper-positioning`
- 若 review 质疑 related work / citation，回调 `literature-survey-builder` 与 `citation-reality-guard`
- 若 review 质疑实验，回调 `experiment-story-builder`
- 若 review 质疑整体风险，回调 `reviewer-risk-audit`
- 总流程由 `academic-paper-factory` 统筹
