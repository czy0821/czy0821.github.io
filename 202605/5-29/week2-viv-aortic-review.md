# Week2 ViV Aortic 复刻 Review

生成日期：2026-05-29  
范围：本周四天围绕 ViV Aortic 原生 App 复刻做的结构理解、证据整理、spec、代码、验证和下周计划。

## 1. 我本周理解了什么项目结构？

我本周理解到当前项目是一个静态 Web SPA，用 hash route 复刻原生 `ViV Aortic` App：

```text
index.html
-> scripts/app.js
-> scripts/router.js
-> scripts/pages/*
-> scripts/components.js
-> scripts/data/*
-> styles/app.css
```

核心数据和页面分层如下：

| 层级 | 文件 / 模块 | 本周理解 |
| --- | --- | --- |
| App shell | `index.html`, `scripts/app.js`, `styles/app.css` | 承载原生 App 风格的顶部栏、底部 Tab、浮动 Quick Selector、modal / overlay。 |
| Router | `scripts/router.js` | 负责把 `#/category/*`、`#/valve/*`、`#/utility/*` 等 hash route 映射到页面渲染函数。 |
| 页面层 | `scripts/pages/home.js`, `category.js`, `valve-detail.js`, `size-detail.js`, `thv-selector.js`, `thv-plan.js`, `thv-placement.js`, `guidance.js`, `utility.js` | 每类页面对应原生 App 的一个 screen 或 screen family。 |
| 组件层 | `scripts/components.js` | 复用 native row、bottom tabs、media panel、video panel、Quick Selector 等结构。 |
| 数据层 | `scripts/data/categories.js`, `utilities.js`, `valves.js` | 承载分类入口、工具入口、valve / size / THV / placement / video metadata。 |
| 测试层 | `tests/routes.test.mjs`, `tests/navigation.test.mjs`, `scripts/verify-routes.mjs` | 验证 route matrix、页面 marker、导航入口、media schema、fallback 和 overlay contract。 |

我也理解了原生 App 的信息架构不是单页网页，而是三层 App 结构：

- 顶层入口：Home、底部 Tab、Bookmarks、Case of the Month、Identify a Valve、Valve Fracture、More、Quick Selector。
- 类别浏览层：Stented、Stentless、Sutureless、Rings、TAVR，各自有独立 stack。
- 典型深链：`Archive/List -> Content/Detail -> Size -> THV Selector -> Plan / Ideal Placement -> Video`。

## 2. 我本周整理了哪些复刻证据？

本周整理和形成了四类复刻证据。

| 证据类型 | 文件 | 内容 |
| --- | --- | --- |
| 页面结构证据 | `work/docs/page-map-agent.md` | 梳理 Home、底部 Tab、工具入口、Quick Selector，以及 Stented / Stentless / Sutureless / Rings / TAVR 的 screen token 和推断层级。 |
| 原生截图证据 | `work/docs/capture-manifest.md` | 记录真实采集截图：Home、Stented list、Hancock II detail、Size 21、THV Current、Evolut placement、Video overlay、Bookmarks、More、Identify、Quick Selector。 |
| 监督验收证据 | `work/docs/supervisor-checklist.md` | 明确指出不能把 Web route 数量误判为原生全量页面；列出 Lotus、视频状态、全量页面清单、非截图页面盲区等风险。 |
| 原生参考汇总 | `work/docs/viv-aortic-native-reference.md` | 把真实 UI 采集、bundle token、asset 盘点、页面元素和主链关系整理成复刻参考。 |

已经真实采集并能作为复刻基线的主链截图包括：

```text
001-home-tab.png
-> 007-stented-axpress.png
-> 008-hancock-detail.png
-> 010-size21.png
-> 011-thv-current.png
-> 012-evolut-plan.png
-> 013-video-overlay.png
-> 015-after-video-back.png
-> 016-home-after-video.png
```

已经采集到的工具入口截图包括：

```text
017-bookmarks.png
018-more.png
019-identify.png
020-quick-selector.png
```

仍然标为证据不足的截图范围：

- Archived THV selector
- Myval 分支
- Lotus plan / placement 的最新逐页对照
- Case of the Month 深层页面
- Valve Fracture 深层页面
- Stentless / Sutureless / Rings / TAVR 深层页面

## 3. 我本周写了哪些 spec？

本周沉淀和使用的设计 spec 位于 `docs/superpowers/specs/`：

