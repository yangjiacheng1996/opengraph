> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Manage Context Hub for Managed Deep Agents

> Understand how Managed Deep Agents stores instructions, skills, and durable memory in LangSmith Context Hub.

Managed Deep Agents stores deploy-owned instructions and skills, and optional durable memory, in [LangSmith Context Hub](https://docs.langchain.com/langsmith/use-the-context-hub.md). That split lets you change agent behavior without rebuilding application code, while the project remains the source of truth for lasting instruction and skill updates.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

## What lives in Context Hub

| Content | Project source | Synced on `mda deploy` | Writable by the agent |
| - | - | - | - |
| System prompt | [`instructions.md`](https://docs.langchain.com/langsmith/python/managed-deep-agents-instructions) | Yes | No |
| Skills | [`skills/`](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills) | Yes | No |
| Durable memory | Optional [`memory`](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory) declaration | Enables the tree; does not overwrite existing memory | Yes, under `/memories/agent/` |

Tools, middleware, MCP connectors, channels, schedules, sandboxes, and the agent definition ship with the compiled deployment. They are not synced to Context Hub.

For ownership of managed runtime fields, see [Agent definition](https://docs.langchain.com/langsmith/python/managed-deep-agents-agent-definition.md).

## Source of truth

The project and Context Hub both hold copies of `instructions.md` and `skills/`. Which copy wins depends on where you make lasting changes and how you resolve conflicts on deploy.

* **Project files**: Treat `instructions.md` and `skills/` in the project as the source of truth for lasting changes. Each `mda deploy` syncs those files into the agent's Context Hub repo.
* **Context Hub edits**: Edit files in Context Hub when you need a quick change that should apply without a code redeploy. Those Hub edits stay live until a later deploy overwrites them, or until you choose to keep the Hub copy when a conflict prompt appears.
* **Durable memory**: Memory under `/memories/agent/` is agent-owned. Deploys enable the tree but do not overwrite content already stored there.

## How deploy syncs context

`mda deploy` syncs deploy-owned context as part of the deploy pipeline:

* **Instructions and skills**: Each deploy updates the agent's Context Hub repo from the project files. Later deploys sync the project copies again. For skills, deploys also remove deployed skill files that no longer exist locally.
* **Durable memory**: When memory is enabled, the deployment mounts one Context Hub tree at `/memories/agent/`. Deploys do not overwrite content already stored there.
* **Conflicts**: If `instructions.md` or `skills/` changed in Context Hub since the last `mda deploy`, the CLI stops before syncing and asks whether to overwrite the Context Hub copy with the project copy. The default answer keeps the Context Hub version and skips syncing that section. In a non-interactive shell there is no prompt. The deploy exits with an error, and you must re-run with `--context-strategy overwrite` or `--context-strategy keep-hub`. Re-running `mda deploy` on its own reports the same conflict again.
* **Concurrent edits**: If the repo changes during the sync itself, the deploy fails with a commit conflict. Re-run `mda deploy`.

Interactive conflict prompt:

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
◇  Checked Context Hub context
│
▲  Context Hub instructions.md changed since the last MDA sync.
│
◆  Overwrite Hub-edited instructions.md with the local version?
│  ○ Yes / ● No
└
```

Non-interactive conflict (CI or redirected output):

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
└  Context Hub instructions.md and skills/ changed since the last MDA sync. Re-run with --context-strategy overwrite or --context-strategy keep-hub.
```

| Strategy | Effect |
| - | - |
| `overwrite` | Replace the Hub-edited `instructions.md` or `skills/` with the project copy, then continue the deploy. |
| `keep-hub` | Keep the Context Hub version, skip syncing that section, and continue the deploy. |

For the full deploy step list and flags, see the [CLI reference](https://docs.langchain.com/langsmith/python/managed-deep-agents-cli.md#deploy-projects). For secrets routing and deploy options, see [Deploy an agent](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md).

## Edit context in LangSmith

After a successful deploy:

1. Open the deployment in LangSmith.
2. Use the Details sidebar link to the Context Hub repo that holds the deployment's instructions, skills, and memories. Archived repos are labeled when the context is no longer live.
3. Edit files in Context Hub when you need a quick change that should apply without a code redeploy.

For lasting changes, edit the project copies and redeploy. For how deploy resolves Hub edits, see [Source of truth](#source-of-truth).

For browsing commits, promoting environments, and Context Hub concepts outside Managed Deep Agents, see [Use the Context Hub](https://docs.langchain.com/langsmith/use-the-context-hub.md) and [Context engineering concepts](https://docs.langchain.com/langsmith/context-engineering-concepts.md).

## Local development

`mda dev` creates a local Context Hub mock for instructions, skills, and memory. Local Studio sessions do not create or update the hosted Context Hub repo for a deployment.

For more information, see [Develop locally with LangSmith Studio](https://docs.langchain.com/langsmith/python/managed-deep-agents-local-development.md).

## Choose where context belongs

| Goal | Use |
| - | - |
| Always-on system prompt | [Instructions](https://docs.langchain.com/langsmith/python/managed-deep-agents-instructions.md) |
| Task-specific procedures | [Skills](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills.md) |
| Knowledge the agent learns and retains across threads | [Memory](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory.md) |
| Application logic and external calls | [Tools](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md), [MCP connectors](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md), or [middleware](https://docs.langchain.com/langsmith/python/managed-deep-agents-middleware.md) |

## See also

* [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md)
* [Deploy an agent](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md)
* [Use the Context Hub](https://docs.langchain.com/langsmith/use-the-context-hub.md)
* [Manage contexts with the SDK](https://docs.langchain.com/langsmith/manage-contexts-sdk.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-context-hub.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
