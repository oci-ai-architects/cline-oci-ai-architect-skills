# Oracle AI Architect Standards — Windsurf

## Scope and Precedence
1. User request
2. `.windsurf/rules/oracle-standards.md`
3. Selected workflow (from `workflows/` folder)
4. Loaded skill guidance (from `skills/` folder)

## Core Contract
- For complex multi-phase work, follow `workflows/00-workflow-router.md`
- For solution design, execute `workflows/50-solution-design.md` end-to-end
- Always capture missing critical inputs before deep execution
- Apply quality gates and confidentiality checks before delivery

## Verification Policy (MANDATORY)
Always verify volatile claims — never use cached knowledge for:
- Pricing: `https://www.oracle.com/cloud/price-list/`
- Cost estimator: `https://www.oracle.com/cloud/costestimator.html`
- Model catalog: `https://docs.oracle.com/en-us/iaas/Content/generative-ai/pretrained-models.htm`
- OCI docs: `https://docs.oracle.com/en-us/iaas/`
- AI Blueprints: `https://github.com/oracle-quickstart/oci-ai-blueprints`

Never make blanket "X times cheaper" claims without a model-specific comparison.

## Naming Guardrails
- Use `Oracle AI Database 26ai` (not `23ai`)
- Use `OCI GenAI` / `OCI Generative AI`
- Validate model IDs from official OCI catalog at execution time

## OCI GenAI Two-Tier Billing (CRITICAL)
Two billing models exist — NEVER mix them:
- **Character-based:** Cohere + Meta Llama → 1 transaction = 1 character, per 10K
- **Token-based:** Grok + Gemini + OpenAI-OSS → per 1M input / per 1M output tokens

## Confidentiality Protocol
- Codenames are opaque identifiers (A, B, E, K, O, P, R, V)
- Never persist customer-identifying details in committed files
- Run `workflows/60-confidentiality-audit.md` before every customer delivery
- README.md per client: codename + status + role ONLY

## Execution Pattern for Non-Trivial Tasks
1. Classify intent → pick workflow chain
2. Load minimum required skills for that phase
3. Produce staged outputs with quality gates
4. Run cost verification and security review
5. Run confidentiality audit before delivery

## HTML Deliverable Pins (NEVER change in working files)
- Tailwind: `https://cdn.tailwindcss.com/3.4.17`
- Lucide: `https://unpkg.com/lucide@0.563.0/dist/umd/lucide.js`
- Mermaid: `https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js`

## Skills Available
Reference `skills/` folder for:
- `oracle-solution-design` — 5-phase end-to-end solution design
- `oracle-sdd-generator` — structured SDD document generation
- `oracle-confidentiality` — pre-delivery audit with veto power
- `oracle-adk` — Oracle Agent Development Kit patterns
- `oracle-agent-spec` — Open Agent Specification
- `oci-services-expert` — OCI service selection and pricing
- `oracle-ai-blueprints` — OCI AI Blueprints decision tree
- `rag-expert` — RAG pipeline design and implementation
- `oracle-infogenius` — architecture diagram generation
- `oracle-research` — research and validation

## Defensive Editing
- Read files before modifying
- Preserve working dependency versions unless explicitly asked
- No unsolicited restructuring
- Verify behavior after edits