| Spec | 作用 |
| --- | --- |
| `2026-03-29-viv-aortic-web-design.md` | 初版 Web 复刻设计，确定中文优先、Hancock II 样例链和核心页面结构。 |
| `2026-03-30-viv-aortic-high-fidelity-ui-design.md` | 提升 UI fidelity，强调保留医疗工具工作流、native-like density、route fallback。 |
| `2026-03-30-viv-aortic-media-alignment-design.md` | 规范媒体和视频，要求 page-specific media、poster fallback、视频 panel。 |
| `2026-03-30-viv-aortic-responsive-ui-design.md` | 规范移动端和桌面端响应式行为。 |
| `2026-03-31-viv-aortic-app-fidelity-design.md` | 进一步贴近原生 App 的 shell、导航、列表、THV decision module。 |
| `2026-03-31-viv-aortic-layout-repair-design.md` | 修复布局问题，特别是 size / THV / placement 的紧凑信息组织。 |
| `2026-03-31-viv-aortic-screenshot-fidelity-design.md` | 以原生截图链为准，明确 `Home -> Stented -> Hancock II -> Size 21 -> THV -> Plan / Placement / Video`。 |
| `2026-04-01-viv-aortic-cn-upgrade-design.md` | 中文版升级 spec，覆盖页面文案、视觉升级、遗留风险和 Lotus parity gap。 |

对应执行计划位于 `docs/superpowers/plans/`，包括 web 初版、phase 2、responsive、media alignment、high fidelity、app fidelity、layout repair、screenshot fidelity 等计划文件。

## 4. 我本周做了什么代码改动？

本周代码改动可以归纳为六类。

| 类型 | 主要文件 | 改动说明 |
| --- | --- | --- |
| Home 主入口保护 | `scripts/pages/home.js`, `scripts/components.js`, `tests/navigation.test.mjs` | 给 Home 主链入口加上 `data-home-main-flow`、`data-main-flow-entry="stented"`，保证 Stented 是唯一显式主链入口；保留 Quick Selector 和底部 Tab。 |
| 原生 shell / native list | `scripts/pages/category.js`, `styles/app.css`, `tests/routes.test.mjs` | Stented 和其他 category 使用原生 list / topbar / native shell，不再落回 dashboard 式布局。 |
| Hancock II 主链页面 | `scripts/pages/valve-detail.js`, `size-detail.js`, `thv-selector.js`, `thv-plan.js`, `thv-placement.js`, `guidance.js` | 强化 detail、size、THV selector、plan、placement、guidance 的页面结构和主链可达性。 |
| 数据补齐 | `scripts/data/valves.js` | 保留 Stented 三个 demo valve：`hancock-ii`、`aspire`、`avalus`；补齐 media schema、THV option、placement、video metadata；当前 `Hancock II / Size 21 / Archived / Lotus` 已存在。 |
| 视频 / 图片 overlay | `scripts/components.js`, `scripts/state.js`, `styles/app.css`, `tests/navigation.test.mjs`, `tests/routes.test.mjs` | 增强 video panel、poster fallback、image preview panel、Quick Selector dialog contract。 |
| 测试加强 | `tests/routes.test.mjs`, `tests/navigation.test.mjs`, `scripts/verify-routes.mjs` | 覆盖 route matrix、fallback、media schema、Lotus、video metadata、THV deep routes、navigation contract。 |

当前数据层核对结果：

```text
Stented demo valves: hancock-ii, aspire, avalus
Hancock II sizes: 21, 23, 25, 27, 29
Hancock II Size 21 Current: acurate, allegra, evolut, myval, navitor, s3
Hancock II Size 21 Archived: acurate-neo, acurate-ta, jenavalve, lotus, portico, sapien-xt
Lotus placement media: ./assets/images/hancock-lotus-board.png
Current route matrix: 331 routes, 331 unique routes
```

## 5. 我本周跑了哪些验证？

本周验证分为静态 route 验证、单元级 render 验证、导航 contract 验证和数据基线核对。

| 验证 | 当前结果 | 说明 |
| --- | --- | --- |
| `tests/navigation.test.mjs` | PASS | 覆盖 Home 主入口、Stented shell、THV CTA、current / archived toggle、video panel、Quick Selector dialog、image preview panel。 |
| `tests/routes.test.mjs` | PASS | 覆盖 demo valve 数据、media schema、Hancock screenshot media、Lotus 存在性、THV deep routes、fallback、unsupported routes。 |
| `scripts/verify-routes.mjs` | 早期记录为 325 routes；当前数据基线为 331 routes | `supervisor-checklist.md` 中 325 是旧快照；当前按数据重新计算为 331 条唯一 route。 |
| 当前数据核对 | 331 routes / 331 unique | 通过读取 `scripts/data/valves.js` 计算，确认 Lotus archived placement 数据已存在。 |

本次补写 review 时，我通过 Node REPL 重新导入了：

```text
tests/navigation.test.mjs -> PASS
tests/routes.test.mjs -> PASS
```

同时尝试直接导入 `scripts/verify-routes.mjs` 时，因为当前 Node REPL 的模块链接方式无法解析 `components.js` 中的相对导入，未作为最终验证结果使用。此前 shell 中直接跑 `node scripts/verify-routes.mjs` 也出现过权限拒绝。因此本周 review 的 route 数采用数据层计算结果和测试文件覆盖情况共同说明。

## 6. 当前主链路是否可走通？

结论：主链路在 Web route / 数据 / render 测试层面已经可走通；但如果按“完整 App 复刻验收”口径，还没有完全闭环。

当前可走通的主链：

