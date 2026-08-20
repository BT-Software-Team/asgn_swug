# Software User Guide — Change Summary

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

---

**Not yet resolved / worth double-checking against your Word doc:**
- The CFTR redline you flagged earlier (about `c.2051_2052delinsG` / duplicated-amplicon `PHASE` flag wording) — I couldn't locate the "original" (pre-redline) text in any accessible repository; the guide already carries your "updated" wording. Flagging again here in case it points to a document I still haven't checked.
