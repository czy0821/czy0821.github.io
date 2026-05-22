# AI 任务流管理系统数据库 Schema v0

## 1. 设计目标

本文档基于 AI 任务流管理系统页面设计最小数据库表结构。

页面中涉及的核心能力包括：

- 展示任务列表
- 按任务标题、负责人、智能体搜索
- 按状态和优先级筛选
- 查看任务详情
- 展示执行步骤和备注
- 标记待确认
- 标记完成
- 编辑任务
- 归档任务
- 重试异常任务

因此第一版数据库至少需要两张表：

| 表名 | 作用 |
| --- | --- |
| `ai_task` | 保存任务本身的数据 |
| `ai_task_operation_log` | 记录任务被谁、在什么时候、做了什么操作 |

---

## 2. 任务表：`ai_task`

### 2.1 表用途

`ai_task` 用于保存 AI 任务的核心信息。

页面任务列表、任务详情、搜索、筛选、状态统计都主要依赖这张表。

### 2.2 建表 SQL

```sql
CREATE TABLE ai_task (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '任务ID',

  title VARCHAR(200) NOT NULL COMMENT '任务标题',
  description VARCHAR(1000) NOT NULL COMMENT '任务描述',

  status VARCHAR(30) NOT NULL DEFAULT 'running' COMMENT '任务状态：running/review/done/error',
  priority VARCHAR(20) NOT NULL DEFAULT 'medium' COMMENT '优先级：high/medium/low',

  owner_name VARCHAR(100) NOT NULL COMMENT '负责人名称',
  agent_name VARCHAR(100) NOT NULL COMMENT '执行任务的AI Agent名称',

  note VARCHAR(1000) NULL COMMENT '任务备注',
  steps_json JSON NULL COMMENT '任务执行步骤',

  archived TINYINT NOT NULL DEFAULT 0 COMMENT '是否归档：0否，1是',

  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',

  INDEX idx_status (status),
  INDEX idx_priority (priority),
  INDEX idx_archived (archived),
  INDEX idx_owner_name (owner_name),
  INDEX idx_agent_name (agent_name),
  INDEX idx_created_at (created_at),
  INDEX idx_updated_at (updated_at)
) COMMENT='AI任务表';
```

### 2.3 字段说明

| 字段 | 类型 | 是否必填 | 为什么需要 |
| --- | --- | --- | --- |
| `id` | BIGINT | 是 | 每条任务都需要唯一标识。前端点击任务、查询详情、更新状态时都需要用任务 ID 定位具体任务。 |
| `title` | VARCHAR(200) | 是 | 页面列表和详情都要展示任务标题，也支持通过任务标题搜索。 |
| `description` | VARCHAR(1000) | 是 | 对应页面中的任务描述，用于列表摘要和详情展示。页面原型中字段名是 `desc`，数据库中使用更清晰的 `description`。 |
| `status` | VARCHAR(30) | 是 | 保存任务当前状态。页面需要展示状态标签、按状态筛选，并统计进行中、待确认、已完成、异常任务数量。 |
| `priority` | VARCHAR(20) | 是 | 保存任务优先级。页面需要展示高、中、低优先级，并支持按优先级筛选。 |
| `owner_name` | VARCHAR(100) | 是 | 保存负责人名称。页面列表展示负责人，并支持按负责人搜索。 |
| `agent_name` | VARCHAR(100) | 是 | 保存执行任务的 AI Agent 名称。页面列表展示智能体，并支持按智能体搜索。 |
| `note` | VARCHAR(1000) | 否 | 保存人工备注。页面详情区需要展示备注信息，例如异常原因、人工补充说明。 |
| `steps_json` | JSON | 否 | 保存任务执行步骤。页面详情区需要展示每一步的名称和状态，例如“读取客户列表：完成”。第一版为了保持最小表结构，先用 JSON 存步骤，不单独拆步骤表。 |
| `archived` | TINYINT | 是 | 支持归档任务。归档后任务可以从默认列表中移出，但数据仍保留在数据库中。 |
| `created_at` | DATETIME | 是 | 保存任务创建时间。页面列表需要展示创建时间，也可用于排序。 |
| `updated_at` | DATETIME | 是 | 保存任务最后更新时间。状态变更、编辑、重试后都需要更新该字段，页面列表也需要展示更新时间。 |

### 2.4 字段取值约定

任务状态 `status`：

| 值 | 页面显示 | 说明 |
| --- | --- | --- |
| `running` | 进行中 | AI 任务正在执行 |
| `review` | 待确认 | 需要人工确认任务结果 |
| `done` | 已完成 | 任务已经完成 |
| `error` | 异常 | 任务执行失败或出错 |

任务优先级 `priority`：

| 值 | 页面显示 | 说明 |
| --- | --- | --- |
| `high` | 高 | 高优先级任务 |
| `medium` | 中 | 中优先级任务 |
| `low` | 低 | 低优先级任务 |

`steps_json` 示例：

