---
name: "experiment-story-builder"
description: "把实验设计、图表、caption 和统计结果组织成可审稿证据链。Invoke when 用户设计实验、生成图表、写实验节，或区分技术类与 benchmark/evaluation 类实验叙事。"
---

# Experiment Story Builder

这个 Skill 用于把“有实验结果”升级为“有说服力、低风险、可审稿的实验叙事”。它专门解决 AI 论文中最常见的问题：实验很多，但故事线弱；图很多，但证据链散；结果有提升，但审稿人不信。

## 何时调用

- 用户开始设计实验 section
- 用户已有结果图、表、日志，但不知道怎么组织论文叙事
- 用户要生成或修订 figure / table / caption
- 用户担心 baseline 不公平、统计不规范、图文不一致
- 用户准备投稿前做“实验是否足够支撑主张”的终审

## 核心目标

1. 让实验直接服务论文主张，而不是堆结果
2. 让每张图表都有明确任务，不重复、不游离
3. 让 caption 自洽、设置完整、可复现
4. 让统计规范、baseline 公平、视觉表达清晰
5. 让 reviewer 能顺着实验 section 快速确认核心结论

## 工作模式

### `core-experiment-story`

默认模式，适合绝大多数方法型或理论+实验型论文：

- 主结果证明核心主张成立
- 消融和机制分析解释为什么有效
- 鲁棒性测试说明边界

### `benchmark-evaluation mode`

适合 benchmark、evaluation、diagnostic、scaling、meta-evaluation 类论文：

- 不把重点放在“我的方法赢了多少”
- 把重点放在“这个评估资产纠正了什么误判、揭示了什么失效、为什么对社区有价值”
- 强调覆盖性、公平性、诊断性、可复用性和结论稳定性

## 默认证据链

一篇 AI 论文的实验节优先按以下逻辑组织：

1. 主结果：证明论文主张确实成立
2. 关键消融：证明不是偶然提升
3. 机制分析：解释为什么有效
4. 鲁棒性或边界测试：说明何时有效、何时失效
5. 质性可视化：帮助理解，但不替代主证据

若是 `benchmark-evaluation mode`，证据链优先改为：

1. 评估缺口：现有 benchmark / protocol 哪里不足
2. 覆盖性：任务、数据、指标、setting 是否足够代表问题空间
3. 诊断性：新 benchmark / protocol 是否真的揭示了旧结论看不到的差异
4. 稳定性：不同 seeds、子集、评估设置下结论是否稳
5. 社区价值：是否能改变方法排序、暴露失效模式、或指导后续研究

## Step 1: 先写实验主张，不先画图

对每个实验块，先回答：

- 这组实验要证明什么？
- 如果 reviewer 质疑这点，哪张图/哪个表能直接回应？
- 这是主证据、辅助证据还是 sanity check？

模板：

- `Main result`: 证明方法/理论在核心任务上成立
- `Ablation`: 证明关键模块必要
- `Mechanism`: 证明你提出的解释不是空话
- `Boundary`: 说明限制与适用范围

若是 `benchmark-evaluation mode`，改成：

- `Gap`: 现有 benchmark / evaluation protocol 漏掉了什么
- `Coverage`: 你的评估资产覆盖了哪些关键维度
- `Diagnosis`: 哪些旧结论被重新解释、推翻或细化
- `Boundary`: 你的 benchmark / evaluation 资产在哪些范围内有效

## Step 2: 建图表任务清单

每个 figure/table 必须明确角色：

- `Figure 1`: 总体现象或框架图
- `Table 1`: 主 benchmark / 主结果
- `Figure 2`: scaling trend / loss curve / mechanism
- `Table 2`: ablation
- `Appendix Figure/Table`: 额外设置、更多维度、鲁棒性

若是 `benchmark-evaluation mode`，优先考虑：

- `Table 1`: benchmark / dataset / protocol coverage summary
- `Figure 1`: 旧评估结论与新评估结论的对比或失效案例
- `Table 2`: 不同方法在新 protocol 下的重排或稳定性结果
- `Appendix Figure/Table`: 更多子群、更多 metrics、更多 seeds、更多诊断维度

删除信号：

- 两张图证明的是同一件事
- 图内信息不能比正文多任何关键证据
- caption 不能独立让人明白图在说什么

## Step 3: baseline 与实验公平性审查

逐项检查：

- baseline 是否是当前最强或最相关路线
- 超参数预算是否公平
- 训练步数、模型规模、数据量是否可比
- 是否只挑对自己有利的 setting
- 是否缺少最自然的负对照

若是 `benchmark-evaluation mode`，额外检查：

