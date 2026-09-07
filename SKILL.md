---
name: snapgene-plasmid-builder
version: 1.4.0
description: Simulate SnapGene plasmid construction from native .dna templates while preserving topology, features, primer display, primer provenance, and traceable construction records.
---

# SnapGene Plasmid Builder Skill

## 1. Source-of-truth priority

When information conflicts, use this order:

1. current user instruction
2. latest `primers/primer_table.csv`
3. this `SKILL.md`
4. uploaded native SnapGene `.dna` metadata
5. legacy primer names in template files

The primer table is authoritative for **official primer names**, but native `.dna` files are important evidence for historical primer binding, display, and use.

## 2. Required inputs

Prefer receiving:

- backbone/parent `.dna`
- insert/template `.dna`
- latest primer table
- target plasmid name
- construction goal
- optional Gibson/Golden Gate preference and culture conditions

## 3. Native SnapGene preservation

Whenever possible, modify from a native SnapGene template rather than rebuilding a generic file from scratch.

Preserve when applicable:

- circular/linear state
- double-stranded state
- DNA flags/methylation metadata
- feature names, directions, colors, qualifiers, and translation frames
- primer XML/display conventions
- relevant notes/description metadata

Do not add broad overlay features such as `cargo` if they obscure the source feature hierarchy unless explicitly requested.

Final plasmids should normally open as circular dsDNA when the parent is a circular dsDNA plasmid.

## 4. SnapGene History policy

Do not fabricate native SnapGene History.

If native History cannot be generated safely, write a `Manual Construction History` into Description containing:

- parent/template names
- PCR primer pairs
- fragment sizes
- assembly method
- final size
- modification relative to parent
- preserved elements
- colony-PCR pair and expected product
- primer provenance/legacy aliases
- QC status

State explicitly that this is manual Description history, not native SnapGene History.

## 5. Primer provenance: preserve useful historical primers

Do **not** use the 58–60 °C design target to automatically replace useful historical laboratory primers.

Classify primer candidates in this order:

1. **User-locked primer** — explicitly requested by the user; keep sequence/name unchanged.
2. **Established laboratory primer** — already in the current primer table and/or already present on the relevant native SnapGene template with a correct binding site. Preserve by default.
3. **Legacy template primer** — present in an uploaded `.dna` template but absent from the primer table. Reuse when experimentally useful; if it becomes active in the new construction, normalize it to the next formal JC name and record the legacy alias in Description.
4. **New primer** — design only when no suitable historical primer can perform the task.

For established/legacy primers, do **not** redesign solely because a different Tm calculator gives a value a few degrees away from an ideal target. Historical use, correct binding geometry, native SnapGene BindingSite/Tm metadata, existing inventory, and reduced need to order new oligos all have value.

An established primer pair already used together on the same backbone has especially high reuse priority.

Replace an existing primer only for a material reason, such as:

- wrong template or orientation
- wrong junction for the current assembly
- meaningful 3′ mismatch or insufficient specific annealing
- known experimental failure
- severe secondary-structure or pair incompatibility
- explicit user request to optimize or replace it

If the user locks a primer (for example `JC2522F`), retain it unchanged and report any QC warning rather than redesigning it.

## 6. Primer-table normalization

For matching primer sequences:

- uppercase
- remove spaces/hyphens
- compare full sequence exactly first

If a template primer sequence matches a primer-table entry, use the primer-table name as the official name and record the old name as a legacy alias.

If a useful template primer is absent from the primer table, preserve its sequence and assign the next valid JC identifier rather than redesigning merely to improve Tm.

Never create a duplicate new primer when the same sequence already exists in the authoritative table.

## 7. Tm rules

The following are primarily **design targets for genuinely new oligos**.

### New PCR annealing region

- target Tm: **58–60 °C**
- adjust the 3′ template-binding length to meet the target when practical

### New Gibson overlap

- target overlap Tm: **50–55 °C**
- choose overlap length from Tm, not from a fixed 20/25-bp rule

Always distinguish:

- 3′ template-annealing region and its Tm
- 5′ Gibson overlap and its Tm

For established/legacy primers, preserve a useful historical sequence even if it is modestly outside these windows. When native SnapGene BindingSite `meltingTemperature` metadata exists, preserve and report it as evidence of historical intended use rather than overriding it with a different calculator solely for optimization.

## 8. Primer3 / thermodynamic QC

Before final delivery, run Primer3/ntthal-compatible checks when available for:

- every newly designed primer
- every new PCR primer pair, including reused primers in a new pairing

Check at minimum:

- hairpin
- self-dimer/self complementarity
- 3′ self complementarity
- pair heterodimer/pair complementarity
- 3′ pair complementarity

For Gibson/Golden Gate primers, secondary-structure analysis should use the **full synthesized oligo including 5′ additions**, while annealing Tm is reported for the 3′ template-binding region separately.

If actual Primer3/ntthal cannot be executed, write exactly:

`Primer3 QC: NOT VERIFIED`

Do not claim a Primer3 pass based on an informal calculation.

Lack of Primer3 execution does not automatically justify replacing an established historical primer; report the unverified status instead.

## 9. Gibson/Golden Gate primer display in SnapGene

