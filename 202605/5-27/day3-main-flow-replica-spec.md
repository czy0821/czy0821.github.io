# Main Flow Replica Spec

## 说明

这是一份主链路复刻 spec，不是产品优化建议。

目标链路限定为：

`Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay`

这份 spec 以两类证据为准：

1. 仓库内主链路截图证据、同目录 `day3-main-flow-file-map.md` 和 `day3-implementation-plan.md`
2. 当前代码中的已实现路由、页面文件、结构化数据和测试

当两者不完全一致时，执行优先级如下：

1. 截图证据决定可见页面构图、信息层级、入口控件和页面密度
2. 当前代码决定本轮可复用的路由形态、数据节点和测试边界
3. 未被截图和现有代码同时证明的内容，不应在本 spec 中被当作“必须新增”的目标

## 目标

- 复刻当前仓库已经有截图证据和代码支撑的主链路，而不是扩展整个产品范围
- 保持这条链路从 `#/` 到视频 overlay 可点击、可返回、可验证
- 让 screenshot-backed 页面优先贴近原生 app 的单列壳层、信息顺序、视觉密度和控件形态
- 继续使用当前仓库里的路由结构、页面文件分工和 `scripts/data/valves.js` 数据模型承载这条链路
- 对 `Hancock II`、`Size 21`、`THV Current`、`Evolut`、`Video overlay` 保持截图优先的复刻口径

## 本轮不做什么（非目标）

- 不是对全站做视觉优化、现代化重设计或通用体验升级
- 不扩展到 `Stentless`、`Sutureless`、`Rings`、`TAVR`、`Bookmarks`、`Identify`、`More` 等非本轮主链路页面
- 不重做现有路由系统，不把这轮 spec 扩成新的 IA 或新的导航体系
- 不新增没有截图证据支持的临床说明、推荐逻辑或设备内容
- 不要求恢复真实视频 URL、YouTube ID 或原生多视频目录
- 不把 `Lotus`、archived 全量闭环、utility 页面补齐等旁支问题并入本 spec
- `THV ARCHIVED` 只要求在 Size 21 页面保留可见入口，不要求打通 archived 后续链路或补齐 archived 全量数据
- 不因为“看起来可以更好”而增加新的卡片、双栏布局、桌面侧栏、额外说明区或装饰性视觉元素

## 页面清单

| 步骤 | 对应路由或状态 | 对应实现文件 | 数据节点 | 截图证据 | 关键素材 |
| --- | --- | --- | --- | --- | --- |
| Home | `#/` | `scripts/router.js`, `scripts/pages/home.js`, `scripts/components.js` | `scripts/data/categories.js` 中 `slug: "stented"` | `work/captures/raw/001-home-tab.png` | `./assets/images/focus-stented.png`, `./assets/images/quick.png` |
| Stented list | `#/category/stented` | `scripts/router.js`, `scripts/pages/category.js`, `scripts/components.js` | `getValvesByCategory("stented")` | `work/captures/raw/007-stented-axpress.png` | 原生壳层与列表控件 |
| Hancock II detail | `#/valve/hancock-ii` | `scripts/router.js`, `scripts/pages/valve-detail.js`, `scripts/components.js` | `getValve("hancock-ii")` | `work/captures/raw/008-hancock-detail.png` | `./assets/images/hancock-detail-board.png`, `./assets/images/hancock-detail-exterior.png`, `./assets/images/hancock-detail-fluoro.png`, `./assets/images/hancock-detail-marker.png`, `./assets/images/hancock-detail-topview.png` |
| Size 21 | `#/valve/hancock-ii/size/21` | `scripts/router.js`, `scripts/pages/size-detail.js`, `scripts/components.js` | `getValveSize("hancock-ii", "21")`, `HANCOCK_II_SIZE_21` | `work/captures/raw/010-size21.png` | `./assets/images/newSize21.png` |
| THV Current | `#/valve/hancock-ii/size/21/thv/current` | `scripts/router.js`, `scripts/pages/thv-selector.js`, `scripts/components.js` | `size.thvCurrent` | `work/captures/raw/011-thv-current.png` | `./assets/images/INFO.png`, `./assets/images/INFO-BLK.png` |
| Evolut screenshot-backed state | `#/valve/hancock-ii/size/21/thv/current/plan/evolut` | `scripts/router.js`, `scripts/pages/thv-plan.js`, `scripts/components.js` | `getThvOption("hancock-ii", "21", "current", "evolut")` | `work/captures/raw/012-evolut-plan.png` | `./assets/images/hancock-evolut-board.png`, `./assets/images/prototype/hancock-video-poster.png` |
| Placement continuity state | `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` | `scripts/router.js`, `scripts/pages/thv-placement.js`, `scripts/components.js` | `option.placement` | `work/captures/raw/015-after-video-back.png` 仅作为 video 返回后的状态证据，不作为独立 native 首屏基线 | `./assets/images/hancock-evolut-board.png`, `./assets/images/prototype/hancock-video-poster.png` |
| Video overlay | 路由基座为 `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`，打开条件为 `state.videoPanel != null` | `scripts/components.js` 中的 `videoPanel()`，由 `scripts/app.js` 触发并由 `scripts/state.js` 保存状态 | `option.placement.video -> state.videoPanel` | `work/captures/raw/013-video-overlay.png` | `./assets/images/prototype/hancock-video-poster.png` |

