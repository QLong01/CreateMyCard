---
name: harmony-card-generation-v5
description: "根据 harmonyos-a2ui-form-protocol 生成 HarmonyOS A2UI Form 服务卡片 DSL JSONL：使用 Form catalog、10 个 Form 支持组件、onClick 行为链、表达式/DataModel 和 2x2 或横版 2x4 卡片构造规则。适用于创建、优化、校验或写入 HarmonyOS/A2UI/Form/服务卡片/widget 卡片/DSL/JSONL 产物，目标场景为 160x160vp 或 320x160vp。"
---

# Harmony 卡片生成 V5

## 这个 Skill 做什么

- 根据一句话生成 HarmonyOS A2UI Form 服务卡片 DSL。
- 使用可泛化的构图规则构造卡片：
  - `2x2`：`160 x 160vp`
  - `2x4`：`320 x 160vp` 横版
- 遵循自包含工作流：模式识别、按需读取参考、先说明布局理由再写 JSON、显式改进、优先落盘迭代、脚本校验和最终验收。
- 使用 `harmonyos-a2ui-form-protocol.md` 的 Form 裁剪协议。协议冲突时，以该协议为准。

## 执行边界

- 默认不要联网。使用本地 `harmonyos-a2ui-form-protocol.md` 和本 skill 自带参考。
- 除非用户明确提供已有 DSL 文件用于优化，否则不要主动读取、复制、改造或模仿历史卡片模板、示例卡片、旧产物、截图或已生成 JSON。
- 不要把“选择模板”作为生成步骤。布局决策只来自可泛化的构造规则。
- 除非用户明确改变任务，否则不要把桌面卡片扩展成页面、文章、长列表或仪表盘。
- 不要发明组件、样式键或预定义函数。只有在用户/宿主提供或明确声明为宿主自定义函数假设时，才允许使用应用特定函数。

## 协议优先级

按以下顺序解决冲突：

1. 仓库根目录的 `harmonyos-a2ui-form-protocol.md`
2. [`reference/protocol.md`](reference/protocol.md)
3. [`reference/component-catalog.md`](reference/component-catalog.md) 和 [`reference/data-binding.md`](reference/data-binding.md)
4. 设计、构图和评审文档

设计文档只负责卡片质量，不得放宽 Form 协议约束。

## 第一原则：用户需求优先

- 把用户的显式需求作为最高优先级。
- 当显式用户需求与默认卡片规则冲突时，满足用户需求并记录这个受限例外。
- 其他无关的校验和设计检查仍然生效。不要因为一个例外绕过整个工作流。
- 从用户场景选择视觉语言，不要从示例或历史输出继承。

## 场景识别

生成前，先对用户的一句话分类：

- `status glance`：当前状态、指标、天气、电量、健康、进度或告警。
- `time/reminder`：会议、课程、约会、倒计时、行程、截止日期、提醒。
- `action card`：一个明确的下一步动作，例如呼叫、打开、专注、提醒、导航、切换。
- `device/product`：资源图片、电量、模式、播放、连接状态。
- `information summary`：把地点、人物、事件、概念或实体压缩成紧凑卡片。
- `unsupported/page-like`：多个章节、长说明、复杂表单、表格、完整页面。

对于信息摘要类请求，保持输出像卡片：

- 结构：标题 -> 1 到 3 个核心属性 -> 简短摘要 -> 可选标签/动作。
- 正文 <= 2 行短文本。
- 默认不要使用动态列表模式。
- 不要使用全宽 16:9 hero 图。
- 只有当用户明确要求丰富细节时，才升级为页面。

## 尺寸选择

写 DSL 前必须且只能选择一个尺寸：

### `2x2` / `160 x 160vp`

默认尺寸。用于一个主要结论和一个辅助上下文：

- 状态速览、简单提醒、天气+下个事件、设备状态、单个进度指标、一个动作。
- 主区域：<= 3。
- 适合无需滚动、无需阅读段落即可一眼理解的答案。

