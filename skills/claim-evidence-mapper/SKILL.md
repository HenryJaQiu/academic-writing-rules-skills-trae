---
name: "claim-evidence-mapper"
description: "把论文 claims 映射到 theorem、experiment、figure、table 和 citation 证据。Invoke when 用户要检查证据闭环、发现 claim 偏大、或投稿前核对每个主张是否被支撑。"
---

# Claim Evidence Mapper

这个 Skill 负责把论文中的主张从“听起来合理”变成“有明确证据承接、可被审稿人逐条核对”的结构化资产。

它不直接替代 `experiment-story-builder`、`proof-consistency-checker` 或 `citation-reality-guard`，而是站在它们之上，做一层显式的 claim-to-evidence 映射与风险分级。

## 何时调用

- 用户说“这些 claim 够不够被支撑”
- 用户担心 abstract / intro / conclusion 写得比正文更大
- 用户要检查每个 contribution 是否都能对应到 theorem、experiment、figure、table 或 citation
- 做大改后，需要确认本轮修改没有破坏证据闭环
- 投稿前或 rebuttal 前，需要整理一份 reviewer 可追踪的证据地图

## 核心目标

1. 提取论文中的核心 claims，而不是只看措辞
2. 为每个 claim 找到直接证据与间接证据
3. 识别“证据缺失”“证据错位”“证据强度不足”“claim 超出边界”
4. 输出可直接服务审稿、改稿和 rebuttal 的 claim-evidence matrix

## 强制规则

- 不允许把“经验现象”包装成“理论保证”
- 不允许把“附录局部结果”写成“正文主结论已完全证明”
- 不允许把“引用中的背景事实”当成“本文贡献证据”
- 发现证据不足时，优先建议收紧 claim，而不是硬补空话

## 执行步骤

### Step 1: 抽取 claims

按强度分层抽取：

- `C0`：背景事实或领域共识
- `C1`：本文的局部观察或经验发现
- `C2`：本文的核心方法/理论/系统贡献
- `C3`：摘要、引言、结论中的强主张

优先关注：

- 标题
- Abstract 每一句
- Introduction 中的 contribution bullets
- Conclusion 中的总结句
- theorem statement、实验主结论、图表标题与 caption 中的结论句

### Step 2: 建立证据清单

对每个 claim，标记其证据来源：

- `T`：theorem / lemma / proposition / proof
- `E`：experiment / ablation / benchmark / user study
- `F`：figure / table / caption
- `R`：reference / prior work
- `A`：appendix / supplementary

每条证据都要回答：

- 证据是直接支撑还是间接支撑
- 证据是否只在特定设定下成立
- 证据是否已经在正文中被清楚引用

### Step 3: 评估支撑强度

建议分四档：

- `S0`：没有证据或证据找不到
- `S1`：只有弱相关、间接或背景性证据
- `S2`：有证据，但边界、设定或力度不足
- `S3`：有直接且一致的强证据

### Step 4: 标记失配类型

高频问题包括：

- `P0`：claim 明显超出已有证据
- `P1`：正文有证据，但 abstract / intro / conclusion 写大了
- `P1`：证据存在，但分散在 appendix 或 caption，主文没有承接
- `P2`：证据足够，但呈现顺序让审稿人不易跟踪

### Step 5: 输出修复建议

修复动作优先级：

1. 收紧 claim
2. 补明确指向的 figure / table / theorem / citation
3. 把附录中的关键支撑上提到主文
4. 若仍缺证据，再 handoff 给对应 Skill 补实验、补证明或补文献

## 默认输出格式

- `Claim Inventory`
- `Evidence Map`
- `Unsupported or Overstated Claims`
- `Suggested Downgrades or Additions`
- `Next Skill Handoff`

## 标准输入

- 当前草稿、指定 section 或 claim 列表
- 相关 theorem、figures、tables、captions、appendix、references
- 用户最担心的 claim 类型：新颖性 / 理论 / 实验 / 系统 / 应用
- 是否允许只收紧表述、不新增内容

## 标准交付

- claim-evidence matrix
- 每个核心 claim 的支撑强度等级
- 需要降级的表述
- 需要补充的证据位置
- 推荐后续 Skill：实验 / 证明 / 引用 / 审稿风险

## 不该做什么

- 不要把所有句子都当成 claim，失去优先级
- 不要只报“证据不足”，必须指出缺哪类证据
- 不要建议新增用户并不存在的数据、代码或实验结果
- 不要把格式问题误判为证据问题

## 与其他 Skills 的关系

- claim 来自 `paper-positioning`
- 实验证据缺口交给 `experiment-story-builder`
- 理论证据缺口交给 `proof-consistency-checker`
- 引用型证据缺口交给 `citation-reality-guard`
- 最终审稿风险交给 `reviewer-risk-audit`
- 总流程由 `academic-paper-factory` 组织

## Examples

- “帮我看 abstract 里的每一句 claim 在正文里有没有证据承接。”
- “这些 contribution bullets 会不会写太大？请给我一个 claim-evidence matrix。”
- “投稿前做一轮证据闭环检查，指出哪些结论该降级，哪些必须补实验或补引用。”
