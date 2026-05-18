[LangGraph 入门教程 | 菜鸟教程](https://www.runoob.com/ai-agent/langgraph-quick-start.html)
一个底层及Agent编排框架，专为构建有状态，长时允许的工作流而设计。LangGraph将工作流建模为有向图，其中：节点(Node）是执行具体操作的函数，边（Edge）是定义节点之间的流转路径，支持条件分支，状态（State）是在整个工作流中共享并传递的数据。
相比传统LLM Chain，Lang Graph有哪些优势？
LangGraph是图结构，支持循环，而LLM Chain是线性的，只能单向执行。LangGraph原生支持条件路由，而LLM Chain实现条件路由比较复杂。LangGraph一流支持多Agent协调，而LLM Chain实现困难。LangGraph支持内置状态持久化，而LLM Chain需要手动管理。