## 主链路映射要求

- 本 spec 中列出的每个主链路步骤都必须同时具备：
  - 对应路由，或明确的“路由基座 + 交互状态”
  - 对应实现文件
  - 对应数据来源，或明确素材来源
- `Video overlay` 虽然不是独立 hash 路由，但它仍然必须有明确映射：
  - 路由基座：`#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`
  - 实现文件：`scripts/components.js` 的 `videoPanel()`
  - 触发文件：`scripts/app.js`
  - 状态来源：`scripts/state.js` 中的 `videoPanel`
  - 数据来源：`option.placement.video`

## 视觉与响应式验收口径

- 截图比对优先使用移动端视口 `390x844`，因为本轮复刻目标是原生 app 单列壳层；桌面端使用 `1440x900` 验证页面不被错误拉伸成双栏、仪表盘或侧栏布局。
- 桌面端允许沿用当前仓库已有 native-style shell 的居中宽度、外层背景和页面容器规则；不要求把内容铺满整个浏览器宽度。
- “贴近截图”至少要求页面区域顺序、主要文字层级、入口控件、列表/按钮密度、核心图片或 poster 的位置与截图一致；系统字体渲染、浏览器默认抗锯齿和极小像素差异不作为失败原因。
- 所有 route-backed 主链路页面在 `390px` 宽度下不得出现横向滚动，主要按钮、tile、图片、视频入口和 bottom tabs 不得互相遮挡。
- 本轮不新增桌面专属信息架构；响应式只做必要的宽度适配和防溢出处理。

## 运行与验收入口

- 验收时在仓库根目录 `D:\学习\vie-aorta-master` 启动本地服务。
- 推荐启动方式沿用仓库 README：`PORT=14712 ./start.sh start --daemon`；如当前环境不能运行 shell 脚本，可用等价静态服务打开仓库根目录的 `index.html`。
- 默认验收入口为 `http://localhost:14712/#/`。
- 主链路深链验收入口至少包括：
  - `http://localhost:14712/#/`
  - `http://localhost:14712/#/category/stented`
  - `http://localhost:14712/#/valve/hancock-ii`
  - `http://localhost:14712/#/valve/hancock-ii/size/21`
  - `http://localhost:14712/#/valve/hancock-ii/size/21/thv/current`
  - `http://localhost:14712/#/valve/hancock-ii/size/21/thv/current/plan/evolut`
  - `http://localhost:14712/#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`

## 交互边界

