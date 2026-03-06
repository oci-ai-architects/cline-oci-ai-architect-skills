---
name: oracle-solution-design
description: Orchestrate a full 5-phase OCI solution design — from discovery to customer-ready deliverables. Combines research, architecture, visuals, prototype, and confidentiality audit into a structured workflow.
version: 1.1.0
platform: [cline, cursor, roocode, windsurf]
keywords: [solution-design, oci, architecture, sdd, customer, deliverable]
triggers:
  - "solution design"
  - "SDD"
  - "architecture for [client]"
  - "customer deliverable"
---

# Oracle Solution Design Orchestrator

> **Method:** 5-phase workflow with parallel research + quality gates at each phase
> **Confidentiality:** Conversation-only context model. Zero persistent state with client details.

## When to Use

Invoke when you need:
- A complete solution design for a customer engagement
- A Solution Design Document (SDD) for an OCI-based solution
- An architecture recommendation with BOM and visuals
- A prototype or interactive demo for a customer presentation

**Trigger:** `/workflow 50-solution-design` or when user says "solution design", "SDD", "architecture for [codename]"

---

## The 5-Phase Workflow

```
  DISCOVER --> ARCHITECT --> VISUALIZE --> PROTOTYPE --> DELIVER
     |             |             |              |            |
  [Gate 1]     [Gate 2]     [Gate 3]       [Gate 4]    [Gate 5]
```

### Phase Summary

| Phase | Mode | Key Output |
|-------|------|-----------|
| DISCOVER | Research + requirements | DISCOVERY.md |
| ARCHITECT | Service selection + BOM + security | SOLUTION-DESIGN.md sections 4-6 |
| VISUALIZE | Architecture diagrams | 4 images in deliverables/images/ |
| PROTOTYPE | Interactive HTML demo | deliverables/html/index.html |
| DELIVER | Package + confidentiality audit | Complete client folder |

---

## Phase 1: DISCOVER

### Checklist Before Starting
1. READ client README.md for existing context
2. CHECK OCI AI Blueprints decision tree (see oracle-standards.md)
3. VERIFY data residency and compliance constraints

### Discovery Document
Create `clients/[CODE]/deliverables/docs/DISCOVERY.md`:

```markdown
# Discovery: [Solution Name]
## Codename: [CODE] | Date: [YYYY-MM-DD]

## Business Context
- Problem: [What pain exists today?]
- Current State: [What tools/processes in use?]
- Success Criteria: [What does good look like?]

## Top Use Cases
| # | Use Case | Priority | Data Sources | Expected Output |
|---|----------|----------|-------------|-----------------|

## Constraints
| Constraint | Value | Impact |
|-----------|-------|--------|
| Compliance | [GDPR/HIPAA/SOC2/None] | |
| Data Residency | [EU/US/Any] | |
| Timeline | [Weeks] | |
| Budget | [Range or TBD] | |
```

### Quality Gate 1
- [ ] Top 3 use cases documented with priorities
- [ ] OCI services mapped to each use case
- [ ] Constraints captured (compliance, residency, timeline, budget)
- [ ] OCI AI Blueprints decision tree consulted
- [ ] GenAI model selection justified if applicable
- [ ] No real customer names in ANY file

---

## Phase 2: ARCHITECT

### Service Selection Table
| Service | Purpose | Why | Alternative | Docs |
|---------|---------|-----|-------------|------|

### Architecture Tiers (MANDATORY)
| Aspect | Basic | Advanced | Premium |
|--------|-------|----------|---------|
| Users | < 50 | 50-500 | 500+ |
| AI Models | OCI GenAI shared | OCI GenAI dedicated | DAC + fine-tuned |
| Monthly Est. | $X,XXX | $XX,XXX | $XXX,XXX |

### Pricing (MANDATORY)
- Verify ALL prices at: https://www.oracle.com/cloud/price-list/
- Include verification date in BOM

