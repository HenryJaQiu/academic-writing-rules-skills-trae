# Skills Index

这份索引文档用于快速回答三个问题：

- 这个 Skill 做什么
- 什么时候该调用它
- 它通常接在谁前面或后面

详细规范见：

- `PLAYBOOK.md`
- `CONVENTIONS.md`

## 入口层

### `paper-intake-router`

- 作用：识别论文所处阶段、主风险和下一步 Skill 链
- 何时调用：用户请求模糊、任务跨多个阶段、或需要自动分流
- 常见后续：`academic-paper-factory`、`paper-positioning`、`reviewer-risk-audit`

## 总控层

### `academic-paper-factory`

- 作用：统筹从 idea 到 submission-ready 的完整推进流程
- 何时调用：从零开始写论文，或需要把多个写作子任务组织成稳定流程
- 常见后续：`paper-positioning`、`literature-survey-builder`、`claim-evidence-mapper`

## 核心执行层

### `paper-positioning`

- 作用：澄清问题定义、差异点、最小贡献集、intro 主线和不能乱说的边界
- 何时调用：选题、定题、标题摘要反复改但贡献仍不清，或需要 introduction flowchart / 技术类与 benchmark 类定位模板
- 常见后续：`literature-survey-builder`、`claim-evidence-mapper`

### `idea-evaluator`

- 作用：从导师视角评估 idea 的价值、可行性、能力匹配和投稿潜力
- 何时调用：用户还在问“值不值得做”“先做哪个 idea”“能不能投”
- 常见后续：`paper-positioning`、`venue-fit-selector`

### `literature-survey-builder`

- 作用：系统整理文献地图、差异表、coverage 说明和 related work 资产
- 何时调用：需要补 related work、比较近两年工作、建立综述视图，或做更严格的 systematic-review 模式
- 常见后续：`paper-positioning`、`citation-reality-guard`

### `grounded-paper-qa`

- 作用：只基于指定论文、摘录和笔记做带出处问答与事实核查
- 何时调用：用户要“只根据这些论文回答”，或先问清文献再写作
- 常见后续：`literature-survey-builder`、`claim-evidence-mapper`

### `reading-note-synthesizer`

- 作用：把读书笔记、高亮、摘录和会议记录整理成结构化写作资产
- 何时调用：用户已有很多阅读材料，但还没有形成 related work、定位或 rebuttal 素材
- 常见后续：`literature-survey-builder`、`paper-positioning`

### `claim-evidence-mapper`

- 作用：把 claims 显式映射到 theorem、experiment、figure、table、citation
- 何时调用：担心 abstract / intro / conclusion 写大了，或要做 claim-evidence matrix
- 常见后续：`experiment-story-builder`、`proof-consistency-checker`、`reviewer-risk-audit`

### `citation-reality-guard`

- 作用：核验引用真实存在、可查询性、来源交叉确认、误匹配风险与正文描述准确性
- 何时调用：补参考文献、检查幻觉引用、准备投稿前事实核查，或怀疑某条引用是低置信匹配
- 常见后续：`reviewer-risk-audit`、`latex-submission-packager`

### `experiment-story-builder`

- 作用：组织实验叙事、图表角色、baseline、ablation、caption 和统计信息，并支持 benchmark/evaluation 类实验模板
- 何时调用：实验很多但不知道怎么讲，实验节说服力不够，或需要区分技术类与 benchmark/evaluation 类实验叙事
- 常见后续：`submission-integrity-audit`、`claim-evidence-mapper`

### `figure-design-advisor`

- 作用：把图的表达目标转成动机图、方法总览图或实验结果图的设计方案
- 何时调用：用户要设计核心 figure、优化图的信息层次，或不知道一张图该怎么画才像顶会
- 常见后续：`experiment-story-builder`、`submission-integrity-audit`

### `latex-first-compile-bootstrap`

- 作用：解决论文项目初次编译、依赖缺失和最小可编译版本问题
- 何时调用：新接手 LaTeX 项目，或还没法稳定编译 PDF
- 常见后续：`latex-submission-packager`

