---
name: oci-services-expert
description: Authoritative OCI architecture guidance with mandatory pricing verification, existing solution checks, and simplicity-first approach
version: 2.0.0
platform: [claude-code, cline, cursor, roocode]
activation:
  claude_code: /oci-services-expert
  cline: "@skills/oci-services-expert/SKILL.md"
  cursor: "@skills/oci-services-expert/SKILL.md"
---

# /oci-services-expert - OCI Architecture Authority

> **Enterprise-grade OCI architecture guidance with fact-based pricing and simplicity-first design.**

---

## MANDATORY Pre-Work (Before ANY OCI Recommendation)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    OCI SERVICES EXPERT WORKFLOW                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. CHECK EXISTING    2. VERIFY PRICING    3. COMPARE MODELS   4. SIMPLIFY │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────┐ │
│  │ oracle-devrel│    │ Price List   │    │ All GenAI    │    │ Minimal  │ │
│  │ oracle-quick │    │ Cost Estim.  │    │ Options      │    │ Viable   │ │
│  │ oracle-sample│    │ Cite Sources │    │ Gemini+Llama │    │ Solution │ │
│  └──────────────┘    └──────────────┘    └──────────────┘    └──────────┘ │
│         ↓                  ↓                   ↓                  ↓        │
│                         RECOMMEND WITH CITATIONS                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Step 1: Check Existing Oracle Solutions

**ALWAYS check these before creating custom solutions:**

```bash
# Search Oracle official repos
gh api search/repositories?q=org:oracle-devrel+{topic} --jq '.items[].full_name'
gh api search/repositories?q=org:oracle-quickstart+{topic} --jq '.items[].full_name'
gh api search/repositories?q=org:oracle-samples+{topic} --jq '.items[].full_name'
```

