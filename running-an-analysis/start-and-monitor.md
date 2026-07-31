# Start & Monitor Execution

After you click **Analyze**, the analysis is queued and runs on the Software's pipeline. This page explains how to read each status, customize the dashboard tables, and find the logs an analysis produces.

> Analyses run **one at a time, in order**. A newly started analysis waits with status **In Queue** until the one ahead of it finishes.

## Track Status

Navigate to the **Analysis Dashboard** and check the **Status** column. Refresh the page to see updates.

| Status | Meaning | What to do |
|--------|---------|------------|
| **Import Initializing** | Dataset is being transferred from the source endpoint | Wait |
| **Import Error** | Dataset did not import correctly | Verify `fastq.gz` files exist in `fastq_pass/barcodeXX` directories |
| **New** | Analysis created, data imported, ready to configure | Load Analysis → assign panel and sample sheet → click Analyze |
| **In Queue** | Waiting for a previous analysis to complete | Wait — analyses run one at a time, in order |
| **Running** | Analysis is actively processing | Wait |
| **X processed of Y** | Analysis is progressing | Wait — the Y value increases as tasks are discovered |
| **Error** | Analysis did not complete or was cancelled | See [Troubleshooting](../troubleshooting/common-errors.md); reload and retry |
| **Complete** | Analysis finished successfully | Review results or download files |

> **Analysis time** varies by number of samples, read depth, and available compute resources.

## Row Actions

Click the **⋯** at the end of any row on the Analysis Dashboard for actions available to that analysis:

| Status | Available actions |
|--------|--------------------|
| **New** / **Error** | Configure Analysis, Delete |
| **In Queue** / **Running** / **X processed of Y** | Cancel |
| **Complete** | Sample Summary, View Files, Delete |
| **Import Error** / **Import Initializing** | Delete |

**Cancel a queued or running analysis:** click **⋯** → **Cancel**, then confirm **Yes, cancel this analysis**. Allow a few seconds for the status to update.

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
