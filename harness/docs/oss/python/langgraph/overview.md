> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# LangGraph overview

> Gain control with LangGraph to design agents that reliably handle complex tasks

Trusted by companies shaping the future of agents—including Klarna, Uber, J.P. Morgan, and more—LangGraph is a low-level orchestration framework and runtime for building, managing, and deploying long-running, stateful agents. LangGraph gives you fine-grained control to mix deterministic, hand-coded steps with LLM-driven agentic steps in the same graph, so you can build bespoke agents that behave exactly the way your application requires.

LangGraph is very low-level, and focused entirely on agent **orchestration**. Before using LangGraph, we recommend you familiarize yourself with some of the components used to build agents, starting with [models](https://docs.langchain.com/oss/python/langchain/models.md) and [tools](https://docs.langchain.com/oss/python/langchain/tools.md).

We will commonly use [LangChain](https://docs.langchain.com/oss/python/langchain/overview.md) components throughout the documentation to integrate models and tools, but you don't need to use LangChain to use LangGraph. If you are just getting started with agents or want a higher-level abstraction, we recommend you use LangChain's [agents](https://docs.langchain.com/oss/python/langchain/agents.md) that provide prebuilt architectures for common LLM and tool-calling loops.

LangGraph is focused on the underlying capabilities important for agent orchestration: durable execution, streaming, human-in-the-loop, and more.

One of LangGraph's core strengths is the ability to mix deterministic steps with LLM-driven agentic steps in a single graph. This lets you build bespoke workflows where parts of the logic are fully predictable and auditable while other parts are flexible and model-driven, giving you fine-grained control over exactly where and how AI is applied.

**how LangChain products fit together**

**LangChain vs. LangGraph vs. Deep Agents**

Start with [Managed Deep Agents](https://docs.langchain.com/langsmith/python/managed-deep-agents-overview.md) for a batteries-included harness (context compression, virtual filesystem, subagents) when you want to run production agents without having to manage infrastructure. Alternatively, for maximum flexibility while using the same Deep Agents harness, use the [Deep Agents SDK](https://docs.langchain.com/oss/python/deepagents/overview.md#self-host-with-the-sdk) and host agents yourself, or run a custom [LangSmith Deployment](https://docs.langchain.com/langsmith/deployment.md). Deep Agents is built on LangChain [agents](https://docs.langchain.com/oss/python/langchain/agents.md), which run on LangGraph.

Use [LangChain](https://docs.langchain.com/oss/python/langchain/agents.md) (`create_agent`) for a highly customizable agent framework with abstractions and integrations for models, tools, and agent loops.

Use [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview.md), the low-level orchestration framework, for advanced needs combining deterministic and agentic workflows.
LangGraph is the orchestration runtime: durable execution, streaming, human-in-the-loop, and persistence.

Use [LangSmith](https://docs.langchain.com/langsmith/observability.md) to trace, debug, and evaluate agents built with any of these frameworks. Follow the [tracing quickstart](https://docs.langchain.com/langsmith/trace-with-langchain.md) to get set up. We recommend you also set up [LangSmith Engine](https://docs.langchain.com/langsmith/engine.md) which monitors your traces, detects issues, and proposes fixes. You can open a pull request with the proposed fix directly from the Engine tab.

Use [LangSmith Fleet](https://docs.langchain.com/langsmith/fleet/index.md) for no-code agent building with templates, integrations, and routine automation.

Read [Frameworks, runtimes, and harnesses](https://docs.langchain.com/oss/python/concepts/products.md) for a comparison of the open source stack.

## &#x20;Install

```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
pip install -U langgraph
```

```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
uv add langgraph
```

Then, create a simple hello world example:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langgraph.graph import StateGraph, MessagesState, START, END

def mock_llm(state: MessagesState):
    return {"messages": [{"role": "ai", "content": "hello world"}]}

graph = StateGraph(MessagesState)
graph.add_node(mock_llm)
graph.add_edge(START, "mock_llm")
graph.add_edge("mock_llm", END)
graph = graph.compile()

graph.invoke({"messages": [{"role": "user", "content": "hi!"}]})
```

> [!TIP]
>
> Use [LangSmith](https://docs.langchain.com/langsmith/observability.md) to trace requests, debug agent behavior, and evaluate outputs. Set `LANGSMITH_TRACING=true` and your API key to get started. Follow the [tracing quickstart](https://docs.langchain.com/langsmith/trace-with-langchain.md) to get set up.  We recommend you also set up [LangSmith Engine](https://docs.langchain.com/langsmith/engine.md) which monitors your traces, detects issues, and proposes fixes.

## Core benefits

LangGraph provides low-level supporting infrastructure for *any* long-running, stateful workflow or agent. LangGraph does not abstract prompts or architecture, and provides the following central benefits:

* **Mix deterministic and agentic steps**: Combine hand-coded, deterministic logic with LLM-driven decision-making in a single graph. Use deterministic steps where you need reliability and predictability, and agentic steps where you need flexibility—giving you precise control over every part of your agent's behavior.
* [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence.md): Build agents that persist through failures and can run for extended periods, resuming from where they left off.
* [Human-in-the-loop](https://docs.langchain.com/oss/python/langgraph/interrupts.md): Incorporate human oversight by inspecting and modifying agent state at any point.
* [Comprehensive memory](https://docs.langchain.com/oss/python/concepts/memory.md): Create stateful agents with both short-term working memory for ongoing reasoning and long-term memory across sessions.
* [Debugging with LangSmith](https://docs.langchain.com/langsmith/observability.md): Gain deep visibility into complex agent behavior with visualization tools that trace execution paths, capture state transitions, and provide detailed runtime metrics.
* [Production-ready deployment](https://docs.langchain.com/langsmith/deployment.md): Deploy sophisticated agent systems confidently with scalable infrastructure designed to handle the unique challenges of stateful, long-running workflows.

## LangGraph ecosystem

While LangGraph can be used standalone, it also integrates seamlessly with any LangChain product, giving developers a full suite of tools for building agents. To improve your LLM application development, pair LangGraph with:

> [card] [LangSmith Observability](https://docs.langchain.com/langsmith/observability.md)
>
> Trace requests, evaluate outputs, and monitor deployments in one place. Prototype locally with LangGraph, then move to production with integrated observability and evaluation to build more reliable agent systems.

> [card] [LangSmith Deployment](https://docs.langchain.com/langsmith/deployment.md)
>
> Deploy and scale agents effortlessly with a purpose-built deployment platform for long running, stateful workflows. Discover, reuse, configure, and share agents across teams — and iterate quickly with visual prototyping in Studio.

> [card] [LangChain](https://docs.langchain.com/oss/python/langchain/overview.md)
>
> Provides integrations and composable components to streamline LLM application development. Contains agent abstractions built on top of LangGraph.

## Acknowledgements

LangGraph is inspired by [Pregel](https://research.google/pubs/pub37252/) and [Apache Beam](https://beam.apache.org/). The public interface draws inspiration from [NetworkX](https://networkx.org/documentation/latest/). LangGraph is built by LangChain Inc, the creators of LangChain, but can be used without LangChain.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/langgraph/overview.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
