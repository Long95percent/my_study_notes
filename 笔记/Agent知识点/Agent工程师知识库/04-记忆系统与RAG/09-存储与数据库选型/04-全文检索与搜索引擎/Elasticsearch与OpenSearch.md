---
tags: [搜索引擎, Elasticsearch, OpenSearch, BM25]
---
# Elasticsearch 与 OpenSearch

## 适合

- 关键词搜索、日志搜索、复杂过滤和聚合。
- 文档字段多，需要倒排索引。
- RAG 中做 BM25 召回，与向量检索混合。

## 不适合

- 简单小项目，运维成本偏高。
- 强事务业务主库。

## Agent 场景

- 企业知识库关键词搜索。
- 工具调用日志检索。
- 与向量库做 Hybrid Search。

## 关联

- [[混合检索Hybrid Search]]
- [[RAG评测指标]]
