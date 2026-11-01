> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Auth-aware tool responses

> Format tool responses to trigger OAuth flows and resume execution automatically.

Some [tools](https://docs.langchain.com/langsmith/fleet/tools.md) require user authorization (for example, Google, Slack, GitHub). LangSmith Fleet includes middleware to detect when a tool needs authorization and to pause the run with a clear prompt to the user. After the user completes auth, the same tool call is retried automatically.

## Return shape to request auth

If a tool detects missing authorization, return a JSON string containing the following fields:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{
  "auth_required": true,
  "auth_url": "https://auth.example.com/start",
  "auth_id": "opaque-tracking-id"
}
```

* `auth_required`: set to `true` to signal an interrupt is needed.
* `auth_url`: where the user should be redirected to authorize.
* `auth_id`: optional correlation ID to track the auth session.

When Fleet detects this response, it interrupts the run, displays the authentication UI to the user, and automatically retries the tool call once authorization completes.

If you want your custom tools to reuse the same authentication required interrupt + UI, ensure your tools return the same shape of JSON.

> [!NOTE]
>
> Return only this JSON as the tool's output. Avoid including additional text or content. Fleet parses the response to trigger the authentication flow.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/fleet/auth-format.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
