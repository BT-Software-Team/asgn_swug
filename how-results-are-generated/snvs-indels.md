# SNVs/Indels

Single nucleotide variants (SNVs) and small insertions/deletions (<50 bp).

**Applicable genes:** CFTR, SMN1/2 (Mix A, D/phased), HBA1/2, HBB, CYP21A2, TNXB, GBA1, F8

**Method:** Reads aligned to GRCh38 and assigned to allele groups via [sequence deconvolution](overview.md#sequence-deconvolution) are used to call variants with [Clair3](https://github.com/HKU-BAL/Clair3). Variants within the same amplicon are phased (indicated with `|`). Variants observed at a group frequency below 0.15 are filtered out. Detection in or adjacent to homopolymers may have reduced sensitivity.

## Phasing & Zygosity

Variants are phased only when they occur on the same amplicon; this is reflected in the Genotype Summary and Variant Results views. Several panel genes (e.g., CYP21A2, SMN1/2) can present with more than 2 copies — phasing information across these views is what distinguishes which overlapping variants sit on which allele.

## Visualization

To review SNVs/Indels in the context of read alignment, download the sample's BAM/BAI files (see [Download Results](../analysis-results/download-results.md)), load them into [IGV](https://software.broadinstitute.org/software/igv/), and navigate using the coordinates from [Variant Results](../analysis-results/variant-results.md). Some gene targets also produce separate visual representations of paralog-specific variants — see [Fusions](structural-variants.md#fusions).

## What's next

- [Copy Number Variants](copy-number-variants.md), [Structural Variants](structural-variants.md), [Short Tandem Repeats](short-tandem-repeats.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
