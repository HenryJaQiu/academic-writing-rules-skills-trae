---
name: "grounded-paper-qa"
description: "基于用户提供的论文、摘录和笔记做带出处问答与事实核查。Invoke when 用户要从指定文献回答问题、核某个说法、或先问清文献再写作。"
---

# Grounded Paper QA

这个 Skill 负责把“我大概记得这些论文提过”变成“基于指定材料、可回溯到出处的回答”。它不追求泛泛而谈的综述，而是强调回答必须被用户提供的论文、摘录、批注或读书笔记支撑。

它吸收了高热科研助手里最有价值的一类能力: grounded paper QA, citation-backed answers, fact-check mode。但这里保持工具无关，不绑定 Zotero、Obsidian 或某个单独平台。

## 何时调用

- 用户说“这几篇论文到底怎么比较”
- 用户说“只根据我给你的论文回答，不要泛化”
- 用户要核查某个 claim 是否真被现有文献支持
- 用户要在写 introduction / related work / rebuttal 前先问清楚文献细节
- 用户有一批 PDF 摘录、读书笔记、文献摘要，想做带出处问答
- 用户怀疑自己记错了某篇论文的方法、实验设置、结论或局限

## 核心目标

1. 只基于给定材料作答，不把外部常识混进来
2. 每个关键判断都指出出处位置或至少指出对应论文/摘录来源
3. 明确区分“材料支持”“材料不足”“材料反驳”“当前未知”
4. 把问答结果转成可继续写作的证据资产

## 顶会写作适配

这个 Skill 默认优先服务 4 类高频写作动作：

- `Introduction support`：先核清现有工作到底做到哪一步，再决定 novelty 口径
- `Related work support`：先问清哪些工作真可比较、哪些只是表面相关
- `Claim support`：先核清某句强表述有没有被文献支持
- `Rebuttal support`：先回答 reviewer 质疑点，再组织回应文本

## 不该做什么

- 不把未提供的论文当成已知事实
- 不把一句模糊摘要扩写成强结论
- 不把“论文提到过”改写成“论文严格证明了”
- 不在证据不足时装作已经回答清楚
- 不把 citation key、论文标题或摘要片段直接当作 claim 已被证明
- 不把 PDF 自动解析结果中的表格、公式、页码、脚注错位内容直接当成最终事实

## 执行步骤

### Step 0: 检查 PDF / 文档解析质量

若输入来自 PDF、OCR、扫描件、文档转 Markdown 或自动摘录，先标记材料状态：

- `Native text PDF`
- `OCR text`
- `Scanned pages / screenshots`
- `Secondary notes / manual excerpts`

并额外检查：

- 是否保留页码、section、figure/table 编号等锚点
- 是否可能存在双栏串行、换行断词、脚注混入正文的问题
- 表格、公式、算法伪代码、caption 是否在解析后明显变形

若命中以下情况，默认把证据状态先降一级，直到回原 PDF 或截图复核：

- 关键结论只来自 OCR 文本
- 关键数字只来自扁平化表格
- 关键理论判断依赖被破坏的公式或符号
- 引用位置、页码或 figure/table 对应关系不清

### Step 1: 锁定材料边界

先明确本轮允许使用的证据范围：

- 用户提供的 PDF、摘录、章节、批注、Markdown 笔记
- 用户列出的论文题目、摘要、bib 条目
- 已经整理好的 reading notes、表格、对比清单

若用户要求“只基于这些材料”，不得自行引入额外文献。

若材料来自 PDF 解析结果，还要显式写明：

- 当前用的是原 PDF、解析文本，还是二手摘录
- 哪些答案可直接基于解析文本给出
- 哪些答案必须回原 PDF / 图表 / 公式截图核验

### Step 2: 把问题改写成可核验问题

优先把问题改成以下类型之一：

- 定义型：某篇工作到底解决什么问题
- 对比型：A 和 B 在方法、设定、证据上有什么区别
- 支持型：某个 claim 是否被现有材料支持
- 归因型：某个结果来自哪个机制、设置或假设
- 边界型：哪些内容论文没说、不能说或只在特定条件下成立

### Step 3: 做证据抽取

对每个问题，抽出最相关的证据块：

