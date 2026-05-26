# Day 2 ViV Aortic 完整 App 页面总表

整理依据：今天已阅读和讨论过的项目资料，包括：

- `D:\学习\vie-aorta-master\work\docs\page-map-agent.md`
- `D:\学习\vie-aorta-master\work\docs\capture-manifest.md`
- `D:\学习\vie-aorta-master\work\docs\supervisor-checklist.md`
- `D:\学习\vie-aorta-master\work\docs\viv-aortic-native-reference.md`
- `D:\学习\vie-aorta-master\work\assets-inventory\asset-map-agent.md`
- `D:\学习\vie-aorta-master\scripts\router.js`
- `D:\学习\vie-aorta-master\scripts\data\categories.js`
- `D:\学习\vie-aorta-master\scripts\data\utilities.js`
- `D:\学习\vie-aorta-master\scripts\data\valves.js`

说明：

- “已采集”表示已有真实原生截图或明确截图清单。
- “已实现”表示当前 Web 项目中有对应路由、数据或页面结构。
- “待确认/盲区”表示资料中只看到 bundle token、旧截图、推断关系，或监督文件明确标为未闭环。

## 页面总表

| 模块 | 至少覆盖 | 当前状态 | 证据来源 | 备注 |
| --- | --- | --- | --- | --- |
| 顶层入口 | Home | 已采集 / 已实现 / 已验证 | `page-map-agent.md` 说明首页为 `HomeScreen`；`capture-manifest.md` 记录 `001-home-tab.png` 和 `016-home-after-video.png`；`router.js` 默认路由为 `#/` | Home 是 App 总入口，承载分类入口、工具入口和 Quick Selector |
| 底部 Tab | Home、Stented、Stentless、Sutureless、Rings、TAVR | 已识别；Home 和 Stented 主链已重点采集；其余深层仍有盲区 | `page-map-agent.md` 明确底部 Tab 为 `Home / Stented / Stentless / Sutureless / Rings / TAVR`；`categories.js` 定义 5 个业务分类；`supervisor-checklist.md` 要求扩大盲区扫描范围 | 底部 Tab 是原 App 主导航层 |
| 首页工具 | Bookmarks、Case of the Month、Identify a Valve、Valve Fracture、More、Quick Selector | Bookmarks / Identify / More / Quick Selector 已采集；Case of the Month、Valve Fracture 深层待确认 | `page-map-agent.md` 列出首页工具入口；`utilities.js` 定义 `bookmarks / cases / identify / fracture / more`；`capture-manifest.md` 记录 `017-bookmarks.png`、`018-more.png`、`019-identify.png`、`020-quick-selector.png` | Quick Selector 是右下角悬浮入口，不是普通列表页 |
| Stented 主链路 | Stented list、Hancock II、Size 21、THV Current、THV Archived、Plan、Placement、Video | 主链已采集 / 已实现 / 已验证；Archived、Myval、Lotus 属于旁支或待重验 | `capture-manifest.md` 记录主链 `Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut Ideal Placement -> Video overlay`；`supervisor-checklist.md` 记录 Web 路由链；`valves.js` 定义 Hancock II、Size 21、THV current/archived、Evolut/Myval/Lotus 数据 | 最稳主链是 Current -> Evolut -> Video；Lotus 证据存在版本冲突 |
| Stentless 浏览链 | Archive/List、Content/Detail、Size、THV Select、Ideal Placement、Stentless Valve Tricks | 主要来自 bundle/page map，深层真实采集不足 | `page-map-agent.md` 列出 `StentlessArchiveScreen`、`StentlessContentScreen`、`StentlessSizeScreen`、`StentlessTHVSelectScreen`、`StentlessIdealPlacementScreen`、`StentlessValveTricksPage` | 需要补真实逐页采集和 Web 对应关系 |
| Sutureless 浏览链 | Archive/List、Content/Detail、Size、THV Select、Ideal Placement | 主要来自 bundle/page map，深层真实采集不足 | `page-map-agent.md` 列出 `SuturelessArchiveScreen`、`SuturelessContentScreen`、`SuturelessSizeScreen`、`SuturelessTHVSelectScreen`、`SuturelessIdealPlacementScreen` | 需要补真实逐页采集和 Web 对应关系 |
| Rings 浏览链 | SewingRingArchive、Content、RingSize、RingTHVSelect、RingIdealPlacement | 主要来自 bundle/page map，深层真实采集不足 | `page-map-agent.md` 列出 `SewingRingArchiveScreen`、`DimensionsewingRingContentScreen`、`RingSizeScreen`、`RingTHVSelectScreen`、`RingIdealPlacementScreen`、`RingStackScreen` | 需要确认 Rings 与普通瓣膜链路的差异 |
| TAVR 浏览链 | TavrSelect / AVRContentScreen、AVRSizeScreen、AVRInfoScreen | 待确认 / 盲区 | `page-map-agent.md` 列出 `AVRStackScreen`、`AVRContentScreen`、`AVRSizeScreen`、`AVRInfoScreen`、`TavrSelect`；并说明 TAVR 内部顺序仍不完全确定 | 需要确认列表页、详情页、帮助页是否并列或串联 |
| Bookmarks | BookmarkScreen、空状态、从收藏返回原页面能力 | 空状态已采集；深层返回链待确认 | `capture-manifest.md` 记录 `017-bookmarks.png`；`page-map-agent.md` 说明 bundle 含 `BookmarkScreen` 和 `navFromBookmarkStackScreen` | 更像首页工具入口，不是底部 Tab |
| Case of the Month | COTMStackScreen、COTM list、COTM article | 待确认 / 深层盲区 | `page-map-agent.md` 说明 bundle 含 `COTMStackScreen`、`COTMarticle`、`COTMlist`；`supervisor-checklist.md` 将 Case of the Month 深层页面列为未真实逐页采集范围 | 首页入口存在，但深层列表/文章页需要补采集 |
| More 栈 | Contact、Disclaimer、Acknowledgements、Introduction、Download Preferences、Manufacturer Contact、Report a Problem 等 | More 列表已采集；深层页面待确认 | `capture-manifest.md` 记录 `018-more.png` 且已见 12 个条目；`page-map-agent.md` 列出 `ContactModal`、`ContactPage`、`DisclaimerPage`、`AcknowledgementModel`、`AppIntroductionPage / IntroductionPage`；`viv-aortic-native-reference.md` 列出 More 已见条目 | `AcknowledgementModel` 是否是 page 或 modal 仍需确认 |
| Identify / Fracture | 识别主页、透视分类、透视识别、透视标记、透视可见性、裂环信息 | Identify 入口已采集；Identify 与 Valve Fracture 关系待确认 | `capture-manifest.md` 记录 `019-identify.png`；`page-map-agent.md` 列出 `FluoMainScreen`、`Fluoroscopic Classification`、`Fluoroscopic Identification`、`Fluoroscopic Markers`、`Fracturable`、`Non-Fracturable`；`page-map-agent.md` 明确二者关系未完全确定 | 可能是两个入口，也可能 Valve Fracture 是 Identify 栈下子页 |
| Quick Selector | QuickSelectScreen、STENTLESS VALVE、THV CURRENT、THV ARCHIVED、下载偏好弹窗 | 已采集入口状态；承载形态待确认 | `capture-manifest.md` 记录 `020-quick-selector.png`；`page-map-agent.md` 说明 bundle 含 `QuickSelectScreen`、`DownloadPrefModal`；`viv-aortic-native-reference.md` 说明当前打开到 `STENTLESS VALVE` 且包含 `THV CURRENT / THV ARCHIVED` | 仍需确认它是 modal、独立 stack，还是 Home 覆盖层 |
| 视频状态 | Video overlay、Video Guidance、播放器打开态 | 主链视频 overlay 已采集；真实视频链接/文件缺失 | `capture-manifest.md` 记录 `013-video-overlay.png`；`supervisor-checklist.md` 要求视频必须有打开状态证据；`asset-map-agent.md` 说明实际视频文件、YouTube/外链 URL、原生多视频标题表未恢复 | 视频不是稳定独立路由，不能只靠按钮验收 |

