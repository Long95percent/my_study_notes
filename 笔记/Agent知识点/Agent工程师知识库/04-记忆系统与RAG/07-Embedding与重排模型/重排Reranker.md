---
tags: [RAG, Reranker]
---
# 重排 Reranker

## 一句话定位

Reranker 用更精细的模型对初步召回结果重新排序，提升最终进入上下文的材料质量。

## 为什么需要

- 向量相似不等于答案相关。
- Top-K 召回可能混入语义相近但不回答问题的片段。
- 重排能用 query-doc pair 做更细粒度判断。

## 常见模型

- bge-reranker
- Cohere Rerank
- Cross-Encoder reranker

## 工程取舍

- 优点：提升准确性。
- 缺点：增加延迟和成本。
- 常用策略：先召回 Top 50，再重排取 Top 5。

## 关联

- [[边界化召回]]
- [[RAG评测指标]]