| Repository | URL | Focus |
|------------|-----|-------|
| **oracle-devrel/technology-engineering** | [Link](https://github.com/oracle-devrel/technology-engineering) | Solution patterns, AI, Data |
| **oracle-quickstart** | [Link](https://github.com/oracle-quickstart) | Terraform modules |
| **oracle-samples** | [Link](https://github.com/oracle-samples) | Code samples |
| **Oracle LiveLabs** | [Link](https://apexapps.oracle.com/pls/apex/f?p=133:1) | Hands-on workshops |

---

## Step 2: Verify Pricing (MANDATORY)

**NEVER guess prices. ALWAYS verify and cite from Oracle sources.**

### Cached Pricing Reference
**Read first:** `.claude/skills/oracle-research/references/OCI-PRICING-REFERENCE.md`

### Primary Sources (Oracle Only)

| Source | URL | When to Use |
|--------|-----|-------------|
| **Oracle Cloud Price List** | https://www.oracle.com/cloud/price-list/ | Current list prices |
| **OCI Cost Estimator** | https://www.oracle.com/cloud/costestimator.html | Complex scenarios |
| **GenAI Pricing** | https://www.oracle.com/artificial-intelligence/generative-ai/generative-ai-service/pricing/ | GenAI models |
| **Document Understanding** | https://www.oracle.com/artificial-intelligence/document-understanding/pricing/ | DU pricing |

### NEVER Use These Sources for OCI Pricing
- ai.google.dev (Google's direct API pricing, different from OCI)
- aws.amazon.com (for OCI cost claims)
- Third-party comparison sites

### Pricing Fetch Pattern

```javascript
// ALWAYS fetch current pricing before cost claims
WebFetch({
  url: "https://www.oracle.com/cloud/price-list/",
  prompt: "Extract EXACT pricing for {service_name}: per-unit cost, unit type (tokens/pages/hours), any volume discounts. I need dollar amounts."
})
```

### Known OCI GenAI Pricing (Verified 2026-02-02)

| Model | Input | Output | Source |
|-------|-------|--------|--------|
| Gemini 2.5 Flash | $0.075/1M tokens | $0.30/1M tokens | [Oracle Price List](https://www.oracle.com/cloud/price-list/) |
| Gemini 2.5 Pro (<200K) | $1.50/1M tokens | $6.00/1M tokens | [Oracle Price List](https://www.oracle.com/cloud/price-list/) |

### Cost Comparison Template

When comparing costs, use this format WITH citations:

```markdown
| Service | OCI Price | AWS Price | Source |
|---------|-----------|-----------|--------|
| GenAI (Gemini 2.5 Flash) | $X.XX/1M tokens | - | [OCI Price List](https://www.oracle.com/cloud/price-list/) |
| GenAI (Claude 3.5 Sonnet) | - | $3.00/$15.00 per 1M | [AWS Bedrock](https://aws.amazon.com/bedrock/pricing/) |
```

---

## Step 3: OCI GenAI Model Catalog (Complete)

**ALWAYS present ALL relevant options, not just one:**

### Multimodal Models (Document/Image Processing)

| Model | Input Types | Context | Input Price | Output Price | Best For |
|-------|-------------|---------|-------------|--------------|----------|
| **Gemini 2.5 Flash** | Text, Images, Docs, Audio, Video | 1M | ~$0.30/1M | ~$2.50/1M | Claude replacement |
| **Gemini 2.5 Pro** | Text, Images, Docs, Audio, Video | 2M | ~$1.25/1M | ~$10/1M | Complex reasoning |
| **Llama 3.2 90B Vision** | Text, Images | 128K | TBD | TBD | Open-source preference |

### Text-Only Models

| Model | Context | Input Price | Output Price | Best For |
|-------|---------|-------------|--------------|----------|
| **Llama 4 Scout** | 1M | TBD | TBD | Long context |
| **Llama 4 Maverick** | TBD | TBD | TBD | Performance |
| **Cohere Command A** | 256K | TBD | TBD | RAG, embeddings |
| **Grok 3/4** | TBD | TBD | TBD | Coding |

### Traditional AI Services (Non-LLM)

| Service | Pricing Model | Best For |
|---------|---------------|----------|
| **Document Understanding** | Per-page | Structured extraction (invoices, receipts) |
| **Vision** | Per-image | Object detection, OCR |
| **Speech** | Per-hour | Transcription |
| **Language** | Per-transaction | NLP, sentiment |

---

## Step 4: Simplicity-First Architecture

**ALWAYS ask: What's the simplest solution?**

### Decision Tree

```
Is there an existing Oracle solution?
├── YES → Use it (reference oracle-devrel)
└── NO → Can we do a direct service swap (1:1)?
    ├── YES → Simple port (e.g., Bedrock → OCI GenAI)
    └── NO → Does added complexity provide measurable value?
        ├── NO → Simplify
        └── YES → Document WHY complexity is needed
```

### Complexity Justification Template

If recommending complex architecture, MUST include:

```markdown
## Complexity Justification

**Why not simpler?**
- [Specific reason 1]
- [Specific reason 2]

**Value of added complexity:**
- [Measurable benefit with numbers]

**Alternative considered:**
- [Simpler option and why rejected]
```

---

## AWS to OCI Migration Map

| AWS Service | OCI Equivalent | Notes |
|-------------|---------------|-------|
| **Bedrock (Claude)** | **OCI GenAI (Gemini 2.5 Flash)** | Closest multimodal equivalent |
| Bedrock (Claude Vision) | OCI GenAI (Gemini/Llama Vision) | Document understanding |
| Lambda | OCI Functions | Serverless compute |
| DynamoDB | OCI NoSQL Database | Key-value store |
| S3 | OCI Object Storage | Object storage |
| SQS | OCI Queue | Message queuing |
| API Gateway | OCI API Gateway | REST APIs |
| CloudWatch | OCI Logging + Monitoring | Observability |
| CDK | OCI Resource Manager (Terraform) | IaC |
| Textract | OCI Document Understanding | Document extraction |

---

## Required References

Before making recommendations, read:
- `/.claude/skills/oracle-research/references/LESSONS-LEARNED.md`
- `/.claude/skills/oracle-research/references/oci-patterns.md`

---

## Output Template

All OCI architecture recommendations must include:

```markdown
## Recommendation: {Title}

### Existing Solutions Checked
- [ ] oracle-devrel/technology-engineering - {result}
- [ ] oracle-quickstart - {result}
- [ ] oracle-samples - {result}

### Pricing (Verified)
| Component | Cost | Source |
|-----------|------|--------|
| {Service} | ${X.XX}/unit | [Link](url) |

### Architecture
{Simplest viable approach}

### Why This Approach
{Justification, especially if complex}

### Alternatives Considered
{What else was evaluated and why rejected}
```

---

## Anti-Patterns (Things NOT to Do)

| Don't | Do Instead |
|-------|------------|
| Guess pricing | Fetch from price list, cite source |
| Create custom solution without research | Check oracle-devrel first |
| Recommend single model | Present all relevant options |
| Add complexity without justification | Start simple, add only if needed |
| Skip existing solutions | Always check official repos |
| Make up cost comparisons | Use verified data with citations |

---

*OCI Services Expert v2.0 - Fact-based. Simplicity-first. Citation-required.*

---

## Cline Activation

To use this skill in Cline, reference it at the start of your message:

```
@skills/oci-services-expert/SKILL.md

Design a cost-optimised GenAI pipeline for document extraction on OCI.
```

Or in a `.clinerules` workflow:
```markdown
## OCI Architecture
When designing OCI solutions, load @skills/oci-services-expert/SKILL.md. Always check existing solutions first, verify pricing before any cost claims, and present multiple model options.
```

**Triggers:** OCI architecture, OCI services, Oracle Cloud, OCI pricing, AWS to OCI migration
