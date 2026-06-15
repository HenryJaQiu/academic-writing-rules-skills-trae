---
name: "citation-reality-guard"
description: "核验引用真实存在、近年覆盖充分且元数据准确。Invoke when 用户添加引用、写 related work、或做投稿前事实核查。"
---

# Citation Reality Guard

这个 Skill 专门防止“幻觉引用”“引用错配”“年份作者 venue 错误”“related work 过时”。

它吸收了 `CiteCheck` 一类仓库里最有价值的做法，但保留为工具无关的通用 Skill：

- 结构化来源梯度，而不是只信单一数据源
- 显式拒绝低相似度误匹配，而不是“搜到一个像的就算”
- 把存在性、元数据、queryability、语义一致性分开报告

## 何时调用

- 用户要新增或修改 `.bib`
- 用户在写 abstract / intro / related work / conclusion
- 用户要求“确认所有引用都真实存在”
- 用户准备投稿前最终核查
- 用户担心近两年工作覆盖不足

## 核心目标

1. 所有引用必须是真实存在的学术文献
2. 正文对引用的描述必须与原文结论一致
3. `.bib` 的作者、年份、venue、arXiv / OpenReview / DOI 信息尽量准确
4. Related Work 需要覆盖近 2–5 年代表性工作
5. 高风险条目应尽量能在 2 个以上可信来源中被交叉确认

## 强制规则

- 不得杜撰引用
- 不得用不存在的会议接收状态
- 不得把 arXiv 误写成已发表 venue
- 不得把 workshop / preprint 写成 full conference paper
- 不得写不完整作者列表而暗示是完整元数据
- 不得把别人没有证明的结果归因为别人已经证明

## 执行流程

### Step 1: 建立核验清单

优先核验：

- 正文和附录中实际被 `\cite{}` 使用的条目
- 摘要、引言、related work、结论中出现的核心引用
- 最近两年（当前年份往前推 2–3 年）的条目
- arXiv / OpenReview / workshop / oral / poster 等高风险元数据
- 从 PDF、LaTeX 或正文段落中临时提到、但还未进入 `.bib` 的候选引用

### Step 2: 先做格式与可查询性检查

不要一上来就下真假结论，先看条目是否“可查”：

- 标题是否完整到足以检索
- 作者名是否至少保留主要作者信息
- 年份是否像真实发表年份，而不是把 arXiv 编号片段误当成年份
- DOI / arXiv / venue 字段是否结构正常
- citekey 是否和条目内容明显脱节

若条目本身残缺到无法可靠查询，应先标为 `P1` 或 `P0`，而不是硬匹配。

若条目来自 PDF 自动解析、OCR 或正文抓取，还要额外检查：

- 标题是否被断行、断词或多栏拼接污染
- 作者列表是否混入 affiliation、页眉页脚或脚注符号
- DOI / arXiv 号是否因 OCR 丢字符、错字符或多余空格而失真
- 表格/参考文献区是否把相邻两条引用错误合并

### Step 3: 三层核验

每条引用尽量走三层：

1. 存在性：论文是否真实存在
2. 元数据：作者、标题、年份、venue、DOI、arXiv 编号是否一致
3. 语义一致性：正文描述是否准确反映论文贡献

优先来源：

- 期刊官网
- 会议官网 / OpenReview / PMLR / proceedings
- Crossref
- Semantic Scholar
- OpenAlex
- arXiv
- DBLP / Google Scholar / WebSearch 作为辅助

### Step 4: 拒绝低置信误匹配

不要把“搜到一个很像的结果”当成核验成功。至少检查：

- 标题相似度是否足够高
- 作者是否有明显重合
- 年份是否接近且合理
- venue 类型是否一致，例如 preprint / workshop / main conference / journal

若出现以下情况，默认拒绝匹配，不直接写成“已核验”：

- 标题只有部分词重合，但核心名词不一致
- 作者几乎不重合
- 年份偏差明显且无法解释为 preprint -> camera-ready -> journal extension
- 把 arXiv 版本误认成正式发表版本
- 只在低可信来源找到，但主流来源完全缺失

### Step 5: 区分 queryability 与真实性

把以下状态分开：

- `Verified`：存在性和元数据都比较稳
- `Ambiguous`：疑似存在，但匹配不够稳或来源冲突
- `Unqueryable`：条目太残缺，暂时无法可靠查询
- `Rejected`：高度怀疑是假引用或错配

queryability 差不等于一定是假的，但一定是投稿风险。

### Step 6: 标记风险等级

- `P0`：疑似假文献、完全找不到、低置信误匹配被拒绝、正文描述与文献相反
- `P1`：年份/venue/作者/接收状态错误，或条目可疑但仍需人工定夺
- `P2`：近年工作覆盖不足、引用定位不够精确、可查询性差但不一定错误

### Step 7: 修正正文口径

对每个高风险引用，检查正文是否有以下问题：

- 把“初步研究”说成“完整理论”
- 把“参数估计”说成“通用函数逼近”
- 把“construction-based result”说成“general minimax theory”
- 把“经验改进”说成“理论保证”

### Step 8: 最终交付

输出：

- 已核验真实的关键条目
- 需要修正的 `.bib` 条目
- 需要降级的正文表述
- 缺失的近年相关工作建议

## 默认输出格式

- `确认真实的引用`
- `可查询性 / 来源交叉确认`
- `需要修正的元数据`
- `正文描述风险`
- `近年工作覆盖缺口`
- `建议改动`

## 高风险信号

- 条目 key 像真实论文，但年份与 arXiv 编号不匹配
- 作者字段只有第一作者，但像是正式论文条目
- `journal={arXiv preprint}` 与 `note={Accepted to ...}` 同时存在，需要核实最终状态
- 正文宣称“证明了/首次/统一/最优”，但被引文献其实只做了 preliminary study
- Crossref 能搜到相似标题，但作者、年份或 venue 明显对不上
- 只在一个弱来源里能找到，而主流来源都没有
- PDF 抓出的标题含明显断词、连字符、页码残片或两条参考文献串接
- OCR 后 DOI / arXiv 编号只差 1-2 个字符，看似可查但高风险错配

## 面向 AI 投稿的额外要求

- 理论论文：必须覆盖最近两年强相关理论工作，并说明目标/设定差异
- 方法论文：必须覆盖最新最强 baseline 与同类替代路线
- 系统/Agent 论文：必须区分 blog、技术报告、预印本、正式会议文献

## 与其他 Skills 的关系

- 与 `paper-positioning` 配合，确保 positioning 不是建立在错引之上
- 与 `reviewer-risk-audit` 配合，降低 reviewer 一眼识破的硬伤
- 与 `paper-ratchet-optimizer` 配合，在每轮修改后重新核验高风险引用

## 标准输入

- `.bib` 文件或引用条目列表
- `.tex`、PDF、正文段落或从稿件中抽出的引用清单
- 正文中高风险 section：abstract / intro / related work / conclusion
- 目标年份窗口，默认近 2–5 年
- 用户特别担心的条目：arXiv / OpenReview / workshop / accepted status

## 标准交付

- 已确认真实的关键引用
- 每条核心引用的 queryability / source status
- 需要修正的元数据条目
- 正文描述不准确的地方
- 缺失的近年代表工作
- 推荐修改优先级：P0 / P1 / P2

## Examples

- “帮我确认 references.bib 里所有引用都真实存在，不要有幻觉引用。”
- “related work 里 2024–2026 的文献够不够？顺便核一下 arXiv 条目。”
- “把这篇稿子的引用做一轮投稿前事实核查，并指出正文描述是否夸大。”
