# Panel Filter Configurations

Panel filters control which variants are included in analysis results. The Software ships with a default filter, and you can create custom configurations to support specific use cases.

> **Note:** Panel filter configurations cannot be deleted from the user interface. To delete a config, navigate to `[User Data Path]\panels\custom\` and delete the relevant JSON file. This requires administrator permissions.

> **Note:** Custom panel filter configurations are deleted when the Software is uninstalled. If you need them for future use, copy the files from `[User Data Path]\panels\custom\` to another location before uninstalling.

---

## Analyze vs. Summarize

Panel filters operate at two levels:

- **Analyze** — variants included in variant-level results ([Variant Results](../analysis-results/variant-results.md))
- **Summarize** — a subset of Analyzed variants that are also included in gene-level ([Genotype Summary](../analysis-results/genotype-summary.md)) and sample-level ([Sample Summary](../analysis-results/sample-summary.md)) results

The Summarize filter is always a subset of the Analyze filter — this is enforced in the UI. A variant that passes neither filter does not appear in any result.

The table below shows where a variant appears depending on which filter it passes:

| File / View | Passes Analyze | Passes Summarize |
|-------------|----------------|------------------|
| Variant Results view / `variants.csv` | Included | Included |
| `[Sample]_VARIANTS.CSV` / `.VCF` | Included | Included |
| Genotype Summary view / `genotypes_summary.csv` | Not included | Included |
| Sample Summary view / `sample_summary.csv` | Not included | Included |

### Inclusion logic

For a variant to appear in Analyze or Summarize results, the following must be true:

```
Gene AND EITHER (Default OR ((ClinVar Classification OR VEP OR Inclusion List) NOT Exclusion List))
```

### Default Filter behavior

Some variant types — Structural Variants, Short Tandem Repeats, and specific SMN1/SMN2 variants — are not classified by the ClinVar or VEP settings. The Default Filter covers them through its variant inclusion and exclusion lists. These are ordinary filter settings, not fixed behavior: a custom panel filter can change them on the [VARIANT LISTS tab](#variant-lists-tab-optional).

**Analysis.** The Default Filter selects all genes, all ClinVar classifications, and all VEP consequences, and its inclusion list adds `Variant_Type=SV` and `Variant_Type=STR`. Every called variant is therefore Analyzed, including Structural Variants (SVs, including copy number variants) and Short Tandem Repeats (STRs). Its exclusion list is empty.

**Summaries.** The Default Filter selects ClinVar **pathogenic** and **likely pathogenic** and VEP **nonsense**. Its inclusion list adds the following regardless of classification:

| Inclusion list entry | Effect |
|----------------------|--------|
| `SMN1:c.*3+80T>G`, `SMN1:c.*211_*212del` | The two SMN1 Linked Variants (LVs) |
| `SMN2:c.859G>C` | The SMN2 disease modifier variant |
| `Variant_Info%5T` | `5T` poly-T tract variants |
| `Variant_Type=SV` | All Structural Variants |
| `Gene!=CFTR&Variant_Type=STR` | Short Tandem Repeats in every gene except CFTR |

Its exclusion list removes the following exon-deletion subtypes, which overrides the `Variant_Type=SV` inclusion above:

```
Variant_Info=deleEx01, Variant_Info=deleEx02, Variant_Info=deleEx03, Variant_Info=deleEx04,
Variant_Info=deleEx05Ex07, Variant_Info=deleEx08Ex09, Variant_Info=deleEx10, Variant_Info=deleEx11,
Variant_Info=deleEx12Ex13, Variant_Info=deleEx14, Variant_Info=deleEx15, Variant_Info=deleEx16Ex17,
Variant_Info=deleEx18, Variant_Info=deleEx21, Variant_Info=deleEx22, Variant_Info=deleEx23,
Variant_Info=deleEx24, Variant_Info=deleEx25Ex27
```

---

## Open Panel Filters

1. From the **Analysis Dashboard**, click the **gear icon** in the table toolbar to open **Analysis Configuration**.
2. Select **Panel Filters** in the left-hand menu.

The Panel Filters screen lists:

| Section | Contents |
|---------|----------|
| **Add new panel filter** | The **Create New** button. |
| **Default Filter** | The filter that ships with the Software. It can be viewed but not changed. |
| **Custom Panel Filters** | Every configuration you have created. |

Click any filter in either list to open it.

---

## View a Panel Filter

Clicking a filter opens a **read-only summary** of that configuration — a single scrollable page, not the editing form. The filter name appears in the breadcrumb at the top (**Panel Filters / your-filter-name**); the back arrow beside it returns to the list.

The summary is organized into these sections:

| Section | What it shows |
|---------|--------------|
| **Configuration Details** | The filter's **Name**. |
| **Gene Targets** | Four counts across the top — **Kits selected**, **Genes analyzed**, **Summaries on**, **Genes excluded** — followed by one card per selected Mix listing its genes. |
| **ClinVar Classifications** | The classifications included in analysis. Shows *No ClinVar Classifications Selected* if none are set. |
| **Variant Effect** | The VEP consequences included in analysis. Shows *No Variant Effects Selected* if none are set. |
| **Variant Lists** | The exclusion and inclusion lists, listed separately for **Analysis** and for **Summaries**. |

**Reading the gene cards.** Each Mix card is headed by the Mix name and a count — `9 genes` when every gene is on, or `3 of 9 genes` when some are off. Within the card, each gene shows:

| Indicator | Meaning |
|-----------|---------|
| Blue check + full-strength text | The gene is **analyzed** — its variants appear in variant-level results. |
| Grey dash + dimmed text | The gene is **not analyzed** — it is excluded from this filter entirely. |
| **Summary on** (blue, at right) | The gene is also **summarized** — it appears in gene-level and sample-level results. |
| **Summary off** (grey, at right) | The gene is analyzed but not summarized. |

**Reading the counts.** *Kits selected* is how many Mixes the filter covers. *Genes analyzed* and *Summaries on* count genes within those selected Mixes. *Genes excluded* counts every gene across **all** kits that this filter does not analyze — so genes belonging to a Mix you did not select are counted as excluded.

To change anything on this page, click the **edit (pencil) icon** in the upper-right corner. The page becomes the editing form described below.

> The **Default Filter** opens in this same summary view, and its Mix and gene selections cannot be edited. To work from the defaults, create a new filter instead — a new configuration starts out pre-filled with the default filter's settings.

**Default filter settings:**

- **Gene Targets:** All Mixes and all genes are selected for both Analyze and Summarize.
- **ClinVar:** All categories are Analyzed; only Pathogenic and Likely Pathogenic are Summarized.
- **Variant Effect:** All consequences are Analyzed; only `nonsense` is Summarized.
- **Variant Lists:** No inclusion or exclusion lists are defined by default.

---

## Create a New Panel Filter Configuration

1. From the **Analysis Dashboard**, click the **gear icon** in the table toolbar to open **Analysis Configuration**.
2. Select **Panel Filters** in the left-hand menu.
3. Under **Add new panel filter**, click **Create New**.
4. Enter a name in **Panel Filter Title**. Names may contain letters, numbers, periods, dashes, and underscores only.
5. Work through the tabs below, then click **Save Filters**.

> A new configuration opens pre-filled with the default filter's settings, so you only need to change what differs from the defaults.
>
> **Cancel** discards the configuration. Because unsaved work is lost, you are asked to confirm first.

### GENE FILTER tab (required)

#### What a Mix is

The left pane of this tab lists **Mix A**, **Mix B**, **Mix C**, and **Mix D**. A Mix corresponds directly to the AmplideX Nanopore Carrier Plus kit used to prepare the library — the kit you ordered and ran at the bench determines which Mix your data belongs to:

| Kit | Reference | Appears here as |
|-----|-----------|-----------------|
| AmplideX Nanopore Carrier Plus Kit A | A00627 | **Mix A** |
| AmplideX Nanopore Carrier Plus Kit B | A00628 | **Mix B** |
| AmplideX Nanopore Carrier Plus Kit C | A00629 | **Mix C** |
| AmplideX Nanopore Carrier Plus Kit D | A00630 | **Mix D** |

Each kit targets its own set of genes, so selecting a Mix here determines which gene targets appear in the right pane. Selecting Mix A shows the genes covered by Kit A, and so on.

**Which Mixes should you select?** The ones matching the kits you used. If your runs are prepared with Kit A and Kit D, select Mix A and Mix D. Leaving all four selected — as the default filter does — is also fine: a Mix you did not run simply contributes no results.

> A gene covered by more than one kit appears under each of those Mixes, and the two entries are configured independently. For example, SMN1 and SMN2 appear under both Mix A and Mix D, and what each kit reports for them differs — see [Which Variants Appear in Sample Summary (by Gene)](../analysis-results/sample-summary.md#which-variants-appear-in-sample-summary-by-gene) for the specifics.

The Mixes included in a given run are recorded with the analysis and shown in your results — see the `Mixes` field in [Configure an Analysis](../running-an-analysis/configure-an-analysis.md).

> The interface uses both terms: the selectable chips are labeled **Mix A**–**Mix D**, while the summary view counts them as **Kits selected**. They refer to the same thing.

#### Set the filters

This tab is headed **Gene Target Configuration**.

1. Click the **Mix chips** at the top to select the Mixes this filter covers. A selected chip is filled in; click it again to deselect. Until at least one is selected, the tab shows *Select a kit above to configure gene targets*.
2. The **Included in analysis** panel below lists the genes for each selected Mix, with a running count in its header (`4 of 9 genes · 2 summaries on`).
3. For each gene, use its checkbox to include it in **analysis** — its variants appear in variant-level results.
4. For each analyzed gene, use its **Summary** toggle to also include it in **summary** results at the gene and sample level. A gene that is not analyzed cannot be summarized.
5. Use **Toggle all summaries** in the panel header to turn every summary on at once, and **Deselect all summaries** to turn them all off.

### CLINVAR tab (optional)

Select which ClinVar variant classifications are included in Analyze and Summarize results. If no selections are made, variants are not filtered by ClinVar classification.

| Classification | Grouped ClinVar Terms |
|---------------|-----------------------|
| **pathogenic** | Pathogenic, Pathogenic\|drug_response, Pathogenic\|risk_factor, Pathogenic/Likely_pathogenic, Pathogenic/Likely_pathogenic\|risk_factor, Pathogenic\|other, Pathogenic/Likely_pathogenic\|other |
| **likely pathogenic** | Likely_pathogenic, Pathogenic/Likely_pathogenic, Pathogenic/Likely_pathogenic\|risk_factor, Pathogenic/Likely_pathogenic\|other |
| **uncertain significance** | Uncertain_significance, not_provided, no_classification_for_the_single_variant, Uncertain_significance\|other |
| **likely benign** | Likely_benign, Benign/Likely_benign\|risk_factor, Benign/Likely_benign |
| **benign** | Benign, Benign/Likely_benign\|risk_factor, Benign/Likely_benign |
| **conflicting** | Conflicting_interpretations_of_pathogenicity, Conflicting_interpretations_of_pathogenicity\|risk_factor, Conflicting_classifications_of_pathogenicity, Conflicting_classifications_of_pathogenicity\|other, Conflicting_classifications_of_pathogenicity\|risk_factor |
| **drug response** | drug_response, Pathogenic\|drug_response, Benign/Likely_benign\|drug_response |
| **risk factor** | risk_factor, Pathogenic\|risk_factor, Pathogenic/Likely_pathogenic\|risk_factor, Benign/Likely_benign\|risk_factor, Conflicting_*\|risk_factor |
| **other** | Pathogenic\|other, other, Conflicting_classifications_of_pathogenicity\|other. Includes unannotated variants. |
| **all** | Reports all variants regardless of ClinVar status |
| **annotated** | All variants present in ClinVar. Excludes unannotated variants (assigned `.` or `N/A`). Note: includes benign/likely benign variants. |

For all categories except `all`, `annotated`, `conflicting`, and `other`, terms are unchanged from [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/docs/clinsig/).

### VARIANT EFFECT tab (optional)

Select which VEP consequences are included. Based on [Ensembl VEP](https://useast.ensembl.org/info/genome/variation/prediction/predicted_data.html).

| VEP Consequence | Grouped Terms |
|----------------|---------------|
| **nonsense** | stop_gained |
| **frameshift** | frameshift_variant |
| **splice** | splice_acceptor_variant, splice_donor_variant, splice_donor_5th_base_variant, splice_region_variant, splice_donor_region_variant, splice_polypyrimidine_tract_variant |
| **missense** | missense_variant, stop_lost, start_lost, inframe_deletion, inframe_insertion, incomplete_terminal_codon_variant |
| **silent** | start_retained_variant, stop_retained_variant, synonymous_variant |
| **intronic** | intron_variant |
| **intergenic** | intergenic_variant |
| **other_high_impact** | transcript_amplification, transcript_ablation, feature_elongation, feature_truncation |
| **other_non_high_impact** | protein_altering_variant, coding_sequence_variant, mature_miRNA_variant, 5_prime_UTR_variant, 3_prime_UTR_variant, non_coding_transcript_exon_variant, NMD_transcript_variant, coding_transcript_variant, upstream_gene_variant, downstream_gene_variant, TFBS_ablation, TFBS_amplification, TF_binding_site_variant, regulatory_region_ablation, regulatory_region_amplification, regulatory_region_variant, sequence_variant, non_coding_transcript_variant. Includes unannotated variants. |
| **annotated** | All VEP terms except `.` and `N/A` |

### VARIANT LISTS tab (optional)

Use this tab to include or exclude specific variants by HGVS identifier. If no lists are provided, all variants passing the Gene, ClinVar, and VEP filters are included.

The tab has two groups — **Analysis** and **Summaries** — each with its own **Exclusion list** and **Inclusion list**. The four lists are independent: excluding a variant from Summaries does not remove it from Analysis results. Each list is seeded with the [Default Filter](#default-filter-behavior) values and can be restored to them.

**Exclusion list:** variants excluded from that level regardless of other settings. The exclusion list overrides the inclusion list.

**Inclusion list:** variants included at that level regardless of ClinVar/VEP settings. Gene-level Analyze/Summarize toggles still apply — a variant won't be included if its gene is not toggled on.

#### Variant list file format

Upload a plain text file containing a comma-separated list of variants in modified HGVS format: `GENE_NAME:NAME`

- `GENE_NAME` must match the gene name shown in the GENE FILTER tab.
- `NAME` is the variant in MANE Select reference notation.
- No header row.
- The list must not end with a trailing comma.

Valid examples:

```
HBB:c.316-185C>T,GBA1:c.1226A>G
HBB:c.316-185C>T, GBA1:c.1226A>G
HBB:c.316-185C>T
```

---

## Edit an Existing Panel Filter

1. Open **Analysis Configuration** → **Panel Filters**, then click the filter under **Custom Panel Filters** to open its summary.
2. Click the **edit (pencil) icon** in the upper-right corner.
3. Change settings using the same tabs described above.
4. Click **Save Filters**. The button appears once you have made a change; if nothing has changed, there is nothing to save.

To leave without saving, click **Cancel** and confirm when asked. The Default Filter cannot be edited — create a new configuration instead.
