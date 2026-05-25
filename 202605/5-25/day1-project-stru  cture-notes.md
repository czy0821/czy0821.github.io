# Day 1: Project Structure Notes

## 1. 这个项目是什么类型？

这个项目是一个静态 Web SPA。

它不是 React、Vue、Next 项目，而是使用原生 HTML、CSS 和 JavaScript module 写成的单页应用。

可以这样理解：

- 静态 Web：项目主要由 HTML、CSS、JS、图片等静态文件组成。
- SPA：Single Page Application，单页应用。看起来有很多页面，但主要共用一个 `index.html`。
- 页面切换：不是打开多个 HTML 文件，而是通过 JavaScript 根据 hash 路由切换内容。

核心结构是：

```text
index.html
styles/app.css
scripts/app.js
scripts/router.js
scripts/pages/
scripts/data/
assets/images/
tests/
```

## 2. 项目如何启动？

项目通过 `start.sh` 启动。

README 里的启动命令是：

```bash
chmod +x ./start.sh
./start.sh
```

第一行：

```text
给 start.sh 加上可执行权限。
```

第二行：

```text
运行 start.sh。
```

`start.sh` 做的核心事情是：

```text
找到 Python
用 Python 启动本地静态服务器
把项目目录作为网站目录提供给浏览器访问
```

默认访问地址是：

```text
http://localhost:14712
```

所以它不是启动复杂后端，而是启动一个本地静态服务器。

## 3. index.html 负责什么？

`index.html` 是浏览器最先加载的入口文件。

它主要负责三件事：

1. 设置网页基础信息，比如编码、视口、标题。
2. 引入 CSS 样式文件。
3. 提供一个空容器，并加载 JavaScript 入口文件。

最关键的代码是：

```html
<div id="app"></div>
<script type="module" src="./scripts/app.js"></script>
```

可以理解为：

```text
index.html 提供舞台
app.js 负责往舞台里放页面内容
```

## 4. scripts/app.js 负责什么？

`scripts/app.js` 是前端 JavaScript 的入口文件。

它主要负责：

- 找到 `index.html` 里的 `#app` 容器；
- 读取当前网址里的 hash；
- 调用 `router.js` 生成当前页面 HTML；
- 把 HTML 填进 `#app`；
- 给搜索框、收藏按钮、弹窗、图片预览、视频面板等绑定事件；
- 监听 hash 路由变化，重新渲染页面。

核心逻辑可以理解为：

```text
当前网址是什么
router.js 判断该显示哪一页
把页面 HTML 填进 #app
重新绑定页面上的按钮和交互
```

`app.js` 里有一行很重要：

```js
window.addEventListener("hashchange", render);
```

意思是：

```text
只要网址里 # 后面的内容变了，就重新渲染页面。
```

## 5. scripts/router.js 负责什么？

`scripts/router.js` 是路由分发文件。

它负责看当前 hash 路由，然后决定显示哪个页面。

比如：

```text
#/                       -> 首页
#/category/stented       -> 分类页
#/valve/hancock-ii       -> 瓣膜详情页
#/utility/bookmarks      -> 工具页
```

`router.js` 会把 hash 拆成一段一段：

```text
#/category/stented
```

拆成：

```text
category
stented
```

然后判断：

```text
这是 category 路由，所以渲染分类页。
```

如果路由格式不对，`router.js` 会返回一个“未找到页面”或安全说明页，避免页面直接崩掉。

## 6. scripts/pages/ 负责什么？

`scripts/pages/` 负责具体页面内容。

里面每个文件通常对应一种页面。

主要文件包括：

```text
scripts/pages/home.js
scripts/pages/category.js
scripts/pages/valve-detail.js
scripts/pages/size-detail.js
scripts/pages/thv-selector.js
scripts/pages/thv-plan.js
scripts/pages/thv-placement.js
scripts/pages/guidance.js
scripts/pages/utility.js
```

可以这样理解：

- `home.js`：首页。
- `category.js`：分类列表页。
- `valve-detail.js`：瓣膜详情页。
- `size-detail.js`：尺寸详情页。
- `thv-selector.js`：THV 推荐选择页。
- `thv-plan.js`：THV 方案详情页。
- `thv-placement.js`：理想放置位置页。
- `guidance.js`：视频指导页。
- `utility.js`：工具页面。

这些页面文件的任务是：

```text
根据数据生成页面 HTML。
```

## 7. scripts/data/ 负责什么？

`scripts/data/` 负责存放页面使用的数据。

主要文件包括：

```text
scripts/data/categories.js
scripts/data/valves.js
scripts/data/utilities.js
```

它们的作用是：

