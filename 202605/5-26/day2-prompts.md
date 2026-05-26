# Day 2 关键 Codex Prompts 与关键结论

整理目标：沉淀今天最值得复用的提问方式，方便后续继续让 Codex 帮我们梳理 ViV Aortic 的页面结构、证据链和复刻缺口。

## Prompt 1

```text
请阅读 work/docs/page-map-agent.md、work/docs/capture-manifest.md、work/docs/supervisor-checklist.md。
先不要改代码。
请输出 ViV Aortic App 的信息架构：顶层入口、分类浏览层、工具与信息层。
如果某个文件不存在，请先告诉我应该用 work/docs/ 下哪个实际文件替代。
```

关键结论：

- ViV Aortic 的信息架构可分成 3 层：顶层入口、分类浏览层、工具与信息层。
- 顶层入口包括 `Home`、底部 Tab、首页工具入口、右下角 `Quick Selector`。
- 分类浏览层的核心是 `Stented / Stentless / Sutureless / Rings / TAVR`。
- 其中最稳的主链是：`Home -> Stented -> Hancock II -> Size 21 -> THV Current -> Evolut -> Video`。

## Prompt 2

```text
请根据截图和项目资料输出主链路证据表。
格式：截图文件 / 页面或状态 / 入口路径 / 对应 Web 路由 / 是否已实现 / 是否已验证 / 风险。
不要凭空补内容，必须写证据来源。
```

关键结论：

- 主链路截图证据表的作用，是把“原 App 怎么一路点进去”变成一张可验收、可核对、可追责的对账表。
- 它证明的不只是页面长什么样，还包括页面之间是否能串起来、截图是否能对应到唯一页面、Web 路由是否能和原 App 对上。
- 在当前项目里，最稳的证据链是 `首页.png -> stented页.png -> hancock-II首页图.png -> hancock-II-21尺寸图.png -> hancock-II-21尺寸的THVCurrent.png -> hancock-II-21尺寸-Evolut23的瓣膜.png -> hancock-II-21尺寸-23的瓣膜的示例视频.png`。

## Prompt 3

```text
请输出当前完整 App 复刻缺口清单。
按高风险、中风险、低风险分类。
每个缺口都要写证据来源，不要凭空猜测。
格式：优先级、缺口、证据来源、影响页面、风险类型、建议处理方式。
```

关键结论：

- “复刻缺口”不是单纯“没做”，而是“原 App 里应该有的东西，我们还没有证据证明已经完整、正确地复刻了”。
- 当前最高优先级的缺口有 4 类：
  `原生 App 全量页面清单未建立`
  `非 screenshot 范围页面仍有明显采集盲区`
  `Lotus 链路存在证据冲突`
  `视频状态不能只靠按钮或 metadata 验收`
- 缺口既可能是医学内容风险，也可能是 UI / 交互风险，很多时候两者都有。

## Prompt 4

```text
请依据今天的内容，整理完整 App 页面总表。
至少包含：模块、至少覆盖、当前状态、证据来源、备注。
重点覆盖：顶层入口、底部 Tab、首页工具、Stented 主链路、More 栈、Identify / Fracture、Quick Selector、视频状态。
```

关键结论：

- 完整 App 页面总表的作用，是把“这个 App 至少应该有哪些页面和模块”一次性盘清。
- 它和主链路证据表的区别是：
  页面总表关注“应该覆盖哪些页面模块”；
  主链路证据表关注“这些截图和路由怎么一一对应”。
- 当前最稳闭合的是 `Home + Stented 主链`，而 `Stentless / Sutureless / Rings / TAVR / Case of the Month / More 深层页 / Identify / Fracture 深层页` 还需要继续补齐。

## 今天最值得记住的 3 个判断

1. `已实现` 不等于 `高保真`。
   页面能打开、路由存在，只能说明做出来了；只有当页面、数据、素材、交互状态都和原 App 对得上，才更接近高保真。

2. `截图像` 不等于 `页面对`。
   必须能回答：这张图对应哪个页面、哪个状态、哪个路由、哪份数据、哪组素材。

3. `Web 路由很多` 不等于 `完整 App 已复刻`。
   只有当页面总表、截图证据表、缺口清单三者都能闭环，才有资格说完整 App 复刻在推进。
