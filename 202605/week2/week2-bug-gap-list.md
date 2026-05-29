# Week 2 Bug / Gap List

生成日期: 2026-05-29

## 1. 来源

本清单基于本周四天成果整理:

- `week2-thread-1-summary.md`
- `week2-thread-2-summary.md`
- `week2-thread-3-summary.md`
- `week2-thread-4-summary.md`
- `week2-viv-aortic-review.md`

本清单用于指导下一阶段复刻、验收和返工。它不等同于完整产品 backlog，而是聚焦本周已经暴露出的 bug、缺口、证据不足和验收风险。

## 2. 优先级规则

| 优先级 | 含义 |
| --- | --- |
| P0 | 阻塞主链路验收或会导致验收结论误判。 |
| P1 | 影响主要页面、主要入口、关键证据链或下一阶段开发。 |
| P2 | 影响体验、文档一致性、环境稳定性或后续维护。 |
| P3 | 细节问题，可延后处理。 |

## 3. Bug 清单

### BUG-001: Plan -> Placement 缺少用户可见入口

| 字段 | 内容 |
| --- | --- |
| 类型 | Bug / Interaction |
| 优先级 | P0 |
| 模块 | THV Plan / Ideal Placement |
| 影响路由 | `#/valve/hancock-ii/size/21/thv/current/plan/evolut` |
| 当前表现 | Plan 页中存在通往 `/placement` 的 deep link，但它是 `visually-hidden`，用户视觉上找不到可点击入口。 |
| 证据来源 | `week2-viv-aortic-review.md`；当前 `scripts/pages/thv-plan.js` 中隐藏 placement link。 |
| 影响 | route render 可以通过，但真实用户点击链不闭合；容易把“路由可达”误判为“主链路可达”。 |
| 复现步骤 | 打开 Evolut plan route，观察页面内是否有可见 `Ideal Placement` 入口。 |
| 验收标准 | 用户可以从 plan 页通过可见 CTA 进入 placement；或确认 native 逻辑是 tile 直接进入 placement，并相应调整 route / spec。 |

### BUG-002: PowerShell 直接运行 `node` 出现 `Access is denied`

| 字段 | 内容 |
| --- | --- |
| 类型 | Environment / Test |
| 优先级 | P1 |
| 模块 | 本地验证环境 |
| 当前表现 | PowerShell 直接执行 `node tests/routes.test.mjs` 等命令时出现 `Program 'node.exe' failed to run: Access is denied`。 |
| 证据来源 | Thread 2、Thread 4 均记录该问题；Thread 1 记录曾使用完整 Node 路径绕过。 |
| 影响 | 阻碍学员和后续 agent 稳定复跑测试；容易误判为代码测试失败。 |
| 复现步骤 | 在 PowerShell 中直接运行 `node tests/routes.test.mjs`。 |
| 验收标准 | 明确一条可复用验证方式，例如固定系统 Node 完整路径，或修复 PATH / 权限问题；文档中说明不要把该错误当作测试断言失败。 |

### BUG-003: 旧监督文档中的结论已过期

| 字段 | 内容 |
| --- | --- |
| 类型 | Documentation |
| 优先级 | P1 |
| 模块 | `work/docs/supervisor-checklist.md` |
| 当前表现 | 旧文档仍记录 `PASS verify-routes.mjs (325 routes)`，并记录 Lotus route 会 fallback。 |
| 本周新结论 | 当前验证为 331 条 route 可渲染；当前代码和测试中已包含 Lotus 数据与 `hancock-lotus-board.png`。 |
| 影响 | 后续 review 或开发可能引用旧结论，导致 Lotus 状态、route 数量和风险判断混乱。 |
| 验收标准 | 更新旧监督清单，或在旧结论旁明确标注 superseded，并引用 Week 2 新结论。 |

### BUG-004: 自动化测试容易掩盖“可见性”问题

| 字段 | 内容 |
| --- | --- |
| 类型 | Test Coverage |
| 优先级 | P1 |
| 模块 | `tests/routes.test.mjs`、`tests/navigation.test.mjs` |
| 当前表现 | 测试能证明 route render、DOM marker、metadata 存在，但不能证明入口对用户可见。Plan -> Placement 隐藏链接就是典型例子。 |
| 影响 | 自动化通过后仍可能存在真实点击链缺口。 |
| 验收标准 | 对主链路关键入口增加“可见且可点击”级别测试，而不只是 HTML includes。 |

