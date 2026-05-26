# ViV Aortic 完整 App 复刻缺口清单

整理原则：

- 只基于当前项目资料、截图清单、页面图谱、监督清单和资产盘点。
- 每个缺口都写明证据来源。
- “风险类型”指该缺口主要影响什么：页面覆盖、证据链、路由映射、数据素材、交互状态或验收判断。

## 高风险

| 优先级 | 缺口 | 证据来源 | 影响页面 | 风险类型 | 建议处理方式 |
| --- | --- | --- | --- | --- | --- |
| 高 | 原生 App 全量页面清单尚未建立，不能证明所有页面都已获取、实现、验证 | `work/docs/supervisor-checklist.md:40` 要求建立原生页面清单并标注 `已截图 / 已实现 / 已验证 / 未获取`；`work/docs/supervisor-checklist.md:163-168` 明确说不能宣称“所有页面已获取”，且“原生 app 全量页面清单尚未建立” | 全 App | 页面覆盖 / 验收判断 | 建立总表：bundle screen 名称、原生截图、Web 路由、数据节点、素材、状态。每页标注 `已截图 / 已实现 / 已验证 / 缺失 / fallback` |
| 高 | 非 screenshot 范围页面仍有明显采集盲区 | `work/docs/supervisor-checklist.md:158-159` 要求至少把 `stentless`、`sutureless`、`rings`、`AVR/TAVR`、`bookmarks`、`identify`、`case of the month`、`more` 标成“已覆盖”或“待采集”；`work/docs/supervisor-checklist.md:168` 明确说非 screenshot 范围页面仍有明显采集盲区 | Stentless、Sutureless、Rings、TAVR、Case of the Month、Valve Fracture、More 深层页 | 页面覆盖 / 采集盲区 | 按模块补采集清单，逐页记录截图、入口路径、Web 路由和状态，不能只靠 bundle token 判断完成 |
| 高 | Lotus 链路存在证据冲突，不能直接判定已闭环 | `work/docs/supervisor-checklist.md:89-92` 说 Lotus 目标链未闭合并落到 `未找到理想位置` fallback；`work/docs/supervisor-checklist.md:114` 将 Lotus 目标页失真列为高风险；但 `scripts/data/valves.js:903-906` 当前已有 Lotus archived 选项，`scripts/data/valves.js:991-1004` 当前已有 Lotus media/video override | `#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement`；`hancock-II-21尺寸-LotusIdealPlacement的瓣膜.png` | 路由映射 / 数据素材 / 证据冲突 | 重新跑 Lotus 路由验证，截图确认当前是否仍 fallback；若已修复，更新监督清单和资产清单；若未修复，补数据或修正截图映射 |
| 高 | 视频状态不能只靠按钮或 metadata 验收 | `work/docs/supervisor-checklist.md:44` 要求视频类页面必须有打开视频状态证据；`work/docs/supervisor-checklist.md:109` 说当前只有按钮 metadata 和测试 overlay shell 的间接证据；`work/docs/capture-manifest.md:21` 记录已采到 `013-video-overlay.png` | Evolut Placement Video、所有 Video Guidance / overlay 状态 | 交互状态 / 验收判断 | 每个视频入口记录触发前页面、触发控件、打开后的 overlay 截图或录屏、poster、video metadata；不要只验按钮存在 |

## 中风险

| 优先级 | 缺口 | 证据来源 | 影响页面 | 风险类型 | 建议处理方式 |
| --- | --- | --- | --- | --- | --- |
| 中 | 缺少完整的“截图文件 -> 路由 -> 数据节点 -> 资产文件 -> 校验结果”逐页映射表 | `work/docs/supervisor-checklist.md:41-43` 要求截图唯一映射到页面，并能指向图片资产、参数来源和视频 metadata；`work/docs/supervisor-checklist.md:106` 明确说目前缺少逐页映射表；`work/docs/supervisor-checklist.md:145-147` 建议至少覆盖 10 个 screenshot target | examples/images 下 10 张截图、Hancock II 主链、旁支截图 | 证据链 / 数据素材 | 维护一张对账表：截图文件、页面状态、入口控件、Web 路由、数据节点、素材文件、验证结果、风险 |
| 中 | `work/captures` 采集产物不足以支撑“全量逐页核对已完成” | `work/docs/capture-manifest.md:15-27` 只列出本轮真实采集的主链和部分工具页；`work/docs/capture-manifest.md:68-75` 已验证范围仅包括 Home、Stented list、Hancock II detail、Size 21、THV Current、视频打开态；`work/docs/supervisor-checklist.md:116` 将采集产物极少列为中风险 | 主链外页面、非 Stented 深层页面、工具深层页 | 采集证据 / 验收判断 | 继续补 `work/captures/raw`，每个目标页面保存裁切图和必要的 fullscreen 图 |
| 中 | Archived THV selector、Myval 分支、Lotus 目标图属于旧截图或未复采范围 | `work/docs/capture-manifest.md:77-81` 明确说 Archived THV selector、Myval 分支、`LotusIdealPlacement` 目标图已有旧截图，但本轮未逐页复采；`work/assets-inventory/asset-map-agent.md:142` 说明 THVSelector 命名与 current/旧 selector 对应不稳定 | THV Archived、Myval plan/placement、Lotus placement | 截图基线 / 路由映射 | 逐页复采 Archived、Myval、Lotus；重新确认截图对应的是 current、archived、plan 还是 placement |
| 中 | TAVR 栈内部页面顺序仍不完全确定 | `work/docs/page-map-agent.md:91-95` 列出 `AVRStackScreen / AVRContentScreen / AVRSizeScreen / AVRInfoScreen / TavrSelect`；`work/docs/page-map-agent.md:311-313` 明确说 TAVR 内部顺序仍不完全确定 | TAVR、AVRContent、AVRSize、AVRInfo、TavrSelect | 页面层级 / 路由关系 | 从原生 App 逐步采集 TAVR 入口、列表/选择页、详情页、尺寸页、信息页，确认是否串联或并列 |
| 中 | Identify a Valve 与 Valve Fracture 的关系未完全确定 | `work/docs/page-map-agent.md:101-104` 说明两个首页入口都有截图/bundle 证据；`work/docs/page-map-agent.md:314-316` 明确说二者关系未完全确定；`work/docs/page-map-agent.md:121-126` 多个 Identify/Fracture 子页只见 bundle 文案，未见稳定独立 screen token | Identify a Valve、Valve Fracture、Fluoroscopic Classification、Fluoroscopic Identification、Fracturable/Non-Fracturable | 页面层级 / 工具流程 | 分别从 Home 点击 Identify 和 Valve Fracture，采集入口页和子页，确认是两个独立 stack 还是同一 stack 分支 |
| 中 | More 栈深层页面未完整采集验证 | `work/docs/page-map-agent.md:107-115` 列出 Contact、Disclaimer、Acknowledgements、Introduction 等 More 栈页面；`work/docs/page-map-agent.md:321` 说明 `AcknowledgementModel` 是否为真正 page 名尚不稳定；`work/docs/capture-manifest.md:25` 只证明 More 列表已见 12 个条目 | More、Contact、Disclaimer、Acknowledgements、Introduction、Download Preferences、Manufacturer Contact、Report a Problem | 工具页覆盖 / 页面层级 | 从 More 列表逐项进入，保存每个深层页截图和返回路径，确认 modal/page 命名和 Web 路由 |
| 中 | Quick Selector 承载形态未完全确认 | `work/docs/capture-manifest.md:27` 记录 Quick Selector 截图；`work/docs/page-map-agent.md:37-41` 说明 bundle 含 `QuickSelectScreen`；`work/docs/page-map-agent.md:320` 明确说 Quick Selector 是 modal、独立 stack 还是 Home 覆盖层仍未确认 | Quick Selector、DownloadPrefModal、THV CURRENT / THV ARCHIVED 快捷入口 | 交互状态 / 页面层级 | 记录打开、选择、返回、关闭流程，确认是否独立路由、modal 或 overlay，并补状态机说明 |

