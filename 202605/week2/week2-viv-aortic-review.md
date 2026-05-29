# Week 2 ViV Aortic Review

生成日期: 2026-05-29

## 1. 验收结论

第二周 ViV Aortic 复刻项目当前可评为:

```text
B：主链路分析和基础保护已建立，核心 route / DOM contract 可验证，但完整 App 复刻尚未达到验收通过。
```

本周的实际成果更接近“复刻工程基线搭建 + 主链路证据整理 + 最小代码保护落地”，而不是完整页面高保真重做。

可以确认已经完成:

- 理解并梳理了项目结构、运行方式、路由机制和数据来源。
- 建立了 `Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay` 主链路口径。
- 梳理了 page map、capture manifest、supervisor checklist、native reference 和素材证据。
- 产出了主链路 spec、文件映射、implementation plan、gap list、review notes 等文档基础。
- Day 4 已对 `#/ -> Stented` 这个主链起点做了最小代码保护。
- 当前 `routes.test.mjs`、`navigation.test.mjs` 通过。
- 当前可枚举验证 331 条 route，失败 0。

不能声明已经完成:

- 不能声明完整 App 已复刻完成。
- 不能声明所有 native 页面都已采集。
- 不能声明所有一级入口和深层页面都达到 native parity。
- 不能把 route render 可达等同于截图高保真或真实点击链完全闭环。

## 2. Review 范围

本次第二周验收 review 汇总了四个线程的成果:

| 文件 | 对应阶段 | 重点 |
| --- | --- | --- |
| `week2-thread-1-summary.md` | Day 1 | 项目结构、运行方式、主链路基础、验证命令。 |
| `week2-thread-2-summary.md` | Day 2 | 页面地图、截图证据、复刻缺口、Lotus / Video / Quick Selector 风险。 |
| `week2-thread-3-summary.md` | Day 3 | 主链路 spec、文件映射、implementation plan、执行前验收口径。 |
| `week2-thread-4-summary.md` | Day 4 | 最小实现任务: 保护 `#/ -> Stented` 主链起点，并补测试。 |

本次 review 范围包括:

- 项目结构理解。
- 主链路可达性。
- 截图证据。
- 页面 spec。
- 代码改动。
- 测试验证。
- 完整 App backlog。

## 3. 项目结构理解

当前 `vie-aorta-master` 是一个原生 HTML + CSS + ES Module JavaScript 写成的静态 Web SPA。

它不是:

```text
React / Vue / Next / Vite / Webpack 项目
```

核心运行链路:

```text
index.html
-> scripts/app.js
-> scripts/router.js
-> scripts/pages/*
-> scripts/data/*
-> styles/app.css
```

关键文件职责:

| 文件 / 目录 | 职责 |
| --- | --- |
| `index.html` | 浏览器入口，提供 `#app` 挂载点，加载 `scripts/app.js`。 |
| `scripts/app.js` | App 启动器和事件总管，负责 render、hashchange、搜索、收藏、弹层、图片预览、视频面板等事件绑定。 |
| `scripts/router.js` | hash 路由分发器，根据 route segments 调用不同页面渲染函数。 |
| `scripts/pages/home.js` | 首页入口列表和 Quick Selector 浮动入口。 |
| `scripts/pages/category.js` | 分类列表页，当前 Stented 是主链重点。 |
| `scripts/pages/valve-detail.js` | 瓣膜详情页，例如 Hancock II。 |
| `scripts/pages/size-detail.js` | 尺寸页，例如 Size 21。 |
| `scripts/pages/thv-selector.js` | THV Current / Archived tile 选择页。 |
| `scripts/pages/thv-plan.js` | option plan 页，目前包含隐藏 placement deep link。 |
| `scripts/pages/thv-placement.js` | Ideal Placement 页面。 |
| `scripts/pages/guidance.js` | guidance 页面和视频入口。 |
| `scripts/data/categories.js` | 5 个底部分类入口数据。 |
| `scripts/data/utilities.js` | 5 个首页工具入口数据。 |
| `scripts/data/valves.js` | valve、size、THV、placement、video、media schema。 |
| `scripts/components.js` | shell、topbar、bottom tabs、native row、quick modal、video panel、image panel 等共享 UI。 |
| `styles/app.css` | 全局视觉系统、native shell、布局、selector、placement、overlay、响应式。 |
| `tests/routes.test.mjs` | route、数据结构、media contract、fallback、deep route 验证。 |
| `tests/navigation.test.mjs` | navigation、Home 主入口、overlay、quick selector、image preview 验证。 |
| `scripts/verify-routes.mjs` | 批量验证主要 hash routes 是否可渲染。 |

