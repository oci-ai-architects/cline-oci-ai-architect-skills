# Workflow: Oracle Solution Design

## Purpose
Orchestrate a complete OCI solution design through 5 phases — from discovery to customer-ready deliverables. Each phase has concrete steps and quality gates.

## When to Use
- Customer engagement requires a solution design / architecture recommendation
- Building an SDD (Solution Design Document) for an OCI-based solution
- Need prototype + visuals + BOM for a customer presentation

## Prerequisites
- Customer context provided using codename (NEVER real names)
- OCI tenancy/region known (or to be determined)

---

## Phase 1: DISCOVER

### 1.1 Capture Requirements

Create `clients/[CODE]/deliverables/docs/DISCOVERY.md`:

```markdown
# Discovery: [Solution Name]
## Codename: [CODE]
## Date: [YYYY-MM-DD]

## Business Context
- **Problem**: [What pain exists today?]
- **Current State**: [What tools/processes in use?]
- **Success Criteria**: [What does good look like?]

## Top Use Cases
| # | Use Case | Priority | Data Sources | Expected Output |
|---|----------|----------|-------------|-----------------|
| 1 | | High | | |
| 2 | | High | | |
| 3 | | Medium | | |

## Constraints
| Constraint | Value | Impact |
|-----------|-------|--------|
| Compliance | [GDPR/HIPAA/SOC2/None] | [Affects region + encryption] |
| Timeline | [Weeks/months] | [Affects tier recommendation] |
| Budget | [Range or TBD] | [Affects tier recommendation] |
| Data Residency | [EU/US/Any] | [Affects model + region choice] |
| Existing Cloud | [OCI/AWS/Azure/None] | [Affects integration pattern] |

## Stakeholders
| Role | Concern | Communication |
|------|---------|---------------|
| Executive Sponsor | ROI, timeline | Monthly summary |
| Technical Lead | Integration, security | Weekly detail |
| End Users | Usability, speed | Demo at Phase 4 |
```

### 1.2 Research OCI Capabilities

Search for matching OCI services and existing patterns:

```bash
# Check if OCI AI Blueprints covers this use case
# Visit: https://github.com/oracle-quickstart/oci-ai-blueprints

# Check oracle-quickstart for Terraform modules
# Visit: https://github.com/oracle-quickstart

# Check oracle-devrel for solution patterns
# Visit: https://github.com/oracle-devrel/technology-engineering
```

**OCI AI Blueprints Decision Tree:**
```
Customer needs GenAI on OCI?
├── Standard LLM inference (Llama, Mistral)?
│   └── USE: AI Blueprints - vLLM or Llama Stack
├── Fine-tuning required?
│   └── USE: AI Blueprints - LoRA Fine-Tuning
├── Route optimization?
│   └── USE: AI Accelerator Pack - cuOPT
├── Video analysis?
│   └── USE: AI Accelerator Pack - VSS
├── Enterprise reasoning agent?
│   └── USE: AI Accelerator Pack - AI-Q
├── OCI GenAI Service (managed)?
│   └── USE: Cohere/Llama/Grok/Gemini endpoints
└── Custom/unique requirements?
    └── THEN: Build custom with oracle-devrel patterns
```

### 1.3 Model Selection

Use this matrix when the solution involves GenAI:

| Use Case | PRIMARY Model | Alternative | Why |
|----------|---------------|-------------|-----|
| Complex Reasoning | Cohere Command A Reasoning | Gemini 2.5 Pro | Multi-step logic |
| Multimodal (Images+Text) | Cohere Command A Vision | Llama 3.2 90B Vision | Charts, docs |
| Agentic Workflows | Llama 4 Maverick | Grok 4.1 Fast | Tool-calling, MoE |
| Coding | Grok Code Fast 1 | Llama 4 Maverick | Code specialist |
| RAG / Retrieval | Cohere Command R (08-2024) | Cohere Command A | 128K, retrieval-optimized |
| Embeddings | Cohere Embed 4 (multimodal) | Embed Multilingual 3 | Text + image |
| Reranking | Cohere Rerank 3.5 | - | Relevance scoring |
| Speed/Cost | Gemini 2.5 Flash-Lite | Grok 3 Mini Fast | High-volume |
| EU Data Residency | Cohere Command A series | - | EU OCI regions |
| Long Context (2M) | Grok 4.1 Fast | Gemini 2.5 Pro | Massive documents |
| Fine-tuning | Llama 3.3 70B | Cohere Command | Dedicated AI Clusters |

**Model Selection Decision:**
1. Data residency required? -> Cohere (EU deployable)
2. Fine-tuning needed? -> Llama (LoRA) or Cohere (T-few)
3. Multimodal? -> Cohere Vision, Llama Vision, or Gemini
4. Coding focus? -> Grok Code Fast 1
5. Budget constrained? -> Flash-Lite or Mini Fast
6. Agentic/tool-calling? -> Llama 4 or Grok 4.1 Fast

### Quality Gate 1
- [ ] Top 3 use cases documented with priorities
- [ ] OCI services mapped to each use case
- [ ] Constraints captured (compliance, timeline, budget, residency)
- [ ] AI Blueprints / oracle-quickstart checked
- [ ] GenAI model selection justified (if applicable)
- [ ] No real customer names in ANY file

