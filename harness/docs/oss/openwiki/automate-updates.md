> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Automate updates

> Schedule OpenWiki documentation updates with GitHub Actions, GitLab CI, or Bitbucket Pipelines

OpenWiki can refresh repository documentation on a schedule and open a pull request or merge request when the wiki changes. Example workflows ship in the [OpenWiki repository](https://github.com/langchain-ai/openwiki/tree/main/examples).

## Add a workflow

Copy the example for your Git provider:

| Provider | Example | Destination |
| - | - | - |
| GitHub Actions | [`openwiki-update.yml`](https://github.com/langchain-ai/openwiki/blob/main/examples/openwiki-update.yml) | `.github/workflows/openwiki-update.yml` |
| GitLab CI | [`openwiki-update.gitlab-ci.yml`](https://github.com/langchain-ai/openwiki/blob/main/examples/openwiki-update.gitlab-ci.yml) | `.gitlab-ci.yml`, or include it from an existing pipeline |
| Bitbucket Pipelines | [`openwiki-update.bitbucket-pipelines.yml`](https://github.com/langchain-ai/openwiki/blob/main/examples/openwiki-update.bitbucket-pipelines.yml) | `bitbucket-pipelines.yml`, then schedule the `openwiki-update` custom pipeline |

## Run update in CI

For repository documentation in CI, use:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
openwiki code --update --print
```

You do not need to run `--init` in CI. `--update` creates the initial `openwiki/` docs if they do not exist yet, as long as the workflow provides the required provider and model environment variables.

Provide credentials as repository secrets or CI variables. Typical values include:

* Provider API key (or Copilot OAuth token / cloud credentials for keyless providers)
* `OPENWIKI_PROVIDER`
* `OPENWIKI_MODEL_ID`
* Optional `LANGSMITH_API_KEY` for tracing
* Optional `OPENWIKI_LANGSMITH_API_KEY` when the repository uses the [LangSmith connector](https://docs.langchain.com/oss/openwiki/code-mode.md#langsmith-connector)

The scheduled workflow includes generated wiki files, Claims under `openwiki/.claims/`, `AGENTS.md`, `CLAUDE.md`, and the workflow itself in the documentation pull request when those files change.

Repository generation is resumable on persistent workspaces through `openwiki/.run.json` and `openwiki/.page-manifest.json`. Ephemeral CI runners start fresh after failure unless their workspace is preserved, and OpenWiki can still publish partial progress when a run fails after some pages complete.

## Empty updates

A clean `--update` run skips model work and leaves wiki content untouched while refreshing `openwiki/.last-update.json` so freshness checks reflect that the check ran. If the wiki Markdown and Claims do not change, CI does not open a pull request.

## Telemetry in CI

Scheduled and CI runs send anonymous reliability telemetry under a shared CI identifier. To disable telemetry in CI, set:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
OPENWIKI_TELEMETRY_DISABLED=1
```

You can uncomment the corresponding line in the example workflow. To understand what is collected and how to opt out locally, see [Customize OpenWiki](https://docs.langchain.com/oss/openwiki/customize.md#telemetry).

## See also

* [Code mode](https://docs.langchain.com/oss/openwiki/code-mode.md)
* [Model providers](https://docs.langchain.com/oss/openwiki/providers.md)
* [CLI reference](https://docs.langchain.com/oss/openwiki/cli-reference.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/openwiki/automate-updates.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
