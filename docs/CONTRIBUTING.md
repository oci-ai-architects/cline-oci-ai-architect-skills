# Contributing to Cline OCI AI Architect Skills

## How to Contribute

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/new-skill`
3. Add or improve a skill, workflow, or rule
4. Ensure all Oracle service names are current (2026)
5. Test in Cline to verify functionality
6. Submit a PR with description of changes

## File Naming

- **Rules:** `.clinerules/kebab-case-name.md`
- **Workflows:** `workflows/NN-kebab-case-name.md` (numbered by category)
- **Skills:** `skills/kebab-case-name/README.md`

## Workflow Numbering

| Range | Category |
|-------|----------|
| 01-09 | Project setup |
| 10-19 | Core development |
| 20-29 | Architecture & design |
| 25-29 | Deployment |
| 30-39 | Cost & security |
| 40-49 | Documentation & visuals |

## Quality Standards

- All Oracle service names must be current (2026 naming)
- Pricing claims must include verification source
- Code examples must be functional (not pseudocode)
- No Oracle logos in any visual assets
- Customer codenames must be opaque (no industry mappings)

## Cross-Platform

When adding rules, update all platform directories:
- `.clinerules/` (Cline)
- `.cursor/rules/` (Cursor)
- `.roo/rules/` (RooCode)
