
[MCP (Model Context Protocol)，一篇就够了。 - 知乎](https://zhuanlan.zhihu.com/p/29001189476)

# 1.什么是MCP
MCP （Model Context Protocol，模型上下文协议）定义了应用程序和 AI 模型之间交换上下文信息的方式。这使得开发者能够**以一致的方式将各种数据源、工具和功能连接到 AI 模型**（一个中间协议层），就像 USB-C 让不同设备能够通过相同的接口连接一样。MCP 的目标是创建一个通用标准，使 AI 应用程序的开发和集成变得更加简单和统一。

# 2.MCP的原理
三个核心组件，分为Host、Client、Server
![[Pasted image 20260422161216.png]]
输入为用户输入以及system prompt以及上一轮得到的工具调用描述，LLM决定是否使用MCP Server以及使用哪些，然后执行对应的MCP Server 并对执行结果进行重新处理，直到n次循环后LLM判断不用再调用了，然后输出最终结果

其中，模型通过prompt来确定当前有哪些工具，将工具的具体使用描述以文本形式传递给模型，模型了解有哪些工具以及结合实际情况进行选择。**模型是通过 prompt engineering，即提供所有工具的结构化描述和 few-shot 的 example 来确定该使用哪些工具**

在工具执行阶段，模型如果判断无需工具，则生成自然语言回复，如果判断需要工具，就输出结构化JSON格式的工具调用请求，客户端会根据这个json代码执行对应工具，并将工具执行结果和system prompt和user input 一起发送给模型。

如果tool call的json代码存在问题或者产生幻觉，会skip掉无效的请求调用,详情见源码


