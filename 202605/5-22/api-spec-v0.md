# AI 任务列表页 API Spec v0

## 0. 文档说明

本文档用于定义 AI 任务列表页所需的后端 API，包括接口用途、请求参数、响应字段和错误情况。

本版本包含 4 个核心接口：

```text
1. 创建任务 API
2. 查询任务列表 API
3. 查询任务详情 API
4. 更新任务状态 API
```

接口统一前缀：

```text
/api/ai-tasks
```

---

## 1. 基础约定

### 1.1 任务状态

页面上显示中文，但 API 中建议使用英文值。

| API 字段值 | 页面显示 | 含义 |
| --- | --- | --- |
| `running` | 进行中 | AI 任务正在执行 |
| `pending_confirm` | 待确认 | 需要人工确认结果 |
| `completed` | 已完成 | 任务已经完成 |
| `failed` | 异常 | 任务执行失败或出错 |

### 1.2 任务优先级

| API 字段值 | 页面显示 | 含义 |
| --- | --- | --- |
| `high` | 高 | 需要优先处理 |
| `medium` | 中 | 正常优先级 |
| `low` | 低 | 可以稍后处理 |

### 1.3 统一错误响应格式

当接口失败时，后端统一返回类似下面的结构：

```json
{
  "code": "TASK_NOT_FOUND",
  "message": "任务不存在"
}
```

字段说明：

| 字段 | 说明 |
| --- | --- |
| `code` | 错误编码，用于前端判断错误类型 |
| `message` | 错误信息，用于前端展示或调试 |

---

# 2. 创建任务 API

```http
POST /api/ai-tasks
```

## 2.1 用途

用户点击“新建任务”，填写表单并保存时调用。接口用于创建任务并保存到后端数据库中。

## 2.2 页面动作

对应页面上的：

```text
新建任务按钮 -> 填写表单 -> 保存
```

## 2.3 请求参数

前端发送给后端的数据示例：

```json
{
  "title": "生成日报",
  "description": "根据今日业务数据生成日报摘要",
  "assigneeName": "张三",
  "agentName": "日报智能体",
  "priority": "high",
  "inputContent": "请生成今日经营日报",
  "note": "优先处理"
}
```

请求字段说明：

| 字段 | 是否必填 | 页面字段 | 说明 |
| --- | --- | --- | --- |
| `title` | 是 | 任务名称 | 任务标题 |
| `description` | 是 | 任务描述 | 任务的说明文字 |
| `assigneeName` | 是 | 负责人 | 负责处理这个任务的人 |
| `agentName` | 是 | 智能体 | 执行任务的 AI Agent |
| `priority` | 是 | 优先级 | 可选值：`high` / `medium` / `low` |
| `inputContent` | 否 | 输入内容 | 给 AI Agent 的输入信息 |
| `note` | 否 | 备注 | 人工补充说明 |

## 2.4 响应字段

后端创建成功后，返回新任务的信息。

```json
{
  "id": 1,
  "title": "生成日报",
  "description": "根据今日业务数据生成日报摘要",
  "status": "running",
  "priority": "high",
  "assigneeName": "张三",
  "agentName": "日报智能体",
  "createdAt": "2026-05-22 10:00:00",
  "updatedAt": "2026-05-22 10:00:00"
}
```

响应字段说明：

| 字段 | 页面显示 | 说明 |
| --- | --- | --- |
| `id` | 内部使用 | 任务 ID，用于唯一识别一条任务 |
| `title` | 任务名称 | 新创建的任务名称 |
| `description` | 任务描述 | 新创建的任务描述 |
| `status` | 状态 | 新任务默认状态，通常是 `running` |
| `priority` | 优先级 | 任务优先级 |
| `assigneeName` | 负责人 | 任务负责人 |
| `agentName` | 智能体 | 执行任务的 AI Agent |
| `createdAt` | 创建时间 | 任务创建时间 |
| `updatedAt` | 更新时间 | 任务最后更新时间 |

## 2.5 错误情况

| HTTP 状态码 | 错误 code | 说明 | 前端提示建议 |
| --- | --- | --- | --- |
| 400 | `INVALID_REQUEST` | 请求格式错误 | 请求格式错误 |
| 422 | `VALIDATION_FAILED` | 必填字段为空或字段不合法 | 请检查表单内容 |
| 500 | `TASK_CREATE_FAILED` | 创建任务失败 | 新建任务失败，请稍后重试 |

---

# 3. 查询任务列表 API

```http
GET /api/ai-tasks
```

## 3.1 用途

该接口用于获取任务列表数据。页面在以下场景调用该接口：