### BUG-005: Quick Selector 当前形态可能与 native 不一致

| 字段 | 内容 |
| --- | --- |
| 类型 | Product Behavior |
| 优先级 | P1 |
| 模块 | Quick Selector |
| 当前表现 | Web 当前更像快捷入口 modal；native capture 中 Quick Selector 打开后进入 `STENTLESS VALVE` 选择器，并包含尺寸表和 THV Current / Archived。 |
| 证据来源 | `capture-manifest.md`；Thread 2、Thread 4。 |
| 影响 | 一级入口看似存在，但真实功能和信息结构可能偏离 native。 |
| 验收标准 | 明确 Quick Selector 是保留 Web shortcut，还是复刻 native selector；若保留差异，需写入 gap / deferred。 |

### BUG-006: Home/native shell 宽度修改尚未做足够回归抽查

| 字段 | 内容 |
| --- | --- |
| 类型 | Layout Regression Risk |
| 优先级 | P2 |
| 模块 | `styles/app.css` native shell |
| 当前表现 | Day 4 为修复 Home 横向 overflow，收紧了 native shell 宽度并给 topbar / screen / bottom tabs 增加约束。 |
| 风险 | 改动可能间接影响 Stented、Hancock II、Size 21、THV Current 等其他 native shell 页面。 |
| 验收标准 | 抽查至少 `#/category/stented`、`#/valve/hancock-ii`、`#/valve/hancock-ii/size/21`、`#/valve/hancock-ii/size/21/thv/current`，确认无横向滚动和布局压缩异常。 |

## 4. Gap 清单

### GAP-001: 整条主链路尚未截图级复刻完成

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Fidelity |
| 优先级 | P0 |
| 范围 | `Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay` |
| 当前状态 | Day 4 只实现并保护 `#/ -> Stented` 起点。 |
| 缺口 | 未完成整条主链路的截图级视觉对照、移动端/桌面端验收、视频闭环验收。 |
| 验收标准 | 每个主链节点都有 native 截图、Web 截图、route、文件、数据/素材来源、测试结果和 gap 结论。 |

### GAP-002: 主链路真实点击 E2E 不完整

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / E2E |
| 优先级 | P0 |
| 当前状态 | 当前测试能证明 route render 和 Home 起点 DOM contract。 |
| 缺口 | 尚未证明用户可以从 Home 一路点击到 Video overlay，并关闭/返回 placement。 |
| 验收标准 | 建立浏览器级 E2E: Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video open -> Video close/back。 |

### GAP-003: `#/category/stented` 尚未作为下一主链节点完成保护

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Main Flow |
| 优先级 | P0 |
| 当前状态 | Day 4 已保护 Home -> Stented 入口，但 Stented 落点本身还不是本周最小任务完成对象。 |
| 缺口 | Stented list 需要作为下一最小任务，建立截图对照、列表结构断言和视觉基线。 |
| 验收标准 | `#/category/stented` 不进 fallback，搜索框存在，Hancock II / Aspire / Avalus 列表存在，bottom tabs 与 native shell 正常，无横向 overflow。 |

### GAP-004: Lotus 状态需要重新闭环验证

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Evidence Conflict |
| 优先级 | P1 |
| 当前状态 | 旧监督文档记为 fallback；当前代码和测试中已有 Lotus 数据、素材和 route。 |
| 缺口 | 缺少“当前真实页面 + 截图 baseline + 数据节点 + 素材文件”的闭环验证。 |
| 验收标准 | 验证 `#/valve/hancock-ii/size/21/thv/archived/plan/lotus/placement` 渲染真实 Lotus 页面，而非 fallback；标题、文案、图片与 `LotusIdealPlacement` 截图一致；素材引用 `hancock-lotus-board.png`。 |

### GAP-005: Myval / THV Archived 分支未完成高保真验证

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Branch Fidelity |
| 优先级 | P1 |
| 当前状态 | 有截图或数据基础，但本周未逐页复采和对照。 |
| 影响页面 | `THV Archived`、`Myval 20`、相关 plan / placement。 |
| 缺口 | 不能仅凭 route / data 存在判定高保真完成。 |
| 验收标准 | 对 Myval 和 Archived 分支建立单独截图对照、route、data、asset 和测试记录。 |