- `categories.js`：存放分类数据，比如 stented、stentless、rings、tavr 等。
- `valves.js`：存放瓣膜数据，比如 Hancock II、尺寸、图片、THV 推荐、视频信息等。
- `utilities.js`：存放工具页数据，比如收藏、病例、识别、裂环、更多等。

可以这样理解：

```text
pages/ 负责怎么展示
data/ 负责展示什么内容
```

如果只是新增内容，很多时候应该优先看 `scripts/data/`，不要一开始就改页面结构。

## 8. styles/app.css 负责什么？

`styles/app.css` 负责整个项目的视觉样式。

它控制：

- 页面布局；
- 字体、颜色、间距；
- 顶部栏；
- 底部导航；
- 分类列表；
- 卡片；
- 按钮；
- 搜索框；
- 视频弹窗；
- 图片弹窗；
- 移动端和桌面端适配。

简单说：

```text
HTML/JS 决定页面有什么
CSS 决定页面长什么样
```

如果要复刻 App 页面视觉，比如间距、颜色、字号、列表样式、弹窗样式，通常会改 `styles/app.css`。

## 9. tests/ 负责什么？

`tests/` 负责测试项目是否还能正常工作。

主要文件包括：

```text
tests/routes.test.mjs
tests/navigation.test.mjs
tests/test-helpers.mjs
```

另外还有：

```text
scripts/verify-routes.mjs
```

它们主要检查：

- 主要 hash 路由能不能渲染；
- 错误路由会不会安全回退；
- 分类页、详情页、尺寸页是否还有关键内容；
- 导航链接是否存在；
- 视频弹窗、图片弹窗、快速选择器结构是否正常；
- 数据结构是否完整。

常见验证命令：

```bash
node scripts/verify-routes.mjs
node tests/routes.test.mjs
node tests/navigation.test.mjs
```

测试的作用是保护项目：

```text
改完页面后，确认没有把原来的路由、导航和弹窗弄坏。
```

## 10. 如果我要复刻一个新页面，应该先看哪些文件？

建议按这个顺序看。

### 第一步：先看参考资料

先看这些文件和目录：

```text
README.md
examples/images/
work/docs/page-map-agent.md
work/docs/viv-aortic-native-reference.md
work/assets-inventory/asset-map-agent.md
```

目的：

```text
先知道原 App 页面长什么样
已有 Web 页面对应到哪里
已有图片素材有哪些
项目怎么启动
```

### 第二步：再看项目入口和主流程

看这些文件：

```text
index.html
scripts/app.js
scripts/router.js
scripts/components.js
```

目的：

```text
理解页面从哪里进入
如何渲染
如何通过 hash 路由切换
有哪些公共组件可以复用
```

### 第三步：看相似页面

如果你要复刻首页，看：

```text
scripts/pages/home.js
```

如果你要复刻分类页，看：

```text
scripts/pages/category.js
```

如果你要复刻瓣膜详情页，看：

```text
scripts/pages/valve-detail.js
```

如果你要复刻尺寸页，看：

```text
scripts/pages/size-detail.js
```

如果你要复刻 THV 相关页面，看：

```text
scripts/pages/thv-selector.js
scripts/pages/thv-plan.js
scripts/pages/thv-placement.js
```

如果你要复刻视频指导页，看：

```text
scripts/pages/guidance.js
```

如果你要复刻工具页，看：

```text
scripts/pages/utility.js
```

### 第四步：看数据来源

看这些文件：

```text
scripts/data/categories.js
scripts/data/valves.js
scripts/data/utilities.js
```

目的：

```text
确认页面内容来自哪里
新增页面是否只需要加数据
图片、标题、尺寸、视频等字段如何写
```

### 第五步：看样式

看：

```text
styles/app.css
```

目的：

```text
找到相似页面的样式
尽量复用已有 class
再补必要的新样式
```

### 第六步：改完后跑验证

建议跑：

```bash
node scripts/verify-routes.mjs
node tests/routes.test.mjs
node tests/navigation.test.mjs
```

再启动项目手动检查：

```bash
./start.sh
```

打开：

```text
http://localhost:14712
```

手动重点检查：

```text
页面能打开
hash 路由正确
刷新后不丢页面
返回按钮正确
底部导航高亮正确
图片能加载
弹窗能打开和关闭
移动端和桌面端布局正常
```

## 最后总结

如果要复刻一个 App 新页面，最稳的顺序是：

```text
先看参考图和资料
再看入口、路由、组件
然后看相似页面和数据
再改页面、数据、样式
最后跑路由测试和导航测试
```

不要一上来就改代码。

先建立页面基线和路由基线，再动手复刻，会更安全。