### `proof-consistency-checker`

- 作用：检查 theorem、assumption、proof、appendix 之间的一致性
- 何时调用：理论口径不稳、证明有跳步、附录与主文不一致
- 常见后续：`claim-evidence-mapper`、`reviewer-risk-audit`

### `writing-naturalness-guard`

- 作用：用模式库降低模板化和 AI 味，并先检查提示语残留、元编辑残留、明显引用风险与二次审计信号
- 何时调用：用户明确要求 humanize，或稿件读起来过于规整，或怀疑存在 ChatGPT 残留、AI 痕迹或需做 voice calibration
- 常见后续：`paper-ratchet-optimizer`、`citation-reality-guard`

### `reviewer-risk-audit`

- 作用：从审稿人视角做结构化发现、量化打分、confidence 评估和 `GO / CONDITIONAL GO / NO-GO` 决策，并支持技术类与 benchmark/evaluation 类论文的类型化校准
- 何时调用：用户说“review 一下”“投稿前找硬伤”，或需要按评分矩阵和论文类型判断当前版本值不值得投
- 常见后续：`paper-ratchet-optimizer`、`submission-integrity-audit`

### `paper-ratchet-optimizer`

- 作用：按棘轮式迭代原则读取 `Findings + Scorecard + Decision`，只修一个高杠杆问题并保留可验证改进，并支持技术类与 benchmark/evaluation 类论文的差异化修复
- 何时调用：中后期精修、审稿后按 findings 修稿、反复修改后需要控回归，或需要按论文类型决定先修“方法是否成立”还是“评估资产是否可信”
- 常见后续：`reviewer-risk-audit`、`writing-naturalness-guard`

### `venue-fit-selector`

- 作用：在多个会议/期刊之间比较 fit、成熟度、改投成本和风险
- 何时调用：还没定 venue、想比较会议与期刊路线、或准备转投
- 常见后续：`venue-submission-adapter`、`submission-integrity-audit`

### `submission-integrity-audit`

- 作用：联合审计图表出版质量、caption、一致性与 disclosure/checklist
- 何时调用：临近投稿，担心图表质量、声明、supplementary 或提交材料不一致
- 常见后续：`latex-submission-packager`

### `latex-submission-packager`

- 作用：检查编译、匿名性、文件完整性和最终 submission package
- 何时调用：准备正式投稿、需要确认可交付的 LaTeX 输出
- 常见后续：`venue-submission-adapter`

### `venue-submission-adapter`

- 作用：把稿件适配到某个已明确 venue 的页数、格式、平台和匿名要求
- 何时调用：目标会议/期刊已知，需要 venue-specific 改造
- 常见后续：`latex-submission-packager`

## 投稿后阶段

### `rebuttal-response-drafter`

- 作用：拆解 reviewer comments、制定回应策略并起草 rebuttal / response letter
- 何时调用：已经收到审稿意见
- 常见前置：`reviewer-risk-audit`

## 快速选择

- 需求模糊：`paper-intake-router`
- 从零起稿：`academic-paper-factory`
- idea 值不值得做：`idea-evaluator`
- 定位不清：`paper-positioning`
- 文献综述：`literature-survey-builder`
- 指定论文问答：`grounded-paper-qa`
- 读书笔记整理：`reading-note-synthesizer`
- claim 过大：`claim-evidence-mapper`
- 引用核验：`citation-reality-guard`
- 实验叙事：`experiment-story-builder`
- 核心图设计：`figure-design-advisor`
- 理论核查：`proof-consistency-checker`
- 去 AI 味或查提示语残留：`writing-naturalness-guard`
- 审稿前找硬伤或打分：`reviewer-risk-audit`
- 按 findings 做单轮高杠杆修复：`paper-ratchet-optimizer`
- 选投哪里：`venue-fit-selector`
- 投稿一致性审计：`submission-integrity-audit`
- 最终打包：`latex-submission-packager`
- venue 格式适配：`venue-submission-adapter`
- rebuttal：`rebuttal-response-drafter`