## 4. 主链路可达性

本周收敛出的主链路:

```text
Home
-> Stented list
-> Hancock II detail
-> Size 21
-> THV Current
-> Evolut
-> Placement
-> Video overlay
```

对应 route 和文件:

| 页面 / 状态 | Route | 主要文件 |
| --- | --- | --- |
| Home | `#/` | `scripts/pages/home.js` |
| Stented list | `#/category/stented` | `scripts/pages/category.js` |
| Hancock II detail | `#/valve/hancock-ii` | `scripts/pages/valve-detail.js` |
| Size 21 | `#/valve/hancock-ii/size/21` | `scripts/pages/size-detail.js` |
| THV Current | `#/valve/hancock-ii/size/21/thv/current` | `scripts/pages/thv-selector.js` |
| THV Archived | `#/valve/hancock-ii/size/21/thv/archived` | `scripts/pages/thv-selector.js` |
| Evolut plan | `#/valve/hancock-ii/size/21/thv/current/plan/evolut` | `scripts/pages/thv-plan.js` |
| Evolut placement | `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` | `scripts/pages/thv-placement.js` |
| Lotus placement | `#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement` | `scripts/pages/thv-placement.js` |
| Video overlay | 不切换 hash，由 placement 页面按钮触发 | `scripts/app.js`、`scripts/state.js`、`scripts/components.js` |

当前可达性结论:

- `#/ -> Stented` 起点已被 Day 4 代码和测试保护。
- Stented、Hancock II、Size 21、THV Current / Archived、Evolut placement、Lotus placement 都具备 route render 基础。
- Video overlay 是页面内交互状态，不是独立 route；验收时必须验证打开后的 overlay，而不能只看 `data-video-*`。
- Plan -> Placement 当前仍有风险: `thv-plan.js` 中 placement 链接是 `visually-hidden`，route 可达不等于用户可见可点。

## 5. 截图证据

本周已整理出的 native 证据链主要来自:

- `work/docs/capture-manifest.md`
- `work/docs/viv-aortic-native-reference.md`
- `examples/images/*`

主链路截图证据:

| Native 截图 / 证据 | 页面 / 状态 | 说明 |
| --- | --- | --- |
| `work/captures/raw/001-home-tab.png` | Home | 证明首页入口、底部 tab、工具入口、Quick Selector。 |
| `work/captures/raw/007-stented-axpress.png` | Stented list | 证明 Stented 搜索框和列表结构。 |
| `work/captures/raw/008-hancock-detail.png` | Hancock II detail | 证明详情页、size buttons、marker 区。 |
| `work/captures/raw/010-size21.png` | Size 21 | 证明尺寸参数区和 THV Current / Archived。 |
| `work/captures/raw/011-thv-current.png` | THV Current | 证明 6 个 current option tile。 |
| `work/captures/raw/012-evolut-plan.png` | Evolut Ideal Placement | 证明 native 从 Evolut 进入 ideal placement 状态。 |
| `work/captures/raw/013-video-overlay.png` | Video overlay | 证明视频是交互打开后的播放器态。 |
| `work/captures/raw/015-after-video-back.png` | Back from video | 证明视频返回链。 |
| `work/captures/raw/017-bookmarks.png` | Bookmarks | 证明书签空状态。 |
| `work/captures/raw/018-more.png` | More | 证明 More 深层入口列表。 |
| `work/captures/raw/019-identify.png` | Identify | 证明 Identify 二级入口。 |
| `work/captures/raw/020-quick-selector.png` | Quick Selector | 证明 Quick Selector 打开态。 |

