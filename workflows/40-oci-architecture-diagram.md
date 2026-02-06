# Workflow: OCI Architecture Diagram Generation

## Purpose
Generate professional OCI architecture diagrams using text-based or visual tools.

## Steps

### 1. Define Architecture Scope
- What OCI services are involved?
- What are the data flows?
- What are the security boundaries?
- What is the network topology?

### 2. Choose Diagram Type

| Type | When to Use | Format |
|------|-------------|--------|
| ASCII/Text | Quick iteration, documentation | Markdown |
| draw.io/Excalidraw | Detailed technical | XML/JSON |
| AI-generated visual | Customer presentations | PNG/SVG |

### 3. Standard OCI Layers

```
┌─────────────────────────────────────────────────────────┐
│                    SECURITY / GOVERNANCE                  │
│  IAM │ Vault │ Cloud Guard │ Security Zones │ Audit     │
├─────────────────────────────────────────────────────────┤
│                    APPLICATION LAYER                      │
│  OKE │ Functions │ API Gateway │ Integration Cloud       │
├─────────────────────────────────────────────────────────┤
│                    AI / ML SERVICES                       │
│  GenAI │ AI Agents │ Data Science │ AI Blueprints        │
├─────────────────────────────────────────────────────────┤
│                    DATA LAYER                             │
│  AI Database 26ai │ Object Storage │ Streaming           │
├─────────────────────────────────────────────────────────┤
│                    INFRASTRUCTURE                         │
│  VCN │ Compute │ GPU │ Load Balancer │ FastConnect       │
├─────────────────────────────────────────────────────────┤
│                    OBSERVABILITY                          │
│  Monitoring │ Logging │ APM │ Notifications              │
└─────────────────────────────────────────────────────────┘
```

### 4. Enterprise RAG Reference Architecture

```
┌─────────────────── Security & Governance ──────────────────┐
│                                                             │
│  ┌──────────┐    ┌──────────────┐    ┌─────────────────┐   │
│  │Data      │───▶│ Processing   │───▶│ Embedding       │   │
│  │Sources   │    │ (Doc Under-  │    │ (Cohere Embed 4)│   │
│  │          │    │  standing)   │    │                 │   │
│  └──────────┘    └──────────────┘    └────────┬────────┘   │
│                                               │             │
│                                      ┌────────▼────────┐   │
│                                      │ Vector Store    │   │
│                                      │ (26ai AI Vector │   │
│                                      │  Search)        │   │
│                                      └────────┬────────┘   │
│                                               │             │
│  ┌──────────┐    ┌──────────────┐    ┌───────▼─────────┐   │
│  │  Query   │───▶│  Retrieval   │───▶│  Reranking      │   │
│  │          │    │  + Hybrid    │    │  (Rerank 3.5)   │   │
│  └──────────┘    └──────────────┘    └────────┬────────┘   │
│                                               │             │
│                  ┌──────────────┐    ┌───────▼─────────┐   │
│                  │   Response   │◀───│  Generation     │   │
│                  │              │    │  (Command A)    │   │
│                  └──────────────┘    └─────────────────┘   │
│                                                             │
└──────────────── Observability & Evaluation ─────────────────┘
```

### 5. Brand Guidelines for Visuals

| Element | Specification |
|---------|---------------|
| Primary color | Oracle Red #C74634 |
| AI/Agents | Purple #7B1FA2 |
| Security | Green #388E3C |
| Data | Amber #F59E0B |
| Background | Dark #1a1a2e or White #FFFFFF |
| **Logos** | **NEVER include Oracle logos — text labels only** |
| Font | Clean sans-serif (Arial, Inter, or similar) |

### 6. AI-Generated Visual Prompt Template

```
Create a professional enterprise architecture diagram showing [DESCRIPTION].

Layout: Three horizontal tiers
- Top tier: Security/Governance (green accents)
- Center: Core pipeline/services
- Bottom: Observability/Evaluation

Style: Clean isometric or flat technical
Color scheme: Dark background (#1a1a2e), Oracle Red (#C74634) accents
Text: All labels perfectly spelled, professional font
NO logos — use text labels for "Oracle Cloud", "OCI"
Resolution: 16:9 aspect ratio, presentation-ready
```

### 7. Validation Checklist

- [ ] All OCI service names are current (2026 naming)
- [ ] Data flows have clear direction arrows
- [ ] Security boundaries are visible
- [ ] No Oracle logos (text labels only)
- [ ] Readable at presentation scale
- [ ] Color-coded by function (compute, AI, data, security)
