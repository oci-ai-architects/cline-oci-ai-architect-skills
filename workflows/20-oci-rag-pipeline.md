# Workflow: Build RAG Pipeline on OCI

## Purpose
End-to-end Retrieval-Augmented Generation pipeline with quality metrics.

## Steps

### 1. Choose Architecture

```
BEFORE building custom, check OCI AI Blueprints:
https://github.com/oracle-quickstart/oci-ai-blueprints

Available pre-built:
- Llama Stack (vLLM + ChromaDB + Postgres + Jaeger)
- vLLM Inference
- Autoscaling Inference with KEDA
```

### 2. Document Processing

```python
# Option A: OCI Document Understanding (structured docs)
du_client = oci.ai_document.AIServiceDocumentClient(config)
response = du_client.analyze_document(
    analyze_document_details=AnalyzeDocumentDetails(
        features=[DocumentTextExtractionFeature(feature_type="TEXT_EXTRACTION")],
        document=InlineDocumentContent(data=base64_content)
    )
)

# Option B: Custom parser (unstructured)
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(
    chunk_size=512,
    chunk_overlap=50,
    separators=["\n\n", "\n", ". ", " "]
)
chunks = splitter.split_text(document_text)
```

### 3. Embedding Pipeline

```python
import oci
from concurrent.futures import ThreadPoolExecutor

def embed_batch(texts: list[str], input_type: str = "SEARCH_DOCUMENT") -> list:
    response = genai_client.embed_text(
        EmbedTextDetails(
            inputs=texts,
            serving_mode=OnDemandServingMode(model_id="cohere.embed-english-v3.0"),
            compartment_id=compartment_id,
            input_type=input_type
        )
    )
    return response.data.embeddings

# Process in batches of 96 (Cohere limit)
BATCH_SIZE = 96
all_embeddings = []
for i in range(0, len(chunks), BATCH_SIZE):
    batch = chunks[i:i + BATCH_SIZE]
    all_embeddings.extend(embed_batch(batch))
```

### 4. Vector Store (Oracle AI Database 26ai)

```sql
-- Store chunks with metadata
CREATE TABLE rag_chunks (
    id          NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    doc_id      NUMBER REFERENCES documents(id),
    chunk_text  CLOB,
    embedding   VECTOR(1024, FLOAT32),
    chunk_index NUMBER,
    metadata    JSON
);

CREATE VECTOR INDEX rag_chunk_idx ON rag_chunks(embedding)
    ORGANIZATION NEIGHBOR PARTITIONS
    DISTANCE COSINE
    WITH TARGET ACCURACY 95;
```

### 5. Retrieval + Generation

```python
def rag_query(question: str, top_k: int = 5) -> str:
    # 1. Embed query
    query_embedding = embed_batch([question], input_type="SEARCH_QUERY")[0]

    # 2. Retrieve relevant chunks
    cursor.execute("""
        SELECT chunk_text, VECTOR_DISTANCE(embedding, :1, COSINE) AS dist
        FROM rag_chunks
        ORDER BY dist
        FETCH FIRST :2 ROWS ONLY
    """, [query_embedding, top_k])
    chunks = cursor.fetchall()

    # 3. Rerank (optional but recommended)
    reranked = genai_client.rerank_text(RerankTextDetails(
        input=question,
        documents=[c[0] for c in chunks],
        serving_mode=OnDemandServingMode(model_id="cohere.rerank-english-v3.5"),
        compartment_id=compartment_id
    ))

    # 4. Generate answer
    context = "\n\n".join([chunks[r.index][0] for r in reranked.data.results[:3]])
    response = genai_client.chat(ChatDetails(
        compartment_id=compartment_id,
        serving_mode=OnDemandServingMode(model_id="cohere.command-a-03-2025"),
        chat_request=CohereChatRequest(
            message=f"Based on the following context, answer: {question}\n\nContext:\n{context}",
            max_tokens=1024,
            temperature=0.2
        )
    ))
    return response.data.chat_response.text
```

### 6. Quality Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Retrieval Recall | >90% | Ground truth comparison |
| Answer Relevance | >4.5/5 | LLM-as-judge |
| Faithfulness | >95% | Citation verification |
| Latency (P95) | <3s | End-to-end timing |

### 7. Production Checklist

- [ ] Chunking strategy validated with sample queries
- [ ] Embedding batch pipeline handles errors/retries
- [ ] Vector index created with appropriate accuracy target
- [ ] Reranking improves retrieval quality (A/B test)
- [ ] Generation includes source citations
- [ ] Monitoring: token usage, latency, error rates
- [ ] Security: data encryption, access controls, audit logging