---

## Phase 2: ARCHITECT

### 2.1 Service Selection

Create a service selection table with rationale:

```markdown
## OCI Service Selection

| Service | Purpose | Why This Service | Alternative Considered | Docs |
|---------|---------|-----------------|----------------------|------|
| OCI GenAI | LLM inference | Managed, pay-per-token | Self-hosted vLLM | [docs](https://docs.oracle.com/iaas/Content/generative-ai/) |
| Oracle AI Database 26ai | Vector + relational | HNSW index, Select AI Agent | Standalone vector DB | [docs](https://docs.oracle.com/en/database/oracle/oracle-database/26/) |
| OKE | Container orchestration | GPU scheduling, AI Blueprints | VM-based deployment | [docs](https://docs.oracle.com/iaas/Content/ContEng/) |
| Object Storage | Data lake | S3-compatible, tiered | Block Storage | [docs](https://docs.oracle.com/iaas/Content/Object/) |
```

### 2.2 Architecture Tiers

Define 3 tiers (every solution needs this):

```markdown
## Architecture Tiers

| Aspect | Basic | Advanced | Premium |
|--------|-------|----------|---------|
| Users | < 50 | 50-500 | 500+ |
| Data Volume | < 100 GB | 100 GB - 1 TB | 1+ TB |
| AI Models | OCI GenAI (shared) | OCI GenAI (dedicated) | DAC + fine-tuned |
| Availability | 99.5% | 99.9% | 99.95% |
| Vector Search | Basic HNSW | Hybrid (vector + keyword) | GraphRAG + reranking |
| Agents | Single agent | Multi-agent, tool-calling | Autonomous orchestration |
| Support | Standard | Business | Enterprise |
| Monthly Est. | $X,XXX | $XX,XXX | $XXX,XXX |
```

### 2.3 Security Architecture

```markdown
## Security Architecture

### IAM
- Compartment isolation per workload
- Dynamic groups for OCI service-to-service auth
- Federated identity (SAML/OIDC) for user access

### Encryption
- At rest: Oracle-managed or customer-managed keys (OCI Vault)
- In transit: TLS 1.3 for all endpoints
- Database: Transparent Data Encryption (TDE)

### Network
- Private subnets for all AI workloads
- Service Gateway for OCI service access (no internet)
- Network Security Groups (NSGs) per service tier
- WAF for public-facing endpoints

### Compliance Mapping
| Requirement | OCI Control | Evidence |
|------------|-------------|---------|
| Data encryption at rest | TDE + OCI Vault | Audit logs |
| Access control | IAM policies + MFA | IAM audit trail |
| Network isolation | Private subnets + NSG | VCN flow logs |
| Data residency | Region selection | Tenancy config |
```

### 2.4 Pricing (BOM)

**MANDATORY:** Verify ALL prices at https://www.oracle.com/cloud/price-list/

```markdown
## Bill of Materials

**Prices verified on: [DATE] at oracle.com/cloud/price-list/**

### Basic Tier
| Service | Shape/Config | Qty | Monthly ($) | Annual ($) | Source |
|---------|-------------|-----|-------------|------------|--------|
| OCI GenAI | [Model] on-demand | [tokens/mo] | $[VERIFIED] | $[VERIFIED] | price-list |
| Autonomous DB | [OCPU] BYOL/License | 1 | $[VERIFIED] | $[VERIFIED] | price-list |
| Object Storage | [GB] Standard | 1 | $[VERIFIED] | $[VERIFIED] | price-list |
| Compute | [Shape] | [count] | $[VERIFIED] | $[VERIFIED] | price-list |
| **TOTAL** | | | **$X,XXX** | **$XX,XXX** | |

### Notes
- Region: [target region]
- Commitment: [PAYG / Annual Flex / Universal Credits]
- Assumptions: [list all assumptions]
```

### Quality Gate 2
- [ ] Every OCI service verified against docs.oracle.com
- [ ] Pricing from oracle.com/cloud/price-list/ with date
- [ ] No "X times cheaper" claims without model-specific comparison
- [ ] AI Blueprints decision tree consulted
- [ ] Security architecture documented (IAM, encryption, network)
- [ ] 3 tiers defined (Basic / Advanced / Premium)
- [ ] Confidentiality: codename only, no client names

---

## Phase 3: VISUALIZE

### 3.1 Generate Architecture Diagrams

