> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trace OpenAI Codex sessions

> Capture OpenAI Codex agent turns, tool calls, model metadata, and subagent threads in LangSmith.

The LangSmith tracing plugin sends your [OpenAI Codex](https://developers.openai.com/codex) sessions to LangSmith so you can see what the agent did. Each turn becomes a trace, and a session's turns are grouped into a thread.

## Prerequisites

* [Codex CLI](https://developers.openai.com/codex/quickstart?setup=cli) v0.153.4 or later.
* A LangSmith account.
* [Node.js](https://nodejs.org/) 22.x or later on Windows and Linux. On macOS the plugin runs its own build and does not need Node.js.

## Install the plugin

Add the marketplace and install the plugin:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
codex plugin marketplace add langchain-ai/langsmith-codex-plugins
codex plugin add tracing@langsmith-codex-plugins
```

Then trust the plugin's hooks with `/hooks` inside Codex, because nothing traces until you do. Codex asks only in the interactive interface, so `codex exec` skips the hooks until you have trusted them once. Restart Codex after you trust them. To install the plugin for everyone, see [Deploy to your organization](#deploy-to-your-organization). For more on installing plugins, see [Plugins](https://developers.openai.com/codex/plugins) in the Codex docs.

## Sign in

Sign in once with the LangSmith CLI. See [Install the LangSmith CLI](https://docs.langchain.com/langsmith/langsmith-cli.md#install) if you do not have it yet.

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
langsmith auth login
```

If you use the CLI sign-in and have more than one profile, set `profile` in your config file to the one the plugin should use. Your own `api_key` ignores this setting, but a generic `LANGSMITH_API_KEY` is not used as a fallback while `profile` is set.

To use an API key instead, add it to `~/.codex/langsmith.json`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "api_key": "lsv2_pt_...", "agent_id": "shared-codex" }
```

If more than one credential is set, the plugin uses the first one it finds:

1. The plugin's own key from `api_key` or `LANGSMITH_CODEX_API_KEY`.
2. Your `langsmith auth login` sign-in.
3. A generic `LANGSMITH_API_KEY` from your environment.

Tracing stops when your sign-in expires or your key is rejected, and turns from that gap are never uploaded. Codex shows a notice in the chat until you fix it. Admins can also [require a working sign-in](#require-a-working-sign-in) for every prompt.

## Turn on tracing

Tracing is off until you turn it on in `~/.codex/langsmith.json`, unless your admin already turned it on with a [managed file](#lock-settings). Send traces to an [agent](https://docs.langchain.com/langsmith/log-traces-to-agent.md#send-traces-to-an-agent) with `agent_id`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "agent_id": "shared-codex" }
```

Or send them to a project with `project`:

```json theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
{ "enabled": true, "project": "shared-codex" }
```

Use either one. If you set both, `agent_id` wins.

> [!WARNING]
>
> A project can override these settings with `<project>/.codex/langsmith.json` or `<project>/langsmith-plugins.json`. A cloned repository can use them to turn tracing on and send your conversations and files to someone else's server. Before you trust a repository, review its LangSmith config files along with `.codex/config.toml` and `.codex/hooks.json`.

Complete a turn and open your agent or project in [LangSmith](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-trace-with-codex).

## Deploy to your organization

Admins can install the plugin for everyone and control how it traces from one place.

### Install for everyone

ChatGPT workspace admins can import the repository as a marketplace, so the plugin installs for everyone. Each user still has to trust the plugin's hooks. For the setup, see [Plugin management](https://developers.openai.com/codex/enterprise/plugin-management).

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
> Strict auth also blocks `codex exec` runs. Leave it off for automated runs unless a failed sign-in should stop the job.

## Hide a conversation

Send `langsmith-tracing:mute` as a normal message with no leading slash to keep a thread's content out of LangSmith, and `langsmith-tracing:unmute` to turn it back on. The confirmation appears as a prompt blocked by a hook, because that is how Codex shows a plugin's reply. To mute every thread by default, set `defaultMuted` to `true`.

## Secret redaction

The plugin strips secrets on your machine before anything is uploaded, so they never reach LangSmith or a replica. Redaction is on by default.

It catches provider API keys, JSON Web Tokens, and PEM private keys, along with common patterns such as `API_KEY=<value>` or a password in a URL. Each match is replaced with `[SECRET_DETECTED]`. For the full rule list, see [Redact secrets from traces](https://docs.langchain.com/langsmith/redact-secrets.md#rules-in-the-preset).

Redaction only matches known credential shapes, so treat it as a safety net rather than a guarantee. Run names, tags, and attachments skip it, and a redacted trace still holds your prompts and file contents. Limit who can read the tracing project.

To turn redaction off or add your own patterns, see the [plugin settings](https://github.com/langchain-ai/langsmith-codex-plugins#settings).

## Settings

Each setting has a config file key and an environment variable. The environment variable wins when both are set.

| Config key | Environment variable | Default | Description |
| - | - | - | - |
| `enabled` | `TRACE_TO_LANGSMITH` | `false` | Turns tracing on. |
| `api_key` | `LANGSMITH_CODEX_API_KEY` | none | LangSmith API key. Not needed if you sign in with the CLI. |
| `project` | `LANGSMITH_CODEX_PROJECT` | `codex` | Project the traces go to. Use this or `agent_id`. |
| `agent_id` | `LANGSMITH_AGENT_ID` | none | Agent the traces go to. Wins over `project` if both are set. |
| `api_url` | `LANGSMITH_CODEX_ENDPOINT` | `https://api.smith.langchain.com` | LangSmith server to send to. |

For every setting, see the [plugin settings](https://github.com/langchain-ai/langsmith-codex-plugins#settings). To send traces to more than one place, see [Send a trace to more than one place](https://github.com/langchain-ai/langsmith-codex-plugins#send-a-trace-to-more-than-one-place).

## Troubleshooting

* **No traces**: Check that the plugin is installed, its hooks are trusted with `/hooks`, and tracing is on.
* **Stopped after an update**: Codex asks you to trust a hook again whenever a release changes it. Run `/hooks` and approve it.
* **Tracing is paused**: Run `langsmith auth login` again or replace the rejected API key.

For more fixes, see [When nothing shows up](https://github.com/langchain-ai/langsmith-codex-plugins#when-nothing-shows-up).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/trace-with-codex.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
