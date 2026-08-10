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

## Considerations & Limitations

Per-gene target design, known artifacts, and variant-class-specific caveats. BED files with amplicon genomic coordinates are available from Asuragen for users who need them.

**All genes:** Detection of SNVs/Indels in or adjacent to homopolymers may have poor sensitivity due to sequencing technology limitations. Unless otherwise noted, phasing of variants across amplicons is not assumed.

### CFTR

- **General:** Covers all 27 exons and known pathogenic intronic variants across 19 amplicons (1–3 exons each). Annotations use transcript `ENST00000003084.11`. Phasing is only performed within a single amplicon, except contiguous multi-amplicon deletions, where it's inferred across amplicons. `c.350G>A` (R117H, Exon 4) and `c.1210-11T>G`-type Poly-T/TG variants (5′ of Exon 10) can't be phased against each other — different amplicons. Due to an alignment artifact, `c.2051_2052delinsG` displays correctly but appears as 2 distinct variants (`c.2052del`, `c.2052A>G`) in read pileups. SNVs in duplicated amplicons detected via allele deconvolution get a `PHASE` flag if the Genotype (e.g., `0|1|0`) can't be collapsed to match the copy number call (duplications aren't currently supported).
- **SV:** Performance is claimed for the single Exon19-20 amplicon deletion and deletions spanning ≥2 contiguous amplicons. All single-amplicon deletions are Analyzed by default, but only the Exon19-20 deletion is ever Summarized — other single-amplicon deletions are never Summarized regardless of filter settings. To exclude Exon19-20 deletions entirely, add `CFTR_Ex19-20_Deletion` to the Exclusion List. Copy number uses 4 endogenous controls (`EC`-prefixed) in stable genomic regions. Amplicon/exon duplications aren't detected. Full-gene deletions are Summarized as independent SVs per amplicon. Within-amplicon SVs >50 bp are Summarized by default.

### SMN1/2 (Mix A)

- **General:** Covers all 8 exons across 5 amplicons (1–3 exons each); the same primers amplify both SMN1 and SMN2. Annotations use transcripts `ENST00000380707.9` (SMN1) and `ENST00000380743.9` (SMN2). Sequence deconvolution groups reads for SNV/Indel and SV calling but not for copy number. The `c.840`-containing amplicon (Exon07-08) is assigned to SMN1 if the majority base is C, SMN2 if T; all other amplicons are always assigned to SMN1, since their PSVs don't reliably distinguish the paralogs.
- **SNV/Indel:** `SMN1: c.*3+80 T>G`, `SMN1: c.*211_*212del`, and `SMN2: c.859G>C` are Summarized by default when identified.
- **CNV:** Copy number is reported for samples with >3 copies, but performance was only established for the 0/1/2/≥3 categories (both genes). Uses 4 endogenous controls (`EC`-prefixed).
- **SV:** 15 PSVs within the Exon07-08 amplicon (identified via GRCh38 paralog alignment) drive "Hybrid" calling: SMN2 PSVs found in an SMN1 phase group (or vice versa) are flagged `Hybrid` in `Variant_Info`. Within-amplicon SVs >50 bp are Summarized by default.

### FMR1

- **General:** A single primer pair amplifies the CGG-repeat region of all alleles. Annotations use transcript `ENST00000370475.9`. CGG sizing is performed for alleles <250 repeats; larger alleles may still be sized from the read distribution, but sizing accuracy above 200 repeats hasn't been verified against orthogonal methods. If exact sizing fails for a full-mutation sample, it's reported as `>200CGG[noAGG]`. Relative mosaicism must be assessed manually from the normalized peak height. Alleles of the same size, or within ≤2 repeats of each other, may not be distinguished — manual review of the CGG read-depth histogram and AGG interrupt pattern can help. Total AGG interrupt count per allele has been performance-evaluated; interrupt location has not.

  Sizing precision:

  | CGG Repeat Range | Precision |
  |---|---|
  | 1–70 | ±1 |
  | 71–120 | ±3 |
  | 121–199 | ±5% |
  | ≥200 | N/A |

### HBA1/2

