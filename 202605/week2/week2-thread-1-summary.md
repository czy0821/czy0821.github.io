# Week 2 Thread 1 Summary

## 1. 目标

本轮对话的目标是帮助零基础学员理解 `vie-aorta-master` 项目的基本运行机制，并沉淀 Day 1 学习资料。

重点包括：

- 判断项目类型；
- 理解本地服务器、`localhost`、端口、启动命令；
- 理解 `index.html -> scripts/app.js -> scripts/router.js -> scripts/pages/* -> scripts/data/* -> styles/app.css` 的关系；
- 建立主链路、路由基线和页面基线；
- 跑通项目验证命令；
- 生成学习笔记、prompt 笔记、主链路表和 review 交接资料。

## 2. 已完成

已解释并梳理：

- 什么是静态 Web SPA；
- React / Vue / Next 项目与当前项目的区别；
- 什么是本地服务器、`localhost`、hash 路由、挂载；
- 为什么打开 `http://localhost:14712` 前必须先启动本地服务器；
- `chmod +x ./start.sh`、`./start.sh` 的含义；
- Git Bash / PowerShell / VS Code 终端的使用差异；
- 不安装 Python 时，可用 `npx serve . -l 14712` 替代 `start.sh`；
- 端口 `14712` 被占用时如何处理；
- 如何关闭占用 `14712` 的旧服务器。

已只读查看项目压缩包/项目文件：

- `README.md`
- `index.html`
- `start.sh`
- `scripts/app.js`
- `scripts/router.js`
- `scripts/components.js`
- `scripts/pages/*`
- `scripts/data/*`
- `tests/*`
- `scripts/verify-routes.mjs`

已生成学习资料：

- `day1-project-structure-notes.md`
- `day1-route-walkthrough.md`
- `day1-prompts.md`
- `day1-review-notes.md`

已对用户 Downloads 中的四份文档进行 review：

- `cyf-day1-route-walkthrough.md`
- `cyf-day1-prompts.md`
- `cyf-day1-verification-result.md`
- `cyf-day1-project-structure-notes.md`

## 3. 关键结论

这个项目是：

```text
原生 HTML + CSS + ES Module JavaScript 写成的静态 Web SPA
```

它不是：

```text
React / Vue / Next / Vite / Webpack 项目
```

核心运行链路是：

```text
index.html
-> scripts/app.js
-> scripts/router.js
-> scripts/pages/*
-> scripts/data/*
-> styles/app.css
```

职责理解：

- `index.html`：浏览器入口，提供 `<div id="app"></div>` 挂载点，并加载 `scripts/app.js`。
- `scripts/app.js`：App 启动器和事件总管，负责初始化渲染、监听 `hashchange`、绑定搜索/点击/弹层事件。
- `scripts/router.js`：路由分发器，根据 hash 路由决定调用哪个页面渲染函数。
- `scripts/pages/*`：具体页面生成器，比如首页、分类页、详情页、尺寸页、THV 页、Placement 页。
- `scripts/data/*`：页面数据来源，比如分类、瓣膜、尺寸、THV、视频、工具入口。
- `scripts/components.js`：公共 UI 组件和页面外壳，比如 topbar、bottom tabs、modal、video panel。
- `styles/app.css`：全局样式，控制布局、颜色、导航、卡片、弹层和响应式。
- `tests/*` 和 `scripts/verify-routes.mjs`：保护路由、页面结构和导航交互不被改坏。

主链路：

```text
Home
-> Stented
-> Hancock II
-> Size 21
-> THV Current
-> Evolut Plan
-> Placement
-> Video overlay
```

主链路对应文件：

| 页面 | 路由 | 主要负责文件 |
| --- | --- | --- |
| Home | `#/` | `scripts/pages/home.js` |
| Stented | `#/category/stented` | `scripts/pages/category.js` |
| Hancock II | `#/valve/hancock-ii` | `scripts/pages/valve-detail.js` |
| Size 21 | `#/valve/hancock-ii/size/21` | `scripts/pages/size-detail.js` |
| THV Current | `#/valve/hancock-ii/size/21/thv/current` | `scripts/pages/thv-selector.js` |
| Evolut Plan | `#/valve/hancock-ii/size/21/thv/current/plan/evolut` | `scripts/pages/thv-plan.js` |
| Placement | `#/valve/hancock-ii/size/21/thv/current/plan/evolut/placement` | `scripts/pages/thv-placement.js` |
| Video overlay | 不切新 hash | `scripts/app.js` + `scripts/state.js` + `scripts/components.js` |

