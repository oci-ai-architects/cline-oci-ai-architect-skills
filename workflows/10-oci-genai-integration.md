# Workflow: OCI GenAI Service Integration

## Purpose
Add OCI GenAI capabilities to an application with proper model selection and SDK setup.

## Steps

### 1. Select Model (MANDATORY)

Check current models at: https://docs.oracle.com/en-us/iaas/Content/generative-ai/pretrained-models.htm

| Use Case | PRIMARY Model | Alternative |
|----------|---------------|-------------|
| Complex Reasoning | Cohere Command A Reasoning | Gemini 2.5 Pro |
| Multimodal (Images) | Cohere Command A Vision | Llama 3.2 90B Vision |
| Agentic Workflows | Llama 4 Maverick | Grok 4.1 Fast |
| Coding | Grok Code Fast 1 | Llama 4 Maverick |
| RAG / Retrieval | Cohere Command R (08-2024) | Cohere Command A |
| Embeddings | Cohere Embed 4 | Embed Multilingual 3 |
| Reranking | Cohere Rerank 3.5 | - |
| Speed/Cost | Gemini 2.5 Flash-Lite | Grok 3 Mini Fast |
| EU Data Residency | Cohere Command A series | - |
| Long Context (2M) | Grok 4.1 Fast | Gemini 2.5 Pro |

### 2. Set Up Python SDK
```bash
pip install oci
```

### 3. Configure Authentication
```python
import oci

# Option A: Config file (development)
config = oci.config.from_file()

# Option B: Instance principal (production)
signer = oci.auth.signers.InstancePrincipalsSecurityTokenSigner()
config = {'region': 'eu-frankfurt-1'}
```

### 4. Text Generation
```python
from oci.generative_ai_inference import GenerativeAiInferenceClient
from oci.generative_ai_inference.models import (
    ChatDetails, CohereChatRequest, OnDemandServingMode
)

client = GenerativeAiInferenceClient(config)

response = client.chat(
    ChatDetails(
        compartment_id=compartment_id,
        serving_mode=OnDemandServingMode(
            model_id="cohere.command-a-03-2025"
        ),
        chat_request=CohereChatRequest(
            message="Summarize this document",
            max_tokens=2048,
            temperature=0.3
        )
    )
)
print(response.data.chat_response.text)
```

### 5. Embeddings
```python
from oci.generative_ai_inference.models import EmbedTextDetails

response = client.embed_text(
    EmbedTextDetails(
        inputs=["Your text here"],
        serving_mode=OnDemandServingMode(
            model_id="cohere.embed-english-v3.0"
        ),
        compartment_id=compartment_id,
        input_type="SEARCH_DOCUMENT"
    )
)
embeddings = response.data.embeddings
```

### 6. Verify Integration
```python
# Test with a simple prompt
response = client.chat(ChatDetails(...))
assert response.status == 200
print(f"Model: {response.data.model_id}")
print(f"Tokens: {response.data.chat_response.finish_reason}")
```

## Decision Points
- **On-Demand vs Dedicated**: On-demand for <10K requests/day, DAC for production
- **Cohere vs Llama**: Cohere for EU residency, Llama for fine-tuning
- **API vs Blueprints**: API for managed, Blueprints for self-hosted vLLM