## 最小验收覆盖

按照今天讨论和图片要求，完整 App 页面总表至少要覆盖：

| 模块 | 至少覆盖 |
| --- | --- |
| 顶层入口 | Home |
| 底部 Tab | Stented、Stentless、Sutureless、Rings、TAVR |
| 首页工具 | Bookmarks、Case of the Month、Identify a Valve、Valve Fracture、More、Quick Selector |
| Stented 主链路 | Stented list、Hancock II、Size 21、THV Current、THV Archived、Plan、Placement、Video |
| More 栈 | Contact、Disclaimer、Acknowledgements、Introduction、Download Preferences、Manufacturer Contact、Report a Problem |
| Identify / Fracture | 识别主页、透视分类、透视识别、透视标记、透视可见性、裂环信息 |

## 当前结论

当前已经最稳闭合的是：

```text
Home
-> Stented list
-> Hancock II detail
-> Size 21
-> THV Current
-> Evolut Ideal Placement
-> Video overlay
```

当前仍需要补齐或确认的是：

```text
Stentless / Sutureless / Rings / TAVR 深层页面
Case of the Month 深层页面
Valve Fracture 深层页面
More 栈内深层页面
Quick Selector 承载形态
Lotus 链路版本冲突
视频真实链接 / 文件 / 多视频标题表
```
