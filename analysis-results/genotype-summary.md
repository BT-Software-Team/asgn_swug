# Genotype Summary

Translates variants into genotypes for each gene target. Like [Sample Summary](sample-summary.md), only Summarized variants are included. All samples have an entry for every summarized gene.

**Access**

- **In the Software:** Sample Summary → select a sample row → **View Genotype Summary**. See [Review Results](review-results.md).
- **On disk:** `[Analysis Id]/results/analysis_results/genotypes_summary.csv`

## Columns

| Column | Description |
|--------|-------------|
| **SampleID** | Sample_Name concatenated with Barcode — unique sample identifier |
| **Sample_Name** | User-provided sample name |
| **Barcode** | Barcode applied to the sample |
| **Mix** | Mix the gene target is in |
| **Calibrator** | Calibrator Mixes for this sample; `N` if not a calibrator |
| **QC** | Per-gene QC flags. `PASS` if all pass; `FLAG (ID)` for a single flag; `FLAG (Multiple)` for multiple; `FAIL (Mix)` for Mix-level QC failure (Genotype and Summary columns also show FAIL). For SMN1/2 analyzed in Mixes A and D, Mix A status appears first. If an HBA endogenous control–affecting genotype is detected without other coverage/model/calibrator flags, shows `PASS(CNV_Warn)`. See [Quality Control](../how-results-are-generated/quality-control.md). |
| **Gene** | Gene of interest |
| **Summary** | High-level genotype overview; same nomenclature as `Genes[Status]` in [Sample Summary](sample-summary.md). Gene copy number is always reported here. SMN2 is always reported when summarized. |
| **Genotype** | Detailed gene-specific genotype. See [Gene-Specific Details](../how-results-are-generated/gene-specific-details.md) for per-gene notation rules. |

## What's next

- [Variant Results](variant-results.md) — variant-level detail behind each genotype.
- [Gene-Specific Details](../how-results-are-generated/gene-specific-details.md) — per-gene genotype notation rules.
- [SNVs/Indels](../how-results-are-generated/snvs-indels.md), [Copy Number Variants](../how-results-are-generated/copy-number-variants.md), [Structural Variants](../how-results-are-generated/structural-variants.md), [Short Tandem Repeats](../how-results-are-generated/short-tandem-repeats.md) — how each variant type is called.
