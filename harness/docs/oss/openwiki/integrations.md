> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Coding-agent integrations

> Run OpenWiki inside Codex, Claude Code, GitHub Copilot, OpenCode, Cursor, IBM Bob, Kiro, Oh My Pi, or Antigravity using the host model and tools

OpenWiki can run inside an existing coding agent instead of launching its own model. The coding agent investigates the repository, plans the wiki, and writes each assigned page with its native repository tools. OpenWiki owns the durable page-job lifecycle, Claims validation and persistence, source-drift handling, and deterministic finalization.

Host-driven runs support only repository [code mode](https://docs.langchain.com/oss/openwiki/code-mode.md) wikis, not [personal mode](https://docs.langchain.com/oss/openwiki/personal-mode.md). They use the coding agent's authenticated model session, so OpenWiki provider credentials are not required. Connector-sourced context, including LangSmith, is not yet supported on this path. Host-driven page authoring is sequential; [parallel page workers](https://docs.langchain.com/oss/openwiki/code-mode.md#parallel-page-workers) apply to the native CLI only.

## Install an integration

Supported hosts are **Codex**, **Claude Code**, **GitHub Copilot**, **OpenCode**, **Cursor**, **IBM Bob** (Bob Shell), **Kiro**, **Oh My Pi** (`omp`), and **Antigravity CLI**. Installations default to user level, so one install works from any Git repository:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki integrations install codex
openwiki integrations install claude
openwiki integrations install copilot
openwiki integrations install opencode
openwiki integrations install cursor
openwiki integrations install bob
openwiki integrations install kiro
openwiki integrations install omp
openwiki integrations install antigravity
```

Restart the coding agent after installation. Then open the repository and ask:

```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
Initialize this repository's OpenWiki from the current source and tests.
```

For an existing wiki, ask:

```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
Update this repository's OpenWiki for changes since its last successful run.
```

Installed integrations also expose [search and read tools](https://docs.langchain.com/oss/openwiki/search-and-link.md) so the host can query an existing wiki without starting a generation run.

### Inspect or uninstall

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki integrations list
openwiki integrations uninstall <codex|claude|copilot|opencode|cursor|bob|kiro|omp|antigravity>
```

Add `--project [path]` to `list`, `install`, or `uninstall` for repository-scoped state. Project paths resolve to the Git repository root.

Host-specific user-level locations:

* **OpenCode**: `~/.config/opencode`
* **IBM Bob / Bob Shell**: `~/.agents/skills` and `~/.bob/settings/mcp.json` (project scope uses `.agents/skills` and `.bob/mcp.json`)
* **Kiro**: `~/.kiro/skills` and `~/.kiro/settings/mcp.json`
* **Oh My Pi**: `~/.omp/agent` for the default profile. Use `--project` for named profiles or a relocated `PI_CODING_AGENT_DIR`
* **Antigravity CLI**: `~/.gemini/antigravity-cli/skills` and `~/.gemini/config/mcp_config.json`

## Pi package

Pi loads OpenWiki's skill and lifecycle tools from the npm package:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
pi install npm:openwiki
```

Use `pi install --local npm:openwiki` for a project-local install. The Pi extension starts OpenWiki's CLI through Node, so `openwiki` need not be on `PATH`. Pi is not a target of `openwiki integrations install`.

## How host-driven runs work

Host-driven runs follow the same resumable page-job lifecycle as native `openwiki --init` and `openwiki --update`. The coding agent drives repository research and page authoring; OpenWiki validates each completion, persists [Grounded Claims](https://docs.langchain.com/oss/openwiki/code-mode.md#grounded-claims), and finalizes metadata through MCP.

On each page, the host submits sparse Claim decisions (confirm, revise, add, or retract). OpenWiki retains unaffected Claims, applies the explicit decisions, and refuses to finish until the final Claims state is durable. The host can call `openwiki_inspect_page_claims` when it needs the complete Claims set for the current page.

## See also

* [Search and link wikis](https://docs.langchain.com/oss/openwiki/search-and-link.md)
* [Code mode](https://docs.langchain.com/oss/openwiki/code-mode.md)
* [CLI reference](https://docs.langchain.com/oss/openwiki/cli-reference.md)
* [Quickstart](https://docs.langchain.com/oss/openwiki/quickstart.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/openwiki/integrations.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
