# Quality Control

All samples undergo QC checks covering read statistics, copy number model confidence, phasing/copy number consistency, and variant-level quality classifications.

- **QC flags** indicate potential issues but do not prevent results from being reported.
- **QC failures (FAIL)** occur when aggregate Mix coverage is insufficient for any analysis to be confidently performed.

QC information is summarized in the Genotype Summary and Sample Summary, and detailed in the `quality_control/` output files.

**Primary QC output files:**
- `[Analysis Id]/results/quality_control/quality_control.csv` — all entries
- `[Analysis Id]/results/quality_control/quality_control_flagged.csv` — flagged or failing only
- `[Analysis Id]/results/quality_control/quality_control_fail.csv` — failing only

## QC Table Columns

| Column | Description |
|--------|-------------|
| SampleID | SampleName + Barcode |
| SampleName | User-provided name |
| Barcode | Barcode applied |
| Mix | Mix the QC applies to |
| Analysis | Analysis type affected |
| Measurement under Consideration | Specific measurement identifier |
| Measurement Pass Threshold | Required value to pass |
| Measurement Value | Observed value |
| QC Flag | `PASS` or the associated flag identifier |

## QC Flags Reference

Some flags are listed more than once because the criteria differ by gene.

| Flag | Description | Variant Class | Target (Mix) | Criteria | Notes |
|------|-------------|:---:|:---:|---------|-------|
| **FAIL** | Mix does not have sufficient fully spanning reads (FSRs) for any analysis | All | All (A, B, C, D) | Mix A: ≥820 FSRs; Mix B: ≥500; Mix C: ≥530; Mix D: ≥195 | — |
| **CalGT** | Calibrators do not match expected genotypes | CNV, SNV/Indel, SV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: 2/2 copies; CFTR: no deletions; HBA1/2: 2 copies; HBB: 2 copies; no SVs | Propagates to all calibrator-dependent analyses for all samples in the affected Mix. |
| **CalLowCov** | Calibrator does not have sufficient FSR coverage | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | Informs LowCovEC criteria | Propagates to all calibrator-dependent analyses for all samples in the affected Mix. |
| **LowConfidence** | Copy number call does not have sufficient model confidence | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C), HBA EC (C) | Confidence ≥0.5 (≥0.6 for HBA EC) | Confidence metric ranges 0–1. |
| **FC** | Amplicon fold changes outside expected gene-specific range | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: [0,7]; CFTR: [0,6]; HBA: [0,5]; HBB: [0,5] | — |
| **LowCovEC** | Endogenous control geometric mean FSRs insufficient for CN calling | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: ≥200; CFTR: ≥200; HBA: ≥50; HBB: ≥50 | — |
| **CNV_Warn** | Genotype impacts endogenous control regions used for HBA1/2 or HBB CN | CNV | HBA1/2 (C), HBB (C) | High-confidence alpha-globin cluster dup/del or HS40 genotype detected. Performance not claimed for HBA1/2 and HBB CN when this flag is present. | — |
| **LowCov** | Insufficient FSR depth for FMR1 allele detection | STR (CGG/AGG) | FMR1 (B) | <2 alleles and no FM flag: ≥2000 FSRs; otherwise ≥500 FSRs | — |
| **LowCov** | Insufficient FSR depth for CFTR PolyT/TG sizing | STR (PolyT/TG) | CFTR (A) | ≥60 FSRs | — |
| **LowCov** | Insufficient FSRs for F8 inversion detection | F8 inversions | F8 (D) | ≥50 FSRs | — |
| **LowCov** | Insufficient FSRs for within-sequence variant detection | SNV/Indel, SV | CFTR (A), SMN1/2 (A), HBA1/2 (C), HBB (C) | ≥30 FSRs per predicted copy number | — |
| **LowCov** | Insufficient FSRs across paralogs | SNV/Indel, SV, CNV | GBA (D), CYP21A2 (D), SMN1/2-LR (D) | ≥90 FSRs | — |
| **LowQUAL** | Clair3 variant calling quality score insufficient (variants table only) | SNV/Indel | CFTR, SMN1/2, HBA1/2, HBB, GBA, CYP21A2, SMN1/2-LR, F8 | Quality Score ≥2 | Output directly from external software. Only shown in [Variant Results](../analysis-results/variant-results.md). |
| **PHASE** | Discrepancy between variant phasing and predicted copy number (variants table only) | SNV/Indel, SV, CNV | SMN1/2 (A), HBA1/2 (C), HBB (C), GBA (D), CYP21A2 (D), SMN1/2-LR (D) | Zygosity from sequence deconvolution aligns with CNV call | Only listed for individual variants in [Variant Results](../analysis-results/variant-results.md). Investigate flagged variants to determine whether zygosity or copy number is incorrect. |
| **RareGTWarning** | Predicted copy number is unexpected | CNV | CFTR (A), GBA (D), CYP21A2 (D), SMN1/2-LR (D) | Mix A: no homozygous single-amplicon CFTR deletions (excl. Ex19-20). Mix D: total copies across both paralogs ≥1 | Not propagated to results. |

## Analysis Identifier Reference

Decodes the `Analysis` / `Measurement under Consideration` identifiers that appear in `quality_control.csv`.

