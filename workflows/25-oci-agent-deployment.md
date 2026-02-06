# Workflow: Deploy AI Agents on OCI

## Purpose
Deploy production-grade AI agents using Oracle ADK with governance and monitoring.

## Steps

### 1. Design Agent (AGENT Blueprint)

```yaml
name: "[Agent Name]"
version: "1.0.0"

# A - Architecture
primary_function: "[One-sentence mission]"
core_competencies:
  - "[Capability 1]"
  - "[Capability 2]"
  - "[Capability 3]"

# G - Governance
decision_authority:
  full: "Domain expertise, low risk, reversible"
  collaborative: "Cross-domain, medium risk"
  advisory: "Outside domain, high risk, irreversible"

# E - Execution (Tools)
tools:
  - name: "[tool_name]"
    description: "[What it does]"
    inputs: [...]
    outputs: [...]

# N - Network (Multi-Agent)
orchestration_pattern: "sequential | parallel | hierarchical"

# T - Transformation (KPIs)
metrics:
  efficiency: "[Time saved, error reduction]"
  quality: "[Accuracy, satisfaction]"
  business: "[Revenue, cost savings]"
```

### 2. Create Knowledge Base

```python
from oci.generative_ai_agent import GenerativeAiAgentClient
from oci.generative_ai_agent.models import *

agent_client = GenerativeAiAgentClient(config)

# Create knowledge base
kb = agent_client.create_knowledge_base(
    CreateKnowledgeBaseDetails(
        compartment_id=compartment_id,
        display_name="agent-knowledge-base",
        description="Agent domain knowledge",
        index_config=DefaultIndexConfig(
            should_enable_hybrid_search=True
        )
    )
)

# Add data source (Object Storage)
agent_client.create_data_source(
    CreateDataSourceDetails(
        compartment_id=compartment_id,
        knowledge_base_id=kb.data.id,
        display_name="docs-source",
        data_source_config=OciObjectStorageDataSourceConfig(
            object_storage_prefixes=[
                ObjectStoragePrefix(
                    namespace=namespace,
                    bucket=bucket_name,
                    prefix="agent-docs/"
                )
            ]
        )
    )
)
```

### 3. Create Agent

```python
agent = agent_client.create_agent(
    CreateAgentDetails(
        compartment_id=compartment_id,
        display_name="production-agent",
        description="Enterprise AI agent",
        knowledge_base_ids=[kb.data.id],
        welcome_message="How can I help you today?"
    )
)

# Create endpoint
endpoint = agent_client.create_agent_endpoint(
    CreateAgentEndpointDetails(
        compartment_id=compartment_id,
        agent_id=agent.data.id,
        display_name="agent-endpoint",
        should_enable_citation=True,
        should_enable_trace=True
    )
)
```

### 4. Test Agent

```python
from oci.generative_ai_agent_runtime import GenerativeAiAgentRuntimeClient
from oci.generative_ai_agent_runtime.models import ChatDetails, CreateSessionDetails

runtime = GenerativeAiAgentRuntimeClient(config)

# Create session
session = runtime.create_session(
    CreateSessionDetails(
        display_name="test-session",
        description="Testing"
    ),
    agent_endpoint_id=endpoint.data.id
)

# Chat
response = runtime.chat(
    agent_endpoint_id=endpoint.data.id,
    chat_details=ChatDetails(
        user_message="What are the key findings from Q4?",
        session_id=session.data.id,
        should_stream=False
    )
)
print(response.data.message.content.text)
```

### 5. Production Checklist

- [ ] Agent tested with representative queries
- [ ] Knowledge base indexed and verified
- [ ] Citation enabled for traceability
- [ ] Tracing enabled for debugging
- [ ] IAM policies restrict access appropriately
- [ ] Monitoring configured (OCI Monitoring + custom metrics)
- [ ] Error handling and fallback responses defined
- [ ] Governance framework documented (decision authority levels)
