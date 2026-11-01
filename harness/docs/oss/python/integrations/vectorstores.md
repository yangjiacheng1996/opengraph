> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Vector store integrations

> Integrate with vector stores using LangChain Python.

## Overview

A vector stores [embedded](https://docs.langchain.com/oss/python/integrations/embeddings.md) data and performs similarity search.

```mermaid theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
flowchart LR

    subgraph "📥 Indexing phase (store)"
        A[📄 Documents] --> B[🔢 Embedding model]
        B --> C[🔘 Embedding vectors]
        C --> D[(Vector store)]
    end

    subgraph "📤 Query phase (retrieval)"
        E[❓ Query text] --> F[🔢 Embedding model]
        F --> G[🔘 Query vector]
        G --> H[🔍 Similarity search]
        H --> D
        D --> I[📄 Top-k results]
    end

    classDef process fill:#E5F4FF,stroke:#006DDD,stroke-width:2px,color:#030710
    class A,B,C,D,E,F,G,H,I process
```

Integration components
Arrows show a typical retrieval flow. The highlighted component is the one this page covers.

### Typical retrieval flow

### Agent capabilities

### Execution and state

### Interface

LangChain provides a unified interface for vector stores, allowing you to:

* `add_documents` - Add documents to the store.
* `delete` - Remove stored documents by ID.
* `similarity_search` - Query for semantically similar documents.

This abstraction lets you switch between different implementations without altering your application logic.

### Initialization

To initialize a vector store, provide it with an embedding model:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain_core.vectorstores import InMemoryVectorStore
vector_store = InMemoryVectorStore(embedding=SomeEmbeddingModel())
```

### Adding documents

Add [`Document`](https://reference.langchain.com/python/langchain-core/documents/base/Document) objects (holding `page_content` and optional metadata) like so:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
vector_store.add_documents(documents=[doc1, doc2], ids=["id1", "id2"])
```

### Deleting documents

Delete by specifying IDs:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
vector_store.delete(ids=["id1"])
```

### Similarity search

Issue a semantic query using `similarity_search`, which returns the closest embedded documents:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
similar_docs = vector_store.similarity_search("your query here")
```

Many vector stores support parameters like:

* `k` — number of results to return
* `filter` — conditional filtering based on metadata

### Similarity metrics & indexing

Embedding similarity may be computed using:

* **Cosine similarity**
* **Euclidean distance**
* **Dot product**

Efficient search often employs indexing methods such as HNSW (Hierarchical Navigable Small World), though specifics depend on the vector store.

### Metadata filtering

Filtering by metadata (e.g., source, date) can refine search results:

```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
vector_store.similarity_search(
  "query",
  k=3,
  filter={"source": "tweets"}
)
```

<important>
  Support for metadata-based filtering varies between implementations.
  Check the documentation of your chosen vector store for details.
</important>

## Top integrations

**Select embedding model:**

> [accordion] OpenAI
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-openai
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-openai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("OPENAI_API_KEY"):
>   os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")
>
> from langchain_openai import OpenAIEmbeddings
>
> embeddings = OpenAIEmbeddings(model="text-embedding-3-large")
> ```

> [accordion] Azure
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-azure-ai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("AZURE_OPENAI_API_KEY"):
>   os.environ["AZURE_OPENAI_API_KEY"] = getpass.getpass("Enter API key for Azure: ")
>
> from langchain_openai import AzureOpenAIEmbeddings
>
> embeddings = AzureOpenAIEmbeddings(
>     azure_endpoint=os.environ["AZURE_OPENAI_ENDPOINT"],
>     azure_deployment=os.environ["AZURE_OPENAI_DEPLOYMENT_NAME"],
>     openai_api_version=os.environ["AZURE_OPENAI_API_VERSION"],
> )
> ```

> [accordion] Google Gemini
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-google-genai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("GOOGLE_API_KEY"):
>   os.environ["GOOGLE_API_KEY"] = getpass.getpass("Enter API key for Google Gemini: ")
>
> from langchain_google_genai import GoogleGenerativeAIEmbeddings
>
> embeddings = GoogleGenerativeAIEmbeddings(model="models/gemini-embedding-001")
> ```

> [accordion] Gemini Enterprise Agent Platform
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-google-vertexai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_vertexai import VertexAIEmbeddings
>
> embeddings = VertexAIEmbeddings(model="text-embedding-005")
> ```

> [accordion] AWS
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-aws
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_aws import BedrockEmbeddings
>
> embeddings = BedrockEmbeddings(model_id="amazon.titan-embed-text-v2:0")
> ```

> [accordion] HuggingFace
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-huggingface
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_huggingface import HuggingFaceEmbeddings
>
> embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-mpnet-base-v2")
> ```

> [accordion] Ollama
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-ollama
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_ollama import OllamaEmbeddings
>
> embeddings = OllamaEmbeddings(model="nomic-embed-text")
> ```

> [accordion] Cohere
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-cohere
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("COHERE_API_KEY"):
>   os.environ["COHERE_API_KEY"] = getpass.getpass("Enter API key for Cohere: ")
>
> from langchain_cohere import CohereEmbeddings
>
> embeddings = CohereEmbeddings(model="embed-english-v3.0")
> ```

> [accordion] Mistral AI
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-mistralai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("MISTRALAI_API_KEY"):
>   os.environ["MISTRALAI_API_KEY"] = getpass.getpass("Enter API key for MistralAI: ")
>
> from langchain_mistralai import MistralAIEmbeddings
>
> embeddings = MistralAIEmbeddings(model="mistral-embed")
> ```

> [accordion] Nomic
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-nomic
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("NOMIC_API_KEY"):
>   os.environ["NOMIC_API_KEY"] = getpass.getpass("Enter API key for Nomic: ")
>
> from langchain_nomic import NomicEmbeddings
>
> embeddings = NomicEmbeddings(model="nomic-embed-text-v1.5")
> ```

> [accordion] NVIDIA
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-nvidia-ai-endpoints
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("NVIDIA_API_KEY"):
>   os.environ["NVIDIA_API_KEY"] = getpass.getpass("Enter API key for NVIDIA: ")
>
> from langchain_nvidia_ai_endpoints import NVIDIAEmbeddings
>
> embeddings = NVIDIAEmbeddings(model="NV-Embed-QA")
> ```

> [accordion] Voyage AI
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-voyageai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("VOYAGE_API_KEY"):
>   os.environ["VOYAGE_API_KEY"] = getpass.getpass("Enter API key for Voyage AI: ")
>
> from langchain_voyageai import VoyageAIEmbeddings
>
> embeddings = VoyageAIEmbeddings(model="voyage-3")
> ```
>
> For more information, see the [Voyage AI documentation](https://www.mongodb.com/docs/voyageai/models/text-embeddings/).

> [accordion] IBM watsonx
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-ibm
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("WATSONX_APIKEY"):
>   os.environ["WATSONX_APIKEY"] = getpass.getpass("Enter API key for IBM watsonx: ")
>
> from langchain_ibm import WatsonxEmbeddings
>
> embeddings = WatsonxEmbeddings(
>     model_id="ibm/slate-125m-english-rtrvr",
>     url="https://us-south.ml.cloud.ibm.com",
>     project_id="<WATSONX PROJECT_ID>",
> )
> ```

> [accordion] Fake
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-core
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_core.embeddings import DeterministicFakeEmbedding
>
> embeddings = DeterministicFakeEmbedding(size=4096)
> ```

> [accordion] xAI
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-xai
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("XAI_API_KEY"):
>   os.environ["XAI_API_KEY"] = getpass.getpass("Enter API key for xAI: ")
>
> from langchain.chat_models import init_chat_model
>
> model = init_chat_model("grok-2", model_provider="xai")
> ```

> [accordion] Perplexity
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-perplexity
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("PPLX_API_KEY"):
>   os.environ["PPLX_API_KEY"] = getpass.getpass("Enter API key for Perplexity: ")
>
> from langchain.chat_models import init_chat_model
>
> model = init_chat_model("llama-3.1-sonar-small-128k-online", model_provider="perplexity")
> ```

> [accordion] DeepSeek
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-deepseek
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import getpass
> import os
>
> if not os.environ.get("DEEPSEEK_API_KEY"):
>   os.environ["DEEPSEEK_API_KEY"] = getpass.getpass("Enter API key for DeepSeek: ")
>
> from langchain.chat_models import init_chat_model
>
> model = init_chat_model("deepseek-chat", model_provider="deepseek")
> ```

**Select vector store:**

> [accordion] In-memory
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-core
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-core
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_core.vectorstores import InMemoryVectorStore
>
> vector_store = InMemoryVectorStore(embeddings)
> ```

> [accordion] Amazon OpenSearch
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU boto3
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from opensearchpy import RequestsHttpConnection
>
> service = "es"  # must set the service as 'es'
> region = "us-east-2"
> credentials = boto3.Session(
>     aws_access_key_id="xxxxxx", aws_secret_access_key="xxxxx"
> ).get_credentials()
> awsauth = AWS4Auth("xxxxx", "xxxxxx", region, service, session_token=credentials.token)
>
> vector_store = OpenSearchVectorSearch.from_documents(
>     docs,
>     embeddings,
>     opensearch_url="host url",
>     http_auth=awsauth,
>     timeout=300,
>     use_ssl=True,
>     verify_certs=True,
>     connection_class=RequestsHttpConnection,
>     index_name="test-index",
> )
> ```

> [accordion] Astra DB
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-astradb
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-astradb
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_astradb import AstraDBVectorStore
>
> vector_store = AstraDBVectorStore(
>     embedding=embeddings,
>     api_endpoint=ASTRA_DB_API_ENDPOINT,
>     collection_name="astra_vector_langchain",
>     token=ASTRA_DB_APPLICATION_TOKEN,
>     namespace=ASTRA_DB_NAMESPACE,
> )
> ```

> [accordion] Azure Cosmos DB NoSQL
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-azure-cosmosdb azure-cosmos
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-azure-cosmosdb azure-cosmos
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_azure_cosmosdb import AzureCosmosDBNoSqlVectorSearch
>
> vector_search = AzureCosmosDBNoSqlVectorSearch.from_documents(
>     documents=docs,
>     embedding=openai_embeddings,
>     cosmos_client=cosmos_client,
>     database_name=database_name,
>     container_name=container_name,
>     vector_embedding_policy=vector_embedding_policy,
>     full_text_policy=full_text_policy,
>     indexing_policy=indexing_policy,
>     cosmos_container_properties=cosmos_container_properties,
>     cosmos_database_properties={},
>     full_text_search_enabled=True,
> )
> ```

> [accordion] Azure Cosmos DB Mongo vCore
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-azure-ai pymongo
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add pymongo
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_azure_ai.vectorstores.azure_cosmos_db_mongo_vcore import (
>     AzureCosmosDBMongoVCoreVectorSearch,
> )
>
> vectorstore = AzureCosmosDBMongoVCoreVectorSearch.from_documents(
>     docs,
>     openai_embeddings,
>     collection=collection,
>     index_name=INDEX_NAME,
> )
> ```

> [accordion] Chroma
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-chroma
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-chroma
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_chroma import Chroma
>
> vector_store = Chroma(
>     collection_name="example_collection",
>     embedding_function=embeddings,
>     persist_directory="./chroma_langchain_db",  # Where to save data locally, remove if not necessary
> )
> ```

> [accordion] CockroachDB
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-cockroachdb
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-cockroachdb
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_cockroachdb import AsyncCockroachDBVectorStore, CockroachDBEngine
>
> CONNECTION_STRING = "cockroachdb://user:pass@host:26257/db?sslmode=verify-full"
>
> engine = CockroachDBEngine.from_connection_string(CONNECTION_STRING)
> await engine.ainit_vectorstore_table(
>     table_name="vectors",
>     vector_dimension=1536,
> )
>
> vector_store = AsyncCockroachDBVectorStore(
>     engine=engine,
>     embeddings=embeddings,
>     collection_name="vectors",
> )
> ```

> [accordion] Elasticsearch
>
> Install the package and start Elasticsearch locally using the [start-local](https://github.com/elastic/start-local) script:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-elasticsearch
> curl -fsSL https://elastic.co/start-local | sh
> ```
>
> This creates an `elastic-start-local` folder. To start Elasticsearch:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> cd elastic-start-local
> ./start.sh
> ```
>
> Elasticsearch will be available at `http://localhost:9200`. The password for the `elastic` user and API key are stored in the `.env` file in the `elastic-start-local` folder.
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_elasticsearch import ElasticsearchStore
>
> vector_store = ElasticsearchStore(
>     index_name="langchain-demo",
>     embedding=embeddings,
>     es_url="http://localhost:9200",
> )
> ```

> [accordion] Google AlloyDB
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-google-alloydb-pg
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-google-alloydb-pg
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_google_alloydb_pg import AlloyDBEngine, AlloyDBVectorStore
>
> engine = AlloyDBEngine.from_instance(
>     project_id="my-project",
>     region="us-central1",
>     cluster="my-cluster",
>     instance="my-instance",
>     database="my-database",
> )
>
> vector_store = AlloyDBVectorStore.create_sync(
>     engine=engine,
>     table_name="my_vectors",
>     embedding_service=embeddings
> )
> ```

> [accordion] Milvus
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-milvus
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-milvus
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_milvus import Milvus
>
> URI = "./milvus_example.db"
>
> vector_store = Milvus(
>     embedding_function=embeddings,
>     connection_args={"uri": URI},
>     index_params={"index_type": "FLAT", "metric_type": "L2"},
> )
> ```

> [accordion] MongoDB
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-mongodb
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_mongodb import MongoDBAtlasVectorSearch
>
> vector_store = MongoDBAtlasVectorSearch(
>     embedding=embeddings,
>     collection=MONGODB_COLLECTION,
>     index_name=ATLAS_VECTOR_SEARCH_INDEX_NAME,
>     relevance_score_fn="cosine",
> )
> ```
>
> For more information, see the [MongoDB LangChain integration docs](https://www.mongodb.com/docs/atlas/ai-integrations/langchain/#vector-store).

> [accordion] PGVector
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-postgres
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-postgres
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_postgres import PGVector
>
> vector_store = PGVector(
>     embeddings=embeddings,
>     collection_name="my_docs",
>     connection="postgresql+psycopg://..."
> )
> ```

> [accordion] PGVectorStore
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-postgres
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-postgres
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_postgres import PGEngine, PGVectorStore
>
> pg_engine = PGEngine.from_connection_string(
>     url="postgresql+psycopg://..."
> )
>
> vector_store = PGVectorStore.create_sync(
>     engine=pg_engine,
>     table_name='test_table',
>     embedding_service=embedding
> )
> ```

> [accordion] Pinecone
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-pinecone
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-pinecone
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_pinecone import PineconeVectorStore
> from pinecone import Pinecone
>
> pc = Pinecone(api_key=...)
> index = pc.Index(index_name)
>
> vector_store = PineconeVectorStore(embedding=embeddings, index=index)
> ```

> [accordion] Qdrant
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-qdrant
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-qdrant
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from qdrant_client.models import Distance, VectorParams
> from langchain_qdrant import QdrantVectorStore
> from qdrant_client import QdrantClient
>
> client = QdrantClient(":memory:")
>
> vector_size = len(embeddings.embed_query("sample text"))
>
> if not client.collection_exists("test"):
>     client.create_collection(
>         collection_name="test",
>         vectors_config=VectorParams(size=vector_size, distance=Distance.COSINE)
>     )
> vector_store = QdrantVectorStore(
>     client=client,
>     collection_name="test",
>     embedding=embeddings,
> )
> ```

> [accordion] Redis
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-redis
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-redis
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import os
> from langchain_redis import RedisConfig, RedisVectorStore
>
> config = RedisConfig(
>     index_name="my_vectors",
>     redis_url=os.getenv("REDIS_URL", "redis://localhost:6379"),
>     distance_metric="COSINE"
> )
>
> vector_store = RedisVectorStore(embeddings=embeddings, config=config)
> ```

> [accordion] Oracle AI Database
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-oracledb
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-oracledb
> ```
>
> > [!WARNING]
> >
> > The `langchain-community` package is no longer maintained. Examples that import from `langchain_community` may be outdated or broken. Use with caution.
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import oracledb
> from langchain_oracledb.vectorstores import OracleVS
> from langchain_oracledb.vectorstores.oraclevs import create_index
> from langchain_community.vectorstores.utils import DistanceStrategy
>
> username = "<username>"
> password = "<password>"
> dsn = "<hostname>:<port>/<service_name>"
>
> connection = oracledb.connect(user=username, password=password, dsn=dsn)
>
> vector_store = OracleVS(
>     client=connection,
>     embedding_function=embedding_model,
>     table_name="VECTOR_SEARCH_DEMO",
>     distance_strategy=DistanceStrategy.EUCLIDEAN_DISTANCE
> )
> ```

> [accordion] turbopuffer
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-turbopuffer
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-turbopuffer
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_turbopuffer import TurbopufferVectorStore
> from turbopuffer import Turbopuffer
>
> tpuf = Turbopuffer(region="gcp-us-central1")
> ns = tpuf.namespace("langchain-test")
>
> vector_store = TurbopufferVectorStore(embedding=embeddings, namespace=ns)
> ```

> [accordion] Valkey
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU "langchain-aws[valkey]"
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-aws --extra valkey
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> from langchain_aws.vectorstores import ValkeyVectorStore
>
> vector_store = ValkeyVectorStore(
>     embedding=embeddings,
>     valkey_url="valkey://localhost:6379",
>     index_name="my_index"
> )
> ```

> [accordion] Weaviate
>
> ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> pip install -qU langchain-weaviate
> ```
>
> ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> uv add langchain-weaviate
> ```
>
> ```python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> import weaviate
> from langchain_weaviate import WeaviateVectorStore
>
> # Assumes a local Weaviate instance on http://localhost:8080 with gRPC on 50051.
> # See the Weaviate guide for other deployment options (Weaviate Cloud, Docker, etc.).
> weaviate_client = weaviate.connect_to_local()
>
> vector_store = WeaviateVectorStore(
>     client=weaviate_client,
>     index_name="langchain_example",
>     text_key="text",
>     embedding=embeddings,
> )
> ```

| Vectorstore | Delete by ID | Filtering | Search by Vector | Search with score | Async | Passes Standard Tests | Multi Tenancy | IDs in add Documents | Downloads |
| :- | :- | :- | :- | :- | :- | :- | :- | :- | :- |
| [`ValkeyVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/valkey) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-aws/month)](https://pypi.org/project/langchain-aws/) |
| [`MongoDBAtlasVectorSearch`](https://docs.langchain.com/oss/python/integrations/vectorstores/mongodb_atlas) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-mongodb/month)](https://pypi.org/project/langchain-mongodb/) |
| [`QdrantVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/qdrant) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-qdrant/month)](https://pypi.org/project/langchain-qdrant/) |
| [`AzureCosmosDBMongoVCoreVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/azure_cosmos_db_mongo_vcore) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-azure-ai/month)](https://pypi.org/project/langchain-azure-ai/) |
| [`DatabricksVectorSearch`](https://docs.langchain.com/oss/python/integrations/vectorstores/databricks_vector_search) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/databricks-langchain/month)](https://pypi.org/project/databricks-langchain/) |
| [`PineconeVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/pinecone) | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-pinecone/month)](https://pypi.org/project/langchain-pinecone/) |
| [`Milvus`](https://docs.langchain.com/oss/python/integrations/vectorstores/milvus) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-milvus/month)](https://pypi.org/project/langchain-milvus/) |
| [`Weaviate`](https://docs.langchain.com/oss/python/integrations/vectorstores/weaviate) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-weaviate/month)](https://pypi.org/project/langchain-weaviate/) |
| [`Oracle AI Database`](https://docs.langchain.com/oss/python/integrations/vectorstores/oracle) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-oracledb/month)](https://pypi.org/project/langchain-oracledb/) |
| [`AstraDBVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/astradb) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-astradb/month)](https://pypi.org/project/langchain-astradb/) |
| [`ElasticsearchStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/elasticsearch) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-elasticsearch/month)](https://pypi.org/project/langchain-elasticsearch/) |
| [`RedisVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/redis) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-redis/month)](https://pypi.org/project/langchain-redis/) |
| [`Google AlloyDB`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_alloydb) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-alloydb-pg/month)](https://pypi.org/project/langchain-google-alloydb-pg/) |
| [`AzureCosmosDBNoSqlVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/azure_cosmos_db_no_sql) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-azure-cosmosdb/month)](https://pypi.org/project/langchain-azure-cosmosdb/) |
| [`InMemoryVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/in_memory) | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ | N/A |

## All vector stores

| Vectorstore | Delete by ID | Filtering | Search by Vector | Search with score | Async | Passes Standard Tests | Multi Tenancy | IDs in add Documents | Downloads |
| :- | :- | :- | :- | :- | :- | :- | :- | :- | :- |
| [`Feature Store on Gemini Enterprise Agent Platform`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_vertex_ai_feature_store) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-vertexai/month)](https://pypi.org/project/langchain-google-vertexai/) |
| [`Google bigquery vector search`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_bigquery_vector_search) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-vertexai/month)](https://pypi.org/project/langchain-google-vertexai/) |
| [`Vector Search on Gemini Enterprise Agent Platform`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_vertex_ai_vector_search) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-vertexai/month)](https://pypi.org/project/langchain-google-vertexai/) |
| [`Amazon MemoryDB`](https://docs.langchain.com/oss/python/integrations/vectorstores/memorydb) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-aws/month)](https://pypi.org/project/langchain-aws/) |
| [`ValkeyVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/valkey) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-aws/month)](https://pypi.org/project/langchain-aws/) |
| [`PGVector`](https://docs.langchain.com/oss/python/integrations/vectorstores/pgvector) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-postgres/month)](https://pypi.org/project/langchain-postgres/) |
| [`PGVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/pgvectorstore) | ✅ | ✅ | ✅ | ✅ | ✅ | | | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-postgres/month)](https://pypi.org/project/langchain-postgres/) |
| [`MongoDBAtlasVectorSearch`](https://docs.langchain.com/oss/python/integrations/vectorstores/mongodb_atlas) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-mongodb/month)](https://pypi.org/project/langchain-mongodb/) |
| [`Chroma`](https://docs.langchain.com/oss/python/integrations/vectorstores/chroma) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-chroma/month)](https://pypi.org/project/langchain-chroma/) |
| [`QdrantVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/qdrant) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-qdrant/month)](https://pypi.org/project/langchain-qdrant/) |
| [`AzureCosmosDBMongoVCoreVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/azure_cosmos_db_mongo_vcore) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-azure-ai/month)](https://pypi.org/project/langchain-azure-ai/) |
| [`DatabricksVectorSearch`](https://docs.langchain.com/oss/python/integrations/vectorstores/databricks_vector_search) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/databricks-langchain/month)](https://pypi.org/project/databricks-langchain/) |
| [`Pinecone (Sparse)`](https://docs.langchain.com/oss/python/integrations/vectorstores/pinecone_sparse) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-pinecone/month)](https://pypi.org/project/langchain-pinecone/) |
| [`PineconeVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/pinecone) | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-pinecone/month)](https://pypi.org/project/langchain-pinecone/) |
| [`Milvus`](https://docs.langchain.com/oss/python/integrations/vectorstores/milvus) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-milvus/month)](https://pypi.org/project/langchain-milvus/) |
| [`Weaviate`](https://docs.langchain.com/oss/python/integrations/vectorstores/weaviate) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-weaviate/month)](https://pypi.org/project/langchain-weaviate/) |
| [`Oracle AI Database`](https://docs.langchain.com/oss/python/integrations/vectorstores/oracle) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-oracledb/month)](https://pypi.org/project/langchain-oracledb/) |
| [`AstraDBVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/astradb) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-astradb/month)](https://pypi.org/project/langchain-astradb/) |
| [`Neo4j vector index`](https://docs.langchain.com/oss/python/integrations/vectorstores/neo4jvector) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-neo4j/month)](https://pypi.org/project/langchain-neo4j/) |
| [`ElasticsearchStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/elasticsearch) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-elasticsearch/month)](https://pypi.org/project/langchain-elasticsearch/) |
| [`RedisVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/redis) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-redis/month)](https://pypi.org/project/langchain-redis/) |
| [`Google AlloyDB`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_alloydb) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-alloydb-pg/month)](https://pypi.org/project/langchain-google-alloydb-pg/) |
| [`AzureCosmosDBNoSqlVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/azure_cosmos_db_no_sql) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-azure-cosmosdb/month)](https://pypi.org/project/langchain-azure-cosmosdb/) |
| [`Google cloud SQL for postgresql`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_cloud_sql_pg) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-cloud-sql-pg/month)](https://pypi.org/project/langchain-google-cloud-sql-pg/) |
| [`Sap hana cloud vector engine`](https://docs.langchain.com/oss/python/integrations/vectorstores/sap_hanavector) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-hana/month)](https://pypi.org/project/langchain-hana/) |
| [`Google firestore`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_firestore) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-firestore/month)](https://pypi.org/project/langchain-google-firestore/) |
| [`SemanticaVectorStore`](https://docs.getsemantica.ai/integrations/langchain) | ❌ | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/semantica/month)](https://pypi.org/project/semantica/) |
| [`Google spanner`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_spanner) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-spanner/month)](https://pypi.org/project/langchain-google-spanner/) |
| [`AsyncCockroachDBVectorStore`](https://github.com/cockroachdb/langchain-cockroachdb/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-cockroachdb/month)](https://pypi.org/project/langchain-cockroachdb/) |
| [`OceanbaseVectorStore`](https://pypi.org/project/langchain-oceanbase/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-oceanbase/month)](https://pypi.org/project/langchain-oceanbase/) |
| [`IBM db2 vector store and vector search`](https://github.com/langchain-ai/langchain-ibm/tree/main/libs/langchain-db2) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-db2/month)](https://pypi.org/project/langchain-db2/) |
| [`InfinoVectorStore`](https://infino.ai/docs) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-infino/month)](https://pypi.org/project/langchain-infino/) |
| [`OpensolrVectorStore`](https://opensolr.com/langchain) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-opensolr/month)](https://pypi.org/project/langchain-opensolr/) |
| [`TeradataVectorStore`](https://github.com/Teradata/langchain-teradata) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-teradata/month)](https://pypi.org/project/langchain-teradata/) |
| [`CyborgVectorStore`](https://docs.cyborg.co/versions/latest/integrations/langchain/introduction) | ✅ | ✅ | ✅ | ✅ | ✅ | | ✅ | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-cyborgdb/month)](https://pypi.org/project/langchain-cyborgdb/) |
| [`SingleStoreVectorStore`](https://docs.singlestore.com/cloud/developer-resources/functional-extensions/working-with-vector-data/) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-singlestore/month)](https://pypi.org/project/langchain-singlestore/) |
| [`YDB`](https://ydb.tech/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-ydb/month)](https://pypi.org/project/langchain-ydb/) |
| [`CouchbaseSearchVectorStore`](https://docs.couchbase.com/server/current/vector-search/vector-search.html) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-couchbase/month)](https://pypi.org/project/langchain-couchbase/) |
| [`Azure database for postgresql - flexible server`](https://docs.langchain.com/oss/python/integrations/vectorstores/azure_db_for_postgresql) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-azure-postgresql/month)](https://pypi.org/project/langchain-azure-postgresql/) |
| [`Intel's visual data management system (VDMS)`](https://github.com/IntelLabs/vdms) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-vdms/month)](https://pypi.org/project/langchain-vdms/) |
| [`SQLServer`](https://learn.microsoft.com/en-us/azure/azure-sql/database/ai-artificial-intelligence-intelligent-applications?view=azuresql) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-sqlserver/month)](https://pypi.org/project/langchain-sqlserver/) |
| [`FalkorDBVector`](https://docs.falkordb.com/genai-tools/langchain.html) | | ✅ | ✅ | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-falkordb/month)](https://pypi.org/project/langchain-falkordb/) |
| [`VastDBVectorStore`](https://github.com/vast-data/vast-vector-store) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-vastdb/month)](https://pypi.org/project/langchain-vastdb/) |
| [`Mariadb`](https://mariadb.com/docs/connectors/other/langchain-mariadb/api-reference) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-mariadb/month)](https://pypi.org/project/langchain-mariadb/) |
| [`AMUVectorStore`](https://github.com/sangaraju1988/amu-pgvector#readme) | ✅ | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-amu/month)](https://pypi.org/project/langchain-amu/) |
| [`MixpeekVectorStore`](https://mixpeek.com/docs/agent-integrations/langchain) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-mixpeek/month)](https://pypi.org/project/langchain-mixpeek/) |
| [`SurrealDBVectorStore`](https://surrealdb.com/docs/build/deployment/surrealdb-cloud/getting-started) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-surrealdb/month)](https://pypi.org/project/langchain-surrealdb/) |
| [`PolarDBXVectorStore`](https://github.com/polardb/langchain-polardbx) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-polardbx/month)](https://pypi.org/project/langchain-polardbx/) |
| [`InfinispanVectorStore`](https://github.com/infinispan/langchain-infinispan/blob/main/libs/infinispan/README.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-infinispan/month)](https://pypi.org/project/langchain-infinispan/) |
| [`BigtableVectorStore`](https://cloud.google.com/bigtable) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-bigtable/month)](https://pypi.org/project/langchain-google-bigtable/) |
| [`ShannonBaseVectorStore`](https://github.com/apoorva-01/langchain-shannonbase) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-shannonbase/month)](https://pypi.org/project/langchain-shannonbase/) |
| [`openGauss`](https://github.com/mpb159753/langchain-opengauss) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-opengauss/month)](https://pypi.org/project/langchain-opengauss/) |
| [`Google memorystore for Redis`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_memorystore_redis) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-memorystore-redis/month)](https://pypi.org/project/langchain-google-memorystore-redis/) |
| [`ChronoVecVectorStore`](https://mchl-labs.github.io/chronovec/integrations/langchain) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/chronovec/month)](https://pypi.org/project/chronovec/) |
| [`turbopuffer`](https://docs.langchain.com/oss/python/integrations/vectorstores/turbopuffer) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-turbopuffer/month)](https://pypi.org/project/langchain-turbopuffer/) |
| [`TypesenseVectorStore`](https://github.com/typesense/langchain-typesense#readme) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-typesense/month)](https://pypi.org/project/langchain-typesense/) |
| [`LambdaDB`](https://docs.lambdadb.ai/guides/get-started/quickstart) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-lambdadb/month)](https://pypi.org/project/langchain-lambdadb/) |
| [`ZeusDB`](https://docs.zeusdb.com/en/latest/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-zeusdb/month)](https://pypi.org/project/langchain-zeusdb/) |
| [`PixeltableVectorStore`](https://docs.pixeltable.com/overview/pixeltable) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-pixeltable/month)](https://pypi.org/project/langchain-pixeltable/) |
| [`Gel`](https://github.com/geldata/langchain-gel) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-gel/month)](https://pypi.org/project/langchain-gel/) |
| [`VectorPandaStore`](https://www.vectorpanda.com/docs/tutorials/langchain) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-vectorpanda/month)](https://pypi.org/project/langchain-vectorpanda/) |
| [`Google cloud SQL for mysql`](https://docs.langchain.com/oss/python/integrations/vectorstores/google_cloud_sql_mysql) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-google-cloud-sql-mysql/month)](https://pypi.org/project/langchain-google-cloud-sql-mysql/) |
| [`Kinetica vectorstore`](https://github.com/kineticadb/langchain-kinetica) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-kinetica/month)](https://pypi.org/project/langchain-kinetica/) |
| [`Moorcheh`](https://github.com/moorcheh-ai/langchain-moorcheh) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-moorcheh/month)](https://pypi.org/project/langchain-moorcheh/) |
| [`PolarDBPGVector`](https://github.com/polardb/langchain-polardb-pg) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-polardb-pg/month)](https://pypi.org/project/langchain-polardb-pg/) |
| [`Vectara`](https://docs.vectara.com/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-vectara/month)](https://pypi.org/project/langchain-vectara/) |
| [`HouyiVectorStore`](https://github.com/houyidb-community/houyidb/blob/main/sdk/python/langchain-houyidb/README.en.md) | ✅ | ✅ | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-houyidb/month)](https://pypi.org/project/langchain-houyidb/) |
| [`Actian VectorAI DB`](https://docs.vectoraidb.actian.com) | ✅ | ✅ | ✅ | ✅ | ✅ | | | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-actian-vectorai/month)](https://pypi.org/project/langchain-actian-vectorai/) |
| [`PolignVectorStore`](https://github.com/Polign/polign/tree/main/python/langchain-polign) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-polign/month)](https://pypi.org/project/langchain-polign/) |
| [`Firebolt`](https://docs.firebolt.io/guides/integrations/langchain) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-firebolt/month)](https://pypi.org/project/langchain-firebolt/) |
| [`SereneDB`](https://serenedb.com/docs/clients/langchain-serenedb) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-serenedb/month)](https://pypi.org/project/langchain-serenedb/) |
| [`DeweyVectorStore`](https://github.com/meetdewey/langchain-dewey) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-dewey/month)](https://pypi.org/project/langchain-dewey/) |
| [`Activeloop Deep lake`](https://docs.deeplake.ai/) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-deeplake/month)](https://pypi.org/project/langchain-deeplake/) |
| [`Vedb for mysql`](https://docs.volcengine.com/docs/6357?lang=en) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-volcengine-mysql/month)](https://pypi.org/project/langchain-volcengine-mysql/) |
| [`Volcengine rds for mysql`](https://docs.volcengine.com/docs/6313?lang=en) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-volcengine-mysql/month)](https://pypi.org/project/langchain-volcengine-mysql/) |
| [`RostamVectorStore`](https://github.com/rostamlabs/rostam/tree/main/clients/langchain-rostam) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-rostam/month)](https://pypi.org/project/langchain-rostam/) |
| [`Alibaba cloud mysql`](https://github.com/wangkuahai/langchain-alibabacloud-mysql) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-alibabacloud-mysql/month)](https://pypi.org/project/langchain-alibabacloud-mysql/) |
| [`LindormVectorStore`](https://help.aliyun.com/en/lindorm/user-guide/enable-vector-engine) | | | | | | | | | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-lindorm-integration/month)](https://pypi.org/project/langchain-lindorm-integration/) |
| [`ChDBVectorStore`](https://github.com/chdb-io/langchain-chdb) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-chdb/month)](https://pypi.org/project/langchain-chdb/) |
| [`YantrikDBVectorStore`](https://github.com/yantrikos/langchain-yantrikdb) | ✅ | | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | [  ![Downloads per month](https://static.pepy.tech/badge/langchain-yantrikdb/month)](https://pypi.org/project/langchain-yantrikdb/) |
| [`FAISS`](https://github.com/facebookresearch/faiss) | | | | | | | | | N/A |
| [`InMemoryVectorStore`](https://docs.langchain.com/oss/python/integrations/vectorstores/in_memory) | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ | N/A |

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/python/integrations/vectorstores/index.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
