---
name: "venue-submission-adapter"
description: "将通用论文草稿适配到目标会议或期刊的结构、页数、投稿平台字段与附件要求。Invoke when 用户切换 venue、准备投稿、或需要平台/格式适配。"
---

# Venue Submission Adapter

这个 Skill 负责把“已经基本成形的论文”适配到具体会议或期刊，但不把自己绑死在 NeurIPS、OpenReview 或某个单独模板上。

它处理的是 venue-specific 约束层：结构、页数、匿名性、投稿平台字段、附件和 supplemental 口径。

## 何时调用

- 用户说“改成 NeurIPS / ICML / ICLR / Nature MI / TPAMI 格式”
- 用户要从一个 venue 切换到另一个 venue
- 用户需要准备投稿系统字段、keywords、subject area、supplementary
- 用户要检查某会议/期刊的特定顺序、页数或双盲要求

## 核心目标

1. 识别目标 venue 的真实约束
2. 将稿件从通用草稿适配成该 venue 的 submission-ready 版本
3. 区分“内容修改”和“venue 适配修改”
4. 明确哪些规则属于 venue、哪些属于项目、哪些属于论文内容本身

## 执行步骤

### Step 1: 明确 venue profile

先收集：

- venue 名称与年份
- conference / journal
- submission / rebuttal / camera-ready 阶段
- 页数限制
- 双盲与否
- 是否需要 checklist / ethics / code / supplementary / platform metadata

### Step 2: 生成 venue 约束清单

按以下维度整理：

- `结构顺序`
- `页数与版式`
- `匿名性`
- `引用格式`
- `投稿平台字段`
- `supplementary / checklist / reproducibility`

### Step 3: 划分改动边界

将适配动作分为三类：

- `必须适配`：不改会直接踩格式雷
- `建议适配`：提升该 venue 下的接受概率或专业感
- `不应混入`：和 venue 无关，属于内容层问题

### Step 4: 执行或移交

根据任务类型决定：

- LaTeX 模板、页数、匿名性 -> `latex-submission-packager`
- OpenReview / ScholarOne / Editorial Manager 字段 -> 在本 Skill 内组织输出
- 内容层的定位、引用、实验、证明问题 -> handoff 给对应通用 Skill

### Step 5: 生成 venue-ready 交付

默认输出：

- venue 规则摘要
- 当前稿件与 venue 的差距
- 需要修改的文件或字段
- 可直接填写的平台字段草稿

## 默认输出格式

- `Venue Profile`
- `必须适配项`
- `建议适配项`
- `不属于 venue 适配的问题`
- `下一步执行链`

## 常见高风险信号

- 把项目局部规则误当作 venue 通用规则
- 把 venue 结构问题和内容正确性问题混在一起
- 页数要求、双盲要求或 checklist 要求记错年份
- 平台字段和论文正文内容不一致

## 与其他 Skills 的关系

- 具体 LaTeX 终审交给 `latex-submission-packager`
- 内容风险审计交给 `reviewer-risk-audit`
- 总流程由 `academic-paper-factory` 编排

## 标准输入

- 目标 venue 与年份
- 当前论文文件或草稿状态
- 是否需要平台字段
- 是否要求实际编译或只做适配清单

## 标准交付

- venue-specific 约束清单
- 当前差距与修改建议
- 需要 handoff 的后续 Skill
- 平台字段与投稿材料草稿

## Examples

- “把这篇稿子从 NeurIPS 风格改成 TPAMI 风格。”
- “准备投 OpenReview，帮我整理 submission fields。”
- “切到一个期刊版式后，哪些是必须改的，哪些只是建议改？”
