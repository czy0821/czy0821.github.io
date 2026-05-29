# full-app-replica-roadmap.md

## 1. 已完成

- 已理解项目结构：当前项目是静态 Web SPA，主结构为 `index.html -> scripts/app.js -> scripts/router.js -> scripts/pages/* -> scripts/components.js -> scripts/data/* -> styles/app.css`。
- 已整理原生 App 页面结构：`page-map-agent.md` 已梳理 Home、底部 Tab、Stented / Stentless / Sutureless / Rings / TAVR、工具入口和 Quick Selector。
- 已整理原生截图证据：`capture-manifest.md` 已记录 Home、Stented list、Hancock II detail、Size 21、THV Current、Evolut placement、Video overlay、Bookmarks、More、Identify、Quick Selector。
- 已整理监督缺口：`supervisor-checklist.md` 已指出不能把 Web route 数量等同于原生 App 全量复刻。
- 已整理 Week2 review：`week2-viv-aortic-review.md` 已回答项目结构、证据、spec、代码改动、验证、主链状态、差距和下周计划。
- 已整理 Week2 bug / gap：`week2-bug-gap-list.md` 已用表格列出 Bug / Gap、证据、影响范围、优先级和下一步。
- 已完成主样例数据基础：当前 Stented demo 包含 `hancock-ii`、`aspire`、`avalus`。
- 已补 Lotus 数据层：`Hancock II / Size 21 / Archived / Lotus` 当前已存在，media 指向 `hancock-lotus-board.png`。
- 已建立当前 route 基线：当前数据可计算出 `331 routes / 331 unique routes`。
- 已有基础测试保护：`tests/navigation.test.mjs` 和 `tests/routes.test.mjs` 可覆盖 Home 主入口、Stented shell、THV selector、Lotus、media schema、video panel contract、fallback 等。

## 2. 主链路待补

- 补真实浏览器 E2E：从 `#/` 开始点击进入 Stented、Hancock II、Size 21、THV Current、Evolut plan、placement、video overlay，再返回 Home。
- 补主链 Web 截图：为 Home、Stented list、Hancock II detail、Size 21、THV Current、Evolut placement、Video overlay 保存 Web 对照图。
- 补 Plan -> Placement 可见入口验收：确保 plan 页面不是只靠 deep link，而是用户能看到并点击 Ideal Placement。
- 补 Video overlay 可视证据：当前有原生 `013-video-overlay.png` 和 Web contract，但缺 Web 打开后的截图或录屏。
- 补 Archived 分支闭环：从 Size 21 进入 THV Archived，再进入具体 archived device plan / placement。
- 补 Myval 分支闭环：从 THV Current 点击 Myval plan / placement，并保存截图和测试证据。
- 补 Lotus 分支闭环：数据已补，但仍需从 Archived 点击到 Lotus placement，证明不是 fallback，并保存 Web / 原生对照。
- 补返回路径：验证 video overlay -> placement -> Home 的返回链路和底部 Tab 状态。

## 3. 一级入口待补

- Home：补完整入口状态表，标注每个入口是 complete / skeleton / pending / bundle-only。
- Stented：扩展真实 Stented catalog，不只保留 `hancock-ii / aspire / avalus` 三个 demo。
- Stentless：补列表页、详情页、size 页、THV select、ideal placement 骨架。
- Sutureless：补列表页、详情页、size 页、THV select、ideal placement 骨架。
- Rings：补 sewing ring archive、ring detail、ring size、Ring THV Select、Ring Ideal Placement。
- TAVR：确认 `TavrSelect / AVRContent / AVRSize / AVRInfo` 的真实层级，再补入口和代表链路。
- Bookmarks：补收藏态、删除、列表、跳回源页面。
- Case of the Month：补 list / article 骨架，并确认是否有历史 case。
- Identify a Valve：补 `Unknown Surgical Valve`、`Similar Looking Valves` 两个二级入口流程。
- Valve Fracture：确认它与 Identify 的关系，补 Fracturable / Non-Fracturable 信息页。
- More：补 12 个条目的子页，包括 Contact、Report Problem、Instructions、About、Disclaimer、Download Preferences、Acknowledgements 等。
- Quick Selector：补类别切换、valve / size 选择、THV Current / Archived 跳转。

## 4. 深层页面待补

