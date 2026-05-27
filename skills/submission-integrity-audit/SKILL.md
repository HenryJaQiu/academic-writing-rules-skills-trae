---
name: "submission-integrity-audit"
description: "联合审计图表出版质量、caption、一致性与 ethics/code/data/checklist 声明。Invoke when 用户临近投稿、检查 figures/tables、或核对 disclosure 与 submission 材料是否真实一致。"
---

# Submission Integrity Audit

这个 Skill 把原本容易分散在多个小能力里的两类高风险问题合并到一起：

- 图表是否达到可投稿、可审稿的出版质量
- 声明、checklist、ethics、code/data availability、supplementary 与正文是否真实一致

它不负责 LaTeX 编译、模板排版和文件打包，那部分交给 `latex-submission-packager`。它更像是投稿前的“语义一致性 + 合规完整性”审计层。

## 何时调用

- 用户说“投稿前再帮我查一轮硬伤”
- 用户重点担心 figures、tables、captions 是否专业
- 用户要检查 checklist / ethics / data availability / code availability / AI use disclosure
- 用户担心正文、supplementary、submission form 之间说法不一致
- 用户需要一个“可以提交 / 暂不建议提交”的合规判断

## 核心目标

1. 检查 figures、tables、captions 是否足以支撑审稿阅读体验
2. 检查披露与声明是否完整、真实、不过度承诺
3. 检查正文、附录、supplementary、checklist 与平台字段之间的一致性
4. 输出必须修复项，而不是泛泛建议

## 强制规则

- 不得建议虚写代码、数据、伦理审批、匿名仓库或 supplementary 内容
- 不得把“准备公开”写成“已公开”
- 不得把图表可读性问题当成纯审美问题忽略
- 若 venue 要求或披露政策未核实，必须显式标注

## 执行步骤

### Step 1: 图表出版级审计

逐项检查：

- 图内字体、线宽、marker、颜色区分度
- 黑白打印或投影下是否仍可读
- legend 是否遮挡、轴标签是否缺单位
- 小数位、显著性标记、误差条、CI、seeds 是否一致
- table 是否过宽、压缩过度或读者无法快速定位最佳结果

### Step 2: caption 与正文一致性检查

重点核对：

- caption 是否独立可读
- 正文、caption、figure/table 中的数字是否一致
- 图表标题是否过度宣称
- 主文与 appendix 的图表分工是否合理

### Step 3: disclosure 与 checklist 盘点

至少检查以下项目是否存在、是否真实：

- ethics / IRB / broader impact
- limitations
- code availability
- data availability
- reproducibility / checklist
- supplementary / appendix 承诺
- AI assistance disclosure（若项目或 venue 需要）

### Step 4: 跨材料一致性审计

核对下列材料之间是否一致：

- main paper
- appendix / supplementary
- checklist
- code / data / anonymous repository 说法
- submission system fields 草稿

### Step 5: 风险分级与提交判断

- `P0`：声明失真、关键材料缺失、图表明显误导或不可读
- `P1`：存在高风险不一致，容易被 reviewer / editor 抓住
- `P2`：可提交，但专业感和可信度仍可提升

## 默认输出格式

- `Figure / Table Findings`
- `Disclosure Findings`
- `Cross-Material Consistency Gaps`
- `Must-Fix Items`
- `Can Submit / Not Yet`

## 标准输入

- 目标 venue 与年份
- 草稿、figures、tables、captions
- checklist、ethics、code/data availability、supplementary 相关文本
- 是否已有 submission fields 草稿
- 用户最担心的风险：图表 / disclosure / checklist / 一致性

## 标准交付

- 图表出版质量问题清单
- 缺失或不准确的 disclosure 列表
- 正文与提交材料不一致的地方
- 必修项与建议项
- `可以提交 / 暂不建议提交`

## 不该做什么

- 不要把编译失败、模板错误、页数计算问题作为本 Skill 主任务
- 不要只说“图不够好看”，必须指出为何影响审稿
- 不要默认所有 venue 都要求相同 disclosure
- 不要把内容层的新颖性争议混入合规审计

## 与其他 Skills 的关系

- 实验叙事与图表角色分工来自 `experiment-story-builder`
- venue 规则来源于 `venue-submission-adapter`
- 文件与匿名提交包终审交给 `latex-submission-packager`
- 全局 reject risk 交给 `reviewer-risk-audit`
- 总流程由 `academic-paper-factory` 协调

## Examples

- “投稿前帮我检查 figures、captions 和 checklist，有没有会被一眼抓住的硬伤。”
- “正文说代码会提供，supplementary 里也提了，帮我核对这些 disclosure 是否一致。”
- “这些表格已经能投了吗？顺便看看 ethics、data availability 和 anonymous repo 的说法有没有踩雷。”
