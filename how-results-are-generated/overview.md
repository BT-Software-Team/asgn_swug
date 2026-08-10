# How Results Are Generated

This section covers the methods behind the results — how each class of variant is called, and how quality control is applied. For the contents of each results view and output file, see [Analysis Results](../analysis-results/review-results.md).

The primary results are annotated variant calls in `.csv` and `.vcf` format ([Variant Results](../analysis-results/variant-results.md)). These are summarized into per-gene genotypes ([Genotype Summary](../analysis-results/genotype-summary.md)) and then into a per-sample overview ([Sample Summary](../analysis-results/sample-summary.md)). Both summaries include QC flags and are reported in `.csv` format.

- [SNVs/Indels](snvs-indels.md), [Copy Number Variants](copy-number-variants.md), [Structural Variants](structural-variants.md), and [Short Tandem Repeats](short-tandem-repeats.md) — how each variant class is detected.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
- [Quality Control](quality-control.md) — QC flags, the analysis identifier reference, and coverage reporting.

---

## Sequence Deconvolution

Several variant classes reference "sequence deconvolution" as part of their method — this section defines it once.

Sequence deconvolution is an algorithm that identifies groups of reads sharing an underlying sequence from aligned sequencing data. Reads are grouped by similarity while accounting for sequencing and basecalling errors. It enables variant phasing and, for some targets, informs copy number (see the relevant gene's entry in [Gene-Specific Details](gene-specific-details.md)). Reads assigned to a group carry a deconvolution group (`dg`) tag in output BAM files.

Grouping is done on a single-target basis by default. Reads from paralogous genes are aligned to the primary target gene before deconvolution; each resulting group is then assigned to the paralog with the highest similarity score to the group's consensus sequence. SMN1/2 and CYP21A2/CYP21A1P use gene-specific alternative assignment logic instead — see their entries in [Gene-Specific Details](gene-specific-details.md).

**Applicable genes:** CFTR, SMN1/2 (Mix A; Mix D amplicons are used only for phasing, not copy number calls), HBA1/2, HBB, CYP21A2, TNXB, GBA1
