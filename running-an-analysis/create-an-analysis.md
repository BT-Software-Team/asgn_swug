# Create an Analysis

An **analysis** pairs a sequencing dataset with the Software's pipeline so it can call and report variants. Creating one names the analysis, picks the pipeline, and selects the run data to process — you'll assign a panel configuration and sample sheet in the next step.

> **Before you begin:** Make sure the sequencing run has **fully completed**. Importing a run that is still in progress may result in missing FASTQ files, insufficient read depth, missing samples, QC flags, or analysis failures.

## Create the Analysis

1. From the **Analysis Dashboard**, click the **+ (plus) icon** in the table toolbar. The **Create Analysis** dialog opens.

   > If the plus icon is greyed out, hovering it shows **No pipelines available** — the Software has not loaded a pipeline to run against. Contact support if this persists.

2. Enter a name in the **Analysis Id** field.

   > **Naming rules:** Only letters, numbers, dashes (`-`), and underscores (`_`) are accepted. Spaces and special characters are not allowed. The name must also be unique — reusing the name of an existing analysis reports *This Analysis name already exists in your table.*

3. Under **Pipelines**, select the pipeline to run. Each available pipeline and version is listed as its own option; the first is selected for you, so you can leave this as-is if only one is available.

4. In the **Dataset** dropdown, choose an already-imported dataset, or choose **Import Dataset...** to bring in a new one.

5. If you chose **Import Dataset...**, a table of datasets found across all configured data sources appears below (see [Configure Data Sources](../configuration/data-sources.md)). It has two columns:

   | Column | Contents |
   |--------|----------|
   | **Endpoint** | The data source the dataset was found on. |
   | **Name** | The run folder name. |

   Click a row to select that dataset. Use the toolbar above the table to search, filter, or change which columns are shown, and the pager beneath it to move through longer lists.

   If you select a dataset that has already been imported, a warning appears: *This dataset already exists and will be re-imported.* Continuing replaces the current contents of the stored dataset — useful when FASTQ files were added to the run folder after the first import. Completed analyses that used the earlier import are unaffected. You are asked to confirm in a **Re-Import Dataset** dialog before anything is replaced.

   > **Dataset not visible?** See [Missing Datasets](../troubleshooting/common-errors.md#missing-datasets) in Troubleshooting.

6. Click **Create Analysis**. The button stays disabled until both an Analysis Id and a dataset are provided.

   To close without creating anything, click **Cancel**.

The new analysis appears in the Analysis Dashboard, ready to be configured.

## What's next

- [Configure an Analysis](configure-an-analysis.md) — assign a panel configuration and sample sheet.
