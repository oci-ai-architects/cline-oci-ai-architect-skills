# Oracle Agent Development Kit (ADK)

You are an expert in Oracle's Agent Development Kit for building production-grade AI agents on OCI.

## When to Use
- Building multi-agent systems on OCI
- Implementing tool-calling patterns
- Creating RAG-enabled agents
- Enterprise agent orchestration

## Agent Architecture
```
┌─────────────────────────────────────┐
│           Agent Endpoint            │
├─────────────────────────────────────┤
│  ┌─────────┐  ┌─────────┐          │
│  │  Agent  │  │ Tools   │          │
│  │  Logic  │◄─┤ Registry│          │
│  └────┬────┘  └─────────┘          │
│       │                             │
│  ┌────▼────┐  ┌─────────┐          │
│  │Knowledge│  │ Session │          │
│  │  Bases  │  │ Memory  │          │
│  └─────────┘  └─────────┘          │
└─────────────────────────────────────┘
```

## Agent Types
1. **Simple Agent** — Single-turn Q&A with tools
2. **RAG Agent** — Knowledge base retrieval + generation
3. **Multi-Agent** — Orchestrated agent workflows
4. **Streaming Agent** — Real-time response streaming

## SDK Quick Reference

### Python Agent Creation
```python
from oci.generative_ai_agent_runtime import GenerativeAiAgentRuntimeClient
from oci.generative_ai_agent_runtime.models import ChatDetails

client = GenerativeAiAgentRuntimeClient(config)

response = client.chat(
    agent_endpoint_id="ocid1.genaiagentendpoint.oc1...",
    chat_details=ChatDetails(
        user_message="What are our Q4 sales figures?",
        session_id=session_id
    )
)
```

## Multi-Agent Patterns

### Sequential
```
Agent A → Agent B → Agent C → Result
```

### Parallel
```
         ┌─ Agent A ─┐
Request ─┼─ Agent B ─┼─ Synthesizer → Result
         └─ Agent C ─┘
```

### Hierarchical
```
      Orchestrator
         │
    ┌────┼────┐
    ▼    ▼    ▼
   A1   A2   A3  (Specialists)
```

## Enterprise Considerations
- **Security**: Agent isolation, credential management via OCI Vault
- **Observability**: Tracing, logging, metrics via OCI Monitoring
- **Governance**: Audit trails, approval workflows
- **Scaling**: Auto-scaling endpoints, load balancing

## Documentation
- [Oracle ADK](https://docs.oracle.com/en-us/iaas/Content/generative-ai-agents/adk/)
- [GenAI Agents](https://docs.oracle.com/en-us/iaas/Content/generative-ai-agents/overview.htm)
