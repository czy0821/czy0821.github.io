# Day 5 Notes：API、请求响应与数据映射

## 1. API 是什么

API（Application Programming Interface，应用程序接口）可以理解为后端对外提供的“功能入口”。

前端不直接操作数据库，而是通过 API 告诉后端要做什么事情，例如：

- 查询任务列表
- 创建任务
- 查询任务详情
- 更新任务状态

示例：

```http
GET /api/ai-tasks
POST /api/ai-tasks
GET /api/ai-tasks/{taskId}
PATCH /api/ai-tasks/{taskId}/status
```

---

## 2. 请求和响应是什么

请求（Request）是客户端发给服务器的数据；响应（Response）是服务器返回给客户端的数据。

### 2.1 请求通常包含

- 请求方法：`GET`、`POST`、`PATCH`、`DELETE`
- 请求 URL：例如 `/api/ai-tasks`
- 请求参数：查询参数或请求体

### 2.2 响应通常包含

- 状态码：`200`（成功）、`400`（参数错误）、`404`（资源不存在）、`500`（服务器错误）
- 响应数据：一般是 JSON

示例：

```json
{
  "id": 1,
  "title": "生成日报",
  "status": "running"
}
```

---

## 3. 数据库表和字段是什么

数据库表（Table）可以理解为“按业务分类的数据表格”，字段（Field/Column）是表格里的列。

- 表：存同一类数据（如任务表、操作日志表）
- 字段：描述这一类数据的属性（如任务标题、状态、创建时间）

示例（任务表 `ai_task`）：

- `id`：任务唯一 ID（主键）
- `title`：任务标题
- `description`：任务描述
- `status`：任务状态
- `priority`：优先级
- `owner_name`：负责人
- `agent_name`：智能体
- `created_at`：创建时间
- `updated_at`：更新时间

---

## 4. 任务列表页需要哪些 API

按页面功能，至少需要以下 4 个核心 API：

1. `POST /api/ai-tasks`：创建任务
2. `GET /api/ai-tasks`：查询任务列表（支持搜索、状态筛选、优先级筛选）
3. `GET /api/ai-tasks/{taskId}`：查询任务详情
4. `PATCH /api/ai-tasks/{taskId}/status`：更新任务状态（如标记待确认、标记完成）

为了完整支持页面交互，通常还会扩展：

1. `GET /api/ai-tasks/summary`：顶部统计（进行中/待确认/已完成/异常）
2. `PATCH /api/ai-tasks/{taskId}`：编辑任务
3. `POST /api/ai-tasks/{taskId}/archive`：归档任务
4. `POST /api/ai-tasks/{taskId}/retry`：重试异常任务

---

## 5. 页面字段、API 字段、数据库字段如何对应

### 5.1 核心字段映射

| 页面字段 | API 字段 | 数据库字段 |
| --- | --- | --- |
| 任务名称 | `title` | `title` |
| 任务描述 | `description` | `description` |
| 状态 | `status` | `status` |
| 优先级 | `priority` | `priority` |
| 负责人 | `assigneeName`（或 `owner`） | `owner_name` |
| 智能体 | `agentName`（或 `agent`） | `agent_name` |
| 创建时间 | `createdAt` | `created_at` |
| 更新时间 | `updatedAt` | `updated_at` |
| 备注 | `note` | `note` |
| 执行步骤 | `steps`/`currentStep` | `steps_json` |

说明：

- 页面是给人看的中文名称
- API 是前后端传输用的字段命名（常用 camelCase）
- 数据库是存储用命名（常用 snake_case）

### 5.2 状态值映射（原型版 vs API 版）

当前原型页面状态值：

- `running`（进行中）
- `review`（待确认）
- `done`（已完成）
- `error`（异常）

API 设计中常见状态值：

- `running`（进行中）
- `pending_confirm`（待确认）
- `completed`（已完成）
- `failed`（异常）

如果两套命名并存，需要在后端或前端做一次状态映射，保持展示一致。

---

## 6. 一句话总结

任务列表页的数据链路是：

前端页面字段 -> 调用 API（请求）-> 后端处理 -> 数据库存储/查询 -> API 返回（响应）-> 页面渲染显示。
