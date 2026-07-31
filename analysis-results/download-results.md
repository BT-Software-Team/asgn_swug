# Download Results

Every completed analysis writes its full output to disk, and you can browse or download any file directly from the Software. This page covers downloading results and the layout of the output folder.

> **Prerequisite:** The analysis must have status **Complete**.

## Download Results

1. From the Analysis Dashboard, click the **⋯** menu on an analysis with status **Complete** and choose **View Files**.

2. Navigate the folder tree to find specific files. The folder structure mirrors the on-disk layout at:

   ```
   [User Data Path]\analyses\{Analysis Id}\results\
   ```

3. Click any file to open it in the browser or download it. Supported formats include `.png`, `.html`, `.csv`, `.json`, `.log`, `.txt`, and other common types.

### Access Results Outside the Software

Analysis results remain accessible on disk even after the Software is uninstalled. Navigate to:

```
[User Data Path]\analyses\{Analysis Id}\results\
```

---

## Output Structure

```
results/
├── analysis_results/
│   ├── genotypes_summary.csv     # Per-gene genotype summary for all samples
│   ├── sample_summary.csv        # Per-sample variant and QC summary
│   └── variants.csv              # All variants for all samples
├── figures/                      # Interactive (.html) and static (.png) visualizations
│   ├── mix_a/
│   │   ├── cftr_large_exon_deletion/
│   │   │   └── [sample]_BCXX_CFTR.{png,html}        # CFTR copy number signal
│   │   └── smn/
│   │       └── SMN_Fold_Change.{png,html}             # SMN1/2 copy number scatter plot
│   ├── mix_b/
│   │   └── fmr1/
│   │       ├── [sample]_BCXX_FMR1.{png,html}         # FMR1 allele waterfall plot
│   │       └── [sample]_BCXX_FMR1_signal.html         # CGG sizing histogram
│   ├── mix_c/
│   │   └── hba_cnv/
│   │       └── [sample]_BCXX_HBA.{png,html}          # HBA1/2 copy number signal
│   └── mix_d/
│       ├── cyp21a2_tnxb/
│       │   └── [sample]_BCXX_CYP21_TNXB.{png,html}  # CYP21A2-TNXB allele transcript map
│       ├── f8/
│       │   └── [sample]_BCXX_F8.{png,html}           # F8 inversion read depth bar chart
│       ├── gba/
│       │   └── [sample]_BCXX_GBA.html                # GBA1 allele transcript map
│       └── smn/
│           ├── [sample]_BCXX_SMN1.{png,html}         # SMN1 allele transcript map
│           └── [sample]_BCXX_SMN2.{png,html}         # SMN2 allele transcript map
├── inputs/
│   ├── [panel_config_name].json  # Panel filter configuration used
│   └── sample_sheet.txt          # Sample sheet used for this analysis
├── json/                         # JSON versions of analysis_results files (for UI/LIMS)
│   ├── genotypes_summary.json
│   ├── sample_summary.json
│   └── variants.json
├── logs/
│   ├── run.log                   # Summary log with start/end time and failures
│   ├── failed_processes_log.txt  # Failed pipeline processes and hashes
│   ├── errors/                   # Working directories for each failed process
│   └── pipeline_info/            # Nextflow execution trace, reports, and versions
├── quality_control/
│   ├── coverage.csv              # Per-amplicon read counts and statistics
│   ├── coverage.html             # Visual coverage report (per mix, per sample)
│   ├── quality_control.csv       # All QC measurements and flags
│   ├── quality_control_fail.csv  # Failing QC entries only
│   ├── quality_control_flagged.csv  # Flagged or failing QC entries only
│   └── sample_qc/
│       └── [sample]_BCXX_qc_ampl_plots.html  # Per-sample amplicon coverage bar chart
├── sample_files/
│   ├── bam/
│   │   ├── [sample]_BCXX.bam     # Aligned reads for genome browser review (e.g., IGV)
│   │   └── [sample]_BCXX.bam.bai
│   ├── variants_csv/
│   │   └── [sample]_BCXX_variants.csv
│   └── vcf/
│       └── [sample]_BCXX_variants.vcf
└── supplementary/
    ├── bam_files/                # BAMs with both analyzed and unanalyzed reads
    └── raw_variants.csv          # Unfiltered variants table
```

For detailed descriptions of each file's columns and contents, see [Results Description](../reference/results-description.md).