### Security Architecture
- IAM: Compartment isolation, dynamic groups, federated identity
- Encryption: TDE + OCI Vault, TLS 1.3
- Network: Private subnets, Service Gateway, NSGs, WAF

### Quality Gate 2
- [ ] Every OCI service verified against docs.oracle.com
- [ ] Pricing verified at oracle.com/cloud/price-list/ with date
- [ ] No blanket "X times cheaper" claims
- [ ] AI Blueprints decision tree consulted
- [ ] Security architecture documented
- [ ] 3 tiers defined (Basic / Advanced / Premium)

---

## Phase 3: VISUALIZE

### Required Visual Set
| # | Image | Audience | File |
|---|-------|----------|------|
| 1 | Master Architecture | Technical + Executive | architecture.png |
| 2 | User Journey | Executive | user-journey.png |
| 3 | Data Flow | Technical | data-flow.png |
| 4 | Tier Comparison | Commercial | tier-comparison.png |

### Visual Rules (Non-Negotiable)
- **NO Oracle logos** — text labels only ("Oracle Cloud", "OCI GenAI")
- Oracle Red (#C74634) for accents
- Dark background (#1A1A2E or similar)
- 2026 branding: "Oracle AI Database 26ai" not "23ai"
- Minimum 1920x1080 resolution

### Quality Gate 3
- [ ] ZERO Oracle logos (visual inspection)
- [ ] Service names correct (26ai, not 23ai)
- [ ] Readable at presentation size
- [ ] Architecture matches SOLUTION-DESIGN.md
- [ ] 4 images generated

---

## Phase 4: PROTOTYPE

### HTML Deliverable Standard
Single HTML file. All CDN dependencies PINNED to specific versions:

```html
<!-- PINNED VERSIONS — NEVER use @latest for deliverables -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<script src="https://unpkg.com/lucide@0.563.0/dist/umd/lucide.js"></script>
<script src="https://cdn.tailwindcss.com/3.4.17"></script>
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
```

Tailwind config colors:
```js
tailwind.config = {
  theme: {
    extend: {
      colors: {
        oracle: { red: '#C74634', 'red-dark': '#A33B2C' }
      }
    }
  }
}
```

Always include: `<div class="proto-banner">PROTOTYPE — For Demonstration Purposes Only</div>`

### Quality Gate 4
- [ ] Core interaction works end-to-end
- [ ] No broken buttons or dead links
- [ ] PROTOTYPE banner displayed
- [ ] No real customer data
- [ ] Opens standalone in browser
- [ ] ALL CDN URLs pinned (zero @latest)
- [ ] Tailwind v3.4.17, Lucide v0.563.0, Mermaid v10

---

## Phase 5: DELIVER

### Folder Structure
```
clients/[CODE]/
├── SOLUTION-DESIGN.md                # Full SDD (gitignored)
├── README.md                         # Codename + status ONLY (committed)
└── deliverables/                     # (gitignored)
    ├── images/
    │   ├── architecture.png
    │   ├── user-journey.png
    │   ├── data-flow.png
    │   └── tier-comparison.png
    ├── html/
    │   ├── index.html
    │   └── uc/
    └── docs/
        ├── DISCOVERY.md
        └── BOM.md
```

### Quality Gate 5
- [ ] All files in correct folder structure
- [ ] README has codename only
- [ ] Images are logo-free
- [ ] Prototype opens standalone
- [ ] BOM prices verified with dates
- [ ] Confidentiality audit PASSED (`/workflow 60-confidentiality-audit`)

---

## Anti-Patterns

| Don't | Do Instead |
|-------|-----------|
| Skip discovery | Validate requirements first |
| Use memorized pricing | Verify at oracle.com/cloud/price-list/ |
| Include Oracle logos in images | Text labels, Oracle Red accents |
| Use @latest for CDN | Pin: Tailwind 3.4.17, Lucide 0.563.0 |
| Deliver without confidentiality audit | Run /60-confidentiality-audit first |

---

*Version: 1.1 | Ported from claude-code-oci-ai-architect-skills 2026-03-06*
