> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Managed Deep Agents project structure

> Understand the project layout for Managed Deep Agents.

A Managed Deep Agents project is a normal Python package with one required root agent entry. Other paths are either ordinary modules you import, or files and directories that MDA discovers to enable managed capabilities.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

## Project layout

```text Project layout theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
my-agent/
├── agent.py                        # Core agent definition

├── instructions.md                 # Managed context
├── skills/
│   └── <name>/
│       └── SKILL.md

├── tools/                          # Application code
│   └── mcp.py                      # MCP server declaration
├── middleware/

├── channels/                       # Managed configuration
│   └── <name>.py
├── schedules/
│   └── <name>.py
├── sandbox/
│   └── __init__.py
├── identity.py
├── memory.py

├── pyproject.toml                  # Dependencies
├── .env                            # Local and deploy secrets

└── evals/                          # Harbor workspace
    ├── harbor-job.json
    └── <task>/                     # Harbor task
        ├── Task.md
        ├── instruction.md
        ├── environment/
        └── tests/
```

The only required file is `agent.py` at the project root containing the [agent definition](https://docs.langchain.com/langsmith/python/managed-deep-agents-agent-definition.md) as a named `agent`. It must export a named `agent` created with `define_deep_agent`. Use only one agent entry in a project.

## How MDA treats project files

* **Managed context**: [`instructions.md`](https://docs.langchain.com/langsmith/python/managed-deep-agents-instructions) defines the system prompt. Each directory under [`skills/`](https://docs.langchain.com/langsmith/python/managed-deep-agents-skills) contains task-specific instructions, such as a `SKILL.md` and any supporting files. MDA syncs both `instructions.md` and `skills/` to [Context Hub](https://docs.langchain.com/langsmith/python/managed-deep-agents-context-hub.md). Optional [durable memory](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory.md) is also backed by Context Hub.

* **Application code**: Files under [`tools/`](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools) and [`middleware/`](https://docs.langchain.com/langsmith/python/managed-deep-agents-middleware) are ordinary project modules. Import them from the agent entry. Other local modules work the same way.

* **Managed configuration**: Certain paths enable capabilities when present. For `channels/` and `schedules/`, only direct children are managed declarations; nested modules are not.

  | Path | Enables |
  | - | - |
  | `identity.py` | [Caller authentication](https://docs.langchain.com/langsmith/python/managed-deep-agents-identity.md) |
  | `memory.py` | [Durable memory](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory.md) |
  | `channels/<name>.py` | [Messaging channels](https://docs.langchain.com/langsmith/python/managed-deep-agents-channels.md) |
  | `tools/mcp.py` | [MCP connectors](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md) |
  | `schedules/<name>.py` | [Cron schedules](https://docs.langchain.com/langsmith/python/managed-deep-agents-schedules.md) |
  | `sandbox/__init__.py` | [Sandbox filesystem and shell](https://docs.langchain.com/langsmith/python/managed-deep-agents-sandboxes.md) |

  `tools/` holds ordinary modules with one exception. `tools/mcp.py` is a managed declaration and exports a module-level `mcp`. Every other module under `tools/` is application code you import.

* **Dependencies and secrets**: Declare dependencies in `pyproject.toml`, which also sets the [deployment Python version](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md#set-the-python-version). MDA loads `.env` locally and forwards non-reserved values as deployment secrets. Reserved platform variables and `.env` files are not included in the build archive. For more information, see [Deploy a Managed Deep Agent](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md).

* **Evals**: Managed Deep Agents [evals](https://docs.langchain.com/langsmith/python/managed-deep-agents-evals.md) are Harbor evals. Run `mda evals init -i` and develop tasks with a coding agent and the `eval-engineering` skill. Generated runtime files stay under `.mda/evals/` and are not included in the deployed agent build.

## Share code from outside the project

The build copies only the project directory. To use a package from a sibling directory, declare it as a local dependency:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
uv add ../shared
```

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from shared import helper
```

Put the shared code in its own package. Its `pyproject.toml` needs `[project]` and `[build-system]`:

```toml ../shared/pyproject.toml theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
[project]
name = "shared"
version = "0.1.0"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

MDA copies the package into the build. `mda dev` reloads when you edit it, and `mda deploy` ships it.

## Next steps

> [card] [Quickstart](https://docs.langchain.com/langsmith/python/managed-deep-agents-quickstart.md)
>
> Create and deploy your first Managed Deep Agent with the `mda` CLI.

> [card] [Tutorial](https://docs.langchain.com/langsmith/python/managed-deep-agents-tutorial.md)
>
> Add durable memory and a daily schedule to the quickstart research assistant.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-project-structure.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
