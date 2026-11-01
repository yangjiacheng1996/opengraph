> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect Managed Deep Agents to channels

> Connect Managed Deep Agents to external messaging services that can start runs and receive responses.

A channel makes a managed deep agent available in an external messaging service. Messages from the service can start agent runs, and the agent's final responses return through the same service.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

## Project structure

Channel declarations live in the project-level `channels/` directory:

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
my-agent/
  agent.py
  channels/
    slack.py
```

## Understand channels

A channel combines three parts of an external messaging integration:

* **Inbound events**: Verify and normalize provider events, then start an agent run.
* **Outbound messaging**: Send the agent's response back to the originating conversation.
* **Provisioning**: Create and configure the provider resources that connect the service to the deployed agent.

In Managed Deep Agents, a channel connects a deployed agent to a messaging provider.

The managed runtime handles the channel lifecycle:

```mermaid theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
flowchart LR
    Provider["Messaging provider"] --> Verify["Verify and normalize event"]
    Verify --> Thread["Resolve identity and thread"]
    Thread --> Run["Run agent"]
    Run --> Reply["Post response"]
    Reply --> Provider

    classDef trigger fill:#F6FFDB,stroke:#6E8900,stroke-width:2px,color:#2E3900;
    classDef process fill:#E5F4FF,stroke:#006DDD,stroke-width:2px,color:#030710;
    classDef output fill:#EBD0F0,stroke:#885270,stroke-width:2px,color:#441E33;
    class Provider trigger;
    class Verify,Thread,Run process;
    class Reply output;
```

Provider adapters determine which events are accepted, how provider conversations map to Managed Deep Agents threads, and how responses are delivered. The channel declaration exposes the supported configuration for that provider.

## Declare channels in a project

Put each channel in a separate module under `channels/`.

Export a module-level `channel` from each file.

The file name becomes the configured channel name and identifies the channel within the deployment.

Do not name a declaration `channels/channel.py`.

Channel names must be unique within a project.

The provider factory creates the declaration.

For example, `channels.slack()` creates a Slack channel.

See the provider guide for its declaration, app configuration, and deployment procedure.

## Distinguish channels from connectors

A channel receives messages that start agent runs and delivers responses. An [MCP connector](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-connectors.md) gives the agent tools from a remote MCP server. A project can use either or both.

## Supported channels

> [card] [Slack](https://docs.langchain.com/langsmith/python/managed-deep-agents-channels-slack.md)
>
> Start runs from Slack mentions, direct messages, and thread replies.

> [card] [HTTP](https://docs.langchain.com/langsmith/python/managed-deep-agents-channels-http.md)
>
> Start runs from any service that can send a JSON webhook.

An HTTP adapter can also live in a community-maintained package. See [Share a channel adapter as a package](https://docs.langchain.com/langsmith/python/managed-deep-agents-channels-http.md#share-a-channel-adapter-as-a-package) to publish reusable provider code that users import into their channel declarations.

## See also

* [Identity](https://docs.langchain.com/langsmith/python/managed-deep-agents-identity.md): authenticate callers and scope channel runs to the resolved user.
* [Schedules](https://docs.langchain.com/langsmith/python/managed-deep-agents-schedules.md): deliver scheduled results through a configured channel.
* [Deploy an agent](https://docs.langchain.com/langsmith/python/managed-deep-agents-deploy.md): deploy project changes and configure secrets.
* [CLI reference](https://docs.langchain.com/langsmith/python/managed-deep-agents-cli.md): review channel project-file conventions.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-channels.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
