# Day 4 Change Summary

## 1. 实际改了哪些文件？

实际改动文件：

- `scripts/pages/home.js`
- `scripts/components.js`
- `styles/app.css`
- `tests/navigation.test.mjs`

新增文档：

- `day4-change-plan.md`
- `day4-change-summary.md`

## 2. 每个文件改了什么？

### `scripts/pages/home.js`

改动内容：

- 给 Home 主分类 section 增加 `data-home-main-flow`
- 给 `Stented` 分类 row 传入 `mainFlowEntry: "stented"`

作用：

- 明确标记 `#/` 页面上的主链路入口区
- 让 `Stented` 入口可以被自动化测试稳定识别

### `scripts/components.js`

改动内容：

- `nativeRow()` 增加 `mainFlowEntry` 参数
- 当 `mainFlowEntry` 存在时，输出：
  - `native-row--main-flow`
  - `data-main-flow-entry="stented"`

作用：

- 为 Home 主链入口提供稳定 DOM contract
- 让测试可以确认 `Stented` 是唯一显式主链路入口

### `styles/app.css`

改动内容：

- 收紧 `--native-shell-width`
- 给 native topbar、native screen、native bottom tabs 增加 `max-width`
- 给 Home 主链入口增加轻量样式 `.page--home .native-row--main-flow`

作用：

- 避免 Home/native shell 在移动端出现横向滚动
- 保持 Home 主链入口在 native shell 中稳定呈现

### `tests/navigation.test.mjs`

改动内容：

- 增加 Home 主链路入口断言：
  - Home 不进入 fallback
  - Home 包含 `data-home-main-flow`
  - Home 包含 `href="#/category/stented"`
  - Home 包含 `data-main-flow-entry="stented"`
  - `data-main-flow-entry` 只出现一次
  - Home 保留 bottom tabs
  - Home 保留 Quick Selector
  - Home 保持 native screenshot shell

作用：

- 将 Home 主链路入口和基础壳层变成可验证基线
- 防止后续改动误删或误改 `Stented` 入口

## 3. 是否与原计划一致？

一致。

本次实际改动符合 `day4-change-plan.md` 的最小任务边界：

- 只保护 `#/ -> Stented` 这个主链路起点
- 没有改医学内容
- 没有改 `scripts/data/valves.js`
- 没有改 Hancock II、Size 21、THV Current、Evolut、Placement 或 Video overlay 页面内容
- 没有补 Lotus 或 archived 全量闭环
- 没有重做路由系统
- 没有做全站视觉重设计

唯一需要说明的是：

- `scripts/components.js` 最初在计划里是“优先复用，缺失时才最小补充”
- 实际实现中确实对 `nativeRow()` 做了最小补充，用来承载 `mainFlowEntry` 和 `data-main-flow-entry`
- 这个补充仍然服务于 Home 主链路入口保护，没有扩大到其他页面业务逻辑

## 4. 是否有意外问题？

有一个环境问题和一个布局问题。

### 环境问题

在当前 Codex PowerShell 环境中，直接运行：

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

会出现：

```text
Program 'node.exe' failed to run: Access is denied
```

判断：

- 这是本地执行器无法启动 `node.exe` 的权限问题
- 不是测试断言失败
- 不是 routes 被破坏

处理：

- 使用同一 Node 运行环境直接加载测试文件完成验证

验证结果：

```text
tests/routes.test.mjs: PASS
tests/navigation.test.mjs: PASS
scripts/verify-routes.mjs: PASS
```

其中：

```text
PASS verify-routes.mjs (331 routes)
```

### 布局问题

浏览器检查 Home 时发现 native shell 有轻微横向 overflow。

处理：

- 收紧 native shell 宽度
- 给 native shell 相关容器增加 `max-width`

处理后浏览器检查结果：

- `Stented` 主链入口存在
- `data-main-flow-entry` 只出现一次
- bottom tabs 存在
- Quick Selector 存在
- Home 没有 fallback
- 横向滚动已消除

## 5. 是否需要下周继续处理？

需要，但不是继续扩大本任务。

建议下周继续处理的内容：

1. 按 `day3-main-flow-replica-spec.md` 继续选择下一个最小任务，例如 `#/category/stented` 列表页基线保护或截图复刻。
2. 对 `#/category/stented` 和后续主链 native shell 页面做移动端截图抽查，确认本次 shell 宽度约束没有造成间接布局问题。
3. 如果需要严格截图验收，补齐或确认 `work/captures/raw/001-home-tab.png` 等 raw capture 文件位置。
4. 处理本地 `node.exe` 在 PowerShell 中 `Access is denied` 的执行环境问题，避免以后验证需要绕行。

不建议下周直接做的内容：

- 不要把本任务扩大成整条主链路重构
- 不要因为 Home 入口保护已经完成，就顺手改医学数据或深链页面
- 不要绕过新增测试断言来让失败消失