| Analysis Identifier (Variant Class) | Description | Possible Measures for QC Consideration |
|---|---|---|
| `CFTR_POLYTTG` (STR) | CFTR PolyT/TG sizing | `CFTR_Ex10`: FSR coverage of the CFTR_Ex10 amplicon (contains the PolyT/TG region) |
| `[GeneName]_CN` (CNV) | Amplicon copy number prediction | `[AmpliconName]_FC`: fold change of the target amplicon. `[GeneName]_FC`: fold change of the single amplicon used for the gene's CN call (e.g., `SMNx-Ex07-08`). `EC_geomean`: geometric mean of all endogenous controls. `[GeneName]_Model_Confidence`: confidence of the predicted copy number. `[GeneName]_EC_Model_Confidence`: minimum confidence of EC-affecting genotype calls if ≥1 model was evaluated, otherwise the confidence of the first model in series with a non-Normal EC-affecting call. `Model_Confidence`: confidence for a distinct gene using the same EC amplicons. `[GeneName]_geomean`: geometric mean of the gene's EC amplicons. `EC_Adjusted`: read counts of non-2 CN endogenous control regions artificially adjusted to simulate 2 CN for downstream CNV models. `CFTR_LED_HSAD`: homozygous single-amplicon deletion detected — investigate copy number signal if the corresponding RareGT flag is also shown. `[Paralog1_Paralog2]_sum`: FSR coverage summed across both paralogs |
| `[EC or HBA]_geomean` (CNV) | Endogenous control geometric mean calculation used for downstream CN calculations | `[ECAmpliconName]`: FSR coverage of the endogenous control |
| `SNV-Indel/SV` (SNV, Indel, SV) | SNV/Indel and SV calling | `[AmpliconName]`: FSR coverage of the target amplicon |
| `Calibrator_Matches_Expectations` (CNV) | Verifies calibrator variant calls match expectations | `[GeneName]_CN` or `[GeneName_AmpliconName]_CN`: copy number of the gene or amplicon. `CFTR_LED`: amplicon deletions in CFTR |
| `FMR1_CGG_AGG` (STR) | FMR1 CGG sizing and AGG interrupt detection | `Mix_B_FS_reads`: FSR coverage of FMR1 |
| `[Paralog1_Paralog2]_sum` (CNV, informational) | FSR coverage summed across both paralogs | `[Paralog1]` / `[Paralog2]`: FSR coverage of each paralog. Informs `[GeneName]_CN` QC; no individual threshold |
| `F8_[Intron]_sum` (SV, informational) | FSR coverage summed across all F8 amplicons for the intron/inversion contigs | `[Amplicon]`: FSR coverage of each amplicon involved in `F8_[Intron]_INV` analyses. Informs `F8_[Intron]_INV` QC; no individual threshold |
| `F8_[Intron]_INV` (SV) | Inversion detection for F8 introns 1 and 22 | `F8_[Intron]_sum`: FSR coverage summed across the intron's F8 amplicons |
| `[GeneName]_DECONVOLUTION` (SV, Mix D only) | Sequence deconvolution applied to SMN1/2 in Mix D only | `SMN_Ex03-08`: FSR coverage summed across both SMN1/2-LR paralogs |
| `Calibrator_Matches_Expectations` | Calibrator genotype results as expected (calibrators only) | `CFTR_LED`: copy number predictions for all CFTR amplicons. `[GeneName or AmpliconName]_CN`: copy number prediction |

## Coverage Report

The `coverage.csv` file reports fully spanning reads (FSRs), partially spanning reads, and super spanning reads per amplicon, along with analyzed vs. unanalyzed read counts.

**Access:**
- Table: `[Analysis Id]/results/quality_control/coverage.csv`
- Visual: `[Analysis Id]/results/quality_control/coverage.html` — the same data summed to the Mix and sample level, as bar charts of total read counts per Mix and per Sample ID/Mix combination.

Reads are tagged Boolean `AR` = analyzed (`TRUE`) or unanalyzed (`FALSE`) — reads may go unanalyzed due to a missing complementary primer, downsampling, or evidence of a PCR artifact. Analyzed reads (`AR` = `TRUE`) appear in both the primary BAM (`[Analysis Id]/results/sample_files/bam/`) and the supplementary BAM (`[Analysis Id]/results/supplementary/bam_files/`); unanalyzed reads (`AR` = `FALSE`) appear only in the supplementary BAM. Only analyzed fully spanning reads are used in downstream analysis.

| Column | In primary BAM? | Description |
|--------|:---:|-------------|
| SampleID | N/A | SampleName + Barcode |
| Amplicon | N/A | Amplicon for the summarized reads |
| Panel | N/A | Mix version |
| Calibrator | N/A | Whether sample is a calibrator for this Mix |
| Total RC | Subset | Total read count (fully + super + partially spanning) |
| Fully Spanning RC | Yes | FSRs with matching forward/reverse primers, AR = TRUE |
| Unanalyzed Fully Spanning RC | No | FSRs with matching forward/reverse primers, AR = FALSE |
| Super Spanning RC | Yes | Reads with mismatched forward/reverse primers, AR = TRUE |
| Unanalyzed Super Spanning RC | No | Reads with mismatched forward/reverse primers, AR = FALSE |
| Partially Spanning RC | Yes | Single-primer reads (forward or reverse), AR = TRUE |
| Unanalyzed Partially Spanning RC | No | Single-primer reads (forward or reverse), AR = FALSE |
| Partial Span Fwd Found RC | Subset | Reads with the forward primer only, AR = FALSE |
| Partial Span Rev Found RC | Subset | Reads with the reverse primer only, AR = FALSE |
| Ratio | N/A | Total RC / Median |
| Median | N/A | Median Total RC for the SampleID-Mix combination |
| Sample_Within5xMedian | N/A | Whether amplicon Total RC is Over, In Range, or Under 5× the cross-sample median for that amplicon |
| Sample_Within4xMedian | N/A | Same, at 4× the cross-sample median |
| Sample_Within3xMedian | N/A | Same, at 3× the cross-sample median |

## What's next

- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
- [Variant Results](../analysis-results/variant-results.md) — where per-variant QC flags appear.
