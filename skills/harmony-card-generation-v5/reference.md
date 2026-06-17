# Harmony 卡片生成 V5 参考索引

此文件只用于导航。按当前卡片任务加载最少必要参考。

## 核心原则

V5 由 Form 协议和规则共同驱动：

- 协议事实源是仓库根目录的 `harmonyos-a2ui-form-protocol.md`。
- 生成时先读 [`reference/protocol.md`](reference/protocol.md) 取得 Form 硬约束。
- 不要选择或改造内置 DSL 模板。
- 除非用户明确提供已有 DSL 产物要求编辑，否则不要模仿历史输出或示例卡片。
- 从语义角色、尺寸预算和可泛化构造规则推导卡片。

## 按任务读取

- 新的一句话桌面卡片：
  [`reference/protocol.md`](reference/protocol.md),
  [`reference/capability.md`](reference/capability.md),
  [`reference/card-composition-rules.md`](reference/card-composition-rules.md),
  [`reference/card-design.md`](reference/card-design.md),
  然后 [`reference/guide.md`](reference/guide.md)。
- 组件或样式不确定：
  [`reference/component-catalog.md`](reference/component-catalog.md)。
- 数据绑定、表达式或重复项路径：
  [`reference/data-binding.md`](reference/data-binding.md)。
- 交互、图片、CTA 或点击行为：
  [`reference/visual-interaction.md`](reference/visual-interaction.md)。
- 间距、圆角、阴影、视觉层次：
  [`reference/spacing-elevation.md`](reference/spacing-elevation.md)。
- 需要在 GenUI 约束内增强视觉表现：
  [`reference/expressiveness-toolkit.md`](reference/expressiveness-toolkit.md)。
- 最终润色和卡片质量评审：
  [`reference/design-review.md`](reference/design-review.md)。
- 校验器失败：
  先读 [`reference/review-validation.md`](reference/review-validation.md)，再读上面与问题直接相关的参考。

## 来源依据

本 skill 基于本仓库中的 Form 协议裁剪文档：

- `harmonyos-a2ui-form-protocol.md`

不要把历史示例产物作为卡片布局来源。协议原文是权威依据；本 skill 的协议参考只是生成用摘要。

## 校验

运行：

```bash
python scripts/validate_genui_card.py path/to/card.dsl.jsonl
```

脚本接受 JSONL 消息，也接受 JSON 消息数组。最终交付物仍应为 JSONL。
