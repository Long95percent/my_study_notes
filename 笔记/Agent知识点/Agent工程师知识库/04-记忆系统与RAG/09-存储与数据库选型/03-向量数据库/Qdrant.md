---
tags: [向量数据库, Qdrant, RAG]
---
# Qdrant

## 适合用在什么情况下

- 需要向量检索 + 元数据过滤 Payload Filter。
- 希望部署相对简单，工程体验好。
- 中小到较大规模 RAG、Agent 记忆检索。

## 特点

- Collection / Point / Payload 模型清晰。
- 支持 HNSW 向量索引。
- Filter 能按用户、权限、时间、标签过滤。

## Agent 场景

- 用户长期记忆按 user_id 过滤。
- 知识库按 tenant_id / permission_scope 隔离。
- 结合边界化召回做时间和主题过滤。

## 关联

- [[边界化召回]]
- [[Embedding模型选型]]
