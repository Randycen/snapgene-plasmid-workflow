# Changelog

## v1.1 — 2026-09-07

- Final assembled SnapGene plasmids now display a primer as a continuous full-length match whenever the entire primer sequence exists continuously in the final product.
- Gibson/Golden Gate 5′ additions remain folded/non-hybridizing only on the original PCR template or when that sequence is truly absent from the final product.
- Construction Description retains the original 3′ annealing region and 5′ overlap/addition semantics.
- Newly designed PCR template-annealing regions preferentially target 58–60 °C.
- Newly designed Gibson overlaps preferentially target 50–55 °C; overlap length is chosen to reach the target Tm rather than by a fixed bp length.
- Existing suitable primers from the authoritative primer table still take priority over redesign solely to meet the preferred Tm range.

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