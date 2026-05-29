# codex-replica-prompts.md

## 1. 项目阅读 prompt

### 1.1 项目全局阅读

```text
请先阅读当前 ViV Aortic Web 复刻项目，不要改代码。

必须阅读：
- README.md
- work/docs/page-map-agent.md
- work/docs/capture-manifest.md
- work/docs/supervisor-checklist.md
- work/docs/viv-aortic-native-reference.md
- work/docs/week2-viv-aortic-review.md
- work/docs/week2-bug-gap-list.md
- work/docs/full-app-replica-roadmap.md

请输出：
1. 当前项目是什么架构
2. 当前主链路是什么
3. 当前已有证据有哪些
4. 当前已实现到什么程度
5. 当前最重要的缺口
6. 哪些结论是旧快照，不能直接沿用

要求：
- 不要把 route 数量等同于完整 App 复刻。
- 明确说明 `325 routes` 是旧快照，当前基线是 `331 routes / 331 unique routes`。
- 明确说明 Lotus 数据已补，但截图和 E2E 证据未闭环。
```

### 1.2 代码结构阅读

```text
请阅读项目代码结构，重点理解 Web SPA 的渲染链路，不要改代码。

重点文件：
- index.html
- scripts/app.js
- scripts/router.js
- scripts/components.js
- scripts/pages/home.js
- scripts/pages/category.js
- scripts/pages/valve-detail.js
- scripts/pages/size-detail.js
- scripts/pages/thv-selector.js
- scripts/pages/thv-plan.js
- scripts/pages/thv-placement.js
- scripts/pages/guidance.js
- scripts/pages/utility.js
- scripts/data/categories.js
- scripts/data/utilities.js
- scripts/data/valves.js
- styles/app.css
- tests/navigation.test.mjs
- tests/routes.test.mjs

请输出：
1. route 如何分发
2. 页面如何读取 data
3. 组件如何复用
4. video / image / quick selector overlay 如何工作
5. 当前测试覆盖了哪些层
6. 后续改动最容易影响哪些文件
```

## 2. 页面地图 prompt

### 2.1 生成原生 App 页面地图

```text
请根据现有证据更新 ViV Aortic 原生 App 页面地图。

读取：
- work/docs/page-map-agent.md
- work/docs/capture-manifest.md
- work/docs/viv-aortic-native-reference.md
- work/docs/full-app-replica-roadmap.md

请输出页面地图，至少包含：
- Home
- 底部 Tab: Stented / Stentless / Sutureless / Rings / TAVR
- 工具入口: Bookmarks / Case of the Month / Identify a Valve / Valve Fracture / More
- Quick Selector
- Stented 主链: Archive -> Detail -> Size -> THV Current / Archived -> Plan -> Placement -> Video
- Stentless / Sutureless / Rings / TAVR 的推断深层结构

要求：
- 标注证据等级：真实截图 / bundle token / Web 数据 / 推断。
- 不确定关系必须标注“待确认”。
```

### 2.2 生成 Web route 地图

```text
请根据当前代码生成 Web route 地图。

读取：
- scripts/router.js
- scripts/data/categories.js
- scripts/data/utilities.js
- scripts/data/valves.js
- tests/routes.test.mjs
- tests/navigation.test.mjs

请输出：
1. 当前所有顶层 route 类型
2. Stented demo route 结构
3. Hancock II Size 21 主链 route
4. Current / Archived / Myval / Lotus / Guidance 分支 route
5. Utility route
6. fallback route
7. 当前 route count 基线

要求：
- 当前基线按 331 routes / 331 unique routes 说明。
- 标注哪些 route 只是 Web demo，不等于原生全量页面。
```

## 3. 截图证据 prompt

### 3.1 更新截图清单

```text
请更新原生截图证据清单。

读取：
- work/docs/capture-manifest.md
- work/docs/viv-aortic-native-reference.md
- work/docs/week2-bug-gap-list.md

请检查并更新：
- 已采集截图
- 过程截图
- 主链截图关系
- 工具入口截图关系
- 仍缺的截图

必须标注：
- Home
- Stented list
- Hancock II detail
- Size 21
- THV Current
- THV Archived
- Evolut placement
- Myval plan / placement
- Lotus plan / placement
- Video overlay
- Bookmarks
- More
- Identify
- Quick Selector

要求：
- 区分“已真实采集”和“只有旧截图 / bundle 推断”。
- 不要把缺失截图写成已完成。
```

### 3.2 建立截图到 route 映射

```text
请建立 screenshot-to-route-to-data-to-asset-to-test 映射表。

读取：
- work/docs/capture-manifest.md
- work/docs/viv-aortic-native-reference.md
- work/assets-inventory/asset-map-agent.md
- scripts/data/valves.js
- tests/routes.test.mjs

输出表格列：
- screenshot
- 原生页面 / 状态
- Web route
- 入口路径
- data node
- asset
- 测试覆盖
- 当前状态
- 缺口

要求：
- 覆盖主链和工具入口。
- Lotus 必须写成“数据已补，截图 / E2E 未闭环”。
- 没有证据的项目标注 missing / pending。
```

## 4. spec 编写 prompt

### 4.1 编写页面复刻 spec

