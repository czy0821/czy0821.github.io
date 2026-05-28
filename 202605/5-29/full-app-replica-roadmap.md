# ViV Aortic Full App Replica Roadmap

生成日期: 2026-05-28

## 依据

本路线图基于以下证据整理:

- `work/docs/page-map-agent.md`: 原生 App 页面结构、bundle token、疑似路由关系。
- `work/docs/capture-manifest.md`: 已采集 native 截图链与工具入口截图。
- `work/docs/supervisor-checklist.md`: 监督验收清单、风险项、旧缺口记录。
- 本周验收成果: Web 端主链路截图、浏览器验证、`routes.test.mjs` / `navigation.test.mjs` 通过、331 条路由渲染检查通过。

路线图的目标不是只完成 Hancock II demo，而是把当前项目推进到“完整 App 复刻”可验收状态。每个条目都需要最终落到页面状态、证据来源、实现状态、测试结果四类证据。

## 已完成

| 范围 | 当前状态 | 证据 / 说明 |
| --- | --- | --- |
| 静态 Web App 基础 | 已完成 | 项目为静态 ES module + hash router SPA，入口为 `index.html`，渲染入口为 `scripts/app.js`，路由分发在 `scripts/router.js`。 |
| 原生页面结构初步梳理 | 已完成 | `page-map-agent.md` 已整理 Home、5 个底部 Tab、工具入口、Stented 主链路，以及 Stentless / Sutureless / Rings / TAVR 的平行深链推断。 |
| Native 截图主链采集 | 已完成 | `capture-manifest.md` 已列出 Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut Ideal Placement -> Video overlay 的采集链。 |
| 原生参考总览 | 已完成 | `viv-aortic-native-reference.md` 已合并真实采集、bundle token、素材盘点、页面特征。 |
| Home 复刻 | 已完成 | Web 已呈现原生风格首页: 5 个类别入口、5 个工具入口、底部 6 tab、Quick Selector 浮动入口。 |
| Stented 列表复刻 | 已完成 | Web 已呈现搜索框 + Hancock II / Aspire / Avalus 三个 demo valve 列表。 |
| Hancock II 详情页 | 已完成 | 已有厂商、瓣叶材料、implant summary、Fluoroscopic Markers、2x2 参考图、5 个 size 入口、Similar Looking Valves。 |
| Size 21 页 | 已完成 | 已有 schematic、Stent ID / Height / True ID、Non-Fracturable、THV CURRENT / THV ARCHIVED。 |
| THV Current / Archived | 已完成 | Current 与 Archived 均可渲染 6 个 option tile；Archived 已包含 Lotus。 |
| Evolut placement | 已完成 | `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` 可渲染并可打开 video overlay。 |
| Lotus placement | 本周修正为已完成 | 旧监督清单记录 Lotus 只能 fallback；本周验收确认 `#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement` 已可渲染 Lotus 页面。 |
| Video overlay | 已完成基础态 | 从 Evolut placement 的 `video-row` 可打开全屏 video panel，浏览器截图已保存。 |
| 三个 Stented demo schema | 已完成 | `hancock-ii` / `aspire` / `avalus` 均有 sizes、THV selector、plan、placement、guidance schema。 |
| 自动化 route / navigation 测试 | 已完成基础覆盖 | `routes.test.mjs` 与 `navigation.test.mjs` 本周通过。 |
| 路由渲染覆盖 | 本周修正为 331 条 | 旧监督清单记录 325 条；本周枚举验证 331 条 unique routes，失败 0。 |
| 本周截图证据 | 已完成 | 已保存 Home、Stented list、Hancock detail、Size 21、THV Current、THV Archived、Evolut plan、Evolut placement、Lotus placement、Video overlay 截图。 |

## 主链路待补

主链路定义:

```text
Home
-> Stented
-> Hancock II
-> Size 21
-> THV Current / Archived
-> Option plan
-> Ideal Placement
-> Video overlay
```

