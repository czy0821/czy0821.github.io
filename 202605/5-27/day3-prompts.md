# day3-prompts.md

## 说明

下面整理的是今天对话里最关键、可复用的 Codex prompt，以及每条 prompt 对应产出的关键结论。
这些 prompt 都偏向主链路梳理、复刻 spec、implementation plan 和执行前检查。

## Prompt 1：定位主链路事实链

**可复用 prompt**

```text
请阅读主链路相关页面文件和 scripts/data/valves.js。
先不要改代码。
请输出 Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut Plan -> Placement -> Video overlay
每一步涉及的路由、页面文件、数据节点、图片素材和测试覆盖。
```

**关键结论**

- 主链路可以稳定落到这条路径：
  `#/ -> #/category/stented -> #/valve/hancock-ii -> #/valve/hancock-ii/size/21 -> #/valve/hancock-ii/size/21/thv/current -> #/valve/hancock-ii/size/21/thv/current/plan/evolut -> #/valve/hancock-ii/size/21/thv/current/plan/evolut/placement`
- `Video overlay` 不是独立 hash 路由，而是基于 `placement` 页面打开后的交互状态。
- 主链路核心页面文件包括：
  `scripts/pages/home.js`
  `scripts/pages/category.js`
  `scripts/pages/valve-detail.js`
  `scripts/pages/size-detail.js`
  `scripts/pages/thv-selector.js`
  `scripts/pages/thv-plan.js`
  `scripts/pages/thv-placement.js`
- `router.js` 是主链路控制文件，`guidance.js` 是旁支，不属于主链路直行页。

## Prompt 2：生成复刻 spec

**可复用 prompt**

```text
请根据主链路截图证据表和现有代码，整理一份 main-flow-replica-spec。
要求包含：目标、非目标、页面清单、验收标准、风险、验证命令。
请注意：这是复刻 spec，不是随意优化建议。
```

**关键结论**

- 这是“复刻 spec”，不是产品优化单，也不是自由发挥的 UI 改版建议。
- spec 里必须把“不做什么”写清楚，否则 Codex 很容易顺手扩大范围。
- 每个主链路步骤都应该明确到：
  - 对应路由，或“路由基座 + 交互状态”
  - 对应实现文件
  - 对应数据来源或素材来源
- `Video overlay` 必须被当成主链路中的交互状态来定义，不能因为它没有独立路由就被漏掉。

## Prompt 3：review 自己的 spec

**可复用 prompt**

```text
review 我自己的 spec，指出风险。
请优先指出会让实现或验收跑偏的地方。
```

**关键结论**

- 最大风险不是“有没有内容”，而是“边界有没有写硬”。
- `plan/evolut` 不能被轻率理解成一个已被原生截图单独证明的全新页面；更准确的口径是：
  当前 web 路由中的可见状态，对应 native 的 `Evolut Ideal Placement` 证据。
- `Placement` 在现有证据里更像 continuity 状态，而不是一个已经有独立 native 首屏基线的页面。
- `Video overlay` 不能只验“有个海报弹层”，而要验：
  - 打开方式
  - 播放器结构
  - 关闭返回后回到 placement
- 现有自动化测试主要证明路由、metadata 和 overlay shell 还在，不能单独证明截图 fidelity 完成。

## Prompt 4：把实现拆成最小任务

**可复用 prompt**

```text
请根据 main-flow-replica-spec，把后续实现拆成 5 个最小任务。
生成 implementation plan。
每个任务必须说明：要改哪些文件、为什么改、如何验证。
任务必须小到一天内能完成。
```

**关键结论**

- implementation plan 不能只是“按页面分一分”，还要保证每个任务都是一天内可完成的粒度。
- 过大的任务会直接降低可执行性，尤其是把多个页面入口和共享壳层绑在一起时。
- 更稳的拆法是：
  1. Home
  2. Stented
  3. Hancock II detail
  4. Size 21 + THV Current
  5. Evolut state + Placement continuity + Video overlay
- 每个任务都必须同时写清楚：
  - 改哪些文件
  - 为什么是这些文件
  - 用什么命令和什么截图来验证

## Prompt 5：检查 implementation plan 能不能直接执行

**可复用 prompt**

```text
此 implementation plan 是否可以直接执行。
如果可以，请帮我执行。
如果不可以，请帮我修改。
```

**关键结论**

- “能不能执行”要分两层判断：
  - 任务拆分本身是否足够具体
  - 当前权限环境是否允许真的去改目标仓库
- 如果任务边界过大，或者 implementation plan 还沿用旧 spec 口径，就不该直接开工。
- 即使 plan 已经可执行，真正实施前仍要确认：
  - 目标仓库是否可写
  - 验证命令是否可运行
  - 最深链路任务是否与最新 spec 保持一致

## 今日最重要的总结合集

- 先找主链路，再写 spec，再拆 implementation plan，这个顺序是对的。
- 对“复刻型任务”来说，`非目标` 和 `约束条件` 跟 `目标` 一样重要。
- `Video overlay` 这类没有独立路由的交互状态，必须被显式写进 spec 和文件映射里。
- review spec 时，最值得盯的不是文案是否漂亮，而是有没有留下让 Codex 自由脑补的口子。
- implementation plan 只有在“任务够小、路径够清、验证够具体、权限够用”时，才算真正可以执行。
