# Start & Monitor Execution

After you click **Start Analysis**, the analysis is queued and runs on the Software's pipeline. This page explains how to read each status, what you can do at each stage, how to customize the dashboard tables, and where to find the logs an analysis produces.

> Analyses run **one at a time, in order**. A newly started analysis waits with status **In Queue** until the one ahead of it finishes.

## Track Status

Navigate to the **Analysis Dashboard** and check the **Status** column. Refresh the page to see updates.

| Status | Meaning |
|--------|---------|
| **Import Initializing** | Dataset is being transferred from the source endpoint |
| **Import Error** | Dataset did not import correctly. Verify `fastq.gz` files exist in `fastq_pass/barcodeXX` directories. |
| **New** | Analysis created, data imported, ready to configure |
| **In Queue** | Waiting for a previous analysis to complete — analyses run one at a time, in order |
| **Running** | Analysis is actively processing |
| **X processed of Y** | Analysis is progressing. The Y value increases as tasks are discovered. |
| **Error** | Analysis did not complete or was cancelled. See [Troubleshooting](../troubleshooting/common-errors.md). |
| **Complete** | Analysis finished successfully |

> **Analysis time** varies by number of samples, read depth, and available compute resources.

For what you can do to an analysis at each status, see [Row Actions](#row-actions) below.

## Row Actions {#row-actions}

Every row has an **⋯ (overflow) menu** at its right-hand end. Click it for the actions available to that analysis; the button is greyed out when the status offers no actions at all.

| Status | Actions in the ⋯ menu |
|--------|-----------------------|
| **New** / **Error** | Configure Analysis, Delete |
| **In Queue** / **Running** / **X processed of Y** | Cancel |
| **Complete** | Sample Summary, View Files, Delete |
| **Import Error** / **Import Initializing** | Delete |

| Action | What it does |
|--------|--------------|
| **Configure Analysis** | Opens the analysis to assign a panel filter and sample sheet — see [Configure an Analysis](configure-an-analysis.md). Double-clicking the row does the same thing. |
| **Sample Summary** | Opens the results for a completed analysis — see [Review Results](../analysis-results/review-results.md). |
| **View Files** | Opens the downloadable output files — see [Download Results](../analysis-results/download-results.md). |
| **Cancel** | Stops a queued or running analysis. |
| **Delete** | Removes the analysis from the dashboard. |

**Cancel a queued or running analysis:** click **⋯** → **Cancel**, then confirm **Yes, cancel this analysis** (or **No, I changed my mind** to keep it running). Allow a few seconds for the status to update.

**Delete an analysis:** click **⋯** → **Delete**, then confirm **Yes, I want to delete this analysis**.

> **Deleting is permanent in the interface.** A deleted analysis cannot be restored to the dashboard, though its result files remain on disk — see [Data Storage & Archival](../reference/data-storage.md).

> If a red banner reads *We cannot locate any pipelines connected to your application*, **Configure Analysis** is greyed out in the menu and no new analysis can be started until pipelines are available.

---

## Table Controls

Each data table in the Software has a toolbar that can be used to customize its view.

**Search** — click the magnifying glass to expand a quick-search box and filter rows by any visible text.

**Columns** — click **Columns** in the toolbar and toggle checkboxes to show or hide columns. For example, hide the Pipeline column in the Analysis Dashboard to give more space to Dataset names.

**Filters** — click **Filters** to add filter criteria. Filters can be stacked with AND/OR logic. A dot badge appears on the **Filters** icon while any filter is active. To remove filters, click the **X** next to a filter row or **Remove All** at the bottom.

**Export** — click the **Export** (download) icon → **Download as CSV** or **Print** to export the current filtered/column view.

**Column sorting** — click the arrow next to any column name to sort.

**Pin columns** — click the three vertical dots at the right edge of a column header to pin it left or right. For example, pin Sample ID in Variant Results so it stays visible when scrolling horizontally.

---

## Logs

Each analysis produces a `run.log` file containing the start and completion time, pipeline version, sample summary, and a list of any failed analyses. Additional logs are available for diagnosing errors.

If an error is listed in the **Failed Analyses** section of `run.log`, refer to [Troubleshooting](../troubleshooting/common-errors.md) or contact Asuragen Technical Support.

**Output path:** `[Analysis Id]/results/logs/run.log`