额外 screenshot baseline:

- `examples/images/首页.png`
- `examples/images/stented页.png`
- `examples/images/hancock-II首页图.png`
- `examples/images/hancock-II-21尺寸图.png`
- `examples/images/hancock-II-21尺寸的THVCurrent.png`
- `examples/images/hancock-II-21尺寸的THVSelector.png`
- `examples/images/hancock-II-21尺寸-Evolut23的瓣膜.png`
- `examples/images/hancock-II-21尺寸-Myval20的瓣膜.png`
- `examples/images/hancock-II-21尺寸-LotusIdealPlacement的瓣膜.png`
- `examples/images/hancock-II-21尺寸-23的瓣膜的示例视频.png`

截图证据缺口:

- Day 4 实现只保护 `#/ -> Stented`，没有完成整条主链路截图级复刻。
- `THV Archived`、`Myval`、`Lotus` 虽有截图或数据，但仍需要逐页重验 native / Web 对照。
- Video overlay 已有 native 证据，但 Web 侧仍需系统化保存打开、关闭、返回三类截图。
- Web 本周浏览器检查记录了结构状态和 overflow 状态，但还需要把 Web 对照截图统一归档到项目证据目录。

## 6. 页面 Spec 对照

### 6.1 Home

已完成:

- Home 分类入口。
- 工具入口。
- 底部 tabs。
- Quick Selector 浮动入口。
- Day 4 新增 `data-home-main-flow`。
- Day 4 将 Stented 标记为唯一显式主链入口: `data-main-flow-entry="stented"`。

待补:

- Quick Selector 打开态仍未完全对齐 native。
- 需要将 Home 截图对照状态写入 page status tracker。

### 6.2 Stented list

已完成:

- `#/category/stented` 可渲染。
- 列表结构和搜索入口存在。
- 当前 demo valve 为 Hancock II / Aspire / Avalus。

待补:

- Native Stented full catalog 远多于 3 项，需要决定后续是保持 demo scope，还是恢复 full catalog。
- 下一个最小任务建议优先选 `#/category/stented` 页面。

### 6.3 Hancock II detail

已完成基础:

- 厂商、瓣叶材质、implant summary。
- Fluoroscopic Markers。
- 图片参考板。
- 5 个 size 入口。
- Similar Looking Valves。

待补:

- 需要截图级视觉对齐。
- 右上角 action 语义需要确认。

### 6.4 Size 21

已完成基础:

- schematic。
- Stent ID / Height / True ID。
- Non-Fracturable。
- THV Current / Archived。

待补:

- 需要移动端 / 桌面端视觉验收。
- 其他 size 页面仍未截图级验收。

### 6.5 THV Current / Archived

已完成基础:

- Current route 可渲染。
- Archived route 可渲染。
- Lotus 数据和 `hancock-lotus-board.png` 当前已出现在 `valves.js` 和测试断言中。

待补:

- Archived -> Lotus -> placement 的真实点击链需要复核。
- Myval / Lotus / Archived 不能只因“有数据、有 route”就判定高保真通过。

### 6.6 Plan / Placement / Video overlay

已完成基础:

- Evolut plan route 可渲染。
- Evolut placement route 可渲染。
- Lotus placement route 可渲染。
- Video panel 结构有测试保护。

待补:

- `thv-plan.js` 中 placement deep link 是隐藏链接，用户可见入口缺失。
- 需要确认 native 是否存在独立 plan 页，还是从 tile 直接进入 Ideal Placement。
- Video overlay 需要补打开、关闭、返回三步证据。

## 7. 代码改动

Day 1-3 主要产出文档和 spec，没有直接修改 `vie-aorta-master` 业务代码。

Day 4 发生了最小代码改动，范围限定为保护 `#/ -> Stented` 起点。

### 7.1 `scripts/pages/home.js`

改动:

- 给 Home 主分类区增加 `data-home-main-flow`。
- 给 Stented row 传入 `mainFlowEntry: "stented"`。

