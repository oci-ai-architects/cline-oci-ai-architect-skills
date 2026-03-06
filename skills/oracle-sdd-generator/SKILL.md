---
name: oracle-sdd-generator
description: Generate structured Solution Design Documents (SDD) for OCI-based customer solutions. Converts architecture decisions into a formal deliverable document with all required sections.
version: 1.0.0
platform: [cline, cursor, roocode, windsurf]
keywords: [sdd, solution-design-document, documentation, oracle, oci, deliverable]
triggers:
  - "generate SDD"
  - "write solution design doc"
  - "create SDD"
  - "solution design document"
---

# Oracle SDD Generator

> **Purpose:** Generate structured Solution Design Documents for OCI-based customer solutions.
> **Prerequisite:** Phase 2 (ARCHITECT) of oracle-solution-design should be complete, or user must provide equivalent context in conversation.

## When to Use

Invoke when you need:
- A formal SDD for a customer engagement
- A structured technical document from architecture decisions
- To convert conversation-based design into a deliverable

**Trigger:** `/workflow 55-sdd-generator`

---

## SDD Structure

Every SDD follows this structure:

```
clients/[CODE]/SOLUTION-DESIGN.md
  1. Executive Summary
  2. Business Context
  3. Requirements Summary
  4. Solution Architecture
     4.1 Architecture Overview
     4.2 OCI Service Selection
     4.3 Data Architecture
     4.4 Integration Architecture
     4.5 Security Architecture
  5. Implementation Approach
     5.1 Tier Definitions
     5.2 Phased Roadmap
     5.3 Dependencies and Assumptions
  6. Cost Estimation (BOM)
     6.1 Per-Tier Breakdown
     6.2 Pricing Sources
  7. Risk Assessment
  8. Success Criteria
  Appendix A: OCI Service Reference
  Appendix B: Glossary
```

---

## Section Guidelines

### 1. Executive Summary
- 3-5 sentences maximum
- Business outcome focus, not technical details
- Understandable by a non-technical executive
- Include solution tier recommendation

### 2. Business Context
- Problem statement
- Current state
- Desired future state
- **CONFIDENTIALITY:** Use "the organization" — never name the customer

### 3. Requirements Summary
Table format:
| Requirement | Priority | OCI Service | Notes |

Top 3-5 functional + top 3-5 non-functional requirements.

### 4. Solution Architecture

**4.2 OCI Service Selection — MANDATORY TABLE:**
| Service | Purpose | Why This Service | Alternative Considered | Docs |

Each selection MUST reference official documentation (docs.oracle.com).

**4.5 Security Architecture — MANDATORY SECTIONS:**
- IAM and RBAC model (compartments, dynamic groups, federated identity)
- Encryption (TDE + OCI Vault, TLS 1.3)
- Network isolation (VCN, private subnets, NSGs, WAF)
- Compliance framework mapping

### 5. Implementation Approach

**5.1 Tier Definitions — MANDATORY:**
| Aspect | Basic | Advanced | Premium |

**5.2 Phased Roadmap:**
- Phase 1: Foundation (weeks 1-4)
- Phase 2: Core capabilities (weeks 5-8)
- Phase 3: Advanced features (weeks 9-12)
- Phase 4: Optimization (weeks 13-16)

Timeline is indicative only. Never commit to dates without customer input.

### 6. Cost Estimation (BOM)

**MANDATORY RULES:**
- All prices MUST come from `oracle.com/cloud/price-list/`
- Include date of price verification
- Show per-month and per-year totals

**Format:**
| Service | Shape/Config | Qty | Monthly ($) | Annual ($) | Source |
| **TOTAL** | | | **$X,XXX** | **$XX,XXX** | |

**Pricing Note:** Include date + URL for every price. Never extrapolate savings.

### 7. Risk Assessment
| Risk | Likelihood | Impact | Mitigation |

Minimum 5 risks: technical, schedule, resource, compliance, adoption.

### 8. Success Criteria
- Measurable KPIs tied to business outcomes
- Technical acceptance criteria
- User adoption metrics

---

## Confidentiality Rules for SDD

1. Solution name: Use codename or generic name (e.g., "AI Document Intelligence Platform")
2. Customer: NEVER named — use "the organization" or "the customer"
3. Data: All examples use synthetic/mock data
4. File location: `clients/[CODE]/SOLUTION-DESIGN.md` (gitignored via clients/.gitignore)

---

## Generation Sequence

1. Read all Phase 1+2 outputs from conversation context
2. Generate architecture sections (4.1-4.5) — use system-architect reasoning
3. Generate pricing section (6) — verify at oracle.com/cloud/price-list/
4. Generate executive summary, business context, risk assessment
5. Run `/workflow 60-confidentiality-audit` for final audit

---

## Two-Tier Billing Reminder

When calculating OCI GenAI costs, remember two billing models exist:

| Model Family | Billing Unit | Rate |
|---|---|---|
| Cohere + Meta Llama | Characters (transactions) | Per 10K characters |
| Grok + Gemini + OpenAI-OSS | Tokens | Per 1M input / Per 1M output |

**NEVER mix these models in the same calculation.**

---

*Version: 1.0 | Ported from claude-code-oci-ai-architect-skills 2026-03-06*
