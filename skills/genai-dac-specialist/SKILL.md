---
name: genai-dac-specialist
description: Expert guidance for OCI Dedicated AI Clusters — private LLM hosting, fine-tuning, sizing, and cost optimisation for enterprise deployments
version: 1.0.0
platform: [claude-code, cline, cursor, roocode]
activation:
  cline: "@skills/genai-dac-specialist/SKILL.md"
  cursor: "@skills/genai-dac-specialist/SKILL.md"
---

# OCI GenAI Dedicated AI Cluster (DAC) Specialist

You are an expert in Oracle's Dedicated AI Clusters for private LLM hosting and fine-tuning.

## When to Use
- Deploying private LLM infrastructure on OCI
- Sizing and costing DAC deployments
- Fine-tuning models with proprietary data
- High-throughput inference requirements

## Overview

A DAC is a private, isolated GPU cluster in OCI for:
- **Hosting** LLMs without shared infrastructure
- **Fine-tuning** with LoRA adapter training on your data
- **High throughput** with multiple endpoints per cluster
- **Data sovereignty** — data never leaves your tenancy

## Sizing Guide

| Workload | Units | Approx Monthly | Use Case |
|----------|-------|----------------|----------|
| Dev/Test | 2-5 | Verify pricing | Experimentation |
| Production | 5-15 | Verify pricing | Standard workloads |
| High Volume | 15-30 | Verify pricing | Enterprise scale |
| Enterprise | 30-50 | Verify pricing | Maximum throughput |

**IMPORTANT:** Always verify current pricing at oracle.com/cloud/price-list

## Model Selection for DAC

| Model | Best For | Context |
|-------|----------|---------|
| Cohere Command A | Complex reasoning, enterprise | 256K |
| Cohere Command R/R+ | RAG-optimized, retrieval | 128K |
| Llama 3.3 70B | Fine-tuning, customization | 128K |
| Llama 3.1 405B | Maximum capability | 128K |

## Fine-Tuning

### Methods
| Method | Models | Use Case |
|--------|--------|----------|
| LoRA | Llama 3.3 70B, Llama 3.1 405B | Domain-specific adaptation |
| T-few | Cohere Command | Quick enterprise customization |
| Vanilla | Cohere Command | Full fine-tune |

### Process
```
1. Upload training data → Object Storage (JSONL format)
2. Create fine-tuning job → DAC processes
3. Deploy custom endpoint → Use fine-tuned model
4. Monitor performance → Iterate
```

### Also Consider: AI Blueprints LoRA Fine-Tuning
Before setting up a DAC, check if the AI Blueprints LoRA Fine-Tuning blueprint meets your needs:
https://github.com/oracle-quickstart/oci-ai-blueprints

## Terraform Quick Start

```hcl
resource "oci_generative_ai_dedicated_ai_cluster" "main" {
  compartment_id = var.compartment_id
  display_name   = "production-dac"
  type           = "HOSTING"
  unit_count     = 10
  unit_shape     = "LARGE_COHERE"
}

resource "oci_generative_ai_endpoint" "chat" {
  compartment_id         = var.compartment_id
  dedicated_ai_cluster_id = oci_generative_ai_dedicated_ai_cluster.main.id
  model_id               = "cohere.command-a-03-2025"
  display_name           = "chat-endpoint"
}
```

## Cost Optimization
1. Start with on-demand endpoints — validate before committing to DAC
2. Begin with 5 units, scale based on actual usage
3. Multi-model endpoints — share DAC across use cases
4. Monitor utilization — scale down if under 60%

## Documentation
- [DAC Overview](https://docs.oracle.com/en-us/iaas/Content/generative-ai/ai-cluster.htm)
- [Fine-Tuning Guide](https://docs.oracle.com/en-us/iaas/Content/generative-ai/fine-tuning.htm)
- [OCI Fine-tuning Repo](https://github.com/oracle-devrel/oci-genai-finetuning)

---

## Cline Activation

To use this skill in Cline, reference it at the start of your message:

```
@skills/genai-dac-specialist/SKILL.md

Size a Dedicated AI Cluster for a production Cohere Command A deployment with 500 concurrent users.
```

Or in a `.clinerules` workflow:
```markdown
## DAC Sizing
When asked about private LLM hosting on OCI, load @skills/genai-dac-specialist/SKILL.md. Always verify pricing, recommend starting small (5 units), and check AI Blueprints before recommending a full DAC.
```

**Triggers:** Dedicated AI Cluster, DAC, private LLM hosting, OCI fine-tuning, GPU cluster sizing, model fine-tuning OCI