- **General:** HBA1 and HBA2 are each fully covered by one amplicon; 12 additional amplicons across the alpha cluster differentiate common structural variants. Annotations use transcripts `ENST00000320868.9` (HBA1) and `ENST00000251595.11` (HBA2). Cross-amplicon phasing is only assumed for the 10 common named SVs (see [Structural Variants](structural-variants.md#hba1-2-named-genotype-reference)). Because the alpha cluster region is poorly assembled in GRCh38, initial alignment uses the T2T assembly and reads are then re-mapped to GRCh38 for annotation. Sequence deconvolution identifies and phases variants within HBA1 and HBA2 individually — it isn't applied to other alpha-cluster amplicons or used for HBA1/2 copy number.
- **SNV/Indel:** Only reported for the HBA1 and HBA2 amplicons themselves. `HBA2:c.*93_*94del` (3′ UTR, pathogenic) causes dropout of the affected HBA2 amplicon, which appears as an HBA2 copy-number loss rather than as this variant directly.
- **SV:** The full alpha-cluster amplicon set differentiates 10 common SVs (MED-I, MED-II, THAI, FIL, alpha20.5, SEA, 4.2del, 3.7del, anti-4.2, anti-3.7); cross-amplicon phasing is assumed for these. Rare SVs can mimic one of these 10 due to amplicon design. An amplification pattern that doesn't match a known genotype is still reported (with individual HBA1/HBA2 copy numbers) if the confidence metric is ≥0.5 (see [Quality Control](quality-control.md)). Copy number normalization uses 4 endogenous controls within the alpha cluster (Rgn01, Rgn02, Rgn13, Rgn14) — the algorithm is generally robust to losing one, but per-amplicon copy number signal may be elevated as a result. HS-40 deletions and alpha-cluster deletions/duplications that affect these EC regions are still reported, with read counts artificially adjusted for the affected ECs when call confidence exceeds the model threshold; resulting HBA1/2 copy number changes are flagged `CNV_Warn` due to that adjustment. Within-amplicon SVs >50 bp are Summarized by default.

### HBB

- **General:** Full exon coverage via 2 amplicons (Exon01-02, Exon03). Annotations use transcript `ENST00000335295.4`. Neither amplicon undergoes sequence deconvolution, so low-frequency (≪50%) SNVs in duplicated amplicons may not be detected.
- **SV:** Uses the same endogenous controls as HBA1/2 CNV calling to detect full-amplicon deletions; exon duplications aren't detected. The algorithm is generally robust to losing one EC region, but per-amplicon signal may be elevated. If a full HBA alpha-cluster duplication is detected, HBB copy number (derived from the artificially-adjusted EC signal) is reported with `CNV_Warn` — accurate CN isn't claimed in that case, since HBA and HBB CN share the same EC regions. Reviewing variant phasing in IGV can help confirm results in this scenario. Within-amplicon SVs >50 bp are Summarized by default; very large within-amplicon deletions spanning most of an amplicon have been observed being called as complete amplicon deletions instead.

### CYP21A2

- **General:** A single long amplicon covers all of CYP21A2 and extends into TNXB exons 33–44; the same primers amplify the corresponding CYP21A1P-TNXA region. Annotations use transcripts `ENST00000644719.2` (CYP21A2) and `ENST00000342991.10` (CYP21A1P). ClinVar annotations for variants overlapping both CYP21A2 and TNXB transcripts share a ClinVar ID but get transcript-appropriate HGVS/consequence/pathogenicity info (pathogenicity is only assigned to the gene named in ClinVar's own "Identifiers" record). Allele groups are assigned to CYP21A2-TNXB vs. CYP21A1P-TNXA by majority-rule PSVs across the amplicon, except when CYP21A2 PSVs match a known fusion subtype (below), in which case the group is always assigned to CYP21A2-TNXB. PSVs are from [Concolino et al., 2018](https://pubmed.ncbi.nlm.nih.gov/29450859/).
- **CNV:** Gene copy number is inferred from the number of unique sequence deconvolution groups; a single allele group for both paralogs is inferred as one copy per paralog.
- **SV:** 11 PSV sites are evaluated (`c.92C>T`/P30L, `c.293-13C>G`/IVS2-13A/C>G, `c.332_339del`/G110Efs, `c.518T>A`/I172N, `c.710T>A`/I236N, `c.713T>A`/V237E, `c.719T>A`/M239K, `c.844G>T`/V281L, `c.923dup`/L307fx, `c.955C>T`/Q318X, `c.1069C>T`/R356W), each assigned `P` (pseudogene), `G` (gene), or `A` (alternate). Concatenating these calls in genomic order and matching against known patterns assigns a fusion subtype:

  | Subtype | Pattern(s) |
  |---|---|
  | CH-1 | `PPPGGGGGGGGGGGGGG` |
  | CH-2 | `PPPPGGGGGGGGGGGGG` |
  | CH-3 | `PPPPPPPPPPGGGGGGG` |
  | CH-4 | `PGGGGGGGGGGGGGGGG` or `PAGGGGGGGGGGGGGGG` |
  | CH-5 | `PPPPPPPPPGGGGGGGG` or `PPPPPPPGPGGGGGGGG` |
  | CH-6 | `PPGGGGGGGGGGGGGGG` |
  | CH-7 | `PPPPPPPGGGGGGGGGG` |
  | CH-8 | `PPPPPPPPPPPGGGGGG` |
  | CH-9 | `PGGGGGGGGGGGGGGGG` or `PAGGGGGGGGGGGGGGG` |

  A PSV pattern that doesn't match any subtype is reported as a microconversion instead. Within-amplicon SVs >50 bp are Summarized by default.

### TNXB

- **General:** Shares CYP21A2's long amplicon (extends into TNXB exons 33–44) and the CYP21A1P-TNXA primer set. Annotations use transcript `ENST00000644971.2`. ClinVar annotation-sharing behavior and paralog assignment logic are the same as described under [CYP21A2](#cyp21a2).
- **CNV:** A single unique allele group for both paralogs is *not* assumed to mean 2 copies.
- **SV:** TNXB/A fusions aren't assigned named subtypes. A PSV position list was generated from all distinguishing positions across GRCh38 TNXB exons 32–44; any PSV match is reported as a microconversion. Within-amplicon SVs >50 bp are Summarized by default.

### GBA1

- **General:** A single long amplicon covers the entire gene. GBA1 and GBAP1 share a forward primer but have different reverse primers. Annotations use transcripts `ENST00000368373.8` (GBA1) and `ENST00000486869.5` (GBAP1). Allele groups are assigned to GBA1 or GBAP1 by highest sequence similarity to the paralogous reference region.
- **CNV:** Paralog alleles are often homologous in size — a single similarly-sized allele group for both paralogs is reported as 2 copies each.
- **SV:** PSVs are from [Toffoli et al., 2022](https://pubmed.ncbi.nlm.nih.gov/35794204/); any PSV match is reported as a microconversion. Within-amplicon SVs >50 bp are Summarized by default.

### F8

- **General:** A 4-primer system amplifies wild-type and intron 1 inversion alleles; a 3-primer system covers intron 22. Annotations use transcript `ENST00000360256.9`.
- **SV:** Intron inversions are reported as `INVERSION, Pathogenic`; non-inversion SVs are reported as `SV`. Inversions where zygosity assignment failed (potentially due to concomitant homologous-site duplication plus flanking-region deletion) are still reported as `INVERSION, Pathogenic`, but listed as unphased.

### SMN1/2 (Mix D)

- **General:** A single long amplicon covers SMN1 exons 3–8 for long-range phasing; the same primers amplify SMN2. Annotations use transcripts `ENST00000380707.9` (SMN1) and `ENST00000380743.9` (SMN2).
- **SNV/Indel:** Sequence deconvolution identifies and phases SNV/Indels and SVs to a specific gene, but copy number isn't detected or reported from Mix D.
- **SV:** Within-amplicon SVs >50 bp are Summarized by default.

## What's next

- [Quality Control](quality-control.md) — QC flags and coverage reporting.
- [Genotype Summary](../analysis-results/genotype-summary.md) — where this notation appears.
