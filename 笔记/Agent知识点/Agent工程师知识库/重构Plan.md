# Agent 工程师知识库重构 Plan

> 目标：在不删除任何原有笔记的前提下，先搭建一个面向 Agent 工程师成长与面试复盘的知识库框架，并把现有笔记按主题复制归类进去。

## 现状盘点

现有笔记主要分为：
- 基础概念：Prompt Engineering、LangGraph、MCP、Multi-Agent/SubAgent。
- 上下文与持久化：Chunking、Redis、FAISS/Milvus、边界化召回、长期记忆管理。
- 工具调用工程：Tool Calling、ToolRegistry、tool_runtime、whitelist、asyncio。
- 设计题：Agent 协作架构、记忆系统、记忆写入机制。
- 面试材料：Jarvis Life 模拟面试、记忆系统设计、边界化召回、项目困难复盘。

## 重构原则

1. **不删除原笔记**：原目录完整保留，新知识库采用复制归档方式搭建。
2. **面向 Agent 工程师能力树**：从 LLM/Prompt → Agent 编排 → 工具/MCP → 记忆/RAG → Multi-Agent → 工程化 → 项目设计 → 面试。
3. **先框架后细节**：先建立目录、索引、学习路径、模板和归档入口，后续再逐篇精修。
4. **一篇笔记可多处引用**：当前先放入最主要分类，后续可通过索引建立交叉引用。
5. **面试优先可检索**：每个模块保留“核心问题/面试表达/项目落地”三个视角。

## 目标结构

```text
Agent工程师知识库/
├── README.md
├── 00-总览/
│   ├── Agent工程师能力地图.md
│   ├── 学习路线.md
│   └── 笔记整理规范.md
├── 01-大模型与提示词/
├── 02-Agent基础与编排/
├── 03-工具调用与MCP/
├── 04-记忆系统与RAG/
├── 05-Multi-Agent协作/
├── 06-工程化与后端基础/
├── 07-项目设计案例/
├── 08-面试题库/
└── 09-资料与草稿/
```

## 归类策略

- `Prompt Engineering.md` → `01-大模型与提示词/`
- `LangGraph.md` → `02-Agent基础与编排/`
- `mcp协议.md`、Tool Calling、ToolRegistry、tool_runtime、whitelist → `03-工具调用与MCP/`
- Chunking、Redis、Milvus/FAISS、边界化召回、长期记忆管理、记忆系统、记忆写入机制 → `04-记忆系统与RAG/`
- Multi-Agent/SubAgent、Agent协作架构 → `05-Multi-Agent协作/`
- JAVA后端、asyncio、Python SDK → `06-工程化与后端基础/`
- Jarvis 项目困难复盘 → `07-项目设计案例/`
- 模拟面试相关 → `08-面试题库/`
- 空白/未命名/来源说明 → `09-资料与草稿/`

## 后续补全方向

- 补“Agent 运行时核心链路”：Planning → Tool Selection → Tool Execution → Observation → Reflection。
- 补“工程稳定性”：JSON Schema、重试、超时、幂等、权限、审计、降级。
- 补“评测体系”：任务成功率、工具调用准确率、检索命中率、记忆污染率、成本与延迟。
- 补“项目表达模板”：背景、目标、架构、难点、指标、Trade-off、复盘。
- 补“面试题卡片”：每题整理成 30 秒版、2 分钟版、追问版。
