# Day 2 Page Map Review Notes

Review 对象：

- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md>)

Review 口径：

- 本次 review 以今天已经核对过的项目资料为准：
  `work/docs/page-map-agent.md`
  `work/docs/capture-manifest.md`
  `work/docs/supervisor-checklist.md`
  `work/docs/viv-aortic-native-reference.md`
  `work/assets-inventory/asset-map-agent.md`
  `scripts/router.js`
  `scripts/data/valves.js`
  `tests/routes.test.mjs`
  `tests/navigation.test.mjs`
- 我把“已验证”理解为：至少要能分清是 `Web 路由/测试已验证`，还是 `原生截图/高保真已验证`。如果两者混写，就会把状态写得过满。

## Review Findings

### 1. [高] `Lotus placement` 被写成“已实现 / 已验证”，和其引用证据直接冲突

问题位置：

- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:110>)

为什么是问题：

- 这份 page map 在 `5. 主链路页面总表` 里把 `Lotus placement` 标成了“已实现 / 已验证”。
- 但今天我们反复核对过的监督文档明确写过：`Lotus` 目标链当时**没有闭合**，而且会落到 `未找到理想位置` fallback。
- 当前代码里确实又出现了 `Lotus` 数据和素材 override，所以真实情况不是“稳定已验证”，而是“资料版本冲突，必须重验”。

证据：

- [supervisor-checklist.md](<D:/学习/vie-aorta-master/work/docs/supervisor-checklist.md:89>)
- [supervisor-checklist.md](<D:/学习/vie-aorta-master/work/docs/supervisor-checklist.md:92>)
- [supervisor-checklist.md](<D:/学习/vie-aorta-master/work/docs/supervisor-checklist.md:165>)
- [valves.js](<D:/学习/vie-aorta-master/scripts/data/valves.js:903>)
- [valves.js](<D:/学习/vie-aorta-master/scripts/data/valves.js:991>)

建议：

- 把 `Lotus placement` 改成：`当前代码存在对应数据；与旧监督记录冲突，待重验`
- 不要在页面总表里直接写“已验证”，除非补上当前版本的真实路由验证和截图证据

### 2. [中] `THV Archived` 和 `Myval plan` 被写成“已验证”，但今天证据更支持“已实现，未完成高保真复采”

问题位置：

- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:107>)
- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:109>)

为什么是问题：

- `capture-manifest.md` 已经明确写到：`Archived THV selector`、`Myval 分支` 属于“已有旧截图，但本轮未逐页复采”。
- 这意味着我们可以说它们的 `Web route / data` 可能存在，但不能轻易写成“已验证”，至少不能让人误会成“原生高保真已验证”。

证据：

- [capture-manifest.md](<D:/学习/vie-aorta-master/work/docs/capture-manifest.md:77>)
- [capture-manifest.md](<D:/学习/vie-aorta-master/work/docs/capture-manifest.md:79>)
- [capture-manifest.md](<D:/学习/vie-aorta-master/work/docs/capture-manifest.md:80>)
- [asset-map-agent.md](<D:/学习/vie-aorta-master/work/assets-inventory/asset-map-agent.md:142>)
- [asset-map-agent.md](<D:/学习/vie-aorta-master/work/assets-inventory/asset-map-agent.md:145>)

建议：

- `THV Archived` 建议改成：`已实现；Web 路由已覆盖；原生高保真复采待确认`
- `Myval plan` 建议改成：`已实现；旧截图存在；本轮未逐页复采`

### 3. [中] `Stentless / Sutureless / Rings` 的“入口和列表样式已实现”写法偏满，证据支撑不够对称

问题位置：

- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:54>)
- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:55>)
- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:56>)

为什么是问题：

- `Stentless` 那一行引用了 `routes.test.mjs` 里的 `freestyle fallback`，这更能证明“unsupported detail/fallback behavior”，不直接证明“原生列表样式已实现且可信”。
- `Sutureless` 和 `Rings` 这里还引用了 `capture-manifest.md`，但 `capture-manifest` 只说明这些模块在“非 screenshot 范围 / 未逐页采集”的区域里，并没有提供这两条链路的真实采集页证据。
- 更稳妥的写法应该是：`分类入口和基础 category route 已实现；深层页面仍缺原生逐页对照`。

证据：

