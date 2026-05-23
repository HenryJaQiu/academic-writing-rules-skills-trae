# Pattern Recognition（PR）期刊投稿规则（可复用）

本文件定义对 Pattern Recognition 期刊投稿普适的 venue 约束，适用于未来投 PR 的所有稿件。项目/文章特有规则应写在 `.trae/rules/project_rules.md`。

官方作者指南（以最新版本为准）：
https://www.sciencedirect.com/journal/pattern-recognition/publish/guide-for-authors#writing-and-formatting-file-format

## 1. 审稿与匿名

- 审稿机制：single anonymized（作者信息保留；审稿人匿名）
- 不按双盲处理，不在正文中刻意去除作者/机构信息，但需避免不必要的自我宣传性措辞

## 2. 模板与版式（必须）

- 必须使用 Elsevier 官方 LaTeX 模板
- 本项目仓库内模板路径：`els-cas-templates/`
- 推荐类文件：`cas-sc`（单栏），并按 PR 要求保持单栏、双倍行距、页码
- 参考样式：
  - `\documentclass[a4paper,fleqn]{cas-sc}`
  - `\usepackage[numbers]{natbib}` + `\bibliographystyle{model1-num-names}`
  - 使用 `\doublespacing` 实现 double spaced

## 3. 篇幅与排版要求（必须）

- 单栏、double spaced、带页码
- 常规研究论文页数建议区间：20–35 页（review 可更长），按期刊指南执行

## 4. 必备投稿要素（必须/强建议）

- Title / Abstract / Keywords：关键词用 `\sep` 分隔
- Highlights：按指南准备（模板内支持 `highlights` 环境）
- Declaration of competing interests：必须提供
- Funding statement：无经费也建议显式写“no specific grant”
- Data availability statement：建议提供（若无法公开需说明原因与访问方式）
- Generative AI 使用声明：如在写作/润色中使用生成式 AI 或 AI 辅助工具，需在参考文献前加入声明；若无则不必添加

## 5. 参考文献与覆盖（必须）

- 所有引用必须真实存在，且正文描述与原文贡献一致
- 覆盖经典奠基工作 + 近两年关键工作，并明确与本文差异
- 与 PR 期刊相关工作对齐：
  - 在本方向存在 PR 发表的代表性工作时，应优先引用并在 Related Work 中形成明确对比
  - 禁止为“凑 PR 引用数”而引入与本文无关的 PR 文章

## 6. 图表与附录（必须）

- 图表可读性优先：字体、线宽、标注在最终 PDF 中清晰
- 不用 LaTeX 的 `trim/clip` 掩盖图本身排版问题，优先在出图脚本中修复
- Supplementary/附录若使用，应与正文口径一致，并避免新增未经验证的强结论

## 7. 投稿前检查清单

- [ ] 单栏、double spaced、页码满足 PR 要求
- [ ] Highlights 已准备并与正文贡献一致
- [ ] competing interests / funding / data statement / AI 声明（如适用）齐全
- [ ] 引用真实存在，且覆盖近两年关键工作（含 PR 相关工作对齐）
- [ ] 图表与 caption 可读且不越界