- Home 中 `Stented` 入口必须可点击并进入 `#/category/stented`；bottom tabs 和 Quick Selector 在本轮主要验收可见性与壳层一致性，非主链路 tab 的后续闭环不纳入验收。
- `THV ARCHIVED` 在 Size 21 页面只验收入口可见；点击后的现有行为、禁用态或未完成后续页都不作为本轮失败条件。
- Stented 列表和 THV Current tile 必须按截图与当前结构化数据顺序展示，不得为了视觉均衡随意重排。
- Video overlay 的最低交互验收是：点击视频入口后打开 overlay；点击 overlay 的关闭/返回控制后关闭 overlay，并保持在 placement 路由与 placement 状态。

## 验收标准

### 总体标准

- 这条主链路必须从 `#/` 一路点通到视频 overlay，并且从 overlay 返回后能回到 placement 状态
- screenshot-backed 页面必须优先贴近截图构图，不得为了“更现代”擅自改成双栏、宽仪表盘、侧栏布局或额外卡片堆叠
- 主链路页面继续运行在当前 native-style shell 中，保持单列阅读路径
- 页面所用的 Hancock II / Evolut 专属图片板和 poster 必须继续使用现有结构化数据映射，不允许退回成通用占位图
- 不允许把 fallback 页面、缺失节点或旁支页面冒充成主链路目标页

### Home

- `#/` 必须可渲染 Home 主入口
- 页面中必须存在可见且可点击的 `Stented` 入口
- Home 必须保留 native-style bottom tabs 和 Quick Selector 入口；非主链路 tab 和 Quick Selector 后续页面不属于本轮闭环验收

### Stented list

- `#/category/stented` 必须是单列 plain-list 形态，不改成网格或多栏
- 列表中必须按当前数据/截图顺序包含 demo 范围内的 3 个 stented valve：`Aspire`、`Avalus`、`Hancock II`
- `Hancock II` 行必须可点击进入详情页
- 页面需要保留搜索栏和当前列表密度

### Hancock II detail

- `#/valve/hancock-ii` 必须显示 `Hancock II` 对应详情页
- 页面必须优先使用 Hancock II 专属 `detailBoard`
- 页面必须保留 Fluoroscopic Markers 区块
- 页面必须保留 size pill 行，并包含 `21 / 23 / 25 / 27 / 29`
- `21` 必须可点击进入 `#/valve/hancock-ii/size/21`

### Size 21

- `#/valve/hancock-ii/size/21` 必须显示 `Size: 21`
- 页面必须显示以下参数：
  - `Stent ID 18.5`
  - `Height 15`
  - `True ID 17`
  - `Non-Fracturable`
  - `True Balloon Size: N/A`
- 页面必须同时保留 `THV CURRENT` 和 `THV ARCHIVED` 两个入口按钮
- `THV ARCHIVED` 在本轮只验收入口可见，不验收 archived 后续页面闭环
- `THV CURRENT` 必须进入 `#/valve/hancock-ii/size/21/thv/current`

### THV Current

- `#/valve/hancock-ii/size/21/thv/current` 必须显示当前推荐列表
- 页面必须按以下顺序保留 6 个 tile：
  - `ACURATE`
  - `Allegra 23`
  - `Evolut 23`
  - `Myval 20`
  - `NAVITOR`
  - `S3 20`
- warning 类 tile 必须继续保留 caution 表达；当前主链路至少要确认 `ACURATE` 和 `NAVITOR` 的 caution / warning 图标、色块或文案表达没有被删成普通 tile
- `Evolut 23` 必须可点击进入 `#/valve/hancock-ii/size/21/thv/current/plan/evolut`

### Evolut screenshot-backed state