| 优先级 | 待补项 | 当前问题 | 完成标准 |
| --- | --- | --- | --- |
| P0 | Plan -> Ideal Placement 可见入口 | 当前 plan 页到 placement 的链接是 `visually-hidden`，只有 1x1 可访问隐藏链接；用户视觉上找不到入口。 | Evolut / Myval / Lotus 等 plan 页提供可见 CTA，或按 native 行为直接从 selector tile 进入 Ideal Placement。 |
| P0 | 明确 plan 与 placement 的产品关系 | 当前 plan 页标题也叫 `Ideal Placement`，与 placement 页语义重叠。 | 路由、标题、截图目标统一: 如果 native 中 tile 直接进入 Ideal Placement，则去掉中间 plan 误导；如果保留 plan，则标题与 CTA 明确区分。 |
| P1 | Topbar 标题移动端溢出 | `.topbar h1` 全局样式覆盖 native title，导致 `THV Selector: Current` 等标题变成大字多行。 | native shell title 有专属样式，移动端标题不挤压内容区。 |
| P1 | Archived 链路点击验收 | Archived selector 可见 Lotus tile，Lotus placement 可直接渲染，但需要补点击链证据。 | 从 Size 21 -> THV Archived -> Lotus -> Lotus placement 可用真实点击路径闭合。 |
| P1 | Video overlay 回退链 | 已能打开 overlay，但需要确认 close / back 后回到 placement 页面。 | 打开视频、关闭视频、返回上一页均有截图或测试证据。 |
| P1 | Main flow route matrix | 当前只有测试脚本，没有 PM 可读的主链路验收矩阵。 | 建立 `work/docs/phase-1-route-matrix.md`，每一行包含 route、页面名、入口、证据、状态、测试方式。 |
| P2 | 桌面 shell 验收 | 当前本周截图主要覆盖 390x844 移动视口。 | 390x844、768x1024、桌面宽度均截图通过；底部 nav、quick button、内容区不重叠。 |

## 一级入口待补

一级入口包括 5 个底部 category tab、5 个首页 utility entry、Quick Selector。

| 优先级 | 入口 | 当前状态 | 待补目标 |
| --- | --- | --- | --- |
| P0 | `Home` | 已完成基础复刻 | 补 page status tracker 状态，确认截图对比通过。 |
| P0 | `Stented` | 已完成 3 demo valve 列表 | 明确当前是 demo scope，后续是否恢复 full Stented catalog 需单独决策。 |
| P1 | `Stentless` | 有 category list / native shell 基础态 | 补 native 截图证据、真实列表项、详情/size/THV 深链路线。 |
| P1 | `Sutureless` | 有 category list / native shell 基础态 | 补 native 截图证据、真实列表项、详情/size/THV 深链路线。 |
| P1 | `Rings` | 有 category list / native shell 基础态 | 补 SewingRingArchive、RingSize、RingTHVSelect、RingIdealPlacement 对应状态。 |
| P1 | `TAVR` | 有 category list / native shell 基础态 | 补 TavrSelect / AVRContent / AVRSize / AVRInfo 的准确前后关系。 |
| P1 | `Bookmarks` | 有 utility 页面基础态 | 对齐 native 空状态 `Currently no bookmarks`，补书签来源和反跳回业务页逻辑。 |
| P1 | `Case of the Month` | 有 utility 占位 | 补 COTM list / article 结构、截图、内容策略。 |
| P1 | `Identify a Valve` | 有 utility 占位 | 补 Unknown Surgical Valve / Similar Looking Valves 二级入口。 |
| P1 | `Valve Fracture` | 有 utility 占位 | 补 Fracturable / Non-Fracturable / Fluoroscopy Information 结构。 |
| P1 | `More` | 有 utility 占位 | 补 native More 的 11-12 个条目: Contact、Report a Problem、Instructions、About、Disclaimer、Download Preferences、Acknowledgements 等。 |
| P0 | `Quick Selector` | 有浮动按钮与 modal 基础态 | 对齐 native `Quick Selector` 打开后的 STENTLESS VALVE 选择器、size table、THV Current / Archived 入口。 |
| P2 | Download Preferences | bundle 已发现 token，Web 未形成验收项 | 确认是否为首启/Quick Selector 关联弹窗；决定复刻或标为 deferred。 |

## 深层页面待补

深层页面按 page-map 的平行链路整理。原则: 不能把“顶层入口可见”当作“深层页面已复刻”。

