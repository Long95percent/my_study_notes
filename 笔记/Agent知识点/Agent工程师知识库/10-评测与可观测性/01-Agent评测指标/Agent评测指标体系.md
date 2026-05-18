---
tags: [Agent, 评测]
---
# Agent 评测指标体系

## 一句话定位

Agent 评测要覆盖任务是否完成、工具是否调对、过程是否安全、成本是否可控。

## 指标表

| 维度 | 指标 | 说明 |
|---|---|---|
| 任务结果 | Task Success Rate | 最终是否完成任务 |
| 工具调用 | Tool Call Accuracy | 工具选择和参数是否正确 |
| 过程质量 | Step Error Rate | 每一步错误率 |
| 恢复能力 | Recovery Rate | 失败后是否能修复 |
| 安全 | Unauthorized Call Rate | 越权调用率 |
| 成本 | Tokens per Task | 单任务 Token 成本 |
| 延迟 | End-to-end Latency | 端到端耗时 |
| 人工介入 | Human Intervention Rate | 需要人工审批/纠偏比例 |

## 关联

- [[工具执行可靠性]]
- [[Tracing与可观测性]]
- [[遇到的最大的困难是什么？]]