```text
#/
-> #/category/stented
-> #/valve/hancock-ii
-> #/valve/hancock-ii/size/21
-> #/valve/hancock-ii/size/21/thv/current
-> #/valve/hancock-ii/size/21/thv/current/plan/evolut
-> #/valve/hancock-ii/size/21/thv/current/plan/evolut/placement
-> video panel / overlay contract
```

当前也具备的分支：

```text
#/valve/hancock-ii/size/21/thv/current/plan/myval
#/valve/hancock-ii/size/21/thv/current/plan/myval/placement
#/valve/hancock-ii/size/21/thv/archived
#/valve/hancock-ii/size/21/thv/archived/plan/lotus
#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement
#/valve/hancock-ii/size/21/guidance/evolut-placement
```

需要注意的边界：

- “可走通”目前主要由 route render、HTML marker、data schema、navigation contract 证明。
- 还缺少从真实浏览器不手输 URL 的完整点击 E2E。
- 还缺少 Web 截图与原生截图的逐页对照归档。
- Video overlay 有 contract 测试，但仍需要保存打开后的 Web 截图或录屏证据。

所以当前主链可以说“结构上可达、测试层可渲染、核心数据已连上”，但还不能说“完整 App 复刻已验收完成”。

## 7. 当前完整 App 复刻还差什么？

完整 App 复刻还差以下几类工作。

| 缺口 | 具体内容 |
| --- | --- |
| 原生全量页面清单 | 还没有把 bundle 中所有 screen、真实截图、Web route、实现状态统一成一张全量状态表。 |
| Screenshot-to-route 映射 | 还缺 `screenshot -> route -> data node -> asset -> test` 的逐页映射表。 |
| 主链浏览器 E2E | 还缺从 Home 开始真实点击到 placement / video overlay / back / Home 的浏览器级验证。 |
| Web 截图对照 | 原生截图已有一批，但 Web 复刻截图归档不足。 |
| Lotus / Myval / Archived 证据闭环 | Lotus 数据已补，不再是单纯 fallback；但仍缺最新 native / Web 截图对照和点击链路证据。 |
| Video 证据 | 原生 video overlay 已采集，Web video panel 有测试，但还缺打开后的截图或录屏证据。 |
| 一级入口完整性 | Home 上 Stentless、Sutureless、Rings、TAVR、Case、Fracture、More 子页、Bookmarks 深层、Quick Selector 深层还没达到完整复刻。 |
| 深层页面族 | Stentless / Sutureless / Rings / TAVR 的 `Archive -> Content -> Size -> THV -> Placement` 还主要停留在 bundle token / 推断层。 |
| 素材 provenance | 图片、poster、video metadata 和真实来源还需要逐页追踪。 |
| 多视口视觉验收 | 需要 390px、768px、1014px、desktop 的截图和横向溢出检查。 |

一句话：当前完成的是“主样例链 + 结构化 demo + 证据体系第一版”，不是完整 App 全量复刻。

## 8. 我下周建议优先做什么？

下周建议按 P0 到 P1 推进。

| 优先级 | 建议任务 | 目标 |
| --- | --- | --- |
| P0 | 补主链浏览器 E2E | 从 `#/` 点击进入 Stented、Hancock II、Size 21、THV Current、Evolut plan、placement、video overlay，并验证返回。 |
| P0 | 建 screenshot-to-route 映射表 | 覆盖 Home、Stented、Hancock detail、Size 21、THV Current、Archived、Evolut、Myval、Lotus、Video、Bookmarks、More、Identify、Quick Selector。 |
| P0 | 归档 Web 对照截图 | 为主链每页保存 Web 截图，与 `work/captures/raw/` 成对记录。 |
| P0 | 补 Lotus / Myval / Archived 证据闭环 | 证明 Lotus 已从早期 fallback 风险推进到数据/route 可达，但仍需截图和 E2E。 |
| P1 | 补 Video overlay 可视证据 | 保存 Web video overlay 截图或录屏，记录触发页面、触发控件、poster、source。 |
| P1 | 补一级入口骨架 | Stentless、Sutureless、Rings、TAVR、Case、Fracture、More 子页、Bookmarks、Quick Selector 至少达到可达且状态明确。 |
| P1 | 建原生全量页面状态表 | 每个页面标注 `已截图 / 已实现 / 已验证 / 缺失 / fallback / bundle-only`。 |
| P1 | 修正并稳定 route matrix 验证命令 | 让 `node scripts/verify-routes.mjs` 在本地环境稳定输出当前 route count，避免 325 / 331 这类快照混淆。 |

建议下周验收口径：

```text
不是看页面数量，而是看每条关键链是否具备：
入口可点击
route 可达
页面内容可信
素材来源明确
截图可对照
测试可回归
```

## 本周结论

本周最核心的成果是把 ViV Aortic 从“能做一个 Web demo”推进成了“有原生证据、有 page map、有主链 route、有数据 schema、有测试保护、有监督缺口清单”的复刻项目。

当前主链已经具备可走通基础，但完整 App 复刻仍处在中期：证据体系刚搭好，下一步要把主链点击闭环、截图对照、视频状态和一级入口骨架补齐。
