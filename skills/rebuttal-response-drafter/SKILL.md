---
name: "rebuttal-response-drafter"
description: "拆解审稿意见、制定回应策略并起草带 reviewer-specific replies 与 inline manuscript revisions 的 rebuttal。Invoke when 用户收到 reviews、要写 rebuttal、或要在字符限制下压缩 response。"
---

# Rebuttal Response Drafter

这个 Skill 负责把“收到审稿意见后的混乱状态”变成“逐条、克制、可信、可执行的 rebuttal 与修稿计划”。它既关注语言风格，也关注回应是否真正解决 reviewer 的核心担忧。

## 何时调用

- 用户收到 conference / journal reviews
- 用户要写 rebuttal、author response、revision memo
- 用户想先判断哪些 review 必须正面回应、哪些可以澄清
- 用户需要把“计划修改内容”与“书面回复内容”统一起来
- 用户需要在严格字符限制下压缩 rebuttal，但不想丢掉 reviewer-specific 结构

## 核心目标

1. 逐条拆解 reviewer concern，而不是按情绪回应
2. 区分可立即澄清、可承诺修改、无法在 rebuttal 期完成的问题
3. 保持尊重、克制、具体、可验证的回应风格
4. 让 rebuttal 与后续修稿动作一致，避免空头承诺
5. 保持 response 时态稳定，优先用现在完成时描述已完成澄清、已补充证据和已修正表述
6. 保持用词自然精确，避免华丽化或 AI 式不自然替换
7. 默认保留 `Overall Response + Reviewer-specific Responses` 结构，而不是只写统一总回复
8. 让每个 concern 都尽量对应一个真实会发生的 `Revision in manuscript`

## 工作模式

### `full`

默认模式，适合字符限制较宽松或 journal revision：

- 保留 `Overall Response`
- 为每位 reviewer 单独建节
- 对关键 weakness / questions / concerns 逐点回应
- 每条尽量带 `Revision in manuscript`

### `tight`

适合有明显字数压力，但仍希望保留 reviewer-specific 尊重：

- 保留 `Overall Response + Reviewer 1-N`
- 每位 reviewer 只保留最关键的 `2-5` 个 concern
- 每条保留 `Concern / Response / Revision`
- 压短礼貌语与重复限定语

### `submission-compact`

适合 rebuttal 字符限制极严的会议系统：

- 保留 `Overall Response + Reviewer 1-N`
- 每位 reviewer 只保留最关键的 `2-3` 个 concern
- 合并次级 concern 到相邻条目
- 删单独长 `Planned Revisions`
- `Revision in manuscript` 压成 1 句，但不断掉与 concern 的对应关系

## 执行步骤

### Step 0: 先补齐不确定信息

若以下任何一项不明确，默认不要直接起草完整 rebuttal，而应先向用户确认：

- rebuttal / response 的字数或字符上限
- 是否系统上要求统一提交，但用户仍希望保留 reviewer-specific 结构
- 一共有几位 reviewer，以及每位 reviewer 最关键的 `2-5` 个 concern
- 哪些是本轮绝对不能回避的 hardest points
- 是否允许承诺新实验、新 ablation、多 seed 重跑或新理论证明
- 哪些修改已经完成，哪些只是计划在 revision 中完成
- 是否必须保留 `Overall Response`
- 是否需要把 `Revision in manuscript` inline 写回每条 response

默认询问优先级：

1. 字数/字符上限
2. 是否允许承诺新实验
3. hardest points
4. reviewer-specific 结构要求
5. 已完成 vs 计划完成的边界

若用户没有明确回答：

- 默认保守处理为 `不承诺高风险新实验`
- 默认保留 `Overall Response + Reviewer-specific Responses`
- 默认只把已完成的澄清/补充写成现在完成时
- 对 revision 动作一律写成保守、可落地、不会误导 reviewer 的 manuscript-level 改动

### Step 1: 解析 review

把每条意见拆成：

- concern 类型：novelty / evidence / experiment / theory / writing / formatting
- 严重程度：P0 / P1 / P2
- reviewer 真正在问什么
- 这条意见是否基于误解