- 是否只选择对某类方法有利的任务、数据或指标
- benchmark 构造规则是否让部分方法天然吃亏
- protocol change 是否足以解释结论重排，而不是引入新偏置
- 是否对所有比较对象使用同样的预算、清洗、过滤和报分规则

高风险信号：

- 只和老 baseline 比
- 只汇报最好 seed
- 缺标准差/置信区间
- 图里看起来提升，表里并不稳定
- 声称 benchmark 更公平，但没有展示公平性或覆盖性证据
- 声称改变了社区认知，但只展示了少量 cherry-picked 方法

## Step 4: 统计规范

默认要求：

- 关键结果用 3–10 个 seeds，优先 5+
- 报均值与方差、标准差或 95% CI
- 说明拟合区间、统计口径、筛选规则
- 若是 log-log slope / scaling 指数，写清全区间还是 tail-fit

若统计不足，必须降级表述：

- 从 `improves performance` 降为 `shows a favorable trend`
- 从 `consistently outperforms` 降为 `is competitive in the tested settings`

若是 `benchmark-evaluation mode`，还要额外控制：

- 从 `reveals a general failure mode` 降为 `reveals a failure mode in the tested protocol`
- 从 `changes the ranking of current methods` 降为 `changes the ranking under this evaluation setup`

## Step 5: caption 写法

caption 必须能单独回答：

- 任务/数据/设置是什么
- 横轴纵轴是什么
- 统计量是什么
- 阴影、误差线、颜色、线型表示什么
- 读者应该看到的结论是什么，但不能夸大

禁止：

- 图内再放结论性标题，和 caption 互相打架
- caption 不写 seed / CI / fit protocol
- caption 写出的结论比图本身更强
- 通过调大图内字体“硬撑可读性”但不检查遮挡与越界，导致 legend 压住曲线、标题被裁切、tick label 出界（必须在导出的图 PDF 和编译后的 paper.pdf 里双重检查）

## Step 6: 主文与附录分工

主文应保留：

- 最能支撑核心 claim 的结果
- 必要的消融和机制图
- 一张能让 reviewer 快速建立信心的主表或主图

附录应承接：

- 更多维度
- 更多 seeds
- 额外鲁棒性
- 非主线但能增强可信度的结果

若是 `benchmark-evaluation mode`：

- 主文优先放最能说明评估缺口、方法重排、失效模式和社区价值的结果
- 附录承接更细的 breakdown、更多 task families、更多 metrics、更多 subgroup 分析

## 默认输出格式

- `实验主张地图`
- `图表任务分配`
- `baseline / 统计风险`
- `caption 修改建议`
- `主文 vs 附录安排`
- `还缺哪些关键实验`

若用户明确是 benchmark/evaluation 稿件，可追加：

- `Coverage / Fairness Risks`
- `Re-ranking or Diagnostic Value`
- `Type-Specific Overclaim Warnings`

## 常见 reviewer 攻击点

- “实验没证明核心贡献，只证明你调参了”
- “图里看不出显著差异”
- “caption 不足以复现”
- “只在对你有利的 setting 有效”
- “机制解释没有直接证据”
- “这更像换了个评测口径，并没有证明新 protocol 更好”
- “benchmark coverage 不够，不能支持你的社区结论”

## 与其他 Skills 的关系

- 选题主线来自 `paper-positioning`
- 若前面已在 `paper-positioning` 中确定为 `benchmark/evaluation` 类论文，这里应沿用同一类型模板
- 若要先设计动机图、总览图或主结果图，前置 `figure-design-advisor`
- 总控流程由 `academic-paper-factory` 协调
- 最终硬伤审计交给 `reviewer-risk-audit`
- 若要分轮优化实验节，交给 `paper-ratchet-optimizer`

## 标准输入

- 当前实验主张或论文主线
- 图表草稿、结果表、日志或 caption 初稿
- baseline 列表与实验设置
- 统计信息：seeds、均值、方差、CI、拟合区间
- 用户最担心的 reviewer 攻击点
- 是否更像 `方法/理论+实验`，还是 `benchmark/evaluation` 类论文

## 标准交付

- 实验主张地图
- 图表角色分工
- baseline / 统计风险清单
- caption 修改建议
- 主文与附录拆分建议
- 必补实验与可选实验
- 若需要，附 benchmark/evaluation 模式下的 coverage / fairness / overclaim 风险

## Examples

- “这些实验结果怎么组织成 NeurIPS 风格的实验节？”
- “帮我看 Table 1、Figure 2 和 appendix 里的图应该怎么分工。”
- “这套 ablation 和 caption 够不够支撑我的主要 claim？”
