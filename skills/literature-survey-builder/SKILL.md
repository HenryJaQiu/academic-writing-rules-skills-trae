---
name: "literature-survey-builder"
description: "组织文献地图、系统综述模式、差异表和 related work 资产。Invoke when 用户要系统梳理方向、补近年工作、或把文献变成可写作的证据底座。"
---

# Literature Survey Builder

这个 Skill 负责把“零散找文献”升级成“可投稿使用的系统性综述资产”。它既服务前期选题定位，也服务中后期的 related work、差异表和综述段落撰写。

参考了近期热门科研写作与 Claude research skill 仓库里最稳定的几类模式：

- lit-review mode：快速搭建方向地图
- systematic-review mode：保留检索逻辑、筛选标准和 coverage 说明
- fact-check friendly survey：文献结论要能回到证据，而不是只靠印象

但这里不绑定特定工具，不要求 Zotero，也不把 PRISMA 或系统综述流程写死成唯一模式。

## 何时调用

- 用户要系统梳理一个研究方向
- 用户说“先帮我过一遍最近两年文献”
- 用户要写 related work，但当前只有零散引用
- 用户要做论文定位、差异表、代表工作分组
- 用户准备写 survey 段落或需要给学生/合作者一份文献地图
- 用户要做更严格的 systematic review / scoping review / PRISMA-lite 文献整理

## 核心目标

1. 建立一个面向投稿的文献地图，而不是随意堆 citation
2. 覆盖近 2-5 年代表工作，同时保留必要经典奠基文献
3. 输出可直接服务 `paper-positioning`、`grounded-paper-qa` 和 `citation-reality-guard` 的结构化资产
4. 形成差异表、主题分组、研究空白和 related work 初稿
5. 在需要时提供更严格的检索记录、筛选逻辑和 coverage stopping rule

## 工作模式

### `scan`

适合快速定方向：

- 快速收集代表工作
- 识别主题簇
- 找近两年高相关工作

### `lit-review`

适合大多数论文 related work：

- 建立主题分组
- 做差异表
- 生成可写段落蓝图

### `systematic-review`

适合需要更强检索可解释性的任务：

- 记录检索词与时间窗
- 记录纳入/排除标准
- 保留 seed papers 与扩展逻辑
- 输出 PRISMA-lite 或 scoping review 风格的覆盖说明

## 执行步骤

### Step 1: 明确任务类型与范围

先收集：

- 研究主题与关键词
- 目标 venue / 学科方向
- 年份窗口，默认近 2-5 年
- 论文类型：理论 / 方法 / 系统 / 实证 / Agent
- 工作模式：`scan / lit-review / systematic-review`
- 是否要最终落成 related work 初稿、survey 备忘录或系统综述说明

若用户要求“系统综述”，必须补充：

- 检索词
- 纳入标准
- 排除标准
- 需要保留的数据库/来源边界

### Step 2: 建 seed set 与扩展策略

不要一开始无穷铺开，先建立高质量 seed set：

- 已知代表论文
- 最近两年高相关工作
- 必要经典文献

然后再决定是否做扩展：

- backward citation expansion：追关键工作的参考文献
- forward citation expansion：看哪些后续工作沿着同一问题线推进
- author / lab / benchmark line expansion：按作者群、实验平台或方法谱系追踪

### Step 3: 建文献分组

优先按“研究问题”和“比较轴”而不是按“时间顺序”分组，例如：

- 问题定义与经典基线
- 最近方法线 A
- 最近方法线 B
- 理论分析线
- 系统/应用线
- 失败路线或局限突出路线

### Step 4: 建差异表

对每组代表工作提炼：

- 解决的问题
- 核心方法或理论工具
- 证据类型：theory / experiment / benchmark / system
- 主要局限
- 与本文的关系：替代 / 补充 / 泛化 / 对立 / 并行
- 是否适合被写进 related work 主文，还是只放附录/背景

### Step 5: 识别研究空白

回答：

- 近作之间到底还缺什么
- 哪些空白是真空白，哪些只是实验没补全
- 哪些 gap 只是口号，哪些有足够证据支持
- 你的论文最适合占据哪一个 gap

gap 判断必须尽量回到证据，不要只凭“好像没人做过”。

### Step 6: 决定 coverage 是否足够

至少给出一个 stopping rule，例如：

- 同一主题簇已经出现明显重复，新增论文不再带来新差异轴
- 近两年顶会/主流期刊相关路线已经覆盖到主要代表工作
- 对关键 claim 已经有足够支撑，不再只是单篇孤证

如果还没有到 stopping point，就明确哪些子方向需要继续补。

### Step 7: 输出可写作资产

输出：

- 文献分组清单
- 差异表
- related work 大纲
- related work 初稿段落
- 近年代表工作补充建议
- 若是系统综述模式，再输出检索词、筛选逻辑和 coverage 说明

## 低风险规则

- 不要只列经典工作而忽略近作
- 不要把不同问题设置的论文硬拉到同一赛道比较
- 不要只按作者或会议堆列表，必须写出差异轴
- 不要在未核验真实性前写死关键定位句
- 不要把“搜到的论文数很多”误当成“覆盖已经充分”
- 不要把笔记或二手摘要直接当成原文结论

## 标准输入

- 研究主题或问题定义
- 目标 venue 与年份
- 已有引用条目、候选论文、笔记或文献卡片
- 希望覆盖的年份范围
- 工作模式：`scan / lit-review / systematic-review`
- 是否需要最终落成 related work 初稿

## 标准交付

- 文献主题地图
- 近年代表工作列表
- 差异维度表
- 研究空白判断
- coverage 是否足够的说明
- related work 大纲
- related work 初稿或摘要版
- 若需要，附检索与筛选记录

## Examples

- “先帮我系统梳理 2023-2026 的 sparse MoE 理论文献，并做一个差异表。”
- “我想投 NeurIPS，帮我把 related work 的文献地图搭起来。”
- “不要只给 citation list，帮我整理成可以直接写进论文的 survey 资产。”
- “按 systematic review 的方式梳理 AI for peer review 最近两年的工作，给我一个 PRISMA-lite 记录。”

## 与其他 Skills 的关系

- 若用户先想从给定论文回答具体问题，前置 `grounded-paper-qa`
- 若已有大量阅读笔记和摘录，前置 `reading-note-synthesizer`
- 前期定位交给 `paper-positioning`
- 引用真实性与元数据核验交给 `citation-reality-guard`
- 总流程由 `academic-paper-factory` 组织
- 如果用户请求模糊，先经 `paper-intake-router` 分流
