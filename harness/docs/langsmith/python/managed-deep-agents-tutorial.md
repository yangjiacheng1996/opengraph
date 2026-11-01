> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add a custom search tool, memory, and a schedule

> Replace provider search with a Tavily tool, then add durable memory and a daily schedule to the research assistant from the quickstart.

This tutorial continues from the [quickstart](https://docs.langchain.com/langsmith/python/managed-deep-agents-quickstart.md). Use the `research-assistant` project you created there, with your model, instructions, and a working `mda dev` setup.

`mda init` may also scaffold files such as `identity` and `sandbox/`. Leave those as they are; this tutorial does not change them.

This guide replaces the quickstart's built-in provider search with an authored [Tavily](https://tavily.com) search tool, enables durable memory, adds a daily schedule, then deploys.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

## Extend the agent

**1. Add a custom search tool**

Built-in provider search is convenient for a first run. Authored tools give you more control: choose the search API, tune parameters, and keep the tool code in your project.

> [!NOTE]
>
> If you followed the steps to use Tavily in the [Quickstart](https://docs.langchain.com/langsmith/python/managed-deep-agents-quickstart.md), skip to the next step.

Add a [Tavily API key](https://app.tavily.com) to `.env`:

```text .env theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
TAVILY_API_KEY=<TAVILY_API_KEY>
```

Install the Tavily client:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
uv add tavily-python
```

Create a custom `internet_search` tool:

```python tools/search.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
import os
from typing import Literal

from langchain.tools import tool
from tavily import TavilyClient


tavily_client = TavilyClient(api_key=os.environ["TAVILY_API_KEY"])


@tool
def internet_search(
    query: str,
    max_results: int = 5,
    topic: Literal["general", "news", "finance"] = "general",
) -> dict:
    """Search the internet for relevant sources."""
    return tavily_client.search(
        query,
        max_results=max_results,
        topic=topic,
    )
```

Replace the provider search tool dict with your authored tool. Keep the `model` value from the quickstart:

```python agent.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import define_deep_agent

from tools.search import internet_search

agent = define_deep_agent(
    name="research-assistant",
    model="openai:gpt-5.5",
    tools=[internet_search],
)
```

Restart `mda dev` if it is already running. In Studio, ask:

```txt wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
What were the main announcements from the latest LangChain release?
```

Confirm the agent calls `internet_search` and returns an answer with citations. For more authored tools, see [Custom tools](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md).

**2. Update the instructions for memory**

Extend `instructions.md` so the agent knows what shared knowledge to keep. Keep the research behavior and add a memory policy:

```markdown instructions.md theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
# Research assistant

You are a careful research assistant. Use internet search to find sources,
keep notes, and return concise answers with citations.

## Memory

- Record reusable research procedures and project knowledge that can improve future work.
- For release research, check the project's official changelog before secondary sources.
- Never store personal data or secrets in memory.
```

**3. Enable and use durable memory**

Durable memory is opt-in. Before asking the agent to remember anything, add a memory declaration at the project root:

```python memory.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import MemoryLayer, define_memory

memory = define_memory(agent=MemoryLayer())
```

Memory is shared across the deployment and visible to all callers, so do not store personal data or secrets.

Restart `mda dev` so it discovers the new file. In one thread, ask the agent to research a release and to record a reusable project rule, such as "For release research, check the official changelog before secondary sources." Then create a **new thread** in Studio and ask how it will research the next release. Confirm that it applies the shared rule even though the new thread has no conversation history.

See [Memory](https://docs.langchain.com/langsmith/python/managed-deep-agents-memory.md) for details.

**4. Schedule a daily digest**

Add a `schedules/` module so the agent runs on a cron cadence without a user message. This schedule runs every weekday at 8am Pacific:

```python schedules/daily_digest.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import define_schedule

schedule = define_schedule(
    cron="0 8 * * 1-5",
    timezone="America/Los_Angeles",
    prompt=(
        "Review durable memory for reusable research rules. "
        "Summarize anything useful, then list open questions for today."
    ),
)
```

If memory is empty on the first fire, the agent still returns open questions.

`mda deploy` reconciles this schedule into a LangSmith cron job after the deployment is live. After you deploy in the next step, you should see:

* `mda deploy` finish without schedule errors (do not pass `--no-wait`, or schedules are not reconciled).
* A managed cron for this file on the deployment. The schedule name matches the module stem: `daily_digest` (Python) or `daily-digest` (TypeScript).
* No immediate digest run from this cron. The first fire waits until 8:00 America/Los\_Angeles on a weekday.

For thread behavior and constraints, see [Schedules](https://docs.langchain.com/langsmith/python/managed-deep-agents-schedules.md).

**5. Deploy and inspect**

Deploy the project to LangSmith:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
uv run mda deploy
```

On success, the CLI prints the deployment dashboard URL. The deploy syncs the instructions to Context Hub, uploads the compiled project, and reconciles the daily schedule.

Open that URL and confirm:

* The deployment is ready.
* The `daily_digest` or `daily-digest` cron exists.
* A test chat run shows model calls, `internet_search` tool calls, and memory reads or writes in the traces.

For deploy flags and troubleshooting, see [Deploy an agent](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md) and the [CLI reference](https://docs.langchain.com/langsmith/python/managed-deep-agents-cli.md#deploy-projects).

## Next steps

> [card] [Custom middleware](https://docs.langchain.com/langsmith/python/managed-deep-agents-middleware.md)
>
> Add logging, retries, limits, and guardrails around model and tool calls.

> [card] [Identity](https://docs.langchain.com/langsmith/python/managed-deep-agents-identity.md)
>
> Authenticate callers and use verified identity in tools and middleware.

> [card] [Evals](https://docs.langchain.com/langsmith/python/managed-deep-agents-evals.md)
>
> Develop Harbor evals with a coding agent and the eval-engineering skill.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-tutorial.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
