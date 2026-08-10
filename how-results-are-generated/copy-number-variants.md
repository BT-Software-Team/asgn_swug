# Copy Number Variants

Copy number changes at the amplicon or gene level.

**Applicable genes:** CFTR (exon-level deletions), SMN1/2, HBA1/2, HBB, CYP21A2, GBA1, TNXB

**Method:** Copy number is inferred from fold-change calculations using calibrated, normalized read counts relative to endogenous control regions. For genes with paralogs (SMN1/2, CYP21A2/A1P, GBA1/P, TNXB/A), sequence deconvolution is used to assign alleles to the correct paralog before copy number calculation.

**Output:** Copy number results appear in the `Amplicon_Copies` column and are reported as SVs in the Genotype column (`0/1` for deletion, `./1` for duplication).

## What's next

- [SNVs/Indels](snvs-indels.md), [Structural Variants](structural-variants.md), [Short Tandem Repeats](short-tandem-repeats.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
