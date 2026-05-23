---
name: "latex-submission-packager"
description: "检查并封装 LaTeX 投稿包的格式、匿名性、顺序与可复现声明。Invoke when 用户接近投稿、要编译终稿、或做 submission package 终审。"
---

# LaTeX Submission Packager

这个 Skill 负责论文的最后一公里：从“内容差不多了”变成“投稿包不踩雷”。它关注的不是研究内容本身，而是 submission package 是否规范、匿名、完整、低风险。

## 何时调用

- 用户说“准备投稿了”
- 用户要生成 final pdf
- 用户要检查 appendix / references / checklist 顺序
- 用户要检查匿名性、补充材料、代码声明、camera-ready 差异
- 用户要做格式终审而不是内容重写

## 核心目标

1. 生成符合目标 venue 规则的单个 PDF 或最终提交包
2. 检查 references / appendix / checklist / supplementary 的顺序
3. 检查匿名性、文件命名、链接、仓库、补充材料声明
4. 检查编译日志、交叉引用、页数、版面溢出
5. 确保“文中说有的材料”真的在提交包里

## Step 1: 明确投稿目标

先收集：

- target venue 与年份
- submission 还是 camera-ready
- 单 PDF 还是 PDF + ZIP
- 是否双盲
- 是否要求 checklist / ethics / broader impact / reproducibility statement

## Step 2: 检查文档顺序

默认检查：

1. main paper
2. references
3. appendix / technical appendices
4. checklist

并确认：

- references 是否不计入主文页数
- appendix 是否允许并入同一 PDF
- checklist 是否在末尾
- acknowledgements 在双盲阶段是否被移除或隐藏

## Step 3: 匿名性审计

检查：

- 作者名、单位、邮箱是否泄露
- 自引是否暴露身份
- 代码仓库链接是否实名
- PDF metadata 是否泄露作者信息
- 附录、图内、脚本生成文本中是否残留姓名/路径/机构

## Step 4: 版面与交叉引用检查

检查：

- `undefined references`
- `citation warnings`
- `overfull hbox`
- 表格/图像/legend 是否超边界
- 页数是否满足 venue 要求
- figure/table/theorem/section 编号是否稳定

### 正文页数精确检查（NeurIPS 等页数限制 venue）

正文 = 从第 1 页到 References 出现之前的最后一页。References / Appendix / Checklist 不计入正文。

推荐方法：在主 `.tex` 文件中 `\bibliography` 之前插入：

```latex
\typeout{MAIN_CONTENT_LAST_PAGE=\number\numexpr\value{page}-1\relax}
```

编译后从 `.log` 文件中读取 `MAIN_CONTENT_LAST_PAGE=` 的值即正文页数。

禁忌：
- 不能靠肉眼数 PDF 页数（浮动体可能溢出到 references 之后）
- 不能删除任何正文内容——唯一合法操作是将内容原封不动移入 Appendix
- 移动正文后必须在原位置添加 `Appendix~\ref{...}` 指向句

### 引用格式检查

- 确认使用 `\bibliographystyle{unsrt}`（按首次出现顺序编号），不使用 `plain`（字母序导致正文中引用号跳跃）
- 编译后核对 `.bbl` 文件中 `\bibitem` 顺序是否与正文首次出现顺序一致
- 检查同一 `\cite{}` 内多篇引用是否保持升序
- 检查 `.bib` 中是否存在未被正文引用的死条目——必须在投稿前删除

## Step 5: 可复现与 supplementary 一致性

核查：

- 文中声称提供代码/脚本/匿名仓库时，是否真的存在
- supplementary 中的文件是否覆盖正文承诺
- checklist 中的回答是否和正文一致
- 如果只会在 submission system 上传 ZIP，就不要在正文假装公开仓库已上线

## Step 6: 输出最终投稿检查单

输出：

- `结构顺序`
- `匿名性风险`
- `编译与排版风险`
- `supplementary / checklist 一致性`
- `必须修复项`
- `可以提交 / 暂不建议提交`

## 常见 P0 硬伤

- references 和 appendix 顺序错误
- checklist 遗漏
- 匿名链接泄露身份
- 正文说"代码已提供"，但投稿包没有
- 编译成功但有未定义引用
- figure / table 超出页边界
- **正文页数超出 venue 限制（如 NeurIPS 正文 >9 页）**
- **使用 `plain` 导致引用序号在正文中跳跃不连续**
- **`.bib` 中存在未被引用的死条目**

## 默认编译策略

- 只有当用户明确要求编译时才全量编译
- 编译顺序一般为 `pdflatex -> bibtex -> pdflatex -> pdflatex`
- 每次终稿编译后都检查日志而不是只看退出码

## 与其他 Skills 的关系

- 内容问题交给 `reviewer-risk-audit`
- 引用真实性交给 `citation-reality-guard`
- 总流程由 `academic-paper-factory` 管理

## 标准输入

- 目标 venue、年份、submission 或 camera-ready 状态
- 主 `.tex` 文件与相关 `.bib`
- 是否双盲、是否有 checklist / ethics / supplementary
- 是否需要实际编译或仅做静态检查

## 标准交付

- 投稿包结构检查结果
- 匿名性风险列表
- 编译与排版风险列表
- checklist / supplementary 一致性结论
- `可以提交 / 暂不建议提交`

## Examples

- “帮我做一轮 NeurIPS 投稿包终审，重点看顺序、匿名性和 checklist。”
- “检查这个 LaTeX 项目能不能直接投稿，不要只看编译是否成功。”
- “准备 camera-ready 了，帮我核对 submission 和 final 版本差异。”
