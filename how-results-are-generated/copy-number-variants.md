# Copy Number Variants

Copy number changes at the amplicon or gene level.

**Applicable genes:** CFTR (exon-level deletions), SMN1/2, HBA1/2, HBB, CYP21A2, GBA1, TNXB

**Method:** Copy number is inferred from fold-change calculations using calibrated, normalized read counts relative to endogenous control regions. For genes with paralogs (SMN1/2, CYP21A2/A1P, GBA1/P, TNXB/A), [sequence deconvolution](overview.md#sequence-deconvolution) is used to assign alleles to the correct paralog before copy number calculation. For CYP21A2, TNXB, and GBA1, copy number is estimated from the relative read count and group count of each paralog's sequence deconvolution groups. For SMN1/2 (Mix A), copy number is assessed from the `c.840`-containing amplicon (Exon07-08) using the covariance model below.

**Output:** Copy number results appear in the `Amplicon_Copies` column and are reported as SVs in the Genotype column (`0/1` for deletion, `./1` for duplication).

Amplicon- and exon-level deletion/duplication calls (copies ≠ 2 for a specific exon or amplicon) use one of two statistical models, trained per gene:

## Method 1 — Covariance Model {#method-1-covariance-model}

A per-gene model is built from training data capturing the covariation of normalized fold change across multiple exons/amplicons — normalized using fully-spanning-read coverage of endogenous controls and calibrators sequenced alongside the sample. A multivariate normal distribution is constructed per copy-number combination, and multivariate probabilistic inference against the resulting covariance matrix identifies deletion/duplication events.

**Used for:** CFTR, SMN1/2, HBA1/2

A related multivariate probabilistic inference method — using a reduced set of Mix C endogenous controls, scaled by the trained likelihood of the minimum allele proportion expected per combination (from sequence deconvolution grouping) — is used specifically for **HBA HS-40 deletions**.

## Method 2 — Logistic Regression Model

A normalized fold-change model is built per amplicon using endogenous controls and calibrators sequenced with the sample. A logistic regression model, trained on normalized fold change against known outcomes, predicts deletion or duplication for each amplicon.

**Used for:** HBB

For alpha-globin cluster duplications/deletions specifically, a distinct logistic regression model adds the sequence deconvolution group count of sentinel amplicons to the normalized HBB fold-change features.

**Used for:** HBA alpha-globin cluster duplications and deletions

## What's next

- [SNVs/Indels](snvs-indels.md), [Structural Variants](structural-variants.md), [Short Tandem Repeats](short-tandem-repeats.md) — the other variant classes.
- [Gene-Specific Details](gene-specific-details.md) — per-gene notation and considerations.
