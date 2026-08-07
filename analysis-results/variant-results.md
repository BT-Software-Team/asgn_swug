# Variant Results

Detailed entries for each identified variant. The **Analyze** filter setting determines which variants appear (see [Panel Filter Configurations](../configuration/panel-filters.md)). Available in `.csv` and `.vcf` format.

## Variants (.csv)

Each row represents one variant.

**Access**

- **In the Software:** Genotype Summary → select a row → **View Variant Results**. See [Review Results](review-results.md).
- **On disk:** `[Analysis Id]/results/analysis_results/variants.csv`

The **Default View** column below indicates whether that column is shown when you open Variant Results. Hidden columns can be turned on with the **Columns** control in the table toolbar — see [Table Controls](../running-an-analysis/start-and-monitor.md#table-controls).

| Column | Default View | Description |
|--------|:---:|-------------|
| SampleID | No | Sample_Name + Barcode |
| Sample_Name | Yes | User-provided name |
| Barcode | Yes | Barcode applied to the sample |
| Mix | No | Mix the variant is in |
| QC | Yes | QC flags for this variant. Multiple flags separated by `;`. See [Quality Control](../reference/results-description.md#quality-control). |
| Gene | Yes | Gene the variant is in. Overlapping transcripts show concatenated name (e.g., `CYP21A2_TNXB`). `HBA` for alpha-globin cluster amplicons that are not HBA1 or HBA2. |
| Variant | Yes | Variant call. cDNA change (HGVS) for SNVs/Indels; SV subtype for structural variants; CGG/AGG for FMR1; `.` for reference-matching variants (e.g., copy numbers). |
| Genotype | Yes | VCF-format phasing. `\|` = phased; `/` = unphased. For copy number SVs: `0/1` = single deletion; `./1` = single duplication; `1/1` = two duplications; `./.` = unknown. |
| Annotation | Yes | Pathogenicity (primarily from ClinVar). |
| VEP_Protein_Change | Yes | Predicted amino acid change in HGVS notation (from Ensembl VEP) |
| VEP_Consequence | Yes | Molecular consequence from Ensembl VEP |
| Amplicon | Yes | Amplicon the variant is on |
| Variant_Coordinates | Yes | GRCh38 coordinates. Full-amplicon SVs show `IMPRECISE`. |
| Amplicon_Coordinates | No | GRCh38 coordinates of the amplicon |
| Variant_Type | No | `SNV`, `Indel`, `STR`, or `SV` |
| Amplicon_Copies | No | Total copies of the amplicon |
| Variant_Info | Yes | Phasing information in gene context. For high-confidence HBA alpha-cluster or HS-40 genotypes, shows shorthand for affected endogenous control amplicons. |
| Annotation_Source | No | `ClinVar`, `ACMG`, or `Asuragen`. ClinVar annotations last updated 2025-12-03. |
| Clinvar_ID | No | ClinVar identifier |
| CLNSIGCONF | No | ClinVar confidence of annotation |
| CLNREVSTAT | No | ClinVar review status |
| CLNHGVS | No | ClinVar HGVS annotation |
| Transcript | No | MANE Select transcript |
| VEP_Impact | No | Genetic impact rating corresponding to VEP_Consequence |
| Read_Depth | Yes | Fully spanning read count for the amplicon |
| Fold_Change | Yes | Fold change of the amplicon (numerical only for amplicons with fold change calculation) |
| Normalized_Peak_Height | No | Normalized peak height for FMR1 alleles (0–1 range; FMR1 only) |
| RSID | No | dbSNP variant identifier |
| REF | No | Reference sequence (as in the .vcf) |
| ALT | No | Alternate sequence (as in the .vcf) |
| Summarized | Yes | Whether this variant is included in summarized views |

### Variant Figures

Rows that have an associated figure show a **chart icon** at the left of the row. Click it to open an interactive visualization for that variant in a new browser tab.

## Variants (.vcf)

Per-sample VCF files follow VCFv4.2 specifications.

**Access**

- **In the Software:** not currently viewable as a table, but the file can be opened or downloaded via **⋯ → View Files**. See [Download Results](download-results.md).
- **On disk:** `[Analysis Id]/results/sample_files/vcf/[SampleID]_variants.vcf`

**Standard VCF columns:**

| Column | Description |
|--------|-------------|
| CHROM | Sample_Name + Barcode |
| POS | Variant position |
| REF | Reference sequence |
| ALT | Alternate sequence (or symbolic allele: `<DEL>`, `<INS>`, `<DUP:TANDEM>`) |
| QUAL | Quality score from external tools, if available |
| FILTER | QC flags |
| INFO | Variant details (see INFO tags below) |
| FORMAT | Tag order for the Sample_Name column |
| Sample_Name | Sample-level data |

**Selected INFO tags:**

| Tag | Source | Description |
|-----|--------|-------------|
| ROI | Asuragen | Amplicon the variant is on |
| VARIANT_INFO | Asuragen | Gene-context phasing information |
| ANNOTATION_SOURCE | Asuragen | `ClinVar`, `ACMG`, or `Asuragen` |
| Variant_Type | Asuragen | `SNV`, `Indel`, `SV`, or `STR` |
| Gene | Asuragen | Gene the variant is in |
| Transcript | Asuragen | MANE Select transcript |
| CONFIDENCE | Asuragen | Estimated genotype call confidence (0–1) |
| Clinvar_ID | ClinVar | ClinVar identifier |
| ANNOTATION | ClinVar | Clinical significance |
| CLNHGVS | ClinVar | Top-level HGVS expression |
| CLNREVSTAT | ClinVar | ClinVar review status |
| CLNSIGCONF | ClinVar | Conflicting classifications |
| VEP_Consequence | VEP | VEP consequence |
| VARIANT | VEP | Variant cDNA |
| END | Sniffles | End position of variant |
| SVTYPE | Sniffles/Asuragen | Structural variant type |
| SUPPORT | Sniffles | Reads supporting the SV |
| AF | Sniffles | Allele frequency |

**FORMAT tags:**

| Tag | Description |
|-----|-------------|
| GT | Genotype |
| DP | Read depth |
| AF | Estimated allele frequency (0–1) |
| CN | Copy number genotype (imprecise events) |
| FC | Fold change of the amplicon |
| AG | Number of allele groups detected |
| GP | Read proportions of allele groups |
| GQ | Genotype quality |
| AD | Allelic depths for ref and alt alleles |
| PL | Phred-scaled genotype likelihoods |
| PS | Phase set identifier |
| PR | Normalized peak height (0–1; FMR1 only) |
| DR | Number of reference reads |
| DV | Number of variant reads |

## What's next

- [Variant Classes](../reference/results-description.md#variant-classes) — how SNVs/Indels, CNVs, and SVs are called.
- [Quality Control](../reference/results-description.md#quality-control) — QC flags and coverage reporting.