| 优先级 | 页面族 | 待补范围 | 完成标准 |
| --- | --- | --- | --- |
| P0 | Stented / Hancock II 主链 | plan/placement 可见入口、video close/back、截图矩阵 | 主链从 Home 到 Video overlay 点击闭合，截图与 route matrix 都通过。 |
| P1 | Stented full catalog | Aspire / Avalus 已有 demo schema，但 native Stented 列表远多于 3 项 | 决定 full catalog 范围；若恢复全量，则每个 valve 至少有 detail fallback、size/THV 状态说明。 |
| P1 | Stentless chain | Archive -> Content -> Size -> THVSelect -> IdealPlacement -> tricks | 每层有 native 证据、route、数据节点、截图或明确 deferred。 |
| P1 | Sutureless chain | Archive -> Content -> Size -> THVSelect -> IdealPlacement | 同上。 |
| P1 | Rings chain | SewingRingArchive -> DimensionsewingRingContent -> RingSize -> RingTHVSelect -> RingIdealPlacement | 同上。 |
| P1 | TAVR chain | TavrSelect / AVRContent -> AVRSize -> AVRInfo | 先确认准确 IA，再实现或标记 deferred。 |
| P1 | More stack | ContactModal、ContactPage、DisclaimerPage、Acknowledgements、Introduction/About、Download Preferences | 至少完成一级可见条目与核心信息页；未实现 modal 需明确 gap。 |
| P1 | Identify stack | FluoMain、Fluoroscopic Classification、Identification、Markers、Visible 判断 | 建立页面树和截图证据，避免只做单页占位。 |
| P1 | Case of the Month | COTM list / article | 建立列表、文章页、空状态或样例内容。 |
| P2 | Bookmarks deep return | bookmarked page -> 原业务页反跳 | 验证收藏、取消收藏、空状态、从书签进入原页面。 |
| P2 | Report a Problem | bundle 中有提交成功和问题描述文案 | 明确入口位置，复刻表单或标为 deferred。 |

## 素材和视频待补

| 优先级 | 待补项 | 当前问题 | 完成标准 |
| --- | --- | --- | --- |
| P0 | 截图 -> route -> data -> asset 映射表 | 目前证据散在 page map、capture manifest、data 文件和截图中。 | 建立一张矩阵，至少覆盖 10 个 screenshot-backed target。 |
| P0 | 本周 Web 截图归档 | 本周截图在 Codex 工作目录，尚未纳入项目证据目录。 | 将验收截图复制或重新采集到项目约定目录，如 `work/captures/web/week2/`，并写入 manifest。 |
| P1 | Native capture manifest 更新 | 旧 supervisor checklist 中有过期结论: 325 routes、Lotus fallback。 | 更新监督文档或在 roadmap 中标记 superseded，避免后续误用旧结论。 |
| P1 | Video metadata | 当前多为 poster fallback 或本地示意，真实 URL / YouTube ID 不完整。 | 每个 video entry 记录 title、poster、source type、youtubeId/videoUrl、fallback 策略。 |
| P1 | Video overlay 真实截图 | capture manifest 已有 native video overlay；Web 本周有 overlay 截图，但未入项目 manifest。 | 将 Web overlay 截图纳入证据表，并测试 close/back。 |
| P1 | Quick Selector 截图复核 | Native 中 Quick Selector 打开到 STENTLESS VALVE；Web 当前 quick modal 是简化快捷入口。 | 决定是复刻原生 selector 还是保留 Web shortcut，并把差异写入 gap list。 |
| P1 | Non-stented assets | Stentless / Sutureless / Rings / TAVR 深层素材未完成逐页映射。 | 每个页面族至少建立核心图片、icon、fallback asset 的 inventory。 |
| P2 | 术语与中文文案 | 项目源码存在中英文混用和部分编码输出问题；PM 文档要求中文术语字典。 | 建立 `terminology-dictionary.md`，并将页面 title、button、empty state 对齐。 |
| P2 | Asset provenance | 需要区分 native 提取、examples 截图、Web 生成/占位素材。 | 每个素材有来源、用途、目标页面、是否可替换真实素材。 |

## 测试待补

