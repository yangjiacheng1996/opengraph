> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Revisions

> Create and manage revisions for a LangSmith Cloud deployment.

A revision deploys a version of your application to an existing Cloud deployment. Create a revision to release code changes without creating another deployment.

## Create a revision

When you [create a deployment](https://docs.langchain.com/langsmith/deploy-to-cloud.md), LangSmith creates its first revision. Use the LangSmith UI or LangGraph CLI to create subsequent revisions.

> [tab] LangSmith UI
>
> From the [LangSmith UI](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-deployment-revisions), select **Deployments**, then select a deployment.
>
> 1. In the top-right corner of the **Deployment** view, select **+ New Revision**.
> 2. In the **New Revision** modal, specify the full path to the [API configuration file](https://docs.langchain.com/langsmith/cli.md#configuration-file), including the file name. For example, enter `langgraph.json` if the file is in the repository root.
> 3. Choose whether to make the deployment shareable through [Studio](https://docs.langchain.com/langsmith/studio.md).
> 4. Add, remove, or update environment variables and secrets. Existing values are prepopulated. For more information, see [Environment variables](https://docs.langchain.com/langsmith/env-var-cloud.md).
> 5. Select **Submit**. LangSmith queues the revision for deployment.

> [tab] LangGraph CLI
>
> Run `langgraph deploy` again from your project directory. The command finds the existing deployment by name and creates a revision with your latest code changes:
>
> ```shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> langgraph deploy
> ```
>
> To target a deployment by ID, run:
>
> ```shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> langgraph deploy --deployment-id <DEPLOYMENT_ID>
> ```
>
> To view deployment IDs, run:
>
> ```shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> langgraph deploy list
> ```
>
> > [!NOTE]
> >
> > `langgraph deploy` can update only deployments originally created with `langgraph deploy`. Use the LangSmith UI to update deployments created through the UI or GitHub integration.

## Interrupt a revision

Interrupt a revision only when it is stuck and prevents you from deploying another revision.

> [!WARNING]
>
> Interrupted revisions have undefined behavior. LangChain might remove this feature in the future.

To interrupt a revision:

1. From the **Deployments** view, select a deployment.
2. In the **Revisions** table, select the menu icon for the revision.
3. Select **Interrupt**.
4. Review the confirmation message, then select **Interrupt revision**.

## See also

* [Monitor a deployment](https://docs.langchain.com/langsmith/monitor-deployment.md)
* [Manage a deployment](https://docs.langchain.com/langsmith/manage-deployment.md)

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/deployment-revisions.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