### `2x4` / `320 x 160vp`

仅当场景确实需要横向宽度时使用：

- 左右关系、两个紧凑面板、更宽的受保护文本、更丰富的设备/产品状态、媒体加状态、详情加动作。
- 主区域：<= 4。
- 默认不滚动。如果卡片需要滚动，它很可能已经接近页面。

如果压缩后两个尺寸都装不下请求，使用模式 3。

## 模式选择

必须进入且只进入一个模式：

### 模式 1：一句话桌面卡片

默认模式。用户用自然语言要求生成新卡片，且没有提供现有 DSL。

交付物：

1. `*_card.dsl.jsonl`

### 模式 2：已有 DSL 优化 / 评审

当用户提供已有 GenUI 卡片 DSL 文件，或要求修复/评审某个文件时使用。

交付物：

1. 已编辑的磁盘 DSL 文件
2. 校验器结果
3. 简短的问题/修复摘要

### 模式 3：能力边界 / 升级说明

当请求超出紧凑桌面卡片范围时使用。

交付物：

1. 清楚说明不支持的部分
2. 给出最接近的受支持 2x2 或 2x4 卡片替代方案
3. 只有在用户接受更窄范围时，才继续询问或执行

## 参考路由

先确定模式，再按下面路由读取。不要只看资源列表；资源列表只是索引。

### 默认必读

- 新卡片：[`reference/protocol.md`](reference/protocol.md)、[`reference/capability.md`](reference/capability.md)、[`reference/card-composition-rules.md`](reference/card-composition-rules.md)、[`reference/guide.md`](reference/guide.md)。
- 已有 DSL 修复/评审：[`reference/protocol.md`](reference/protocol.md)、[`reference/review-validation.md`](reference/review-validation.md)、[`reference/component-catalog.md`](reference/component-catalog.md)、[`reference/data-binding.md`](reference/data-binding.md)。
- 能力边界分流：[`reference/protocol.md`](reference/protocol.md)、[`reference/capability.md`](reference/capability.md)、[`reference/card-composition-rules.md`](reference/card-composition-rules.md)。
- 最终验收：只从 [`reference/review-validation.md`](reference/review-validation.md) 进入；该文档会按需调用 [`reference/design-review.md`](reference/design-review.md)。

### 条件触发

- 组件、属性、样式枚举、`children` 形状不确定：读取 [`reference/component-catalog.md`](reference/component-catalog.md)。
- 出现 `updateDataModel`、表达式、模板循环、事件参数或宿主动作 ID：读取 [`reference/data-binding.md`](reference/data-binding.md)。
- 出现 CTA、可点击区域、`Button`、`onClick`、图片来源或媒体真实性问题：读取 [`reference/visual-interaction.md`](reference/visual-interaction.md)。
- 需要确定 padding、`itemMargin`、圆角、阴影、半透明块或视觉层级尺度：读取 [`reference/spacing-elevation.md`](reference/spacing-elevation.md)。
- 没有真实本地图片但需要视觉锚点，或需要渐变、字形、`Progress`、`Divider`、`Stack` 增强表现力：读取 [`reference/expressiveness-toolkit.md`](reference/expressiveness-toolkit.md)。
- 只做视觉润色或卡片质量评审：读取 [`reference/design-review.md`](reference/design-review.md)；最终交付前仍回到 [`reference/review-validation.md`](reference/review-validation.md)。
- 不确定该读哪个文件：先读 [`reference.md`](reference.md)，再只读它指向的相关文件。

## 输出持久化

最终产物默认写入文件。

优先级：

1. 如果用户指定路径，使用该路径。
2. 如果优化已有文件，直接编辑该文件。
3. 否则保存在当前工作目录下，文件名使用简短 slug，例如 `meeting-focus_card.dsl.jsonl`。

默认 JSONL 顺序：