- 论文名或来源名
- 对应 section / table / figure / theorem / appendix
- 关键原句、摘要句或你的读书笔记
- 证据类型：definition / method / experiment / limitation / discussion

如果只有二手笔记，没有原文定位，也要明确标注“来自笔记，不是原文直接核对”。
如果证据来自 PDF 自动解析文本，也要标注“来自解析文本，表格/公式/版面信息可能有损”。

### Step 4: 先给证据判定，再给回答

至少区分 4 类状态：

- `Supported`：材料足以支持回答
- `Partially Supported`：有部分证据，但还不能说满
- `Contradicted`：给定材料与该说法相冲突
- `Unknown from Provided Corpus`：当前材料无法回答

回答必须先服从证据状态，再组织文字。

### Step 5: 生成写作可用输出

若用户后续要写论文，不只给口头回答，还应产出：

- 支持该回答的论文列表
- 不宜直接写进论文的高风险表述
- 可安全使用的保守表达
- 需要补查的缺口

### Step 6: 按用途打包结果

根据后续写作动作，至少补一份对应资产：

- 若用于 `Introduction`：输出“当前工作边界 + 可安全宣称的差异句”
- 若用于 `Related Work`：输出“可比较论文集合 + 不可直接并列的论文集合”
- 若用于 `Claim check`：输出“支持强度 + 建议降级口径”
- 若用于 `Rebuttal`：输出“reviewer concern -> 文献证据 -> 可回应点 -> 仍需承认的限制”

## 默认保守策略

- 若证据只来自摘要、二手笔记或不完整截图，默认不支持强结论
- 若材料之间互相冲突，优先保留冲突而不是强行求平均
- 若只能确认“论文提到过”，默认不升级为“论文证明了/解决了”
- 若多个工作问题设定不同，默认先拆赛道，再比较
- 若关键证据来自 OCR、扫描版转写或疑似错位的 PDF 解析结果，默认不支持精确数字、公式级结论或强比较

## 推荐输出模板

### 模板 A: 问答模式

- `问题`
- `简短答案`
- `证据状态`
- `关键出处`
- `不能说太满的地方`

### 模板 B: 写作支撑模式

- `用户要写的句子或段落目标`
- `可直接支撑的文献`
- `只能弱支撑的文献`
- `不适合拿来支撑的文献`
- `建议写法`
- `需要补查的问题`

### 模板 C: Rebuttal 模式

- `Reviewer concern`
- `What the provided papers support`
- `What they do not support`
- `Safe response angle`
- `Residual limitation`

## 默认输出格式

- `问题重述`
- `证据边界`
- `结论`
- `证据表`
- `不确定点 / 不能下结论的地方`
- `可直接复用的保守写法`
- `下一步建议`

## 标准输入

- 用户问题
- 论文全文、节选、摘要、读书笔记或对比表
- 是否限制为“只基于给定材料”
- 是否需要输出成 related work / rebuttal / claim-support 资产
- 若输入来自 PDF / OCR / 自动解析，当前解析来源与质量状态

## 标准交付

- 带出处的简洁回答
- 证据状态判定: `Supported / Partially Supported / Contradicted / Unknown`
- 证据表
- 高风险误读点
- 可直接交给下游 Skill 的结构化结论
- 若需要，附 `Introduction / Related Work / Claim check / Rebuttal` 四类写作资产之一
- 若涉及 PDF 解析输入，附 `Parsing Risks / Must Verify Against Original PDF`

## Examples

- “只根据这 8 篇论文，回答 sparse MoE 的理论结果到底到了哪一步。”
- “帮我核查这个说法: recent work already proves convergence for practical top-k routing.”
- “不要泛泛综述，先根据我给你的摘录回答这几个问题。”
- “把这些 reading notes 变成一份带出处的 QA，后面我要写 introduction。”

## 与其他 Skills 的关系

- 若用户要横向比较多篇论文，常与 `reading-note-synthesizer` 搭配
- 若目标是系统搭建文献地图，后续接 `literature-survey-builder`
- 若目标是把问答结果转成差异表和定位语句，后续接 `paper-positioning`
- 若目标是核查文中 claims 是否真的被文献支持，后续接 `claim-evidence-mapper`
- 若涉及引用真实性、年份、venue、元数据是否真实，转交 `citation-reality-guard`
- 若用户请求模糊，先经 `paper-intake-router` 分流
