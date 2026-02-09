# Workflow: OCI Solution Design Document Generator

## Purpose
Generate a complete, structured Solution Design Document (SDD) from architecture decisions made in Phase 2 of `/50-solution-design.md`. Produces a customer-ready document with all 8 mandatory sections.

## When to Use
- After architecture decisions are made (Phase 2 complete)
- Customer engagement requires a formal SDD
- Need a structured document for internal review before customer delivery

## Prerequisites
- Discovery completed (`clients/[CODE]/deliverables/docs/DISCOVERY.md` exists)
- OCI services selected with rationale
- Architecture tiers defined (Basic / Advanced / Premium)
- Pricing verified at oracle.com/cloud/price-list/

---

## SDD Structure

Generate `clients/[CODE]/SOLUTION-DESIGN.md` with the following 8 sections.

### Section 1: Executive Summary

```markdown
# Solution Design Document
## Codename: [CODE]
## Date: [YYYY-MM-DD]
## Version: 1.0

---

## 1. Executive Summary

[3-5 sentences maximum. Business outcome focus, not technical details.]

**Recommended Tier:** [Basic / Advanced / Premium]

**Key Benefits:**
- [Benefit 1 — quantified if possible]
- [Benefit 2 — quantified if possible]
- [Benefit 3 — quantified if possible]

**Timeline:** [Estimated phases and duration]
```

**Rules:**
- Non-technical executive audience — no acronyms without explanation
- Focus on BUSINESS outcomes, not OCI service names
- Include tier recommendation with brief justification
- NEVER name the customer — use "the organization"

---

### Section 2: Business Context

```markdown
## 2. Business Context

### 2.1 Problem Statement
[What pain exists today? Be specific about measurable impact.]

### 2.2 Current State
| Aspect | Current | Limitation |
|--------|---------|------------|
| Data Processing | [Manual / Semi-automated] | [Speed, accuracy, cost] |
| AI/ML Capability | [None / Basic / Mature] | [What's missing] |
| Infrastructure | [On-premise / Cloud / Hybrid] | [Scalability, cost] |

### 2.3 Desired Future State
| Aspect | Target | Business Value |
|--------|--------|---------------|
| Data Processing | [Automated with AI] | [X% faster, $Y saved] |
| AI/ML Capability | [Production GenAI] | [New capabilities enabled] |
| Infrastructure | [OCI Cloud-native] | [Elastic scaling, reduced ops] |

### 2.4 Success Criteria
| Metric | Baseline | Target | Timeline |
|--------|----------|--------|----------|
| [Processing time] | [Current] | [Target] | [Months] |
| [Accuracy] | [Current %] | [Target %] | [Months] |
| [Cost per unit] | [$Current] | [$Target] | [Months] |
```

**Rules:**
- Never name the customer — "the organization" only
- Quantify pain where possible (time, cost, error rate)
- Link every desired state to measurable business value

---

### Section 3: Requirements Summary

```markdown
## 3. Requirements Summary

### 3.1 Functional Requirements
| ID | Requirement | Priority | OCI Service | Notes |
|----|------------|----------|-------------|-------|
| FR-01 | [Requirement description] | High | [OCI Service] | [Implementation note] |
| FR-02 | [Requirement description] | High | [OCI Service] | |
| FR-03 | [Requirement description] | Medium | [OCI Service] | |
| FR-04 | [Requirement description] | Medium | [OCI Service] | |
| FR-05 | [Requirement description] | Low | [OCI Service] | |

### 3.2 Non-Functional Requirements
| Category | Requirement | Target | OCI Solution |
|----------|------------|--------|-------------|
| **Performance** | Response time | < [X] seconds | [Service + config] |
| **Availability** | Uptime SLA | [99.5% / 99.9% / 99.95%] | [HA configuration] |
| **Scalability** | Concurrent users | [Number] | [Auto-scaling config] |
| **Security** | Data encryption | At-rest + in-transit | TDE + TLS 1.3 |
| **Compliance** | [GDPR/HIPAA/SOC2] | Full compliance | [OCI controls] |
| **Data Residency** | Region constraint | [EU/US/Any] | [OCI region] |
| **Recovery** | RPO / RTO | [Minutes/Hours] | [Backup strategy] |
```

