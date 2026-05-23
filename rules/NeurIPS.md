# NeurIPS 2026 Venue-Specific 规则

本文件是 **NeurIPS 2026 venue-specific 叠加规则**，默认建立在 `.trae/rules/academic_base.md` 之上；这里只保留 NeurIPS 特有或高度相关的约束。

这个项目的投稿目标是 NeurIPS 2026 regular full paper。文章写作风格、主题、篇幅和最终排版都应符合 NeurIPS 2026 官方要求，可参考 NeurIPS 2025 已发表论文的写法与信息密度。

官方模板与说明：

- NeurIPS 2026 formatting instructions: https://media.neurips.cc/Conferences/NeurIPS2026/Formatting_Instructions_For_NeurIPS_2026.zip
- Conference site: https://neurips.cc/

## 1. 章节结构规范

- Introduction 中不允许出现独立的 Notation / Definitions 子节
- 所有形式化定义、假设、符号定义应放在 Introduction 之后的独立 Preliminary / Problem Setup 节中
- 标准 ME（方法）论文章节顺序优先采用：
  - Introduction
  - Related Work
  - Problem Setup / Notation
  - Method / Theory
  - Experiments
  - Discussion
  - Conclusion
- Related Work 放在 Method 之前是 NeurIPS 主流习惯，除非 Introduction 已给足背景且有充分理由后置

## 2. 实验内容要求

- 正文 Experiments 节必须包含足够的主证据，不允许把所有关键实验推到 Appendix
- 任何直接支撑论文核心论点的实验，都必须出现在正文
- scaling、不同维度、不同平滑度、关键 baseline 对比等，如果直接支撑主张，必须在正文保留
- Appendix 只保留 routing diagnostics、补充消融、额外可视化、详细设置和辅助验证性实验

## 3. 参考文献要求

- 正文引用数下限按 20 篇把握
- 需覆盖：经典奠基工作、近两年关键工作、实验 baselines 的原始出处
- Related Work 中每段末尾最好明确说明本文与所引工作的关键区别
- 所有引用必须真实存在，不得有幻觉引用
- 未被正文或 Appendix 实际引用的 bib 条目应在投稿前移除

## 4. 语言风格

- 严禁直接使用未经改写的一整段 LLM 输出作为稿件正文
- 全文 `Furthermore / Moreover / Additionally` 总出现次数不超过 2 次
- Abstract 不能以 `In this paper, we propose...` 开头
- Conclusion 不要落入 `In summary, we have shown that...` 的模板化三段式
- 句子开头 `We ...` 的比例不能过高，需要混合被动语态、时间状语、名词短语等不同起点
- 段落长度和结构要有自然波动，避免每段都写成“主题句 -> 证据 -> 总结句”

## 5. 篇幅与版式

- NeurIPS 2026 正文页数上限为 **9 页**
- 该上限包含图表，不包含 Acknowledgments / References / Appendix / Checklist
- 缩减正文的唯一合法操作：将内容原封不动移入 Appendix，**禁止删除任何内容**
- 正文每处移出的内容，都要在原位置添加指向 `Appendix~\ref{...}` 的引用句
- 重新编译后，通过 `\typeout{MAIN_CONTENT_LAST_PAGE=...}` 宏确认正文精确页数

## 6. 引用格式

- 使用 `\bibliographystyle{unsrt}`，按正文首次出现顺序编号
- 不使用 `plain`，避免字母序导致正文中编号跳跃
- 引用号在正文中应按阅读顺序自然递增
- 同一 `\cite{}` 内多篇引用也应保持升序
- `.bib` 中所有条目必须被正文或 Appendix 实际引用

## 7. 图表与排版

- 图表内部内容必须在正文 PDF 中仍然清晰可读，不能只在单独图文件里看起来正常
- legend、tick label、标题、标注不得遮挡主要结构，不得被裁切或越界
- 任何字体、线宽、布局、长宽比调整后，都应同时检查导出的图 PDF 和编译后的最终论文 PDF
- 不得通过 LaTeX `trim/clip` 掩盖图本身的布局问题，优先在出图脚本中修复

## 8. 投稿前检查清单

- [ ] 章节顺序符合 NeurIPS 主流习惯
- [ ] 正文页数不超过 9 页
- [ ] 关键实验保留在正文
- [ ] 所有引用真实存在且 `.bib` 无死条目
- [ ] `\bibliographystyle{unsrt}` 设置正确
- [ ] 图表在最终 `paper.pdf` 中无裁切、遮挡、越界
- [ ] Appendix、References、Checklist 顺序正确