1. `createSurface`
2. `updateComponents`
3. `updateDataModel`

`updateComponents` 必须在 `createSurface` 之后，同一 surface 只发送一次完整 `updateComponents`。

首版草稿保存后，在已有文件上继续迭代。除非结构不可用，否则不要每轮重新生成一个全新产物。

## 工作流

1. 读取用户请求并判断场景/模式。
2. 按 [`reference/capability.md`](reference/capability.md) 确认请求是否在能力范围内。不在范围内则使用模式 3。
3. 按 [`reference/card-composition-rules.md`](reference/card-composition-rules.md) 选择 `2x2` 或 `2x4`。
4. 从请求中推导语义角色：identity、primary answer、metric、context、progress/trend、media、action。
5. 按“参考路由”读取该模式和触发条件所需的参考。
6. 写 JSON 前必须明确说明布局理由，覆盖：
   - 选择的尺寸以及原因
   - 语义角色和主区域
   - 视觉焦点
   - 信息节奏
   - 关键横向关系
   - 必须完整显示的关键信息
   - 每个拥挤 Row 的组件内部宽度预算
   - 交互和 DataModel 形状
7. 正式输出前至少做一次显式改进：
   - 指出第一个内部版本缺少什么
   - 改进层级、紧凑度、场景视觉特征，或关键信息完整显示的安全性
8. 生成并保存 JSONL 文件。
9. 运行 `python scripts/validate_genui_card.py <path-to-dsl.jsonl>`。
10. 直接在文件中修复校验错误，并重复运行直到通过。
11. 脚本通过后，按 [`reference/review-validation.md`](reference/review-validation.md) 做最终验收；该文档会在需要时调用 [`reference/design-review.md`](reference/design-review.md) 做视觉、交互和数据语义评审。
12. 如果最终验收修改了文件，重新运行校验器。
13. 只有在校验通过且最终验收完成后交付。

## 不可妥协项

- 协议合法性不能由设计文档放宽。具体组件、事件、媒体、表达式和 catalog 约束以 [`reference/protocol.md`](reference/protocol.md)、[`reference/component-catalog.md`](reference/component-catalog.md)、[`reference/data-binding.md`](reference/data-binding.md) 为准。
- 卡片形态不能放宽成页面。具体尺寸、区域数量、受保护文本和构图规则以 [`reference/card-composition-rules.md`](reference/card-composition-rules.md) 为准。
- 输出必须是落盘 JSONL，并且正式输出前必须有布局理由和至少一次显式改进。
- 每次生成、修复或最终验收后的文件改动，都必须重新运行 `scripts/validate_genui_card.py`。
- 最终交付只能从 [`reference/review-validation.md`](reference/review-validation.md) 完成；不要绕过脚本校验或最终验收。

## 资源

- 子文档索引：[`reference.md`](reference.md)
- Form 协议摘要：[`reference/protocol.md`](reference/protocol.md)
- 能力范围：[`reference/capability.md`](reference/capability.md)
- 可泛化卡片构造规则：[`reference/card-composition-rules.md`](reference/card-composition-rules.md)
- DSL 指南：[`reference/guide.md`](reference/guide.md)
- 组件目录：[`reference/component-catalog.md`](reference/component-catalog.md)
- 数据绑定：[`reference/data-binding.md`](reference/data-binding.md)
- 视觉和交互：[`reference/visual-interaction.md`](reference/visual-interaction.md)
- 间距和层级：[`reference/spacing-elevation.md`](reference/spacing-elevation.md)
- 表现力工具箱：[`reference/expressiveness-toolkit.md`](reference/expressiveness-toolkit.md)
- 设计评审：[`reference/design-review.md`](reference/design-review.md)
- 最终验收：[`reference/review-validation.md`](reference/review-validation.md)
- 校验脚本：[`scripts/validate_genui_card.py`](scripts/validate_genui_card.py)
