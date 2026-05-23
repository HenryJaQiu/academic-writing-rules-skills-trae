---
name: "latex-first-compile-bootstrap"
description: "快速完成新项目首次 LaTeX 编译配置，处理引擎、模板、字体和 BibTeX 流程。Invoke when 用户新建论文项目、第一次编译很慢、或在 Windows 上遇到字体与模板问题。"
---

# LaTeX First Compile Bootstrap

这个 Skill 解决学术项目在第一次 LaTeX 编译时最常见、最耗时的一组问题：模板切换后无法编译、Windows 字体问题、`latexmk` 卡住、`acmart`/`IEEEtran` 与旧宏包冲突、BibTeX 没跑通、编译命令每个项目都要重新试。

目标不是“最终投稿终审”，而是让一个新的 LaTeX 项目在最短时间内达到：

1. 能稳定编译出 PDF
2. 知道该用哪个引擎
3. 知道该用哪条命令链
4. 知道哪些 warning 是首轮可接受的，哪些必须立刻修
5. 形成可复用的本地编译脚本

## 何时调用

- 用户刚导入一个新的论文 LaTeX 项目
- 用户说“先把这个项目编译起来”
- 第一次编译很慢，且反复卡在字体、模板、宏包或 BibTeX
- Windows + MiKTeX / TeX Live 环境下首次编译经常报错
- venue 模板切换后，原项目的 `article`/`IEEEtran`/`acmart` 不兼容

## 核心目标

1. 快速识别模板类型和推荐编译引擎
2. 用最短命令链完成 smoke test，而不是一开始就重度依赖 `latexmk`
3. 优先消除“阻止编译”的错误，再处理“美化日志”的 warning
4. 在当前项目中留下可复用的编译命令或脚本
5. 把字体和宏包问题沉淀成标准化判断规则

## 默认原则

- **先跑最小可行编译**，再做终稿级清理
- **先单引擎直编**，再决定是否切 `latexmk`
- **Windows 下优先保守**，不要第一次就同时动模板、字体、宏包、参考文献和图片
- **能复用官方模板默认包时就不要自定义**
- **优先让 PDF 出来**，然后再判断引用、交叉引用、版式细节

## Step 1: 识别模板和风险级别

先读取主 `.tex` 的以下信息：

- `\documentclass`
- 预加载的字体/编码包，如 `inputenc`、`fontenc`、`times`、`mathptmx`、`fontspec`
- 参考文献样式，如 `natbib`、`biblatex`、`\bibliographystyle`
- 目标 venue 模板，例如 `acmart`、`IEEEtran`、`elsarticle`

### 引擎判断矩阵

- `acmart` / `fontspec` / `unicode-math` / 明显字体冲突：优先 `xelatex`
- 传统英文论文、无字体问题、简单 `article`：先试 `pdflatex`
- 中文正文或混合 Unicode 字体需求：优先 `xelatex`
- `latexmk` 在 Windows/MiKTeX 外层日志卡住时：不要继续死磕，立即改用 `pdflatex` 或 `xelatex` 直编

## Step 2: 先做 smoke test

第一轮不要急着完整四连编。优先：

1. 单次 `pdflatex` 或 `xelatex`
2. 观察错误类型
3. 判断是模板/字体/包冲突，还是 citation/label 未收敛

### 错误分类

- `P0`：阻止 PDF 生成的错误
  - 未定义控制序列
  - 宏包冲突
  - 字体定义冲突
  - 模板类选项无效
- `P1`：PDF 已生成，但投稿还不稳
  - 引用未定义
  - 交叉引用未定义
  - 图浮动过大
  - 明显 overfull / figure 越界
- `P2`：可留到下一轮清理
  - underfull
  - 无障碍 `\Description{}` 缺失
  - 非关键 metadata 提示

## Step 3: 常见首轮错误的标准处理

### 1. `acmart` 与旧宏包冲突

高频冲突：

- `inputenc` / `fontenc` / `times`
- 手动重复加载 `amsmath` / `amssymb` / `booktabs` / `graphicx`
- 自定义 caption 行为与 `acmart` 冲突

处理原则：

