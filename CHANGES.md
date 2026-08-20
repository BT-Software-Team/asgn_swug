# Software User Guide — Change Summary

**Version 5** · Every guide-content commit through `1d63f77` (2026-08-20) is reflected below. (Commits that only touch this file, like the one introducing this line, don't need an entry.) To check for drift: `git log 1d63f77..HEAD -- . ':!CHANGES.md'` on `asgn_swug` — any output means new guide changes have landed since this was last updated.

Covers every substantive change made to the guide since the initial port of `SWUG_Working.docx` into markdown (commits `a2682df`/`52e53fa`, 2026-07-15 — baseline, not itemized below). Organized by *why* the change was made, not chronologically. Commit hashes are given for traceability back into `asgn_swug`.

---

## 1. New Application Functionality Reflected in the Guide

Changes made because the app itself now works differently than the original document described — new screens, renamed flows, or UI elements that didn't exist when the original was written.

- **Table toolbars and navigation** updated to match the current UI throughout the guide (`195a287`).
- **Managing Your Team** — new section documenting the team/user management feature (add user, manage user, reset password, roles & permissions) (`195a287`).
- **Signing In** — new page documenting the OIDC auto-redirect login flow, the sign-in form, and the first-login temporary-password reset flow (`e517463`).
- **Analysis Configuration** (formerly "System Configuration") — new Configuration Overview page introducing the gear-icon settings modal as its own concept (`1389599`).
- **Panel Filters** — rewritten for the current flow: list view, read-only summary view, separate edit/create forms (`0b2e441`).
- **Create an Analysis** — rewritten for the current dialog: toolbar plus-icon entry point, pipeline picker, dataset import table (`6639e83`).
- **Configure an Analysis** — rewritten for the current flow: auto-applied panel filter, file-card sample sheet upload, Start Analysis button behavior (`c51f1b4`).
- **Start & Monitor Execution** — Track Status and Row Actions rewritten around the per-row `⋯` overflow menu, replacing the old action model (`9657040`).
- **Quick Start** — steps refreshed to match the current UI end-to-end (`d7f6c16`).
- **Analysis Results** — toolbar buttons, breadcrumbs, the variant figure icon, and the download dialog rewritten to match the current UI (`7e56f2e`).

## 2. New Information Added for Clarity

Changes that didn't track an app change — reorganizing, explaining, or restoring detail to make the guide easier to use and understand.

- **Journey-based information architecture** — initial restructure of the whole guide around the user's workflow instead of the original document's ordering; broken links fixed (`85403d9`).
- **Quick Start** and **Contact Support** — new onboarding pages with no equivalent in the original document (`dc30235`).
- **Data Sources** — local vs. remote data source types clarified (`a709eaf`); Windows-to-Linux path conversion broken into explicit steps (`80640e7`); import patterns explained piece-by-piece with the default RegEx patterns broken down (`eab911b`); SSH account guidance and the non-C-drive symlink workaround pulled into their own sections (`78f9b69`).
- **Panel Filters** — explained that Mixes in the GENE FILTER tab correspond directly to the physical Carrier Plus kits (`ba27774`).
- **Configure an Analysis** — split into per-task sections for panel filter vs. sample sheet assignment (`fc966a2`); sample sheet requirements moved under "Assign a Sample Sheet" for proximity to where they're used (`aa9c4ce`); the sample sheet review step trimmed to a legend, with full error detail consolidated on the errors page (`933ea3f`).
- **Overview** — new Default File Locations reference table for Linux and Windows (`414bce5`).
- **Start & Monitor Execution** — redundant "suggested actions" removed from the status table now that Row Actions covers it, avoiding two sources of truth (`8bfedfe`).
- **Post-run Basecalling** — version scope and Protocol Guide reference restored (`29eae2a`, see also §3).
- **Analysis Results** — Sample Summary, Genotype Summary, and Variant Results split out of the old monolithic "Results Description" reference page into their own dedicated pages (`2bf2727`).
- **Sample Sheet Errors** and **Data Source Errors** — new dedicated pages consolidating error content that was previously scattered; MinKNOW Warnings moved to the page where it actually appears (`6f11882`, `fa40b78`).
- **How Results Are Generated** — new top-level section promoting Variant Classes and Quality Control out of the generic "Reference" section, and surfaced from the Analysis Results overview for discoverability (`5a23734`, `3013364`).
- **Variant-calling methodology restored from the original specification** — Method 1/2 statistical models (Covariance/Logistic Regression), the HBA1/2 named-genotype reference table, per-gene limitations, and the QC analysis-identifier reference, all present in `SWUG_Working.docx` but lost before ever reaching git (`52cae83`).

## 3. Changes Based on Redline / Review Comments

Direct corrections driven by your feedback — either given in conversation or made via GitBook sync.

- **Whole-document audit fixes** — stale/incomplete third-party dependency versions and ClinVar/VEP classification term tables corrected (`cc907f1`); remaining Medium/Low findings from that audit filled in (`69170fb`).
- **Post-run Basecalling** — `report.json` warning behavior corrected to match actual behavior (`29eae2a`).
- **"Analyze vs. Summarize" table simplified** — removed the confusing "Neither" column; split into two clear columns, **Passes Analyze** / **Passes Summarize** (`37f7fd5`).
- **"Always Analyzed and Summarized" claim corrected** — reworded so it no longer implies fixed, unchangeable behavior, since these variant types are now exposed as ordinary (editable) filter settings (`da3f095`).
- **Compatible Kits table** — Mix moved out of the parenthetical into its own column (`15eaf50`).
- **"System Configuration" renamed to "Analysis Configuration"** throughout the guide, per your direct instruction (`c4f22a2`).
- **Data Sources page reordered** — "Open Analysis Configuration" steps moved to the top of the page, matching your Word-doc edit (`c4f22a2`).
- **"Variant Classes" and "Variant Reports" restored as sub-categories** — nested under How Results Are Generated / Gene-Specific Details after you raised that these might be valuable industry terms worth keeping findable (`c4f22a2`).
- **Panel Filter Configurations — three fixes from your last-draft comments** (`4c2b21e`):
  - "Variant Lists: No inclusion or exclusion lists are defined by default" corrected — the Default Filter does define default inclusion/exclusion lists; the bullet now points to the Default Filter behavior section.
  - Variant Lists tab intro no longer implies HGVS identifiers are the only supported entry format.
  - "Variant list file format" now documents column-value expressions (e.g., `Variant_Type=SNV`, `Read_Depth>10`) alongside HGVS format, with a link to Variant Results for the available columns.
- **Analyze vs. Summarize — detail restored from the original spec, two dead links fixed** (`2e65d82`):
  - Restored the rationale for the two-tier Analyze/Summarize design (why variants get "elevated" into summaries).
  - Restored a plain-English walkthrough of the inclusion-logic formula, reworded so "Default" ties to the active filter's editable inclusion list rather than implying fixed/hardcoded behavior.
  - Restored the Sample Summary row's "additional gene-specific rules" qualifier as a table footnote, without reintroducing the removed "Neither" column.
  - Fixed two dead/stale references: the logic walkthrough now links to the real "Default Filter behavior" anchor instead of a section name that no longer exists, and Sample Summary's dangling "(Table 4 in the source guide)" citation now links to its own gene-rules section.
- **CFTR gene name restored in the duplicated-amplicon `PHASE` flag sentence** (`78f3adf`) — "SNVs in duplicated amplicons" → "SNVs in duplicated CFTR amplicons." HBB's own entry uses the same "duplicated amplicons" phrasing in its own context, so the CFTR sentence read as a general claim once the gene name was dropped. Caught via cross-check against the old Carrier Plus guide (00003932v4) while auditing the Word document against this repo.
- **Troubleshooting by Observation — mechanisms and thresholds restored** (`1ecafd0`), confirmed missing against the old Carrier Plus guide (00003932v4) via the Word-doc handoff: why `BUILTIN\Administrators` is required for the scheduled task; the CalGT use case (Mix A/C issues from sample type/isolation method) plus a pointer to QC Flags Reference for the exact genotype requirements; the NTC `>2 ng/µL` measurement condition; the Amplicon-level LowCov evaporation mechanism (alters PCR efficiency/bead ratios/size selection); Qubit incubation time as a cause of uneven read distribution; and the alpha-cluster duplication mechanism (excessive sequence deconvolution allele groups). Rows that already carried this detail were left alone — see conversation for the row-by-row verification.
- **Troubleshooting by Observation — full table restored from Table 17** (`1d63f77`), sourced directly from the Word user guide (doc 00003932/25-007) rather than reconstructed from partial comparisons. 11 rows → 15 rows: Amplicon-level LowCov and Sample-level QC fail split into one row per cause/action pair (your call, over collapsing into bulleted single-row cells); CalLowCov links to the sample-sheet-requirements section; CalGT and LowConfidence both gained the "classified as `calibrator` in the sample sheet" instruction; LowConfidence/FC gained the "repeat isolation" escalation step (previously left out pending confirmation — now confirmed from source); NTC and alpha-cluster duplication both gained the "don't contaminate the NTC well before barcode PCR" action; uneven read distribution gained instrument error and the Mix D longer-amplicon detail; high-frequency LowCov's causes were made more granular (mass-calculation, pooling-ratio, and volumetric-ratio errors as distinct items); and Protocol Guide references now point at the specific "Start Sequencing" section where applicable.

---

**Resolved:**
- The CFTR redline about `c.2051_2052delinsG` / duplicated-amplicon `PHASE` flag wording — the "original" (pre-redline) text was never in git; it lives only in the old Carrier Plus guide (00003932v4), the ancestor of the section that was rewritten here. The guide already carries the "updated" wording; the only actual gap found was the dropped "CFTR" qualifier, fixed above.
- **Register/house style — decided.** The guide keeps contractions (`can't`, `doesn't`, `aren't`, etc.), the style already applied consistently throughout. There is no DHF house-style rule requiring formal (non-contraction) language. This was raised independently on both sides of the repo/doc sync — recording the decision here so it isn't re-litigated in a future audit pass.
