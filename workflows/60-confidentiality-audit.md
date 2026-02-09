# Workflow: Confidentiality Audit

> Pre-delivery audit to enforce codename protocol and prevent data leakage.
> This workflow has **VETO power** — if it fails, delivery is BLOCKED.

## When to Use
- Before delivering ANY document to a customer
- Before committing any client-folder content to git
- As the final step of `/50-solution-design.md`
- After generating images, prototypes, or presentations
- Anytime you're unsure if content is safe to share

## The Codename Protocol

### Non-Negotiable Rules

| # | Rule | Violation Example | Correct |
|---|------|-------------------|---------|
| 1 | Codenames are OPAQUE | "K is a healthcare company" | "Working on K today" |
| 2 | Never persist context | Industry name in README | Only codename + status in README |
| 3 | Conversation-only details | Scope details in SOLUTION-DESIGN.md | Generic descriptions in SDD |
| 4 | README is only committable file | Committing deliverables/ | deliverables/ in .gitignore |
| 5 | Research is topic-based | `research/projects/K/analysis.md` | `research/topics/healthcare-ai-patterns.md` |

### What CAN Be in Committed Files
- Codename letter only (A, B, E, K, O, P, R, V)
- Status: Active / Prospect / Completed
- Role: AI/Cloud Architecture
- Generic dates and milestones
- Generic technology descriptions (no client-specific configs)

### What MUST NEVER Be in Committed Files
- Real customer name or employees
- Industry vertical tied to codename
- Geographic region tied to client
- Employee count or revenue
- Contract value or pricing terms
- Customer technology stack specifics
- Internal Oracle pricing or discounts
- Competitor analysis tied to a specific deal

---

## Audit Steps

### Step 1: Content Scan

Search ALL output files for prohibited content:

```bash
# Replace [CUSTOMER_NAME] with the real name from conversation context
# Run each of these against the deliverables folder

# 1. Search for customer name (case-insensitive)
grep -ri "[CUSTOMER_NAME]" clients/[CODE]/

# 2. Search for industry terms that could identify the client
# Examples for different industries:
grep -ri "hospital\|clinic\|patient\|HIPAA" clients/[CODE]/
grep -ri "bank\|trading\|portfolio\|PCI-DSS" clients/[CODE]/
grep -ri "refinery\|pipeline\|drilling\|upstream" clients/[CODE]/
grep -ri "carrier\|subscriber\|5G\|spectrum" clients/[CODE]/

# 3. Search for geographic identifiers
grep -ri "headquartered\|based in\|offices in" clients/[CODE]/

# 4. Search for size indicators
grep -ri "employees\|revenue\|market cap\|Fortune" clients/[CODE]/

# 5. Search for internal Oracle pricing
grep -ri "discount\|internal price\|special pricing\|negotiated" clients/[CODE]/

# 6. Search for competitor attack language
grep -ri "worse than\|inferior\|failing\|losing to" clients/[CODE]/
```

**Pass criteria:** ALL searches return zero results.

**If any match found:**
1. Note the file path and line number
2. Replace with generic language:
   - Customer name → "the organization"
   - Industry specifics → generic descriptions
   - Geographic details → remove entirely
   - Size indicators → remove entirely

---

### Step 2: File Location Audit

Verify deliverables are in the correct (gitignored) locations:

```bash
# 1. Verify deliverables are in the right place
ls -la clients/[CODE]/deliverables/
# Expected: images/, prototype/, docs/ subdirectories

# 2. Verify SOLUTION-DESIGN.md is in client folder (gitignored)
ls -la clients/[CODE]/SOLUTION-DESIGN.md

# 3. Check that NO deliverables leaked to other folders
# These should return EMPTY:
find research/ -name "*[CODE]*" -type f 2>/dev/null
find projects/ -path "*/[CODE]/*" -type f 2>/dev/null

# 4. Check no codename appears in research filenames
ls research/topics/ | grep -i "[CODE]"
# This MUST return empty — research is topic-based, never codename-linked
```

**Pass criteria:**
- All deliverables under `clients/[CODE]/deliverables/`
- SOLUTION-DESIGN.md under `clients/[CODE]/`
- Zero codename references in `research/` or `projects/`

---

### Step 3: Git Safety Check

Ensure git will NOT commit sensitive files:

```bash
# 1. Check what .gitignore blocks for clients
cat clients/.gitignore
# Expected contents (minimum):
#   deliverables/
#   notes/
#   docs/
#   SOLUTION-DESIGN.md
#   auto-CLAUDE.md
#   *.private

# 2. Verify no deliverables are tracked by git
git status clients/[CODE]/
# ONLY README.md should appear as trackable
# deliverables/, SOLUTION-DESIGN.md should NOT appear

# 3. Check staged files for client content before any commit
git diff --cached --name-only | grep "clients/"
# Only README.md files should be in staged client files

# 4. Double-check: are any deliverables accidentally tracked?
git ls-files clients/[CODE]/
# Should return ONLY: clients/[CODE]/README.md (or empty)
```

**Pass criteria:**
- `.gitignore` blocks deliverables, notes, docs, SOLUTION-DESIGN.md
- `git status` shows no deliverables as trackable
- `git ls-files` shows only README.md per client

