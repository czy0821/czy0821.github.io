# Day 4 Review Notes

## 自查对象

对照 `day3-main-flow-replica-spec.md`，自查 Day 4 最小任务：

```text
#/ -> Stented
```

本次任务目标是 **Home 主链路入口和基础壳层的可验证保护**，不是整条主链路完整复刻。

## 自查结论

本次最小任务在自己的范围内符合 spec。

已确认：

- `#/` 正常渲染 Home
- Home 没有进入 fallback
- `Stented` 入口存在
- `Stented` 指向 `#/category/stented`
- `Stented` 是唯一显式主链路入口
- Home 保留 bottom tabs
- Home 保留 Quick Selector
- Home 保持 native shell / single-column 基线
- 当前浏览器检查没有横向滚动

不能宣称：

- 整条主链路已经完成复刻
- 深链页面已经完成截图 fidelity
- Video overlay 交互闭环已经由本任务完成

## 符合项

### Home 可渲染

spec 要求：

- `#/` 必须可渲染 Home 主入口

自查结果：

- `#/` 可以正常渲染 Home
- 没有 fallback 文案

### Stented 主链入口存在

spec 要求：

- 页面中必须存在可见且可点击的 `Stented` 入口
- Home 中 `Stented` 入口必须可点击并进入 `#/category/stented`

自查结果：

- Home 中存在 `href="#/category/stented"`
- Home 中存在 `data-main-flow-entry="stented"`
- `data-main-flow-entry` 只出现一次

### bottom tabs 和 Quick Selector 保留

spec 要求：

- Home 必须保留 native-style bottom tabs 和 Quick Selector 入口

自查结果：

- Home 中存在 `.bottom-tabs`
- Home 中存在 `.floating-quick`
- Home 中存在 Quick Selector 文案和触发按钮

### native shell 和移动端防溢出

spec 要求：

- 主链路页面继续运行在当前 native-style shell 中
- route-backed 主链路页面在 `390px` 宽度下不得出现横向滚动

自查结果：

- Home 仍包含 `screen--native-shell`
- Home 仍包含 `page--single-column`
- 浏览器检查 Home 无横向滚动

### 验证结果

已验证：

```text
tests/routes.test.mjs: PASS
tests/navigation.test.mjs: PASS
scripts/verify-routes.mjs: PASS
```

其中：

```text
PASS verify-routes.mjs (331 routes)
```

## 不符合项

本次最小任务范围内没有发现明确不符合项。

需要注意：

- 本次只完成 `#/ -> Stented` 起点保护
- 不代表 `Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay` 已经完成截图级复刻

## 潜在风险

### native shell 样式影响范围

`styles/app.css` 中调整了 native shell 宽度和 `max-width` 约束。

风险：

- 可能间接影响其他使用 native shell 的页面
- 尤其需要抽查 `#/category/stented` 和后续 stented 主链页面

### 自动化测试不能证明完整截图 fidelity

当前测试可以证明：

- 路由可达
- 主链入口存在
- 关键 DOM contract 存在
- 没有进入 fallback

但不能完全证明：

- 页面与原始截图像素级一致
- 所有视觉密度和控件位置完全符合截图

### raw capture 素材位置

如果要做正式截图验收，需要确认：

- `work/captures/raw/001-home-tab.png`
- `work/captures/raw/007-stented-axpress.png`

等 raw capture 文件是否存在并可用于对照。

## 需要补充验证

建议补充：

1. 手动点击 Home 中的 `Stented`，确认进入 `#/category/stented`
2. 在移动端宽度下抽查 `#/category/stented`
3. 抽查主链深链页面没有因为 native shell 宽度约束出现横向滚动或遮挡
4. 如果做正式验收，保存 Home 和 Stented 的移动端截图

## 后续建议

下一个最小任务建议继续从 spec 中选择一个窄边界任务，例如：

- Stented list 基线保护
- Stented list 截图复刻

不要直接把 Day 4 的 Home 入口保护扩大成整条主链路重构。
