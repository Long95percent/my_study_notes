---
tags: [RAG, HybridSearch, BM25, VectorSearch]
---
# 混合检索 Hybrid Search

## 一句话定位

混合检索把关键词检索和向量检索结合，兼顾精确匹配和语义匹配。

## 为什么需要

- 向量检索擅长语义相似，但可能漏掉专有名词、代码、编号。
- BM25 擅长关键词精确匹配，但不懂语义改写。

## 常见做法

- BM25 Top-K + Vector Top-K 合并。
- Reciprocal Rank Fusion，简称 RRF。
- 加 Reranker 做最终排序。

## 适合场景

- 企业文档、API 文档、代码库、法律条文。
- 查询中有大量专有名词、ID、错误码。

## 关联

- [[Elasticsearch与OpenSearch]]
- [[重排Reranker]]
