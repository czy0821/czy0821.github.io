# week2-thread-3-summary.md

## 1. 目标

- 梳理 `vie-aorta-master` 的主链路，明确复刻范围。
- 基于截图证据和现有代码，产出可执行的主链路复刻 spec、文件映射和 implementation plan。
- 在不直接改业务代码的前提下，把“能不能执行、怎么执行、如何验收”先定义清楚。

## 2. 已完成

- 明确主链路为：
  `Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Placement -> Video overlay`
- 阅读并分析了主链路相关代码与数据：
  - `D:\学习\vie-aorta-master\scripts\router.js`
  - `D:\学习\vie-aorta-master\scripts\pages\home.js`
  - `D:\学习\vie-aorta-master\scripts\pages\category.js`
  - `D:\学习\vie-aorta-master\scripts\pages\valve-detail.js`
  - `D:\学习\vie-aorta-master\scripts\pages\size-detail.js`
  - `D:\学习\vie-aorta-master\scripts\pages\thv-selector.js`
  - `D:\学习\vie-aorta-master\scripts\pages\thv-plan.js`
  - `D:\学习\vie-aorta-master\scripts\pages\thv-placement.js`
  - `D:\学习\vie-aorta-master\scripts\data\valves.js`
- 产出了主链路文件表、复刻 spec、implementation plan 和 prompt 复用文档。
- 对用户自己的 spec 做了 review，并根据 review 结果收紧了：
  - `plan/evolut` 的口径
  - `placement` 的证据边界
  - `video overlay` 的验收要求
  - “每个主链路页面必须有路由 / 文件 / 数据或素材来源”的映射要求
- 判断并确认：
  - 当前 implementation plan 在任务设计层面可以执行
  - 但真正开始改 `D:\学习\vie-aorta-master` 代码前，仍需要对目标仓库的写权限

## 3. 关键结论

- 今天的核心学习内容不是“直接画页面”，而是“如何用 Codex 有控制地复刻 App 主链路”。
- 正确顺序是：
  `找主链路 -> 找证据 -> 写 spec -> review 风险 -> 拆 implementation plan -> 再执行`
- `Video overlay` 不是独立页面路由，而是基于 placement 页触发的交互状态；因此 spec 里必须写成“路由基座 + 状态 + 实现文件 + 数据来源”。
- 一份合格的复刻 spec，至少要满足：
  1. 每个主链路页面都有对应路由
  2. 每个主链路页面都有对应实现文件
  3. 每个主链路页面都有数据来源或素材来源
  4. spec 里明确写清楚“不做什么”
- 当前这版 spec 已满足以上 4 条，比旧版更适合作为后续执行基线。
- 当前这版 implementation plan 比旧版更可执行，因为任务边界更小，并且和最新 spec 对齐。

## 4. 文件改动

### 本地工作区新增 / 更新

- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\main-flow-replica-spec.md`
- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\main-flow-file-map.md`
- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\implementation-plan.md`
- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\day3-main-flow-replica-spec.md`
- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\day3-prompts.md`
- `C:\Users\Administrator\Documents\Codex\2026-05-27\1-spec-2-3-4-5\week2-thread-3-summary.md`

### 已同步到用户目录的文档

- `D:\学习\czy0821.github.io\202605\5-27\day3-main-flow-replica-spec.md`
- `D:\学习\czy0821.github.io\202605\5-27\day3-main-flow-file-map.md`
- `D:\学习\czy0821.github.io\202605\5-27\day3-implementation-plan.md`
- `D:\学习\czy0821.github.io\202605\5-27\day3-prompts.md`

### 未改动的业务代码

- 本线程**没有**直接修改 `D:\学习\vie-aorta-master` 下的业务代码、样式或测试文件。

## 5. 测试结果

- 本线程**没有实际执行** `D:\学习\vie-aorta-master` 中的业务测试或本地服务启动。
- 已明确后续验证命令应为：

```powershell
node tests/routes.test.mjs
node tests/navigation.test.mjs
node scripts/verify-routes.mjs
```

- 已明确后续还需要做手动截图验收，重点覆盖：
  - Home
  - Stented
  - Hancock II
  - Size 21
  - THV Current
  - Evolut screenshot-backed state
  - Video overlay 打开态
  - overlay 返回后的 placement 状态

## 6. 未完成 / bug / gap

- 还没有开始执行 `day3-implementation-plan.md` 中的 5 个实现任务。
- 还没有对 `D:\学习\vie-aorta-master` 进行任何代码改动。
- 还没有运行主链路验证命令，也没有启动本地服务做真实页面验收。
- 还没有完成截图对照和移动端 / 桌面端视口验证。
- 当前所有产物仍以“分析、spec、plan、review”文档为主，属于执行前准备阶段。
- 真正开始实现前仍需要确认：
  - 对 `D:\学习\vie-aorta-master` 的写权限
  - 本地服务启动方式是否可用
  - 截图证据目录是否完整可访问

## 7. 下一步建议

1. 授权对 `D:\学习\vie-aorta-master` 的写入后，按 `D:\学习\czy0821.github.io\202605\5-27\day3-implementation-plan.md` 从任务 1 开始执行。
2. 每完成一个任务，立即运行：
   - `node tests/routes.test.mjs`
   - `node tests/navigation.test.mjs`
   - `node scripts/verify-routes.mjs`
3. 每完成一个关键页面后，补做对应截图比对，不要等到所有任务都完成后再统一验收。
4. 优先盯紧最容易跑偏的 3 个点：
   - `plan/evolut` 不要被实现成自由发挥的新页面
   - `placement` 要保持 continuity 状态语义
   - `video overlay` 不能只做成静态海报弹层
5. 实现阶段若新增结论或踩坑，建议继续把变更同步回：
   - `day3-main-flow-replica-spec.md`
   - `day3-implementation-plan.md`
   - `day3-prompts.md`
