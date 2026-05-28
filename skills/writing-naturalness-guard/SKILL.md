---
name: "writing-naturalness-guard"
description: "识别并消除学术稿件中的 AI 写作痕迹，同时检查提示语残留、引用风险与未披露生成风险。Invoke when 用户要去 AI 味、润色摘要/引言、或担心稿件存在明显 AI 残留。"
---

# Writing Naturalness Guard

这个 Skill 用于把“像 AI 写的学术文本”改成“像研究者亲手写的学术文本”，但不改变技术含义、数学口径、引用事实和结论强度。

它吸收了 `deai` 的优点，但改成了可跨会议/期刊复用的通用版本，不绑定 NeurIPS 或 ML theory 单一场景。

它不仅处理“文风像 AI”，还要先拦截两类更危险的问题：

- 提示语残留、元指令残留、system prompt 残留
- 幻觉引用、错引、以及可能涉及未披露实质性生成的合规风险

## 何时调用

- 用户说“去 AI 味”“去掉 AI 痕迹”“humanize”
- 用户想润色 abstract / introduction / related work / discussion / conclusion
- 用户担心句式模板感太强、读起来太像 LLM
- 用户担心稿件里残留了 ChatGPT 式提示语、编辑元话语或奇怪的自动生成痕迹
- 草稿内容基本稳定，但风格还不自然

## 核心目标

1. 降低模板化连接词、句式和段落节奏
2. 保持技术内容、引用、公式、定理口径不变
3. 让不同 section 的风格更像研究者自然写作，而不是同一种机器节奏
4. 在改写前先识别高风险残留，避免把伦理风险误当成普通润色问题
5. 降低审稿人对“AI 生成痕迹”的主观反感

## 不可触碰的内容

- 不改公式、变量、定理编号、交叉引用
- 不擅自新增或删除引用
- 不把保守表述改成更强 claim
- 不把 theorem / experiment / citation 口径改写错
- 不把需要 disclosure 的实质性 AI 生成痕迹“洗掉”后当作纯人工写作

## 风险分级

### `P0`: 必须先拦截，不得直接润色

- 出现明显提示语残留，如 `As an AI language model`, `Certainly, here is`, `Below is a revised version`
- 出现元编辑残留，如 “maintain technical meaning”, “do not change citations”, “rewrite for clarity”
- 出现 system prompt / role prompt / user prompt 泄漏片段
- 文中引用疑似不存在、年份/venue 明显不对、或 claim 与引用关系显著异常
- 文本暴露出可能存在未披露的实质性生成式 AI 代写，而不是单纯 copy editing

### `P1`: 高风险，需要显式标记后再改

- Abstract、Introduction、Conclusion 明显由同一模板批量生成
- Related Work 呈现流水账式作者串联，且比较逻辑几乎缺失
- 全文高频使用 `We show / We demonstrate / We highlight`
- 段落结构和句长高度均匀，显示出明显的机器节奏

### `P2`: 一般风格问题

- 连接词偏多
- 句首类型单一
- 结尾收束略显模板化
- 局部语气过度平滑或过度对称

## 边界说明

- “AI 味重”本身通常不是学术不端，它首先是文风问题。
- 但以下问题不是普通文风问题，而是事实或合规风险：
- 幻觉引用、错引、虚构 DOI / venue / accepted status
- 未披露的实质性生成式 AI 使用
- 提示语残留暴露出未经人工审查的大段生成内容
- AI 生成图像、图表、分析或结论却被包装成人工原创内容

发现这些问题时，应先做风险处置，而不是立即开始自然化改写。

## 执行步骤

### Step 1: 先做门禁检查，不先重写

优先标出高风险信号：

- 高频模板词：`Furthermore`, `Moreover`, `Additionally`, `Notably`, `Importantly`
- 句子开头过多使用 `We ...`
- 每段都按“主题句 -> 证据 -> 总结句”均匀展开
- Abstract 或 Conclusion 明显套模板
- 段落长度和句长过于均匀
- 提示语残留：`As an AI language model`, `Certainly`, `Below is`, `Here is a revised version`
- 元编辑残留：`maintain technical meaning`, `do not change citations`, `rewrite the paragraph`
- 对引用、年份、venue、接收状态的可疑陈述

若命中 `P0`：

- 暂停风格改写
- 先输出风险原因
- 若涉及引用真实性，转交 `citation-reality-guard`
- 若涉及整体伦理/投稿风险，建议联动 `reviewer-risk-audit` 或 `submission-integrity-audit`

### Step 2: 保留技术不变量

重写前明确哪些内容不能动：

- 结论强度
- 假设前提
- 实验设置
- 数值与比较方向
- 定理与 proof sketch 口径
- 引用事实与文献存在性状态

