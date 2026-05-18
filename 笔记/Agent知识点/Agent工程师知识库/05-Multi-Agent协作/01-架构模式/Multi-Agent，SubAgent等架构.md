[大模型智能体架构全解析：Multi-Agent与Sub-Agent选择指南_sub-agents-CSDN博客](https://blog.csdn.net/jennycisp/article/details/157941685)

LangChain在《choosing the right multi-agent architecture》这篇文章中给过一个核心判断：大多数任务仍然适合单智能体，但当"上下文管理"和"分布式开发"成为瓶颈时，多智能体才成为必要选择。




有哪些衡量指标？这里参考博客的
维度一：性能与延迟 最需要权衡
并行能力，前提是子任务独立，如果存在依赖关系，无法并行
Handoffs和Router模式在并行能力上相对较弱。Handoffs是顺序执行的，Agent A做完才能轮到Agent B。Router虽然可以并行调用多个Agent，但Router本身的分类和分发是串行的。

调用次数：
影响延迟的关键因素，在大规模应用中，这个差异会被放大很多
在复杂任务上，SubAgent可以在主Agent 的上下文中持续工作，不需要每次重建上下文
skills模式虽然调用次数比较少，但是上下文不会积累，可能导致后续调用变慢或者质量下降

响应时间
简化公式为：响应时间 = 调用次数 × 单次调用延迟 × 并行因子，Sub-Agents模式调用次数多，但可以并行，总响应时间可能不会太差。Router模式调用次数也多，而且Router的步骤本身可能比较复杂（分类+分发），响应时间可能更长。
Skills模式调用次数最少，但上下文膨胀可能导致单次调用变慢。
Handoffs模式调用次数少，但无法并行，如果任务步骤多，总响应时间也可能很长。
真实的选择，要看你的具体场景。
如果你的任务可以高度并行，Sub-Agents和Router更有优势。如果你的任务需要多轮交互但领域切换 频繁，Skills可能更好。如果你的任务有严格的顺序依赖，Handoffs更合适。

成本控制

Token消耗

模式选择
Sub-Agents模式的一个优势是，不同的子Agent可以用不同的模型。主Agent可以选强大模型，子Agent可以用小模型

重复请求优化
用户如果经常问重复问题，这一点就很关键
但如果每次请求都是新的查询，Multi-Agent的上下文隔离优势就更明显。


可维护性与调试

日志与追踪：
Multi-Agent系统的日志管理复杂度比单Agent高很多，网状
单Agent是一条调用链，Sub-Agent树状，相对简单

状态一致性
当多个Agent协同工作时，如何保证它们对同一个状态有一致的理解？
Sub-Agents模式通过共享上下文来解决这个问题。所有子Agent都在主Agent的上下文中工作，状态由主Agent统一管理。
Multi-Agent模式需要专门的机制，比如共享内存、消息队列、或者事件总线。

错误与处理
当某个Agent出错时，如何恢复？单Agent模式下，你可以重试整个请求，或者部分重试
Multi-Agent模式下，如果Agent A出错，但Agent B和C已经成功执行了，怎么办？是回滚所有Agent的操作，还是只重试Agent A？如果Agent B和C的结果依赖于Agent A，那重试Agent A可能没有意义。
Sub-Agents模式相对简单，因为主Agent可以看到所有子Agent的执行结果，可以决定是重试某个子Agent，还是调整策略后重新分配任务。
Router模式需要Router来协调错误恢复。如果某个子Agent失败，Router需要决定是重试、换一个子Agent，还是返回错误给用户。
Skills和Handoffs模式因为调用链相对简单，错误处理也相对容易

适用场景
Anthropic在ADK架构中将Multi-Agent分为四种模式：Sub-Agents、Skills、Handoffs、Router。

Sub-Agents：
- 多个独立领域需要统一工作流控制
- 需要强上下文隔离
- 团队协作，不同团队负责不同领域

Skills：渐进式能力披露的轻量化扩展
- 单Agent需要覆盖多种专业方向
-  功能切换频繁但单次对话涉及领域较少
-  需要快速迭代和探索

Handoffs：状态驱动的流程化交互
- 分阶段顺序工作流
- 智能体需要全程交互用户
- 后续步骤依赖前序结果

Router：并行派发与结果综合
- 垂直领域多源查询
- 需要整合多专业结果
- 请求无状态，可以独立处理