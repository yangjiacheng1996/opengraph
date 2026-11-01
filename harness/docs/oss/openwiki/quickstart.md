> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Quickstart

> Install OpenWiki, configure a model provider, and generate your first wiki.

OpenWiki is a CLI that writes and maintains a Markdown wiki for your codebase or personal knowledge. Coding agents use that wiki as durable context, so they spend less time and fewer tokens rediscovering architecture, integrations, and other repository details. Humans can read the same docs, but agents are the primary audience. This guide covers installation, provider setup, and your first documentation run. For a feature overview, see [OpenWiki overview](https://docs.langchain.com/oss/openwiki/overview.md).

## Install and generate repository docs

**1. Install the CLI**

OpenWiki requires [Node.js 22.22.0 or later](https://nodejs.org).

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
npm install -g openwiki
```

On Windows, prefer `npm` or `pnpm`. Installing with Bun can fall back to compiling the native `better-sqlite3` dependency and may require Visual Studio Build Tools with the Desktop development with C++ workload.

**2. Initialize in your repository**

From the repository root, run the following command:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki --init
```

On the first interactive run, OpenWiki prompts for:

* An inference provider and model
* The provider API key (or equivalent credentials)
* An optional LangSmith API key for tracing
* LangSmith projects to enrich the wiki from runtime traces

OpenWiki saves its configuration and secrets to `~/.openwiki/.env`.

Running `--init` again regenerates the repository wiki and Claims from scratch while preserving `openwiki/INSTRUCTIONS.md`. Interrupted runs on a persistent checkout resume from `openwiki/.run.json`.

To run OpenWiki inside a coding agent instead of a standalone model session, see [Coding-agent integrations](https://docs.langchain.com/oss/openwiki/integrations.md). After the wiki exists, you can [search it or link related repositories](https://docs.langchain.com/oss/openwiki/search-and-link.md).

**3. Review the generated wiki**

OpenWiki writes documentation to `openwiki/` in the repository, including a quickstart entrypoint and topic pages. It creates or refreshes `AGENTS.md` at the repository root with a block that instructs coding agents to consult the wiki for codebase context. When `CLAUDE.md` already exists, OpenWiki refreshes a managed block that imports `@AGENTS.md`. Factual pages are grounded with Claims under `openwiki/.claims/`.

Repository-specific wiki instructions live in `openwiki/INSTRUCTIONS.md`. OpenWiki reads this file for scope and priorities. To change it, edit the file, or ask OpenWiki in chat to change the brief (for example, `openwiki "Update openwiki/INSTRUCTIONS.md to focus on the public API"`). Normal `--init` and `--update` runs do not rewrite it.

To explore the wiki in a browser, run:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki visualize
```

This opens a local interactive node graph with a side-by-side Markdown reader. See [Visualize your wiki](https://docs.langchain.com/oss/openwiki/visualize.md).

**4. Keep docs up to date**

Refresh documentation after code changes:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki --update
```

In code mode, updates also reconcile stale Claims when source evidence changes. For automated updates in CI, see [Automate updates](https://docs.langchain.com/oss/openwiki/automate-updates.md).

## Personal wiki (optional)

To initialize a local personal brain instead of repository docs:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki personal --init
```

Personal mode writes to `~/.openwiki/wiki` and can ingest configured connectors such as local git repositories, Custom MCP, Gmail, Notion, web search, Hacker News, and X/Twitter. See [Personal mode](https://docs.langchain.com/oss/openwiki/personal-mode.md).

## Interactive and one-shot runs

Bare `openwiki` opens an interactive session in code mode for the current repository. Pass a message to start with a request:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki "Please generate documentation for this repository"
```

Use `-p` / `--print` for a one-shot non-interactive run that prints the final assistant output and exits:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki -p "Summarize what you can do"
```

In chat, use `/api-key` to update the current provider API key, `/langsmith-key` to update or clear LangSmith tracing credentials, and `/effort` to set reasoning effort for supported models.

## Trace with LangSmith

During onboarding, provide a LangSmith API key to trace OpenWiki runs to a LangSmith project named `openwiki`. You can also set these values in `~/.openwiki/.env` or the process environment:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
LANGSMITH_API_KEY=your-key
LANGCHAIN_TRACING_V2=true
LANGCHAIN_PROJECT=openwiki
```

To enrich the repository wiki from LangSmith traces (separate from tracing OpenWiki itself), see [LangSmith connector](https://docs.langchain.com/oss/openwiki/code-mode.md#langsmith-connector).

## Next steps

* [Code mode](https://docs.langchain.com/oss/openwiki/code-mode.md): repository wikis, Claims, OKF output, and agent instruction files
* [Coding-agent integrations](https://docs.langchain.com/oss/openwiki/integrations.md): run OpenWiki inside Codex, Claude Code, OpenCode, Cursor, IBM Bob, Kiro, Oh My Pi, Antigravity, or Pi
* [Search and link wikis](https://docs.langchain.com/oss/openwiki/search-and-link.md): MCP search and read, plus multi-repo workspaces
* [Personal mode](https://docs.langchain.com/oss/openwiki/personal-mode.md): local brain and connectors
* [Model providers](https://docs.langchain.com/oss/openwiki/providers.md): supported providers and credentials
* [Automate updates](https://docs.langchain.com/oss/openwiki/automate-updates.md): GitHub Actions, GitLab CI, and Bitbucket Pipelines
* [CLI reference](https://docs.langchain.com/oss/openwiki/cli-reference.md): commands and flags

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/openwiki/quickstart.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
