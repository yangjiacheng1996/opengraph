> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect to MCP servers

> Add tools from remote MCP servers to a managed deep agent.

Connect a managed deep agent to remote [Model Context Protocol (MCP)](https://docs.langchain.com/oss/python/deepagents/mcp.md) servers to add their tools to the agent. Managed Deep Agents creates the MCP client and loads the tools.

Most remote MCP servers require authentication. A [connection](https://docs.langchain.com/langsmith/python/managed-deep-agents-connections.md) supplies it, and declaring the connection as user-owned makes each caller authorize their own account.

> [!NOTE]
>
> Managed Deep Agents is in **public [beta](https://docs.langchain.com/langsmith/release-stages.md)** on [LangSmith Cloud](https://docs.langchain.com/langsmith/cloud.md).

Declare MCP servers in `tools/mcp.py`:

```text theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
my-agent/
  agent.py
  tools/
    mcp.py
```

For the full project layout, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

To implement application logic in the project instead, use an [authored tool](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md).

## Add MCP servers

Use an MCP server when tools already live on a remote MCP server and you want MDA to load them without importing them into the agent definition.

**1. Declare the servers**

Use `define_mcp` to declare one or more remote servers:

```python tools/mcp.py theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from managed_deepagents import define_mcp

mcp = define_mcp(
    servers={
        "langchainDocs": {
            "transport": "http",
            "url": "https://docs.langchain.com/mcp",
        },
    },
)
```

The module must export a module-level `mcp`. A project has one MCP declaration, so declare every server in it.

Managed Deep Agents supports Streamable HTTP (`"http"`) and legacy SSE (`"sse"`) transports. Stdio MCP servers are not supported. Expose a stdio server over HTTP or implement its operation as an [authored tool](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md) instead.

For connection options, see [Manage connections](https://docs.langchain.com/langsmith/python/managed-deep-agents-connections.md).

**2. Select tools (Optional)**

By default, Managed Deep Agents exposes every tool from each server. To expose only selected tools, set an allowlist inside that server's configuration:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{
    "transport": "http",
    "url": "https://docs.langchain.com/mcp",
    "include_tools": ["search_docs_by_lang_chain"],
}
```

To expose every tool except selected tools, replace `include_tools` with `exclude_tools`.

You can use both options together. The denylist applies after the allowlist, and the same tool cannot appear in both lists.

Selection uses raw MCP tool names before Managed Deep Agents prefixes them. Tool names are prefixed with the server name by default to avoid collisions. For example, the `search_docs_by_lang_chain` tool from the `langchainDocs` server is exposed as `langchainDocs__search_docs_by_lang_chain`.

**3. Pass credentials (Optional)**

If an MCP server requires credentials, declare a connection on the server config and create that connection in the workspace.

* **MCP OAuth**: For servers that advertise OAuth and support automatic client registration, create with `mda connections create <slug>` (inferred from the MCP declaration) or `mda connections create <slug> --mcp <url>`. You do not supply a client ID or secret.
* **Opaque secret or general OAuth**: For a static API key, or for a BYOT OAuth app you register yourself, create an opaque secret or general OAuth connection, then set the server's `connection` option to `connections.get(...)`.

For create modes, owners, and runtime authorization, see [Manage connections](https://docs.langchain.com/langsmith/python/managed-deep-agents-connections.md).

## Configure MCP servers

Each server supports the following core options:

| Option | Description |
| - | - |
| `transport` | Required. Use `http` for Streamable HTTP or `sse` for legacy SSE. |
| `url` | Required. The remote MCP endpoint URL. |
| `headers` | Static headers to send to the server. |
| `include_tools` / `includeTools` | Raw MCP tool names to expose. |
| `exclude_tools` / `excludeTools` | Raw MCP tool names to hide. |
| `default_tool_timeout` / `defaultToolTimeout` | Timeout for each tool call, in seconds for Python and milliseconds for TypeScript. |
| `automatic_sse_fallback` / `automaticSSEFallback` | For HTTP, allow the client to fall back to SSE. |
| `reconnect` | For SSE, configure reconnection behavior. |

The MCP definition also accepts these options:

| Option | Default | Description |
| - | - | - |
| `prefix_tool_name_with_server_name` / `prefixToolNameWithServerName` | `true` | Prefix each tool with `{server}__`. |
| `throw_on_load_error` / `throwOnLoadError` | `true` | Fail loading instead of starting with a partial tool set. |

## Deployment

`mda dev` and `mda deploy` discover the MCP declaration under `tools/` and include it in the managed configuration. The declaration is not synced to Context Hub.

## When to use MCP connectors

| Concept | Kind | How it reaches the agent |
| - | - | - |
| **MCP servers** | Managed configuration | Declared in the MCP module under `tools/`; no import into the agent entry |
| **[MCP endpoint](https://docs.langchain.com/langsmith/python/managed-deep-agents-mcp-endpoint.md)** | Deployment API | Exposes the agent as a tool to MCP clients |
| **[Authored tools](https://docs.langchain.com/langsmith/python/managed-deep-agents-tools.md)** | Application code | Import and pass in the agent definition |
| **[Channels](https://docs.langchain.com/langsmith/python/managed-deep-agents-channels.md)** | Managed configuration | Receive external messages that start agent runs and deliver responses |

For more information, see [Project structure](https://docs.langchain.com/langsmith/python/managed-deep-agents-project-structure.md).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/managed-deep-agents-mcp-connectors.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