- `#/valve/hancock-ii/size/21/thv/current/plan/evolut` 是当前 web 路由中的落点，但本轮必须把它当作“对应 native Evolut Ideal Placement 可见状态”的 screenshot-backed 页面来复刻
- 本轮不把它定义成一个被原生截图单独证明过的“独立 plan screen”
- 页面必须使用 Hancock II 专属 `hancock-evolut-board.png`
- 页面必须保留 Evolut 指令性说明文本，尤其是：
  - 根据 valsalva 窦大小在两个尺寸间做判断
  - `Align Evolut inflow 3-4mm below the fluoroscopic marker in the sewing ring`
- 页面必须保留视频入口行，并透传 `data-video-*` 元数据

### Placement continuity state

- `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` 必须可直达
- 这一层的目标是保持 web 深链连续性、返回路径和当前代码结构，而不是宣称它有一张独立 native 首屏截图作为基线
- 页面必须显示 placement summary、placement image strip、video row 三个核心区块
- placement 页必须作为视频 overlay 的触发前态
- 从 overlay 关闭或返回后，应回到 placement 页而不是跳回 Home、Stented 或 fallback

### Video overlay

- Video overlay 是交互状态，不是独立 hash 路由；验收时必须把它当作“placement 上打开后出现的全屏层”
- 点击 placement 页的视频入口后，必须出现全屏视频 overlay
- overlay 必须尽量贴近真实采集到的播放器态，而不只是“有个海报弹层就算完成”
- overlay 至少要保留以下结构特征：
  - 顶部工具条
  - 关闭/返回控制
  - 中央 stage 区
  - 底部控制条/时间轴样式
- 点击关闭/返回控制后，overlay 必须关闭，并回到 `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`
- 如果当前结构化数据没有可信可嵌入视频源，允许 stage 使用 poster 作为内容回退
- 但即使使用 poster 回退，外围播放器结构、交互状态和返回路径也必须对齐真实 overlay 证据
- 不允许伪造 iframe、YouTube ID 或未知来源视频地址来“假装完成播放”

## 风险

- `#/.../plan/evolut` 这个 web 路由与采集文档里的 “Evolut Ideal Placement” 不是一一对应的命名关系；实现时不能把这层差异当作自由扩写新页面的理由
- Video overlay 是交互态而非独立路由；如果只验 route，不验点击后的状态，很容易误判为“已复刻完成”
- 当前截图证据闭环的是一条窄主链，不代表整个 app 已被逐页采集
- 当前 placement 路由的 native 证据主要来自“视频返回后的状态”，不能把它表述成“已经有独立 native placement 首屏截图”
- 自动化测试目前主要能证明路由、metadata、overlay shell 和媒体映射存在，不能单独证明截图 fidelity 完成

## 验证命令

在仓库根目录 `D:\学习\vie-aorta-master` 运行：

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

这些命令的验证目标应覆盖：

- Home / Stented / Hancock II / Size 21 / THV Current / Evolut / Placement 主链可达
- Hancock II 专属媒体映射仍然存在
- Evolut screenshot-backed state 与 placement continuity state 的 `data-video-*` 元数据仍然存在
- Video overlay shell 结构仍然存在
- 主链中没有被错误替换成 fallback 的节点

## 手动验收补充

自动化测试通过后，仍需手动完成以下验收：

1. 对照 `001-home-tab.png`、`007-stented-axpress.png`、`008-hancock-detail.png`、`010-size21.png`、`011-thv-current.png`、`012-evolut-plan.png` 做逐页截图比对
2. 在 `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` 点击视频入口，确认 overlay 打开
3. 对照 `013-video-overlay.png` 检查 overlay 的顶部、stage、底部控件样式
4. 从 overlay 返回，确认回到 placement 状态，并用 `015-after-video-back.png` 做返回后状态对照

手动验收应保留以下产物，便于复核：

- 移动端 `390x844` 截图：保存到 `work/captures/verification/day3/mobile/`
- 桌面端 `1440x900` 截图：保存到 `work/captures/verification/day3/desktop/`
- 每个主链路页面至少保留一张截图；Video overlay 打开态和 overlay 返回后的 placement 状态必须分别保留截图
