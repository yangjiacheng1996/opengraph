> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# LangSmith Evaluation

> Evaluate and test agent quality at scale with datasets, evaluators, prompts, and Studio.

LangSmith's testing tools help you measure agent quality, iterate on prompts, and debug live in an interactive environment. Evaluation is the core of testing: it scores your agent's outputs against datasets and criteria so you can benchmark versions, catch regressions, and track quality over time.

Add real traces to a dataset so a failure you saw once becomes a test you run every time.

LangSmith supports two types of evaluation based on when and where they run:

> [card] Offline Evaluation
>
> **Test before you ship**
>
> Run evaluations on curated datasets during development to compare versions, benchmark performance, and catch regressions.

> [card] Online Evaluation
>
> **Monitor in production**
>
> Evaluate real user interactions in real-time to detect issues and measure quality on live traffic.

## Set up your account

**1. Create an account**

Sign up at [smith.langchain.com](https://smith.langchain.com?utm_source=docs\&utm_medium=cta\&utm_campaign=langsmith-signup\&utm_content=snippets-langsmith-account-api-key-quickstart) (no credit card required).
You can log in with **Google**, **GitHub**, or **email**.

**2. Create an API key**

Go to your [Settings page](https://smith.langchain.com/settings) > **API Keys** > **Create API Key**.
Copy the key and save it securely.

Once your account and API key are ready, [run your first evaluation](https://docs.langchain.com/langsmith/evaluation-quickstart.md).

## Evaluation workflow

> [tab] Offline evaluation flow
>
> **1. Create a dataset**
>
> Create a [dataset](https://docs.langchain.com/langsmith/manage-datasets.md) with [examples](https://docs.langchain.com/langsmith/evaluation-concepts.md#examples) (Individual test cases with inputs and reference outputs) from manually curated test cases, historical production traces, or synthetic data generation.
>
> **2. Define evaluators**
>
> Create [evaluators](https://docs.langchain.com/langsmith/evaluation-concepts.md#evaluators) (Functions that score how well your application performs) to score performance:
>
> * [Human](https://docs.langchain.com/langsmith/evaluation-concepts.md#human) review
> * [Code](https://docs.langchain.com/langsmith/evaluation-concepts.md#code) rules
> * [LLM-as-judge](https://docs.langchain.com/langsmith/llm-as-judge.md)
> * [Pairwise](https://docs.langchain.com/langsmith/evaluate-pairwise.md) comparison
>
> **3. Run an experiment**
>
> Execute your application on the dataset to create an [experiment](https://docs.langchain.com/langsmith/evaluation-concepts.md#experiment) (Results of evaluating a specific application version on a dataset). Configure [repetitions, concurrency, and caching](https://docs.langchain.com/langsmith/experiment-configuration.md) to optimize runs.
>
> **4. Analyze results**
>
> Compare experiments for [benchmarking](https://docs.langchain.com/langsmith/evaluation-types.md#benchmarking), [unit tests](https://docs.langchain.com/langsmith/evaluation-types.md#unit-tests), [regression tests](https://docs.langchain.com/langsmith/evaluation-types.md#regression-tests), or [backtesting](https://docs.langchain.com/langsmith/evaluation-types.md#backtesting).

> [tab] Online evaluation flow
>
> **1. Deploy your application**
>
> Each interaction creates a [run](https://docs.langchain.com/langsmith/evaluation-concepts.md#runs) (A single execution trace including inputs, outputs, and intermediate steps) without reference outputs.
>
> **2. Configure online evaluators**
>
> Set up [evaluators](https://docs.langchain.com/langsmith/online-evaluations-llm-as-judge.md) to run automatically on production traces: safety checks, format validation, quality heuristics, and reference-free LLM-as-judge. Apply [filters and sampling rates](https://docs.langchain.com/langsmith/online-evaluations-llm-as-judge.md#configure-a-sampling-rate) to control costs.
>
> **3. Monitor in real-time**
>
> Evaluators run automatically on [runs](https://docs.langchain.com/langsmith/evaluation-concepts.md#runs) or [threads](https://docs.langchain.com/langsmith/online-evaluations-multi-turn.md) (Collections of related runs forming multi-turn conversations), providing real-time monitoring, anomaly detection, and alerting.
>
> **4. Establish a feedback loop**
>
> Add failing production traces to your [dataset](https://docs.langchain.com/langsmith/manage-datasets.md), create targeted evaluators, validate fixes with offline experiments, and redeploy.

> [!TIP]
>
> For more on the differences between offline and online evaluation, refer to the [Evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts.md#quick-reference-offline-vs-online-evaluation) page.

## Get started

> [card] [Evaluation quickstart](https://docs.langchain.com/langsmith/evaluation-quickstart.md)
>
> Get started with offline evaluation.

> [card] [Manage datasets](https://docs.langchain.com/langsmith/manage-datasets.md)
>
> Create and manage datasets for evaluation through the UI or SDK.

> [card] [Run offline evaluations](https://docs.langchain.com/langsmith/evaluate-llm-application.md)
>
> Explore evaluation types, techniques, and frameworks for comprehensive testing.

> [card] [Analyze results](https://docs.langchain.com/langsmith/analyze-an-experiment.md)
>
> View and analyze evaluation results, compare experiments, filter data, and export findings.

> [card] [Run online evaluations](https://docs.langchain.com/langsmith/online-evaluations-llm-as-judge.md)
>
> Monitor production quality in real-time from the Observability tab.

> [card] [Follow tutorials](https://docs.langchain.com/langsmith/evaluate-chatbot-tutorial.md)
>
> Learn by following step-by-step tutorials, from simple chatbots to complex agent evaluations.

> [card] [Studio](https://docs.langchain.com/langsmith/studio.md)
>
> Use an interactive environment for developing and debugging agents.

> [!NOTE]
>
> To set up a LangSmith instance, visit the [Platform setup section](https://docs.langchain.com/langsmith/platform-setup.md) to choose between cloud, hybrid, or self-hosted. All options include observability, evaluation, prompt engineering, and deployment.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/evaluation.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
