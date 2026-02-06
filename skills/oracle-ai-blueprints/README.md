# OCI AI Blueprints & Accelerator Packs

You are an expert in OCI AI Blueprints — the no-code platform for deploying GenAI workloads on OKE.

## CRITICAL: Check AI Blueprints BEFORE designing custom GenAI infrastructure.

**Repository:** https://github.com/oracle-quickstart/oci-ai-blueprints

## Available Blueprints

| Blueprint | Use Case | Key Features |
|-----------|----------|--------------|
| **LLM Inference (vLLM)** | Deploy Llama/Mistral | NVIDIA GPU shapes, auto-scaling |
| **Llama Stack** | Complete GenAI runtime | vLLM + ChromaDB + Postgres + Jaeger |
| **LoRA Fine-Tuning** | Custom model training | HuggingFace models, hyperparameter tuning |
| **Multi-node Inference** | Llama-405B sized | RDMA + H100 nodes + LeaderWorkerSet |
| **Autoscaling Inference** | Production serving | KEDA + latency-based scaling |
| **MIG Inference** | Cost optimization | Multi-instance GPU partitioning |
| **CPU Inference** | Budget/testing | Ollama + Mistral/Gemma |

## AI Accelerator Packs (Enterprise ORM Stacks)

| Pack | Industry | NVIDIA Tech | GPU |
|------|----------|-------------|-----|
| **Route Optimizer** | Logistics | cuOPT | A100 |
| **Video Intelligence** | Media/Enterprise | Cosmos + Parakeet NIMs | A100 |
| **AI-Q Self-Hosted** | Enterprise (sovereign) | NVIDIA NIMs | 16x A100 |
| **AI-Q with OCI GenAI** | Enterprise (managed) | Llama Stack + GenAI | CPU + GenAI |
| **Content Moderation** | Media platforms | Multimodal analysis | A100 |

**All packs include:** Prometheus, Grafana, PostgreSQL, KEDA, MLFlow

## Tech Stack
```
┌─────────────────────────────────────────────┐
│              AI ACCELERATOR PACK             │
│  ┌─────────────────────────────────────┐    │
│  │         Application Layer           │    │
│  │  Route Optimizer │ VSS │ AI-Q       │    │
│  └─────────────────────────────────────┘    │
├─────────────────────────────────────────────┤
│              NVIDIA SOFTWARE                 │
│  cuOPT │ Cosmos │ NVIDIA NIMs               │
├─────────────────────────────────────────────┤
│              INFERENCE LAYER                 │
│  vLLM (OpenAI-compatible API)               │
├─────────────────────────────────────────────┤
│              OBSERVABILITY                   │
│  Prometheus │ Grafana │ MLFlow │ KEDA       │
├─────────────────────────────────────────────┤
│              OCI INFRASTRUCTURE              │
│  OKE │ GPU Compute │ Object Storage         │
└─────────────────────────────────────────────┘
```

## Customer Decision Tree

```
Customer needs GenAI on OCI?
├── Standard inference (Llama, Mistral)?
│   └── USE: AI Blueprints - vLLM or Llama Stack
├── Fine-tuning required?
│   └── USE: AI Blueprints - LoRA Fine-Tuning
├── Route optimization?
│   └── USE: AI Accelerator Pack - cuOPT
├── Video analysis?
│   └── USE: AI Accelerator Pack - VSS
├── Enterprise reasoning agent?
│   └── USE: AI Accelerator Pack - AI-Q
├── Multimodal / OCI GenAI Service?
│   └── USE: Cohere Command A Vision + GenAI endpoints
└── Custom/unique requirements?
    └── Build custom with oracle-devrel patterns
```

## Industry Fit Matrix

| Industry | Route Optimizer | Video Intelligence | AI-Q | GenAI Search |
|----------|:-:|:-:|:-:|:-:|
| Logistics | +++ | + | ++ | +++ |
| Healthcare | ++ | +++ | +++ | +++ |
| Financial Services | + | + | +++ | +++ |
| Manufacturing | +++ | +++ | ++ | ++ |
| Media | + | +++ | ++ | ++ |

## References
- [AI Blueprints GitHub](https://github.com/oracle-quickstart/oci-ai-blueprints)
- [AI Accelerator Packs](https://github.com/oracle-quickstart/oci-ai-blueprints/blob/main/docs/ai_accelerator_packs/about.md)
- [Product Page](https://www.oracle.com/artificial-intelligence/ai-accelerator-packs/)
