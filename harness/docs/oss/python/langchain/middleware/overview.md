> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Middleware overview

> Control and customize agent execution at every step

Middleware provides a way to more tightly control what happens inside the agent. Middleware is useful for the following:

* Tracking agent behavior with logging, analytics, and debugging.
* Transforming prompts, [tool selection](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md#llm-tool-selector), and output formatting.
* Adding [retries](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md#tool-retry), [fallbacks](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md#model-fallback), and early termination logic.
* Applying [rate limits](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md#model-call-limit), guardrails, and [PII detection](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md#pii-detection).

Add middleware by passing them to [`create_agent`](https://reference.langchain.com/python/langchain/agents/factory/create_agent):

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain.agents import create_agent
from langchain.agents.middleware import SummarizationMiddleware, HumanInTheLoopMiddleware

agent = create_agent(
    model="gpt-5.5",
    tools=[...],
    middleware=[
        SummarizationMiddleware(...),
        HumanInTheLoopMiddleware(...)
    ],
)
```

## The agent loop

The core agent loop involves calling a model, letting it choose tools to execute, and then finishing when it calls no more tools:

![Core agent loop diagram](https://mintcdn.com/langchain-5e9cc07a/Tazq8zGc0yYUYrDl/oss/images/core_agent_loop.png?fit=max\&auto=format\&n=Tazq8zGc0yYUYrDl\&q=85\&s=ac72e48317a9ced68fd1be64e89ec063)

Middleware exposes hooks before and after each of those steps:

![Middleware flow diagram](https://mintcdn.com/langchain-5e9cc07a/RAP6mjwE5G00xYsA/oss/images/middleware_final.png?fit=max\&auto=format\&n=RAP6mjwE5G00xYsA\&q=85\&s=eb4404b137edec6f6f0c8ccb8323eaf1)

## Use middleware inside a LangGraph workflow

Middleware is not a separate runtime: hooks run inside the compiled [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview.md) that [`create_agent`](https://reference.langchain.com/python/langchain/agents/factory/create_agent) returns. You can drop the whole agent (middleware and all) into a larger [StateGraph](https://reference.langchain.com/python/langgraph/graph/state/StateGraph) as a node or subgraph, and every middleware hook continues to run.

Reach for this pattern when the surrounding topology is more than a standard "loop until done": classifying input before routing to one of several agents, fanning out work in parallel, or stitching agent calls together with deterministic steps.

`HumanInTheLoopMiddleware` matches against each tool's `.name`.

`@tool`-decorated functions take their name from the function, so the key below is `"send_email"`.

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain.agents import AgentState, create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langgraph.graph import START, StateGraph

# Assumes read_email, send_email, classify_node, and route are defined elsewhere.
email_agent = create_agent(
    model="claude-sonnet-4-6",
    tools=[read_email, send_email],
    middleware=[HumanInTheLoopMiddleware(interrupt_on={"send_email": True})],
)

graph = (
    StateGraph(AgentState)
    .add_node("classify", classify_node)
    .add_node("email_agent", email_agent)
    .add_edge(START, "classify")
    .add_conditional_edges("classify", route)
    .compile()
)
```

The HITL interrupt, summarization, PII redaction, retries, and any custom hooks all travel with the agent node. See [Use subgraphs](https://docs.langchain.com/oss/python/langgraph/use-subgraphs.md) for the full set of composition patterns, including subgraph checkpointer scoping (per-invocation versus per-thread).

## Additional resources

> [card] [Built-in middleware](https://docs.langchain.com/oss/python/langchain/middleware/built-in.md)
>
> Explore built-in middleware for common use cases.

> [card] [Custom middleware](https://docs.langchain.com/oss/python/langchain/middleware/custom.md)
>
> Build your own middleware with hooks and decorators.

> [card] [Middleware API reference](https://reference.langchain.com/python/langchain/middleware/)
>
> Complete API reference for middleware.

> [card] [Middleware integrations](https://docs.langchain.com/oss/python/integrations/middleware.md)
>
> Provider-specific middleware for Anthropic, AWS, OpenAI, and more.

> [card] [Testing agents](https://docs.langchain.com/oss/python/langchain/test.md)
>
> Test your agents with LangSmith.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/langchain/middleware/overview.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