### GAP-006: Video overlay 证据链不完整

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Interaction State |
| 优先级 | P1 |
| 当前状态 | 已有 native video overlay 证据；测试中有 video-panel 结构断言。 |
| 缺口 | Web 侧还缺系统化记录: 触发前页面、触发控件、打开后的 overlay、关闭/返回后的页面。真实 video URL / YouTube ID / 多视频标题表也未恢复。 |
| 验收标准 | 每个 video entry 记录 poster、title、summary、youtubeId、videoUrl、fallback 策略，并保存打开/关闭/返回截图或录屏。 |

### GAP-007: Quick Selector 高保真范围未确认

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Scope |
| 优先级 | P1 |
| 当前状态 | Web 中有 Quick Selector 浮动入口和 modal 结构；测试保护其存在。 |
| 缺口 | 未确认它应该是 modal、stack、overlay，还是 native 风格的 valve selector。 |
| 验收标准 | 明确 Quick Selector 的目标复刻形态，并补对应 route/state、截图证据、交互测试。 |

### GAP-008: 一级入口深层页面缺口大

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Full App Scope |
| 优先级 | P1 |
| 范围 | `Stentless / Sutureless / Rings / TAVR / Case of the Month / Identify / Valve Fracture / More` |
| 当前状态 | page map 和 bundle token 已证明这些模块存在；部分 Web 侧可能有入口或占位。 |
| 缺口 | 缺少完整 native 截图、页面树、route、数据节点、素材映射和高保真实现。 |
| 验收标准 | 每个一级入口至少标记为 `approved / partial / blocked / deferred`，并给出证据来源。 |

### GAP-009: Stented full catalog 范围未决

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Scope Decision |
| 优先级 | P1 |
| 当前状态 | Web 当前保留 3 个 demo valves: Hancock II / Aspire / Avalus。 |
| 缺口 | Native Stented 列表包含更多 valve；当前未决是保持 demo scope，还是恢复 full catalog。 |
| 验收标准 | 明确 Week 2 / Phase 1 是否只验收 3 demo valves；若要 full catalog，建立全量条目和 fallback 策略。 |

### GAP-010: 缺少完整原生页面总表

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Documentation |
| 优先级 | P1 |
| 当前状态 | Thread 2 已说明需要建立真正可执行的原生页面总表。 |
| 缺口 | 现有 page map 已有结构，但还缺统一状态表: 页面名、入口路径、原生证据、Web route、数据节点、素材文件、当前状态。 |
| 验收标准 | 建立 page status tracker，所有页面标记 `not_started / in_progress / approved / blocked / deferred`。 |

### GAP-011: 缺少截图链逐页映射表

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Evidence |
| 优先级 | P1 |
| 当前状态 | 截图证据散落在 `capture-manifest.md`、`examples/images` 和各 thread 总结里。 |
| 缺口 | 缺少统一表格把截图文件映射到 route、页面文件、数据节点和素材文件。 |
| 验收标准 | 建立 screenshot -> route -> page file -> data node -> asset -> verification result 的映射表。 |

### GAP-012: Web 验收截图尚未统一归档

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Evidence Storage |
| 优先级 | P1 |
| 当前状态 | 本周有浏览器检查和部分截图证据，但未统一归档到项目证据目录。 |
| 缺口 | 后续 review 难以复用或追溯 Web 侧截图。 |
| 验收标准 | 建立如 `work/captures/web/week2/` 的目录，并更新 manifest。 |

### GAP-013: 缺少 Phase 1 route matrix

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / PM Acceptance |
| 优先级 | P0 |
| 当前状态 | Week 2 review 中已提出，但尚未完成。 |
| 缺口 | 没有 PM 可读的 mandatory route 验收矩阵。 |
| 验收标准 | 建立 `phase-1-route-matrix.md`，包含 route、页面名、必需性、预期标题、验证方式、当前结果。 |

### GAP-014: 缺少 terminology dictionary

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Localization |
| 优先级 | P1 |
| 当前状态 | 中文术语和英文临床锚点并存。 |
| 缺口 | `THV / Current / Archived / Placement / Guidance / True ID / Stent ID / Quick Selector` 等术语尚未统一。 |
| 验收标准 | 建立 `terminology-dictionary.md`，并用它约束页面标题、按钮、提示、空状态。 |

