# demo-script.md

## 演示结构

### 说明项目是什么

大家好，我这周做的是 `ViV Aortic` 原生 App 的 Web 复刻项目。

这个项目不是普通展示页，而是把原生 App 里的医疗工具流程复刻到一个静态 Web SPA 中。当前 Web 版本用 hash route 驱动页面，核心目标是复刻原生 App 的浏览路径、页面层级、截图证据、素材和交互状态。

本周我重点围绕一条主链路推进：

```text
Home
-> Stented
-> Hancock II
-> Size 21
-> THV Current / Archived
-> Plan
-> Placement
-> Video overlay
```

当前阶段的结论是：主样例链已经具备结构可达和测试保护，但完整 App 全量复刻还没有完成。

### 说明项目如何启动

这个项目是静态 Web 项目，入口文件是 `index.html`。

本地启动方式在 `README.md` 中：

```bash
./start.sh
```

默认访问地址：

```text
http://localhost:14712
```

如果要后台启动，可以使用：

```bash
PORT=14712 ./start.sh start --daemon
```

启动后主要看两个东西：

- 首页是否正常出现原生 App 风格的入口。
- hash route 是否能进入主链页面，例如 `#/category/stented`、`#/valve/hancock-ii`、`#/valve/hancock-ii/size/21`。

### 演示主链路

接下来演示主链路。

第一步，从首页进入 `Stented`。这里首页已经保留了主链入口标记：

```text
data-home-main-flow
data-main-flow-entry="stented"
```

第二步，进入 Stented 列表页。当前 demo 数据里保留了三个 Stented valve：

```text
hancock-ii
aspire
avalus
```

第三步，点击 `Hancock II`，进入详情页。这个页面对应原生截图中的 Hancock II detail，核心内容包括厂家信息、图片区域、size 按钮和相似瓣膜信息。

第四步，选择 `Size 21`。Size 页面展示：

```text
Stent ID
Height
True ID
THV CURRENT
THV ARCHIVED
```

第五步，进入 `THV Current`。当前 Size 21 的 Current 选项包括：

```text
acurate
allegra
evolut
myval
navitor
s3
```

第六步，进入 Evolut plan 和 placement。Placement 页面需要能展示 video guidance，并能打开 video overlay。

当前主链路可以说是 route / data / render 测试层面可走通，但还缺真实浏览器 E2E 和 Web 截图归档。

### 说明主链路文件结构

主链路涉及的代码结构如下：

```text
index.html
-> scripts/app.js
-> scripts/router.js
-> scripts/pages/home.js
-> scripts/pages/category.js
-> scripts/pages/valve-detail.js
-> scripts/pages/size-detail.js
-> scripts/pages/thv-selector.js
-> scripts/pages/thv-plan.js
-> scripts/pages/thv-placement.js
-> scripts/pages/guidance.js
-> scripts/components.js
-> scripts/data/valves.js
-> styles/app.css
```

其中：

- `scripts/router.js` 负责 route 分发。
- `scripts/pages/*` 负责各页面渲染。
- `scripts/components.js` 负责复用 native row、bottom tabs、video panel、Quick Selector 等组件。
- `scripts/data/valves.js` 存放 valve、size、THV、placement、video metadata。
- `tests/routes.test.mjs` 和 `tests/navigation.test.mjs` 负责验证主链和页面 contract。

这周理解到一个关键点：这个项目不能只看页面能不能打开，还要看截图、route、data、asset、test 是否能对应起来。

### 说明本周写的 spec

本周整理和使用的 spec 主要在 `docs/superpowers/specs/`：

```text
2026-03-29-viv-aortic-web-design.md
2026-03-30-viv-aortic-high-fidelity-ui-design.md
2026-03-30-viv-aortic-media-alignment-design.md
2026-03-30-viv-aortic-responsive-ui-design.md
2026-03-31-viv-aortic-app-fidelity-design.md
2026-03-31-viv-aortic-layout-repair-design.md
2026-03-31-viv-aortic-screenshot-fidelity-design.md
2026-04-01-viv-aortic-cn-upgrade-design.md
```