**Rules:**
- Minimum 5 functional requirements with priorities
- Every NFR must map to a specific OCI solution
- Include RPO/RTO for production workloads

---

### Section 4: Solution Architecture

```markdown
## 4. Solution Architecture

### 4.1 Architecture Overview

[1-2 paragraph description of the overall architecture approach.]

> **Diagram Reference:** See `deliverables/images/architecture.png`

### 4.2 OCI Service Selection
| Service | Purpose | Why This Service | Alternative Considered | Docs |
|---------|---------|-----------------|----------------------|------|
| OCI GenAI | LLM inference | Managed, pay-per-token, multi-model | Self-hosted vLLM on OKE | [docs](https://docs.oracle.com/iaas/Content/generative-ai/) |
| Oracle AI Database 26ai | Vector + relational storage | HNSW index, Select AI Agent, SQL integration | Standalone vector DB | [docs](https://docs.oracle.com/en/database/oracle/oracle-database/26/) |
| OKE | Container orchestration | GPU scheduling, AI Blueprints compatible | VM-based deployment | [docs](https://docs.oracle.com/iaas/Content/ContEng/) |
| Object Storage | Data lake / document store | S3-compatible, tiered, cross-region | Block Storage | [docs](https://docs.oracle.com/iaas/Content/Object/) |
| OCI Vault | Key management | HSM-backed, FIPS 140-2, policy-based | Oracle-managed keys | [docs](https://docs.oracle.com/iaas/Content/KeyManagement/) |

### 4.3 Data Architecture

#### Data Flow
```
[Source Systems] → [OCI Object Storage] → [Processing Pipeline]
    → [Oracle AI Database 26ai (Vector + Relational)]
    → [OCI GenAI (Inference)] → [Application Layer]
```

#### Storage Strategy
| Data Type | Storage | Retention | Access Pattern |
|-----------|---------|-----------|---------------|
| Raw documents | Object Storage (Standard) | [Duration] | Write-once, read-many |
| Vector embeddings | AI Database 26ai (HNSW) | Persistent | High-frequency read |
| Structured metadata | AI Database 26ai (Relational) | Persistent | OLTP queries |
| Model artifacts | Object Storage (Infrequent) | Versioned | Deployment only |
| Logs / telemetry | Logging Analytics | 90 days | Diagnostic |

### 4.4 Integration Architecture

| Integration Point | Protocol | Authentication | Direction |
|-------------------|----------|---------------|-----------|
| [Source system] | REST API / JDBC | OAuth 2.0 / mTLS | Inbound |
| OCI GenAI | REST API | OCI IAM (Resource Principal) | Internal |
| AI Database 26ai | SQL*Net / JDBC | Wallet + mTLS | Internal |
| [Client application] | REST API / WebSocket | OIDC / JWT | Outbound |

### 4.5 Security Architecture

#### Identity & Access Management
- Compartment isolation per workload tier
- Dynamic groups for OCI service-to-service authentication
- Federated identity (SAML/OIDC) for user access
- Least-privilege IAM policies per service

#### Encryption
| Layer | Method | Key Management |
|-------|--------|---------------|
| Data at rest | TDE (Transparent Data Encryption) | OCI Vault (customer-managed) |
| Data in transit | TLS 1.3 | Auto-rotated certificates |
| Database columns | Column-level encryption | Application-managed |
| Object Storage | Server-side encryption | OCI Vault or Oracle-managed |

#### Network Security
- Private subnets for ALL AI workloads (no public IPs)
- Service Gateway for OCI service access without internet
- Network Security Groups (NSGs) per service tier
- WAF for any public-facing endpoints
- VCN Flow Logs enabled for audit

#### Compliance Mapping
| Requirement | OCI Control | Evidence |
|------------|-------------|---------|
| Data encryption at rest | TDE + OCI Vault | Audit logs, key rotation records |
| Access control | IAM policies + MFA | IAM audit trail |
| Network isolation | Private subnets + NSG | VCN flow logs |
| Data residency | Region selection | Tenancy configuration |
| Audit trail | OCI Audit + Logging | 365-day retention |
```

