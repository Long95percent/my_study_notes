---
tags: [MultiAgent, 通信]
---
# Agent 通信协议

## 一句话定位

多 Agent 系统不能只靠随意聊天，必须约定消息结构、状态共享和结果格式。

## 需要设计

- 消息类型：任务分配、进度更新、工具结果、问题请求、最终答案。
- 消息字段：sender、receiver、task_id、context、constraints、output_schema。
- 上下文共享：全量共享、摘要共享、只共享必要字段。
- 权限隔离：不同 Agent 不能默认访问所有工具和记忆。

## 关联

- [[Agent协作架构]]
- [[ToolRegistry]]
