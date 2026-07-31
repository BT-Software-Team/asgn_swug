# Configure an Analysis

Before an analysis can run, it needs a **panel configuration** (which variants to report) and a **sample sheet** (which samples and barcodes to expect). This page walks through assigning both and resolving any sample-sheet errors.

> **Prerequisite:** You must have already [created the analysis](create-an-analysis.md) and imported its dataset.

## Assign a Panel Configuration and Sample Sheet

1. From the **Analysis Dashboard**, double-click an analysis with status **New**, or click its **⋯** menu and choose **Configure Analysis**.

2. Review any **MinKNOW Warnings** displayed at the top of the page (see [MinKNOW Warnings](create-an-analysis.md#minknow-warnings)).

3. Under **Assign a Panel Configuration**, select a configuration from the dropdown and click **Add Panel Configuration** to apply it. To create one first, see [Panel Filter Configurations](../configuration/panel-filters.md).

4. Under **Import Sample Sheet**, click **Choose File**, select your `.txt` sample sheet, then click **ADD SAMPLESHEET**.

   The sample sheet is validated automatically. Any errors are shown in the **Errors** column. Click the pencil icon in the **Actions** column to correct individual rows inline, or upload a corrected file and re-validate.

5. Once all errors are resolved, click **Analyze** to start the analysis.

## Sample Sheet Requirements

The sample sheet must be a tab-delimited `.txt` file with the following required headers (case-sensitive):

| Column | Description |
|--------|-------------|
| `SourceID` | Sample name |
| `BarcodeID` | Barcode in format `BCXX` (e.g., `BC01`) |
| `Mixes` | Letters for each Mix included (e.g., `abc` for Mix A, B, and C) |
| `Calibrators` | `n` for non-calibrators; `a`, `c`, or `ac` for calibrators |

Additional rules:

- Values may only contain letters, numbers, `-`, and `_`. Spaces and other whitespace are not accepted.
- Each `SourceID` + `BarcodeID` combination must be unique.
- For each `BarcodeID`, a matching `barcodeXX` folder must exist in the imported dataset directory.
- If Mix A or C is used, at least one calibrator sample must be designated for those mixes.

> **Default User Data Path is a hidden folder.** To view it in File Explorer on Windows, enable hidden files in View settings. Default path: `C:\ProgramData\asuragen\`

## What's next

- [Start & Monitor Execution](start-and-monitor.md) — run the analysis and track its status.
