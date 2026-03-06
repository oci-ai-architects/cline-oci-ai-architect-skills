---
name: oracle-confidentiality
description: Enforce confidentiality protocol across all Oracle AI Architect deliverables. Pre-delivery audit, codename enforcement, and content sanitization. This skill has VETO power — if it fails, delivery is blocked.
version: 1.0.0
platform: [cline, cursor, roocode, windsurf]
keywords: [confidentiality, audit, codename, security, compliance, delivery]
triggers:
  - "confidentiality audit"
  - "pre-delivery check"
  - "sanitize output"
---

# Oracle Confidentiality Guardian

> **Authority:** This skill has VETO power. If it fails, delivery is blocked.
> **Scope:** Pre-delivery audit, codename enforcement, content sanitization.

## When to Use

Invoke:
- Before delivering ANY document to a customer
- Before committing client-folder content to git
- As the final step of solution design (Phase 5)

**Trigger:** `/workflow 60-confidentiality-audit`

---

## The Codename Protocol

### Rules (Non-Negotiable)

1. **Codenames are OPAQUE** — A, B, E, K, O, P, R, V have no inherent meaning
2. **Never persist context** — Industry, scope, employee count, revenue NEVER in committed files
3. **Conversation-only context** — Client details stay in session memory only
4. **README.md is the only committable file** per client — contains ONLY: status, role, codename
5. **clients/.gitignore blocks:** deliverables/, notes/, docs/, SOLUTION-DESIGN.md

### What CAN Be in Committed Files
- Codename letter only (A, B, K, etc.)
- Status (Active, Prospect, Completed)
- Role (AI/Cloud Architecture)

### What MUST NEVER Be in Committed Files
- Real customer name
- Industry vertical
- Geographic region or country
- Employee count or revenue
- Contract value or pricing
- Customer technology stack details
- Names of customer employees
- Internal Oracle pricing or discounts

---

## Pre-Delivery Audit Checklist

### Step 1: Content Scan
Search all output files for:
- Real customer names (from conversation context)
- Industry-specific terms that could identify the client
- Geographic identifiers tied to the client
- Internal Oracle pricing not on public price list

### Step 2: File Location Check
- Deliverables are in `clients/[CODE]/deliverables/` (gitignored)
- SOLUTION-DESIGN.md is in `clients/[CODE]/` (gitignored)
- No deliverables leaked to `research/` or `projects/`
- No codename appears in `research/topics/` filenames

### Step 3: Git Safety Check
```bash
# Check nothing sensitive is staged
git diff --cached

# Verify clients/ content is untracked
git status

# Verify gitignore is intact
cat clients/.gitignore
```

### Step 4: Image Compliance
- No Oracle logos (text labels only)
- No customer logos
- No identifiable customer branding or colors
- Service names match official Oracle branding

### Step 5: Document Sanitization
- Customer referred to as "the organization" or "the customer"
- Solution name is generic or codename-based
- All data examples use synthetic/mock data
- No internal meeting notes or email quotes

---

## Automated Checks

When invoked, execute:

1. **Grep for known risks:**
   - Real names mentioned in conversation
   - Currency amounts (could indicate contract values)
   - Specific addresses or locations

2. **Verify .gitignore integrity:**
   - Read clients/.gitignore
   - Confirm it blocks: deliverables/, notes/, docs/, SOLUTION-DESIGN.md

3. **Check git status:**
   - Ensure no client deliverables are staged or tracked

4. **Report:**
   - **PASS:** All checks passed, safe to deliver
   - **FAIL:** List specific violations with line numbers and file paths
   - **WARN:** Potential issues that need human review

---

## Emergency Protocol

If confidential data is accidentally committed:

1. DO NOT push
2. Soft reset: `git reset HEAD~1` (undo last commit, keep files)
3. Remove sensitive content from files
4. Re-commit with clean content
5. If already pushed: Contact user immediately, may need force push (with explicit approval)

---

## README Template (Only Committable File)

```markdown
# Project [CODE]

| Field | Value |
|-------|-------|
| Status | Active |
| Role | AI/Cloud Architecture |
| Started | [Month Year] |
```

Nothing else. No industry, no scope, no customer details.

---

*Version: 1.0 | Ported from claude-code-oci-ai-architect-skills 2026-03-06*
