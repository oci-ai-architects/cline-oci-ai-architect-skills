---
name: rag-expert
description: Retrieval-Augmented Generation patterns on Oracle Cloud Infrastructure — embeddings, vector stores, hybrid search, reranking, and production RAG architecture
version: 1.0.0
platform: [claude-code, cline, cursor, roocode]
activation:
  cline: "@skills/rag-expert/SKILL.md"
  cursor: "@skills/rag-expert/SKILL.md"
---

# RAG Expert for OCI

You are an expert in Retrieval-Augmented Generation patterns on Oracle Cloud Infrastructure.

## When to Use
- Building RAG systems on OCI
- Selecting embedding models and vector stores
- Optimizing retrieval quality
- Enterprise RAG architecture

## OCI RAG Architecture

```
┌─────────────────────── Security & Governance ─────────────────────┐
│                                                                    │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────────────┐     │
│  │Documents │───▶│ Processing   │───▶│ Embedding            │     │
│  │          │    │ (Doc Under-  │    │ (Cohere Embed 4)     │     │
│  └──────────┘    │  standing)   │    └───────────┬──────────┘     │
│                  └──────────────┘                │                 │
│                                        ┌────────▼────────┐        │
│                                        │ Vector Store    │        │
│                                        │ (26ai AI Vector │        │
│                                        │  Search)        │        │
│                                        └────────┬────────┘        │
│                                                 │                  │
│  ┌──────────┐    ┌──────────────┐    ┌─────────▼─────────┐       │
│  │  Query   │───▶│  Retrieval   │───▶│  Reranking         │       │
│  │          │    │  + Hybrid    │    │  (Rerank 3.5)      │       │
│  └──────────┘    └──────────────┘    └─────────┬─────────┘       │
│                                                 │                  │
│                  ┌──────────────┐    ┌─────────▼─────────┐       │
│                  │   Response   │◀───│  Generation        │       │
│                  │              │    │  (Command A)       │       │
│                  └──────────────┘    └───────────────────┘       │
│                                                                    │
└──────────────────── Observability & Evaluation ────────────────────┘
```

## OCI Components for RAG

| Component | OCI Service | Alternatives |
|-----------|-------------|--------------|
| **Embeddings** | Cohere Embed 4 (multimodal) | Embed Multilingual 3 |
| **Vector Store** | Oracle AI Database 26ai | OCI Search, OpenSearch |
| **LLM** | Cohere Command A | Llama 4 Maverick, Gemini 2.5 |
| **Document Processing** | Document Understanding | Custom parsers |
| **Reranking** | Cohere Rerank 3.5 | - |
| **Orchestration** | GenAI Agent Hub | Oracle ADK |

## Embedding Models on OCI

| Model | Dimensions | Best For |
|-------|------------|----------|
| Cohere Embed 4 | 1024 | Multimodal (text + images) |
| Cohere Embed Multilingual 3 | 1024 | 100+ languages |

## Vector Store Options

### Oracle AI Database 26ai (Recommended)
- Native AI Vector Search with Unified Hybrid (vector + keyword)
- Select AI Agent for in-database AI
- Combine with relational, JSON, graph data
- Best for: Existing Oracle customers, enterprise

### OCI Search (Managed)
- Fully managed, integrated with GenAI Agents
- Good for: Quick start, managed solution

## Retrieval Optimization

### 1. Hybrid Search (26ai)
```sql
SELECT id, title,
       (0.7 * (1 - VECTOR_DISTANCE(embedding, :qvec, COSINE))
        + 0.3 * SCORE(1)) AS hybrid_score
FROM documents
WHERE CONTAINS(content, :keyword_query, 1) > 0
ORDER BY hybrid_score DESC
FETCH FIRST 10 ROWS ONLY;
```

### 2. Reranking
Always rerank with Cohere Rerank 3.5 for production quality.

### 3. Chunking Strategy
- Fixed size (512 tokens, 50 overlap) for simple docs
- Semantic chunking for complex documents
- Hierarchical (Document > Section > Paragraph) for enterprise

## Quality Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Retrieval Recall | >90% | Ground truth comparison |
| Answer Relevance | >4.5/5 | LLM-as-judge |
| Faithfulness | >95% | Citation verification |
| Latency (P95) | <3s | End-to-end timing |

## Before Building Custom RAG

**Check OCI AI Blueprints first:**
- Llama Stack blueprint includes vLLM + ChromaDB + Postgres + Jaeger
- Repository: https://github.com/oracle-quickstart/oci-ai-blueprints

---

## Cline Activation

To use this skill in Cline, reference it at the start of your message:

```
@skills/rag-expert/SKILL.md

Design a production RAG system on OCI using Oracle AI Database 26ai for hybrid search, Cohere Embed 4 for embeddings, and Cohere Rerank 3.5. The use case is enterprise contract analysis.
```

Or in a `.clinerules` workflow:
```markdown
## RAG Architecture
When designing RAG systems on OCI, load @skills/rag-expert/SKILL.md. Use the 3-tier diagram standard, always include reranking, prefer Oracle AI Database 26ai for hybrid search, and check AI Blueprints before building custom.
```

**Triggers:** RAG, retrieval-augmented generation, vector search OCI, embeddings OCI, hybrid search, Cohere Embed, Rerank 3.5, OCI RAG architecture
