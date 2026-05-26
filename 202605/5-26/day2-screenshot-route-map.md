# ViV Aortic 主链路截图证据表

整理目的：把原 App 截图、页面状态、入口路径、Web 路由、数据来源、素材来源、验证状态和风险逐项对齐，作为复刻验收时的对账表。

## 主链路

```text
首页.png
-> stented页.png
-> hancock-II首页图.png
-> hancock-II-21尺寸图.png
-> hancock-II-21尺寸的THVCurrent.png
-> hancock-II-21尺寸-Evolut23的瓣膜.png
-> hancock-II-21尺寸-23的瓣膜的示例视频.png
```

旁支证据：

```text
hancock-II-21尺寸的THVSelector.png
-> hancock-II-21尺寸-LotusIdealPlacement的瓣膜.png

hancock-II-21尺寸的THVCurrent.png
-> hancock-II-21尺寸-Myval20的瓣膜.png
```

## 证据表

| 截图文件 | 页面或状态 | 入口路径 | 对应 Web 路由 | 数据来源 | 素材来源 | 是否已实现 | 是否已验证 | 风险 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `examples/images/首页.png` | Home 首页 | App 默认入口 / 底部 `Home` | `#/` | `scripts/data/categories.js` 定义五个分类入口；`scripts/data/utilities.js` 定义首页工具入口 | `work/assets-inventory/asset-map-agent.md:138` 映射该截图到 `#/`；分类 icon 来自 `categories.js` | 是。`scripts/router.js:75` 默认路由为 `#/` | 是。`scripts/verify-routes.mjs:19-21` 覆盖 `#/`；`work/docs/supervisor-checklist.md:30` 记录 `PASS verify-routes.mjs` | 低。截图只提供视觉结果，不包含组件/字段结构 |
| `examples/images/stented页.png` | Stented 列表页 | Home -> `Stented` / 底部 `Stented` | `#/category/stented` | `scripts/data/categories.js:3-11` 定义 `stented` 分类；列表数据来自 `scripts/data/valves.js` 中 `category: stented` 的瓣膜集合 | `work/assets-inventory/asset-map-agent.md:139` 映射到 `#/category/stented`；`work/docs/page-map-agent.md:47` 说明截图对应 Stented 列表页 | 是。`scripts/router.js:88` 支持 category 路由 | 是。`work/docs/supervisor-checklist.md:79` 记录 `#/` -> `#/category/stented`；`work/docs/capture-manifest.md:46` 记录真实采集链 | 低 |
| `examples/images/hancock-II首页图.png` | Hancock II 详情页 | Stented 列表 -> `Hancock II` | `#/valve/hancock-ii` | `scripts/data/valves.js:1037-1040` 定义 `hancock-ii` 和厂家 `Medtronic` | `scripts/data/valves.js:747-773` 定义详情页素材，包括 `hancock-detail-board.png`、外观、透视、marker、topview；`work/assets-inventory/asset-map-agent.md:140` 映射到 `#/valve/hancock-ii` | 是。`scripts/router.js:109` 支持 valve detail 路由 | 是。`work/docs/supervisor-checklist.md:80` 记录 Stented -> Hancock II；`work/docs/capture-manifest.md:47` 记录真实采集链 | 低 |
| `examples/images/hancock-II-21尺寸图.png` | Size: 21 尺寸页 | Hancock II -> `21` | `#/valve/hancock-ii/size/21` | `scripts/data/valves.js:837-844` 定义 `Hancock II` 的 21 尺寸参数：`stentId 18.5`、`height 15`、`trueId 17` | `work/assets-inventory/asset-map-agent.md:141` 映射到 `#/valve/hancock-ii/size/21`；`work/docs/page-map-agent.md:49` 说明截图确认尺寸页存在 | 是。`scripts/router.js:121` 支持 size 路由 | 是。`work/docs/supervisor-checklist.md:81` 记录 Hancock II -> Size 21；`work/docs/capture-manifest.md:48` 记录真实采集链 | 低到中。`page-map-agent.md` 说明原 bundle 未稳定抽到独立 Size screen 名，只能由截图确认 |
| `examples/images/hancock-II-21尺寸的THVCurrent.png` | THV Selector: Current | Size 21 -> `THV CURRENT` | `#/valve/hancock-ii/size/21/thv/current` | `scripts/data/valves.js:845-882` 定义 Current 选项：ACURATE、Allegra 23、Evolut 23、Myval 20、NAVITOR、S3 20 | `work/assets-inventory/asset-map-agent.md:143` 映射到 `#/valve/hancock-ii/size/21/thv/current` | 是。`scripts/pages/size-detail.js:92` 输出 `THV CURRENT` 入口；`scripts/router.js:132` 支持 THV 路由 | 是。`work/docs/capture-manifest.md:49` 记录真实采集链；`work/docs/supervisor-checklist.md:82` 记录 Size 21 -> Current | 低 |
| `examples/images/hancock-II-21尺寸的THVSelector.png` | THV Selector: Archived / 旧版 selector | Size 21 -> `THV ARCHIVED` | `#/valve/hancock-ii/size/21/thv/archived` | `scripts/data/valves.js:883-920` 定义 Archived 选项：Acurate NEO、Acurate TA、JenaValve、Lotus、Portico、Sapien XT | `work/assets-inventory/asset-map-agent.md:142` 说明该截图可能对应 current 或旧版 selector 总页，命名未完全稳定 | 是。`scripts/pages/size-detail.js:95` 输出 `THV ARCHIVED` 入口；`scripts/router.js:132` 支持 THV 路由 | 部分验证。`work/docs/supervisor-checklist.md:83` 记录 Size 21 -> Archived；但 `work/docs/capture-manifest.md:77-80` 说明 Archived 是旧截图，本轮未逐页复采 | 中。截图命名和当前 Web 路由不完全稳定，需要人工确认 |
| `examples/images/hancock-II-21尺寸-Evolut23的瓣膜.png` | Evolut Ideal Placement / Evolut 方案页 | THV Current -> `Evolut 23` | `#/valve/hancock-ii/size/21/thv/current/plan/evolut`；placement 深链为 `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` | `scripts/data/valves.js:859-863` 定义 Evolut 23；`scripts/data/valves.js:953-974` 定义 Evolut instruction 和 video metadata | `scripts/data/valves.js:924-930` 定义 `hancock-evolut-board.png`；`work/assets-inventory/asset-map-agent.md:144` 映射到 Evolut plan | 是。`scripts/router.js:134` 支持 plan 路由；`scripts/router.js:148` 支持 placement 路由 | 是。`work/docs/capture-manifest.md:50` 记录真实采集链；`work/docs/supervisor-checklist.md:84-86` 记录 Evolut plan 和 placement 链路 | 中低。截图标题是 Ideal Placement，但资产清单映射到 plan 路由，需要区分 plan 页和 placement 页 |
| `examples/images/hancock-II-21尺寸-Myval20的瓣膜.png` | Myval Ideal Placement / Myval 方案页 | THV Current -> `Myval 20` | `#/valve/hancock-ii/size/21/thv/current/plan/myval` | `scripts/data/valves.js:865-868` 定义 Myval 20；`scripts/data/valves.js:976-989` 定义 Myval media/video override | `scripts/data/valves.js:932-938` 定义 `hancock-myval-board.png`；`work/assets-inventory/asset-map-agent.md:145` 映射到 Myval plan | 是。`scripts/router.js:134` 支持 plan 路由 | 部分验证。`work/docs/supervisor-checklist.md:85` 记录 Current -> Myval；但 `work/docs/capture-manifest.md:77-80` 说明 Myval 分支是旧截图，本轮未逐页复采 | 中。属于旁支，不是本轮真实主采集链 |
| `examples/images/hancock-II-21尺寸-LotusIdealPlacement的瓣膜.png` | Lotus Ideal Placement | THV Archived -> `Lotus` | `#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement` | 当前 `scripts/data/valves.js:903-906` 定义 Lotus archived 选项；`scripts/data/valves.js:991-1004` 定义 Lotus media/video override | `scripts/data/valves.js:940-946` 定义 `hancock-lotus-board.png`；`work/assets-inventory/asset-map-agent.md:146` 映射到原生 Ideal Placement 深链 | 当前代码看起来已实现，但项目资料存在冲突 | 待重验。`tests/routes.test.mjs:157-161` 有 Lotus 断言；但 `work/docs/supervisor-checklist.md:89-92` 说当时 Lotus 未闭合并落到 fallback | 高。`asset-map-agent.md:156` 和旧监督文档说缺 Lotus，但当前 `valves.js` 又有 Lotus，说明资料版本不一致，必须重新跑验证并截图确认 |
| `examples/images/hancock-II-21尺寸-23的瓣膜的示例视频.png` | 视频 overlay / 播放器状态 | Evolut Ideal Placement -> `Evolut Placement Video` | 非独立页面路由；从 `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` 触发 overlay | `scripts/data/valves.js:948-952` 定义 `Evolut Placement Video`；`scripts/data/valves.js:964-968` 定义 Evolut placement video metadata | poster 使用 `./assets/images/prototype/hancock-video-poster.png`，见 `scripts/data/valves.js:951`；截图映射见 `work/assets-inventory/asset-map-agent.md:147` | 部分实现。`scripts/pages/thv-placement.js:83-92` 输出 video launcher 和 `data-video-*` | 部分验证。`work/docs/capture-manifest.md:51` 记录真实视频 overlay；但 `work/docs/supervisor-checklist.md:109` 说视频状态缺少单独截图或录屏证据 | 中高。视频不是稳定独立路由，且实际视频文件/YouTube 链接缺失，见 `work/assets-inventory/asset-map-agent.md:153-155` |

## 验收重点

1. 最稳主链路是 Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Video overlay。
2. Archived、Myval、Lotus 属于旁支，其中 Lotus 风险最高。
3. 视频状态不能只看按钮或 metadata，要保留打开后的 overlay 截图或录屏。
4. Web 路由可达不等于原生 App 全页面都已采集完成。

## 主要证据来源

- `work/docs/page-map-agent.md`
- `work/docs/capture-manifest.md`
- `work/docs/supervisor-checklist.md`
- `work/assets-inventory/asset-map-agent.md`
- `scripts/router.js`
- `scripts/data/categories.js`
- `scripts/data/utilities.js`
- `scripts/data/valves.js`
- `scripts/pages/size-detail.js`
- `scripts/pages/thv-placement.js`
- `scripts/verify-routes.mjs`
- `tests/routes.test.mjs`