同时先做 reviewer 维度整理：

- 哪些 concern 是共性问题，适合放进 `Overall Response`
- 哪些 concern 必须保留在 reviewer-specific 部分逐点回答
- 哪些属于 hardest points，不能被一句“we will clarify”带过

默认优先确保 hardest points 被单独接住，而不是被合并稀释。

### Step 2: 确定回应策略

每条意见只选一个主策略：

- `Clarify`: 论文里已有内容，但 reviewer 没看清
- `Add Evidence`: 需要补实验、补对比、补说明
- `Narrow Claim`: 当前 claim 过强，需要降级
- `Acknowledge Limitation`: 不能在 rebuttal 期解决，但要诚实承认

并补一个 manuscript-level 动作：

- 要改哪一节
- 改什么口径
- 这个修改将直接解决哪个 concern

禁止只写泛泛动作，如：

- `We will improve clarity.`
- `We will strengthen the paper.`
- `We will further revise the manuscript.`

### Step 3: 生成回应草稿

默认输出骨架：

1. `Overall Response`
2. `Response to Reviewer 1`
3. `Response to Reviewer 2`
4. `Response to Reviewer 3`
5. `Response to Reviewer 4`（如有）
6. 简短自然结尾

其中 `Overall Response` 只用来接共性问题，不代替 reviewer-specific 回复。

每位 reviewer 下，按编号逐点回应关键 concern。默认结构：

`### <编号>. <short concern title>`

- `Concern:`
- `Response:`
- `Revision in manuscript:`

每条回应优先内容：

1. 感谢与对问题的准确复述
2. 直接回答 reviewer 关切
3. 指向现有证据或新增证据
4. 如有必要，给出将补入正文的具体位置和内容

时态与措辞规则：

- 优先用现在完成时描述已经完成的解释、补充和修正，如 “We have clarified...”, “We have added...”, “We have revised...”
- 用一般现在时陈述当前稿件中现在成立的事实与结构，如 “Section 4 now reports...”
- 少用会被读成空头承诺的将来式，除非确实是在说明 revision 阶段的后续动作
- 避免不自然的高级词、法律/行政腔和显得像机器替换的近义词
- 避免每条都用相同开头，如反复 `We agree.`、`This is a fair concern.`、`We will revise...`
- `Revision in manuscript` 要像真实修稿动作，而不是项目管理任务单

### Step 4: 检查 inline revision 是否“像真会发生”

逐条检查 `Revision in manuscript`：

- 是否具体到 section / paragraph / contribution bullets / figure / limitation
- 是否说明会收紧什么 claim、补什么说明、改什么 framing
- 是否让 reviewer 一眼看出“我的问题会导致什么具体改稿”

优先使用这类动作：

- 重写 abstract 的问题-边界-证据结构
- 改写 Introduction 结尾与 contribution bullets
- 新增 protocol-focused paragraph / comparison summary / limitation statement
- 补一张 compact figure 或一个紧凑比较表

谨慎承诺这类高风险动作，除非用户已明确确认：

- 新大实验
- 大规模 ablation
- 多 seed 重跑
- 新理论证明

### Step 5: 按字符限制压缩

若用户给出字数或字符上限，主动切换压缩模式，而不是最后整体重写一遍。

压缩优先顺序：

1. 删重复感谢
2. 删重复限定语
3. 压短 reviewer-specific 开头
4. 把 `Response` 压成 2-3 句
5. 把 `Revision in manuscript` 压成 1 句
6. 合并次级 concern

极限情况下，也应尽量保留：

- `Overall Response`
- reviewer-specific sections
- 每位 reviewer 最关键的 `2-3` 个 concern

### Step 6: rebuttal 专用去 AI 味审计

重点检查：

- 是否每位 reviewer 都用了几乎相同的开头句式
- 是否高频重复 `We agree.`, `This is a fair concern.`, `We will revise...`
- 是否每条长度、节奏、结构都过度整齐
- 是否整篇像“高质量模板回复”而不是作者真的在对 reviewer 说话
- `Revision in manuscript` 是否过于行政化