**Rules:**
- Every service must link to official docs.oracle.com
- Security section is MANDATORY — never skip it
- Use "Oracle AI Database 26ai" (not 23ai, not "Autonomous Database 26ai")

---

### Section 5: Implementation Approach

```markdown
## 5. Implementation Approach

### 5.1 Architecture Tiers
| Aspect | Basic | Advanced | Premium |
|--------|-------|----------|---------| 
| **Target Users** | < 50 | 50-500 | 500+ |
| **Data Volume** | < 100 GB | 100 GB - 1 TB | 1+ TB |
| **AI Models** | OCI GenAI (on-demand) | OCI GenAI (dedicated endpoint) | DAC + fine-tuned models |
| **Availability** | 99.5% | 99.9% | 99.95% |
| **Vector Search** | Basic HNSW | Hybrid (vector + keyword) | GraphRAG + reranking |
| **Agents** | Single agent | Multi-agent with tool-calling | Autonomous orchestration |
| **Monitoring** | Basic OCI metrics | Custom dashboards | Full observability stack |
| **Support** | Standard | Business Critical | Enterprise |
| **Monthly Est.** | $X,XXX | $XX,XXX | $XXX,XXX |

### 5.2 Phased Roadmap

#### Phase 1: Foundation (Weeks 1-4)
- [ ] OCI tenancy setup + compartment structure
- [ ] Network architecture (VCN, subnets, gateways)
- [ ] IAM policies + identity federation
- [ ] Oracle AI Database 26ai provisioning
- **Deliverable:** Infrastructure ready for development

#### Phase 2: Core AI Pipeline (Weeks 5-8)
- [ ] Data ingestion pipeline (Object Storage → Processing)
- [ ] Vector embedding generation (Cohere Embed 4)
- [ ] RAG pipeline: retrieval + generation
- [ ] API layer for application integration
- **Deliverable:** Working AI pipeline with test data

#### Phase 3: Advanced Features (Weeks 9-12)
- [ ] Agent workflows (if applicable)
- [ ] Fine-tuning (if applicable)
- [ ] Advanced search (hybrid, reranking)
- [ ] User interface / application integration
- **Deliverable:** Feature-complete system

#### Phase 4: Production Readiness (Weeks 13-16)
- [ ] Performance testing + optimization
- [ ] Security hardening + penetration testing
- [ ] Monitoring + alerting setup
- [ ] Documentation + runbooks
- [ ] User acceptance testing
- **Deliverable:** Production-ready system

### 5.3 Dependencies & Assumptions
| # | Dependency/Assumption | Impact if Wrong | Mitigation |
|---|----------------------|-----------------|------------|
| 1 | [OCI region availability] | [Service unavailable] | [Multi-region fallback] |
| 2 | [Data access from source] | [Pipeline blocked] | [Mock data for dev] |
| 3 | [User adoption rate] | [ROI delayed] | [Change management plan] |
```

---

### Section 6: Cost Estimation (BOM)

```markdown
## 6. Cost Estimation

**Prices verified on: [DATE] at oracle.com/cloud/price-list/**

### 6.1 Basic Tier — Monthly Breakdown
| Service | Shape/Config | Qty | Monthly ($) | Annual ($) | Source |
|---------|-------------|-----|-------------|------------|--------|
| OCI GenAI | [Model] on-demand | [tokens/mo] | $[VERIFIED] | $[VERIFIED] | price-list |
| AI Database 26ai | [ECPU] + [storage] | 1 | $[VERIFIED] | $[VERIFIED] | price-list |
| Compute | [Shape] | [count] | $[VERIFIED] | $[VERIFIED] | price-list |
| Object Storage | [GB] Standard | 1 | $[VERIFIED] | $[VERIFIED] | price-list |
| Networking | VCN + LB | 1 | $[VERIFIED] | $[VERIFIED] | price-list |
| **TOTAL** | | | **$X,XXX** | **$XX,XXX** | |

### 6.2 Advanced Tier — Monthly Breakdown
[Same table format with Advanced configuration]

### 6.3 Premium Tier — Monthly Breakdown
[Same table format with Premium configuration]

### 6.4 Cost Notes
- **Region:** [Target OCI region]
- **Commitment:** [PAYG / Annual Flex / Universal Credits]
- **BYOL:** [Applicable if customer has existing Oracle licenses]
- **Assumptions:** [List all pricing assumptions]
- **Optimization:** [Specific cost reduction recommendations]
```

