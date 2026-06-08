---
name: "idea-evaluator"
description: "从导师视角评估 research idea 的价值、可行性、能力匹配和投稿潜力。Invoke when 用户问这个想法值不值得做、能不能投、或该不该继续投入。"
---

# Idea Evaluator

这个 Skill 负责解决一个在真实科研训练里非常高频、但很多论文写作 skill 没有单独建模的问题：

`一个 idea 到底值不值得继续做？`

它不是直接把 idea 写成论文定位，也不是立刻生成 intro，而是先做更像导师/博导会做的前置判断：

- 问题是否值得做
- 增量是否可能成立
- 资源与能力是否匹配
- 更像短期可落地项目，还是高风险长期方向
- 适合做理论、方法、benchmark，还是根本不该按论文方向推进

## 何时调用

- 用户只有一个想法，问“这个题值不值得做”
- 用户说“这个能不能投 NeurIPS/ICML/ICLR/ACL”
- 用户不确定是继续投入、换问题，还是缩小 scope
- 用户想比较多个 idea，决定先做哪一个
- 用户需要导师式的前期 triage，而不是直接写论文

## 核心目标

1. 在投入大量实验和写作前，先筛掉低价值或高错配方向
2. 区分“好问题但当前不适合做”和“问题本身就不值得做”
3. 输出可执行的下一步，而不是只给抽象评价
4. 为下游 `paper-positioning`、`venue-fit-selector`、`academic-paper-factory` 提供稳的起点

## 评估维度

默认按以下 6 维判断：

1. `Problem Value`
   - 问题是否真实重要
   - 是否有明确受众、场景或理论价值

2. `Novelty Potential`
   - 相比近两年工作，是否存在可形成论文贡献的空间
   - 是真空白，还是只是换表述

3. `Feasibility`
   - 当前资源、时间、数据、算力、方法能力是否支持
   - 是否需要不可控外部条件

4. `Evidence Path`
   - 未来是否容易设计出让 reviewer 信服的最小证据链
   - 是理论支撑容易，还是实验支撑容易

5. `Advisor / Student Fit`
   - 是否符合当前团队积累、作者技能栈和短期产出节奏
   - 是“值得做”还是“值得别人做”

6. `Venue Potential`
   - 更适合 workshop、benchmark、顶会 short/full、journal，还是暂不适合投稿

## 风险分类

### `Green`

- 问题有明确价值
- 增量路径较清晰
- 可在合理时间内做出可审稿版本

### `Yellow`

- 有潜力，但需要缩 scope、换 framing 或换证据路径
- 更适合作为 side project、pilot、workshop 或预研

### `Red`

- 问题价值弱、增量模糊、证据链不成立，或与当前能力资源严重错配
- 不建议直接投入成主线论文

## 执行步骤

### Step 1: 先冻结 idea 版本

先把用户的想法压成 3 个句子：

- 我们想解决什么问题
- 想用什么办法
- 预期想证明什么

若连这三句都写不稳，说明想法还处在过早阶段。

### Step 2: 判断 idea 类型

至少区分以下类型：

- 理论问题
- 新方法/新架构
- benchmark / evaluation
- 系统/应用
- survey / synthesis
- 方向性探索，不足以直接成论文

### Step 3: 做导师式 triage

优先回答：

- 为什么这个问题现在值得做
- 如果 reviewer 第一反应是“这不是早有人做过吗”，最像哪一类旧问题
- 最小贡献能是什么
- 最短的失败路径是什么

### Step 4: 做资源与能力匹配

显式检查：

- 数据是否可得
- 实验是否能跑
- 理论是否在当前能力范围内
- 是否严重依赖不可控 collaborator / external infra / private data
- 是否与当前项目节奏相容

### Step 5: 输出决策

至少给出：

- `Go`
- `Go, but shrink`
- `Go, but reframe`
- `Hold as side project`
- `No-Go`

## 默认输出格式

- `Idea Freeze`
- `6-D Evaluation`
- `Main Risks`
- `Decision`
- `Why`
- `Next Best Action`

## 标准输入

- idea 一句话或一段描述
- 目标方向/venue
- 当前已有资产：理论、代码、数据、观察、实验
- 时间和资源约束
- 用户最担心的问题：创新性/可行性/是否值得投

## 标准交付

- 结构化评估表
- `Green / Yellow / Red` 风险颜色
- `Go / Go but shrink / Go but reframe / Hold / No-Go`
- 最小下一步建议
- 若值得继续，推荐转交的下游 Skill

## Examples

- “这个想法值不值得继续做成主线论文？”
- “我有三个题，帮我判断先做哪一个最有投稿潜力。”
- “这更像 benchmark 论文，还是方法论文？”
- “这个方向能不能投顶会，还是更适合先做 workshop/pilot？”

## 与其他 Skills 的关系

- 若 idea 值得继续，转交 `paper-positioning`
- 若还要比较投哪里，转交 `venue-fit-selector`
- 若用户想从零把项目推进成论文，转交 `academic-paper-factory`
- 若需要先补近两年相关工作再判断，转交 `literature-survey-builder`