若命中上述问题，做一轮局部去模板化 line edit，而不是整篇推倒重写。

### Step 7: 检查承诺风险

逐条检查：

- 有没有承诺用户实际上做不到的实验或证明
- 有没有用攻击性或防御性语气
- 有没有把未完成工作写成“已完成”
- 有没有跟论文正文后续修改计划冲突
- 有没有为了显得谦逊而过度自我降级，替 reviewer 主动下更低评价

## 默认输出格式

- `Review Summary`
- `Hardest Points`
- `Overall Response`
- `Reviewer-specific Responses`
- `Inline Revisions in Manuscript`
- `AI-Flavor Audit`
- `Risks to Avoid`

若字符限制很紧，可改为：

- `submission-compact rebuttal`
- `Concern compression map`
- `Inline Revisions in Manuscript`

## 低风险规则

- 不要直接说 reviewer 错了，改为“the current wording may have caused confusion”
- 不要承诺未经验证的新结果
- 不要在 rebuttal 中继续扩大主张
- 不要回避最关键的 novelty / evidence / fairness 问题
- 不要写空洞感谢，核心是快速、具体、可信
- 不要把“我们已经完成”的动作写得像未来计划
- 不要把普通学术词硬替换成不自然的华丽词，如无必要不要把 `judgment` 改成 `adjudication`
- 不要只有“统一总回复”而没有 reviewer-specific section，除非用户明确要求
- 不要把 manuscript revisions 全堆在最后，而不对应具体 concern
- 不要让 hardest points 被合并成一句泛泛 `we will clarify`
- 不要让 rebuttal 读起来像高度规整的模板回复
- 不要为了显得克制而过度自我降级

## 标准输入

- 全部 reviews / meta-review / AE comments
- 当前稿件状态
- rebuttal 字数限制或期刊回复格式
- 用户打算补做的实验或修改
- 当前最担心的 reviewer
- 是否要求统一提交，但仍希望保留 reviewer-specific 结构
- 当前最难回应的 `2-5` 个 hardest points
- 哪些修改已经完成，哪些只是计划完成
- 是否允许承诺新实验 / 新 ablation / 多 seed / 新理论证明
- 若信息缺失，先向用户询问而不是自行假设

## 标准交付

- 若存在关键信息缺口，先给 `Clarifications Needed`
- review 问题拆解
- 优先级排序
- 每条意见的主回应策略
- rebuttal 初稿：默认 `Overall + Reviewer 1-N + Inline Revision`
- 需要同步修改正文的动作列表
- 高风险措辞提醒
- `AI-Flavor Audit`
- 若有字数限制，附 `tight` 或 `submission-compact` 版本
- 若需要，附 `Tense Fixes / Lexical Precision Warnings`

## Examples

- “我收到 3 个 reviewer 的意见，帮我先拆成可以回应的点，再起草 rebuttal。”
- “不要替我硬杠 reviewer，帮我写得克制但有说服力。”
- “帮我把 rebuttal 和下一版修稿计划对齐，避免空头承诺。”
- “系统要求统一提交，但我还是想保留每位 reviewer 的单独回应。”
- “字符限制很紧，帮我压成 submission-compact 版本，但别丢 reviewer-specific 结构。”

## 与其他 Skills 的关系

- 若 review 质疑定位，回调 `paper-positioning`
- 若 review 质疑 related work / citation，回调 `literature-survey-builder` 与 `citation-reality-guard`
- 若 review 质疑实验，回调 `experiment-story-builder`
- 若 review 质疑整体风险，回调 `reviewer-risk-audit`
- 总流程由 `academic-paper-factory` 统筹

## 默认询问模板

在信息不完整时，优先向用户确认：

- `字符限制是多少？`
- `系统是否要求统一提交，但你仍希望保留 reviewer-specific 结构吗？`
- `最难的 2-5 个 concern 是哪些？`
- `哪些修改已经完成，哪些只是计划在 revision 中完成？`
- `是否允许承诺新实验、多 seed、ablation 或新理论证明？`
