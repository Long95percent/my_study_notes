---
tags: [PostgreSQL, pgvector, 向量检索]
---
# pgvector

## 一句话定位

pgvector 是 PostgreSQL 的向量扩展，适合把结构化数据和中小规模向量检索放在同一个数据库里。

## 适合

- RAG 规模不大，想降低系统复杂度。
- 需要 SQL 过滤 + 向量相似度一起查。
- 原本业务数据就在 PostgreSQL。

## 不适合

- 亿级向量、高 QPS、复杂 ANN 调优。
- 需要专业向量库的分布式扩展能力。

## 关联

- [[PostgreSQL]]
- [[Qdrant]]
- [[Milvus和FAISS]]