```text
1. 用户刚打开任务列表页
2. 用户输入搜索关键词
3. 用户选择任务状态筛选
4. 用户选择优先级筛选
5. 用户点击重新加载
```

## 3.2 页面动作

对应页面上的：

```text
任务列表
搜索框
状态筛选
优先级筛选
重新加载
```

## 3.3 请求参数

请求示例：

```http
GET /api/ai-tasks?keyword=日报&status=running&priority=high&page=1&pageSize=20
```

请求参数说明：

| 参数 | 是否必填 | 页面来源 | 说明 |
| --- | --- | --- | --- |
| `keyword` | 否 | 搜索框 | 搜索任务名称、负责人、智能体 |
| `status` | 否 | 状态筛选 | 可选值：`running` / `pending_confirm` / `completed` / `failed` |
| `priority` | 否 | 优先级筛选 | 可选值：`high` / `medium` / `low` |
| `page` | 否 | 分页 | 当前页码，默认 `1` |
| `pageSize` | 否 | 分页 | 每页数量，默认 `20` |

如果没有搜索和筛选，可以这样请求：

```http
GET /api/ai-tasks?page=1&pageSize=20
```

## 3.4 响应字段

后端返回任务列表。

```json
{
  "items": [
    {
      "id": 1,
      "title": "生成日报",
      "description": "根据今日业务数据生成日报摘要",
      "status": "running",
      "priority": "high",
      "assigneeName": "张三",
      "agentName": "日报智能体",
      "createdAt": "2026-05-22 10:00:00",
      "updatedAt": "2026-05-22 10:00:00",
      "availableActions": ["view", "edit", "archive"]
    }
  ],
  "total": 1,
  "page": 1,
  "pageSize": 20
}
```

响应字段说明：

| 字段 | 页面显示 | 说明 |
| --- | --- | --- |
| `items` | 任务列表 | 当前页的任务数据 |
| `items[].id` | 内部使用 | 任务 ID，用于查询详情和更新状态 |
| `items[].title` | 任务名称 | 列表中的任务名称 |
| `items[].description` | 任务描述 | 列表中的任务描述摘要 |
| `items[].status` | 状态 | 任务当前状态 |
| `items[].priority` | 优先级 | 任务优先级 |
| `items[].assigneeName` | 负责人 | 当前任务负责人 |
| `items[].agentName` | 智能体 | 执行任务的 AI Agent |
| `items[].createdAt` | 创建时间 | 任务创建时间 |
| `items[].updatedAt` | 更新时间 | 最近更新时间 |
| `items[].availableActions` | 操作按钮 | 当前任务可以显示哪些操作按钮 |
| `total` | 总数 | 符合条件的任务总数 |
| `page` | 当前页 | 当前页码 |
| `pageSize` | 每页数量 | 每页任务条数 |

`availableActions` 字段和页面按钮的关系：

| API 值 | 页面按钮 |
| --- | --- |
| `view` | 查看 |
| `edit` | 编辑 |
| `archive` | 归档 |
| `retry` | 重试 |

## 3.5 错误情况

| HTTP 状态码 | 错误 code | 说明 | 前端提示建议 |
| --- | --- | --- | --- |
| 400 | `INVALID_QUERY` | 查询参数不合法 | 筛选条件不正确 |
| 500 | `TASK_LIST_LOAD_FAILED` | 任务列表加载失败 | 任务加载失败，请检查网络后重试 |

---

# 4. 查询任务详情 API

```http
GET /api/ai-tasks/{taskId}
```

示例：

```http
GET /api/ai-tasks/1
```

## 4.1 用途

用户点击某一条任务后，右侧详情区展示该任务的完整信息。接口用于根据任务 ID 查询任务详情。

## 4.2 页面动作

对应页面上的：

```text
点击任务行
点击查看按钮
重新加载详情
```

## 4.3 请求参数

路径参数：

| 参数 | 是否必填 | 说明 |
| --- | --- | --- |
| `taskId` | 是 | 任务 ID |

## 4.4 响应字段

后端返回任务详情。

```json
{
  "id": 1,
  "title": "生成日报",
  "description": "根据今日业务数据生成日报摘要",
  "status": "running",
  "priority": "high",
  "assigneeName": "张三",
  "agentName": "日报智能体",
  "inputContent": "请生成今日经营日报",
  "currentStep": "正在分析销售数据",
  "note": "优先处理",
  "createdAt": "2026-05-22 10:00:00",
  "updatedAt": "2026-05-22 10:00:00"
}
```

响应字段说明：

