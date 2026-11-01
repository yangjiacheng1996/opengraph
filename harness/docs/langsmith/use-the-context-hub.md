> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Use the Context Hub

> Learn how to create, view, and promote context in the LangSmith Context Hub.

The **Context Hub** gives your team version-controlled, environment-aware management of the instructions and tools your agents use in production. A *context* is a versioned bundle of agent instructions and tools, either a skill or a full agent, that you manage in LangSmith.

Use this guide to create your first context, view its files and history, and promote it to an environment so your agents can pull it.

## 1. Open the Context Hub

> [!NOTE]
>
> If you don't see **Context** in the left-hand navigation, verify that Context Hub is enabled for your workspace and that you have the required permissions.

In the [LangSmith UI](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-use-the-context-hub), select **Context** in the
left-hand navigation. The Context Hub lists every agent and skill in your workspace.

![The Context Hub home view listing existing Agent and Skill contexts with their commit metadata.](https://mintcdn.com/langchain-5e9cc07a/VMau0Ih_5sR7Yf36/langsmith/images/context-hub-home.png?fit=max\&auto=format\&n=VMau0Ih_5sR7Yf36\&q=85\&s=7067ab68c361e53e17514009ae3405b0)

## 2. Create a context

1. Click **+ Create** in the top left of the Context Hub.

2. Choose the context type:

   * **Agent:** a full agent bundle including an `AGENTS.md` file and tools.
   * **Skill:** a reusable capability that agents can use, including a `SKILL.md` file.

   ![The Create dropdown showing the Agent and Skill context type options.](https://mintcdn.com/langchain-5e9cc07a/VMau0Ih_5sR7Yf36/langsmith/images/context-hub-create-context.png?fit=max\&auto=format\&n=VMau0Ih_5sR7Yf36\&q=85\&s=8ff29b4fac4c167d55ca593fb943a02a)

3. Fill in a name and description. For skills, a description is required. You can also enter the initial file contents (`SKILL.md` for a skill, `AGENTS.md` for an agent) now, or add them after creation. Click **Create Agent** or **Create Skill**. LangSmith creates the repo and opens it for editing.

## 3. View a context

Click on an agent or skill from the Context Hub to view it.

![An Agent context with an AGENTS.md file open, showing the environments panel, commit history, and file tree.](https://mintcdn.com/langchain-5e9cc07a/VMau0Ih_5sR7Yf36/langsmith/images/context-hub-agent-view.png?fit=max\&auto=format\&n=VMau0Ih_5sR7Yf36\&q=85\&s=a333c5d93ad3532eb10c30053f53c267)

The middle panel shows the file tree for the current commit and the right panel previews the selected file. Click a file in the middle panel to open it.

Markdown files open in **Preview**, which is a read-only rendering. To change a file, switch to the **Edit** tab in the top right of the right panel: it shows the exact text in the file. Save your changes to create a new commit.

Each save creates a new **commit** in the history panel on the left. Select a commit to browse its files. At this time, Context Hub does not provide a file comparison view or an action to restore a prior version as the latest commit.

## 4. Tag and promote a commit

Once a commit is ready to ship, promote it to an environment so downstream
agents can pull it.

> [!NOTE]
>
> Context Hub currently supports two environment tags for promotion: `staging` and `production`.

1. With the target commit selected, click **Promote** in the top right.

2. Choose the destination environment:

   * **Promote to Production:** the commit your production agents pull.
   * **Promote to Staging:** a pre-production environment for validation.

   ![The Promote dropdown with options to promote a commit to Production or Staging.](https://mintcdn.com/langchain-5e9cc07a/VMau0Ih_5sR7Yf36/langsmith/images/context-hub-promote.png?fit=max\&auto=format\&n=VMau0Ih_5sR7Yf36\&q=85\&s=96dc0abf55f702cb873f78eaa99429fe)

3. The environment label (for example, `Production 7ca95573`) moves to the
   promoted commit. Use the **Tag** button next to **Promote** to attach a
   human-readable label to any commit for easy reference.

Agent runtimes that resolve context by environment tag (for example, `:production`) now pull this promoted commit.

To use a prior version in an environment, select that commit and promote it to `staging` or `production`. This moves the environment tag to the selected commit. It does not change the latest files or create a revert commit.

## Next steps

* [Context engineering concepts](https://docs.langchain.com/langsmith/context-engineering-concepts.md): learn about skills, agents, versioning, and sharing.
* [Manage contexts with the SDK](https://docs.langchain.com/langsmith/manage-contexts-sdk.md): push, pull, list, and delete contexts programmatically.
* [Configure commit webhooks](https://docs.langchain.com/langsmith/context-hub-webhooks.md): send workspace Context Hub commits to an external HTTPS endpoint.
* [Mount a Context Hub repo in a sandbox](https://docs.langchain.com/langsmith/sandbox-mounts.md#mount-a-context-hub-repo): give sandbox code read-only filesystem access to a repo that stays in sync as it changes.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/use-the-context-hub.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
