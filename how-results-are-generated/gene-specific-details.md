# Gene-Specific Details

Each gene has unique considerations and nuances for correctly interpreting variant calling results.

## Genotype Notation

How the `Genotype` column in [Genotype Summary](../analysis-results/genotype-summary.md) is formatted, per gene:

**CFTR:** SNV/Indels and within-amplicon SVs shown in brackets after the amplicon name, with pathogenicity if available. Two-amplicon deletions shown as `dele[exons]`; longer or complex deletions as `ExonDeletionOther`. Variants on different alleles within the same amplicon separated by `|` (phased) or marked `unphased`. 5T alleles in the Poly-T/TG region listed with ClinVar annotation. `No Variants` if none found.

**SMN1/2:** Variants shown in brackets after the amplicon name. Phased variants separated by `|`; unphased marked `unphased`. Mix A: `DELETION` or `DUPLICATION` shown for copy number changes. Mix D: copy number not reported; only phased variants. Mix A+D: copy number from Mix A; Mix D variants reported if found in both Mixes.

**FMR1:** Each allele's CGG size shown with AGG interrupts. Expansion status: Normal (5–44), Intermediate (45–54), Premutation / PM (55–200), Full Mutation / FM (>200). Relative allele abundance shown as 0–1 (most abundant normalized to 1). Unsized full mutations shown as `(>200CGG[noAGG], Full_Mutation) (nan)`.

**HBA1/2:** Two copies of HBA1 and HBA2 always individually listed. Deletions/duplications shown with `DELETION` or `DUPLICATION`; canonical SV names shown (e.g., `3.7del`) when applicable.

**HBB:** SNV/Indels and SVs in brackets after the amplicon name. Phased variants separated by `|`; unphased marked `unphased`.

**CYP21A2/A1P and TNXB/A:** All copies individually listed; gene assignment based on majority-rule PSVs. SNV/Indels and within-amplicon SVs in brackets after the gene name. Fusion subtypes shown when matched; PSVs shown as microconversions when pattern doesn't match a known subtype.

**GBA1/P1:** All copies individually listed. SNV/Indels and SVs in brackets. `Fusion` shown next to any allele with detected PSVs.

**F8:** Intron 01 and intron 22 inversions, SNV/Indels, and within-amplicon SVs in brackets after the amplicon name. Inversions labeled `INVERSION, Pathogenic`. Phased variants listed sequentially; variants on different copies separated by `|`; unphased variants labeled `unphased`.

## What's next

- [Quality Control](quality-control.md) — QC flags and coverage reporting.
- [Genotype Summary](../analysis-results/genotype-summary.md) — where this notation appears.
