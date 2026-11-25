# ADR 004: Embeddings and vector store

**Status:** Accepted (direction), to be confirmed by eval in Weeks 6-7 | **Date:** 2026-11-24

## Context
The corpus is small (about 270 KB of markdown, a few hundred chunks) but should be able to grow. Budget is $30-50/month in total. The Anthropic API does not provide embeddings. Spanish arrives later.

## Decision
- **Embeddings:** a hosted multilingual embedding API. Store the model name and version in the manifest. Compare 2-3 candidates on the eval set.
- **Vector store:** Redis. Free tier for development, the 250 MB Essentials plan for launch. A numpy brute-force baseline sits behind the same `Retriever` interface, and the winner is chosen from eval numbers.
- Free-tier limits checked on 2026-11-24 (verify again before launch): 30 MB, 100 ops/sec, 30 concurrent connections, 5 GB monthly bandwidth, no replication or failover. Designed for learning and test projects.
- Ruled out for hosting: Databricks vector search (paid endpoints start around $200/month if left running; Free Edition is non-commercial) and OpenSearch Serverless (well over budget).
- MongoDB Atlas is a possible second backend after launch; verify vector search limits on its free tier first.

## Consequences
- Redis can later also serve rate-limit counters, the token budget, a semantic cache, and agent memory.
- If Redis does not beat or match the baseline on quality and latency, say so here.
- Growth is memory-bound: vectors dominate cost, so re-check the tier when the corpus grows.
