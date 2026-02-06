# Workflow: OCI Cost Estimation

## Purpose
Estimate OCI costs for AI workloads with MANDATORY pricing verification.

## CRITICAL
- **ALWAYS verify pricing at**: https://www.oracle.com/cloud/price-list/
- **NEVER use cached/memorized prices** — they change frequently
- **NEVER make blanket "X times cheaper" claims** — comparisons are model-specific
- **Use OCI Cost Estimator for complex scenarios**: https://www.oracle.com/cloud/costestimator.html

## Steps

### 1. Identify Workload Components

| Component | Questions |
|-----------|-----------|
| **Compute** | GPU type? Instance count? Hours/month? |
| **GenAI API** | Model? Token volume? Input vs output ratio? |
| **Storage** | Data volume? Access patterns? Tier? |
| **Network** | Egress volume? FastConnect needed? |
| **Database** | Type? OCPU count? Storage? |

### 2. GenAI API Cost Framework

```
Monthly Cost = (Input Tokens / 1M × Input Price) + (Output Tokens / 1M × Output Price)

Example (verify current pricing!):
- 1M input tokens/day × 30 days = 30M tokens/month
- Input: 30M / 1M × $[VERIFY_PRICE] = $[CALCULATE]
- Output: estimated 10M / 1M × $[VERIFY_PRICE] = $[CALCULATE]
```

### 3. GPU Compute Costs

| Shape | Use Case | Verify Price At |
|-------|----------|-----------------|
| VM.GPU.A10.1 | Inference | oracle.com/cloud/price-list |
| VM.GPU.A10.2 | Larger inference | oracle.com/cloud/price-list |
| BM.GPU.A100-v2.8 | Training/large models | oracle.com/cloud/price-list |
| BM.GPU.H100.8 | Maximum performance | oracle.com/cloud/price-list |

### 4. Cost Optimization Checklist

- [ ] **Right-size GPU**: A10 for inference, A100/H100 only for training
- [ ] **Use On-Demand first**: Validate before committing to DAC
- [ ] **Cascade routing**: Simple queries → cheap model, complex → premium
- [ ] **Check AI Blueprints**: Shared infrastructure reduces per-workload cost
- [ ] **Storage tiers**: Standard → Infrequent Access → Archive
- [ ] **Reserved capacity**: Commit for predictable workloads (up to 60% savings)

### 5. Output Format

```markdown
## Cost Estimate: [Project Name]

**Date**: [Date] | **Verified**: oracle.com/cloud/price-list

### Monthly Breakdown
| Component | Service | Configuration | Monthly Cost |
|-----------|---------|---------------|--------------|
| GenAI API | OCI GenAI | [Model], [tokens/mo] | $[VERIFIED] |
| Compute | [Shape] | [Count] × [hours] | $[VERIFIED] |
| Storage | Object Storage | [GB] Standard tier | $[VERIFIED] |
| Database | Autonomous DB | [OCPU] | $[VERIFIED] |
| **Total** | | | **$[SUM]** |

### Assumptions
- [List all assumptions]

### Optimization Opportunities
- [Specific recommendations]
```

## Anti-Patterns
- "OCI is 80x cheaper" — WRONG (model-dependent)
- Using memorized prices — WRONG (prices change)
- Ignoring egress costs — WRONG (can be significant)
- Comparing managed vs self-hosted without context — MISLEADING