```json
[
  {
    "name": "读取客户列表",
    "status": "完成"
  },
  {
    "name": "识别高意向客户",
    "status": "进行中"
  },
  {
    "name": "生成跟进建议",
    "status": "等待中"
  }
]
```

---

## 3. 操作日志表：`ai_task_operation_log`

### 3.1 表用途

`ai_task_operation_log` 用于记录任务操作历史。

当任务被创建、编辑、修改状态、归档、重试时，都可以写入一条日志。

这张表主要解决三个问题：

- 追踪任务发生过哪些变化
- 排查异常或误操作
- 保留关键操作记录，方便后续审计

### 3.2 建表 SQL

```sql
CREATE TABLE ai_task_operation_log (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '日志ID',

  task_id BIGINT NOT NULL COMMENT '任务ID',
  operation_type VARCHAR(50) NOT NULL COMMENT '操作类型：create/update/status_change/archive/retry',

  old_status VARCHAR(30) NULL COMMENT '操作前状态',
  new_status VARCHAR(30) NULL COMMENT '操作后状态',

  operator_name VARCHAR(100) NULL COMMENT '操作人名称',
  remark VARCHAR(1000) NULL COMMENT '操作备注',
  detail_json JSON NULL COMMENT '操作详情',

  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '操作时间',

  INDEX idx_task_id (task_id),
  INDEX idx_operation_type (operation_type),
  INDEX idx_created_at (created_at),

  CONSTRAINT fk_ai_task_operation_log_task
    FOREIGN KEY (task_id) REFERENCES ai_task(id)
) COMMENT='AI任务操作日志表';
```

### 3.3 字段说明

| 字段 | 类型 | 是否必填 | 为什么需要 |
| --- | --- | --- | --- |
| `id` | BIGINT | 是 | 每条操作日志都需要唯一标识。 |
| `task_id` | BIGINT | 是 | 记录这条日志属于哪一个任务，用于查询某个任务的操作历史。 |
| `operation_type` | VARCHAR(50) | 是 | 记录操作类型，例如创建、编辑、状态变更、归档、重试。没有这个字段就无法区分用户做了什么操作。 |
| `old_status` | VARCHAR(30) | 否 | 状态变更前的任务状态。用于追踪任务从什么状态变过来。创建任务时可以为空。 |
| `new_status` | VARCHAR(30) | 否 | 状态变更后的任务状态。用于追踪任务变成了什么状态。 |
| `operator_name` | VARCHAR(100) | 否 | 记录操作人名称。正式系统中需要知道是谁创建、编辑、归档或重试了任务。 |
| `remark` | VARCHAR(1000) | 否 | 保存操作说明，例如“人工确认完成”“异常任务重试”。 |
| `detail_json` | JSON | 否 | 保存更详细的操作内容，例如编辑前后的字段变化。第一版不额外拆表，用 JSON 保留扩展能力。 |
| `created_at` | DATETIME | 是 | 记录操作发生时间，用于按时间查看任务历史。 |

### 3.4 操作类型约定

| operation_type | 说明 |
| --- | --- |
| `create` | 创建任务 |
| `update` | 编辑任务基础信息 |
| `status_change` | 更新任务状态，例如标记待确认、标记完成 |
| `archive` | 归档任务 |
| `retry` | 重试异常任务 |

`detail_json` 示例：

```json
{
  "changedFields": {
    "priority": {
      "old": "medium",
      "new": "high"
    },
    "owner_name": {
      "old": "周敏",
      "new": "林可"
    }
  }
}
```

---

## 4. 页面字段与数据库字段对应关系

| 页面字段 | 数据库表 | 数据库字段 |
| --- | --- | --- |
| 任务标题 | `ai_task` | `title` |
| 任务描述 | `ai_task` | `description` |
| 状态 | `ai_task` | `status` |
| 优先级 | `ai_task` | `priority` |
| 负责人 | `ai_task` | `owner_name` |
| 智能体 | `ai_task` | `agent_name` |
| 创建时间 | `ai_task` | `created_at` |
| 更新时间 | `ai_task` | `updated_at` |
| 备注 | `ai_task` | `note` |
| 执行步骤 | `ai_task` | `steps_json` |
| 操作记录 | `ai_task_operation_log` | `operation_type` |
| 操作时间 | `ai_task_operation_log` | `created_at` |
| 操作人 | `ai_task_operation_log` | `operator_name` |

---

## 5. 最小表结构说明

本版本只设计两张表，是为了满足第一版系统的最小可用需求。

未单独拆出的表包括：

| 暂不拆表的对象 | 原因 |
| --- | --- |
| 用户表 | 当前页面只展示负责人名称，第一版可以先用 `owner_name` 保存。 |
| 智能体表 | 当前页面只展示智能体名称，第一版可以先用 `agent_name` 保存。 |
| 执行步骤表 | 当前页面只展示步骤名称和状态，第一版可以先用 `steps_json` 保存。 |

后续如果系统需要管理用户、管理智能体、分析每一步执行耗时，再将这些内容拆成独立表。