| 字段 | 页面显示 | 说明 |
| --- | --- | --- |
| `id` | 内部使用 | 任务 ID |
| `title` | 任务标题 | 当前任务名称 |
| `description` | 任务描述 | 完整任务描述 |
| `status` | 当前状态 | 当前任务状态 |
| `priority` | 优先级 | 当前任务优先级 |
| `assigneeName` | 负责人 | 当前任务负责人 |
| `agentName` | 智能体 | 执行任务的 AI Agent |
| `inputContent` | 输入内容 | 给 AI Agent 的输入 |
| `currentStep` | 当前执行步骤 | 当前任务执行阶段 |
| `note` | 备注 | 人工补充说明 |
| `createdAt` | 创建时间 | 任务创建时间 |
| `updatedAt` | 更新时间 | 任务最后更新时间 |

## 4.5 错误情况

| HTTP 状态码 | 错误 code | 说明 | 前端提示建议 |
| --- | --- | --- | --- |
| 404 | `TASK_NOT_FOUND` | 任务不存在 | 任务不存在 |
| 500 | `TASK_DETAIL_LOAD_FAILED` | 任务详情加载失败 | 任务详情加载失败，请稍后重试 |

---

# 5. 更新任务状态 API

```http
PATCH /api/ai-tasks/{taskId}/status
```

示例：

```http
PATCH /api/ai-tasks/1/status
```

## 5.1 用途

该接口用于修改指定任务的状态。当前页面中主要对应两个按钮：

```text
标记待确认
标记完成
```

## 5.2 页面动作

对应页面上的：

```text
点击“标记待确认”
点击“标记完成”
```

## 5.3 请求参数

标记为待确认：

```json
{
  "status": "pending_confirm",
  "remark": "等待人工确认结果"
}
```

标记为已完成：

```json
{
  "status": "completed",
  "remark": "人工确认完成"
}
```

请求字段说明：

| 字段 | 是否必填 | 页面来源 | 说明 |
| --- | --- | --- | --- |
| `status` | 是 | 按钮动作 | 要更新成的新状态 |
| `remark` | 否 | 操作备注 | 本次状态变更的说明 |

## 5.4 响应字段

后端返回状态更新结果。

```json
{
  "id": 1,
  "oldStatus": "running",
  "newStatus": "pending_confirm",
  "updatedAt": "2026-05-22 10:30:00"
}
```

响应字段说明：

| 字段 | 说明 |
| --- | --- |
| `id` | 被更新的任务 ID |
| `oldStatus` | 修改前的状态 |
| `newStatus` | 修改后的状态 |
| `updatedAt` | 状态更新时间 |

## 5.5 错误情况

| HTTP 状态码 | 错误 code | 说明 | 前端提示建议 |
| --- | --- | --- | --- |
| 400 | `INVALID_STATUS` | 状态值不合法 | 状态不正确 |
| 404 | `TASK_NOT_FOUND` | 任务不存在 | 任务不存在 |
| 409 | `INVALID_STATUS_TRANSITION` | 当前状态不能切换到目标状态 | 当前任务不能修改为该状态 |
| 500 | `TASK_STATUS_UPDATE_FAILED` | 状态更新失败 | 状态更新失败，请稍后重试 |

---

# 6. 页面动作和 API 对应关系

| 页面动作 | 调用 API |
| --- | --- |
| 打开任务列表页 | 查询任务列表 API |
| 搜索任务 | 查询任务列表 API |
| 筛选状态 | 查询任务列表 API |
| 筛选优先级 | 查询任务列表 API |
| 点击任务查看详情 | 查询任务详情 API |
| 点击“新建任务”并保存 | 创建任务 API |
| 点击“标记待确认” | 更新任务状态 API |
| 点击“标记完成” | 更新任务状态 API |

---

# 7. 页面字段和 API 字段对应关系

## 7.1 任务列表字段

| 页面字段 | API 字段 |
| --- | --- |
| 任务名称 | `title` |
| 任务描述 | `description` |
| 状态 | `status` |
| 优先级 | `priority` |
| 负责人 | `assigneeName` |
| 智能体 | `agentName` |
| 创建时间 | `createdAt` |
| 更新时间 | `updatedAt` |
| 操作按钮 | `availableActions` |

## 7.2 任务详情字段

| 页面字段 | API 字段 |
| --- | --- |
| 任务标题 | `title` |
| 任务描述 | `description` |
| 当前状态 | `status` |
| 负责人 | `assigneeName` |
| 智能体 | `agentName` |
| 优先级 | `priority` |
| 输入内容 | `inputContent` |
| 当前执行步骤 | `currentStep` |
| 备注 | `note` |

---
