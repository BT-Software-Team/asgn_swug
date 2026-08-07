# Results Description

This page covers the methods behind the results — how each class of variant is called, and how quality control is applied. For the contents of each results view and output file, see the Analysis Results section.

---

## Overview

The primary results are annotated variant calls in `.csv` and `.vcf` format ([Variant Results](../analysis-results/variant-results.md)). These are summarized into per-gene genotypes ([Genotype Summary](../analysis-results/genotype-summary.md)) and then into a per-sample overview ([Sample Summary](../analysis-results/sample-summary.md)). Both summaries include QC flags and are reported in `.csv` format.

For QC details see [Quality Control](#quality-control). For variant type–specific methods see [Variant Classes](#variant-classes).

See [Review Results](../analysis-results/review-results.md) and [Download Results](../analysis-results/download-results.md) for how to access the outputs.

---

## Variant Classes

### SNVs/Indels

Single nucleotide variants (SNVs) and small insertions/deletions (<50 bp).

**Applicable genes:** CFTR, SMN1/2 (Mix A, D/phased), HBA1/2, HBB, CYP21A2, TNXB, GBA1, F8

**Method:** Reads aligned to GRCh38 and assigned to allele groups via sequence deconvolution are used to call variants with [Clair3](https://github.com/HKU-BAL/Clair3). Variants within the same amplicon are phased (indicated with `|`). Variants observed at a group frequency below 0.15 are filtered out. Detection in or adjacent to homopolymers may have reduced sensitivity.

### Copy Number Variants (CNVs)

Copy number changes at the amplicon or gene level.

**Applicable genes:** CFTR (exon-level deletions), SMN1/2, HBA1/2, HBB, CYP21A2, GBA1, TNXB

**Method:** Copy number is inferred from fold-change calculations using calibrated, normalized read counts relative to endogenous control regions. For genes with paralogs (SMN1/2, CYP21A2/A1P, GBA1/P, TNXB/A), sequence deconvolution is used to assign alleles to the correct paralog before copy number calculation.

**Output:** Copy number results appear in the `Amplicon_Copies` column and are reported as SVs in the Genotype column (`0/1` for deletion, `./1` for duplication).

### Structural Variants (SVs)

Larger genomic rearrangements including deletions/insertions >50 bp, inversions, fusions, and microconversions.

**Applicable genes:** All genes in the panel

**Method:** Within-amplicon SVs are detected using [Sniffles2](https://github.com/fritzsedlazeck/Sniffles). Full-amplicon SVs (copy number changes) are detected by fold-change analysis. Inversions (F8 introns 1 and 22) are detected using a multi-primer system. Gene fusions and microconversions (CYP21A2/A1P, GBA1/P, TNXB/A) are detected by evaluating paralog-specific variant (PSV) patterns.

**Short Tandem Repeats (STRs):** FMR1 CGG repeat expansions are detected and sized by read-based analysis. AGG interrupts within CGG tracts are also detected and reported.

---

## Quality Control

All samples undergo QC checks covering read statistics, copy number model confidence, phasing/copy number consistency, and variant-level quality classifications.

- **QC flags** indicate potential issues but do not prevent results from being reported.
- **QC failures (FAIL)** occur when aggregate Mix coverage is insufficient for any analysis to be confidently performed.

QC information is summarized in the Genotypes Summary and Sample Summary, and detailed in the `quality_control/` output files.

**Primary QC output files:**
- `[Analysis Id]/results/quality_control/quality_control.csv` — all entries
- `[Analysis Id]/results/quality_control/quality_control_flagged.csv` — flagged or failing only
- `[Analysis Id]/results/quality_control/quality_control_fail.csv` — failing only

### QC Table Columns

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

### QC Flags Reference

| Flag | Description | Variant Class | Target (Mix) | Criteria |
|------|-------------|:---:|:---:|---------|
| **FAIL** | Mix does not have sufficient fully spanning reads (FSRs) for any analysis | All | All (A, B, C, D) | Mix A: ≥820 FSRs; Mix B: ≥500; Mix C: ≥530; Mix D: ≥195 |
| **CalGT** | Calibrators do not match expected genotypes | CNV, SNV/Indel, SV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: 2/2 copies; CFTR: no deletions; HBA1/2: 2 copies; HBB: 2 copies; no SVs |
| **CalLowCov** | Calibrator does not have sufficient FSR coverage | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | Informs LowCovEC criteria; propagates to all calibrator-dependent analyses |
| **LowConfidence** | Copy number call does not have sufficient model confidence | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C), HBA EC (C) | Confidence ≥0.5 (≥0.6 for HBA EC) |
| **FC** | Amplicon fold changes outside expected gene-specific range | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: [0,7]; CFTR: [0,6]; HBA: [0,5]; HBB: [0,5] |
| **LowCovEC** | Endogenous control geometric mean FSRs insufficient for CN calling | CNV | SMN1/2 (A), CFTR (A), HBA1/2 (C), HBB (C) | SMN1/2: ≥200; CFTR: ≥200; HBA: ≥50; HBB: ≥50 |
| **CNV_Warn** | Genotype impacts endogenous control regions used for HBA1/2 or HBB CN | CNV | HBA1/2 (C), HBB (C) | High-confidence alpha-globin cluster dup/del or HS40 genotype detected. Performance not claimed for HBA1/2 and HBB CN when this flag is present. |
| **LowCov** | Insufficient FSR depth for FMR1 allele detection | STR (CGG/AGG) | FMR1 (B) | <2 alleles and no FM flag: ≥2000 FSRs; otherwise ≥500 FSRs |
| **LowCov** | Insufficient FSR depth for CFTR PolyT/TG sizing | STR (PolyT/TG) | CFTR (A) | ≥60 FSRs |
| **LowCov** | Insufficient FSRs for F8 inversion detection | F8 inversions | F8 (D) | ≥50 FSRs |
| **LowCov** | Insufficient FSRs for within-sequence variant detection | SNV/Indel, SV | CFTR (A), SMN1/2 (A), HBA1/2 (C), HBB (C) | ≥30 FSRs per predicted copy number |
| **LowCov** | Insufficient FSRs across paralogs | SNV/Indel, SV, CNV | GBA (D), CYP21A2 (D), SMN1/2-LR (D) | ≥90 FSRs |
| **LowQUAL** | Clair3 variant calling quality score insufficient (variants table only) | SNV/Indel | CFTR, SMN1/2, HBA1/2, HBB, GBA, CYP21A2, SMN1/2-LR, F8 | Quality Score ≥2 |
| **PHASE** | Discrepancy between variant phasing and predicted copy number (variants table only) | SNV/Indel, SV, CNV | SMN1/2 (A), HBA1/2 (C), HBB (C), GBA (D), CYP21A2 (D), SMN1/2-LR (D) | Zygosity from sequence deconvolution aligns with CNV call |
| **RareGTWarning** | Predicted copy number is unexpected | CNV | CFTR (A), GBA (D), CYP21A2 (D), SMN1/2-LR (D) | Mix A: no homozygous single-amplicon CFTR deletions (excl. Ex19-20). Mix D: total copies across both paralogs ≥1 |

### Coverage Report

The `coverage.csv` file reports fully spanning reads (FSRs), partially spanning reads, and super spanning reads per amplicon, along with analyzed vs. unanalyzed read counts.

**Access:**
- Table: `[Analysis Id]/results/quality_control/coverage.csv`
- Visual: `[Analysis Id]/results/quality_control/coverage.html`

Key coverage columns:

| Column | Description |
|--------|-------------|
| SampleID | SampleName + Barcode |
| Amplicon | Amplicon for the summarized reads |
| Panel | Mix version |
| Calibrator | Whether sample is a calibrator for this Mix |
| Total RC | Total read count (fully + super + partially spanning) |
| Fully Spanning RC | FSRs with AR-tag = TRUE (used in downstream analysis) |
| Unanalyzed Fully Spanning RC | FSRs with AR-tag = FALSE |
| Super Spanning RC | Reads with mismatched primers, AR-tag = TRUE |
| Partially Spanning RC | Single-primer reads, AR-tag = TRUE |
| Ratio | Total RC / Median |
| Median | Median Total RC for the SampleID-Mix combination |
| Sample_Within5xMedian | Whether amplicon Total RC is Over, In Range, or Under 5× the cross-sample median |