**If tracked files found:**
```bash
# Remove from git tracking (keeps local file)
git rm --cached clients/[CODE]/SOLUTION-DESIGN.md
git rm --cached -r clients/[CODE]/deliverables/
# Then commit the .gitignore fix
```

---

### Step 4: Image Compliance

For EVERY generated image in `deliverables/images/`:

| Check | Pass | Fail |
|-------|------|------|
| Oracle logos | No logos anywhere | Any Oracle logo visible |
| Customer logos | No customer branding | Any customer logo/brand |
| Service names | "Oracle AI Database 26ai" | "Oracle Database 23ai" or "23c" |
| Service names | "OCI GenAI" | "Oracle GenAI Service" (informal) |
| Brand colors | Oracle Red #C74634 for accents | Random colors |
| Text readability | All text readable at 1080p | Blurry or overlapping text |
| Spelling | Zero errors | Any misspelling |

**How to verify:**
1. Open each image in `deliverables/images/`
2. Visual inspection against the checklist above
3. Pay special attention to small text in architecture diagrams

**Common mistakes:**
- AI image generators sometimes hallucinate logos — always check
- "23ai" appears in training data more than "26ai" — verify every reference
- Customer names sometimes leak into diagram labels

---

### Step 5: Document Sanitization

Check every document in `clients/[CODE]/`:

```markdown
## Sanitization Checklist

### SOLUTION-DESIGN.md
- [ ] Section 1 (Executive Summary): No customer name, "the organization" used
- [ ] Section 2 (Business Context): Generic problem description, no identifying details
- [ ] Section 3 (Requirements): Functional requirements don't reveal customer identity
- [ ] Section 4 (Architecture): OCI service names correct, no customer infrastructure names
- [ ] Section 5 (Implementation): Timeline is generic, no internal meeting references
- [ ] Section 6 (BOM): Prices from public price list only, no internal discounts
- [ ] Section 7 (Risks): No customer-specific names or systems
- [ ] Section 8 (Success): KPIs are generic enough to not identify client

### DISCOVERY.md
- [ ] Business context uses generic language
- [ ] No customer employee names
- [ ] No internal meeting notes or email quotes
- [ ] Constraints are generic (e.g., "GDPR compliance" not "because they operate in Germany")

### Prototype (index.html)
- [ ] PROTOTYPE banner visible
- [ ] All data is synthetic/mock
- [ ] No customer logos or branding
- [ ] No hardcoded customer-specific values
- [ ] Page title is generic or codename-based

### README.md (the ONLY committable file)
- [ ] Contains ONLY: codename, status, role, start date
- [ ] NO industry, scope, geography, or technology details
```

**README.md must look EXACTLY like this:**
```markdown
# Project [CODE]

| Field | Value |
|-------|-------|
| Status | Active |
| Role | AI/Cloud Architecture |
| Started | [Month Year] |
```

Nothing else. No exceptions.

---

## Verdict

After completing all 5 steps, issue one of:

### PASS
All checks passed. Safe to deliver.
```
CONFIDENTIALITY AUDIT: PASS
Date: [YYYY-MM-DD]
Scope: clients/[CODE]/ (all deliverables)
Steps completed: 5/5
Issues found: 0
Verdict: CLEAR FOR DELIVERY
```

### FAIL
One or more violations found. Delivery BLOCKED.
```
CONFIDENTIALITY AUDIT: FAIL
Date: [YYYY-MM-DD]
Scope: clients/[CODE]/

VIOLATIONS:
1. [File:Line] — [Description of violation]
2. [File:Line] — [Description of violation]

REQUIRED ACTIONS:
1. [Specific fix for violation 1]
2. [Specific fix for violation 2]

Verdict: DELIVERY BLOCKED — fix violations and re-run audit
```

### WARN
Potential issues needing human judgment.
```
CONFIDENTIALITY AUDIT: WARN
Date: [YYYY-MM-DD]
Scope: clients/[CODE]/

WARNINGS:
1. [File:Line] — [Description of potential issue]

RECOMMENDATION: [Human review needed for items above]
Verdict: CONDITIONAL — human review required before delivery
```

---

## Emergency Protocol

If you discover a confidentiality breach AFTER delivery:
1. **STOP** all work immediately
2. **IDENTIFY** what was exposed and where
3. **NOTIFY** the user immediately with specifics
4. **DO NOT** attempt to fix silently — transparency is mandatory
5. **Document** in conversation (never in committed files) what happened

---

## Anti-Patterns

| Don't | Do Instead |
|-------|-----------| 
| Skip audit because "it looks fine" | Run ALL 5 steps every time |
| Audit only SOLUTION-DESIGN.md | Audit ALL files including images and prototype |
| Trust AI-generated content is clean | AI models hallucinate customer names from context |
| Fix violations silently | Report ALL findings before fixing |
| Commit deliverables "just this once" | NEVER commit deliverables — .gitignore exists for a reason |
| Link research to codenames | Research goes to `research/topics/` with generic names |
| Put industry in README | README has codename + status ONLY |