## 低风险

| 优先级 | 缺口 | 证据来源 | 影响页面 | 风险类型 | 建议处理方式 |
| --- | --- | --- | --- | --- | --- |
| 低 | Stented 的 Size 页面缺少稳定 bundle screen 名 | `work/docs/page-map-agent.md:49` 写到尺寸页未直接看到独立 token，但截图已确认存在；`work/docs/page-map-agent.md:310` 再次说明 Stented 的“尺寸页”在 bundle 中没有稳定 screen 名；`work/docs/capture-manifest.md:18` 记录 `010-size21.png` 是 Size: 21 | Hancock II Size 21 及其他 Stented size 页面 | 命名对齐 / 页面证据 | 在页面总表里标注 Size 页面“由截图和路由确认，不由稳定 bundle token 确认” |
| 低 | Bookmarks 是否为单独 Tab 未见证据 | `work/docs/page-map-agent.md:101` 将 Bookmarks 归为首页工具入口；`work/docs/page-map-agent.md:317-319` 说明 Bookmarks 是否为单独 Tab 未见证据，更像首页工具入口；`work/docs/capture-manifest.md:24` 记录 Bookmarks 空状态已采集 | Bookmarks、navFromBookmarkStackScreen | 导航归类 | 页面地图中将 Bookmarks 先归为 Home 工具入口，后续再验证是否存在独立 Tab 或反跳栈 |
| 低 | 详情页右上角第二个动作按钮用途未确认 | `work/docs/page-map-agent.md:322-324` 写到详情页右上角除收藏外第二个按钮用途未确认；bundle 中出现 `Report a Problem successfully submitted`、`Provide your email and a description of the discrepancy` | Hancock II 详情页、其他 Valve detail 页面 | 交互细节 / 反馈入口 | 点击该按钮并采集弹窗或目标页，确认是否为 Report a Problem / feedback |
| 低 | 原生视频链接、真实视频文件、多视频标题表尚未恢复 | `work/assets-inventory/asset-map-agent.md:153` 说明未发现实际视频文件；`work/assets-inventory/asset-map-agent.md:154` 说明当前 Web 数据全部为空外链；`work/assets-inventory/asset-map-agent.md:155` 说明 bundle 有 `video1Title` 到 `video5Title`、`videoLink` 等字段名但未恢复成对象；`work/docs/viv-aortic-native-reference.md:278-280` 说明尚未恢复视频 URL / YouTube ID / 原生多视频标题表 | Video Guidance、Evolut Placement Video、其他设备视频入口 | 数据素材 / 媒体还原 | 继续解析 bundle 或原生资源，补视频 URL、标题表、poster、真实文件；若无法恢复，明确标为缺失 |

## 当前最稳覆盖范围

```text
Home
-> Stented list
-> Hancock II detail
-> Size 21
-> THV Current
-> Evolut Ideal Placement
-> Video overlay
```

证据来源：

- `work/docs/capture-manifest.md:15-23`
- `work/docs/capture-manifest.md:68-75`
- `work/docs/supervisor-checklist.md:79-86`

## 当前最需要优先处理

1. 建立原生 App 全量页面总表。
2. 补非 screenshot 范围页面采集。
3. 重验 Lotus 链路。
4. 补视频 overlay 触发后证据。
5. 建立截图、路由、数据、素材、验证结果的逐页映射表。
