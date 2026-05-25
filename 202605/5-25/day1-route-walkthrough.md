# Day 1: Route Walkthrough

这份笔记用来记录今天手动点击过的主链路。

主链路可以理解成：

`Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut Plan -> Placement -> Video overlay`

这个项目是静态 Web SPA，页面切换主要靠 hash 路由完成。也就是说，大部分页面都还是同一个 index.html，只是地址里 # 后面的内容变了，scripts/router.js 再根据这个 hash 决定显示哪一页。
## 主链路表
| 步骤 | 页面 | 路由 | 可能涉及文件 | 数据或素材 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | Home | #/ | scripts/pages/home.js | scripts/data/categories.js scripts/data/utilities.js | 首页由 renderHome() 生成。页面上能看到五大分类入口和工具入口。底部导航、顶部栏等公共外壳来自 scripts/components.js。 |
| 2 | Stented | #/category/stented | scripts/pages/category.js | scripts/data/categories.js scripts/data/valves.js | 点击首页里的 Stented 入口后进入分类列表页。这里会根据 stented 分类筛选瓣膜列表。 |
| 3 | Hancock II | #/valve/hancock-ii | scripts/pages/valve-detail.js | scripts/data/valves.js assets/images/hancock-detail-*.png | 点击 Hancock II 后进入瓣膜详情页。页面标题、厂家、图片、尺寸列表等主要来自 valves.js 里的 hancock-ii 数据。 |
| 4 | Size 21 | #/valve/hancock-ii/size/21 | scripts/pages/size-detail.js | scripts/data/valves.js assets/images/newSize21.png | 点击 Size 21 后进入尺寸页。这里会显示该尺寸对应的 stent ID、true ID、THV Current、THV Archived、Guidance 等入口。 |
| 5 | THV Current | #/valve/hancock-ii/size/21/thv/current | scripts/pages/thv-selector.js | scripts/data/valves.js | 当前 THV 推荐页。current 表示当前推荐方案，数据来自 size.thvCurrent，其中包含 Evolut、S3、Myval 等推荐项。 |
| 6 | Evolut Plan | #/valve/hancock-ii/size/21/thv/current/plan/evolut | scripts/pages/thv-plan.js | scripts/data/valves.js assets/images/hancock-evolut-board.png assets/images/prototype/hancock-video-poster.png | 点击 Evolut 推荐项后进入方案详情页。这里展示 Evolut 的推荐尺寸、说明、参考图和视频入口。 |
| 7 | Placement | #/valve/hancock-ii/size/21/thv/current/plan/evolut/placement | scripts/pages/thv-placement.js | scripts/data/valves.js assets/images/hancock-evolut-board.png 视频数据 | 点击 Ideal Placement / 理想位置后进入放置说明页。页面仍然使用 Evolut 这条 THV 方案的数据，只是展示重点从“方案详情”变成“理想放置位置”。 |
| 8 | Video overlay | 页面内弹层，hash 通常不变 | scripts/app.js scripts/components.js 当前页面文件 | scripts/data/valves.js 里的 video 数据 assets/images/prototype/hancock-video-poster.png | 点击视频入口后，不会切换到新的 hash 路由，而是打开页面内视频弹层。app.js 监听 data-video-*，写入 state.videoPanel；components.js 里的 videoPanel() 负责渲染弹层。 |

