---
tags: [Observability, Tracing, Agent]
---
# Tracing 与可观测性

## 一句话定位

Tracing 用来还原 Agent 从用户请求到模型推理、工具调用、检索、最终输出的完整链路。

## 需要记录

- run_id / trace_id / user_id / session_id。
- Prompt、模型、参数、Token 用量。
- Tool call：工具名、参数、耗时、返回、错误。
- Retrieval：query、Top-K、score、chunk_id。
- Final answer：最终答案和引用证据。

## 工具

- OpenTelemetry
- LangSmith
- Langfuse
- Phoenix
- Prometheus + Grafana

## 关联

- [[Agent评测指标体系]]
- [[ClickHouse]]
