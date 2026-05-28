# Day 4 Change Plan

## 1. 我今天选择的最小任务是什么？

今天选择的最小任务是：

**Home 主链路入口和基础壳层的可验证保护。**

范围只限定在主链路起点：

```text
#/ -> Stented
```

这个任务不是完整复刻整条主链路，也不是重做 Home 视觉设计。它的目标是让 `#/` 页面上的 `Stented` 主链路入口、bottom tabs 和 Quick Selector 变成可验证、可防回归的基线。

## 2. 为什么选择这个任务？

根据 `day3-main-flow-replica-spec.md`，主链路从：

```text
Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay
```

开始。

选择 Home 起点作为最小任务，原因是：

- 它是整条主链路的入口，破坏后后续页面即使存在也无法从主流程进入
- 改动边界最小，主要影响 `#/`
- 可以先锁住 `Stented` 入口、bottom tabs 和 Quick Selector
- 不需要改医学内容、设备数据或深链页面
- 可以用自动化测试明确证明没有误进 fallback、没有丢失主链入口

## 3. 预计改哪些文件？

### `scripts/pages/home.js`

预计改动：

- 给 Home 主分类区增加稳定标记
- 将 `Stented` 显式标记为主链路入口

目的：

- 让 `#/` 上的主链路入口可以被测试和 review 明确识别

### `styles/app.css`

预计改动：

- 只调整 Home / native shell 相关的宽度和防溢出约束
- 保证移动端宽度下不出现横向滚动
- 不做全站视觉重设计

目的：

- 满足 spec 中 route-backed 主链页面在移动端不横向滚动的要求

### `tests/navigation.test.mjs`

预计改动：

- 增加或收紧 Home 基线断言

需要验证：

- Home 不进入 fallback
- Home 存在主链路入口区
- `Stented` 入口存在并指向 `#/category/stented`
- `Stented` 是唯一显式主链路入口
- bottom tabs 仍存在
- Quick Selector 仍存在
- Home 仍运行在 native screenshot shell 中

### `scripts/components.js`

预计处理方式：

- 只确认或复用已有 `nativeRow()` 主链入口标记能力
- 如果现有能力缺失，才做最小补充

边界：

- 不重写 shell
- 不重做 bottom tabs
- 不改变非 Home 页面结构

## 4. 这个改动影响哪些路由？

### 直接影响

- `#/`

### 可能间接影响

- `#/category/stented`

原因是 Home 和 Stented list 都使用 native-style shell，CSS 宽度和壳层约束可能会被共享。

### 不应影响

- `#/valve/hancock-ii`
- `#/valve/hancock-ii/size/21`
- `#/valve/hancock-ii/size/21/thv/current`
- `#/valve/hancock-ii/size/21/thv/current/plan/evolut`
- `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`
- archived / Lotus / utility 后续页面

如果这些路由出现变化，应先判断是否是无意扩大范围，而不是继续顺手修更多页面。

## 5. 不会改哪些内容？

本任务明确不改：

- 不改医学内容
- 不改临床结论
- 不改设备推荐逻辑
- 不改 `scripts/data/valves.js`
- 不改 Hancock II 详情页
- 不改 Size 21 页面
- 不改 THV Current 页面
- 不改 Evolut plan 页面
- 不改 Placement 页面
- 不改 Video overlay
- 不补 Lotus 缺口链路
- 不补 archived 全量闭环
- 不改 utility 页面
- 不重做路由系统
- 不做全站视觉优化或现代化重设计

## 6. 计划如何验证？

### 自动化验证

在仓库根目录运行：

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

期望结果：

- 三条命令全部通过
- `#/` 不进入 fallback
- `Stented` 主链入口存在且唯一
- bottom tabs 存在
- Quick Selector 存在
- 主链路 routes 没有被破坏

### 手动验证

打开：

```text
http://localhost:14712/#/
```

检查：

- Home 页面正常渲染
- `Stented` 可见且可点击
- 点击 `Stented` 后进入 `#/category/stented`
- bottom tabs 可见
- Quick Selector 可见
- 移动端宽度下没有横向滚动
- 页面没有 fallback 文案

### 验证失败时的处理原则

如果验证失败，先解释失败原因：

- 是测试断言失败
- 是路由输出不符合预期
- 是本地环境无法执行 `node`
- 还是页面视觉/布局问题

不要因为一个失败直接扩大修复范围；只修与 Home 主链路入口和基础壳层相关的问题。
