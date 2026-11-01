> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trace Claude Code sessions

The LangSmith tracing plugin sends your [Claude Code](https://code.claude.com/docs/en/overview) conversations to LangSmith so you can see what the agent did. Each message becomes a trace, and a session's messages are grouped into a thread.

## Prerequisites

* [Claude Code](https://code.claude.com/docs/en/overview) installed.
* A LangSmith account.
* [Node.js](https://nodejs.org/) 20 or later on Windows and Linux. On macOS the plugin runs its own build and does not need Node.js.

## Install the plugin

Run these commands inside Claude Code:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/plugin marketplace add langchain-ai/langsmith-claude-code-plugins
/plugin install langsmith-tracing@langsmith-claude-code-plugins
/reload-plugins
```

Auto-update is off for this marketplace by default. To turn it on, run `/plugin` and open **Marketplaces**. To install the plugin for everyone, see [Deploy to your organization](#deploy-to-your-organization). For more on installing plugins, see [Install plugins](https://code.claude.com/docs/en/plugins/install) in the Claude Code docs.

## Sign in

Sign in once with the LangSmith CLI. See [Install the LangSmith CLI](https://docs.langchain.com/langsmith/langsmith-cli.md#install) if you do not have it yet.

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
langsmith auth login
```

If you use the CLI sign-in and have more than one profile, set `profile` in your config file to the one the plugin should use. Your own `api_key` ignores this setting, but a generic `LANGSMITH_API_KEY` is not used as a fallback while `profile` is set.

To use an API key instead, add it to `~/.claude/langsmith.json`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "api_key": "lsv2_pt_...", "agent_id": "shared-claude" }
```

If more than one credential is set, the plugin uses the first one it finds:

1. The plugin's own key from `api_key` or `CC_LANGSMITH_API_KEY`.
2. Your `langsmith auth login` sign-in.
3. A generic `LANGSMITH_API_KEY` from your environment.

Tracing stops when your sign-in expires or your key is rejected, and turns from that gap are never uploaded. Claude Code reminds you in the chat until you fix it. Admins can also [require a working sign-in](#require-a-working-sign-in) for every prompt.

## Turn on tracing

Tracing is off until you turn it on in `~/.claude/langsmith.json`, unless your admin already turned it on with a [managed file](#lock-settings). Send traces to an [agent](https://docs.langchain.com/langsmith/log-traces-to-agent.md#send-traces-to-an-agent) with `agent_id`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "agent_id": "shared-claude" }
```

Or send them to a project with `project`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "project": "shared-claude" }
```

Use either one. If you set both, `agent_id` wins.

> [!WARNING]
>
> A project can override these settings with `<project>/.claude/langsmith.json` or `<project>/langsmith-plugins.json`. A cloned repository can use them to turn tracing on and send your conversations to someone else's server. Before you trust a repository, review those files along with `.claude/settings.json` and `.claude/settings.local.json`.

Send a message and open your agent or project in [LangSmith](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-trace-claude-code). Run `/langsmith-tracing:trace` to get a link to the current conversation's trace.

## Deploy to your organization

Admins can install the plugin for everyone and control how it traces from one place.

### Install for everyone

Use Claude Code's managed settings to install the plugin on every machine. For the setup, see [Manage Claude Code plugins for your organization](https://code.claude.com/docs/en/plugins/org).

### Lock settings

An admin can lock tracing settings for every user with a managed file at `~/.langsmith-plugins/managed.json` in each user's home folder. The Claude Code, Cursor, and Codex plugins all read this file, and its settings override the user's own config files and environment variables.

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{
  "api_url": "https://langsmith.example.com",
  "project": "engineering",
  "strict_auth": true
}
```

The managed file works on macOS and Linux. The plugin only trusts it when:

* Root owns the file and its folder.
* No other user can write to either.
* Neither is a symlink.

If the file exists but fails these checks, tracing stops and the user sees a message to contact their admin. Locking a setting also locks the settings tied to it, so users cannot route traces around the lock. For example, locking `project` also stops users from adding their own [replicas](https://docs.langchain.com/langsmith/trace-replicas.md) or sending traces to an agent with `agent_id`.

Admins usually install the file on each machine with a mobile device management (MDM) tool such as Jamf or Microsoft Intune. For every setting you can lock, see the [plugin settings guide](https://github.com/langchain-ai/langsmith-plugin-binary/blob/main/docs/settings.md).

> [accordion] Example setup script
>
> Run this as root for each user, with `USER_HOME` set to that user's home folder:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> USER_HOME=/Users/developer
> mkdir -p "$USER_HOME/.langsmith-plugins"
> cp managed.json "$USER_HOME/.langsmith-plugins/managed.json"
> chown root:wheel "$USER_HOME/.langsmith-plugins" "$USER_HOME/.langsmith-plugins/managed.json"
> chmod 755 "$USER_HOME/.langsmith-plugins"
> chmod 644 "$USER_HOME/.langsmith-plugins/managed.json"
> ```
>
> On Linux, use `root:root` instead of `root:wheel`. For the full script, see [Set it up on macOS](https://github.com/langchain-ai/langsmith-plugin-binary/blob/main/docs/settings.md#set-it-up-on-macos).

### Require a working sign-in

Strict auth blocks every prompt until the user's sign-in or key works, so no session goes untraced. It is off by default and has no effect while tracing is off. To turn it on for everyone, set `strict_auth` to `true` in the managed file.

> [!WARNING]
>
> Strict auth also blocks non-interactive runs such as `claude -p` in CI. Leave it off for automated runs unless a failed sign-in should stop the job.

## Hide a conversation

Run `/langsmith-tracing:mute` inside Claude Code to keep a conversation's content out of LangSmith, and `/langsmith-tracing:unmute` to turn it back on. The confirmation appears as a prompt blocked by a hook, because that is how Claude Code shows a plugin's reply. To mute every conversation by default, set `defaultMuted` to `true`.

## Secret redaction

The plugin strips secrets on your machine before anything is uploaded, so they never reach LangSmith or a replica. Redaction is on by default.

It catches provider API keys, JSON Web Tokens, and PEM private keys, along with common patterns such as `API_KEY=<value>` or a password in a URL. Each match is replaced with `[SECRET_DETECTED]`. For the full rule list, see [Redact secrets from traces](https://docs.langchain.com/langsmith/redact-secrets.md#rules-in-the-preset).

Redaction only matches known credential shapes, so treat it as a safety net rather than a guarantee. Run names, tags, and attachments skip it, and a redacted trace still holds your prompts and file contents. Limit who can read the tracing project.

To turn redaction off or add your own patterns, see the [plugin settings](https://github.com/langchain-ai/langsmith-claude-code-plugins#settings).

## Settings

Each setting has a config file key and an environment variable. The environment variable wins when both are set.

| Config key | Environment variable | Default | Description |
| - | - | - | - |
| `enabled` | `TRACE_TO_LANGSMITH` | `false` | Turns tracing on. |
| `api_key` | `CC_LANGSMITH_API_KEY` | none | LangSmith API key. Not needed if you sign in with the CLI. |
| `project` | `CC_LANGSMITH_PROJECT` | `claude-code` | Project the traces go to. Use this or `agent_id`. |
| `agent_id` | `LANGSMITH_AGENT_ID` | none | Agent the traces go to. Wins over `project` if both are set. |
| `api_url` | `LANGSMITH_ENDPOINT` | `https://api.smith.langchain.com` | LangSmith server to send to. |

For every setting, see the [plugin settings](https://github.com/langchain-ai/langsmith-claude-code-plugins#settings).

## Other examples

The plugin's [Advanced configuration](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md) guide has worked examples for:

* [Setting variables in Claude Code's settings](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#set-variables-in-claude-codes-settings)
* [Adding custom metadata](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#add-custom-metadata)
* [Muting every conversation by default](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#mute-every-conversation-by-default)
* [Redacting extra patterns](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#redact-extra-patterns)
* [Sending the same trace to two places](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#send-the-same-trace-to-two-places)
* [Tracing a Claude Code run in CI](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#trace-a-claude-code-run-in-ci)
* [Nesting traces under a run you already trace](https://github.com/langchain-ai/langsmith-claude-code-plugins/blob/main/ADVANCED.md#nest-traces-under-a-run-you-already-trace) in Python and TypeScript

## Troubleshooting

* **No traces**: Check that the plugin is enabled in `/plugin` and that tracing is on.
* **Tracing is paused**: Run `langsmith auth login` again or replace the rejected API key.
* **Still stuck**: Set `CC_LANGSMITH_DEBUG=true` and check `~/.claude/state/hook.log`.

For more fixes, see [When nothing shows up](https://github.com/langchain-ai/langsmith-claude-code-plugins#when-nothing-shows-up).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/trace-claude-code.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
