# Changelog

## v1.0 — 2026-09-07

Initial plasmid construction workflow.

- Added SnapGene plasmid construction rules.
- primer_table.csv is the authoritative primer registry.
- Existing primers should be reused whenever possible.
- Colony PCR primers should preferentially reuse primers near Gibson/Golden Gate junctions.
- New primers receive JC-series names only when no suitable existing primer is available.
- Legacy primer names from SnapGene templates are recorded in Description for traceability.
- SnapGene output should preserve circular dsDNA topology, feature colors, primer display, and metadata.
- Native SnapGene History is not fabricated; manual construction history is stored in Description.
- Construction logs follow the laboratory minimal TXT format.