**Rules:**
- EVERY price MUST come from oracle.com/cloud/price-list/ with verification date
- NEVER use memorized/cached prices
- Include all 3 tiers
- Note commitment type (PAYG vs Annual Flex — significant discount difference)

---

### Section 7: Risk Assessment

```markdown
## 7. Risk Assessment

| # | Risk | Category | Likelihood | Impact | Mitigation |
|---|------|----------|-----------|--------|------------|
| 1 | Model output quality below expectations | Technical | Medium | High | POC with representative data; fine-tuning option |
| 2 | Data migration complexity underestimated | Technical | Medium | Medium | Phased migration; parallel run period |
| 3 | Timeline pressure from business stakeholders | Schedule | High | Medium | Agile delivery; early demo at Phase 2 |
| 4 | Key resource availability | Resource | Medium | High | Cross-training; OCI partner support |
| 5 | Regulatory requirements change mid-project | Compliance | Low | High | Modular architecture; region-portable design |
| 6 | User adoption resistance | Adoption | Medium | Medium | Change management; champion program |
| 7 | OCI service limits hit at scale | Technical | Low | Medium | Limit increase requests; capacity planning |
```

**Rules:**
- Minimum 5 risks covering: technical, schedule, resource, compliance, adoption
- Every risk MUST have a specific mitigation (not "monitor and manage")
- Likelihood and Impact: Low / Medium / High

---

### Section 8: Success Criteria

```markdown
## 8. Success Criteria

### 8.1 Business KPIs
| KPI | Baseline | Target | Measurement | Timeline |
|-----|----------|--------|-------------|----------|
| [Processing time per unit] | [Current] | [Target] | [How measured] | [When] |
| [Error rate] | [Current %] | [Target %] | [How measured] | [When] |
| [Cost per transaction] | [$Current] | [$Target] | [How measured] | [When] |
| [User satisfaction] | [Current] | [Target] | [Survey/NPS] | [When] |

### 8.2 Technical Acceptance Criteria
- [ ] API response time < [X] ms at P95
- [ ] System availability > [99.X%] over 30-day window
- [ ] AI model accuracy > [X%] on test dataset
- [ ] All security controls passing audit
- [ ] Disaster recovery tested successfully (RPO/RTO met)

### 8.3 Go-Live Readiness Checklist
- [ ] All Phase 4 deliverables complete
- [ ] Performance testing passed
- [ ] Security review passed
- [ ] User acceptance testing signed off
- [ ] Monitoring and alerting operational
- [ ] Runbooks and documentation complete
- [ ] Support team trained
```

---

## Confidentiality Rules (Applied to Every Section)

| Rule | Enforcement |
|------|------------|
| Customer never named | Use "the organization" throughout |
| Solution name is generic | Codename-based or descriptive |
| All data examples are synthetic | Never use real customer data |
| No internal meeting notes | No email quotes or internal references |
| File location | `clients/[CODE]/SOLUTION-DESIGN.md` (gitignored) |

## Quality Checklist

Before marking the SDD as complete:
- [ ] All 8 sections present and populated
- [ ] No placeholder text remaining (no `[TODO]` or `[TBD]`)
- [ ] Every OCI service linked to official documentation
- [ ] Every price verified with date and source
- [ ] Customer never named — "the organization" only
- [ ] Architecture diagram referenced and exists
- [ ] Minimum 5 risks with specific mitigations
- [ ] Success criteria are measurable (baseline + target)
- [ ] Would McKinsey present this to a Fortune 500 CEO? Yes/No
