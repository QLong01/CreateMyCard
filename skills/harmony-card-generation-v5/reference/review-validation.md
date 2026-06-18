# 最终验收

## 目的

首版草稿写入磁盘后，使用本文档作为唯一最终验收入口。本文档负责编排脚本校验、设计评审、受保护文本检查和最终阻塞项；不要在 `SKILL.md` 中另行并列调度 `design-review.md`。

## 职责边界

- 协议合法性：由 `scripts/validate_genui_card.py` 机械检查。
- 视觉、交互、数据语义质量：按 [`design-review.md`](design-review.md) 检查。
- 受保护文本和最终阻塞项：由本文档检查。

## 端到端流程

1. 读取磁盘上的 DSL 文件；不要从记忆重新起草。
2. 运行 `python scripts/validate_genui_card.py <file>`。
3. 直接在同一个文件中修复校验错误，并重复运行直到通过。
4. 脚本通过后，读取 [`design-review.md`](design-review.md) 做视觉、交互和数据语义评审。
5. 按本文档做受保护内容换行评审和最终阻塞项检查。
6. 如果任何评审修改了文件，重新运行校验器。
7. 只有在最终编辑后校验通过时才交付。

## 协议阻塞项

以下任一项失败都不能交付：

- 输出不是 JSONL，或每行不止一个 object。
- `version` 不是 `"v0.9"`。
- `createSurface.catalogId` 不是 `ohos.a2ui.extended.catalog`。
- `createSurface` 包含 `theme`。
- 同一 surface 有多次 `updateComponents`。
- `surfaceId` 不一致。
- component ID 重复或 child 引用不可解析。
- 使用 Form 10 个支持组件之外的组件。
- 使用 `Text.text`、`Image.url`、`Button.child`、`Button.action` 或 CSS kebab-case 样式键。
- 使用 `onClick` 之外的事件。
- 使用 `$__widthBreakpoint`、`$__colorMode`、网络图片 URL、SVG 或占位媒体 URL。

## 卡片阻塞项

以下任一项失败都需要修改或转入能力边界说明：

- 未明确选择 `2x2` 或 `2x4`。
- `2x2` 主区域超过 3 个，或 `2x4` 主区域超过 4 个。
- 主要信息需要滚动、长列表、表格、段落或页面导航才能理解。
- 卡片由示例/模板改造而来，而不是由语义角色和构图规则生成。
- 正式输出前没有布局理由或没有至少一次显式改进。
- 可点击视觉区域没有 `onClick` EventHandler。

## 受保护内容换行评审

对每个横向布局：

1. 列出该行中的受保护内容：日期、星期、CTA、时间、状态、百分比、价格、短标签、主数字、主标题和用户要求字段。
2. 检查它是否位于狭窄固定宽容器中。
3. 检查它是否使用 `textOverflow: "ellipsis"`、`clip` 或 marquee。对受保护内容来说，这是阻塞问题。
4. 检查弱文本是否会先于受保护内容压缩。
5. 按此顺序修复：
   - 加宽受保护列
   - 缩短或移动弱文本
   - 减少 padding、`itemMargin`、分隔线宽度和装饰性固定列
   - 在层级内降低字号
   - 把该行拆成 `Column`
   - 选择 `2x4`，或在受保护文本仍无法完整显示时升级说明

不要接受受保护短短语逐字换行。不要接受受保护文本显示为 `...`。

## 用户需求优先例外

如果用户明确要求有风险的布局或看起来不受支持的细节：

- 使用尽可能小的受限例外。
- 保持协议有效性检查生效。
- 在最终回复中说明这个例外。