### Step 3: 做引用与合规前置判断

在以下情况中，不应直接继续润色，而应先转交相关 Skill：

- 若文本新增或修改了 references、bib、年份、venue、accepted status -> `citation-reality-guard`
- 若用户要“把全文改得不像 AI 写的”，但稿件明显暴露出未披露生成残留 -> 先提示 disclosure / integrity 风险
- 若投稿临近，且图表、checklist、声明材料也可能存在 AI 使用合规问题 -> `submission-integrity-audit`

### Step 4: 分 section 改写

按 section 选择不同策略：

- `Abstract`：减少模板句和空泛 pitch，首句直接进入问题与结果
- `Introduction`：减少口号和宽泛背景，强调具体局限与差异
- `Related Work`：避免流水账式“X did..., Y did...”，改为按问题轴组织
- `Method/Theory`：只做语言节奏调整，不动技术定义
- `Discussion/Conclusion`：避免三段式套话，用具体边界、开放问题或 take-away 收尾

### Step 5: 节奏与句式去同质化

优先做这些操作：

- 长句和短句交替
- 更换句首类型，而不是反复用 `We`
- 让一部分段落直接从事实、限制或观察切入
- 保留少量领域内自然表达，不追求“每句都完美对称”

### Step 6: 回查技术、残留与合规边界

至少复查：

- 是否误改了结论强度
- 是否动了引用或数学含义
- 是否引入新的语言歧义
- 是否只是换同义词而没有真正打破模板节奏
- 是否仍残留提示语、system prompt 或编辑元话语
- 是否存在应当转交 `citation-reality-guard` 但尚未处理的引用风险
- 是否把合规风险误处理成了“单纯去 AI 味”

## 默认输出格式

- `AI 痕迹诊断`
- `风险分级`
- `不可改动项`
- `需要先转交的风险`
- `改写后文本`
- `关键改动说明`
- `残余风险`

## 常见高风险信号

- Abstract 第一行写成 `In this paper, we propose...`
- Conclusion 落入 “In summary ... Future work ...” 固定模板
- Related Work 每句一个作者名，缺少比较结构
- 全文 “We show / We demonstrate / We highlight” 连续出现
- 正文中出现 `As an AI language model`、`Certainly, here is...`、`Below is a revised version`
- 正文中夹杂“请保留技术含义”“不要改引用”这类编辑指令
- references 看似齐全，但个别条目年份、venue、接收状态或主题对不上正文 claim

## 提示语残留检查清单

至少扫描以下模式：

- `As an AI language model`
- `I cannot`
- `Certainly`
- `Here is`
- `Below is`
- `revised version`
- `maintain technical meaning`
- `do not change citations`
- `improve clarity`
- `rewrite the paragraph`

若上述片段出现在论文正文、图注、附录说明、response draft 中，默认至少为 `P0`。

## 幻觉引用与错引处理规则

- 本 Skill 不负责最终裁定文献真实性，但必须识别引用异常信号。
- 若发现疑似假引用、错引、虚构元数据、引用描述与正文 claim 不匹配：
- 停止继续自然化相关段落
- 明确标记为 `P0`
- 转交 `citation-reality-guard`

在 `Related Work`、`Introduction`、`Conclusion` 中，这条规则优先级高于风格改写。

## 与其他 Skills 的关系

- 内容主线来自 `paper-positioning`
- 风险优先级来自 `reviewer-risk-audit`
- 若只是做一轮局部修文，可与 `paper-ratchet-optimizer` 串联
- 若涉及引用真实性，必须转交 `citation-reality-guard`
- 若涉及投稿一致性、声明或材料合规，可转交 `submission-integrity-audit`

## 标准输入

- 待处理段落、section 或整篇草稿
- 目标 venue 或期刊类型
- 用户是否允许只做风格改写、不改内容
- 用户最担心的 AI 痕迹类型
- 是否已完成引用真实性核验
- 是否已确认目标 venue 对 AI disclosure 的要求

## 标准交付

- 风格风险诊断
- 风险分级：`P0 / P1 / P2`
- 是否需要先做引用核验或 disclosure 风险处理
- 改写后的文本
- 保持不变的技术口径说明
- 仍建议人工复核的句子

## Examples

- “把这个 abstract 去 AI 味，但不要改技术含义。”
- “related work 读起来太像 LLM 写的，帮我改自然一点。”
- “帮我做一轮全文语言自然化，不要碰定理和引用。”
- “帮我检查有没有 ChatGPT 提示语残留，再决定是否润色。”
- “先判断这段是不是单纯 AI 味，还是已经有幻觉引用和提示语残留风险。”
