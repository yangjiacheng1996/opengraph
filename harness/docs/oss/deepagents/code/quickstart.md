> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Quickstart

> Install Deep Agents Code, run your first task, and use interactive or non-interactive modes

Deep Agents Code (`dcode`) is a terminal coding agent built on the [Deep Agents SDK](https://docs.langchain.com/oss/python/deepagents/quickstart.md). This guide covers installation, your first task, daily interactive use, automation with piping, and LangSmith tracing. For a feature overview, see [Deep Agents Code overview](https://docs.langchain.com/oss/deepagents/code/overview.md). For `config.toml` and provider settings, see [Configuration](https://docs.langchain.com/oss/deepagents/code/configuration).

## Install and run your first task

**1. Install and launch**

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
curl -LsSf https://langch.in/dcode | bash
```

**2. Add provider credentials**

Deep Agents Code works with any tool-calling LLM. OpenAI, Anthropic, and Google are available out of the box.

Use the `/auth` command to connect with a provider. See [Providers](https://docs.langchain.com/oss/deepagents/code/providers.md) for the full list and credential details.

> [!NOTE]
>
> Web search uses [Tavily](https://tavily.com). Add a key with `/auth`. See [Enable web search](https://docs.langchain.com/oss/deepagents/code/credentials.md#enable-web-search-with-tavily).

**3. Give the agent a task**

```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
Create a Python script that prints "Hello, World!"
```

The agent interprets the query and proposes changes with diffs for your approval before modifying files. If needed, it can run shell commands to test the code, check documentation, or search the web for up-to-date information.

**4. Enable tracing (optional)**

To log agent operations, tool calls, and decisions in LangSmith, run `/auth` and add your LangSmith API key. Tracing is enabled on the next launch.

For choosing an agent environment, advanced options, and CI or headless setup, see [Trace with LangSmith](#trace-with-langsmith).

> [!NOTE]
>
> Deep Agents Code is not officially supported on Windows. Windows users can try running it under [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install).

## Interactive mode

Type naturally as you would in a chat interface.
The agent uses its built-in tools, skills, and memory to help you with tasks.

> [accordion] Slash commands
>
> Use these commands within a Deep Agents Code session:
>
> * `/model`: Switch models or open the interactive model selector.
> * `/effort`: Set reasoning effort for the current model.
> * `/agents`: Hot-swap between pre-configured agents without relaunching. See [Command reference](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#command-line-options) for related flags.
> * `/auth`: Manage stored API keys for model providers and services (such as Tavily web search). See [Provider credentials](https://docs.langchain.com/oss/deepagents/code/credentials.md) for details.
> * `/goal <objective>`: Draft acceptance criteria from a measurable objective. See [Goals and rubrics](https://docs.langchain.com/oss/deepagents/code/goals-and-rubrics.md).
> * `/rubric`: Set explicit acceptance criteria for grading. See [Goals and rubrics](https://docs.langchain.com/oss/deepagents/code/goals-and-rubrics.md).
> * `/remember [context]`: Review conversation and update memory and skills. Optionally pass additional context.
> * `/skill:<name> [args]`: Directly invoke a skill by name. The agent server resolves the skill and loads its `SKILL.md` instructions before the first model call. See [Invoke a skill mid-session](https://docs.langchain.com/oss/deepagents/code/memory-and-skills.md#invoke-a-skill-mid-session).
> * `/skill-creator [task]`: Guide for creating effective agent skills.
> * `/offload` (alias `/compact`) - Free up context window space by offloading messages to storage with a summary placeholder. The agent can retrieve the full history from the offloaded file if needed.
> * `/context`: Open a color-coded context-window usage report with model capacity, usage categories, and remaining space.
> * `/context-doctor`: Audit the context injected into the session and its estimated token cost. See [Audit injected context](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#audit-injected-context).
> * `/tools`: List the built-in and MCP tools available to the current agent. See [List available tools](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#list-available-tools).
> * `/extensions`: List loaded [Python extensions](https://docs.langchain.com/oss/deepagents/code/extensions.md), their registrations, and their source paths. Requires `DEEPAGENTS_CODE_EXPERIMENTAL=1`.
> * `/cost`: Show the thread's estimated cost. See [Track thread cost](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#track-thread-cost).
> * `/btw`: Ask a side question about the conversation without interrupting the main task. See [Ask a side question](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#ask-a-side-question).
> * `/tokens`: Display current context window token usage breakdown.
> * `/clear`: Start a fresh thread.
> * `/force-clear`: Recover a stuck session with a fresh thread.
> * `/copy`: Copy the latest assistant message to the clipboard.
> * `/prompts`: Search, preview, copy, and reuse previously submitted prompts.
> * `/rename [name]`: Name the current thread, or generate an editable suggestion when you omit the name. See [Name a thread](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#name-a-thread).
> * `/threads`: Browse and resume previous conversation threads.
> * `/mcp [login <server> | reconnect]`: Show active MCP servers and tools. `login <server>` runs the OAuth flow for a server; `reconnect` loads deferred logins.
> * `/plugins`: Manage [plugins and marketplaces](https://docs.langchain.com/oss/deepagents/code/plugins.md).
> * `/notifications`: Configure startup warning preferences.
> * `/reload`: Re-read `.env` files, refresh configuration, and re-discover skills without restarting. This also reloads plugin skills and MCP configuration. Conversation state is preserved. See [`DEEPAGENTS_CODE_` prefix](https://docs.langchain.com/oss/deepagents/code/configuration#deepagents_code_-prefix) for override behavior.
> * `/theme`: Open the interactive theme selector to switch color themes. Built-in themes are available plus any [user-defined themes](https://docs.langchain.com/oss/deepagents/code/configuration#themes).
> * `/scrollbar`: Show or hide the chat scrollbar.
> * `/line-numbers`: Show or hide file-relative line numbers in new diffs. See [Diff line numbers](https://docs.langchain.com/oss/deepagents/code/config-file.md#diff-line-numbers).
> * `/update`: Check for and install Deep Agents Code updates inline. Detects your install method (uv, Homebrew, pip) and runs the appropriate upgrade command.
> * `/auto-update`: Toggle automatic updates on or off.
> * `/install`: Install an optional integration.
> * `/trace`: Open the current thread in LangSmith.
> * `/editor`: Open the current prompt in your external editor (`$VISUAL` / `$EDITOR`). See [External editor](#external-editor).
> * `/restart`: Restart the agent server.
> * `/timestamps`: Toggle message timestamp footers.
> * `/changelog`: Open Deep Agents Code changelog in your browser.
> * `/docs`: Open the documentation in your browser.
> * `/feedback`: Send feedback or report an issue.
> * `/version` (alias `/about`) - Show installed `deepagents-code` and SDK versions.
> * `/help`: Show help and available commands.
> * `/quit`: Exit Deep Agents Code.

> [accordion] Shell commands
>
> Type `!` to enter shell mode, then type your command.
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> git status
> npm test
> ls -la
> ```

> [accordion] Keyboard shortcuts
>
> **General**
>
> | Shortcut | Action |
> | - | - |
> | `Enter` | Submit prompt |
> | `Shift+Enter`, `Ctrl+J`, `Alt+Enter`, or `Ctrl+Enter` | Insert newline |
> | `@filename` | Auto-complete files and inject content |
> | `@@query` | Search recent threads and insert a thread reference |
> | `Shift+Tab` | Cycle [approval modes](https://docs.langchain.com/oss/deepagents/code/approval-modes.md) |
> | `Ctrl+G` | Open prompt in external editor |
> | `Ctrl+T` | Expand or collapse the subagent panel when one is present |
> | `Ctrl+N` | Review pending notifications when the panel is open |
> | `Ctrl+O` | Expand/collapse the most recent tool output |
> | `Escape` | Interrupt current operation |
> | `Ctrl+C` | Interrupt or quit |
> | `Ctrl+D` | Exit |
>
> **Text editing in the prompt**
>
> The chat input uses standard readline-style bindings:
>
> | Shortcut | Action |
> | - | - |
> | `Ctrl+A` or `Home` | Move cursor to start of line |
> | `Ctrl+E` or `End` | Move cursor to end of line |
> | `Ctrl+U` | Delete from cursor to start of line |
> | `Ctrl+K` | Delete from cursor to end of line |
> | `Ctrl+W` or `Ctrl+Backspace` | Delete word to the left |
> | `Ctrl+Left` / `Ctrl+Right` | Move cursor one word left/right |
>
> **Search prompt history**
>
> Prompt history search lets you find and reuse previously submitted prompts without leaving the chat input. Press `Ctrl+R` to open an inline search of prompts stored on your machine.
>
> **Reference a previous thread**
>
> Type `@@` in the chat input to search recent threads by their initial prompt, thread ID, agent, branch, or working directory. Select a result to insert an `@@(thread:<thread-id>)` reference. Deep Agents Code can inspect the referenced conversation when its context is relevant to your request.
>
> Add text after `@@` to filter the results. While the suggestions are open, press `Ctrl+R` to browse all threads with the same search. In the full thread picker, press `Ctrl+C` to copy the selected thread ID.
>
> > [!NOTE]
> >
> > **macOS `Cmd+Left` / `Cmd+Right` / `Cmd+Delete`**
> >
> > Terminal emulators intercept `Cmd`-modified keys before they reach the terminal process, so Deep Agents Code never receives them directly. Instead, the terminal translates them into the readline shortcuts above.
> >
> > * **Ghostty:** Works out of the box. `Cmd+Left`, `Cmd+Right`, and `Cmd+Delete` are translated to `Ctrl+A`, `Ctrl+E`, and `Ctrl+U` by default.
> > * **iTerm2:** Not bound by default. Add the following under **Settings > Profiles > Keys > Key Mappings** as `Send Text with vim special chars`:
> >   * `Cmd+Left` → `\x01` (Ctrl+A)
> >   * `Cmd+Right` → `\x05` (Ctrl+E)
> >   * `Cmd+Delete` → `\x15` (Ctrl+U)
> > * **Terminal.app:** No native UI for this remap. Use the `Ctrl`-based shortcuts directly.
> >
> > Word-wise motion (`Option+Left` / `Option+Right`) is handled the same way: terminals send `Esc+b` / `Esc+f`, which Deep Agents Code interprets as word-left/right.

### Inspect context-window usage

Run `/context` to open a color-coded report of the current model's context-window usage. The report shows the model's context limit, used tokens, remaining capacity, and a breakdown between the conversation and system prompt plus tools when those values are available.

Provider-reported totals remain distinct from local conversation estimates. When a provider total is unavailable, the report labels the conversation count as an estimate and marks the total usage as unavailable. Use `/tokens` when you want a text summary in the conversation transcript instead.

When usage grows faster than expected, run `/context-doctor` to attribute tokens to each injected component: the base system prompt, memory files, the skills index, built-in tool schemas, and MCP tool schemas. To confirm which tools the agent can currently call, run `/tools`. For the thread's running cost estimate, run `/cost`; the same figure appears in the status bar. See [Diagnose and audit a session](https://docs.langchain.com/oss/deepagents/code/cli-reference.md#diagnose-and-audit-a-session).

### External editor

Press `Ctrl+G` or type `/editor` to compose prompts in an external editor. Deep Agents Code checks `$VISUAL`, then `$EDITOR`, then falls back to `vi` (macOS/Linux) or `notepad` (Windows). GUI editors (VS Code, Cursor, Zed, etc.) automatically receive a `--wait` flag so Deep Agents Code blocks until you close the file.

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
# Set in your shell profile (~/.zshrc, ~/.bashrc, etc.)
export VISUAL="code"    # GUI editor (--wait auto-injected)
export EDITOR="nvim"    # Terminal fallback
```

## Non-interactive mode and piping

Use `-n` to run a single task without launching the interactive UI:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode -n "Write a Python script that prints hello world"
```

Each non-interactive run starts a fresh thread—conversation history does not carry between invocations. File-based state (memory, skills, configuration) persists.

You can also pipe input via stdin. When input is piped, Deep Agents Code automatically runs non-interactively:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
echo "Explain this code" | dcode
cat error.log | dcode -n "What's causing this error?"
git diff | dcode -n "Review these changes"
git diff | dcode --skill code-review -n 'summarize changes'
```

When you combine piped input with `-n` or `-m`, the piped content appears first, followed by the text you pass to the flag.

> [!NOTE]
>
> The maximum piped input size is 10 MiB.

Shell execution is disabled by default in non-interactive mode. Use `-S`/`--shell-allow-list` to enable specific commands (e.g., `-S "pytest,git,make"`), `recommended` for safe defaults, or `all` to permit any command.

> [accordion] Cap turn count
>
> Long-running or misbehaving agents in CI/CD pipelines can loop indefinitely. `--max-turns N` gives operators a hard upper bound without having to touch SDK internals:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> dcode -n "fix the failing tests" --max-turns 10
> ```
>
> `N` must be a positive integer, and overrides the internal safety default that otherwise caps runaway loops. Exits with code 124 (matching GNU `timeout`) when the budget is exceeded, so CI can distinguish a budget hit from a generic failure. Requires `-n` or piped stdin; otherwise exits with code 2.
>
> For a time-based limit instead of (or in addition to) a turn-count limit, see [Cap wall-clock time with `--timeout`](#non-interactive-mode-and-piping).

> [accordion] Cap wall-clock time
>
> `--timeout SECONDS` enforces a hard wall-clock limit on a non-interactive run. It complements `--max-turns` (turn count) with a time-based budget—whichever limit is hit first cancels the agent.
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> # Fail fast in CI if the task takes more than 2 minutes
> dcode -n "run the test suite and summarise failures" --timeout 120
>
> # Combine with --max-turns—whichever limit is hit first stops the agent
> dcode -n "refactor auth module" --timeout 300 --max-turns 20
> ```
>
> On expiry the agent is cancelled and the process exits with code 124, the same code used by `--max-turns`, so CI can treat both budget hits uniformly. Requires `-n` or piped stdin; otherwise exits with code 2.

> [accordion] Clean output and buffering
>
> Use `-q` for clean output suitable for piping into other commands, and `--no-stream` to buffer the full response (instead of streaming) before writing to stdout:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> dcode -n "Generate a .gitignore for Python" -q > .gitignore
> dcode -n "List dependencies" -q --no-stream | sort
> ```
>
> In non-interactive mode, the agent is instructed to make reasonable assumptions and proceed autonomously rather than ask clarifying questions. It also favors non-interactive command variants (e.g., `npm init -y`, `apt-get install -y`).

> [accordion] Shell execution examples
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> # Allow specific commands (validated against the list)
> dcode -n "Run the tests and fix failures" -S "pytest,git,make"
>
> # Use the curated safe-command list
> dcode -n "Build the project" -S recommended
>
> # Allow any shell command
> dcode -n "Fix the build" -S all
> ```

> [!WARNING]
>
> **Use with caution.**
>
> `-S all` (or `--shell-allow-list all`) lets the agent execute arbitrary shell commands with no human confirmation.

## Trace with LangSmith

> [!NOTE]
>
> This integration uses a legacy destination setting such as `project_name` or `LANGSMITH_PROJECT`. [Create an agent](https://docs.langchain.com/langsmith/create-an-agent.md), choose its environment, and [retrieve that environment's `tracer_session_name`](https://docs.langchain.com/langsmith/create-an-agent#use-an-environment-project). Use that returned value in the destination setting and any overrides. For integrations that support agent addressing, use the [agent identifier and environment](https://docs.langchain.com/langsmith/log-traces-to-agent.md) directly.

Enable [LangSmith](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=oss-deepagents-code-quickstart) tracing to see agent operations, tool calls, and decisions in a LangSmith agent environment.

Run `/auth` and add your LangSmith API key. Tracing is enabled on the next launch and persists across sessions. See [Provider credentials](https://docs.langchain.com/oss/deepagents/code/credentials.md#use-%2Fauth-recommended) for details on the credential manager.

To select the agent environment or configure tracing without the TUI, add keys to `~/.deepagents/.env` so tracing is enabled in every session without per-shell exports:

```bash title="~/.deepagents/.env" theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=lsv2_...
DEEPAGENTS_CODE_LANGSMITH_PROJECT="<coding-agent-tracer-session-name>"
```

Set `DEEPAGENTS_CODE_LANGSMITH_PROJECT` to the coding agent environment's `tracer_session_name`. This setting controls Deep Agents Code's own traces. A separate `LANGSMITH_PROJECT` in your repository's `.env` controls traces from code run in shell commands. See **Separate coding-agent traces from shell-command traces** below.

To override the destination for a specific working directory, add `DEEPAGENTS_CODE_LANGSMITH_PROJECT` to a `.env` in that directory. See [environment variables](https://docs.langchain.com/oss/deepagents/code/configuration#environment-variables) for the full loading order.

> [accordion] Disable agent tracing
>
> Launch `dcode --no-tracing` to disable LangSmith agent tracing for one session. For future launches, set `DEEPAGENTS_CODE_LANGSMITH_TRACING=false` in your shell or `~/.deepagents/.env`.
>
> The flag overrides the environment setting. An explicit `false` in `DEEPAGENTS_CODE_LANGSMITH_TRACING` overrides other tracing-enable flags, including `LANGSMITH_TRACING=true`. Shell exports take precedence over `.env` values for the same variable.
>
> Disabling agent tracing does not disable local conversation history or model-provider requests. It is separate from trace redaction and tracing emitted by agents run in shell commands.

> [accordion] Separate coding-agent traces from shell-command traces
>
> Deep Agents Code can produce two kinds of LangSmith traces:
>
> * `Agent traces` are Deep Agents Code's own model calls, tool calls, orchestration, and middleware.
> * `Shell-command traces` are traces emitted by code that Deep Agents Code runs for you in a shell, such as tests, scripts, or a local LangGraph agent.
>
> To select the environment for Deep Agents Code's own traces, set `DEEPAGENTS_CODE_LANGSMITH_PROJECT`:
>
> ```bash title="~/.deepagents/.env" theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> # Use the coding agent environment's tracer_session_name.
> DEEPAGENTS_CODE_LANGSMITH_PROJECT="<coding-agent-tracer-session-name>"
> ```
>
> For code run in shell commands, this compatibility example uses `LANGSMITH_PROJECT` with a different agent environment's `tracer_session_name`:
>
> ```bash title=".env" theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> LANGSMITH_PROJECT="<shell-agent-tracer-session-name>"
> ```
>
> For example, suppose you ask Deep Agents Code to debug a failing LangGraph test:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv run pytest tests/test_escalation_flow.py
> ```
>
> If the test runs your agent with LangSmith tracing enabled, its traces go to the environment configured for that agent. Deep Agents Code's own reasoning and tool-use traces go to the coding agent's environment.
>
> You can also scope LangSmith credentials to Deep Agents Code using the [`DEEPAGENTS_CODE_` prefix](https://docs.langchain.com/oss/deepagents/code/configuration#deepagents_code_-prefix) (e.g., `DEEPAGENTS_CODE_LANGSMITH_API_KEY`).

> [accordion] Copy traces to a second agent environment
>
> To mirror traces to a second agent environment, set `DEEPAGENTS_CODE_LANGSMITH_REPLICA_PROJECTS`. This is useful for sending the same traces to agent environments in both a personal workspace and a shared team workspace.
>
> ```bash title="~/.deepagents/.env" theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> DEEPAGENTS_CODE_LANGSMITH_REPLICA_PROJECTS="<replica-tracer-session-name>"
> ```
>
> When set and tracing is active, each run is written to both the primary destination and the replica destination. Set both variables to the corresponding environments' `tracer_session_name` values. Leave the replica variable unset to write to one environment.

When configured, Deep Agents Code displays a status line with a link to its trace destination in LangSmith. In supported terminals, click the link to open it directly. You can also use `/trace` to print the URL and open it in your browser.

```sh theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
✓ LangSmith tracing: '<coding-agent-tracer-session-name>'
```

> [!TIP]
>
> We recommend you also set up [LangSmith Engine](https://docs.langchain.com/langsmith/engine.md), which monitors your traces, detects issues, and proposes fixes.

## See also

* [Deep Agents Code overview](https://docs.langchain.com/oss/deepagents/code/overview.md)
* [Configuration](https://docs.langchain.com/oss/deepagents/code/configuration)
* [Provider credentials](https://docs.langchain.com/oss/deepagents/code/credentials.md)
* [CLI reference](https://docs.langchain.com/oss/deepagents/code/cli-reference.md)
* [Providers](https://docs.langchain.com/oss/deepagents/code/providers.md)
* [Memory and skills](https://docs.langchain.com/oss/deepagents/code/memory-and-skills.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/deepagents/code/quickstart.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
