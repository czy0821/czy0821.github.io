# Week 2 Thread 4 Summary

## 1. 目标

本轮目标是根据 `day3-main-flow-replica-spec.md` 和 implementation plan，选择并实现一个最小任务。

最终选择的最小任务是：

```text
Home 主链路入口和基础壳层的可验证保护
```

任务边界限定为：

```text
#/ -> Stented
```

本轮不做整条主链路复刻，不重写页面，不改医学内容，不改深链页面。

## 2. 已完成

已完成以下工作：

- 选择最小任务：保护 Home 起点 `#/ -> Stented`
- 实现 Home 主链路入口 DOM 标记
- 将 `Stented` 标记为唯一显式主链路入口
- 扩展 `nativeRow()`，支持主链入口标记
- 增加 Home/navigation 基线测试
- 修复 Home/native shell 轻微横向 overflow 风险
- 对照 spec 做 review 和自查
- 生成以下文档：
  - `day4-change-plan.md`
  - `day4-change-summary.md`
  - `day4-review-notes.md`
  - `day4-verification-result.md`
  - `day4-prompts.md`
  - `代码改动.md`

## 3. 关键结论

本轮改动的核心不是视觉重做，而是基线保护。

页面可见变化很小，主要变化在：

- Home 中明确标记主链路入口区
- `Stented` 被明确标记为主链入口
- 测试能防止以后误删、误改 `Stented` 入口
- 测试能防止 Home 掉入 fallback
- bottom tabs 和 Quick Selector 被纳入 Home 基线保护
- native shell 宽度被约束，降低移动端横向滚动风险

一句话总结：

```text
今天把首页的 Stented 主入口上了保险，确保 #/ -> Stented 不容易被改坏。
```

## 4. 文件改动

### `scripts/pages/home.js`

改动：

- 给 Home 主分类区增加 `data-home-main-flow`
- 给 `Stented` row 传入 `mainFlowEntry: "stented"`

目的：

- 明确标记 `#/ -> Stented` 主链路起点

### `scripts/components.js`

改动：

- `nativeRow()` 增加 `mainFlowEntry` 参数
- 条件输出 `native-row--main-flow`
- 条件输出 `data-main-flow-entry="stented"`

目的：

- 为 Home 主链入口提供稳定 DOM contract

### `styles/app.css`

改动：

- 收紧 `--native-shell-width`
- 给 native topbar、screen、bottom tabs 增加 `max-width`
- 增加 `.page--home .native-row--main-flow` 轻量样式

目的：

- 避免 Home/native shell 在移动端出现横向滚动

### `tests/navigation.test.mjs`

改动：

- 增加 Home 基线断言：
  - Home 不进入 fallback
  - 存在 `data-home-main-flow`
  - 存在 `href="#/category/stented"`
  - 存在 `data-main-flow-entry="stented"`
  - `data-main-flow-entry` 只出现一次
  - bottom tabs 存在
  - Quick Selector 存在
  - native shell 存在

目的：

- 防止 `#/ -> Stented` 主链路起点被破坏

## 5. 测试结果

计划验证命令：

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

PowerShell 直接运行时出现：

```text
Program 'node.exe' failed to run: Access is denied
```

判断：

- 这是本地执行环境无法启动 `node.exe` 的权限问题
- 不是测试断言失败
- 不是 routes 被破坏

随后使用同一 Node 运行环境直接加载测试文件完成验证。

最终结果：

```text
tests/routes.test.mjs: PASS
tests/navigation.test.mjs: PASS
scripts/verify-routes.mjs: PASS
PASS verify-routes.mjs (331 routes)
```

浏览器补充检查结果：

```text
hasHome: true
hasStentedMainEntry: true
mainFlowEntryCount: 1
hasBottomTabs: true
hasQuickSelector: true
hasFallback: false
horizontalOverflow: false
```

## 6. 未完成 / bug / gap

未完成：

- 没有完成整条主链路截图级复刻
- 没有处理 `Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay` 的完整视觉 fidelity
- 没有做 Video overlay 交互闭环的新实现
- 没有补 Lotus / archived 全量链路

已知 gap：

- 自动化测试能证明路由和 DOM contract，不等于证明截图 fidelity
- `work/captures/raw/001-home-tab.png` 等 raw capture 文件位置需要确认
- `styles/app.css` 的 native shell 宽度调整可能间接影响其他 native shell 页面，需要后续抽查

环境问题：

- PowerShell 中直接执行 `node` 出现 `Access is denied`
- 该问题需要后续单独处理，以免每次验证都需要绕行

## 7. 下一步建议

建议下一个最小任务继续从 spec 中窄范围选择。

优先建议：

1. 选择 `#/category/stented` 列表页作为下一个最小任务
2. 先不要改代码，先说明：
   - 要改哪些文件
   - 为什么改
   - 可能影响哪些路由
   - 不会改哪些内容
   - 如何验证
3. 抽查本次 native shell 宽度约束对以下页面的影响：
   - `#/category/stented`
   - `#/valve/hancock-ii`
   - `#/valve/hancock-ii/size/21`
   - `#/valve/hancock-ii/size/21/thv/current`
4. 处理本地 `node.exe` 执行权限问题
5. 若进入正式视觉验收，补齐或确认 raw capture 文件路径

继续任务时应保留的约束：

```text
只实现一个最小任务。
不要重写无关页面。
不要改动医学内容。
不要扩大范围。
如果验证失败，先解释失败原因，不要直接扩大修复范围。
```
