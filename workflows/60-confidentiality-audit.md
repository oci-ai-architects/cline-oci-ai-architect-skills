# Confidentiality Audit Workflow

> Pre-delivery audit to enforce codename protocol and prevent data leakage.
> This workflow has VETO power — if it fails, delivery is blocked.

## Overview

Run this workflow before delivering ANY document to a customer, before committing client-folder content, or as the final step of /50-solution-design.md.

## The Codename Protocol

### Non-Negotiable Rules
1. Codenames are OPAQUE — A, B, E, K, O, P, R, V have no inherent meaning
2. Never persist context — Industry, scope, employee count, revenue NEVER in committed files
3. Conversation-only context — Client details stay in conversation memory only
4. README.md is the only committable file per client — status, role, codename only
5. Research goes to research/topics/ — NEVER to research/projects/[CODE]/

### What CAN Be in Committed Files
- Codename letter only
- Status (Active, Prospect, Completed)
- Role (AI/Cloud Architecture)
- Generic dates and milestones

### What MUST NEVER Be in Committed Files
- Real customer name
- Industry vertical
- Geographic region tied to client
- Employee count or revenue
- Contract value or pricing
- Customer technology stack specifics
- Names of customer employees
- Internal Oracle pricing or discounts

---

## Audit Steps

### Step 1: Content Scan
Search all output files for:
- Real customer names (from conversation context)
- Industry-specific terms that could identify the client
- Geographic identifiers tied to the client
- Internal Oracle pricing not on public price list
- Competitor names used in attack mode

### Step 2: File Location Check
Verify:
- Deliverables are in clients/[CODE]/deliverables/ (gitignored)
- SOLUTION-DESIGN.md is in clients/[CODE]/ (gitignored)
- No deliverables leaked to research/ or projects/ folders
- No codename appears in research/topics/ filenames

### Step 3: Git Safety Check
Before any commit:
- Check git diff for client content in staged files
- Verify clients/.gitignore blocks deliverables, notes, docs, SOLUTION-DESIGN.md
- Confirm no client deliverables are tracked

### Step 4: Image Compliance
For every generated image:
- No Oracle logos (text labels only)
- No customer logos or branding
- Service names match official Oracle branding

### Step 5: Document Sanitization
In every document:
- Customer = "the organization" or "the customer"
- Solution name is generic or codename-based
- All data examples use synthetic/mock data
- No internal meeting notes or email quotes

---

## Verdict

After all steps:
- **PASS**: All checks passed, safe to deliver
- **FAIL**: List specific violations with file paths and line numbers
- **WARN**: Potential issues needing human review

If FAIL: Fix violations and re-run this workflow. Do NOT deliver.