这些 spec 的作用是把复刻标准从“页面能显示”提升到“像原生 App 一样工作”：

- 页面结构要贴近原生 App。
- 图片和视频要有来源。
- fallback 要诚实，不能伪装成真实内容。
- 主链路要能点击，而不是只靠手输 URL。
- 移动端和桌面端都要能验收。

### 说明本周改动

本周主要改动分成几类。

第一类是 Home 主入口保护：

```text
scripts/pages/home.js
scripts/components.js
tests/navigation.test.mjs
```

保证 Stented 是主链入口，并保留 Quick Selector 和底部 Tab。

第二类是 Stented / Hancock II 主链页面：

```text
scripts/pages/category.js
scripts/pages/valve-detail.js
scripts/pages/size-detail.js
scripts/pages/thv-selector.js
scripts/pages/thv-plan.js
scripts/pages/thv-placement.js
scripts/pages/guidance.js
```

第三类是数据补齐：

```text
scripts/data/valves.js
```

当前 `Hancock II / Size 21 / Archived / Lotus` 已经存在，Lotus media 指向：

```text
./assets/images/hancock-lotus-board.png
```

第四类是测试增强：

```text
tests/routes.test.mjs
tests/navigation.test.mjs
scripts/verify-routes.mjs
```

当前数据基线是：

```text
331 routes / 331 unique routes
```

### 展示验证结果

本周验证主要看两个测试文件：

```text
tests/navigation.test.mjs
tests/routes.test.mjs
```

这两个测试覆盖：

- Home 主入口。
- Stented native shell。
- THV Current / Archived CTA。
- Video panel contract。
- Quick Selector dialog contract。
- Image preview contract。
- Stented demo valve 数据。
- Media schema。
- Lotus 是否存在。
- THV deep routes。
- fallback 是否安全。

需要特别说明的是，`supervisor-checklist.md` 中早期记录过：

```text
325 routes
Lotus fallback
```

但这是旧快照。当前数据层已经是：

```text
331 routes / 331 unique routes
Lotus 数据已补
```

不过 Lotus 还不能直接标为完整完成，因为缺少截图对照和 E2E 点击证据。

### 说明 bug / gap list

本周整理了 `week2-bug-gap-list.md`。

最重要的高优先级 gap 有：

```text
主链缺少真实浏览器 E2E
缺少 screenshot -> route -> data -> asset -> test 映射表
Web 截图没有和原生截图成对归档
Video overlay 缺少 Web 打开状态截图
Lotus 数据已补但证据未闭环
Archived / Myval 缺少逐页复采
原生全量页面状态表未建立
一级入口还没有完整复刻
Stentless / Sutureless / Rings / TAVR 深层仍是推断
```

这些问题说明：当前不是“完整 App 已复刻完成”，而是“主样例链和证据体系已经搭起来，下一步要补闭环”。

### 说明下周路线图

最后讲下周路线图，对应 `full-app-replica-roadmap.md`。

下周优先级是：

```text
P0: 补主链浏览器 E2E
P0: 建立 screenshot-to-route-to-data-to-asset-to-test 映射表
P0: 保存主链 Web 截图，并和原生截图成对归档
P0: 补 Lotus / Myval / Archived 的证据闭环
P0: 补 Web video overlay 截图或录屏
P1: 建立原生全量页面状态表
P1: 补一级入口骨架
P1: 每类非 Stented 业务线先完成一条代表性深层链路
P1: 稳定 verify-routes 输出
P2: 建立素材 provenance 和术语表
```

我的总结是：

本周完成的是从“能做 demo”到“有 page map、有截图证据、有主链 route、有测试保护、有 bug / gap 清单”的转变。下周的目标不是盲目加页面，而是把主链从“结构可达”推进到“真实点击闭环 + 截图对照 + 视频证据 + 可回归测试”。