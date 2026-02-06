# Cline OCI AI Architect Skills

> **Production-grade Cline skills, rules, and workflows for Oracle Cloud AI Architects.**
> Cross-platform compatible: Cline + Cursor + RooCode + Windsurf

**DISCLAIMER:** This is an unofficial community project. Not affiliated with, endorsed by, or sponsored by Oracle Corporation or Cline. Oracle, OCI, and related marks are trademarks of Oracle Corporation.

---

## Quick Start

### Option 1: Copy to your project

```bash
# Clone this repo
git clone https://github.com/oci-ai-architects/cline-oci-ai-architect-skills.git

# Copy to your project
cp -r cline-oci-ai-architect-skills/.clinerules/ your-project/
cp -r cline-oci-ai-architect-skills/workflows/ your-project/
cp -r cline-oci-ai-architect-skills/skills/ your-project/
```

### Option 2: Reference skills directly

In Cline, use `@skill-name` to load a skill:
```
@oci-services-expert Design a GenAI architecture for document processing
@rag-expert Build a RAG pipeline on OCI with Cohere embeddings
@oracle-adk Create a multi-agent system for customer support
```

---

## Architecture

This repo follows Cline best practices: **minimal rules, maximum workflows**.

```
cline-oci-ai-architect-skills/
├── .clinerules/                    # Behavioral rules (always active)
│   └── oracle-standards.md         # ONLY rule: OCI naming, confidentiality, verification
├── workflows/                      # Procedural workflows (on-demand, token-efficient)
│   ├── 01-oci-project-setup.md     # Initialize OCI AI project
│   ├── 10-oci-genai-integration.md # Add OCI GenAI service
│   ├── 15-oci-vector-search.md     # Implement AI Vector Search
│   ├── 20-oci-rag-pipeline.md      # Build RAG pipeline
│   ├── 25-oci-agent-deployment.md  # Deploy AI agents
│   ├── 30-oci-cost-estimation.md   # Estimate OCI costs
│   ├── 35-oci-security-review.md   # Security hardening
│   └── 40-oci-architecture-diagram.md # Generate architecture diagrams
├── skills/                         # Reference skills (loaded via @mention)
│   ├── oci-services-expert/        # OCI service selection
│   ├── oracle-adk/                 # Agent Development Kit
│   ├── oracle-agent-spec/          # Open Agent Specification
│   ├── oracle-infogenius/          # Visual generation
│   ├── rag-expert/                 # RAG on OCI
│   ├── mcp-architecture/           # MCP integration
│   ├── genai-dac-specialist/       # Dedicated AI Clusters
│   ├── frank-method/               # Enterprise methodology
│   ├── oracle-ai-blueprints/       # AI Blueprints & Accelerator Packs
│   └── oracle-research/            # Research & verification
├── .cursor/rules/                  # Cursor compatibility
├── .roo/rules/                     # RooCode compatibility
└── docs/                           # Documentation
```

### Why This Structure?

| Component | Token Cost | Purpose |
|-----------|-----------|---------|
| **Rules** (`.clinerules/`) | Loaded EVERY message | Minimal — only standards that must always apply |
| **Workflows** (`workflows/`) | Loaded on-demand | Procedural — step-by-step OCI tasks |
| **Skills** (`skills/`) | Loaded via @mention | Reference — deep knowledge per topic |

---

## Available Skills

| Skill | Use Case | Load With |
|-------|----------|-----------|
| **OCI Services Expert** | Architecture, service selection, cost optimization | `@oci-services-expert` |
| **Oracle ADK** | Agent Development Kit, multi-agent systems | `@oracle-adk` |
| **Oracle Agent Spec** | Portable agent definitions (YAML/JSON) | `@oracle-agent-spec` |
| **RAG Expert** | Retrieval-Augmented Generation on OCI | `@rag-expert` |
| **MCP Architecture** | Model Context Protocol + Oracle DB/Cloud | `@mcp-architecture` |
| **GenAI DAC Specialist** | Dedicated AI Clusters, fine-tuning | `@genai-dac-specialist` |
| **Oracle InfoGenius** | Architecture visual generation | `@oracle-infogenius` |
| **Frank Method** | Enterprise AI agent methodology | `@frank-method` |
| **AI Blueprints** | No-code GenAI on OKE, Accelerator Packs | `@oracle-ai-blueprints` |
| **Oracle Research** | Pricing verification, model catalog | `@oracle-research` |

## Available Workflows

| Workflow | Invoke With | Steps |
|----------|-------------|-------|
| **Project Setup** | `/workflow 01-oci-project-setup` | Compartment, VCN, OKE, IAM |
| **GenAI Integration** | `/workflow 10-oci-genai-integration` | Model selection, endpoint, SDK |
| **Vector Search** | `/workflow 15-oci-vector-search` | 26ai setup, embeddings, indexing |
| **RAG Pipeline** | `/workflow 20-oci-rag-pipeline` | End-to-end RAG with quality metrics |
| **Agent Deployment** | `/workflow 25-oci-agent-deployment` | ADK agent on OCI with governance |
| **Cost Estimation** | `/workflow 30-oci-cost-estimation` | Mandatory pricing verification |
| **Security Review** | `/workflow 35-oci-security-review` | IAM, encryption, Cloud Guard |
| **Architecture Diagram** | `/workflow 40-oci-architecture-diagram` | Text-based OCI diagram generation |

---

## Cross-Platform Compatibility

This repo serves four AI coding assistants:

| Platform | Config Location | How It Works |
|----------|----------------|--------------|
| **Cline** | `.clinerules/` + `workflows/` | Native support |
| **Cursor** | `.cursor/rules/` | Rules auto-loaded |
| **RooCode** | `.roo/rules/` | Rules + custom modes |
| **Windsurf** | Copy `.clinerules/` content | Manual setup |

---

## OCI Technical Standards (2026)

These standards are enforced by the rules file:

- **Database**: Oracle AI Database 26ai (not 23ai)
- **GenAI Models**: Cohere Command A, Llama 4, Grok 4.1, Gemini 2.5
- **Pricing**: Always verify at [oracle.com/cloud/price-list](https://www.oracle.com/cloud/price-list/)
- **AI Blueprints**: Check [oci-ai-blueprints](https://github.com/oracle-quickstart/oci-ai-blueprints) before custom builds
- **Visuals**: Text labels only, no Oracle logos (trademark policy)

---

## Related Projects

| Project | Platform | Repository |
|---------|----------|------------|
| Claude Code Skills | Claude Code | [claude-code-oci-ai-architect-skills](https://github.com/oci-ai-architects/claude-code-oci-ai-architect-skills) |
| **This Repo** | Cline/Cursor/RooCode/Windsurf | [cline-oci-ai-architect-skills](https://github.com/oci-ai-architects/cline-oci-ai-architect-skills) |

---

## Contributing

1. Fork this repository
2. Add or improve a skill/workflow
3. Ensure all Oracle service names are current (2026)
4. Submit a PR

See [CONTRIBUTING.md](docs/CONTRIBUTING.md) for details.

---

## License

MIT License - see [LICENSE](LICENSE)

---

_Community project by [oci-ai-architects](https://github.com/oci-ai-architects)_
