---
tags: [RAG, Embedding]
---
# Embedding 模型选型

## 一句话定位

Embedding 模型决定文本语义向量的质量，直接影响召回结果。

## 选型维度

- 语言：中文、英文、多语言。
- 领域：通用、代码、法律、医疗、金融。
- 维度：向量维度影响存储成本和检索性能。
- 上下文长度：长文本 embedding 能力。
- 部署方式：API、本地模型、私有化。
- 成本与吞吐：批量索引时尤其重要。

## 常见选择

- OpenAI text-embedding 系列：通用效果好，API 接入简单。
- BGE / bge-m3：中文、多语言、本地部署常见。
- E5：开源通用 embedding。
- Jina Embeddings：长上下文、多语言方向常见。

## 关联

- [[Milvus和FAISS]]
- [[RAG评测指标]]