**Rules for ALL images:**
- NO Oracle logos — text labels only ("Oracle Cloud", "OCI GenAI")
- Oracle Red (#C74634) for accents
- Dark background (#1A1A2E or similar) for professionalism
- All service names match 2026 branding (26ai, not 23ai)
- Minimum 1920x1080 resolution

**Required Visual Set:**

| # | Image | Audience | File Name |
|---|-------|----------|-----------|
| 1 | Master Architecture | Technical + Executive | architecture.png |
| 2 | User Journey | Executive | user-journey.png |
| 3 | Data Flow | Technical | data-flow.png |
| 4 | Tier Comparison | Commercial | tier-comparison.png |

### 3.2 Draft-Then-Refine

1. Generate DRAFT with fast/cheap model first
2. Review: correct service names? Readable? No logos?
3. Refine prompt based on review
4. Generate FINAL with quality model
5. Save to `clients/[CODE]/deliverables/images/`

### Quality Gate 3
- [ ] ZERO Oracle logos in any image (visual inspection)
- [ ] Service names correct (Oracle AI Database 26ai, not 23ai)
- [ ] No spelling errors
- [ ] Readable at presentation size
- [ ] Architecture matches SOLUTION-DESIGN.md
- [ ] 4 images generated: architecture, journey, data-flow, tiers

---

## Phase 4: PROTOTYPE

### 4.1 Build Interactive Demo

Create `clients/[CODE]/deliverables/prototype/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Solution Name] - Prototype</title>
  <style>
    :root {
      --oracle-red: #C74634;
      --bg-dark: #1A1A2E;
      --bg-card: #16213E;
      --text: #E8E8E8;
      --text-muted: #8892B0;
    }
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: -apple-system, BlinkMacSystemFont, sans-serif;
           background: var(--bg-dark); color: var(--text); }
    /* PROTOTYPE BANNER */
    .proto-banner {
      background: var(--oracle-red);
      color: white; text-align: center;
      padding: 8px; font-size: 14px; font-weight: 600;
    }
    /* Add your prototype styles here */
  </style>
</head>
<body>
  <div class="proto-banner">PROTOTYPE — For Demonstration Purposes Only</div>
  <!-- Build your interactive demo here -->
  <!-- Requirements:
    - One core workflow, end-to-end
    - Mock data (realistic but synthetic)
    - Loading/processing states
    - AI insights panel (simulated)
    - Oracle Red accents
    - No real customer data
  -->
  <script>
    // Prototype JavaScript
    // Simulate API calls with setTimeout
    // Use mock data arrays for search results
  </script>
</body>
</html>
```

### 4.2 Prototype Checklist
- Single HTML file (no build tools, no npm)
- Embedded CSS and JS
- One core user workflow working end-to-end
- Loading animations for "AI processing" steps
- Mock data that's realistic but obviously synthetic
- PROTOTYPE banner always visible
- Oracle Red (#C74634) accent color
- Works standalone (double-click to open in browser)

### Quality Gate 4
- [ ] Core interaction works end-to-end
- [ ] Loading/processing states visible
- [ ] No broken buttons or dead links
- [ ] PROTOTYPE banner displayed
- [ ] No real customer data
- [ ] Opens standalone in browser (no server needed)

---

## Phase 5: DELIVER

### 5.1 Folder Structure

Organize deliverables:

```
clients/[CODE]/
├── SOLUTION-DESIGN.md                # Full SDD (use /55-sdd-generator.md)
├── README.md                         # Codename + status ONLY
└── deliverables/
    ├── images/
    │   ├── architecture.png          # Master architecture
    │   ├── user-journey.png          # Executive flow
    │   ├── data-flow.png             # Technical detail
    │   └── tier-comparison.png       # Commercial comparison
    ├── prototype/
    │   └── index.html                # Interactive demo
    └── docs/
        ├── DISCOVERY.md              # Phase 1 output
        └── BOM.md                    # Pricing by tier
```

### 5.2 Confidentiality Audit

Run `/60-confidentiality-audit.md` before delivery. Or manually verify:

```bash
# Search for potential leaks in deliverables
# Replace [CUSTOMER_NAME] with the actual name from conversation
grep -ri "[CUSTOMER_NAME]" clients/[CODE]/

# Verify README has codename only
cat clients/[CODE]/README.md

# Check git won't commit deliverables
git status clients/[CODE]/
# deliverables/ should NOT appear as tracked
```

### 5.3 README Template

The ONLY committable file per client:

```markdown
# Project [CODE]

| Field | Value |
|-------|-------|
| Status | Active |
| Role | AI/Cloud Architecture |
| Started | [Month Year] |
```

Nothing else. No industry, no scope, no customer details.

### Quality Gate 5
- [ ] All files in correct folder structure
- [ ] README has codename only (no industry, no scope)
- [ ] Images are logo-free (final visual inspection)
- [ ] Prototype opens standalone in browser
- [ ] BOM prices verified with dates and sources
- [ ] Confidentiality audit PASSED
- [ ] Would McKinsey present this to a Fortune 500 CEO? Yes/No

---

## Anti-Patterns

| Don't | Do Instead |
|-------|-----------|
| Skip discovery, jump to architecture | Always validate requirements first |
| Use memorized pricing | Verify at oracle.com/cloud/price-list/ every time |
| Include Oracle logos in images | Text labels only, Oracle Red accents |
| Put client details in README | Codename + status only, everything else in deliverables/ |
| Build complex prototype with build tools | Single HTML file, embedded CSS/JS |
| Say "OCI is 80x cheaper" | Cite specific model comparison with source |
| Deliver without confidentiality audit | Run /60-confidentiality-audit.md before every delivery |
| Skip tiers, build one-size-fits-all | Always define Basic / Advanced / Premium |
