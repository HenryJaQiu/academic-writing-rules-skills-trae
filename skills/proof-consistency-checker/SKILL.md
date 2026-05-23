---
name: "proof-consistency-checker"
description: "审查定理、证明与正文口径的一致性，补全推导并标记无法成立之处。Invoke when 用户检查证明、补推导、或担心 theorem 与 appendix 不一致。"
---

# Proof Consistency Checker

这个 Skill 负责把“看起来像成立”的理论部分，升级成“口径一致、步骤可追踪、适合附录和审稿”的理论资产。

它吸收了 `theory` 的严格性，但改成了更适合跨领域学术写作复用的版本，不局限于某一类机器学习理论论文。

## 何时调用

- 用户说“检查推导”“补全证明”“verify the proof”
- 定理、引理、命题、proof sketch 需要扩展成完整版本
- 用户担心正文 theorem statement 与 appendix proof 不一致
- 用户怀疑某个 assumption 过强、缺失或没有被正确使用

## 核心目标

1. 检查 theorem / lemma / proposition 与正文表述的一致性
2. 明确每一步推导所依赖的定义、假设和标准结果
3. 找出不可修复的逻辑漏洞、缺失假设和循环论证
4. 生成可直接写入 appendix 的严谨推导草稿

## 强制规则

- 不允许引入未说明的新假设并假装原文已经有
- 不允许把经验现象包装成理论保证
- 不允许用“显然”“容易验证”跳过关键步骤
- 发现无法修复的问题时，必须明确报错而不是硬补

## 执行步骤

### Step 1: 建立核查清单

优先提取：

- 定理 / 引理 / 命题的正式陈述
- 依赖的 assumptions、definitions、notations
- 正文中的口头 claim
- appendix 中的 proof 或 sketch

### Step 2: 检查 statement 与 proof 口径

逐条核对：

- 范数、距离、概率模型是否一致
- 常数与对数因子是否一致
- 适用范围是否一致
- 正文是否把“存在性”写成了“算法性保证”
- 摘要/引言/结论是否把 theorem 的边界说大了

### Step 3: 逐步补全推导

每一步都要写清：

- 当前式子
- 来源：定义 / 假设 / 已证结论 / 标准定理
- 变形类型：代入、上界、下界、极限交换、矩阵恒等式等

### Step 4: 标记风险级别

- `P0`：命题在当前假设下不成立，或 proof 无法修复
- `P1`：statement / proof / 正文口径不一致
- `P2`：证明可成立，但写法跳步太多，不利于审稿

### Step 5: 生成 appendix-ready 输出

默认输出为可直接写入附录的结构：

- `Setup / Notation`
- `Proof of Theorem ...`
- `Step-by-step derivation`
- `Where the original draft was too strong`

## 默认输出格式

- `一致性诊断`
- `逐步推导`
- `缺失假设或错误`
- `建议如何收紧正文口径`
- `可直接写入附录的版本`

## 常见高风险信号

- theorem statement 比 proof 实际得到的更强
- proof 中出现未定义符号或“因此可得”式跳步
- 引言说“optimal”，证明只给了上界
- 正文说“training dynamics”，附录只证明了存在性近似误差

## 与其他 Skills 的关系

- 新颖性和 framing 由 `paper-positioning` 负责
- 理论边界的审稿风险可交由 `reviewer-risk-audit`
- 若修完证明后还要连带修改摘要/结论，可接 `paper-ratchet-optimizer`

## 标准输入

- 定理、引理或命题文本
- 对应 proof / appendix 草稿
- 相关 assumptions 和 definitions
- 用户最担心的数学风险

## 标准交付

- theorem / proof 一致性结果
- 逐步推导草稿
- 无法成立或需要补假设的部分
- 正文需同步修改的 claim 列表

## Examples

- “帮我检查 Theorem 2 和 appendix 里的证明是否一致。”
- “把这段 proof sketch 补成可放附录的完整证明。”
- “这个 lower bound 证明有没有跳步或缺条件？”
