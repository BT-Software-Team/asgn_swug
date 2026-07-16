# Create an Analysis

An **analysis** pairs a sequencing dataset with the Software's pipeline so it can call and report variants. Creating one names the analysis and imports the run data you want to process — you'll assign a panel configuration and sample sheet in the next step.

> **Before you begin:** Make sure the sequencing run has **fully completed**. Importing a run that is still in progress may result in missing FASTQ files, insufficient read depth, missing samples, QC flags, or analysis failures.

## Create the Analysis

1. From the **Analysis Dashboard**, click **New Analysis**.

2. Enter a name in the **Analysis Id** field. The current pipeline version is shown.

   > **Naming rules:** Only letters, numbers, dashes (`-`), and underscores (`_`) are accepted. Spaces and special characters are not allowed.

3. In **Select Dataset**, choose an already-imported dataset or select **Import Dataset…** to bring in a new one.

4. If importing a new dataset, a table of available datasets from all configured endpoints appears (see [Configure Data Sources](../configuration/data-sources.md)). Use the **Filters** and **Columns** buttons to locate a specific dataset.

   If you previously imported a dataset and have since added FASTQ files to that directory, re-importing will prompt you to overwrite the existing dataset. This will not affect any completed analyses.

   > **Dataset not visible?** See [Missing Datasets](../troubleshooting/common-errors.md#missing-datasets) in Troubleshooting.

5. Click **Create** to confirm. A MinKNOW warning about a missing `report.json` file does not prevent you from proceeding — see [MinKNOW Warnings](#minknow-warnings) for context.

## What's next

- [Configure an Analysis](configure-an-analysis.md) — assign a panel configuration and sample sheet.

---

## MinKNOW Warnings {#minknow-warnings}

A warning in this section means that the run data may not be fully compatible with the selected analysis pipeline. Common causes include an unsupported MinKNOW version, basecaller model mismatch, barcode trimming settings, or an unvalidated barcode kit.

Warnings do not block analysis, but should be reviewed before proceeding. If the warning relates to a basecaller model or incompatible MinKNOW version, see [Post-run Basecalling](post-run-basecalling.md).
