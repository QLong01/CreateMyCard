# 评审和校验

## 目的

首版草稿形成 `genui` 与 `cardspec` 两个代码块后，使用这个流程保持输出有效、紧凑并且视觉完整。

## 端到端流程

1. 读取草稿中的 `genui` 和 `cardspec` 内容；不要从记忆重新起草。
2. 用临时文件运行 `python scripts/validate_genui_card.py <temp.dsl.jsonl>`。
3. 直接修复草稿内容，并重复运行直到通过。
4. 脚本通过后，使用 `design-review.md` 做设计评审。
5. 做受保护内容换行评审。
6. 如果设计评审修改了内容，重新运行校验。
7. 用临时文件运行 `python scripts/validate_cardspec.py <temp.cardspec.json>`。
8. 只有在最终编辑后相关校验都通过时才交付。

## 每轮检查清单

每轮都检查：

- 模式已识别：一句话卡片、已有 DSL 评审，或能力边界。
- 所选尺寸明确：`2x2` 或 `2x4`。
- `genui` 代码块是 JSONL，每行一个 object。
- `cardspec` 代码块是一个 JSON object。
- `createSurface.catalogId` 是 `ohos.a2ui.extended.catalog.form`。
- `createSurface` 没有 `theme`。
- 同一 surface 只有一次完整 `updateComponents`。
- 所有 `surfaceId` 一致。
- 所有 component ID 唯一，引用可解析。
- 组件名来自 Form 支持的 10 个扩展组件。
- 使用 extended 字段名：`content`、`src`、`label`。
- `updateDataModel.path` 和模板 `children.path` 使用 `/` JSON Pointer。
- 组件可见动态值默认使用完整表达式读取 `$__dataModel`。
- 不使用 `$__widthBreakpoint` 或 `$__colorMode`。
- 正式输出前已说明布局理由。
- 最终输出前至少发生一次改进。
- 不存在网络/占位媒体 URL。
- 所有可点击视觉区域都有 `onClick`。
- 卡片仍保持紧凑和摘要感。
- 卡片由可泛化规则构造，而不是从所选模板构造。
- CardSpec 存在，且 `suggestSize` 与 DSL 尺寸一致。
- CardSpec 中所有 `capabilityId`、`capabilityVersion` 和 `arguments` 来自已声明能力；静态卡片没有数据能力时也要有最小 CardSpec。
- DSL 中所有 UI 绑定路径能从 CardSpec 的 `writeResultTo` 和能力 `outputSchema` 推导；静态卡片则从初始 DataModel 推导。

## 组件/卡片检查

- Root 尺寸匹配 `2x2`（`160 x 160vp`）或横版 `2x4`（`320 x 160vp`），或宽度响应式但仍遵守所选布局预算。
- `2x2` 主区域 <= 3，`2x4` 主区域 <= 4。
- 视觉焦点明显。
- 卡片不是页面尺寸块。
- 内部块没有变成多个互相竞争的小卡片。
- 横向行区分受保护内容和可压缩内容。
- CTA、时间、百分比、状态、价格和短标签保持可读。
- 日期、星期、时间、状态、CTA、主指标、主标题和用户要求字段完整显示，不使用省略或裁剪。
- 受保护文本上的固定宽度有明确理由。
- 如果存在本地图片，使用 `Image.src` 和 `styles.objectFit`。

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

不要接受受保护短短语逐字换行。
不要接受截图中受保护文本显示为 `...`。

## 用户需求优先例外

如果用户明确要求有风险的布局或看起来不受支持的细节：

- 使用尽可能小的受限例外。
- 保持协议有效性检查生效。
- 在最终回复中说明这个例外。