目的:

- 明确标记 `#/ -> Stented` 是主链路起点。

### 7.2 `scripts/components.js`

改动:

- `nativeRow()` 增加 `mainFlowEntry` 参数。
- 条件输出 `native-row--main-flow`。
- 条件输出 `data-main-flow-entry="stented"`。

目的:

- 为 Home 主链入口提供稳定 DOM contract。

### 7.3 `styles/app.css`

改动:

- 收紧 `--native-shell-width`。
- 给 native topbar、screen、bottom tabs 增加 `max-width` 约束。
- 增加 `.page--home .native-row--main-flow` 轻量样式。

目的:

- 降低 Home/native shell 在移动端出现横向滚动的风险。

### 7.4 `tests/navigation.test.mjs`

改动:

- 增加 Home 不进入 fallback 的断言。
- 增加 `data-home-main-flow` 断言。
- 增加 `href="#/category/stented"` 断言。
- 增加 `data-main-flow-entry="stented"` 断言。
- 断言 `data-main-flow-entry` 只出现一次。
- 断言 bottom tabs、Quick Selector、native shell 存在。

目的:

- 防止未来误删或误改 `#/ -> Stented` 主链起点。

## 8. 测试验证

计划验证命令:

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

环境情况:

- PowerShell 直接执行 `node` 时曾出现 `Access is denied`。
- 该问题判断为本地执行环境权限问题，不是测试断言失败，也不是 route 破坏。

当前重新验证结果:

```text
PASS routes.test.mjs
PASS navigation.test.mjs
routeCount: 331
uniqueRouteCount: 331
failedCount: 0
```

Day 4 浏览器补充检查结果:

```text
hasHome: true
hasStentedMainEntry: true
mainFlowEntryCount: 1
hasBottomTabs: true
hasQuickSelector: true
hasFallback: false
horizontalOverflow: false
```

测试结论:

- 当前 route render 和 navigation contract 通过。
- 当前 Home 主链入口具备自动化保护。
- 331 条 route 可渲染说明 Web 结构化覆盖较广。
- 但这些测试仍不能替代截图 fidelity 验收和真实点击 E2E。

## 9. 主要风险 / Gap

| 优先级 | Gap | 说明 |
| --- | --- | --- |
| P0 | Plan -> Placement 可见入口缺失 | 当前 plan 页 deep link 是隐藏链接，用户链路不完整。 |
| P0 | Route render 不等于真实点击链 | 331 routes 可渲染，但不代表用户能从入口一路点击到所有目标状态。 |
| P1 | Screenshot fidelity 未完成 | Day 4 只做入口保护，没有完成整条主链截图级复刻。 |
| P1 | Lotus 需要重验 | 旧文档记录 fallback，当前数据和测试已有 Lotus；需要用真实页面和截图重新闭环。 |
| P1 | Video overlay 证据不完整 | 需要打开、关闭、返回三步证据；不能只看按钮和 metadata。 |
| P1 | Quick Selector 形态未最终确认 | Native 更像选择器/overlay，当前 Web 更像快捷 modal。 |
| P1 | 一级入口深层缺口大 | Stentless / Sutureless / Rings / TAVR / Case / Identify / Fracture / More 深层仍未完整复刻。 |
| P1 | full catalog 未决 | Stented 当前只保留 3 demo valves，是否恢复 native full catalog 需决策。 |
| P2 | 文档状态未统一 | 旧 supervisor checklist 的 325 routes / Lotus fallback 结论已过期。 |
| P2 | Node 执行环境不稳定 | PowerShell 直跑 node 有权限问题，需固化可复用验证方式。 |

## 10. 完整 App Backlog

### 10.1 P0: 主链路闭环

- 修复 `Plan -> Placement` 可见入口。
- 明确 plan 与 placement 的产品关系。
- 完成 `Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay` 真实点击 E2E。
- 补 Video overlay 打开 / 关闭 / 返回验收。
- 将主链路 route、文件、数据、素材、截图证据写成矩阵。

