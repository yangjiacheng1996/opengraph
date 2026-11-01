> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# LangSmith Observability

> Instrument your LLM application, investigate traces, and monitor performance in production with LangSmith.

# LangSmith Observability

LangSmith Observability provides full visibility into your LLM application: from individual traces to production-wide performance metrics. Traces are the record of what your agents did in production. Use them to debug failures, monitor quality, and build the datasets you evaluate against.

LangSmith works with many frameworks and providers. Browse [available integrations](https://docs.langchain.com/langsmith/integrations.md) to connect your stack including OpenAI, Anthropic, CrewAI, Vercel AI SDK, Pydantic AI, and more.

## Get started

**1. Create an account**

Sign up at [smith.langchain.com](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=snippets-langsmith-account-api-key-quickstart) (no credit card required).
You can log in with **Google**, **GitHub**, or **email**.

**2. Create an API key**

Go to your [Settings page](https://smith.langchain.com/settings) > **API Keys** > **Create API Key**.
Copy the key and save it securely.

Once your account and API key are ready, set up tracing:

> [card] [Set up tracing](https://docs.langchain.com/langsmith/observability-quickstart.md)
>
> Add tracing to your app in minutes with environment variables, framework integrations, or the SDK.

> [card] [Trace a RAG application](https://docs.langchain.com/langsmith/observability-llm-tutorial.md)
>
> Follow a step-by-step tutorial to instrument a retrieval-augmented generation app from start to finish.

## Investigate and monitor

> [card] [View traces](https://docs.langchain.com/langsmith/view-traces.md)
>
> Inspect threads and runs in the Messages, Turns, and Details views.

> [card] [Monitor performance](https://docs.langchain.com/langsmith/dashboards.md)
>
> Build dashboards and set alerts to track quality and catch issues early.

> [card] [Configure automations](https://docs.langchain.com/langsmith/rules.md)
>
> Automate workflows with rules, webhooks, and online evaluations.

> [card] [Collect feedback](https://docs.langchain.com/langsmith/attach-user-feedback.md)
>
> Annotate outputs and gather user feedback using queues or inline annotation.

> [card] [Find and fix failures with Engine](https://docs.langchain.com/langsmith/engine.md)
>
> Automatically detect recurring issues in your traces, diagnose their root cause, and resolve them with LangSmith Engine.

If your applications call models through the [LLM Gateway](https://docs.langchain.com/langsmith/llm-gateway.md), the gateway traces every call itself, with no tracing setup in your code. For where those traces appear and what they record, see [View LLM Gateway traces](https://docs.langchain.com/langsmith/llm-gateway-traces.md).

For terminology and core concepts, refer to [Observability concepts](https://docs.langchain.com/langsmith/observability-concepts.md). For trace pricing, retention, and limits, see [Usage and billing](https://docs.langchain.com/langsmith/usage-and-billing.md).

> [!NOTE]
>
> To set up a LangSmith instance, visit the [Platform setup section](https://docs.langchain.com/langsmith/platform-setup.md) to choose between cloud, hybrid, or self-hosted. All options include observability, evaluation, prompt engineering, and deployment.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/observability.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