重要认知：

- 路由基线保护“怎么到页面”。
- 页面基线保护“页面本来应该有什么”。
- `Video overlay` 不是新页面路由，而是当前页面上的 UI 状态变化。
- 复刻 App 页面前，应先读项目、建基线，再改数据/页面/样式，最后跑验证。

## 4. 文件改动

本轮在当前工作区创建/修改了以下学习笔记文件：

```text
day1-project-structure-notes.md
day1-route-walkthrough.md
day1-prompts.md
day1-review-notes.md
week2-thread-1-summary.md
```

说明：

- 没有修改 `vie-aorta-master` 项目源码。
- 没有修改用户 Downloads 中的 `cyf-*` 原始文件。
- `day1-prompts.md` 曾按用户要求删除原 Prompt 4，并重新编号整理。

## 5. 测试结果

已成功运行三条项目验证命令，结果全部通过：

```text
PASS verify-routes.mjs (331 routes)
PASS routes.test.mjs
PASS navigation.test.mjs
```

命令含义：

```bash
node scripts/verify-routes.mjs
```

快速批量验证主要 hash 路由能否正常渲染。

```bash
node tests/routes.test.mjs
```

深度检查路由、页面内容、数据结构和异常 fallback。

```bash
node tests/navigation.test.mjs
```

检查导航、返回、THV 切换、快速选择器、视频弹层、图片弹层等交互结构。

环境注意：

- Codex 工具环境里的 `node` 曾优先指向 Codex 内部 Node，出现 `Access is denied`。
- 后续定位到系统 Node 路径为 `D:\software\nodejs\node.exe`，并使用完整路径成功运行验证。

## 6. 未完成 / bug / gap

未完成：

- 没有实际修改或复刻新的 App 页面。
- 没有将 review 建议同步改回用户 Downloads 中的四份 `cyf-*` 文档。
- 没有统一检查所有文档中的项目路径是否完全一致。

潜在 gap：

- 文档中出现过两个项目路径上下文：

```text
D:\学习\vie-aorta-master.zip
D:\week2-viv-aortic\vie-aorta-master
```

后续需要确认当前真实项目目录，以免笔记和实际路径不一致。

- `cyf-day1-verification-result.md` 中记录了 `RequestsDependencyWarning`。该 warning 是 Python requests 相关信息，而验证命令是 Node 命令。建议确认它是否确实属于同一轮终端输出。

- `cyf-day1-prompts.md` 里有“后续选择：1”的描述，但没有解释 `1` 代表什么，后续回看可能不清楚上下文。

已遇到并解释过的问题：

- `localhost 拒绝建立连接`：通常是本地服务器未启动。
- `python3/python not found`：`start.sh` 找不到 Python，可用 `npx serve . -l 14712` 替代。
- `This port was picked because 14712 is in use`：14712 端口被占用，可换端口或关闭旧服务器。
- `bash: $'\E[200~chmod': command not found`：复制命令时混入隐藏字符，可手动输入或用 `bash ./start.sh`。

## 7. 下一步建议

建议下一步按这个顺序推进：

1. 统一真实项目路径。

确认当前以后都使用哪个目录：

```text
D:\week2-viv-aortic\vie-aorta-master
```

或：

```text
D:\学习\vie-aorta-master
```

2. 整理 Day 1 文档。

重点处理：

- 统一路径；
- 标注“白屏 MIME 问题”为本次环境特例；
- 确认 `RequestsDependencyWarning` 来源；
- 给 Prompt 3 的“选择 1”补充上下文。

3. 开始复刻新页面前，先建立基线。

建议先记录：

```text
目标页面截图
目标 hash 路由
对应 pages 文件
对应 data 数据
相关 assets 素材
当前页面应有内容
```

4. 修改顺序建议。

优先顺序：

```text
先看参考图和相似页面
-> 看 router.js
-> 看 pages/*
-> 看 components.js
-> 看 data/*
-> 看 styles/app.css
-> 最后跑验证
```

5. 每次改完都运行验证。

建议固定使用：

```bash
node scripts/verify-routes.mjs
node tests/routes.test.mjs
node tests/navigation.test.mjs
```

6. 需要启动本地服务器时，优先使用当前环境稳定的方式。

如果 Python 不可用：

```bash
npx serve . -l 14712
```

如果 14712 被占用，可以：

- 打开命令提示的新端口；
- 或关闭旧服务器后重新启动。
