> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agent identity

> Choose whether your Fleet agent authenticates with its own credentials or with each user's credentials.

Agent identity controls whose [credentials](https://docs.langchain.com/langsmith/fleet/workspace-admin.md) the agent uses when it interacts with apps and services.

> [!WARNING]
>
> Once an agent identity is set, it cannot be changed.

## Fixed credentials ("Claws")

The agent always authenticates with the same API keys and OAuth tokens, regardless of who is interacting with it.

Use fixed credentials when:

* The agent operates as a shared service (for example, a team Slack bot or a daily briefing agent).
* You want a single set of authenticated accounts for all users.
* The agent needs to run on [channels](https://docs.langchain.com/langsmith/fleet/channels.md) or [schedules](https://docs.langchain.com/langsmith/fleet/schedules.md), which require fixed credentials.

With fixed credentials, all actions the agent takes (sending emails, posting messages, reading calendars) use the account that the agent owner connected during setup.

## User credentials ("Assistants")

The agent authenticates with the API keys and OAuth tokens of the user interacting with it, acting on the user's behalf.

Use user credentials when:

* Each user should act through their own accounts (for example, an email assistant that reads and sends from the user's own inbox).
* You need per-user access control so the agent only sees what that user is authorized to see.
* Audit trails need to reflect which user performed each action.

With user credentials, each user authenticates individually the first time they interact with the agent. The agent uses that user's tokens for all subsequent actions in their threads.

## Set agent identity

To set the identity for an agent:

1. In the [LangSmith UI](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-fleet-agent-identity), navigate to the agent you want to edit.
2. Click  **Edit** in the top right corner.
3. Click **Set identity** and select the identity you want to use.
4. Click **Save**.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/fleet/agent-identity.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
