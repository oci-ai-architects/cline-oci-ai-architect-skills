# Workflow: OCI AI Vector Search Setup

## Purpose
Implement AI Vector Search using Oracle AI Database 26ai for semantic search and RAG.

## Steps

### 1. Set Up Oracle AI Database 26ai

```sql
-- Create vector-enabled table
CREATE TABLE documents (
    id          NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    title       VARCHAR2(500),
    content     CLOB,
    embedding   VECTOR(1024, FLOAT32),
    metadata    JSON,
    created_at  TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

### 2. Create Vector Index

```sql
-- Unified Hybrid Vector Search index (26ai feature)
CREATE VECTOR INDEX doc_embedding_idx ON documents(embedding)
    ORGANIZATION NEIGHBOR PARTITIONS
    DISTANCE COSINE
    WITH TARGET ACCURACY 95;
```

### 3. Generate Embeddings with OCI GenAI

```python
import oci
import oracledb

# OCI GenAI client
genai_client = oci.generative_ai_inference.GenerativeAiInferenceClient(config)

def get_embedding(text: str) -> list:
    response = genai_client.embed_text(
        oci.generative_ai_inference.models.EmbedTextDetails(
            inputs=[text],
            serving_mode=oci.generative_ai_inference.models.OnDemandServingMode(
                model_id="cohere.embed-english-v3.0"
            ),
            compartment_id=compartment_id,
            input_type="SEARCH_DOCUMENT"
        )
    )
    return response.data.embeddings[0]

# Store embedding
connection = oracledb.connect(user="admin", password=pw, dsn=dsn)
cursor = connection.cursor()
embedding = get_embedding("Your document text")
cursor.execute(
    "INSERT INTO documents (title, content, embedding) VALUES (:1, :2, :3)",
    ["Doc Title", "Document content...", embedding]
)
connection.commit()
```

### 4. Implement Similarity Search

```sql
-- Vector similarity search
SELECT id, title,
       VECTOR_DISTANCE(embedding, :query_embedding, COSINE) AS distance
FROM documents
ORDER BY distance
FETCH FIRST 10 ROWS ONLY;
```

### 5. Hybrid Search (Vector + Keyword)

```sql
-- Oracle AI Database 26ai Unified Hybrid Vector Search
SELECT id, title, content,
       VECTOR_DISTANCE(embedding, :query_embedding, COSINE) AS vec_score,
       SCORE(1) AS text_score,
       (0.7 * (1 - VECTOR_DISTANCE(embedding, :query_embedding, COSINE))
        + 0.3 * SCORE(1)) AS hybrid_score
FROM documents
WHERE CONTAINS(content, :keyword_query, 1) > 0
ORDER BY hybrid_score DESC
FETCH FIRST 10 ROWS ONLY;
```

### 6. Add Reranking

```python
# Use Cohere Rerank 3.5 for result quality
rerank_response = genai_client.rerank_text(
    oci.generative_ai_inference.models.RerankTextDetails(
        input=query,
        documents=[doc['content'] for doc in results],
        serving_mode=oci.generative_ai_inference.models.OnDemandServingMode(
            model_id="cohere.rerank-english-v3.5"
        ),
        compartment_id=compartment_id
    )
)
```

### 7. Verify
```sql
-- Check index status
SELECT index_name, status, num_vectors
FROM user_vector_indexes;

-- Test similarity search latency
SET TIMING ON;
SELECT id FROM documents
ORDER BY VECTOR_DISTANCE(embedding, :test_vector, COSINE)
FETCH FIRST 5 ROWS ONLY;
```

## Decision Points
- **26ai vs OpenSearch**: 26ai for existing Oracle customers, OpenSearch for dedicated search
- **Embed 4 vs Embed 3**: Embed 4 for multimodal, Embed 3 for text-only
- **Hybrid vs Pure Vector**: Hybrid for enterprise docs (combines keyword + semantic)
