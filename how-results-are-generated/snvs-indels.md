# SNVs/Indels

Single nucleotide variants (SNVs) and small insertions/deletions (<50 bp).

**Applicable genes:** CFTR, SMN1/2 (Mix A, D/phased), HBA1/2, HBB, CYP21A2, TNXB, GBA1, F8

**Method:** Reads aligned to GRCh38 and assigned to allele groups via sequence deconvolution are used to call variants with [Clair3](https://github.com/HKU-BAL/Clair3). Variants within the same amplicon are phased (indicated with `|`). Variants observed at a group frequency below 0.15 are filtered out. Detection in or adjacent to homopolymers may have reduced sensitivity.

## What's next

- [Copy Number Variants](copy-number-variants.md), [Structural Variants](structural-variants.md), [Short Tandem Repeats](short-tandem-repeats.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
