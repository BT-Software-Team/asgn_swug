# How Results Are Generated

This section covers the methods behind the results — how each class of variant is called, and how quality control is applied. For the contents of each results view and output file, see [Analysis Results](../analysis-results/review-results.md).

The primary results are annotated variant calls in `.csv` and `.vcf` format ([Variant Results](../analysis-results/variant-results.md)). These are summarized into per-gene genotypes ([Genotype Summary](../analysis-results/genotype-summary.md)) and then into a per-sample overview ([Sample Summary](../analysis-results/sample-summary.md)). Both summaries include QC flags and are reported in `.csv` format.

- [SNVs/Indels](snvs-indels.md), [Copy Number Variants](copy-number-variants.md), [Structural Variants](structural-variants.md), and [Short Tandem Repeats](short-tandem-repeats.md) — how each variant class is detected.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
- [Quality Control](quality-control.md) — QC flags, the analysis identifier reference, and coverage reporting.
