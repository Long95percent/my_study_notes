---
tags: [LLM, 推理参数]
---
# Temperature TopP TopK

## 一句话定位

这些参数控制模型输出的随机性，影响 Agent 的稳定性和创造性。

## 怎么理解

| 参数 | 作用 | 适合场景 | 风险 |
|---|---|---|---|
| Temperature | 控制采样随机性 | 创意生成可高，工具调用应低 | 高温更容易跑偏 |
| Top-P | nucleus sampling，只在累计概率内采样 | 通用文本生成 | 设置不当会影响稳定性 |
| Top-K | 只从概率最高 K 个词中选 | 部分模型支持 | K 太小可能死板 |

## Agent 场景建议

- 工具调用、JSON 输出、分类任务：低 temperature。
- 头脑风暴、文案生成：中高 temperature。
- 面试回答、技术解释：低到中等，优先稳定准确。

## 关联

- [[结构化输出与JSON Schema]]
- [[Tool Calling]]
