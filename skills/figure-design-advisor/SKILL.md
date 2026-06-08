---
name: "figure-design-advisor"
description: "把论文图的表达目标转成动机图、方法总览图或实验结果图的设计方案。Invoke when 用户要设计核心 figure、优化图的信息层次、或避免图不专业。"
---

# Figure Design Advisor

这个 Skill 负责补上很多论文写作系统里容易缺失的一层：

`图不是最后随手画出来的，而是论文叙事的一部分。`

它专门处理三类高价值 figure：

- `Motivated Example Figure`
- `Solution Overview Figure`
- `Experimental Results Figure`

重点不是替你生成最终图片文件，而是先把图的叙事目标、信息层次、版式元素和常见踩雷点设计清楚，再交给绘图或排版环节。

## 何时调用

- 用户说“这张图该怎么设计才像顶会论文”
- 用户要做动机图、方法总览图、主结果图
- 用户知道想表达什么，但不知道图里该放哪些元素
- 用户担心图太满、太空、太丑、太像工程截图
- 用户已有草图，希望优化信息层次和 caption 配合

## 核心目标

1. 让图服务论文主线，而不是单纯装饰版面
2. 让 reviewer 在 3-10 秒内看懂这张图的主信息
3. 区分动机图、总览图、实验图的不同职责
4. 在设计阶段就避免信息过载、叙事错位和视觉低级错误

## 三类图的默认职责

### 1. Motivated Example Figure

用于回答：

- 现有方法哪里不够好
- 问题为什么值得研究
- 本文切入点为何自然

应强调：

- 对比感
- 问题暴露
- 直观冲突或失败案例

不应做：

- 放太多技术细节
- 一上来就讲完整方法流程
- 用难读的小字解释一切

### 2. Solution Overview Figure

用于回答：

- 你的方法由哪些模块组成
- 数据/信息怎么流动
- 与 baseline 或旧范式相比关键新增在哪里

应强调：

- 主路径
- 模块边界
- 关键创新点高亮

不应做：

- 把实现细节全塞进去
- 用过多箭头、颜色和小组件让人找不到主线
- 图和方法节命名不一致

### 3. Experimental Results Figure

用于回答：

- 最关键结果是什么
- 趋势是否稳定
- 机制或边界在哪里

应强调：

- 可比较性
- 统计信息
- 读者最该看到的趋势

不应做：

- 同时挤太多 benchmark/setting
- 只图好看，不图可读
- 图题、图例、caption 和正文结论互相打架

## 执行步骤

### Step 1: 先冻结图的核心句子

先写一句话：

- 这张图最想让 reviewer 看懂什么

如果一句话说不清，就不要急着画。

### Step 2: 判断图类型

只能优先选一个主类型：

- 动机图
- 总览图
- 实验结果图

若一张图同时承担三个职责，通常说明图设计过载。

### Step 3: 设计信息层次

至少拆成三层：

- `Primary message`：读者 3 秒内必须看到
- `Secondary details`：读者多看 10 秒能理解
- `Supportive details`：可放 caption 或 appendix，不一定放图里

### Step 4: 指定画面元素

按图类型选择元素，例如：

- 动机图：输入、失败案例、对比框、关键差异标注
- 总览图：模块框、输入输出、数据流、训练/推理路径、创新高亮
- 实验图：坐标轴、误差线、颜色编码、baseline 分组、图例位置

### Step 5: 做审稿视角排雷

重点检查：

- 是否一眼就能看懂主信息
- 颜色、线型、框线是否过多
- 文本是否太小
- 模块名是否与正文一致
- 是否需要黑白打印、投影、缩放后仍可读
- caption 是否能承接而不重复塞满图内

## 默认输出格式

- `Figure Goal`
- `Figure Type`
- `Primary / Secondary / Supportive Info`
- `Recommended Layout`
- `Elements to Include`
- `What Not to Put in the Figure`
- `Caption Coordination Notes`

## 标准输入

- 想表达的核心信息
- 当前论文主线或相关 section
- 图的类型偏好或已有草图
- 是否面向主文、附录、海报或 rebuttal
- 用户最担心的问题：不够直观 / 太复杂 / 不像顶会 / 不知道如何布局

## 标准交付

- 图类型判断
- 信息层次设计
- 版式和元素建议
- 视觉排雷清单
- 与 caption、正文的配合建议

## Examples

- “我想做一张动机图，说明现有 sparse routing 为什么不稳，这张图该怎么设计？”
- “帮我设计一个方法总览图，突出我的核心模块，不要画得太满。”
- “这张主结果图该怎么改，才能让 reviewer 一眼看到关键趋势？”

## 与其他 Skills 的关系

- 前置主线来自 `paper-positioning`
- 若图服务实验叙事，后续接 `experiment-story-builder`
- 若图已经成稿，需要投稿级审计，后续接 `submission-integrity-audit`
- 若用户请求模糊，先经 `paper-intake-router`