### GAP-015: 缺少 page status tracker

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / PM Acceptance |
| 优先级 | P0 |
| 当前状态 | 当前只能从多个总结里推断状态。 |
| 缺口 | 没有一个统一页面状态表说明哪些页面已完成、哪些 partial、哪些 blocked。 |
| 验收标准 | 建立 `page-status-tracker.md`，每个页面包含 owner、status、证据、备注、下一步。 |

### GAP-016: 移动端 / 桌面端视口验收不足

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Responsive QA |
| 优先级 | P1 |
| 当前状态 | Day 4 浏览器检查确认 Home 无横向 overflow。 |
| 缺口 | 没有覆盖整条主链路的移动端、平板、桌面截图验收。 |
| 验收标准 | 至少验证 Home、Stented、Hancock II、Size 21、THV Current、Placement 在移动端和桌面端无错位、无横向滚动、底部 nav 不遮挡关键内容。 |

### GAP-017: 可访问性验收不足

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Accessibility |
| 优先级 | P2 |
| 当前状态 | 代码已有 skip link、modal 结构、部分 aria 和 focus trap。 |
| 缺口 | 没有形成键盘可达、焦点可见、dialog 关闭、按钮 label 的验收清单。 |
| 验收标准 | 建立 accessibility checklist，并覆盖 Home、Stented、THV selector、Video overlay、Quick Selector。 |

### GAP-018: 本地服务和端口流程仍需固化

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Dev Workflow |
| 优先级 | P2 |
| 当前状态 | Day 1 已解释 `localhost`、`14712`、`start.sh`、Python 缺失、`npx serve` 替代方案。 |
| 缺口 | 仍需固化成项目级运行说明，避免不同线程记录不同路径或启动方式。 |
| 验收标准 | 明确当前真实项目目录、推荐启动命令、端口占用处理、Node 验证方式。 |

### GAP-019: 部署验收尚未覆盖

| 字段 | 内容 |
| --- | --- |
| 类型 | Gap / Deployment |
| 优先级 | P2 |
| 当前状态 | 本周主要是本地项目理解、文档、最小代码保护和本地验证。 |
| 缺口 | 未验证静态部署后的资源路径、hash route、控制台错误、刷新行为。 |
| 验收标准 | 线上或静态托管环境可访问；刷新不丢页面；资源路径正确；控制台无关键错误。 |

## 5. 建议修复顺序

### 第一批: 阻塞主链路验收

1. BUG-001: 修复 Plan -> Placement 可见入口。
2. GAP-002: 补真实点击 E2E。
3. GAP-013: 建立 Phase 1 route matrix。
4. GAP-015: 建立 page status tracker。

### 第二批: 阻塞证据闭环

1. GAP-004: 重验 Lotus。
2. GAP-005: 重验 Myval / Archived。
3. GAP-006: 补 Video overlay 打开/关闭/返回证据。
4. GAP-011: 建立截图链逐页映射表。
5. GAP-012: 归档 Web 验收截图。

### 第三批: 扩展完整 App 范围

1. GAP-003: 保护 `#/category/stented`。
2. GAP-008: 梳理一级入口深层页面。
3. GAP-009: 决定 Stented full catalog 范围。
4. GAP-014: 建立术语表。

### 第四批: 工程和体验稳态

1. BUG-002: 固化 Node 验证方式。
2. BUG-006: 回归 native shell 宽度改动影响。
3. GAP-016: 做多视口验收。
4. GAP-017: 做可访问性验收。
5. GAP-019: 做部署验收。

## 6. 当前验收口径

可以声明:

- 第二周已经完成项目结构、主链路、证据链和执行计划梳理。
- 已完成 `#/ -> Stented` 起点保护。
- 当前 route / navigation 测试通过。
- 当前 331 条 route 可渲染，失败 0。

不能声明:

- 不能声明完整 App 复刻完成。
- 不能声明整条主链路截图级复刻完成。
- 不能声明 Lotus / Myval / Archived / Video overlay 已全部高保真闭环。
- 不能把自动化测试通过等同于视觉验收通过。
