> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Templates

> Start faster with curated Fleet templates and customize tools, prompts, and channels.

LangSmith Fleet includes [starter templates](https://www.langchain.com/templates) to help you create agents quickly. Templates include predefined instructions, [tools](https://docs.langchain.com/langsmith/fleet/tools.md), and [channels](https://docs.langchain.com/langsmith/fleet/essentials.md#channels) (if applicable) for common use cases. You can use templates as-is, or as a baseline to customize.

> [!TIP]
>
> If you're new to Fleet, start with the step-by-step [quickstart](https://docs.langchain.com/langsmith/fleet/quickstart.md) to build your first agent using a template.

## Features

Templates are pre-configured agents designed for specific use cases. Each template includes the following components:

### Pre-configured tools

Templates come with a curated set of [tools](https://docs.langchain.com/langsmith/fleet/essentials.md#tools) that enable the agent to perform specific actions. For example, an email assistant template includes tools for reading, sending, and organizing emails. Tools connect to external services through OAuth authentication, allowing your agent to interact with apps like Gmail, Slack, or Linear. For a complete list, refer to [Supported tools](https://docs.langchain.com/langsmith/fleet/tools.md).

### System instructions

Each template includes a *system prompt* (also called *instructions*) that defines the agent's behavior, personality, and capabilities. The system prompt guides how the agent interprets user requests and uses its available tools. You can customize these instructions to match your specific needs.

### Channels (optional)

Some templates include [channels](https://docs.langchain.com/langsmith/fleet/essentials.md#channels) that allow agents to respond to external events automatically. For example, a Slack bot template might include a channel that activates when someone mentions the agent in a Slack conversation. Channels enable proactive agent behavior beyond chat-based interactions.

### Cloning and customization

Templates serve as starting points that you clone to create your own agent. When you clone a template, you create an independent copy that you can customize without affecting the original. You can modify prompts, add or remove tools, attach different channels, and switch models to tailor the agent to your requirements.

## Available templates

> [card] Executive Assistant
>
> Manages your inbox, calendar, and daily brief.

> [card] Software Engineer
>
> Ships code from Slack, Linear, and GitHub in a sandbox.

> [!NOTE]
>
> The available templates may change over time. For the most up-to-date set, open **Templates** in Fleet or the [templates gallery](https://www.langchain.com/templates).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/fleet/templates.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
