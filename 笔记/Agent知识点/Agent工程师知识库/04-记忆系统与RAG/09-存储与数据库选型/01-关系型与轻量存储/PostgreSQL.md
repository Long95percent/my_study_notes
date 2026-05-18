---
tags: [数据库, PostgreSQL, OLTP]
---
# PostgreSQL

## 适合用在什么情况下

- 需要强事务、一致性、复杂 SQL 查询。
- 存用户、任务、会话、权限、工具调用记录。
- 想用 `pgvector` 在中小规模场景同时做向量检索。
- 需要 JSONB 存半结构化 Agent 状态。

## 不适合

- 超大规模向量 ANN 检索，专业向量库更合适。
- 极高吞吐日志分析，ClickHouse 更合适。

## Agent 场景

- `agent_runs`：一次 Agent 运行记录。
- `tool_calls`：工具调用参数、结果、耗时、错误。
- `memories`：结构化长期记忆。
- `approvals`：人工审批记录。

## 关联

- [[pgvector]]
- [[ToolRegistry]]
- [[权限审计与审批]]
