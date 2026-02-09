# OCI Solution Design Document Generator

> Generate a structured SDD from architecture decisions. Uses the standard template.

## Overview

This workflow produces a complete Solution Design Document for an OCI-based customer solution. Run after architecture decisions are made (Phase 2 of /50-solution-design.md).

## SDD Template Structure

Generate the following sections in `clients/[CODE]/SOLUTION-DESIGN.md`:

### Section 1: Executive Summary
- 3-5 sentences, business outcome focus
- Non-technical executive audience
- Include tier recommendation (Basic/Advanced/Premium)

### Section 2: Business Context
- Problem statement (what pain exists today)
- Current state (tools/processes in use)
- Desired future state (what success looks like)
- NEVER name the customer — use "the organization"

### Section 3: Requirements Summary
- Functional requirements table: Requirement | Priority | OCI Service | Notes
- Non-functional requirements: Performance, Security, Compliance, Availability, Scalability

### Section 4: Solution Architecture
- 4.1 Architecture overview with diagram reference
- 4.2 OCI service selection table: Service | Purpose | Why This Service | Docs Reference
- 4.3 Data architecture (storage, vector search, data flow)
- 4.4 Integration architecture (APIs, protocols, OCI services)
- 4.5 Security architecture (IAM, encryption, network, compliance)

### Section 5: Implementation Approach
- 5.1 Tier definitions table: Basic | Advanced | Premium
- 5.2 Phased roadmap (Foundation > Core > Advanced > Production)
- 5.3 Dependencies and assumptions

### Section 6: Cost Estimation (BOM)
- Per-tier breakdown: Service | Shape/Config | Qty | Monthly | Annual | Source
- ALL prices from oracle.com/cloud/price-list/ with verification date
- Never extrapolate one benchmark to all scenarios

### Section 7: Risk Assessment
- Minimum 5 risks: Risk | Likelihood | Impact | Mitigation
- Cover: technical, schedule, resource, compliance, adoption

### Section 8: Success Criteria
- Business KPIs with baseline and target
- Technical acceptance criteria
- User adoption metrics

## Confidentiality Rules
- Customer: NEVER named, use "the organization"
- Solution name: Generic or codename-based
- All data examples: Synthetic/mock only
- File location: clients/[CODE]/SOLUTION-DESIGN.md
