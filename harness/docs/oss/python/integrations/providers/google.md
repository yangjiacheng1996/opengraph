> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Google integrations

> Integrate with Google using LangChain Python.

This page covers all LangChain integrations with [Google Gemini](https://ai.google.dev/gemini-api/docs), [Google Cloud](https://cloud.google.com/), and other Google products (such as Google Maps, YouTube, and [more](#other-google-products)).

> [!NOTE]
>
> **Unified SDK & package consolidation**
>
> As of `langchain-google-genai` 4.0.0, this package uses the consolidated [`google-genai`](https://googleapis.github.io/python-genai/) SDK and now supports **both the Gemini Developer API and Gemini Enterprise Agent Platform** backends.
>
> The `langchain-google-vertexai` package remains supported for Gemini Enterprise Agent Platform-specific features (Model Garden, Vector Search, evaluation services, etc.).
>
> Read the [full announcement and migration guide](https://github.com/langchain-ai/langchain-google/discussions/1422).

Not sure which package to use?

> [accordion] Google Generative AI (Gemini API & Gemini Enterprise Agent Platform)
>
> Access Google Gemini models via the **[Gemini Developer API](https://ai.google.dev/)** or **[Gemini Enterprise Agent Platform](https://cloud.google.com/products/gemini-enterprise-agent-platform)**. The backend is selected automatically based on your configuration.
>
> * **Gemini Developer API**: Quick setup with API key, ideal for individual developers and rapid prototyping
> * **Gemini Enterprise Agent Platform**: Enterprise features with Google Cloud integration (requires GCP project)
>
> Use the `langchain-google-genai` package for chat models, LLMs, and embeddings.
>
> [See integrations.](#google-generative-ai)

> [accordion] Google Cloud (Gemini Enterprise Agent Platform Services)
>
> Access Gemini Enterprise Agent Platform-specific services beyond Gemini models: Model Garden (Llama, Mistral, Anthropic), evaluation services, and specialized vision models.
>
> Use the `langchain-google-vertexai` package for platform services and specific packages (e.g., `langchain-google-community`, `langchain-google-cloud-sql-pg`) for other cloud services like databases and storage.
>
> [See integrations.](#google-cloud)

See Google's guide on [migrating from the Gemini API to Gemini Enterprise Agent Platform](https://ai.google.dev/gemini-api/docs/migrate-to-cloud) for more details on the differences.

***

## Google Generative AI

Access Google Gemini models via the [Gemini Developer API](https://ai.google.dev/gemini-api/docs) or [Gemini Enterprise Agent Platform](https://cloud.google.com/products/gemini-enterprise-agent-platform) using the unified `langchain-google-genai` package.

### Chat models

> [card] [ChatGoogleGenerativeAI](https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai)
>
> Google Gemini chat models via **Gemini Developer API** or **Gemini Enterprise Agent Platform**.

### LLMs

> [card] [GoogleGenerativeAI](https://docs.langchain.com/oss/python/integrations/llms/google_generative_ai)
>
> Gemini models using the (legacy) LLM text completion interface.

### Embedding models

> [card] [GoogleGenerativeAIEmbeddings](https://docs.langchain.com/oss/python/integrations/embeddings/google_generative_ai)
>
> Gemini embedding models via **Gemini Developer API** or **Gemini Enterprise Agent Platform**.

***

## Google Cloud

Access Gemini Enterprise Agent Platform-specific services including Model Garden (Llama, Mistral, Anthropic), Vector Search, evaluation services, and specialized vision models.

> [!NOTE]
>
> **For Gemini models**, use [`ChatGoogleGenerativeAI`](https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai) from `langchain-google-genai`. The classes below focus on **Gemini Enterprise Agent Platform services** not available in the consolidated SDK.

### Chat models

> [card] [ChatAnthropicVertex](https://docs.langchain.com/oss/python/integrations/chat/google_anthropic_vertex)
>
> Anthropic on Gemini Enterprise Agent Platform Model Garden

> [accordion] ChatVertexAI (deprecated)
>
> **Deprecated**—Use [`ChatGoogleGenerativeAI`](https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai) for Gemini models instead.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai import ChatVertexAI
> ```

> [accordion] VertexModelGardenLlama
>
> Llama on Gemini Enterprise Agent Platform Model Garden
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.model_garden_maas.llama import VertexModelGardenLlama
> ```

> [accordion] VertexModelGardenMistral
>
> Mistral on Gemini Enterprise Agent Platform Model Garden
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.model_garden_maas.mistral import VertexModelGardenMistral
> ```

> [accordion] GemmaChatLocalHF
>
> Local Gemma model loaded from HuggingFace.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaChatLocalHF
> ```

> [accordion] GemmaChatLocalKaggle
>
> Local Gemma model loaded from Kaggle.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaChatLocalKaggle
> ```

> [accordion] GemmaChatVertexAIModelGarden
>
> Gemma on Gemini Enterprise Agent Platform Model Garden
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaChatVertexAIModelGarden
> ```

> [accordion] VertexAIImageCaptioningChat
>
> Image captioning model as a chat interface.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.vision_models import VertexAIImageCaptioningChat
> ```

> [accordion] VertexAIImageEditorChat
>
> Edit images given a prompt. Currently supports mask-free editing only.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.vision_models import VertexAIImageEditorChat
> ```

> [accordion] VertexAIImageGeneratorChat
>
> Generate images from a prompt.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.vision_models import VertexAIImageGeneratorChat
> ```

> [accordion] VertexAIVisualQnAChat
>
> Visual question answering model as a chat interface.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.vision_models import VertexAIVisualQnAChat
> ```

### LLMs

(Legacy) string-in, string-out LLM interface.

> [card] [VertexAIModelGarden](https://docs.langchain.com/oss/python/integrations/llms/google_vertex_ai#vertex-model-garden)
>
> Hundreds of OSS models via Gemini Enterprise Agent Platform Model Garden.

> [accordion] VertexAI (deprecated)
>
> **Deprecated**—Use [`GoogleGenerativeAI`](https://docs.langchain.com/oss/python/integrations/llms/google_generative_ai) for Gemini models instead.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai import VertexAI
> ```

> [accordion] Gemma local from Hugging Face
>
> Local Gemma model loaded from HuggingFace.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaLocalHF
> ```

> [accordion] Gemma local from Kaggle
>
> Local Gemma model loaded from Kaggle.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaLocalKaggle
> ```

> [accordion] Gemma on Gemini Enterprise Agent Platform Model Garden
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.gemma import GemmaVertexAIModelGarden
> ```

> [accordion] Gemini Enterprise Agent Platform image captioning
>
> Image captioning model as an LLM interface.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.vision_models import VertexAIImageCaptioning
> ```

### Embedding models

> [accordion] VertexAIEmbeddings (deprecated)
>
> **Deprecated**—Use [`GoogleGenerativeAIEmbeddings`](https://docs.langchain.com/oss/python/integrations/embeddings/google_generative_ai) instead.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai import VertexAIEmbeddings
> ```

### Document loaders

> [card] [AlloyDB for PostgreSQL](https://docs.langchain.com/oss/python/integrations/document_loaders/google_alloydb)
>
> PostgreSQL-compatible database on Google Cloud.

> [card] [BigQuery](https://docs.langchain.com/oss/python/integrations/document_loaders/google_bigquery)
>
> Serverless data warehouse.

> [card] [Bigtable](https://docs.langchain.com/oss/python/integrations/document_loaders/google_bigtable)
>
> Key-value and wide-column store for structured and semi-structured data.

> [card] [Cloud SQL for MySQL](https://docs.langchain.com/oss/python/integrations/document_loaders/google_cloud_sql_mysql)
>
> Managed MySQL database.

> [card] [Cloud SQL for SQL Server](https://docs.langchain.com/oss/python/integrations/document_loaders/google_cloud_sql_mssql)
>
> Managed SQL Server database.

> [card] [Cloud SQL for PostgreSQL](https://cloud.google.com/sql/docs/postgres)
>
> Managed PostgreSQL database.

> [card] [Cloud Storage (directory)](https://docs.langchain.com/oss/python/integrations/document_loaders/google_cloud_storage_directory)
>
> Load documents from a GCS bucket directory.

> [card] [Cloud Storage (file)](https://docs.langchain.com/oss/python/integrations/document_loaders/google_cloud_storage_file)
>
> Load a single document from GCS.

> [card] [El Carro for Oracle Workloads](https://github.com/googleapis/langchain-google-el-carro-python/)
>
> Oracle databases on Kubernetes via El Carro.

> [card] [Firestore (Native Mode)](https://docs.langchain.com/oss/python/integrations/document_loaders/google_firestore)
>
> NoSQL document database.

> [card] [Firestore (Datastore Mode)](https://docs.langchain.com/oss/python/integrations/document_loaders/google_datastore)
>
> Firestore in Datastore mode.

> [card] [Memorystore for Redis](https://docs.langchain.com/oss/python/integrations/document_loaders/google_memorystore_redis)
>
> Managed Redis service.

> [card] [Spanner](https://docs.langchain.com/oss/python/integrations/document_loaders/google_spanner)
>
> Globally distributed relational database.

> [card] [Speech-to-Text](https://docs.langchain.com/oss/python/integrations/document_loaders/google_speech_to_text)
>
> Transcribe audio files.

> [accordion] Cloud Vision loader
>
> Load data using Google Cloud Vision API.
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_community.vision import CloudVisionLoader
> ```

### Document transformers

> [card] [Document AI](https://docs.langchain.com/oss/python/integrations/document_transformers/google_docai)
>
> Extract structured data from unstructured documents.

> [card] [Google Translate](https://docs.langchain.com/oss/python/integrations/document_transformers/google_translate)
>
> Translate text and HTML via Cloud Translation API.

### Vector stores

Store and search vectors using Google Cloud databases and Vector Search on Gemini Enterprise Agent Platform.

> [card] [AlloyDB for PostgreSQL](https://docs.langchain.com/oss/python/integrations/vectorstores/google_alloydb)
>
> PostgreSQL-compatible vector store on AlloyDB.

> [card] [BigQuery Vector Search](https://docs.langchain.com/oss/python/integrations/vectorstores/google_bigquery_vector_search)
>
> Semantic search using GoogleSQL with vector indexes.

> [card] [Memorystore for Redis](https://docs.langchain.com/oss/python/integrations/vectorstores/google_memorystore_redis)
>
> Vector store on Memorystore for Redis.

> [card] [Spanner](https://docs.langchain.com/oss/python/integrations/vectorstores/google_spanner)
>
> Vector store on Cloud Spanner.

> [card] [Bigtable](https://cloud.google.com/bigtable)
>
> Vector store on Cloud Bigtable.

> [card] [Firestore (Native Mode)](https://docs.langchain.com/oss/python/integrations/vectorstores/google_firestore)
>
> Vector store on Firestore.

> [card] [Cloud SQL for MySQL](https://docs.langchain.com/oss/python/integrations/vectorstores/google_cloud_sql_mysql)
>
> Vector store on Cloud SQL for MySQL.

> [card] [Cloud SQL for PostgreSQL](https://docs.langchain.com/oss/python/integrations/vectorstores/google_cloud_sql_pg)
>
> Vector store on Cloud SQL for PostgreSQL.

> [card] [Vector Search on Gemini Enterprise Agent Platform](https://docs.langchain.com/oss/python/integrations/vectorstores/google_vertex_ai_vector_search)
>
> Formerly known as Vertex AI Matching Engine, provides a low latency vector database. These vector databases are commonly referred to as vector similarity-matching or an approximate nearest neighbor (ANN) service.

> [card] [Vector Search on Gemini Enterprise Agent Platform + Datastore](https://docs.langchain.com/oss/python/integrations/vectorstores/google_vertex_ai_vector_search#optional--you-can-also-create-vector-and-store-chunks-in-a-datastore)
>
> Vector search with Datastore for document storage.

### Retrievers

> [card] [Agent Search on Gemini Enterprise Agent Platform](https://docs.langchain.com/oss/python/integrations/retrievers/google_vertex_ai_search)
>
> Generative AI powered search via Agent Search on Gemini Enterprise Agent Platform.

> [card] [Document AI Warehouse](https://cloud.google.com/document-ai-warehouse)
>
> Search, store, and manage documents using Document AI Warehouse.

```python Other retrievers theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain_google_community import VertexAIMultiTurnSearchRetriever
from langchain_google_community import VertexAISearchRetriever
from langchain_google_community import VertexAISearchSummaryTool
```

### Tools

Integrate agents with various Google Cloud services.

> [card] [Text-to-Speech](https://docs.langchain.com/oss/python/integrations/tools/google_cloud_texttospeech)
>
> Synthesize natural-sounding speech with 100+ voices.

### Callbacks

Track LLM/Chat model usage.

> [accordion] Gemini Enterprise Agent Platform callback handler
>
> Track `VertexAI` usage info.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.callbacks import VertexAICallbackHandler
> ```

> [accordion] Google BigQuery
>
> See the [documentation](https://docs.langchain.com/oss/python/integrations/callbacks/google_bigquery) for more details.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_community.callbacks.bigquery_callback import BigQueryCallbackHandler
> ```

### Evaluators

Evaluate model outputs using Gemini Enterprise Agent Platform.

> [accordion] VertexPairWiseStringEvaluator
>
> Pair-wise evaluation using Gemini Enterprise Agent Platform models.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.evaluators.evaluation import VertexPairWiseStringEvaluator
> ```

> [accordion] VertexStringEvaluator
>
> Single prediction evaluation using Gemini Enterprise Agent Platform models.
>
> ```python wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai.evaluators.evaluation import VertexStringEvaluator
> ```

***

## Other Google products

Integrations with various Google services beyond the core Cloud Platform.

### Document loaders

> [card] [Google Drive](https://docs.langchain.com/oss/python/integrations/document_loaders/google_drive)
>
> Load files from Google Drive. Currently supports Google Docs.

### Retrievers

> [card] [Google Drive](https://docs.langchain.com/oss/python/integrations/retrievers/google_drive)
>
> Retrieve documents from Google Drive.

### Tools

> [card] [Google Search](https://docs.langchain.com/oss/python/integrations/tools/google_search)
>
> Web search via Google Custom Search Engine (CSE).

> [card] [Google Drive](https://docs.langchain.com/oss/python/integrations/tools/google_drive)
>
> Interact with Google Drive.

### MCP

> [card] [MCP Toolbox](https://docs.langchain.com/oss/python/integrations/tools/mcp_toolbox)
>
> Connect to databases including Cloud SQL and AlloyDB.

### Toolkits

> [card] [Gmail](https://docs.langchain.com/oss/python/integrations/tools/google_gmail)
>
> Create, search, and send emails via the Gmail API.

***

## 3rd party integrations

Access Google services via unofficial third-party APIs.

### Search

> [card] [cloro](https://cloro.dev/docs)
>
> Google Search results with AI Overview support.

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/python/integrations/providers/google.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
