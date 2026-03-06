---
name: oci-drawio-icon-native
description: Build and validate OCI draw.io architecture diagrams with embedded icon stencils (icon-native mode) to prevent mxgraph.oci fallback rendering issues like red placeholder blocks. Use for any OCI diagram creation or refactor in draw.io.
---

# OCI Draw.io Icon-Native Workflow

## Why this skill exists
Some environments do not load the OCI draw.io library automatically. If a page uses `shape=mxgraph.oci.*`, draw.io can render generic fallback blocks instead of OCI icons.

## Non-negotiable rule
- Do **not** ship OCI pages that rely on `mxgraph.oci.*`.
- Use embedded icon stencils cloned from a known-good local source diagram.
- Validate every generated page before handoff.

## Required build and validation flow
1. Build the page with the icon-native generator:
```bash
python3 drawio/tools/build_agentic_rag_diagram.py \
  --source drawio/KPN_RAG_Arch_ALL.drawio \
  --output drawio/KPN_RAG_Arch_ALL__with_Agentic_Excellence.drawio \
  --standalone drawio/OCI_Agentic_RAG_Excellence_ICON_NATIVE_V2.drawio \
  --diagram-name Agentic-RAG-Excellence-IconNative-v2
```

2. Validate icon integrity on the generated page:
```bash
python3 drawio/tools/validate_drawio_icon_integrity.py \
  --file drawio/KPN_RAG_Arch_ALL__with_Agentic_Excellence.drawio \
  --diagram Agentic-RAG-Excellence-IconNative-v2
```
This validator must report:
- `mxgraph_oci_refs=0`
- `max_icon_group_spread_ratio` close to `1.000` (hard fail if over threshold)

3. (Optional but recommended) Generate a preview URL for visual QA:
```bash
python3 drawio/tools/build_drawio_preview_url.py \
  --file drawio/KPN_RAG_Arch_ALL__with_Agentic_Excellence.drawio \
  --diagram Agentic-RAG-Excellence-IconNative-v2
```

## Acceptance criteria
- `validate_drawio_icon_integrity.py` returns `VALIDATION: PASS`.
- The page contains no `mxgraph.oci.*` style references.
- The page has high stencil density and icon-group density.
- The page has no exploded icon groups (`max_icon_group_spread_ratio` within limit).
- Architecture layout quality is production-grade: clear zones, readable flow, explicit labels, and edge discipline.

## Troubleshooting
- If validation fails on `mxgraph.oci.*`: replace those nodes with cloned stencil groups from the source page.
- If icon-group density is low: clone additional proven symbols from `Page-10` of `drawio/KPN_RAG_Arch_ALL.drawio`.
- If icons appear scattered/tiny across the canvas: fix clone translation logic so only group roots translate; descendants remain relative.
- If layout quality degrades: regenerate with script defaults first, then adjust positioning while preserving cloned symbol groups.
