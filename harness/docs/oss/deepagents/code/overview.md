> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Deep Agents Code

> Terminal coding agent built on the Deep Agents SDK

Deep Agents Code (`dcode`) is an open source coding agent built on the [Deep Agents SDK](https://docs.langchain.com/oss/python/deepagents/quickstart.md).
It works with any large language model and supports switching providers or models.
Persistent memory carries context across conversations, customizable skills shape behavior, and approval controls gate code execution.

## Get started

Run the following command to install Deep Agents Code and launch an interactive session:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
curl -LsSf https://langch.in/dcode | bash
dcode
```

See the [Quickstart](https://docs.langchain.com/oss/deepagents/code/quickstart.md) to add provider credentials, run your first task, and learn interactive mode.

<video autoPlay muted loop playsInline src="https://mintcdn.com/langchain-5e9cc07a/RVTbVyxmLiI04cgS/oss/images/deepagents/dcode-small.mp4?fit=max&auto=format&n=RVTbVyxmLiI04cgS&q=85&s=0d35e29a34f349183e83bd3d1eceb68b" aria-label="Deep Agents Code terminal demo">
  Your browser does not support the video tag.
</video>

## Capabilities

> [card] [Remote sandboxes](https://docs.langchain.com/oss/deepagents/code/remote-sandboxes.md)
>
> Run agent tools remotely instead of on your local machine.

> [card] [Goals and rubrics](https://docs.langchain.com/oss/deepagents/code/goals-and-rubrics.md)
>
> Define measurable objectives or grading criteria so the agent can check whether work is done.

> [card] [Subagents](https://docs.langchain.com/oss/deepagents/code/subagents.md)
>
> Delegate work to task-specific subagents for parallel execution.

> [card] [Memory](https://docs.langchain.com/oss/deepagents/code/memory-and-skills.md#memory)
>
> Store and retrieve information across sessions, including project conventions and learned patterns.

> [card] [Context compaction](https://docs.langchain.com/oss/deepagents/code/quickstart.md#interactive-mode)
>
> Summarize older messages and offload originals to storage.

> [card] [Human-in-the-loop](https://docs.langchain.com/oss/deepagents/code/quickstart.md#interactive-mode)
>
> Require human approval for sensitive tool operations.

> [card] [Skills](https://docs.langchain.com/oss/deepagents/code/memory-and-skills.md#skills)
>
> Extend agent capabilities with custom expertise and instructions.

> [card] [MCP tools](https://docs.langchain.com/oss/deepagents/code/mcp-tools.md)
>
> Load external tools from Model Context Protocol servers.

> [card] [Tracing](https://docs.langchain.com/oss/deepagents/code/quickstart.md#trace-with-langsmith)
>
> Trace agent operations in LangSmith for observability and debugging.

## Next steps

> [card] [Quickstart](https://docs.langchain.com/oss/deepagents/code/quickstart.md)
>
> Install Deep Agents Code, run your first task, and use interactive or non-interactive modes.

> [card] [Configuration](https://docs.langchain.com/oss/deepagents/code/configuration)
>
> Set up credentials, `config.toml`, environment variables, hooks, and CLI flags.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/deepagents/code/overview.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
