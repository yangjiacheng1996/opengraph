> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add custom tools to Managed Deep Agents

> Define authored tools for managed deep agents projects.

Custom tools are application code the agent can call for fetching real-time data, querying databases, executing code, and taking actions. Unlike [instructions](https://docs.langchain.com/langsmith/python/managed-deep-agents-instructions.md) and [skills](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills.md), MDA does not discover them automatically.

To load tools from a remote MCP server, see [Connect to MCP servers](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md).

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

Put authored tools under `tools/`, import them into the agent entry, and pass them to the agent definition:

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
my-agent/
  agent.py
  tools/
    customer.py
```

For the full project layout, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

To load tools from a remote MCP server without importing them into the agent entry, use an [MCP connector](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md) instead.

MCP connectors are declared under `tools/` as well, so the `tools/mcp.py` file name is reserved for that declaration.

## Add a tool

Use authored tools for business logic, private APIs, database access, and other code that belongs in your agent project.

**1. Define a tool module**

```python tools/customer.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain.tools import tool


@tool(parse_docstring=True)
def lookup_customer(customer_id: str) -> str:
    """Look up a customer record by ID.

    Args:
        customer_id: Customer ID from the CRM.
    """
    return f"Customer {customer_id} is on the enterprise plan."
```

Use clear, unique tool names to avoid collisions. For more about LangChain tool definitions, see [Tools](https://docs.langchain.com/oss/python/langchain/tools.md).

**2. Attach the tool to the agent**

Import the tool into the project-root agent entry and pass it in the `tools` list:

```python agent.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import define_deep_agent

from tools.customer import lookup_customer

agent = define_deep_agent(
    name="support-agent",
    model="openai:gpt-5.5",
    tools=[lookup_customer],
)
```

Your imports should work the same way they do in a normal local Python project.

**3. Add human-in-the-loop (Optional)**

Pause the agent before sensitive tool calls so a person can approve, edit, or reject them.

Set `interrupt_on` in the agent definition, and optionally set `permissions` to gate tool and filesystem access:

```python agent.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import define_deep_agent

from tools.customer import lookup_customer

agent = define_deep_agent(
    name="support-agent",
    model="openai:gpt-5.5",
    tools=[lookup_customer],
    interrupt_on={"lookup_customer": True},
)
```

The `interrupt_on` field applies the same interrupt behavior as LangChain's [human-in-the-loop middleware](https://docs.langchain.com/oss/python/langchain/guardrails.md#human-in-the-loop).

For decision types (approve, edit, reject), conditional interrupts, and permission rules, see the Deep Agents [Human-in-the-loop](https://docs.langchain.com/oss/python/deepagents/human-in-the-loop.md) and [Permissions](https://docs.langchain.com/oss/python/deepagents/permissions.md) guides.

To resume a paused run, see [Respond to an interrupt](#respond-to-an-interrupt).

To pause on a form your own tool posts in Slack instead, see [Agent-owned interrupts](https://docs.langchain.com/langsmith/python/managed-deep-agents-agent-owned-interrupts.md).

## Use secrets and context

Tools can read deployment secrets from environment variables. Put local values in `.env` for `mda dev`; `mda deploy` forwards non-reserved `.env` values as hosted deployment secrets.

For per-run values such as request metadata or feature flags, use the normal LangChain runtime context patterns for tools. See [how to access context from within your tools](https://docs.langchain.com/oss/python/langchain/tools.md#access-context).

## Deployment

`mda dev` and `mda deploy` copy project files into the compiled build, including modules under `tools/`. Tools are not synced to Context Hub; they ship with the agent code.

## When to use tools

| Concept | Kind | How it reaches the agent |
| - | - | - |
| **Tools** | Application code | Import and pass in the agent definition |
| **[MCP connectors](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md)** | Managed configuration | Declared in the MCP module under `tools/`; no import into the agent entry |
| **[Skills](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills.md)** | Managed context | Procedures the agent loads when relevant |
| **[Instructions](https://docs.langchain.com/langsmith/python/managed-deep-agents-instructions.md)** | Managed context | Always-on system prompt |

For more information, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

## Respond to an interrupt

When a run hits an interrupt, it pauses and waits for a human response before continuing.

* **During local development**, `mda dev` runs the agent in LangSmith Studio, which surfaces the interrupt so you can inspect the pending tool call and resume the run.
* **On a deployed agent**, resume the paused run through the LangGraph server API with a `Command(resume=...)` payload. See [Human-in-the-loop using server API](https://docs.langchain.com/langsmith/add-human-in-the-loop.md).

Human-in-the-loop needs durable thread state to pause and resume. The managed runtime owns the checkpointer, so no extra setup is required.

## Use tools that require authentication

If a tool requires an API key or OAuth token, use a connection to resolve the credential at runtime. See [Manage connections](https://docs.langchain.com/langsmith/python/managed-deep-agents-connections.md).

## Access runtime context

For per-run values such as request metadata or feature flags, use the normal LangChain runtime context patterns for tools. See [how to access context from within your tools](https://docs.langchain.com/oss/python/langchain/tools.md#access-context).

For the caller, channel, and sandbox fields that Managed Deep Agents adds, see [Runtime](https://docs.langchain.com/langsmith/python/managed-deep-agents-runtime.md).

To read or write files in the thread's sandbox from a tool, use `runtime.backend`. See [Read and write sandbox files from code](https://docs.langchain.com/langsmith/python/managed-deep-agents-sandboxes.md#read-and-write-sandbox-files-from-code).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-tools.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
