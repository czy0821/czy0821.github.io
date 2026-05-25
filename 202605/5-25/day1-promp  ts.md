# Day 1: Prompts And Key Conclusions

## Prompt 1: 先读项目，不要直接改

```text
请先不要修改项目。先只读分析这个项目：

- 列出项目结构；
- 判断它是静态 Web SPA、React、Vue、Next 还是其他类型；
- 找到入口文件；
- 找到 CSS 和 JavaScript module 的引用关系；
- 找到路由实现方式，尤其是 hash 路由；
- 建立当前路由基线；
- 说明运行和验证方式；
- 最后给出修改建议和风险点。

在我明确说“开始修改”之前，不要编辑任何文件。
```

关键结论：

```text
让 Codex 先读项目，可以避免它在不了解结构时直接改代码。
最关键的一句话是：在我确认之前，不要修改任何文件。
```

## Prompt 2: 解释项目启动、渲染和 hash 路由

```text
请阅读 README、index.html、start.sh、scripts/app.js、scripts/router.js、scripts/components.js。
先不要改代码。
请用适合零基础学员的方式解释这个项目如何启动、如何渲染页面、如何通过 hash 路由切换页面。
```

关键结论：

```text
这个项目是静态 Web SPA。
start.sh 用 Python 启动本地静态服务器。
index.html 提供 #app 容器。
scripts/app.js 负责读取 hash、调用路由、渲染页面、绑定事件。
scripts/router.js 根据 hash 路由决定显示哪个页面。
```

## Prompt 3: 输出项目结构说明

```text
请输出这个项目的文件结构说明。
要求按入口文件、路由、页面、组件、数据、样式、素材、测试分类说明。
先不要改代码。
```

关键结论：

```text
项目可以按职责拆成：
入口文件、路由、页面、组件、数据、样式、素材、测试。

理解这个分类后，后续复刻页面时就知道该先看哪里、后改哪里。
```

## Prompt 4: 跑验证命令

```text
node 可以用了，继续刚刚的验证。
```

关键结论：

```text
这个项目的三条关键验证命令是：

node scripts/verify-routes.mjs
node tests/routes.test.mjs
node tests/navigation.test.mjs

它们分别检查：
主要路由能不能渲染；
路由、数据、fallback 是否正常；
导航、弹窗、视频/图片交互结构是否正常。
```

## 总结

今天最重要的学习点是：

```text
先读项目，再建立路由基线，最后再改代码。
```

这个项目的核心运行逻辑是：

```text
start.sh 启动本地服务器
index.html 提供 #app 容器
app.js 接管渲染和事件
router.js 根据 hash 选择页面
pages/ 生成页面内容
data/ 提供页面数据
components.js 提供公共 UI
styles/app.css 控制视觉样式
tests/ 保护路由和导航不被改坏
```
