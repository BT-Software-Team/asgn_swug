# Sample Summary

The highest-level overview of variants per sample. Only variants that pass the **Summarize** filter criteria appear here (see [Panel Filter Configurations](../configuration/panel-filters.md)). Gene entries also apply additional gene-specific rules (Table 4 in the source guide).

**Access**

- **In the Software:** Analysis Dashboard → **⋯** on a Complete analysis → **Sample Summary**. See [Review Results](review-results.md).
- **On disk:** `[Analysis Id]/results/analysis_results/sample_summary.csv`

## Columns

| Column | Description |
|--------|-------------|
| **Sample_Name** | User-provided sample name |
| **QC** | QC flags per Mix. `PASS` if all Mixes pass; `FLAG(Mix)` or `FAIL(Mix)` if a Mix has issues; combinations separated by `;`. See [Quality Control](../how-results-are-generated/quality-control.md). |
| **Genes\[Status\]** | High-level genotype overview. Copy-number genes (HBA1/2, SMN1/2, GBA1, CYP21A2, TNXB) show copy count + `cp`. Variant counts use the symbols in the table below. Phase shown as `(1\|0)` / `(0\|1)`; homozygous as `(H)`. No variants → `No Variants`. |
| **Barcode** | Barcode applied to the sample |
| **Mixes** | Mixes analyzed for the sample |
| **Calibrator** | Mixes the sample was used as calibrator for. `N` if not a calibrator. |

## Variant Symbols in Genes\[Status\]

| Symbol | Variant Type | Genes | Notes |
|--------|-------------|-------|-------|
| V | SNV/Indel | All except FMR1 | |
| SV | Structural Variant (fusions, inversions, large del/ins >50 bp) | All except FMR1 | |
| LV | Linked Variant | SMN1/2 | `c.*3+80 T>G`, `c.*211_*212del` linked to the cis-silent (2+0) carrier haplotype. SMN2 modifier `c.859G>C` is reported as V. |
| PM | Premutation | FMR1 | 55–200 CGG repeats |
| FM | Full Mutation | FMR1 | >200 CGG repeats |

## Which Variants Appear in Sample Summary (by Gene) {#which-variants-appear-in-sample-summary-by-gene}

| Gene | Variants Represented |
|------|---------------------|
| CFTR | Any Summarized variants; any SVs |
| SMN1 | Any Summarized variants; copy number <2; linked variants (Summarized by default) |
| SMN2 | Mix A: copy number + Summarized variants reported only if SMN1 is also reported. Mix D: not reported. Disease modifier `c.859G>C` Summarized by default. |
| FMR1 | Premutations (PM) and full mutations (FM) only |
| HBB | Any Summarized variants; any SVs; copy number <2 |
| HBA | Any alpha-cluster genotypes affecting HBA_Rgn01–14, or HS-40 deletions affecting HBA_Rgn01/02 |
| HBA1/2 | Any Summarized variants; any SVs; both reported if either copy number <2; `HBA1 [2 cp] / HBA2 [2 cp]` shown for silent carriers with concomitant duplication + deletion |
| CYP21A2/A1P | Any Summarized variants; any SVs; copy number <2 |
| GBA1/P | Any Summarized variants; any SVs; copy number <2 |
| TNXA/B | Any Summarized variants; any SVs; copy number <2 |
| F8 | Any Summarized variants; any SVs; all inversions |

## What's next

- [Genotype Summary](genotype-summary.md) — per-gene genotypes for a sample.
- [SNVs/Indels](../how-results-are-generated/snvs-indels.md), [Copy Number Variants](../how-results-are-generated/copy-number-variants.md), [Structural Variants](../how-results-are-generated/structural-variants.md), [Short Tandem Repeats](../how-results-are-generated/short-tandem-repeats.md), and [Quality Control](../how-results-are-generated/quality-control.md) — how variants are called and flagged.