- 先删重复包
- 优先保留模板自带配置
- 不要一边保留旧期刊模板残留，一边切到新模板

### 2. Windows 字体问题

高频症状：

- `\Bbbk already defined`
- Libertine / newtx / fontspec 相关冲突
- `latexmk` 卡在字体生成或日志层

处理原则：

- `acmart` 项目优先尝试 `xelatex`
- 不额外引入 `times`、`mathptmx` 等旧字体包
- 避免首轮就切多个字体系统

### 3. 参考文献问题

如果 `.aux` 已生成但 citations 未定义：

- 跑 `bibtex`
- 再跑两轮同一引擎

默认顺序：

- `xelatex -> bibtex -> xelatex -> xelatex`
- 或 `pdflatex -> bibtex -> pdflatex -> pdflatex`

### 4. `latexmk` 不稳定

在 Windows + MiKTeX 下，如果 `latexmk` 外层日志系统异常、挂起或不输出有效 TeX 错误：

- 立即切换为显式命令链
- 不要把大量时间浪费在 `latexmk` 包装层

## Step 4: 为当前项目落可复用编译方案

当项目可以稳定编译后，至少沉淀一种可重复使用方案：

- 推荐编译引擎
- 推荐命令链
- 是否需要 `bibtex`
- 哪些 warning 当前已知可接受
- 哪些 warning 是下一轮必须清理

优先把这些信息落成：

- 一个 `build.ps1`
- 或一个简单的 `README` / `compile_notes.md`
- 或在回复中明确写出标准命令

## Step 5: 只清理高价值 warning

首轮编译后，不要把所有 warning 一锅端。

优先清理：

- 图片越界
- 未定义引用
- 缺少 bibliography
- 模板明显不合规
- 匿名性泄露

可延后清理：

- `Underfull \vbox`
- 非关键 caption 样式告警
- 若期刊尚未要求，可暂缓处理的 production metadata

## 默认输出格式

- `模板识别`
- `推荐引擎`
- `标准编译命令`
- `首轮报错分类`
- `已完成修复`
- `剩余 warning`
- `下一步是否进入 submission 清理`

## 推荐命令模板

### Windows + `xelatex`

```powershell
xelatex -interaction=nonstopmode -halt-on-error main.tex
bibtex main
xelatex -interaction=nonstopmode -halt-on-error main.tex
xelatex -interaction=nonstopmode -halt-on-error main.tex
```

### Windows + `pdflatex`

```powershell
pdflatex -interaction=nonstopmode -halt-on-error main.tex
bibtex main
pdflatex -interaction=nonstopmode -halt-on-error main.tex
pdflatex -interaction=nonstopmode -halt-on-error main.tex
```

## 不该做什么

- 不要第一次就强依赖 `latexmk`，尤其是在 Windows + MiKTeX 下
- 不要在模板切换时保留一整套旧字体和旧版式宏包
- 不要在 PDF 还没出来前就先修所有 warning
- 不要把 citation 未定义误判成模板致命错误
- 不要在未确认主文件前盲目编译整个目录

## 与其他 Skills 的关系

- 当项目已能稳定编译，进入投稿终审时，交给 `latex-submission-packager`
- 当任务是 venue 切换而非纯编译 bootstrap，优先 `venue-submission-adapter`
- 当日志干净但内容还不适配目标期刊，交给 `paper-ratchet-optimizer` 或对应内容 Skill

## 标准输入

- 主 `.tex` 文件路径
- `.bib` 文件路径
- 目标 venue 或模板类型
- 当前操作系统与 TeX 发行版
- 首轮编译报错日志

## 标准交付

- 可用的首次编译方案
- 推荐引擎与命令链
- 首轮修复后的可编译 PDF
- 剩余 warning 的优先级列表
- 当前项目可复用的构建说明

## Examples

- “这个新项目第一次编译总是报字体错误，先帮我配到能稳定出 PDF。”
- “我刚导入一个 ACM 模板，帮我快速搞定第一次 LaTeX 编译。”
- “Windows 上 `latexmk` 总卡住，帮我整理一个首次编译 bootstrap 流程。”
