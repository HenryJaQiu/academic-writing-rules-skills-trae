---
name: "writing-naturalness-guard"
description: "识别并消除学术稿件中的 AI 写作痕迹，同时保持技术含义不变。Invoke when 用户要去 AI 味、润色摘要/引言、或降低语言模板感。"
---

# Writing Naturalness Guard

这个 Skill 用于把“像 AI 写的学术文本”改成“像研究者亲手写的学术文本”，但不改变技术含义、数学口径、引用事实和结论强度。

它吸收了 `deai` 的优点，但改成了可跨会议/期刊复用的通用版本，不绑定 NeurIPS 或 ML theory 单一场景。

## 何时调用

- 用户说“去 AI 味”“去掉 AI 痕迹”“humanize”
- 用户想润色 abstract / introduction / related work / discussion / conclusion
- 用户担心句式模板感太强、读起来太像 LLM
- 草稿内容基本稳定，但风格还不自然

## 核心目标

1. 降低模板化连接词、句式和段落节奏
2. 保持技术内容、引用、公式、定理口径不变
3. 让不同 section 的风格更像研究者自然写作，而不是同一种机器节奏
4. 降低审稿人对“AI 生成痕迹”的主观反感

## 不可触碰的内容

- 不改公式、变量、定理编号、交叉引用
- 不擅自新增或删除引用
- 不把保守表述改成更强 claim
- 不把 theorem / experiment / citation 口径改写错

## 执行步骤

### Step 1: 先诊断，不先重写

优先标出高风险信号：

- 高频模板词：`Furthermore`, `Moreover`, `Additionally`, `Notably`, `Importantly`
- 句子开头过多使用 `We ...`
- 每段都按“主题句 -> 证据 -> 总结句”均匀展开
- Abstract 或 Conclusion 明显套模板
- 段落长度和句长过于均匀

### Step 2: 保留技术不变量

重写前明确哪些内容不能动：

- 结论强度
- 假设前提
- 实验设置
- 数值与比较方向
- 定理与 proof sketch 口径

### Step 3: 分 section 改写

按 section 选择不同策略：

- `Abstract`：减少模板句和空泛 pitch，首句直接进入问题与结果
- `Introduction`：减少口号和宽泛背景，强调具体局限与差异
- `Related Work`：避免流水账式“X did..., Y did...”，改为按问题轴组织
- `Method/Theory`：只做语言节奏调整，不动技术定义
- `Discussion/Conclusion`：避免三段式套话，用具体边界、开放问题或 take-away 收尾

### Step 4: 节奏与句式去同质化

优先做这些操作：

- 长句和短句交替
- 更换句首类型，而不是反复用 `We`
- 让一部分段落直接从事实、限制或观察切入
- 保留少量领域内自然表达，不追求“每句都完美对称”

### Step 5: 回查技术与风格

至少复查：

- 是否误改了结论强度
- 是否动了引用或数学含义
- 是否引入新的语言歧义
- 是否只是换同义词而没有真正打破模板节奏

## 默认输出格式

- `AI 痕迹诊断`
- `不可改动项`
- `改写后文本`
- `关键改动说明`
- `残余风险`

## 常见高风险信号

- Abstract 第一行写成 `In this paper, we propose...`
- Conclusion 落入 “In summary ... Future work ...” 固定模板
- Related Work 每句一个作者名，缺少比较结构
- 全文 “We show / We demonstrate / We highlight” 连续出现

## 与其他 Skills 的关系

- 内容主线来自 `paper-positioning`
- 风险优先级来自 `reviewer-risk-audit`
- 若只是做一轮局部修文，可与 `paper-ratchet-optimizer` 串联

## 标准输入

- 待处理段落、section 或整篇草稿
- 目标 venue 或期刊类型
- 用户是否允许只做风格改写、不改内容
- 用户最担心的 AI 痕迹类型

## 标准交付

- 风格风险诊断
- 改写后的文本
- 保持不变的技术口径说明
- 仍建议人工复核的句子

## Examples

- “把这个 abstract 去 AI 味，但不要改技术含义。”
- “related work 读起来太像 LLM 写的，帮我改自然一点。”
- “帮我做一轮全文语言自然化，不要碰定理和引用。”
