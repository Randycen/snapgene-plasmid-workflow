# SnapGene Plasmid Builder — Portable Workflow

This package contains a reusable workflow for AI-assisted plasmid subcloning and SnapGene `.dna` generation.

## Files

- `SKILL.md` — complete agent/skill instructions
- `README.md` — quick usage guide
- `INPUT_TEMPLATE.md` — a minimal task template to paste into a new conversation

## Recommended use

At the start of a new task, upload:

1. backbone `.dna`
2. insert/template `.dna`
3. latest `primer_table.csv`
4. optionally any previous target `.dna`

Then provide a short goal such as:

> Build pJC2642. Replace the original T7-idgS-sfp cassette in pJC2641 with the Para-RBS1-idgS-sfp cassette from Para-RBS1-idgS.dna. Preserve EGFP homology regions and all CAST L/R elements. Use Gibson Assembly. Follow SKILL.md.

The workflow will prioritize existing primers in the uploaded primer table, preserve native SnapGene display metadata, and write manual construction history into Description when native History cannot be generated.
