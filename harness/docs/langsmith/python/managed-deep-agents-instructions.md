> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add instructions to Managed Deep Agents

> Define the system prompt for a managed deep agent in instructions.md.

Instructions define always-on agent behavior. They form the core of the agent's system prompt.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

Put the instructions for your agent into `instructions.md` at the project root:

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
my-agent/
  instructions.md
```

For the full project layout, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

## Add instructions

Create or modify `instructions.md` to define the agent's role, behavior, constraints, and guidance for using its tools:

```markdown instructions.md theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
# Assistant

You are a helpful assistant.
```

MDA inserts instructions into the agent's system prompt on every run.
The agent cannot modify these instructions at runtime.

## Deployment

When you run `mda deploy`, MDA syncs `instructions.md` to the agent's [Context Hub](https://docs.langchain.com/langsmith/python/managed-deep-agents-context-hub.md).

You can then edit the instructions in the LangSmith UI and have those changes apply to the agent.

It is best to keep the `instructions.md` file in the repo as the source of truth for lasting changes, as later deployments sync the project copy again.

For what syncs, what does not, and how to open the repo from a deployment, see [Context Hub](https://docs.langchain.com/langsmith/python/managed-deep-agents-context-hub.md).

## When to use instructions

| Concept | Role | Loaded when |
| - | - | - |
| **Instructions** | Always-on system prompt | Every run |
| **[Skills](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills.md)** | Task-specific procedures | When the agent selects them |
| **[Memory](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory.md)** | Knowledge the agent can update | When durable memory is enabled |

For more information, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-instructions.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