```text
请为指定页面编写复刻 spec。

目标页面：
[在这里填写页面，例如 Hancock II detail / Size 21 / THV Current / Lotus placement / More]

请先读取：
- work/docs/page-map-agent.md
- work/docs/capture-manifest.md
- work/docs/viv-aortic-native-reference.md
- docs/superpowers/specs/*
- 当前相关 scripts/pages/*
- 当前相关 scripts/data/*

spec 必须包含：
1. 页面目标
2. 原生证据
3. Web 当前状态
4. 页面结构
5. 数据字段
6. 素材和视频
7. 交互行为
8. fallback / pending 状态
9. 测试要求
10. 不做范围

要求：
- 不写营销页，不改成 dashboard。
- 页面要贴近原生 App 信息结构。
```

### 4.2 编写功能链路 spec

```text
请为指定功能链路编写 spec。

目标链路：
[在这里填写链路，例如 Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut placement -> Video]

请输出：
1. 链路入口
2. 每一步 route
3. 每一步原生截图或证据
4. 每一步 Web 当前状态
5. 必须可点击的控件
6. 页面间返回行为
7. 视频 / 图片 overlay 行为
8. 数据和 asset 来源
9. 测试计划
10. 验收标准

要求：
- 不能只写 route 可达，必须写用户点击路径。
- 对缺截图的步骤明确标注 gap。
```

## 5. implementation plan prompt

### 5.1 编写小步实现计划

```text
请为下面任务编写 implementation plan，不要直接改代码。

任务：
[填写任务，例如补 Lotus placement 点击闭环 / 补 Quick Selector 跳转 / 补 More 子页]

请先读取：
- work/docs/full-app-replica-roadmap.md
- work/docs/week2-bug-gap-list.md
- 相关 scripts/pages/*
- 相关 scripts/data/*
- 相关 tests/*

plan 必须包含：
1. 目标
2. 当前状态
3. 涉及文件
4. 数据改动
5. 页面改动
6. 样式改动
7. 测试改动
8. 验收命令
9. 风险
10. 不做范围

要求：
- 每一步都要小，能单独验证。
- 不要把无证据页面伪装成 complete。
```

### 5.2 编写一周执行计划

```text
请根据当前路线图编写下周执行计划。

读取：
- work/docs/full-app-replica-roadmap.md
- work/docs/week2-bug-gap-list.md
- work/docs/week2-viv-aortic-review.md

请按优先级输出：
- P0
- P1
- P2

每个任务包含：
- 目标
- 文件范围
- 验收标准
- 依赖证据
- 预计产物

必须包含：
- 主链 E2E
- screenshot-to-route 映射表
- Web 截图归档
- Lotus / Myval / Archived 证据闭环
- Video overlay 可视证据
- 一级入口骨架
```

## 6. 小步开发 prompt

### 6.1 执行单个页面改动

```text
请执行一个小步页面改动。

目标：
[填写目标，例如让 Plan 页面有可见 Ideal Placement 入口]

限制：
- 只改必要文件。
- 不做无关重构。
- 不改无关页面。
- 保留现有 route fallback。
- 保留 Home 的 `data-home-main-flow` 和 `data-main-flow-entry="stented"`。

请执行：
1. 阅读相关页面和数据文件
2. 实现最小改动
3. 补或更新测试
4. 运行相关验证
5. 汇报改动文件、验证结果、仍缺证据
```

### 6.2 执行单个数据补齐

```text
请执行一个小步数据补齐。

目标：
[填写目标，例如补 Myval placement media / 补 More 子页条目 / 补 Stentless 代表 valve]

要求：
- 数据必须结构化写入 `scripts/data/*`。
- 图片、poster、video metadata 必须有来源或 fallback 标记。
- 没有原生证据时标注 pending / fallback，不要伪装 exact。

请执行：
1. 找到现有数据结构
2. 按现有 schema 补数据
3. 更新页面渲染需要的字段
4. 增加测试断言
5. 输出验证结果
```

## 7. review 和验证 prompt

### 7.1 代码 review prompt

```text
请对当前改动做 code review。

重点检查：
- 是否破坏主链 route
- 是否破坏 Home 主入口
- 是否误把 fallback 当真实页面
- 是否误把 331 routes 当完整 App 复刻
- Lotus / Myval / Archived 是否真的闭环
- Video overlay 是否有真实交互证据
- 图片和 video metadata 是否有来源
- 测试是否覆盖新行为

输出格式：
1. Findings，按严重程度排序
2. Open questions
3. Missing tests
4. Evidence gaps
5. 是否可以验收
```

### 7.2 验证 prompt

```text
请执行本轮验证并给出结果。

必须检查：
- tests/navigation.test.mjs
- tests/routes.test.mjs
- scripts/verify-routes.mjs
- 主链关键 route
- Lotus placement route
- Video overlay contract
- Quick Selector contract
- fallback route

输出：
1. 实际运行的命令
2. PASS / FAIL
3. 失败日志摘要
4. 当前 route count
5. 未能运行的验证和原因
6. 需要补的测试

要求：
- 如果 `node scripts/verify-routes.mjs` 不能稳定运行，要明确说明环境问题。
- 不要用“看起来可以”代替测试结果。
```
