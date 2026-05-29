| 类型 | 问题 | 证据 | 影响范围 | 优先级 | 建议下一步 |
| --- | --- | --- | --- | --- | --- |
| Gap | 主链路缺少真实浏览器 E2E 点击闭环 | `tests/navigation.test.mjs` 主要是 render / contract；主链目标是 `#/ -> ... -> Video` | 主链验收 | 高 | 补从首页一路点击到 video overlay 的 E2E |
| Gap | 缺少 screenshot -> route -> data -> asset -> test 映射表 | `capture-manifest.md` 只有截图清单 | 证据闭环 | 高 | 建立逐页映射表 |
| Gap | Web 复刻截图归档不足 | 只有原生 `work/captures/raw/*.png` 较完整 | 视觉验收 | 高 | 保存 Web 截图并和原生截图成对记录 |
| Gap | Video overlay 缺少 Web 打开后的截图 / 录屏 | 原生有 `013-video-overlay.png`，Web 只有 contract 测试 | 视频交互 | 高 | 点击视频入口并保存 overlay 证据 |
| Gap | Lotus 数据已补但证据未闭环 | Lotus media 指向 `hancock-lotus-board.png`，但缺最新对照 | Archived / Lotus | 高 | 从 Archived 点击到 Lotus placement 并截图验证 |
| Gap | Archived / Myval 缺逐页复采 | `capture-manifest.md` 标注未完整复采 | THV 分支 | 高 | 补原生截图、Web 截图和映射 |
| Risk | `325 routes` 与 `331 routes` 快照并存 | checklist 是旧快照，当前基线 331 | 验收口径 | 中 | 统一 route count 文档口径 |
| Risk | `verify-routes.mjs` 本地执行不稳定 | 曾出现权限 / 模块解析问题 | 自动验证 | 中 | 修复执行环境，稳定输出当前基线 |
| Gap | 原生全量页面状态表未建立 | `page-map-agent.md` 只有 page map | 完整复刻范围 | 高 | 建立 `已截图 / 已实现 / 已验证 / 缺失` 状态表 |
| Gap | 一级入口还没有完整复刻 | Home 入口包含 Stentless / Rings / TAVR / Case / More 等 | 首页全入口 | 高 | 补一级入口骨架，保证可点击且状态明确 |
| Gap | Stentless / Sutureless / Rings / TAVR 深层仍是推断 | 只有 bundle token 和 page map，缺逐页截图 | 非 Stented 业务线 | 高 | 每类先做一条代表链路 |
| Gap | Stented 列表只有 3 个 demo valve | 当前为 `hancock-ii / aspire / avalus` | Stented catalog | 中 | 扩展真实列表，缺数据条目标注 pending |
| Gap | More 子页深层内容不足 | `018-more.png` 显示 12 个条目 | More 工具页 | 中 | 补 Contact / Report Problem / About / Disclaimer 等子页 |
| Gap | Quick Selector 只有弹窗 contract | `020-quick-selector.png` 有真实选择器，Web 测试只验 modal | 快捷选择器 | 中 | 补类别切换、size 选择和 THV 跳转 |
| Gap | Bookmarks 只有空态 | `017-bookmarks.png` 是空态截图 | 收藏功能 | 中 | 补收藏、删除、跳回源页面 |
| Gap | Identify / Valve Fracture 关系未确认 | `page-map-agent.md` 标注关系未完全确定 | 工具链路 | 中 | 确认原生关系并补深层页面 |
| Gap | 素材 provenance 不完整 | `asset-map-agent.md` 有盘点，但未逐页绑定 route | 素材可信度 | 中 | 建立 asset provenance 表 |
| Gap | 多视口视觉验收不足 | 缺 390 / 768 / 1014 / desktop 截图 | 响应式布局 | 中 | 保存多视口截图并检查遮挡和横向滚动 |
| Gap | 医疗术语未独立锁定 | 文案分散在 `scripts/data/*` 和页面组件中 | 术语一致性 | 低 | 建立 terminology checklist |
