# Oracle Solution Design Workflow

> Orchestrates a complete OCI solution design through 5 phases with quality gates.

## Overview

This workflow guides the creation of a customer-facing solution design. It follows the DISCOVER > ARCHITECT > VISUALIZE > PROTOTYPE > DELIVER pipeline, with mandatory quality gates between phases.

## Prerequisites

- Customer context provided in conversation (codename only, never real names)
- Access to oracle.com/cloud/price-list/ for pricing verification
- Nano Banana or equivalent for architecture diagram generation

---

## Phase 1: DISCOVER

Research the problem space. Gather enough information to design the solution.

### Steps

1. **Identify core use cases** — Ask user for top 3 business requirements
2. **Research OCI services** — Check docs.oracle.com/iaas/ for relevant services
3. **Check existing patterns** — Search github.com/oracle-quickstart and OCI AI Blueprints
4. **Identify constraints** — Compliance, timeline, budget, data residency
5. **Industry context** — Research relevant industry AI patterns and benchmarks

### Quality Gate 1
Before moving to Phase 2, confirm:
- [ ] Top 3 use cases documented
- [ ] OCI services mapped to requirements
- [ ] Constraints captured (compliance, timeline, budget)
- [ ] Existing OCI patterns checked
- [ ] No real customer names in any output

---

## Phase 2: ARCHITECT

Design the technical architecture with OCI service selection.

### Steps

1. **Select OCI services** — Use the OCI GenAI Model Selection Matrix for AI workloads
2. **Design architecture tiers** — Basic / Advanced / Premium with clear differentiation
3. **Map data flow** — Ingestion > Processing > Storage > Retrieval > Presentation
4. **Security architecture** — IAM, encryption, network isolation, compliance mapping
5. **Verify pricing** — Every service price confirmed at oracle.com/cloud/price-list/
6. **Generate SDD** — Use the SDD template to create SOLUTION-DESIGN.md

### Mandatory Checks
- Does an OCI AI Blueprint already cover this? (Check github.com/oracle-quickstart/oci-ai-blueprints)
- Are pricing claims model-specific? (Never say "X times cheaper" without comparison)
- Are services available in the target OCI region?

### Quality Gate 2
- [ ] Every OCI service verified against official docs
- [ ] Pricing sourced from oracle.com/cloud/price-list/ with dates
- [ ] No blanket cost comparison claims
- [ ] Architecture follows OCI Well-Architected Framework

---

## Phase 3: VISUALIZE

Create logo-free architecture diagrams.

### Steps

1. **Generate architecture diagram** — Use text labels only, NO Oracle logos
2. **Apply Oracle brand colors** — Red #C74634 for accents, dark background
3. **Generate draft first** — Use Flash/cheap model for initial draft
4. **Refine** — Only use Pro/expensive model after draft approved
5. **Create visual set** — Master architecture, user journey, data flow, tier comparison

### Quality Gate 3
- [ ] ZERO Oracle logos in any image
- [ ] Service names match official branding (26ai not 23ai)
- [ ] No spelling errors in diagrams
- [ ] Architecture matches the SOLUTION-DESIGN.md

---

## Phase 4: PROTOTYPE

Build an interactive HTML prototype.

### Steps

1. **Single HTML file** — No build tools, embedded CSS/JS
2. **Simulate core workflow** — One main user flow with mock data
3. **Add processing states** — Loading animations, progress bars
4. **Label as PROTOTYPE** — Visible banner marking it as demo
5. **Oracle Red accents** — #C74634, dark theme
6. **No real customer data** — All mock/synthetic

### Quality Gate 4
- [ ] Core interaction works end-to-end
- [ ] Processing states visible
- [ ] PROTOTYPE banner displayed
- [ ] No real customer data

---

## Phase 5: DELIVER

Package everything for customer presentation.

### Steps

1. **Organize folder structure:**
   ```
   clients/[CODE]/
     SOLUTION-DESIGN.md
     deliverables/
       images/ (architecture.png, user-journey.png, data-flow.png)
       prototype/ (index.html)
       docs/ (DISCOVERY.md, BOM.md)
     README.md (codename only)
   ```
2. **Confidentiality audit** — Search all files for real customer names (must find zero)
3. **README check** — Contains ONLY: status, role, codename. No industry or scope details.
4. **Pricing verification** — All BOM items sourced with dates

### Quality Gate 5
- [ ] All files in correct structure
- [ ] README has codename only
- [ ] Images are logo-free
- [ ] Prototype loads standalone
- [ ] BOM pricing verified
- [ ] McKinsey-quality presentation? Yes/No

---

## Confidentiality Reminder

Throughout ALL phases:
- Use codenames only (A, B, E, K, O, P, R, V)
- Never persist client context to committed files
- Research goes to generic topics, not codename-linked folders
- Solution names are generic or codename-based