| 优先级 | 待补项 | 当前问题 | 完成标准 |
| --- | --- | --- | --- |
| P0 | `phase-1-route-matrix.md` | 设计文档要求存在，但当前文件缺失。 | Mandatory route 全部列出，状态包含 pass / partial / blocked / deferred。 |
| P0 | `page-status-tracker.md` | 设计文档要求存在，但当前文件缺失。 | 每个一级入口和主链页面有 owner、status、evidence、notes。 |
| P0 | `terminology-dictionary.md` | 设计文档要求存在，但当前文件缺失。 | 统一 THV、Current、Archived、Guidance、Placement、True ID、Stent ID 等术语。 |
| P0 | 可见点击链 E2E | 当前自动化主要是 render-string 级别测试；Plan -> Placement 隐藏入口未被测试暴露。 | 增加浏览器或 DOM 级测试，断言主链关键入口可见且可点击。 |
| P0 | Video overlay E2E | 目前测试有 shell/metadata，缺少完整打开、关闭、返回断言。 | 测试 `button.video-row` 打开 overlay，close 后回到 placement。 |
| P1 | Visual screenshot checklist | 本周有截图，但还没有项目内 page-by-page 对照表。 | 每个 screenshot-backed route 有 Web screenshot、native screenshot、结果、gap。 |
| P1 | Mobile / tablet / desktop | 本周重点为 390x844。 | 至少验证 390x844、768x1024、desktop shell；记录截图和布局结论。 |
| P1 | Accessibility checks | 已有 skip link、focus trap 基础，但未形成验收清单。 | 键盘导航、focus visible、dialog aria、button label、色彩对比逐项通过。 |
| P1 | Utility route coverage | 当前 route test 会验证 utility 基础页，但不验证真实 native parity。 | 对 Bookmarks、Case、Identify、Fracture、More 添加 route marker 和状态验收。 |
| P1 | Negative route / fallback matrix | 现有测试覆盖很多异常路由，但缺少 PM 可读矩阵。 | 将 invalid route、unsupported valve、empty archived 等 fallback 规则写入测试说明。 |
| P2 | CI / local command reliability | 当前 Windows PowerShell 中 `node.exe` 被拒绝执行，本周通过 Codex 内置 Node 验证。 | 确认可复用的本地测试运行方式，或记录 Windows 环境 workaround。 |
| P2 | Deployment verification | 当前 review 未覆盖线上部署。 | 验证静态部署、刷新 hash route、资源路径、控制台错误、404 行为。 |

## 建议里程碑

| 里程碑 | 目标 | 出口标准 |
| --- | --- | --- |
| M1 主链闭环修复 | 修复 Plan -> Placement 可见入口、topbar 标题、video close/back | Home -> Video overlay 可点击闭合，主链截图通过。 |
| M2 验收文档补齐 | 建立 route matrix、page status tracker、terminology dictionary、截图映射表 | PM 可以按表验收，而不是靠口头说明。 |
| M3 一级入口产品化 | 补齐 5 category entry、5 utility entry、Quick Selector 的状态和截图证据 | 所有一级入口至少达到 approved 或明确 deferred。 |
| M4 深层页面扩展 | Stentless / Sutureless / Rings / TAVR / More / Identify / Case 深层路线逐步落地 | 每个页面族有 page map、数据节点、route、截图或 deferred 理由。 |
| M5 素材与视频归档 | 补齐素材来源、视频 metadata、native/web 对照截图 | 所有关键媒体都有可追踪来源和 fallback 策略。 |
| M6 完整 App 验收 | 路由、视觉、交互、素材、文案、可访问性、部署综合验收 | Full app backlog 中 P0/P1 关闭，P2/P3 明确遗留。 |

## 当前验收口径

当前项目可以声明:

- 已完成 screenshot-backed Stented / Hancock II demo 主链的高可用基础版。
- 已完成 3 个 Stented demo valves 的结构化深链 schema。
- 已完成 331 条 Web route 的渲染级验证。
- 已具备 video overlay、image preview、quick modal 等核心 shell 能力。

当前项目不应声明:

- 不应声明完整 App 已复刻完成。
- 不应声明 native 全量页面均已采集。
- 不应声明所有一级入口和深层页面都已完成 native parity。
- 不应把 render-string 路由可达等同于真实用户点击链可达。
