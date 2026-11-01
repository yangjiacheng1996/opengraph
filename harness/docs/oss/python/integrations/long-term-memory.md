> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Store integrations

> Integrate with store backends for LangGraph long-term memory.

Stores enable [long-term memory](https://docs.langchain.com/oss/python/langgraph/stores.md) in LangGraph, allowing agents to persist and retrieve information across threads.

To implement your own store for a custom storage backend, see [Build a custom store](https://docs.langchain.com/oss/python/langgraph/stores.md#build-a-custom-store).

Integration components
Arrows show a typical retrieval flow. The highlighted component is the one this page covers.

### Typical retrieval flow

### Agent capabilities

### Execution and state

| Backend | Package | Source |
| - | - | - |
| [In-memory](https://reference.langchain.com/python/langgraph.store/memory/InMemoryStore) | [`langgraph-checkpoint`](https://pypi.org/project/langgraph-checkpoint/) | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph/tree/main/libs/checkpoint) |
| [PostgreSQL](https://reference.langchain.com/python/langgraph.store.postgres/PostgresStore) | [`langgraph-checkpoint-postgres`](https://pypi.org/project/langgraph-checkpoint-postgres/) | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph/tree/main/libs/checkpoint-postgres) |
| Redis | [`langgraph-checkpoint-redis`](https://pypi.org/project/langgraph-checkpoint-redis/) | [redis-developer/langgraph-redis](https://github.com/redis-developer/langgraph-redis) |
| [MongoDB](https://docs.langchain.com/oss/python/integrations/memory/mongodb-long-term-memory) | [`langgraph-store-mongodb`](https://pypi.org/project/langgraph-store-mongodb/) | [langchain-ai/langchain-mongodb](https://github.com/langchain-ai/langchain-mongodb/tree/main/libs/langgraph-store-mongodb) |
| Upstash Redis | [`langgraph-store-upstash`](https://pypi.org/project/langgraph-store-upstash/) | [Tghez/langgraph-store-upstash](https://github.com/Tghez/langgraph-store-upstash) |
| OMEM | [`omem-infrastructure`](https://pypi.org/project/omem-infrastructure/) | [OMEM docs](https://infrastructure.omem-cloud.com/docs) |
| [SingleStore](https://www.singlestore.com/docs/) | [`langgraph-singlestore`](https://pypi.org/project/langgraph-singlestore/) | [singlestore-labs/langchain-singlestore](https://github.com/singlestore-labs/langchain-singlestore) |
| [Amazon DynamoDB (agentstate)](https://skamalj.github.io/agentstate-reducer/langgraph/stores/) | [`langgraph-store-dynamodb`](https://pypi.org/project/langgraph-store-dynamodb/) | [skamalj/langgraph-store](https://github.com/skamalj/langgraph-store/tree/main/providers/dynamodb) |
| [PostgreSQL / pgvector (agentstate)](https://skamalj.github.io/agentstate-reducer/langgraph/stores/) | [`langgraph-store-postgres`](https://pypi.org/project/langgraph-store-postgres/) | [skamalj/langgraph-store](https://github.com/skamalj/langgraph-store/tree/main/providers/postgres) |
| [Azure Cosmos DB NoSQL (agentstate)](https://skamalj.github.io/agentstate-reducer/langgraph/stores/) | [`langgraph-store-cosmosdb`](https://pypi.org/project/langgraph-store-cosmosdb/) | [skamalj/langgraph-store](https://github.com/skamalj/langgraph-store/tree/main/providers/cosmosdb) |
| [Google Firestore (agentstate)](https://skamalj.github.io/agentstate-reducer/langgraph/stores/) | [`langgraph-store-firestore`](https://pypi.org/project/langgraph-store-firestore/) | [skamalj/langgraph-store](https://github.com/skamalj/langgraph-store/tree/main/providers/firestore) |

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/python/integrations/long-term-memory/index.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
