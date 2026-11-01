> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Search and link wikis

> Search OpenWiki pages over MCP and group related repository wikis into workspaces

OpenWiki exposes local, read-only retrieval tools so coding agents can answer concrete questions from an existing wiki without starting a generation run. Use `openwiki link` to group related repository wikis into a named workspace, then search and read across that workspace.

## Search and read

Ask your coding agent to look up a question in the wiki:

```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
Search this repository's OpenWiki for how retry handling works, then read the
relevant sections.
```

* **`openwiki_search`**: Returns compact, ranked results for a concrete question
* **`openwiki_read`**: Retrieves the complete sections your agent selects from those results

Agents are guided to use retrieval for concrete architecture or dependency questions, read the relevant sections, and stop once grounded. Retrieval is optional context, not a routine step at the start of every task. The tools are model-free: they do not start a generation run or make their own model calls.

When repositories are linked into workspaces, `openwiki_list_workspaces` and `openwiki_list_wikis` provide discovery. Workspace search results identify their supplying `wiki`; pass that ID to `openwiki_read` with the result's page and section anchors.

These tools ship with [coding-agent integrations](https://docs.langchain.com/oss/openwiki/integrations.md). After you install an integration, restart the host and ask it to search the wiki.

## Create wiki workspaces

A service often spans several repositories. Use `openwiki link` to group their existing wikis into a named workspace so your agent can search across the whole service and read sections from any member wiki.

Start the workspace manager from a directory that contains the repositories, for example `~/dev`:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
cd ~/dev
openwiki link
```

Create a workspace such as `Payments`, then select the repositories to include. Member repositories keep their own wikis, can belong to more than one workspace, and do not need to share a parent directory. The finder streams Git repositories below an editable path starting at the launch directory. Repositories without OpenWiki documentation are dimmed. The registry is stored under `~/.openwiki` (or `OPENWIKI_CONFIG_DIR`).

### Search scope

Search chooses its scope as follows:

1. A wiki in no workspace searches itself.
2. A wiki in one workspace searches that workspace automatically.
3. A wiki in multiple workspaces uses its active workspace.
4. If it belongs to multiple workspaces without an active selection, search returns `workspace_required` and the agent asks which workspace to use.

Set, inspect, or clear a repository's persistent active workspace from anywhere inside that repository:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki workspace use payments
openwiki workspace current
openwiki workspace clear
```

## See also

* [Coding-agent integrations](https://docs.langchain.com/oss/openwiki/integrations.md)
* [Code mode](https://docs.langchain.com/oss/openwiki/code-mode.md)
* [CLI reference](https://docs.langchain.com/oss/openwiki/cli-reference.md)
* [Visualize your wiki](https://docs.langchain.com/oss/openwiki/visualize.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/openwiki/search-and-link.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
