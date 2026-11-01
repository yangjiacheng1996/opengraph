> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Built-in search powered by Parallel

> Add web search powered by Parallel to Managed Deep Agents without a separate account or API key.

[Managed Deep Agents](https://docs.langchain.com/langsmith/python/managed-deep-agents-overview.md) includes built-in web search powered by [Parallel](https://parallel.ai). Give your agent access to current information from the web to help answer questions.

LangSmith manages the search infrastructure, so you can add search without hosting a service or creating a separate Parallel account or API key. The integration offers zero data retention.

> [!NOTE]
>
> Built-in search is available only on LangSmith Plus and Startup plans. Usage is free during private beta.

## Before you begin

You need an existing [Managed Deep Agents project](https://docs.langchain.com/langsmith/python/managed-deep-agents-quickstart.md), your LangSmith workspace ID, and a personal LangSmith API key for use in Studio.

## Add Parallel search

To add Parallel search to your agent:

### Step 1. Set your workspace ID

In your project's `.env` file, set your LangSmith workspace ID:

```shell .env theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
LANGSMITH_WORKSPACE_ID=YOUR_WORKSPACE_ID
```

### Step 2. Set your LangSmith API key

In the same `.env` file, set a personal LangSmith API key for use in Studio:

```shell .env theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
LANGSMITH_API_KEY=YOUR_LANGSMITH_API_KEY
```

You do not need a Parallel API key.

### Step 3. Configure the search server

If the file already defines `mcp`, add `Parallel` to the existing `servers` map. Define `mcp` only once.

> [tab] Python
>
> Add `Parallel` to the `servers` map in `tools/mcp.py`.
>
> ```python tools/mcp.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from managed_deepagents import define_mcp
>
> mcp = define_mcp(
>     servers={
>         "Parallel": {
>             "transport": "http",
>             "url": "https://api.smith.langchain.com/v1/managed-tools/servers/parallel/mcp",
>         },
>     },
> )
> ```

> [tab] TypeScript
>
> Add `Parallel` to the `servers` map in `tools/mcp.ts`.
>
> ```typescript tools/mcp.ts theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import { defineMcp } from "managed-deepagents";
>
> export const mcp = defineMcp({
>   servers: {
>     "Parallel": {
>       transport: "http",
>       url: "https://api.smith.langchain.com/v1/managed-tools/servers/parallel/mcp",
>     },
>   },
> });
> ```

Your agent can now use Parallel search alongside its other tools.

## Terms of use

Your use of this integration must comply with Parallel's terms of service, including its restrictions on prohibited or abusive use.

## See also

* [Connect to MCP servers](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md)
* [Add custom tools](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-search.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