- Stented 深层：把 Hancock II 的 detail、size、THV、plan、placement、guidance 模板扩展到更多真实 Stented valve。
- Stented Archived：补 archived selector 的真实截图、option 顺序、空态和 unavailable 状态。
- Plan 页面族：统一 Evolut / Myval / Lotus 的 recommendation、notes、video launcher、placement CTA。
- Placement 页面族：补 placement strip、说明文案、video guidance、poster fallback。
- Guidance 页面：明确 guidance 与 plan / placement / video 的关系，避免重复或错链。
- Stentless 深层：完成一条 `Archive -> Content -> Size -> THVSelect -> IdealPlacement` 代表链路。
- Sutureless 深层：完成一条 `Archive -> Content -> Size -> THVSelect -> IdealPlacement` 代表链路。
- Rings 深层：完成一条 `SewingRingArchive -> Content -> RingSize -> RingTHVSelect -> RingIdealPlacement` 代表链路。
- TAVR 深层：先补真实 page map，再实现 `TavrSelect / AVRContent / AVRSize / AVRInfo`。
- Identify 深层：补 Fluoroscopic Classification、Identification、Markers、Visible 等页面。
- More 深层：补 Contact、Report Problem、Instructions、About、Disclaimer、Acknowledgements 等。
- Case 深层：补 COTM list、article、图片和文字来源。

## 5. 素材和视频待补

- 建立 screenshot-to-route 映射表：记录 screenshot、原生页面、Web route、入口路径、data node、asset、测试覆盖、缺口。
- 建立 asset provenance 表：记录 asset path、原生来源、Web 使用页面、role、exact / fallback / unknown。
- 补 Web 截图归档：主链每个页面都要有 Web 截图，并和 `work/captures/raw/` 原生截图成对。
- 补 Lotus 素材核验：确认 `hancock-lotus-board.png` 与原生 LotusIdealPlacement 截图的对应关系。
- 补 Myval / Archived 素材核验：确认截图、route、data、asset 是否一致。
- 补 video metadata 清单：记录 title、poster、videoUrl / youtubeId、是否可播放、fallback 文案。
- 补 Video overlay 证据：记录触发 route、触发控件、打开状态、关闭行为、截图或录屏。
- 补 Quick Selector 素材归属：确认 `quick.png`、`qs.png` 等资产在 Web 中的使用状态。
- 补医疗术语表：锁定 THV、Ideal Placement、Fracturable、Fluoroscopic、Use With Caution 等术语。

## 6. 测试待补

- 主链 E2E：从首页一路点击到 video overlay，不手输 URL。
- Archived / Lotus E2E：从 Size 21 -> THV Archived -> Lotus plan -> Lotus placement。
- Myval E2E：从 THV Current -> Myval plan -> Myval placement。
- Video overlay E2E：点击打开、截图验证、关闭返回。
- Quick Selector E2E：打开、类别切换、size 选择、THV Current / Archived 跳转、关闭。
- 一级入口测试：Home 上所有一级入口都可点击，且没有无说明 fallback。
- More 子页测试：12 个条目可达或明确 pending。
- Bookmarks 测试：空态、收藏态、删除、跳回源页面。
- Identify / Fracture 测试：入口关系、二级页、返回行为。
- Route matrix 回归：稳定 `node scripts/verify-routes.mjs`，输出当前 `331 routes` 基线。
- Asset presence 测试：所有页面引用的本地 asset 都存在。
- 多视口截图测试：覆盖 390px、768px、1014px、desktop。
- 视觉回归：检查横向滚动、文字遮挡、底部 Tab、顶部 title bar、Quick Selector。

## 7. 下周优先级

1. P0：补主链浏览器 E2E，从 `#/` 点击到 video overlay，再返回 Home。
2. P0：建立 screenshot-to-route-to-data-to-asset-to-test 映射表。
3. P0：保存主链 Web 截图，并和原生 `work/captures/raw/` 截图成对归档。
4. P0：补 Lotus / Myval / Archived 的截图、route、asset、E2E 证据闭环。
5. P0：补 Web video overlay 截图或录屏证据。
6. P1：建立原生全量页面状态表，标注 `已截图 / 已实现 / 已验证 / 缺失 / fallback / bundle-only`。
7. P1：补一级入口骨架，确保 Stentless、Sutureless、Rings、TAVR、Case、Fracture、More、Bookmarks、Quick Selector 都可达且状态明确。
8. P1：每类非 Stented 业务线先完成一条代表性深层链路。
9. P1：修复并稳定 `node scripts/verify-routes.mjs`，避免 `325 routes` 和 `331 routes` 快照混淆。
10. P2：建立素材 provenance 和术语表，为后续全量扩展做准备。
