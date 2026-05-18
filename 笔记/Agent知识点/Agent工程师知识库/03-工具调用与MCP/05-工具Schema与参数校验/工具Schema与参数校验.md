---
tags: [ToolCalling, Schema, 参数校验]
---
# 工具 Schema 与参数校验

## 一句话定位

Schema 是工具调用的“合同”，参数校验是防止模型幻觉参数进入真实系统的最后防线。

## 需要积累的角度

- Schema 描述：工具名、用途、参数类型、必填项、枚举、默认值。
- 参数校验：类型校验、范围校验、业务校验、权限校验。
- 修复策略：让模型修复参数、程序自动填默认值、拒绝执行。
- 工具版本：工具参数变更要兼容旧调用。
- 日志审计：记录原始参数、修复后参数、失败原因。

## 技术名词

- JSON Schema
- Pydantic
- Zod
- OpenAPI Specification
- Type coercion
- Validation Error

## 关联

- [[结构化输出与JSON Schema]]
- [[ToolRegistry]]
- [[tool_runtime]]
