---
name: oracle-research
description: Research Oracle/AI topics with confidentiality-aware synthesis, OCI patterns, and structured knowledge capture for consulting engagements
version: 2.0.0
platform: [claude-code, cline, cursor, roocode]
activation:
  claude_code: /oracle-research
  cline: "@skills/oracle-research/SKILL.md"
  cursor: "@skills/oracle-research/SKILL.md"
---

# Oracle Research Skill

This skill enables confidential research for Oracle consulting work, with built-in OCI knowledge and abstraction protocols.

---

## Research Protocols

### Quick Research (5 min)
1. Single WebSearch query
2. Summarize top 3 findings
3. Note one actionable insight

### Standard Research (15 min)
1. 3-5 WebSearch queries with variations
2. Cross-reference findings
3. Create topic file in `research/topics/`
4. Link to projects if relevant

### Deep Dive (30+ min)
1. Comprehensive search strategy
2. 5+ sources analyzed
3. Synthesis with patterns identified
4. Recommendations documented
5. Follow-up research noted

---

## Search Strategy by Domain

### OCI Services
```
"OCI [service] best practices 2026"
"Oracle Cloud [service] architecture patterns"
"[service] vs [competitor] enterprise comparison"
```

### Generative AI
```
"enterprise GenAI architecture 2026"
"LLM [use case] production deployment"
"RAG implementation [industry] enterprise"
```

### Kubernetes/Cloud Native
```
"OKE [feature] production patterns"
"Kubernetes [pattern] enterprise scale"
"service mesh [option] OCI integration"
```

### Industry-Specific
```
"[industry] AI use cases 2026"
"[industry] cloud transformation patterns"
"Oracle [industry] solutions"
```

---

## OCI Service Quick Reference

### Compute
| Service | Use Case | Key Feature |
|---------|----------|-------------|
| Compute VMs | General workloads | Flexible shapes |
| Bare Metal | High performance | Dedicated hardware |
| GPU Instances | AI/ML training | A100, A10, L40S |
| Container Instances | Serverless containers | No cluster overhead |

### AI/ML
| Service | Use Case | Key Feature |
|---------|----------|-------------|
| OCI Generative AI | LLM inference | Cohere, Meta models |
| AI Services | Pre-built AI | Vision, Language, Speech |
| Data Science | ML development | Notebooks, jobs, models |
| Dedicated AI Clusters | Large-scale AI | Reserved GPU capacity |

### Data
| Service | Use Case | Key Feature |
|---------|----------|-------------|
| Autonomous DB | Self-managing | Auto-tuning, patching |
| MySQL HeatWave | Analytics | In-memory acceleration |
| NoSQL | Key-value/JSON | Massive scale, low latency |
| Object Storage | Data lake | S3-compatible, tiers |

### Containers
| Service | Use Case | Key Feature |
|---------|----------|-------------|
| OKE | Kubernetes | Managed control plane |
| Virtual Nodes | Serverless K8s | No node management |
| Container Registry | Image storage | Scanning, signing |

---

## Confidentiality Quick Rules

### NEVER in Research
- Customer names in search queries
- Customer-specific configurations
- Internal Oracle pricing
- Unreleased product details

### ALWAYS Abstract
| Real | Abstracted |
|------|------------|
| Customer name | Codename (K, V, E) or "the customer" |
| Specific amount | Range or "significant" |
| Exact date | Quarter (Q1, Q2) |
| Specific region | "primary region" |

### Safe to Include
- Public OCI documentation
- Oracle blog posts
- Conference talks (public)
- Generic architecture patterns
- Industry trends (non-customer specific)

---

## Research Output Templates

### Topic Research
```markdown
# [Topic]

**Date:** YYYY-MM-DD
**Type:** Quick / Standard / Deep Dive
**Project:** K / V / E / General

## Summary
[2-3 sentences]

## Key Findings
1. [Finding with source]
2. [Finding with source]

## Application
[How this applies to current work]

## Sources
- [URL 1]
- [URL 2]
```

### Competitive Research
```markdown
# [Topic]: OCI vs Alternatives

**Date:** YYYY-MM-DD
**Context:** [Why comparing]

## Comparison Matrix
| Criteria | OCI | Alternative |
|----------|-----|-------------|
| [Criterion] | [OCI position] | [Alt position] |

## OCI Advantages
- [Advantage 1]

## Areas to Address
- [Gap 1]

## Positioning Guidance
[How to discuss with customers]
```

---

## Mandatory GitHub Resources

**ALWAYS check these before recommending custom solutions:**

| Repository | URL | When to Check |
|------------|-----|---------------|
| **OCI AI Blueprints** | https://github.com/oracle-quickstart/oci-ai-blueprints | GenAI on OKE, vLLM, Llama Stack, fine-tuning |
| **AI Accelerator Packs** | [docs/ai_accelerator_packs](https://github.com/oracle-quickstart/oci-ai-blueprints/blob/main/docs/ai_accelerator_packs/about.md) | Route optimization, video search, enterprise reasoning |
| **technology-engineering** | https://github.com/oracle-devrel/technology-engineering | Solution patterns, architecture guides |
| **oracle-quickstart** | https://github.com/oracle-quickstart | Terraform modules, reference architectures |

### Customer GenAI Decision Flow
```
Customer needs GenAI deployment?
├── Standard LLM inference → OCI AI Blueprints (vLLM)
├── Complete agentic stack → OCI AI Blueprints (Llama Stack)
├── Fine-tuning → OCI AI Blueprints (LoRA Fine-Tuning)
├── Route optimization → AI Accelerator Pack (cuOPT)
├── Video analysis → AI Accelerator Pack (VSS)
├── Enterprise chat agent → AI Accelerator Pack (AI-Q)
└── Unique requirements → Custom with oracle-devrel patterns
```

---

## Integration Points

### Link to Projects
When research relates to K, V, or E:
1. Save in `research/projects/[CODENAME]/`
2. Update project file's Notes section
3. Suggest application abstractly

### Feed Synthesis
Weekly, review `research/topics/` and create:
`research/synthesis/week-YYYY-WW.md`

### Support Daily Capture
Research findings can become:
- Learnings in daily capture
- Skills developed in project files
- Content for reports

---

*This skill is automatically loaded when research context is detected.*

---

## Cline Activation

To use this skill in Cline, reference it at the start of your message:

```
@skills/oracle-research/SKILL.md

Research OCI Generative AI pricing options for a high-volume document processing use case.
```

Or in a `.clinerules` workflow:
```markdown
## Research Tasks
When asked to research Oracle or OCI topics, load @skills/oracle-research/SKILL.md and follow the Standard Research protocol. Save output to research/topics/.
```

**Triggers:** Oracle research, OCI comparison, architecture patterns, GenAI use cases
