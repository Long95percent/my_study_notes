---
tags: [RAG, 评测]
---
# RAG 评测指标

## 一句话定位

RAG 不能只看“感觉回答对了”，需要分别评估检索质量和生成质量。

## 检索指标

| 指标 | 含义 | 用途 |
|---|---|---|
| Recall@K | 正确材料是否出现在前 K 个结果 | 看召回是否漏掉 |
| Precision@K | 前 K 个结果里有多少相关 | 看噪声多少 |
| MRR | 第一个正确结果排第几 | 看排序质量 |
| nDCG | 排序相关性综合得分 | 多相关文档场景 |

## 生成指标

- Faithfulness：回答是否忠实于证据。
- Answer Relevance：回答是否回应问题。
- Context Relevance：上下文是否相关。
- Hallucination Rate：幻觉率。

## 调优抓手

- Chunk 大小和重叠。
- Embedding 模型。
- Hybrid Search 权重。
- Reranker。
- Top-K 和过滤条件。

## 关联

- [[切片Chunking]]
- [[重排Reranker]]
- [[边界化召回]]
