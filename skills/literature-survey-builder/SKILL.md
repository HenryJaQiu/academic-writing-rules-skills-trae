---
name: "literature-survey-builder"
description: "组织文献综述、近年覆盖、差异表和 related work 初稿。Invoke when 用户要系统梳理文献、补 survey、或为定位与 related work 建证据底座。"
---

# Literature Survey Builder

这个 Skill 负责把“零散找文献”升级成“可投稿使用的系统性综述资产”。它既服务前期选题定位，也服务中后期的 related work、差异表和综述段落撰写。

## 何时调用

- 用户要系统梳理一个研究方向
- 用户说“先帮我过一遍最近两年文献”
- 用户要写 related work，但当前只有零散引用
- 用户要做论文定位、差异表、代表工作分组
- 用户准备写 survey 段落或需要给学生/合作者一份文献地图

## 核心目标

1. 建立一个面向投稿的文献地图，而不是随意堆 citation
2. 覆盖近 2–5 年代表工作，同时保留必要经典奠基文献
3. 输出可直接服务 `paper-positioning` 和 `citation-reality-guard` 的结构化资产
4. 形成差异表、主题分组、研究空白和 related work 初稿

## 执行步骤

### Step 1: 明确检索范围

先收集：

- 研究主题与关键词
- 目标 venue / 学科方向
- 年份窗口，默认近 2–5 年
- 论文类型：理论 / 方法 / 系统 / 实证 / Agent

### Step 2: 建文献分组

优先按“研究问题”而不是按“时间顺序”分组，例如：

- 问题定义与经典基线
- 最近方法线 A
- 最近方法线 B
- 理论分析线
- 系统/应用线

### Step 3: 建差异表

对每组代表工作提炼：

- 解决的问题
- 核心方法或理论工具
- 证据类型：theory / experiment / benchmark / system
- 主要局限
- 与本文的关系：替代 / 补充 / 泛化 / 对立 / 并行

### Step 4: 识别研究空白

回答：

- 近作之间到底还缺什么
- 哪些空白是真空白，哪些只是实验没补全
- 你的论文最适合占据哪一个 gap

### Step 5: 输出可写作资产

输出：

- 文献分组清单
- 差异表
- related work 大纲
- related work 初稿段落
- 近年代表工作补充建议

## 低风险规则

- 不要只列经典工作而忽略近作
- 不要把不同问题设置的论文硬拉到同一赛道比较
- 不要只按作者或会议堆列表，必须写出差异轴
- 不要在未核验真实性前写死关键定位句

## 标准输入

- 研究主题或问题定义
- 目标 venue 与年份
- 已有引用条目或候选论文
- 希望覆盖的年份范围
- 是否需要最终落成 related work 初稿

## 标准交付

- 文献主题地图
- 近年代表工作列表
- 差异维度表
- 研究空白判断
- related work 大纲
- related work 初稿或摘要版

## Examples

- “先帮我系统梳理 2023–2026 的 sparse MoE 理论文献，并做一个差异表。”
- “我想投 NeurIPS，帮我把 related work 的文献地图搭起来。”
- “不要只给 citation list，帮我整理成可以直接写进论文的 survey 资产。”

## 与其他 Skills 的关系

- 前期定位交给 `paper-positioning`
- 引用真实性与元数据核验交给 `citation-reality-guard`
- 总流程由 `academic-paper-factory` 组织
- 如果用户请求模糊，先经 `paper-intake-router` 分流