### On the original PCR template

- 5′ Gibson/Golden Gate additions that are absent from the template should be non-hybridizing components
- only the real 3′ template-binding region should be marked as annealed

### On the final assembled plasmid

If the complete primer sequence is present continuously in the final product, display/map the **full primer as one continuous match**. Do not keep a former Gibson tail visually folded out after assembly if it is now part of the final sequence.

If part of the primer is truly absent from the final sequence, keep that component non-hybridizing.

Record the original 5′ overlap/addition versus 3′ annealing semantics in Description.

## 10. Gibson junction audit

For every Gibson junction, verify and record:

- source fragment side
- primer carrying the overlap
- overlap sequence
- overlap length
- overlap Tm for newly designed overlaps
- exact identity of the assembled junction

A Gibson overlap need only be introduced on one adjacent PCR primer when the opposite fragment already naturally contains the homologous sequence.

## 11. Colony-PCR selection

Recommend one primary colony-PCR pair per new plasmid unless asked otherwise.

Priority:

1. established primer-table primers near the Gibson/Golden Gate interface
2. useful existing template primers near the interface
3. existing internal validation primers
4. newly designed primers only if necessary

Prefer junction PCR with one primer in retained backbone and one in the new insert/regulatory region.

Avoid a pair that gives the same diagnostic band from the parent plasmid.

For an existing primer with a 5′ non-template tail, include the tail length when reporting the expected final PCR product size.

## 12. Assembly simulation

For each target plasmid:

1. parse source `.dna` files
2. identify exact retained backbone interval(s)
3. identify exact insert interval(s)
4. confirm orientation and junctions
5. audit existing primers before designing new ones
6. simulate PCR products including 5′ additions
7. assemble the final expected sequence
8. remap/copy source features and styles
9. normalize active primers to primer-table naming
10. add one recommended colony-PCR pair
11. write Manual Construction History in Description
12. preserve parent topology/display metadata
13. independently verify final sequence and junctions

## 13. Mandatory validation

Before delivery verify:

### Sequence
- expected final length
- exact junctions
- requested CDS/template sequence identity
- no unintended bases at assembly boundaries
- retained regions unchanged

### SnapGene
- intended circular/linear state
- dsDNA state
- feature visibility/colors/directions
- primer visibility/direction
- final-product primer continuous display when full sequence exists
- Description history present

### Primer
- official names match current primer table
- established primers were not unnecessarily redesigned
- new primer annealing region normally targets 58–60 °C
- new Gibson overlap normally targets 50–55 °C
- Primer3 status explicitly reported
- colony-PCR product size is diagnostic

## 14. Compact laboratory construction checklist

When the user asks for `构建清单` or `实验清单`, output an ultra-compact TXT-style record using current UTC+8 time to the minute.

### 14.1 Deduplicate physical PCR fragments before assigning F numbers

Before writing the checklist, compare **all PCR fragments across all target plasmids in the same task**.

Treat two PCR fragments as the **same reusable physical fragment** when they have the same:

- template
- primer pair
- expected PCR product sequence/length

A reusable fragment must be listed **once only** and assigned **one F number only**. Later plasmid assemblies must reference that same F number instead of creating a duplicate F entry.

`F1/F2/F3...` therefore represent **unique PCR products that need to be physically prepared**, not fragment slots inside each plasmid design.

If the same backbone PCR is used for several Gibson assemblies, mark it clearly as shared, for example:

```text
T = pJC2641
P = JC2330F/JC2330R
F1 = 12776（共用，做1次）
```

Then reuse it:

```text
pJC2642/Gibson = f1+f2
pJC2643/Gibson = f1+f3
```

Do not instruct the user to repeat the same PCR merely because the fragment appears in more than one target plasmid. If more DNA quantity may be required, state that separately rather than silently duplicating the PCR in the checklist.

### 14.2 Checklist format

Example:

```text
时间：YYYY-MM-DD HH:MM

共用PCR片段

T = pJC2641
P = JC2330F/JC2330R
F1 = 12776（共用，做1次）


构建 pJC2642（目标：...）

T = insert_template_A
P = primerF/primerR
F2 = length

pJC2642/Gibson = f1+f2
DH5a，LB，“___”，30℃


构建 pJC2643（目标：...）

T = insert_template_B
P = primerF/primerR
F3 = length

pJC2643/Gibson = f1+f3
DH5a，LB，“___”，30℃
```

Rules:

- deduplicate identical PCR products across the entire checklist **before** numbering
- F numbering is continuous across **unique physical PCR products** only
- a shared F can be referenced by any number of downstream assemblies
- Golden Gate uses `pXXXX/GoldenGate = ...`
- unknown culture conditions use `“___”`; do not invent them
- keep the checklist directly copyable and report-like prose out of it

## 15. Standard deliverables

Unless requested otherwise, return:

1. final `pXXXX.dna`
2. optional `.gb` backup
3. updated primer-table draft/validated table as appropriate
4. primer subset for the current construction
5. compact construction-list `.txt`
6. QC report when primer redesign/QC is involved

Do not write unverified new primers into the authoritative GitHub primer table unless the workflow's required validation has been completed or the user explicitly instructs otherwise.
