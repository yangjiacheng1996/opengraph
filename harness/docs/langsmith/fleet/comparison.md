> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agent platform comparison

> Compare LangSmith Fleet with Claude Cowork, Amazon Quick, Google Workspace Studio, and Microsoft Copilot to choose the right enterprise agent platform for your team

[**LangSmith Fleet**](https://docs.langchain.com/langsmith/fleet/index.md) is an enterprise agent platform for building, sharing, and governing agents across your organization. This page compares it with similar platforms to help you choose the right one for your team.

| **Platform** | **Choose if...** |
| - | - |
| [LangSmith Fleet](https://docs.langchain.com/langsmith/fleet/index.md) | You want to build and share purpose-built agents across your organization, stay model-agnostic, and keep full observability via LangSmith. **Fleet** is the only option with a self-hosted deployment path and the ability to export agents to code via [Deep Agents](https://docs.langchain.com/oss/python/deepagents/overview.md). |
| Claude Cowork | You want to delegate open-ended tasks to Claude from the desktop for personal knowledge work, and on-device data storage meets your privacy requirements. |
| Amazon Quick | You are already on AWS and want an AI assistant with direct access to your AWS data sources and enterprise integrations. |
| Google Workspace Studio | Your organization runs on Google Workspace and you want no-code agents that work natively inside Gmail, Drive, and Sheets without leaving the Google ecosystem. |
| Microsoft Copilot | Your organization runs on Microsoft 365 and you want low-code agents (via Copilot Studio) that publish natively to Teams and Microsoft 365 Copilot, governed through the Power Platform admin center. |

## Compare capabilities

* ❌ Not available
* ⚠️ Partial or limited
* ❓ Not confirmed from public documentation.

| **Aspect** | **LangSmith Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| **Primary use case** | Teams building purpose-built agents to share across an organization, with no-code creation and code export for custom deployments; individuals using a general-purpose chat agent for any task | Individual desktop knowledge work | Enterprise AI with AWS data integration | No-code agents for Google Workspace | Low-code agents for Microsoft 365 |
| **Model support** | Model-agnostic: any LLM with an OpenAI-compatible or Anthropic-compatible API | Claude only | ❓ | Gemini 3 | Curated OpenAI + Anthropic models; bring-your-own via Azure AI Foundry |
| **Interface** | Web app, Slack app, Teams app, API | Desktop, mobile, Slack, M365 connectors | Web, desktop, browser extensions, Slack, Teams | Web app, Gmail and Chat sidebars | Teams, M365 apps, web, mobile, Windows, Copilot Studio |
| **Deployment** | Cloud (LangSmith) or self-hosted | Local by default; remote on Anthropic cloud | Cloud (AWS) | Cloud (Google) | Cloud (Microsoft) |
| **Self-hosting** | ✅ [beta](https://docs.langchain.com/langsmith/enable-self-hosted-fleet.md), [contact our sales team](https://www.langchain.com/contact-sales) for production readiness details | ❌ | ❌ | ❌ | ❌ |
| **Code export** | ✅ [Export to Deep Agents](https://docs.langchain.com/langsmith/fleet/code.md) | ❌ | ❌ | ❌ | ❌ |
| **Observability** | LangSmith tracing and evaluations at scale | OpenTelemetry to SIEM | CloudTrail + run logs | Activity tab + audit logs | App Insights + Purview |
| **Platform license** | Proprietary | Proprietary | Proprietary | Proprietary | Proprietary |
| **Code export license** | MIT ([Deep Agents](https://docs.langchain.com/oss/python/deepagents/overview.md)) | N/A | N/A | N/A | N/A |

### Target users

**Fleet** covers both org-wide and personal use cases. Teams can build purpose-built agents to share across an organization (for example, a vendor intake agent that serves an entire ops org, or a weekly report agent that saves every account manager thirty minutes on Monday morning), and any user can get help with any task using any tool via Fleet's general-purpose default chat. Other platforms focus on individual productivity, ecosystem-specific automation, or both, but none combine no-code agent building with org-wide sharing and code export.

**Fleet** also lets you set tool-level approval requirements so agents check with you before executing sensitive steps, with a [centralized inbox](https://smith.langchain.com/agents/inbox) for reviewing, editing, and approving actions. No other platform in this comparison offers a single centralized approvals inbox spanning all agents.

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| General-purpose chat agent | ✅ [Fleet chat](https://smith.langchain.com/agents?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=langsmith-fleet-comparison) | ✅ | ✅ | ❌ | ✅ |
| No-code agent builder | ✅ | ❌ | ✅ | ✅ | ✅ |
| Slack-native integration | ✅ [Native Slack app](https://docs.langchain.com/langsmith/fleet/slack-app.md) | ✅ | ✅ | ⚠️ | ⚠️  (via Azure Bot Service) |
| Microsoft Teams integration | ✅ [Teams app](https://docs.langchain.com/langsmith/fleet/teams-app.md) | ✅ | ✅ | ❌ | ✅ |
| Scheduled runs | ✅ [Schedules](https://docs.langchain.com/langsmith/fleet/schedules.md) | ✅ | ✅ | ✅ | ✅ |
| Sub-agents | ✅ [Sub-agents](https://docs.langchain.com/langsmith/fleet/essentials.md#sub-agents) | ✅ | ✅ | ❌ | ✅ |
| Skills system | ✅ [Skills](https://docs.langchain.com/langsmith/fleet/skills.md) | ✅ | ❌ | ❌ | — |
| Human-in-the-loop | ✅ [Central approvals inbox](https://docs.langchain.com/langsmith/fleet/essentials.md#human-in-the-loop) | ✅ | ✅ | ⚠️ | ⚠️ |
| MCP client | ✅ [Remote MCP servers](https://docs.langchain.com/langsmith/fleet/remote-mcp-servers.md) | ✅ | ✅ | ❌ | ✅ |
| Web search | ✅ (via Exa, Tavily) | ✅ | ✅ | ✅ | ✅ |

### Enterprise controls and access

**Fleet** provides RBAC, attribute-based access control, and per-agent sharing permissions (Clone, Run, and Edit). Among the platforms compared here, only Fleet documents per-MCP-server attribute-based access control. All platforms offer some form of RBAC, but granularity varies.

**Fleet** manages spending at the workspace level. For enterprise billing options, [contact our sales team](https://www.langchain.com/contact-sales).

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| Role-based access control | ✅ [RBAC with per-tool permissions](https://docs.langchain.com/langsmith/rbac.md) | ✅ | ✅ | ✅ | ✅ |
| Attribute-based access control | ✅ [Per MCP server and integration](https://docs.langchain.com/langsmith/fleet/access-and-oversight.md#attribute-based-access-control) | ❌ | ❌ | ❌ | — |
| Per-agent sharing and permissions | ✅ [Clone, Run, and Edit access per agent](https://docs.langchain.com/langsmith/fleet/access-and-oversight.md#permissions-and-sharing) | ⚠️ | ✅ | ⚠️ | ✅ |
| Credential model (fixed or per-user) | ✅ [Configurable per agent](https://docs.langchain.com/langsmith/fleet/access-and-oversight.md#agent-identity-and-credentials) | ✅ | ✅ | ✅ | ✅ |
| Spend limits | ⚠️ Managed at workspace level | ✅ | ⚠️ | ⚠️ | ✅ |
| SCIM provisioning | ✅ | ✅ | ✅ | — | ✅ |
| Audit trail | ✅ [Structured LangSmith traces](https://docs.langchain.com/langsmith/fleet/access-and-oversight.md#observability-and-audit-trail) | ✅ | ✅ | ✅ | ✅ |

### Model flexibility

**Fleet** supports any LLM via the OpenAI or Anthropic chat spec, including self-hosted providers, with no ecosystem dependency. Microsoft Copilot offers curated multi-vendor models and a bring-your-own path via Azure AI Foundry, but full flexibility requires Azure infrastructure. Google Workspace Studio and Amazon Quick are more constrained to their respective vendor ecosystems.

Of the platforms compared here, only Fleet works with any OpenAI- or Anthropic-compatible API endpoint regardless of cloud provider.

### Memory, self-updates, and learning

**Fleet** agents can persist context across conversations using a dedicated memory system, and can update their own instructions, add tools, or remove tools as they learn from interactions. Of the platforms compared here, only Fleet documents agent self-modification at runtime.

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| Long-term memory | ✅ [Persistent memory files across sessions](https://docs.langchain.com/langsmith/fleet/essentials.md#memory) | ✅ | ✅ | ❌ | — |
| Thread-scoped context | ✅ | ✅ | ✅ | ✅ | ✅ |
| Self-updating agents | ✅ [Agents can add tools, remove tools, and update their own instructions](https://docs.langchain.com/langsmith/fleet/essentials.md#self-updates) | ❌ | ❌ | ❌ | ❌ |
| Approval gate for memory writes | ✅ [Configurable per agent](https://docs.langchain.com/langsmith/fleet/manage-agent-settings.md) | ❌ | ❌ | ❌ | — |

### Observability and governance

**Fleet's** clearest advantage is its native connection to LangSmith. Every agent run is traced in LangSmith, making it easy to debug performance and run evaluations at scale. Other platforms offer basic logging and audit trails, but none match Fleet's depth of LLM-aware tracing, evaluations, and debugging through a dedicated observability platform.

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| Native tracing | ✅ [LangSmith traces for every run](https://docs.langchain.com/langsmith/observability.md) | ✅ | ⚠️ | ⚠️ | ⚠️ |
| Evaluations | ✅ [LangSmith evaluations](https://docs.langchain.com/langsmith/evaluation-concepts.md) | ❌ | ❌ | ❌ | ⚠️ |

### Code export and hosting

**Fleet** lets you export any agent you build to code via [Deep Agents](https://docs.langchain.com/oss/python/deepagents/overview.md), the open-source agent runtime that Fleet runs on. Exported agents are MIT-licensed and can be deployed independently of Fleet, modified in code, or integrated directly into your own applications via the [API](https://docs.langchain.com/langsmith/fleet/code.md). None of the other platforms in this comparison offer a code export path.

**Fleet** is the only platform in this comparison with a self-hosted deployment option. For teams with compliance requirements, self-hosted and BYOC (bring your own cloud) configurations let you run Fleet entirely within your own infrastructure. All other platforms are cloud-only managed services.

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| Cloud-hosted | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| Self-hosted | ✅ [beta](https://docs.langchain.com/langsmith/enable-self-hosted-fleet.md), [contact our sales team](https://www.langchain.com/contact-sales) for production readiness details | ❌ | ❌ | ❌ | ❌ |
| Custom models | ⚠️ [Enterprise only](https://docs.langchain.com/langsmith/fleet/essentials.md#custom-models) | ❌ | ❌ | ⚠️ | ⚠️ |
| Call agents from your app | ✅ [API access](https://docs.langchain.com/langsmith/fleet/code.md) | ✅ | ⚠️ | ❌ | ✅ |
| Export to code | ✅ [Export to Deep Agents](https://docs.langchain.com/langsmith/fleet/code.md) | ❌ | ❌ | ❌ | ❌ |

### Integrations and tools

A ✅ indicates the integration is available; supported actions and depth vary by platform. See [Fleet tool integrations](https://docs.langchain.com/langsmith/fleet/tools.md) for the full list of Fleet's built-in integrations and what each one can do.

| Feature | **Fleet** | **Claude Cowork** | **Amazon Quick** | **Google Workspace Studio** | **Microsoft Copilot** |
| - | - | - | - | - | - |
| Google Workspace (Gmail, Drive, Sheets, Docs) | ✅ | ✅ | ⚠️ | ✅ | ⚠️ |
| Microsoft 365 (Outlook, Teams, SharePoint, Excel) | ✅ | ✅ | ✅ | ❌ | ✅ |
| GitHub | ✅ | ✅ | ✅ | — | — |
| Slack | ✅ [Native](https://docs.langchain.com/langsmith/fleet/slack-app.md) | ✅ | ✅ | ⚠️ | ❌ |
| CRM (Salesforce, HubSpot) | ✅ | — | ✅ | ⚠️ | ✅ |
| Project management (Linear, Jira, Notion) | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| Custom tools via MCP | ✅ | ✅ | ✅ | ❌ | ✅ |
| Webhooks | ✅ [Webhooks](https://docs.langchain.com/langsmith/fleet/webhooks.md) | ❌ | ❌ | ⚠️ | ✅ |

For pricing and SLA information, [contact our sales team](https://www.langchain.com/contact-sales).

> [!NOTE]
>
> Last updated May 5, 2026. These products evolve quickly. If something has changed, please [file an issue](https://github.com/langchain-ai/docs/issues) to help us keep this page current.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/fleet/comparison.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
