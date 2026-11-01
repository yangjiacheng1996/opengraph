> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Visualize your wiki

> Explore an OpenWiki Markdown wiki with a local interactive node graph and reader

For exploring OpenWiki Markdown wikis, `openwiki visualize` serves a local interactive node graph beside a live Markdown reader in your browser. You can also export the same graph and reader as a static directory for hosting.

## Open the visualizer

From a repository that already has an `openwiki/` directory:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki visualize
```

This serves `./openwiki` on `127.0.0.1:4321` and opens your browser to the graph. Edits to wiki files are picked up automatically while the server runs. The graph panel is resizable and collapsible beside the Markdown reader.

## Options

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki visualize openwiki --port 4400 --no-open
```

| Argument / flag | Description |
| - | - |
| `[path]` | Wiki directory to serve. Defaults to `./openwiki` |
| `--port <port>` | Preferred port. Defaults to `4321`. Increments if the port is already in use |
| `--no-open` | Do not open the browser automatically |
| `--export <dir>` | Write a static visualizer directory instead of starting the local server. Cannot be combined with `--port` or `--no-open` |

To explore a personal wiki:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki visualize ~/.openwiki/wiki
```

![OpenWiki visualizer with an interactive node graph beside a live Markdown reader](https://mintcdn.com/langchain-5e9cc07a/BPy4qr0YTTF2625M/oss/images/openwiki/visualizer.gif?s=fe0362d13c1436c8a9f1a95cc864c447)

The visualizer shows:

* An interactive node graph of wiki concepts and the Markdown links between them
* A side-by-side live Markdown reader for the selected page

The graph does not show `INSTRUCTIONS.md` and other scaffolding files.

## Export a static site

To publish the visualizer beside generated documentation, export a static directory instead of starting the server:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki visualize <PATH> --export docs/openwiki-visualizer
```

The export contains `index.html`, `client.js`, `client-lib.js`, `styles.css`, and `graph.json`. Its client reads the sibling graph file and does not use live reload, so the directory can be hosted by GitHub Pages, MkDocs, or any other static host.

> [!NOTE]
>
> The page loads its graph, Markdown, and diagram libraries from a public CDN, so an internet connection is required for both local and static viewers.

## See also

* [Quickstart](https://docs.langchain.com/oss/openwiki/quickstart.md)
* [Code mode](https://docs.langchain.com/oss/openwiki/code-mode.md)
* [CLI reference](https://docs.langchain.com/oss/openwiki/cli-reference.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/openwiki/visualize.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
