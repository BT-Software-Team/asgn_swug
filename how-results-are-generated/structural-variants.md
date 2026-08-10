# Structural Variants

Larger genomic rearrangements including deletions/insertions >50 bp, inversions, fusions, and microconversions.

**Applicable genes:** All genes in the panel

**Method:** Within-amplicon SVs are detected using [Sniffles2](https://github.com/fritzsedlazeck/Sniffles). Full-amplicon SVs (copy number changes) are detected by fold-change analysis. Inversions (F8 introns 1 and 22) are detected using a multi-primer system. Gene fusions and microconversions (CYP21A2/A1P, GBA1/P, TNXB/A) are detected by evaluating paralog-specific variant (PSV) patterns.

## Exon/Amplicon Deletions and Duplications

A deletion/duplication call means a specific exon or amplicon has fewer or more than 2 copies. These use the statistical models described in [Copy Number Variants](copy-number-variants.md#method-1-covariance-model).

**Applicable genes/genotypes:** CFTR, HBA1/2, HBB, SMN1/2, HBA alpha-globin cluster deletions/duplications, HBA HS-40 deletions

Reporting takes one of two forms, depending on the gene:

- **Exon/amplicon-level output** — the gene and the specific exon/amplicon affected are reported directly in the genotype and variant outputs; the gene is summarized as having a structural variant (SV).
- **Named genotype (canonical structural variant) output** — some genes have amplicon-alteration combinations that map to a specific named genotype from the literature. In these cases the named genotype is reported, with the individual affected amplicons marked in Variant Results. HBA1/2's named genotypes are listed below (Table 11). If an observed HBA1/2 amplification pattern doesn't match a known named genotype, it falls back to exon/amplicon-level output (only HBA1 and HBA2 copy numbers are then reported) — a "non-canonical" structural variant.

### HBA1/2 Named-Genotype Reference {#hba1-2-named-genotype-reference}

Copy number per amplicon/region for each of the 10 common HBA1/2 structural variants the Software distinguishes. `aa` = wild-type. THAI/SEA = deletions observed in Southeast Asia (incl. Thailand); MED-I/MED-II = deletions observed in the Mediterranean; FIL = Filipino deletion; alpha20.5 = 20.5 kb deletion causing loss of both HBA1/2; 4.2del/3.7del = deletions causing loss of HBA2; anti4.2/anti3.7 = copy gain of HBA2.

| Genotype | Rgn01 | Rgn02 | Rgn03 | Rgn04 | Rgn05 | HBA2 (Rgn06) | Rgn07 | HBA1 (Rgn08) | Rgn09 | Rgn10 | Rgn11 | Rgn12 | Rgn13 | Rgn14 |
|----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| aa/aa | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| THAI/aa | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 | 2 |
| MED-II/aa | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 | 2 |
| FIL/aa | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 |
| alpha20.5/aa | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 | 2 | 2 |
| MED-I/aa | 2 | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 | 2 |
| SEA/aa | 2 | 2 | 2 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 2 |
| 4.2del/aa | 2 | 2 | 2 | 2 | 1 | 1 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| 3.7del/aa | 2 | 2 | 2 | 2 | 2 | 1 | 1 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| anti4.2/aa | 2 | 2 | 2 | 2 | 3 | 3 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| anti3.7/aa | 2 | 2 | 2 | 2 | 2 | 3 | 3 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |

> Alpha-globin cluster duplications/deletions, while expected to affect HBA1/2 copy number, aren't listed here — they can't be detected in combination with other genotypes, and HBA1/2 copy number accuracy isn't claimed when they're present (they affect the endogenous control regions Rgn01, Rgn02, Rgn13, Rgn14 used for HBA1/2 CN calling).

## Inversions

Inversions are large structural variants where portions of the genome invert during recombination, causing the antisense strand to become the sense strand.

**Applicable genes:** F8

**Method:** PCR enrichment and sequencing of the wild-type regions and the homologous recombination-site regions captures reads aligning to both. Both WT and synthetic fusion contigs are used as alignment references, assigning each read to either the WT or inversion amplicon. Zygosity is assigned from the proportion of reads aligning to the inversion amplicon vs. the corresponding WT amplicon: **>0.1 → heterozygous**, **>0.8 → homozygous**.

**F8-specific:** Intron 1 and intron 22 inversions are evaluated independently. Intron 1 is covered by two WT amplicons (H1, H2); intron 22 by a single WT amplicon (H1). Because intron 1 has two WT amplicons, its zygosity call depends on both H1 and H2 read fractions relative to the inversion — deviations from the expected pattern can produce an "Unknown" zygosity call (`./.`).

## Fusions

**Applicable genes:** SMN1/2, CYP21A2, TNXB, GBA1

**Method:** Detects gene–pseudogene fusions arising from homologous recombination, identified by paralog-specific variants (PSVs) occurring in specific combinations. The PSVs that distinguish each gene from its pseudogene were identified via literature review or paralog alignment (see each gene's entry in [Gene-Specific Details](gene-specific-details.md) for its PSV list). The presence/absence pattern of these PSVs across a sample's allele groups (see Structural Variants Within Amplicons, below) determines whether a fusion, microconversion, or hybrid is called — CYP21A2/A1P additionally has 9 recognized named fusion subtypes (CH-1 through CH-9), documented in its [Gene-Specific Details](gene-specific-details.md#cyp21a2) entry.

## Structural Variants Within Amplicons

Identifies insertions, deletions, inversions, and translocations larger than 50 bp from sequencing data.

**Applicable genes:** CFTR, SMN1/2 (Mix A), HBA1/2, HBB, CYP21A2, TNXB, GBA1

**Method:** Structural variants are identified with [Sniffles2](https://github.com/fritzsedlazeck/Sniffles), an alignment-based SV caller for long-read sequencing. The pipeline is alignment → allele grouping via [sequence deconvolution](overview.md#sequence-deconvolution) → SV calling with Sniffles2. A minimum allele frequency threshold of 0.2 and a coverage filter of 20 reads are used to filter likely false positives; calls falling outside an amplicon's range are also filtered out.

## What's next

- [SNVs/Indels](snvs-indels.md), [Copy Number Variants](copy-number-variants.md), [Short Tandem Repeats](short-tandem-repeats.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