- [routes.test.mjs](<D:/学习/vie-aorta-master/tests/routes.test.mjs:417>)
- [routes.test.mjs](<D:/学习/vie-aorta-master/tests/routes.test.mjs:432>)
- [valves.js](<D:/学习/vie-aorta-master/scripts/data/valves.js:1154>)
- [valves.js](<D:/学习/vie-aorta-master/scripts/data/valves.js:1156>)
- [valves.js](<D:/学习/vie-aorta-master/scripts/data/valves.js:1158>)
- [capture-manifest.md](<D:/学习/vie-aorta-master/work/docs/capture-manifest.md:83>)

建议：

- `Stentless / Sutureless / Rings` 统一改成：
  `分类入口已实现，存在样例 slug 或基础 category route；深层链路未完成高保真验证`

### 4. [低] `Quick Selector` 这一块其实可以写得更准，不必只说“有弹层结构”

问题位置：

- [cyf-day2-app-page-map.md](</C:/Users/Administrator/Downloads/cyf-day2-app-page-map.md:39>)

为什么是问题：

- 这里不是“写错了”，而是“可以更准确”。
- 今天的测试证据已经能支持：`Quick Selector` 在 Web 里至少是一个 `dialog/modal shell`，不是模糊意义上的“有弹层结构”。
- 但它和原生是否一比一高保真，仍然不能直接下结论。

证据：

- [navigation.test.mjs](<D:/学习/vie-aorta-master/tests/navigation.test.mjs:92>)
- [navigation.test.mjs](<D:/学习/vie-aorta-master/tests/navigation.test.mjs:93>)
- [navigation.test.mjs](<D:/学习/vie-aorta-master/tests/navigation.test.mjs:94>)
- [page-map-agent.md](<D:/学习/vie-aorta-master/work/docs/page-map-agent.md:320>)

建议：

- 把“当前 Web 有浮动按钮和弹层结构”改成：
  `当前 Web 已有浮动按钮和 modal/dialog 壳层；承载形态与原生高保真关系仍待确认`

## Open Questions

- `Lotus` 当前代码状态和旧监督记录冲突，到底是哪一版资料落后了：监督清单、资产清单，还是代码后来补过但文档没回填。
- `THV Archived` 的“已验证”到底想表达 `Web route/test verified`，还是 `native fidelity verified`。这份 page map 里现在没有把这两个层级分开。

## Today Corrections

| 原说法 | 今天建议修正为 | 修正依据 |
| --- | --- | --- |
| `Lotus placement：已实现 / 已验证` | `Lotus placement：当前代码存在对应数据；与旧监督记录冲突，待重验` | `supervisor-checklist.md:89-92` 与 `scripts/data/valves.js:903, 991` 冲突 |
| `THV Archived：已验证` | `THV Archived：已实现；Web 路由可达；原生高保真复采待确认` | `capture-manifest.md:77-80` 明确说 archived 是旧截图，本轮未逐页复采 |
| `Myval plan：已验证` | `Myval plan：已实现；旧截图存在；本轮未逐页复采` | `capture-manifest.md:77-80`；`asset-map-agent.md:145` |
| `Stentless / Sutureless / Rings：入口和列表样式已实现` | `分类入口已实现；存在样例 slug 或基础 category route；深层页面仍未完成高保真验证` | `routes.test.mjs:417-440` 更偏 fallback 验证；`valves.js:1154-1159` 只证明样例 slug 存在 |
| `Quick Selector：当前 Web 有浮动按钮和弹层结构` | `Quick Selector：当前 Web 已有浮动按钮和 modal/dialog 壳层；与原生高保真关系待确认` | `navigation.test.mjs:92-94` 已能证明 dialog/modal shell |

## Summary

这份 page map 的整体框架是对的，尤其是：

- 顶层入口、底部 Tab、首页工具、主链路、More/Identify/Fracture 分层写法是清楚的
- 它已经比“只列几个页面名”更接近真正能用于验收的页面总表

今天最需要收紧的地方，不是结构，而是**状态措辞**：

- 不要把 `Web route/test verified` 直接写成 `已验证`
- 对 `Lotus`、`Archived`、`Myval` 这种分支，要把“存在代码”与“完成高保真复采”分开写
- 对 `Stentless / Sutureless / Rings / TAVR`，要更诚实地区分“入口存在”和“深层链路闭环”