### 10.2 P0: 验收文档

- 建立 `phase-1-route-matrix.md`。
- 建立 `page-status-tracker.md`。
- 建立 `terminology-dictionary.md`。
- 更新或标注旧 `supervisor-checklist.md` 中过期结论。

### 10.3 P1: 一级入口

- `Stented`: 决定 demo scope vs full catalog。
- `Stentless`: 补 list / detail / size / THV / placement 路线。
- `Sutureless`: 补 list / detail / size / THV / placement 路线。
- `Rings`: 补 Ring archive / size / THV / ideal placement。
- `TAVR`: 明确 TavrSelect / AVRContent / AVRSize / AVRInfo 前后关系。
- `Bookmarks`: 对齐 native 空状态和反跳逻辑。
- `Case of the Month`: 补 list / article。
- `Identify a Valve`: 补 Unknown Surgical Valve / Similar Looking Valves。
- `Valve Fracture`: 补 Fracturable / Non-Fracturable / Fluoroscopy Information。
- `More`: 补 Contact、Report a Problem、Instructions、About、Disclaimer、Download Preferences、Acknowledgements。
- `Quick Selector`: 对齐 native 打开态和 selector 行为。

### 10.4 P1: 素材和视频

- 建立 screenshot -> route -> page file -> data node -> asset 映射表。
- 重验 `LotusIdealPlacement` 与 `hancock-lotus-board.png` 的对应关系。
- 重验 Myval / Archived 分支素材。
- 为每个 video entry 记录 title、poster、summary、youtubeId、videoUrl、fallback 策略。
- 将 Web 验收截图统一归档到项目证据目录。

### 10.5 P1: 测试

- 增加浏览器级点击 E2E。
- 增加 video overlay E2E。
- 增加 Quick Selector 打开态测试。
- 增加 screenshot-backed route 明确断言。
- 增加 mobile / tablet / desktop 视口验收。
- 增加 focus / keyboard / dialog accessibility 验收。

### 10.6 P2: 工程环境

- 固化 Windows 环境下可用的 Node 验证方式。
- 明确 `D:\学习\vie-aorta-master` 为当前真实项目目录。
- 建立本地服务启动和端口占用处理说明。
- 补部署验证: 静态资源、hash route、控制台错误、刷新行为。

## 11. 下一步建议

建议下一轮不要直接扩大到完整 App，而是继续使用“最小任务”方式推进。

推荐下一个最小任务:

```text
复刻并保护 `#/category/stented` 列表页
```

理由:

- 它是 `#/ -> Stented` 之后的第二个主链节点。
- Day 4 已经保护了入口，下一步自然应该保护入口落点。
- 它影响 Hancock II 主链，也影响未来 full Stented catalog 策略。
- 范围足够小，可控、可测、可截图对照。

执行前需要写清:

- 要改哪些文件。
- 为什么改。
- 不改哪些内容。
- 影响哪些 route。
- 如何验证。
- 截图对照用哪张 native baseline。

推荐验证:

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

推荐人工验收:

```text
打开 #/
点击 Stented
确认进入 #/category/stented
确认不进入 fallback
确认搜索框存在
确认 Hancock II / Aspire / Avalus 列表存在
确认 bottom tabs 和 native shell 无横向 overflow
```

## 12. 当前可对外声明

可以声明:

- 第二周已完成 ViV Aortic 项目结构、主链路、截图证据、复刻缺口和执行计划梳理。
- 第二周已完成 `#/ -> Stented` 主链起点的最小代码保护。
- 当前 `routes.test.mjs` 和 `navigation.test.mjs` 通过。
- 当前 331 条 route 可渲染，失败 0。
- 已形成完整 App 复刻 backlog 的基础。

不应声明:

- 不应声明完整 App 已复刻完成。
- 不应声明所有页面都已高保真复刻。
- 不应声明 Lotus / Myval / Archived / Video overlay 已全部完成截图级闭环。
- 不应声明 Quick Selector 已与 native 完全一致。
- 不应把自动化测试通过等同于视觉验收通过。
